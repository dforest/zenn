---
title: "Web3開発をこれからはじめるときに読む資料 2023"
emoji: "📚"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["web3", "solidity", "hardhat"]
published: true
publication_name: "pytokyo"
---
:::message
この記事は2023年8月時点での筆者の開発経験をもとに書きました。Web3開発は変化が激しいため、時間が経つと内容が古くなる可能性があります。
:::

これからWeb3開発をはじめる人に向けて、開発環境や学習資料などを紹介します。
筆者も2023年になってから開発を始めたので、2023年8月時点では比較的新しい情報になっているはずです。

# 開発環境

これからWeb3開発をはじめるなら*Hardhat*が良いです。

https://hardhat.org/

HardhatはNode.jsで動き、Solidityのコンパイルやテスト、デプロイを行うことができる開発環境です。
ローカルにノードを立てることもできます。そしてなにより便利なのが、Ethereum（やPolygonなど）のmainnetをforkしてローカルにノードを立てられることです。

mainnetは本来、ネットワークの本番環境にあたるのでここでテストを行おうとするとガス代のために多額の費用がかかり現実的ではありません。とはいえ、mainnetの他のコントラクトなどと連携した開発をしたい場合、mainnetで試すほかありません。それをローカルで実現できてしまうのがHardhatのforkです。

mainnetをforkすると、その時点でのmainnetの最新状態でローカルのノードが立ち上がります（ローカルに立ったノードはローカルのクローズドなネットワークなのでmainnetには影響しません）。つまり本番相当環境でテストやデバッグがぶん回せます。やったね！

エディタについては筆者はVSCodeで行いましたが、お好みで大丈夫です。

# Hardhatをはじめるときに読む資料

https://hardhat.org/hardhat-runner/docs/getting-started#overview

公式ドキュメントのOverviewです。とりあえず読んでおけば使用感がつかめます。

https://dev.classmethod.jp/articles/hardhat-quick-start/
https://blog.misosi.ru/2022/06/02/smart-contract-development-and-testing-with-hardhat/

日本語が良い場合はこのあたりが良いと思います。

# Solidityをはじめるときに読む資料

Solidityの言語理解には本がおすすめです。

https://amzn.to/3OZDcAi

https://amzn.to/3QKIot0

自分はこの2冊をざっと読みました。どちらも開発環境構築やそれをつかった開発手順の項目がありますが、情報が古いので読み飛ばして大丈夫です。Solidityの言語とスマートコントラクトの仕様の理解が進めばOKです。実際の開発手順は[Hardhatの公式ドキュメント](https://hardhat.org/hardhat-runner/docs/getting-started#overview)を読んでください。[Step-by-stepのチュートリアル](https://hardhat.org/tutorial)もあったりします。

## Open Zeppelin

https://docs.openzeppelin.com/

いわゆるNFTのERC721やERC1155の実装をしたいときに必ずお世話になるのがOpenZeppelinです。それらの基盤となるコントラクトが用意されています。また、コントラクトの実行権限を簡単に扱えるようにしたり、コントラクトのアップグレードを簡単に行えるようにしたりするコントラクトも用意されています。

色々な便利機能がセキュリティを考慮して実装されているので、自分で実装するよりもOpenZeppelinのコントラクトを拡張するほうが安全です。またコードも公開されているので、実装の勉強にもなります。

まずはどのような機能が提供されているのか、知っておくことが大事です。

## Solidity by Example

SolidityのVariableやIf/Elseといった基本的な構文から、Crowd FundやAuctionといった実用的な実装例まで幅広く掲載されています。
2023年8月現在もメンテされているようなので、実現したい機能から実装例を調べるのに便利です。

https://solidity-by-example.org/


# 実際のプロダクトのコントラクトコードを読む

私の持論ですが、プロダクト開発するときは実際に動いているプロダクトコードを読んで設計や実装の参考にするのが最良です。幸いにもコントラクトコードは公開されているものが多いので、自分のやりたいことに近いコントラクトを探して読んでみて開発の参考にするのがおすすめです。

https://etherscan.io/

コントラクトアドレスがわかっていればEtherScanでコードを見にいくのが楽です。Verifyされているコントラクトなら簡単にコード全文を読むことができます。
