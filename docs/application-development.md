# アプリケーション開発

# 基本思想

基本思想「**持続性、高パフォーマンス、安全性を備える高品質なアプリケーションのみをリリースする**」にもとづき、技術スタックを選定し、良質なソースコードとテストコードを書きます。

**持続性、パフォーマンス、安全性、品質／テストの四つの分類で指針をまとめています。** 私達のクオリティ基準においてこれらに劣後はなく、**プロダクション環境へのリリース時にはそのすべてが達成されている必要があります。**

# 持続性

**プロダクションを見据えた開発着手時点で、持続性のある技術スタック選定を行い、アプリケーションアーキテクチャ構成を決定し、コードを書き始める必要があります。**

最悪の選定基準は「とりあえずメンバーがこの技術スタックに／言語に詳しいから」です。持続性を備えたプロダクトのために最良の選択を行うのであって、人に依存した選択はしてはいけません。「最良の選択のイメージがあるが、詳しくない…」という場合は、学習期間を設けます。

## 持続性を備えるための技術スタック選定

アプリケーションは生み出されたされたその時から、時間経過とともに古くなってゆきます。しかし、**適切な技術スタックを選択することで、リフレッシュのしやすさを手に入れることができます。**

逆に不適切な技術スタックを選択すると、リフレッシュしづらく、徐々に「コードやライブラリは不変であること」が優先選択される状況に陥ります。多くのエンジニアなら経験があるでしょう。そして技術負債の蓄積スピードが加速してしまいます。

技術スタックの持続性・選択の善し悪しはデータ化されているものではありませんが、普段から触れている技術情報コミュニティの活発なさまや、世界中の技術者の支持（例：[Stack overflow Developer Survey](https://survey.stackoverflow.co/) や [State of JavaScript](https://stateofjs.com/) など）から伺い知ることができます。常に情報把握に努めてください。

このアーキテクチャ標準では、具体的な今日時点の技術取捨選択についても踏み込んで記載します。これらの選定が正しいとは限らないですが、私たちエンジニアリングチームにおける選定基準として掲げます。正しさについては常に情報収集を続け、議論し、必要であれば慎重に更新してください。

**好ましい技術選択**

- **Programming Language**: TypeScript, JavaScript, Python, Rust, Kotlin, Swift
- **Framework & Library**: Vue3, Nuxt3, React, Rimix, express.js, クラウドインフラが提供する SDK
- **Cloud Infrastructure**: Google Cloud > AWS > Azure
- **Test**: Vitest, Playwright
- **Software Engineering**: DDD, Clean, Layered, Micro services, CI/CD, GitHub

**好ましくない、または用途に応じて慎重に選択すべき技術選択**

- **Programming Language**: PHP, Java, Ruby, Perl
- Google Cloud, AWS, Azure 以外の自称クラウド
- GitLab, Bitbucket
- マルチクラウド アプローチ

## 持続性を備えるためのコード

**美しいコードを書くということに尽きます。** コードの美しさとは、**コードの少なさ、可読性、堅牢さと変更容易性、正常な依存関係、再利用性、過不足のないドキュメント／インラインコメント** といった要素です。これらに触れた優れた書籍や情報、OSS のコードなどから学び、テクニックを磨き続ける必要があります。

次は、その基本原則外で実践すべきことです。

- コードは積極的にリファクタリングします
  - リファクタリングの際にディグレードさせないため、高いカバレッジのテストコードを常に備えておき、心理的負担なく・恐れずリファクタリングを行えるレポジトリ状態をキープします
- ソフトウェアデザイン手法を学び、適用します
  - フロントエンドについては、Vue, Nuxt, React, Remix などでは機能的な整理がディレクトリ ストラクチャーに反映されているため、標準に従いながら逸脱しないよう拡張します
    - 標準にない追加したディレクトリは、そのレポジトリの README に説明を記載します
    - ディレクトリ／機能間の依存関係の方向性も README に記載します
  - バックエンドについては、対象への適性およびメンバーの習熟度に応じて次のいずれかを選択します
    - DDD、クリーンアーキテクチャ、レイヤードアーキテクチャ

## 持続性を備えるためのライブラリ選定

すべてをスクラッチ実装せず、外部ライブラリを活用します。

目的の機能を実現するライブラリの候補が複数ある場合は、次から総合的に判断してください：

- レポジトリのスター数
- レポジトリのコントリビューターの数
- 放置されている issue の数
- 最近も release されているか
- 中長期的なダウンロード数
- エコシステム汚染があった場合、再発がなさそうか

## 持続性を備えるアプリケーション アーキテクチャ構成

**フロントエンド（UI） とバックエンド（BFF）はアプリケーションレイヤーを分離します。**

バックエンドアプリケーションがフロントエンドをレンダリングする仕組みは禁じ手とします。なお SSR はこの分類ではありません（開発時にはレイヤーが分離しているため）。

# パフォーマンス

**インフラストラクチャのスケーリング＆スペック頼みではなく、アプリケーション自身が高いパフォーマンスで動作する必要があります。** これにより、利用ユーザーや利用システムのストレスを下げます。アプリケーションのアクティブ時間を減らすことでのインフラコスト削減の利点もあります。

以下は標準的な規定です。

- サーバサイドからの応答（SSR や API）については、数 ms から 300ms でのレスポンスを基本とします
  - 実行環境のコールドスタート時のウォームアップ時間は除きます
- 画面レンダリングを高速化します
  - 具体的には）Intersection Observer の活用、バックエンドからのデータの 少量 fetch、画面を構成するコンポーネントやタグの構造と階層をシンプルに保つ、圧縮配信、CDN キャッシュ
  - スピードを計測します。Web ブラウザの場合は Lighthouse で計測できます
  - 体感の緩和策として）プログレスインジケーターや Empty State の表示
- 非同期処理を活用し、処理をフォアグランドから切り離します
  - 具体的には）await / async (Promise), クラウドインフラが備える非同期タスク処理サービスの利用
- リアルタイムな情報の同期を活用することで、都度のバックエンドへのリクエスト数を減らします
  - 具体的には）Cloud Firestore の onSnapshot, WebSocket
- React や Nuxt のように SSR, SSG, SPA をミックスできるフレームワークを採用しつつ適切に切り分けて適用することで、操作性とパフォーマンスを最適化します
  - 例）クローラー向けに見せたいページやユーザーにキャッシュさせたいページは SSR 、フォーム中心に構成されたページは SPA
- 速いアルゴリズムを選択します。ただし、速さよりも安定性と安全性は優先されます
- 次の工夫でオリジンへの負荷を下げると同時に高速化します
  - バンドルサイズを軽量にします
    - 具体的には）重厚なライブラリを利用しない、使われないリソースを含まない、Tree Shaking、
  - コンテンツや API に適切なキャッシュ設定を施す
  - 画像リソース等を CDN 経由で配信

## アプリケーションのパフォーマンスを保つためのデータストア・アーキテクチャ

データストアの選定・設計・設定はアプリケーション開発者らが行います。つまり、用意されたデータストアしか利用できないと考えるのではなく、アプリケーションのためにデータストアを選定します。

> [!NOTE]
> データストア ＝ リレーショナルデータベース、KVS データベース、データレイク、ストレージといった、データを保管し管理できるレイヤーすべてを指しています

この際、次の観点に留意します

- プライマリキー／セカンダリキー、インデックスによるアクセスを基本とします
- スキャンは件数が少ない場合に限ります。1,000 件程度を目安としてください
- RDB の場合、すべての SQL クエリの実行計画を確認し、上記二点の正しさを確認します
- データ量の多くなる場合、テーブルを適切にパーティショニング／シャーディングします。これを自前で実装せず、自動で行われるデータベースを選定します
- ハウスキーピング。履歴系テーブルはあらかじめ決められた保存期間または保存件数を超えたデータをアーカイブまたは削除します
  - DynamoDB の TTL やストレージサービスのオブジェクトライフサイクル管理のように、自動で対象データ／オブジェクトを消す仕組みを活用します

# 安全性

冒頭で劣後はないとは言いましたが、**アプリケーションの安全性は最優先されます。** つまり、安全性を優先するためにパフォーマンスや持続性を犠牲にすることができます。

- 一般的な Web アプリケーションの安全な実装については、IPA の「[安全なウェブサイトの作り方](https://www.ipa.go.jp/security/vuln/websecurity/about.html)」は古典的ながらも有用なため、必ず把握します
- ユーザー（※画面、API 問わず）からの INPUT 値は validation します
- Validation は、取り得る既定値との完全一致確認が優先です。それができないケースでは null 値のチェック、桁数のチェック、必要な場合は型のチェックも行います
- SPA の場合、すべての環境変数、コード、store 保存値はユーザー（やハッカー）が閲覧が可能です。見られても構わない値のみを扱います。見られてはいけない値は、バックエンドで処理します
- クレデンシャルをレポジトリに push してはいけません。プロダクション環境においては、実行環境側で安全な方法で注入します
  - 具体的には）Google Cloud であれば Secret Manager、AWS は AWS Secrets Manager
  - 実行時環境変数での注入は原則避けること

利用しているライブラリを可能な限り最新に保ちます。

- コードレポジトリには GitHub のみが利用可能です。その上で、レポジトリの GitHub Depandabot セキュリティアップデートを有効にします
  - depandabot.yml が必要なユースケースは少なく、GitHub レポジトリコンソール上からの設定で十分な場合がほとんどです
- セキュリティアップデートは、Dependabot 検出情報記載の CVSS 値による分類を基準に実施します。Critical または High のものは、即座にアップデートを適用し展開します
  - セキュリティアップデートはパッチのため、後方互換を気にしすぎる必要はありません
- バージョンアップデートは、目安として毎週、長くても隔週で実施します。つまり、頻繁にバージョンアップを行います。これを維持するために、テストを充実させます

# 品質／テスト

[Testing Trophy](https://kentcdodds.com/blog/the-testing-trophy-and-testing-classifications) と [DevOps Bug filter](https://note.com/globis_engineers/n/n455d7322fa33) はテストにおける優れた構造整理です。

また [Google が提唱するテストサイズ分類(small, medium, large)](https://testing.googleblog.com/2010/12/test-sizes.html) もテスト設計において有用です。

これらも念頭に、テスト全体の構成を設計します。

古典的な表現である単体テスト（Unit test）、結合テスト（Integration test）、総合テスト（E2E テスト）は、共通語であるためこれらを基準に次のとおり整理しています。

## **Static test**

- eslint, prettier を導入・設定します
- エディタのファイル保存時の自動整形を推奨します
- CI にテストを組み込み、コード書式の統一が失われていないかを確認します。違反しているコードは merge できません

## **Unit test**

コンポーネント・機能単位のテストです。通常はソースコードのファイル単位であることが多いです。 stub 化や mock 化を活用してかまいません。

- アプリケーションコード群の構成、すなわち機能や画面が整理されて固まってきたと判断したら、テストコードを書きます
  - プロトタイピングフェーズや α バージョン開発中は、テストは実装しないか、局所的な実装のみ行います
  - テストコードから書き始める手法（TDD）は、そのコードの大きな構成変更はしばらくないと確信できる場合のみとします
- 各言語が備えるテストコードライブラリを利用します。ライブラリは持続性観点で選択します。例えば TS/JS フロントエンドでは、シェアの多い Jest よりも Vitest の伸びと将来性を選びます
- CI に組み込みます。通常は PR 作成時にテストを実行します。テストをパスしない場合は merge できません
- CI ではカバレッジを計測し、CI 結果レポート and/or PR コメントとして残します

## **Integration Test**

コンポーネントを stub 化せず実際に mount したり、動作する API を呼び出しながら行うテストです。

- Vue, Nuxt, React では page を routing してテストします。この際、利用コンポーネントは実際に mount します
- API 呼び出しについてはケースバイケースですが、Integration test での実装が難しい場合は E2E Test でカバーすることも考えてください
- CI への組み込みは Unit test 同様です

## **E2E Test**

ユーザーの操作や機能の連続的な作用など、連続性のある動作や処理が期待どおりであることを確認するテストです。

- Playwright をはじめとしたテストフレームワークを活用し、逆にマニュアル E2E テストの比率を下げます
- 実行頻度はケースバイケースですが、Unit test / Integration test と比べて実行頻度を下げて構いません。具体的にはスケジューラ（毎日など）や main へのマージ後に行います
- マニュアル E2E テストについてはシナリオを用意します

以上