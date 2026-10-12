# Mermaid 基本文法ガイド

本ドキュメントは、DocBaseのヘルプ記事（[Mermaid (マーメイド) 記法と書き方](https://help.docbase.io/posts/3719897)）をベースに、Mermaidの基本的な文法から各種ダイアグラムの書き方、実践例、トラブルシューティングまでをまとめたリファレンスです。

---

## 目次
1. [Mermaidとは](#1-mermaidとは)
2. [Mermaidを使うメリット](#2-mermaidを使うメリット)
3. [基本構文とコードブロックの書き方](#3-基本構文とコードブロックの書き方)
4. [フローチャート (Flowchart)](#4-フローチャート-flowchart)
5. [シーケンス図 (Sequence Diagram)](#5-シーケンス図-sequence-diagram)
6. [クラス図 (Class Diagram)](#6-クラス図-class-diagram)
7. [状態図 (State Diagram)](#7-状態図-state-diagram)
8. [ER図 (Entity-Relationship Diagram)](#8-er図-entity-relationship-diagram)
9. [ガントチャート (Gantt Chart)](#9-ガントチャート-gantt-chart)
10. [ジャーニーチャート (User Journey)](#10-ジャーニーチャート-user-journey)
11. [Gitグラフ (Git Graph)](#11-gitグラフ-git-graph)
12. [円グラフ (Pie Chart)](#12-円グラフ-pie-chart)
13. [要件図 (Requirement Diagram)](#13-要件図-requirement-diagram)
14. [C4モデル (C4 Context Diagram)](#14-c4モデル-c4-context-diagram)
15. [アーキテクチャダイアグラム (Architecture Diagram)](#15-アーキテクチャダイアグラム-architecture-diagram)
16. [実践的なユースケース](#16-実践的なユースケース)
17. [よくあるエラーと解決方法](#17-よくあるエラーと解決方法)

---

## 1. Mermaidとは

**Mermaid（マーメイド）** は、シンプルなテキストベースの記法を用いて様々な図表をレンダリングできるJavaScriptベースのツールです。  
Markdown風のテキストで記述できるため、作図ツールのバイナリファイルや画像ファイルと異なり、**Git等のバージョン管理システムと非常に高い親和性**を持ちます。

---

## 2. Mermaidを使うメリット

- **文書と図表の一体管理**: Markdown文書の中にコードとして図を埋め込めるため、設計書と図の乖離が起きにくい。
- **チーム編集が容易**: 専用ツールのインストールや有料ライセンスが不要で、誰でもテキストエディタで修正可能。
- **変更差分（diff）の可視化**: Gitで「どこがどう修正されたか」を行単位で追跡・レビュー可能。
- **検索性の向上**: 図の中の文言がテキストデータであるため、全文検索にヒットする。

---

## 3. 基本構文とコードブロックの書き方

Markdownファイル内でコードブロックの言語指定に `mermaid` を記述します。

````markdown
```mermaid
[図の種類] [オプション]
    要素1 関係 要素2
```
````

### 最小の例
```mermaid
graph TD;
    A[開始] --> B[処理];
    B --> C[終了];
```

---

## 4. フローチャート (Flowchart)

プロセスやワークフローの流れを視覚化します。

### 4.1 基本例
```mermaid
graph TD;
    A[スタート] --> B[処理開始];
    B --> C{条件判定};
    C -- Yes --> D[成功処理];
    C -- No --> E[失敗処理];
    D --> F[終了];
    E --> F;
```

### 4.2 方向の指定
グラフの先頭で描画の向きを指定します。
- `TD` / `TB`: 上から下 (Top to Down / Top to Bottom)
- `BT`: 下から上 (Bottom to Top)
- `LR`: 左から右 (Left to Right)
- `RL`: 右から左 (Right to Left)

### 4.3 ノードの形状
| 記述例 | 形状 | 用途 |
|---|---|---|
| `[テキスト]` | 四角形 | 一般的な処理 |
| `(テキスト)` | 角丸四角形 | 処理・ステップ |
| `([テキスト])` | 楕円 / スタジアム型 | 開始 / 終了 |
| `[[テキスト]]` | サブルーチン | 別定義の処理呼び出し |
| `[(テキスト)]` | 円柱（データベース） | DB / ストレージ |
| `((テキスト))` | 円 | 結合点・イベント |
| `{テキスト}` | ひし形 | 条件分岐・判断 |

### 4.4 矢印・線の種類
- `-->` : 実線矢印
- `---` : 実線（矢印なし）
- `==>` : 太線矢印
- `-.->` : 点線矢印
- `--o` : 先端が丸
- `--x` : 先端がバツ
- `-- テキスト -->` または `-->|テキスト|` : ラベル付き矢印

### 4.5 サブグラフ（グループ化）
```mermaid
graph TD
    subgraph クライアント環境
        A[ブラウザ] --> B[フロントエンド]
    end
    subgraph サーバー環境
        C[APIサーバー] --> D[(DB)]
    end
    B --> C
```

---

## 5. シーケンス図 (Sequence Diagram)

システム間やオブジェクト間のメッセージのやり取りを時系列で表現します。

### 5.1 基本例
```mermaid
sequenceDiagram
    autonumber
    actor User as ユーザー
    participant Front as フロントエンド
    participant API as バックエンドAPI
    participant DB as データベース

    User->>Front: ログインボタン押下
    Front->>+API: POST /api/login
    API->>+DB: ユーザー認証照会
    DB-->>-API: 認証OK
    API-->>-Front: JWTトークン発行
    Front-->>User: マイページ表示
```

### 5.2 記号と機能
- `participant 名前 as 表示名`: 参加者の定義
- `actor 名前 as 表示名`: 人型アイコンでの表示
- `->>` : 実線矢印（同期呼び出し）
- `-->>` : 破線矢印（戻り値・レスポンス）
- `+` / `-` : アクティベーション（処理実行中バー）の開始・終了（例: `A->>+B: 処理`）
- `Note over A,B: メモ`: ノートの挿入
- `alt ... else ... end`: 条件分岐
- `loop ... end`: 繰り返し
- `par ... and ... end`: 並列処理

---

## 6. クラス図 (Class Diagram)

オブジェクト指向設計におけるクラスの構造や関係性を表現します。

### 6.1 基本例
```mermaid
classDiagram
    class Person {
        +String name
        +int age
        +getDetails() String
    }
    class Employee {
        +int salary
        +String position
        +calculateBonus() int
    }
    class Manager {
        -List~Employee~ team
        +addTeamMember(Employee) void
    }

    Person <|-- Employee : 継承
    Employee <|-- Manager : 継承
    Manager o-- Employee : 集約
```

### 6.2 可視性（アクセス修飾子）
- `+` : public
- `-` : private
- `#` : protected
- `~` : package / internal

### 6.3 クラス間の関係
| 記号 | 関係 | 意味 |
|---|---|---|
| `<\|--` | 継承 (Inheritance) | 親クラスから子クラスへの継承 |
| `*--` | コンポジション (Composition) | 強い所有（全体が消えれば部分も消える） |
| `o--` | 集約 (Aggregation) | 弱い所有（全体と部分が独立可能） |
| `-->` | 関連 (Association) | クラス間の一般的な参照関係 |
| `..\|>` | 実装 (Realization) | インターフェースの実装 |
| `..>` | 依存 (Dependency) | 一時的な利用などの依存 |

---

## 7. 状態図 (State Diagram)

オブジェクトやシステムの状態遷移を表現します。

### 7.1 基本例
```mermaid
stateDiagram-v2
    [*] --> 待機中
    待機中 --> 実行中: ジョブ開始
    実行中 --> 完了: 成功
    実行中 --> エラー: 失敗
    完了 --> [*]
    エラー --> 待機中: リトライ
    エラー --> [*]: 中止
```

### 7.2 ネスト（複合状態）
```mermaid
stateDiagram-v2
    [*] --> アイドル
    
    state アイドル {
        [*] --> 省電力
        省電力 --> 通常待機: 入力検知
        通常待機 --> 省電力: タイムアウト
    }
    
    アイドル --> 処理中: リクエスト受信
    処理中 --> アイドル: 完了
```

---

## 8. ER図 (Entity-Relationship Diagram)

リレーショナルデータベースのテーブル設計・エンティティ間リレーションを表現します。

### 8.1 基本例
```mermaid
erDiagram
    USER ||--o{ POST : "writes"
    POST ||--o{ COMMENT : "has"
    USER ||--o{ COMMENT : "makes"

    USER {
        int id PK
        string username
        string email
        string password_hash
    }
    POST {
        int id PK
        int user_id FK
        string title
        string content
        datetime created_at
    }
    COMMENT {
        int id PK
        int post_id FK
        int user_id FK
        string content
        datetime created_at
    }
```

### 8.2 カーディナリティ（多重度）の記法
| 記法 | 意味 |
|---|---|
| `\|\|--\|\|` | 1 対 1（必須 対 必須） |
| `\|\|--o{` | 1 対 多（必須 対 0以上） |
| `\|\|--\|{` | 1 対 多（必須 対 1以上） |
| `}\|--\|{` | 多 対 多（1以上 対 1以上） |
| `}|..|{` | 多 対 多（0以上 対 0以上、破線） |

---

## 9. ガントチャート (Gantt Chart)

プロジェクトのスケジュールやタスク依存関係を可視化します。

### 9.1 基本例
```mermaid
gantt
    title プロジェクト開発スケジュール
    dateFormat  YYYY-MM-DD
    axisFormat  %m/%d
    
    section 要件定義・設計
      要件定義          :done, a1, 2026-03-01, 10d
      基本設計          :crit, a2, after a1, 10d
      詳細設計          :a3, after a2, 15d

    section 実装・テスト
      バックエンド実装  :b1, after a3, 20d
      フロント実装      :b2, after a3, 15d
      結合テスト        :crit, b3, after b1, 10d

    section リリース
      本番リリース      :milestone, after b3, 0d
```

### 9.2 タスクの修飾子
- `done`: 完了済みタスク
- `active`: 進行中タスク
- `crit`: クリティカルパス（重要タスク）
- `milestone`: マイルストーン（期間0の日程）
- `after タスクID`: 指定タスク完了後に開始

---

## 10. ジャーニーチャート (User Journey)

ユーザーの行動ステップと感情・満足度スコア（1〜5）を可視化します。

```mermaid
journey
    title オンラインショッピングのカスタマージャーニー
    section 検索・検討
      トップページ訪問: 5: ユーザー
      キーワード検索: 3: ユーザー
      商品詳細閲覧: 4: ユーザー
    section 購入手続き
      カート投入: 5: ユーザー
      会員ログイン: 2: ユーザー
      決済情報入力: 3: ユーザー
      注文完了: 5: ユーザー
```

---

## 11. Gitグラフ (Git Graph)

ブランチの派生やマージ履歴を視覚化します。

```mermaid
gitGraph
    commit id: "Initial commit"
    branch develop
    checkout develop
    commit id: "Feature A start"
    branch feature/login
    checkout feature/login
    commit id: "Add login form"
    commit id: "Add validation"
    checkout develop
    merge feature/login
    checkout main
    merge develop tag: "v1.0.0"
```

---

## 12. 円グラフ (Pie Chart)

データの比率をシンプルな円グラフで表示します。

```mermaid
pie title 使用プログラミング言語の割合
    "TypeScript" : 48.5
    "Python" : 26.2
    "Go" : 14.3
    "その他" : 11.0
```

---

## 13. 要件図 (Requirement Diagram)

システム要件の定義と検証・関連要素との紐付けを記述します。

```mermaid
requirementDiagram

    requirement user_auth {
        id: REQ-001
        text: "多要素認証（MFA）をサポートすること"
        risk: high
        verifymethod: test
    }

    element auth_service {
        type: service
    }

    auth_service - satisfies -> user_auth
```

---

## 14. C4モデル (C4 Context Diagram)

ソフトウェアアーキテクチャのコンテキストを可視化します。

```mermaid
C4Context
    title オンライン予約システム - システムコンテキスト図
    
    Person(customer, "顧客", "予約を行うエンドユーザー")
    System(booking_sys, "予約システム", "予約・空席管理を行うコアシステム")
    System_Ext(payment_gw, "外部決済サービス", "クレジットカード決済を実行")
    System_Ext(mail_service, "メール配信サービス", "予約確認通知メールを送信")
    
    Rel(customer, booking_sys, "空席検索・予約申し込み")
    Rel(booking_sys, payment_gw, "決済処理リクエスト")
    Rel(booking_sys, mail_service, "予約完了メール送信依頼")
```

---

## 15. アーキテクチャダイアグラム (Architecture Diagram)

クラウドインフラやマイクロサービスのコンポーネント配置・接続関係を表現する構文です（`architecture-beta`）。

```mermaid
architecture-beta
    group api(cloud)[API Layer]
    group app(server)[Application Layer]
    group data(database)[Data Layer]
    
    service web(internet)[Web Client] 
    service gateway(cloud)[API Gateway] in api
    service auth(server)[Auth Service] in app
    service order(server)[Order Service] in app
    service db(database)[PostgreSQL] in data
    service cache(disk)[Redis Cache] in data
    
    web:R --> L:gateway
    gateway:B --> T:auth
    gateway:B --> T:order
    auth:B --> T:db
    order:B --> T:db
    order:R --> L:cache
```

- 方向指定: `T` (上), `B` (下), `L` (左), `R` (右)
- デフォルト利用可能アイコン: `cloud`, `database`, `disk`, `internet`, `server`

---

## 16. 実践的なユースケース

### 16.1 業務フロー（左右配置: graph LR）
```mermaid
graph LR
    A[問い合わせ受付] --> B{緊急度判定}
    B -- 高 --> C[即時オンコール]
    B -- 低 --> D[通常トリアージ]
    C --> E[障害対応実施]
    D --> E
    E --> F[報告書作成]
    F --> G{顧客承認}
    G -- 修正あり --> E
    G -- 承認 --> H[対応完了クローズ]
```

### 16.2 例外・エラーハンドリングを含むAPI連携シーケンス
```mermaid
sequenceDiagram
    participant User as ユーザー
    participant Front as フロントエンド
    participant API as バックエンドAPI
    participant Pay as 外部決済API
    participant DB as DB

    User->>Front: 購入確定
    Front->>API: POST /orders
    API->>DB: 在庫引き当てチェック
    
    alt 在庫なし
        API-->>Front: 409 Conflict (在庫切れ)
        Front-->>User: 「申し訳ありません、在庫が切れました」
    else 在庫あり
        API->>Pay: 決済実行
        alt 決済成功
            Pay-->>API: 決済成功 (Transaction ID)
            API->>DB: 注文ステータス更新 (確定)
            API-->>Front: 200 OK (購入完了)
            Front-->>User: 購入完了画面を表示
        else 決済失敗
            Pay-->>API: カードエラー
            API->>DB: 在庫引き当て解除
            API-->>Front: 400 Bad Request (決済失敗)
            Front-->>User: 「決済に失敗しました。カード情報をご確認ください」
        end
    end
```

---

## 17. よくあるエラーと解決方法

### 17.1 構文エラー（レンダリングされない）
- **バッククォートの確認**: 開始 ` ```mermaid ` と終了 ` ``` ` が正しく対になっているか確認します。
- **予約語・記号のコンフリクト**: ラベル内に `( )`, `[ ]`, `{ }` などの括弧やダブルクォートが含まれる場合は、ダブルクォートで文字列全体を囲みます。
  - 例: `A["処理(オプション)"] --> B`
- **コロンやセミコロン**: 各ダイアグラムの構文規則（セミコロンの要否など）を確認します。

### 17.2 日本語・特殊文字の文字化けやエラー
- ファイルの文字コードが **UTF-8** で保存されていることを確認します。
- 比較演算子 `<` や `>` をラベルで使う場合は、`&lt;` や `&gt;` とHTML実体参照にエスケープするか、クォートで囲みます。

### 17.3 矢印の方向が崩れる・複雑で見づらい場合
- グラフ全体の向き（`TD` と `LR`）を切り替えてみる。
- 関連するノードを `subgraph` でグループ化する。
- 1つの図に詰め込みすぎず、**概要図** と **各機能の詳細図** に分割する。

---

## 参考リンク
- [Mermaid 公式サイト](https://mermaid.js.org/)
- [Mermaid Live Editor（ブラウザでリアルタイム編集）](https://mermaid.live/)
- [DocBase 公式ヘルプ: Mermaid (マーメイド) 記法と書き方](https://help.docbase.io/posts/3719897)
