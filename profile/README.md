# PEC Digital Manufacturing Hub

PEC Digital Manufacturing Hub は、Panasonic Energyにおけるデジタル製造・データ活用・業務アプリケーション開発に関する
**ソフトウェア、データ処理ロジック、データ構造、技術定義を継続的に管理・共有・再利用するためのGitHub Organization** です。

GitHubをデジタルエンジニアリング資産の **Single Source of Truth（SSOT）** として活用し、
個人に閉じた開発から、チームで共有・レビュー・改善・再利用できる開発スタイルへの移行を目指します。

---
## ⚠️ Security Notice

> [!WARNING]
> 個人情報、お客様預かり情報、パスワード、認証情報などの機微情報をRepositoryへCommit / Pushしないでください。

Repositoryへ情報を登録する前に、その情報をGitHub上で管理してよいか必ず確認してください。

以下のような情報はRepositoryへ登録しないでください。

- 個人情報
- お客様からお預かりした情報
- パスワード
- API Key
- Access Token / Personal Access Token
- Secret / Client Secret
- 認証情報を含むConnection String
- Certificate Private Key
- `.env` 等に保存された認証情報
- 本番データや機密性のあるサンプルデータ
- その他、公開・共有範囲を制限すべき機微情報

### 機微情報の管理

機微情報を開発時に使用する必要がある場合は、原則としてGit Repositoryおよびプロジェクトフォルダの外で管理してください。

推奨する構成例：

```text
workspace/
├── project/                 # Git Repository
│   ├── src/
│   ├── README.md
│   ├── .gitignore
│   └── .env.example        # 実値を含まないテンプレート
│
└── local-config/            # Git Repository外
    └── credentials/
```


プロジェクトフォルダ内にローカル設定ファイル等を配置する必要がある場合は、`.gitignore` を適切に設定してください。

`.gitignore` の設定例：

```.gitignore
# Local environment
.vscode
.env.*
!.env.example

# Credentials and secrets
secrets/
credentials/
*.key
*.pem

# Local data
data/local/
data/private/
```

> [!IMPORTANT]
> `.gitignore` は補助的な安全対策です。機微情報そのものをプロジェクトフォルダ外で管理することを優先し、`.gitignore`だけに依存しないでください。

Commit / Push前には、意図しないファイルや変更が含まれていないことを確認してください。

```bash
git status
git diff --cached
```

誤って機微情報をCommitした場合は、ファイルを削除して再Commitするだけでは不十分な場合があります。
*git履歴に情報が残っている可能性を考慮し*、パスワード、Token、API Key等の認証情報については無効化・再発防止を含めて適切に対応してください。

---

## Purpose

このOrganizationでは、特定の製品やプラットフォームに限定せず、
デジタル製造・データ活用・業務アプリケーション開発に必要となる技術資産を管理します。

主な対象は以下です。

- アプリケーションのソースコード
- データベース定義
  - DDL
  - View
  - Schema
  - Index
  - Stored Procedure 等
- ER図・テーブル定義・データモデル
- DWH / Data Platform上で動作するデータパイプライン
- データ変換・データ加工ロジック
- SQL / Python / PySpark / dbt等によるデータ処理コード
- API / システム間連携コード
- Infrastructure as Code（IaC）
- CI/CD関連コード
- 開発・運用に必要な技術ドキュメント
- 再利用可能な共通ライブラリ / 共通コンポーネント

---

## Platform Policy

このOrganizationで管理する技術資産は、特定の製品やクラウドサービスに限定しません。

例えばDWH / Data Platform領域では、

- Databricks
- Snowflake
- Google BigQuery
- Palantir Foundry
- その他のDWH / Data Platform

など、利用する実行基盤にかかわらず、

**再利用・変更管理・レビュー・再現性が必要なコードや技術定義をGitHubで管理すること**

を基本とします。

実行基盤や利用サービスが将来変更された場合でも、
技術資産を可能な範囲で特定プラットフォームから分離して管理することを目指します。

---

## Basic Architecture

基本的な役割分担は以下とします。

```text
GitHub
  │
  ├─ Source Code
  ├─ Data Pipeline
  ├─ Data Model
  ├─ Database Definition
  ├─ IaC / CI/CD
  └─ Technical Documentation
          │
          ▼
Development Environment
  │
  └─ VS Code etc.
          │
          ▼
Execution Platform
  │
  ├─ Databricks
  ├─ Snowflake
  ├─ BigQuery
  ├─ Foundry
  ├─ Cloud / Container
  └─ Other Platforms
```

GitHubをコードおよび技術定義のSSOTとし、
各Platformをそれらの実行・デプロイ先として位置づけます。

---

## Organization Principles

このOrganizationでは、以下を基本原則とします。

1. **GitHubをデジタルエンジニアリング資産のSSOTとする**
2. **特定製品ではなく技術資産を中心に管理する**
3. **Repositoryは独立して開発・変更・管理できる単位で作成する**
4. **コードと、それを理解するための技術定義・ドキュメントを可能な限り一体で管理する**
5. **重要な変更はPull Requestを通じてレビューする**
6. **各RepositoryでDevelopment Standardsを明確にする**
7. **RepositoryごとにOwner / Maintainerを明確にする**
8. **再利用可能な技術資産として継続的に維持・改善する**

---

## Repository Policy

Repositoryは組織図や担当部署単位ではなく、

**独立して開発・変更・レビュー・リリースできる技術資産の単位**

で作成します。

例えば以下のような単位を想定します。

```text
mes-data-pipeline
mes-web-app
lims-data-pipeline
plm-integration
manufacturing-data-model
manufacturing-database
common-python-library
```

プラットフォーム名がRepositoryの本質ではない場合、
DatabricksやSnowflakeなど特定製品の名称をRepository名へ含める必要はありません。

新しいRepositoryを作成する前に、以下を確認します。

- 既存Repositoryで管理できないか
- 明確なPurpose / Scopeがあるか
- 独立した技術資産として管理する合理性があるか
- Owner / Maintainerを設定できるか
- 継続的にMaintenanceできるか
- READMEを作成し、利用方法を説明できるか

---

## Data & Database Assets

Database / DWH / Data Platformについても、
Application Codeと同様に重要なデジタルエンジニアリング資産として扱います。

GitHubでの管理対象には、例えば以下を含みます。

```text
Database / Data Platform
│
├─ DDL / Schema
├─ View Definition
├─ Index / Stored Procedure
│
├─ Data Pipeline
├─ Transformation
├─ Data Model
├─ Semantic Definition
│
├─ ER Diagram
├─ Table / Column Definition
├─ Naming Convention
│
├─ SQL / dbt
├─ Python / PySpark
│
├─ IaC / CI/CD
└─ Architecture / Documentation
```

可能なものについては、
SQL、DDL、Markdown、Mermaid、dbt等の
**テキストベースで差分管理可能な形式**を優先します。

実データそのものをGitHubで管理することは目的としません。

---

## Development Standards

各Repositoryでは、技術領域、使用言語、Framework、
Library、実行Platform等に応じて**Development Standardsを明確に定義**します。

Organization全体ですべての技術ルールを統一するのではなく、

> **各Repositoryが採用する開発標準を明文化し、チームで共有・維持すること**

を共通原則とします。

### Standards to Define

各Repositoryでは、必要に応じて以下を明確にします。

- Programming Language / Runtime
- Framework / Library
- Package Management
- Naming Convention
- Object Naming Convention
- Directory / Package Structure
- Coding Style
- Lint / Format
- Testing
- Build / Deployment
- CI/CD
- Logging / Error Handling
- Data / Database Naming Convention
- Version Management

例えばLanguage、Framework、Test、Lintなどについて、
そのRepositoryで標準的に使用するTechnology Stackを明確にします。

新しいLibraryを導入する場合は、
既存のTechnology Stackで実現できないかを確認し、
依存関係を必要以上に増やさないことを基本とします。

---

## Documentation Structure

README.mdへすべての詳細な規則や設定値を重複して記載することは避けます。

基本的な役割を以下のように分離します。

```text
README.md
  │
  └─ Purpose / Architecture / Technology Stack / Development Policy
          │
          ▼
CONTRIBUTING.md / docs/
  │
  └─ Detailed Coding Standards / Naming Convention / Development Flow
          │
          ▼
Configuration Files
  │
  └─ Dependency / Lint / Format / Build / CI/CD
          │
          ▼
Code
```

機械的に管理できる情報については、それぞれの設定ファイルをSSOTとします。

例：

```text
Python Dependencies    → pyproject.toml
Node.js Dependencies   → package.json
dbt Dependencies       → packages.yml
Lint / Format Rules    → pyproject.toml etc.
CI/CD                  → .github/workflows/
Environment Example    → .env.example
```

可能なものについてはLint、Formatter、Test、CI等を利用し、
Development Standardsを機械的に検証できる状態を目指します。

---

## Development Flow

原則として以下の流れで開発します。

```text
Issue
  ↓
Branch
  ↓
Development
  ↓
Pull Request
  ↓
Review
  ↓
Merge
  ↓
Deploy / Release
```

重要な変更はPull Requestを通じてレビューし、
Repositoryの成熟度、重要度、利用人数に応じて
Ruleset、Required Review、Status Check等を適用します。

---

## Repository README

各Repositoryには原則として `README.md` を配置します。

READMEは単なる説明資料ではなく、

**そのRepositoryを初めて見る開発者が、目的・構造・開発方法を理解し、開発を開始できる入口**

として維持します。

例えば以下を記載します。

```text
1. Overview / Purpose
2. Architecture
3. Repository Structure
4. Technology Stack
5. Development Standards
6. Development Environment
7. Setup
8. Usage
9. Test
10. Deployment / Operation
11. Owner / Maintainer
```

Database / Data Platform関連Repositoryでは、必要に応じて以下も記載します。

```text
12. Data Source
13. Data Model
14. Data Pipeline
15. Data Quality
16. Dependencies
17. Database / Object Naming Convention
```

詳細なNaming ConventionやCoding Standardは、
必要に応じて `CONTRIBUTING.md` や `docs/` 配下へ分離します。

---

## Getting Started

このOrganizationで新たに開発を開始する場合は、以下を基本的な流れとします。

1. 開発対象とPurposeを明確にする
2. 既存Repositoryで対応できないか確認する
3. 必要な場合、新しいRepositoryを作成する
4. Owner / Maintainerを明確にする
5. READMEを作成する
6. Technology StackとDevelopment Standardsを定義する
7. 開発環境を再現できる状態にする
8. Issue / Branch / Pull Requestによる開発を開始する

最初からすべてを完璧に定義することよりも、
**チームで理解・再現・改善できる状態から始め、継続的に成熟させること**
を重視します。

---

## Documentation Policy

GitHubで管理する技術情報は、
可能な限り関連するコードと同じRepositoryで管理します。

```text
Code
Configuration
Database Definition
Data Model
Architecture
Development Standards
README
Setup Guide
Operation Guide
```

コードとドキュメントを同時にバージョン管理することで、

**実装とドキュメントの状態が一致していること**

を目指します。

一方で、正式承認、申請、契約、業務文書など、
Office文書として管理することが適切な情報については、
適切な文書管理基盤と役割を分担します。

GitHubへすべての情報を集約することを目的とはしません。

---

## Repository Lifecycle

Repositoryは作成するだけでなく、継続的に維持します。

利用を終了したRepositoryは放置せず、
必要に応じてArchiveします。

Archiveする場合は、以下を確認します。

- 後継Repositoryの有無
- 最終Owner / Maintainer
- 利用終了理由
- 関連ドキュメント
- 他Repositoryからの依存関係

Repositoryの数を増やすことではなく、
**継続的にMaintenanceできる技術資産を増やすこと**
を重視します。

---

## Naming Convention

Repository名は、
その技術資産の目的や責務を推測できる短く一貫した名称とします。

基本的には以下のような形式を推奨します。

```text
<domain>-<purpose>
```

例：

```text
mes-data-pipeline
mes-web-app
lims-data-pipeline
plm-integration
manufacturing-data-model
manufacturing-database
```

Repositoryの責務と関係のない、

- 個人名
- 担当者名
- 一時的な組織名称
- 不要な製品名

への依存は可能な限り避けます。

Variable、Class、Function、Table、Column等の
Repository内部のObject Naming Conventionは、
各RepositoryのDevelopment Standardsで定義します。

---

## Security Principles

GitHub Repositoryには、認証情報やSecretを直接保存しません。

例えば以下を対象とします。

- Password
- API Key
- Access Token
- Connection String containing credentials
- Secret
- Certificate Private Key
- その他の認証情報

認証情報が必要な場合は、
対象PlatformやCI/CD環境が提供する適切なSecret Management機能を利用します。

---

## What GitHub Is Not

このOrganizationは、
すべての情報をGitHubへ集約することを目的とはしません。

GitHubは主に、

**変更履歴・レビュー・再利用・再現性が重要となるデジタル技術資産**

を管理する場所として使用します。

```text
Source Code           → GitHub
Data Pipeline         → GitHub
Database Definition   → GitHub
Data Model            → GitHub
IaC / CI/CD           → GitHub
Development Standards → GitHub
Technical Docs        → GitHub

Actual Data           → Data Platform
Secret                → Secret Management
Formal Documents      → Document Management Platform
```

---

## Our Goal

このOrganizationは単なるソースコード置き場ではありません。

```text
個人のコード・技術情報
          ↓
チームで管理された技術資産
          ↓
標準化・レビュー
          ↓
再利用可能なデジタル資産
          ↓
PECのデジタルエンジニアリング基盤
```

GitHubを中心として、

**「誰かが作ったもの」から「チームが理解し、継続的に維持・改善・再利用できる資産」へ**

変えていくことを目指します。
