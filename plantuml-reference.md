# PlantUML実践ポケットリファレンス

## はじめに

PlantUMLは、シンプルなテキスト記述からUML（統一モデリング言語）図を生成できる強力なツールです。本書は「超図解PlantUMLを学ぶ」の姉妹書として、日常的な開発現場ですぐに活用できる実践的な知識をコンパクトにまとめました。

## 目次

1. [PlantUMLの基本](#plantumlの基本)
2. [セットアップ方法](#セットアップ方法)
3. [シーケンス図](#シーケンス図)
4. [クラス図](#クラス図)
5. [ユースケース図](#ユースケース図)
6. [アクティビティ図](#アクティビティ図)
7. [コンポーネント図](#コンポーネント図)
8. [状態遷移図](#状態遷移図)
9. [オブジェクト図](#オブジェクト図)
10. [配置図](#配置図)
11. [タイミング図](#タイミング図)
12. [ER図](#er図)
13. [ワイヤーフレーム図](#ワイヤーフレーム図)
14. [共通スタイリング](#共通スタイリング)
15. [高度なテクニック](#高度なテクニック)
16. [トラブルシューティング](#トラブルシューティング)

## PlantUMLの基本

PlantUMLは以下の基本構造に従います：

```
@startuml
  [図の内容をここに記述]
@enduml
```

図の種類によって、内部の記述方法が変わります。基本的にテキストベースで直感的に記述できるのがPlantUMLの大きな特徴です。

### ファイル保存形式

PlantUMLは以下の形式で出力できます：
- PNG（デフォルト）
- SVG
- EPS
- PDF
- LaTeX
- ASCII Art

## セットアップ方法

### インストール方法

1. **Java**のインストール（JRE 8以上が必要）
2. **PlantUML.jar**のダウンロード：[公式サイト](https://plantuml.com/ja/download)
3. **Graphviz**のインストール（一部の図で必要）

### 統合環境

多くのIDEとテキストエディタにPlantUMLプラグインが用意されています：

- **Visual Studio Code**: PlantUML拡張機能
- **IntelliJ IDEA**: PlantUMLプラグイン
- **Eclipse**: PlantUMLプラグイン
- **Atom**: language-plantuml パッケージ

### オンラインエディタ

- [PlantUML Web Server](https://www.plantuml.com/plantuml/uml/)
- [PlantText](https://www.planttext.com/)

## シーケンス図

シーケンス図はオブジェクト間のメッセージのやり取りを時系列で表現します。

### 基本構文

```
@startuml
  参加者1 -> 参加者2: メッセージ
  参加者2 --> 参加者1: 返信
@enduml
```

### 参加者の定義

```
@startuml
  actor ユーザー
  participant "Webブラウザ" as Browser
  participant "Webサーバー" as Server
  database データベース

  ユーザー -> Browser: ページを要求
  Browser -> Server: HTTP要求
  Server -> データベース: クエリ実行
  データベース --> Server: 結果返却
  Server --> Browser: HTMLレスポンス
  Browser --> ユーザー: ページ表示
@enduml
```

### 矢印のスタイル

- `->`: 実線矢印
- `-->`: 点線矢印
- `->>`: 開いた矢印
- `->x`: 失敗メッセージ
- `->o`: エンドポイント

### ノート

```
@startuml
  A -> B: リクエスト
  note right of B: このリクエストを処理
  note left of A
    複数行の
    ノートも書ける
  end note
@enduml
```

### 活性化

```
@startuml
  A -> B: リクエスト
  activate B
  B -> C: 処理依頼
  activate C
  C --> B: 結果
  deactivate C
  B --> A: レスポンス
  deactivate B
@enduml
```

### 同期メッセージと非同期メッセージ

```
@startuml
  A -> B: 同期メッセージ
  A ->> B: 非同期メッセージ
@enduml
```

### 条件分岐

```
@startuml
  A -> B: リクエスト
  alt 成功の場合
    B --> A: 成功レスポンス
  else 失敗の場合
    B --> A: エラーレスポンス
  end
@enduml
```

### ループ

```
@startuml
  loop 1から10まで
    A -> B: リクエスト #n
    B --> A: レスポンス #n
  end
@enduml
```

## クラス図

クラス図はクラス間の関係を表現します。

### 基本構文

```
@startuml
  class クラス名 {
    +公開フィールド
    -非公開フィールド
    #保護フィールド
    ~パッケージフィールド
    +公開メソッド()
    -非公開メソッド()
  }
@enduml
```

### クラス間の関係

```
@startuml
  Class01 <|-- Class02: 継承
  Class03 *-- Class04: コンポジション
  Class05 o-- Class06: 集約
  Class07 --> Class08: 関連
  Class09 -- Class10
  Class11 ..> Class12: 依存
  Class13 ..|> Class14: 実装
  Class15 ..* Class16
@enduml
```

### 抽象クラスとインターフェース

```
@startuml
  abstract 抽象クラス
  interface インターフェース

  class 具象クラス

  抽象クラス <|-- 具象クラス
  インターフェース <|.. 具象クラス
@enduml
```

### ステレオタイプ

```
@startuml
  class "クラス名" <<stereotype>>
  
  class User <<entity>>
  class Order <<boundary>>
  class PaymentController <<control>>
@enduml
```

### パッケージ

```
@startuml
  package "外部パッケージ" {
    class ClassA
  }
  
  package "コアモジュール" {
    class ClassB
    class ClassC
  }
  
  ClassA --> ClassB
@enduml
```

## ユースケース図

システムとアクターの相互作用を表すユースケース図です。

### 基本構文

```
@startuml
  actor アクター
  usecase ユースケース

  アクター -- ユースケース
@enduml
```

### 関係の表現

```
@startuml
  左アクター -- (ユースケース1)
  (ユースケース1) ..> (ユースケース2): <<include>>
  (ユースケース3) ..|> (ユースケース1): <<extends>>
  右アクター -- (ユースケース3)
@enduml
```

### ノートと説明

```
@startuml
  actor ユーザー
  usecase (ログイン)
  
  ユーザー -- (ログイン)
  
  note right of (ログイン)
    このユースケースは
    認証システムに依存します
  end note
@enduml
```

## アクティビティ図

処理の流れを表現するアクティビティ図です。

### 基本構文

```
@startuml
  start
  :アクティビティ1;
  :アクティビティ2;
  stop
@enduml
```

### 条件分岐

```
@startuml
  start
  :リクエストを受信;
  if (ユーザーは認証済み?) then (はい)
    :リクエスト処理;
  else (いいえ)
    :ログインページへリダイレクト;
  endif
  :レスポンス送信;
  stop
@enduml
```

### 繰り返し

```
@startuml
  start
  repeat
    :データ処理;
  repeat while (まだデータがある?) is (はい)
  ->いいえ;
  stop
@enduml
```

### 平行処理

```
@startuml
  start
  fork
    :タスクA;
  fork again
    :タスクB;
  end fork
  :タスクC;
  stop
@enduml
```

### スイムレーン

```
@startuml
  |ユーザー|
  start
  :フォーム入力;
  |システム|
  :バリデーション;
  if (入力は正しい?) then (はい)
    :データ保存;
  else (いいえ)
    :エラー表示;
    |ユーザー|
    :入力修正;
  endif
  |システム|
  :完了画面表示;
  |ユーザー|
  :画面確認;
  stop
@enduml
```

## コンポーネント図

システムのコンポーネントと関係を表現します。

### 基本構文

```
@startuml
  [コンポーネント1]
  [コンポーネント2]
  
  [コンポーネント1] --> [コンポーネント2]: 使用
@enduml
```

### インターフェース

```
@startuml
  [コンポーネント1] - インターフェース1
  インターフェース1 - [コンポーネント2]
  
  interface "インターフェース2" as I2
  [コンポーネント3] -- I2
  I2 -- [コンポーネント4]
@enduml
```

### ポート表現

```
@startuml
  component [コンポーネント] {
    port 入力
    port 出力
  }
  
  データソース --> 入力
  出力 --> データ保存
@enduml
```

## 状態遷移図

オブジェクトの状態変化を表現します。

### 基本構文

```
@startuml
  [*] --> 状態1
  状態1 --> 状態2: イベント
  状態2 --> 状態3: イベント
  状態3 --> [*]: 終了
@enduml
```

### 複合状態

```
@startuml
  [*] --> 未ログイン
  
  未ログイン --> ログイン中: ログイン
  
  state ログイン中 {
    [*] --> アイドル
    アイドル --> 処理中: 操作
    処理中 --> アイドル: 完了
  }
  
  ログイン中 --> 未ログイン: ログアウト
@enduml
```

### ノート

```
@startuml
  [*] --> 状態1
  状態1 --> 状態2
  
  note left of 状態1: これは初期状態
  note right of 状態2
    この状態では
    何らかの処理を実行中
  end note
@enduml
```

## オブジェクト図

オブジェクトの具体的な状態を表現します。

### 基本構文

```
@startuml
  object オブジェクト名 {
    属性1 = 値1
    属性2 = 値2
  }
@enduml
```

### オブジェクト間の関係

```
@startuml
  object 顧客 {
    名前 = "田中太郎"
    メール = "tanaka@example.com"
  }
  
  object 注文 {
    注文番号 = "O12345"
    日付 = "2023-05-20"
    金額 = 5400
  }
  
  顧客 --> 注文: 発注
@enduml
```

### マップテーブル表現

```
@startuml
  map 設定 {
    タイムアウト => 30秒
    最大接続数 => 100
    デバッグモード => false
  }
@enduml
```

## 配置図

物理的な配置関係を表現します。

### 基本構文

```
@startuml
  node ノード名
  node [ノード名2]
  
  ノード名 -- [ノード名2]
@enduml
```

### ノードタイプ

```
@startuml
  cloud クラウド {
    node Webサーバー
    node APサーバー
  }
  
  database データベース
  
  Webサーバー -- APサーバー
  APサーバー -- データベース
@enduml
```

### 詳細配置

```
@startuml
  node "Webサーバー" as web {
    node "Apache" {
      [mod_php]
    }
  }
  
  node "DBサーバー" as db {
    database "MySQL" {
      [UserDB]
      [LogDB]
    }
  }
  
  web -- db: TCP/IP
@enduml
```

## タイミング図

時間に沿った状態変化を表現します。

### 基本構文

```
@startuml
  clock "クロック" as C
  
  C is 高 
  @0
  C is 低 
  @5
  C is 高 
  @10
@enduml
```

### 複数タイミング

```
@startuml
  robust "信号A" as A
  robust "信号B" as B
  
  @0
  A is 0
  B is 0
  
  @5
  A is 1
  
  @10
  B is 1
  
  @15
  A is 0
  
  @20
  B is 0
@enduml
```

### 状態タイミング

```
@startuml
  concise "プロセス" as P
  
  @0
  P is "アイドル"
  
  @3
  P is "準備中"
  
  @7
  P is "実行中"
  
  @10
  P is "完了"
@enduml
```

## ER図

データベースのエンティティ関係を表現します。

### 基本構文

```
@startuml
  entity ユーザー {
    * ユーザーID: number <<PK>>
    --
    * 名前: text
    * メール: text
  }
@enduml
```

### リレーションシップ

```
@startuml
  entity ユーザー {
    * ユーザーID: number <<PK>>
    --
    * 名前: text
    * メール: text
  }
  
  entity 注文 {
    * 注文ID: number <<PK>>
    --
    * ユーザーID: number <<FK>>
    * 注文日: date
    * 金額: number
  }
  
  ユーザー ||--o{ 注文: 発注する
@enduml
```

### 詳細なカーディナリティ

```
@startuml
  entity 学生 {
    * 学生ID: number <<PK>>
  }
  
  entity 授業 {
    * 授業ID: number <<PK>>
  }
  
  entity 登録 {
    * 学生ID: number <<FK>>
    * 授業ID: number <<FK>>
    --
    成績: text
  }
  
  学生 ||--o{ 登録
  登録 }o--|| 授業
@enduml
```

## ワイヤーフレーム図

シンプルなUI設計を表現します。

### 基本要素

```
@startuml
  salt
  {
    ログイン
    {
      ユーザー名 | "admin"
      パスワード | "****"
    }
    [ログイン] | [キャンセル]
  }
@enduml
```

### グリッドレイアウト

```
@startuml
  salt
  {+
    ユーザー一覧
    {#
      . | ID | 名前 | メール
      1 | 001 | 田中 | tanaka@example.com
      2 | 002 | 鈴木 | suzuki@example.com
      3 | 003 | 佐藤 | sato@example.com
    }
    [新規] [編集] [削除]
  }
@enduml
```

### タブとツリー

```
@startuml
  salt
  {
    {T
      + 設定
        ++ ユーザー
        ++ セキュリティ
      + ヘルプ
    }
    
    {
      {/ "ホーム" | "検索" | "設定" }
      --
      コンテンツ部分
    }
  }
@enduml
```

## 共通スタイリング

### 色の設定

```
@startuml
  skinparam backgroundColor #EEEEEE
  
  skinparam class {
    BackgroundColor PaleGreen
    ArrowColor SeaGreen
    BorderColor SpringGreen
  }
  
  skinparam stereotypeCBackgroundColor Yellow
  
  class クラス1 {
    フィールド1
  }
  
  class クラス2 #Pink {
    フィールド2
  }
  
  クラス1 --> クラス2
@enduml
```

### フォントとスタイル

```
@startuml
  skinparam defaultFontName MS Gothic
  skinparam defaultFontSize 12
  
  skinparam class {
    FontColor red
    FontSize 14
    FontStyle bold
  }
  
  class クラス1
  class クラス2
@enduml
```

### テーマ適用

```
@startuml
  !theme cerulean
  
  class クラス1
  class クラス2
  
  クラス1 -- クラス2
@enduml
```

利用可能なテーマ：
- cerulean
- materia
- cyborg
- superhero
- minty
など

## 高度なテクニック

### プリプロセッサとマクロ

```
@startuml
  !define RECTANGLE(x,y) class x as "y" << (R,#FF7700) >>
  
  RECTANGLE(Web, "Webサーバー")
  RECTANGLE(DB, "データベース")
  
  Web -- DB
@enduml
```

### インクルード

```
@startuml
  !include common.iuml
  
  class 新しいクラス
  新しいクラス --> BASE_CLASS
@enduml
```

### スプライト/アイコン

```
@startuml
  !define ICONURL https://raw.githubusercontent.com/tupadr3/plantuml-icon-font-sprites/master
  !include ICONURL/common.puml
  !include ICONURL/font-awesome-5/database.puml
  !include ICONURL/font-awesome-5/server.puml
  
  FA5_DATABASE(db, "ユーザーDB")
  FA5_SERVER(web, "Webサーバー")
  
  web --> db
@enduml
```

### レイアウト調整

```
@startuml
  together {
    class クラス1
    class クラス2
  }
  
  class クラス3
  
  クラス1 -- クラス3
  クラス2 -- クラス3
  
  クラス1 -[hidden]-> クラス2
@enduml
```

## トラブルシューティング

### 一般的な問題

1. **図が表示されない**
   - Graphvizがインストールされているか確認
   - 構文エラーがないか確認

2. **日本語が表示されない**
   - フォント設定を確認: `skinparam defaultFontName MS Gothic`
   - エンコードをUTF-8に設定

3. **レイアウトが崩れる**
   - `together`キーワードで関連するオブジェクトをグループ化
   - `[hidden]`矢印でレイアウトを調整

### デバッグ方法

```
@startuml
  ' デバッグモードを有効化
  !pragma svek_trace on
  
  class クラス1
  class クラス2
  
  クラス1 -- クラス2
@enduml
```

### 一般的なエラーメッセージ

1. **Syntax Error**
   - 括弧やクォーテーションが閉じられているか確認
   - 特殊文字がエスケープされているか確認

2. **Dot executable not found**
   - Graphvizが正しくインストールされているか確認
   - PATHが正しく設定されているか確認

3. **File not found**
   - インクルードするファイルのパスが正しいか確認

---

本ポケットリファレンスがPlantUMLを使った効率的な設計とドキュメンテーションのお役に立てることを願っています。さらに詳しい情報は公式サイト（https://plantuml.com/ja）をご参照ください。
