---
title: "Unityのゲーム以外をいい感じに設計する"
emoji: "📚"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Unity", "LayeredArchitecture", "CleanArchitecture", "CSharp"]
published: true
---

Unityはそれ自体からの提案される設計思想が薄いので、実装者が比較的自由に方針を決められる一方で、初期設計を誤るとのちのちカオス化しやすいです。とくに外部APIや内部データベースを利用したデータやり取りが多いと、それがより顕著になります。この記事では、そうした部分をどう設計すれば不具合が少なく、柔軟性や保守性を高められるかを提案してみます。

この設計アプローチは、Layered ArchitectureとClean ArchitectureのDIPに基づいています。本格的なゲーム開発よりもモバイルアプリ開発でのUnity利用において特に有効だと思います。

# Layered Architectureとは

Layered Architectureは、UIとそれ以外（特にデータまわり）を分離するための設計として理解しやすく、使いやすい優れた考え方です。
私自身はAndroid開発をしていたときに慣れ親しんでおり、GoogleがAndroidアプリ開発において推奨するアーキテクチャとしても紹介されています。

https://developer.android.com/topic/architecture?hl=ja

# Clean ArchitectureのDIP（Dependency Inversion Principle）とは

Clean Architectureの中でもDIP（依存性逆転の原則）は、Layered Architectureに比べると優先度は高くないものの、中長期の運用を見据えるならば柔軟性を高めるために必須の考え方です。今は見えない将来の変更に備えてDIPを取り入れておくと安心です。

https://zenn.dev/yoshinani_dev/articles/c743a3d046fa78

# 設計のベース

![依存関係の図](/images/20250609/image.png)

これは自分が複数のUnityプロジェクトで実際に採用した依存関係の図です。各要素が矢印に向かって依存していると考えてください。大きくData、Service、UIの3つのレイヤーに分かれていて、DataとServiceは基本的にPureC#で実装します。

- **Data**: 外部APIや内部DBなどとデータをやりとりする部分
    - Remote: 外部APIとのやりとり
    - Local: 内部DBやPreferencesとのやりとり
    - Repository: RemoteとLocalのI/Oロジックとそれらのインターフェースを定義
- **Service**: データのビジネスロジックを実装する部分
    - データのI/OはRepositoryを通してのみ行う
- **UI**: ユーザーインターフェースを実装する部分
    - EntryPoint: DIコンテナを使って依存を解決
    - Controller: ユーザーの入力を受け取り、Serviceを呼び出して結果をUIに反映
        - ServiceはEntryPointから注入される

プロジェクトによって多少の違いはありますが、Data、Service、UIの3つのレイヤーはどんなプロジェクトでも利用しやすい構成です。

# 実装例

Data/LocalをPreferencesで実装し、UIで文字入力してボタンを押すとPreferencesに保存されるシンプルな例を紹介します。RepositoryでTrimやToLowerなどの前処理を行い、Serviceで文字列のバリデーションを行ってみます。

## Data層

### Local

PlayerPrefsはUnityのどこからでもアクセスできますが、キーの管理や依存関係を明確にするために専用のラッパークラスを作成します。そうすることで仮にPlayerPrefsの実装を変更したい場合でも、ラッパーを通じて行うことができ他のコードに影響を与えずに済みます。


```csharp
public interface IDataPreferences
{
    public string InputText { get; set; }
}

public class DataPreferences : IDataPreferences
{
    private const string InputTextKey = "InputText";

    public string InputText
    {
        get => PlayerPrefs.GetString(InputTextKey, string.Empty);
        set => PlayerPrefs.SetString(InputTextKey, value);
    }
}
```

### Repository

Repositoryでは、データの前処理やバリデーション、I/Oロジックを実装します。実際に読み書きを行うのはDataPreferencesとはInterfaceを通じて行います。こうすることで、読み書き先がPlayerPrefsからAPIに変わったとしてもRepositoryの変更はせずIDataPreferencesの実体を変更するだけでよくなります。

```csharp
public interface ITextRepository
{
    void Save(string value);
    string Load();
}

public class TextRepository : ITextRepository
{
    private readonly IDataPreferences _localDataSource;

    public TextRepository(IDataPreferences localDataSource)
    {
        _localDataSource = localDataSource;
    }

    public void Save(string value)
    {
        // 前処理: TrimとToLower
        var processed = value.Trim().ToLower();

        // バリデーション: 空文字は許可しない
        if (string.IsNullOrWhiteSpace(value))
        {
            throw new ArgumentException("Input cannot be empty.");
        }

        _localDataSource.InputText =  processed;
    }

    public string Load()
    {
        return _localDataSource.InputText;
    }
}
```

## Service層

Serviceでは、ビジネスロジックを実装しますが、今回は特にないのでRepositoryを通じてデータの保存と取得を行うだけのシンプルな実装にします。将来的に複雑なロジックが必要になった場合でも、Service層で対応できます。

```csharp
public interface ITextService
{
    void ValidateAndSave(string value);
    string GetSavedText();
}
public class TextService : ITextService
{
    private readonly ITextRepository _repository;

    public TextService(ITextRepository repository)
    {
        _repository = repository;
    }

    public void ValidateAndSave(string value)
    {
        _repository.Save(value);
    }

    public string GetSavedText()
    {
        return _repository.Load();
    }
}
```

## UI層

UIからデータをやりとりするためにまずはEntryPointで依存を解決します。実際のUIロジックを書くControllerはMonoBehaviorなのでコンストラクタインジェクションは利用できないためメソッドインジェクションでServiceを注入します。ControllerではAwakeやStartではなくInitializeが必ず動作の起点になるように実装します。

### EntryPoint

```csharp
public class TextInputEntryPoint : MonoBehaviour
{
    [SerializeField] private TextInputController textInputController;

    private void Awake()
    {
        // DIコンテナの初期化
        var dataPreferences = new DataPreferences();
        var textRepository = new TextRepository(dataPreferences);
        var textService = new TextService(textRepository);

        // ControllerにServiceを注入
        textInputController.Initialize(textService);
    }
}
```

### Controller

```csharp
public class TextInputController : MonoBehaviour
{
    [SerializeField] private InputField inputField;
    [SerializeField] private Button saveButton;
    [SerializeField] private Text displayText;

    private ITextService _textService;

    // DIコンテナを使ってServiceとRepositoryを注入
    public void Initialize(ITextService textService)
    {
        _textService = textService;
        LoadSavedText();
    }

    private void Start()
    {
        saveButton.onClick.AddListener(OnSaveButtonClicked);
    }

    private void OnSaveButtonClicked()
    {
        try
        {
            _textService.ValidateAndSave(inputField.text);
            LoadSavedText();
        }
        catch (ArgumentException ex)
        {
            Debug.LogError(ex.Message);
        }
    }

    private void LoadSavedText()
    {
        displayText.text = _textService.GetSavedText();
    }
}
```

今回はVContainerなどのDIコンテナプラグインは使わずシンプルに実装しました。Unityを使ったDIの理解には下記の記事がわかりやすくておすすめです。

https://zenn.dev/qemel/articles/ad6cf484d8280a

# まとめ

この設計でいくつかUnity製モバイルアプリを実装してきましたが、特にデータまわりの柔軟性と保守性が高く、データ層とUI層で分担してチーム開発がしやすい上に、理解も難しくないのでとてもおすすめです。

他にも良い設計があればぜひ教えて下さい。
