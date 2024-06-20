---
title: APRLスクリプトの概要と使用方法
weight: 10
geekdocCollapseSection: false
---

このセクションでは、Azure Proactive Resiliency Library v2 (APRL) スクリプトの概要とその使用方法について説明します。以下のシナリオについて説明します。

{{< toc >}}

## 1 - Collector スクリプト

- [GitHub Link to Download](https://github.com/yahanda/Azure-Proactive-Resiliency-Library-v2/blob/main/tools/1_wara_collector.ps1)
- [GitHub Link to Sample Output](https://github.com/yahanda/Azure-Proactive-Resiliency-Library-v2/blob/main/tools/sample-output/WARA_File_2024-05-07_11_59.json)

Collector PowerShell スクリプトは、Azure Proactive Resiliency Library (APRL) ツール スイートで実行される最初のスクリプトです。これは、Azure 環境からデータを収集し、このリポジトリ内の Azure Resource Graph クエリを使用して、潜在的な問題と改善すべき領域を特定するのに役立つように設計されています。このスクリプトでは、Az.ResourceGraph モジュールを利用して、Azure Resource Graph に関連データのクエリを実行します。

---

**Collector スクリプトを実行するには、次の 2 つのオプションがあります**

1. Cloud Shell - 同じテナント内のファイル共有への書き込みアクセス権を持つ Cloud Shell を構成する必要があります。

1. Local Machine - スクリプトで利用されている現在のモジュールがインストールされている必要があります。

### 1.1 - Cloud Shell

1. [Azure Portal](https://portal.azure.com/) で Cloud Shell を開き、BASH ではなく PowerShell を選択します。

    - Cloud Shell を初めて使用する場合は、Microsoft Learn のファースト ステップ ガイドを参照してください - [Azure Cloud Shell](https://learn.microsoft.com/en-us/azure/cloud-shell/get-started/classic?tabs=azurecli#start-cloud-shell).

    {{< figure src="../../img/tools/collector-1.png" width="100%" >}}

1. WARA Collector スクリプトを Cloud Shell にアップロードします。
  {{< figure src="../../img/tools/collector-2.png" width="60%" >}}

1. オプションのパラメーターを利用してスクリプトを実行します。

    - パラメータには次のものが含まれます
      - **TenantID**:  *任意* ; 使用するテナント
      - **SubscriptionIds**:  *任意 (or SubscriptionsFile)* ; 分析に含めるサブスクリプションを指定します: Subscription1,Subscription2
      - **SubscriptionsFile**:  *任意 (or SubscriptionIds)* ; 分析するサブスクリプション・リストを含むファイルを指定します (1 行に 1 つのサブスクリプション)
      - **RunbookFile**:  *任意* ; 使用する Runbook (セレクタとチェック) を含むファイルを指定します
      - **ResourceGroups**:  *任意* ; 分析に含めるリソース グループを指定します: "ResourceGroup1","ResourceGroup2"
      - **Debug**: *任意* ; 実行中にスクリプトのデバッグ情報を書き込みます

    {{< figure src="../../img/tools/collector-3.png" width="100%" >}}

1. 「A」を選択して、モジュールのインストールを許可します。
  {{< figure src="../../img/tools/collector-4.png" width="100%" >}}

1. スクリプトが完了したら、結果をダウンロードします。
  {{< figure src="../../img/tools/collector-5.png" width="100%" >}}

### 1.2 Local Machine

1. スクリプトを実行するには、最初に完了する必要がある 5 つの前提条件があります。
    1. **スクリプトは、Windows PowerShell や PowerShell ISE ではなく、PowerShell 7 から実行する必要があります**
      {{< figure src="../../img/tools/collector-6.png" width="40%" >}}
    1. **Git をローカル コンピューターにインストールする必要があります - [Git](https://git-scm.com/download/win)**
    1. **必要な PowerShell モジュールをインストールします**
        - Install-Module -Name ImportExcel -Force -SkipPublisherCheck
        - Install-Module -Name Az.ResourceGraph -SkipPublisherCheck
        - Install-Module -Name Az.Accounts -SkipPublisherCheck
    1. **スクリプトのブロックを解除します**
        - スクリプトはデジタル署名されていますが、PowerShell モジュールの ImportExcel は署名されていません。したがって、現時点では、ローカルで署名されていないスクリプトの実行を許可する必要があります。
          - Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser
          - Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope LocalMachine
    1. **対象サブスクリプションに対する閲覧者のアクセス許可**

1. 前提条件の完了後に新しい PowerShell 7 セッションを開きます。

1. ディレクトリを、WARA Collector スクリプトをダウンロードしたのと同じ場所に変更します。

    - ファイル パスの長さに関連するエラーを回避するために、これを C:\ パスの近くで実行することをお勧めします
    {{< figure src="../../img/tools/collector-7.png" width="40%" >}}

1. オプションのパラメーターを利用してスクリプトを実行します。

      - パラメータには次のものが含まれます
        - **TenantID**:  任意; 使用するテナント
        - **SubscriptionIds**:  任意 (or SubscriptionsFile); 分析に含めるサブスクリプションを指定します: Subscription1,Subscription2
        - **SubscriptionsFile**:  任意 (or SubscriptionIds); 分析するサブスクリプション・リストを含むファイルを指定します (1 行に 1 つのサブスクリプション)
        - **RunbookFile**:  任意; 使用する Runbook (セレクタとチェック) を含むファイルを指定します
        - **ResourceGroups**:  任意; 分析に含めるリソース グループを指定します: ResourceGroup1,ResourceGroup2
        - **Debug**:  実行中にスクリプトのデバッグ情報を書き込みます
        {{< figure src="../../img/tools/collector-8.png" width="100%" >}}

1. ターゲット サブスクリプションに対する閲覧者アクセス許可を持つアカウントで認証します。
  {{< figure src="../../img/tools/collector-9.png" width="40%" >}}

1. スクリプトが完了すると、結果は同じフォルダーの場所に保存されます。

## 2 - Data Analyzer スクリプト

- [GitHub Link to Download](https://github.com/yahanda/Azure-Proactive-Resiliency-Library-v2/blob/main/tools/2_wara_data_analyzer.ps1)
- [GitHub Link to Sample Output](https://github.com/yahanda/Azure-Proactive-Resiliency-Library-v2/blob/main/tools/sample-output/WARA%20Action%20Plan%202024-05-07_12_07.xlsx)

Data Analyzer PowerShell スクリプトは、Azure Proactive Resiliency Library (APRL) ツール スイートの 2 番目のスクリプトです。Collector スクリプトによって収集された出力を Azure Proactive Resiliency Guidelines (APRL) と比較し、ActionPlan Excel スプレッドシートを生成します。このツールの目的は、収集されたデータを要約し、Azure 環境の正常性と回復性に関する実用的な分析情報を提供することです。

---

### Local Machine - スクリプト実行

**Data Analyzer スクリプトは、Excel がインストールされている Windows コンピューターから実行する必要があります**

1. ディレクトリを、WARA Data Analyzer スクリプトをダウンロードしたのと同じ場所に変更します。

    - ファイル パスの長さに関連するエラーを回避するために、これを C:\ パスの近くで実行することをお勧めします
    {{< figure src="../../img/tools/collector-7.png" width="40%" >}}

1. WARA Collector スクリプトによって作成されたファイルを -JSONFile パラメーターでポイントしてスクリプトを実行します。
  {{< figure src="../../img/tools/analyzer-1.png" width="100%" >}}

1. 「R」を選択して、スクリプトの実行を許可します。
  {{< figure src="../../img/tools/analyzer-2.png" width="100%" >}}

1. スクリプトが完了すると、WARA Action Plan.xlsx ファイルが同じファイル パスに保存されます。

### Local Machine - Action Plan 分析

1. スクリプトが完了したら、Excelアクションプランを開き、ファイルの構造、生成されたデータ、収集されたリソース、ピボットテーブル、および作成されたグラフについて理解します。

    - ワークシートには次のものが含まれます
      - **Recommendations**: すべての推奨事項、そのカテゴリ、影響、説明、詳細情報のリンクなどが表示されます。
        - 列 A と列 B は、RecommendationID に関連付けられている Azure リソースの数をカウントしていることに注意してください。
      - **ImpactedResources**: RecommendationID に関連付けられている Azure リソースの一覧が表示されます。これらは、信頼性に関する Microsoft のベスト プラクティスに従っていない Azure リソースです。
      - **PivotTable**: チャートを自動的に作成するために使用されるピボットテーブルがいくつかあります。
      - **Charts**: Executive Summary PPTx で使用される3つのチャートがあります。
      - **ResourceTypes**: お客様が使用しているすべてのResourceTypeのリスト、各ResourceTypeにデプロイされたリソースの数、およびAPRLのResourceTypeに関する推奨事項があるかどうかが表示されます。
    - この時点で、APRL で使用可能な推奨事項と Azure Resource Graph クエリを含むすべての Azure リソースが自動的に検証されました。以下のステップに従って、自動化されていない、またはAPRLにまだ存在しない残りのサービスを検証します。

1. "ImpactedResources" ワークシートに移動し、列 "B" を "IMPORTANT" でフィルター処理し、残りのリソース構成の信頼性パターンを手動で検証します。

    - "IMPORTANT - Query under development"
    - "IMPORTANT - Recommendation cannot be validated with ARGs - Validate Resources manually"
    - "IMPORTANT - ServiceType Not Available in APRL - Validate Resources manually if Applicable, if not delete this line"

1. レポートを生成する前に、分析に基づいて推奨事項を削除/追加します。

## 3 - Reports Generator スクリプト

- [GitHub Link to Download](https://github.com/yahanda/Azure-Proactive-Resiliency-Library-v2/blob/main/tools/3_wara_reports_generator.ps1)
- [GitHub Link to Sample Output - Executive Summary Presentation](https://github.com/yahanda/Azure-Proactive-Resiliency-Library-v2/blob/main/tools/sample-output/Executive%20Summary%20Presentation%20-%20Contoso%20Hotels%20-%202024-05-07-12-12.pptx)
- [GitHub Link to Sample Output - Assessment Report](https://github.com/yahanda/Azure-Proactive-Resiliency-Library-v2/blob/main/tools/sample-output/Assessment%20Report%20-%20Contoso%20Hotels%20-%202024-05-07-12-12.docx)

Reports Generator PowerShell スクリプトは、Azure Proactive Resiliency Library (APRL) ツール スイートの最後の手順として機能します。Data Analyzer スクリプトによって生成された Excel スプレッドシートを Microsoft Word および PowerPoint 形式に変換します。Reports Generator は、分析されたデータから包括的なレポートを作成するプロセスを自動化し、洞察と推奨事項の共有を容易にします。

---

### Local Machine - レポート生成

1. Word テンプレートと PowerPoint テンプレートの両方を同じファイルの場所にダウンロードする必要があります。
  {{< figure src="../../img/tools/generator-1.png" width="80%" >}}

1. ディレクトリを、WARA Reports Generator スクリプトをダウンロードしたのと同じ場所に変更します。

    - ファイル パスの長さに関連するエラーを回避するために、これを C:\ パスの近くで実行することをお勧めします
    {{< figure src="../../img/tools/collector-7.png" width="40%" >}}

1. 必要なパラメーターを利用してスクリプトを実行します。

    - パラメーターには次のものが含まれます
      - **ExcelFile**:  *必須*; 「2_wara_data_analyzer.ps1」スクリプトによって生成され、カスタマイズされたWARA Excelファイル。
      - **CustomerName**:  *任意*; PPTx ファイルと DOCx ファイルに追加するお客様名を指定します。
      - **Heavy**:  *任意*; スクリプトを遅いペースで実行して、負荷の高い環境で処理できるようにします。
      - **WorkloadName**:  *任意*; PPTx ファイルおよび DOCx ファイルに追加する分析のワークロード名を指定します。
      - **PPTTemplateFile**:  *任意*; ソースとして使用する PPTx テンプレート ファイルを指定します。指定しない場合、スクリプトはスクリプトと同じパスでファイルを検索します。
      - **WordTemplateFile**:  *任意*; ソースとして使用する DOCx テンプレート ファイルを指定します。指定しない場合、スクリプトはスクリプトと同じパスでファイルを検索します。
      - **Debugging**: *任意*; デバッグ情報をログ ファイルに書き込みます。
    {{< figure src="../../img/tools/generator-2.png" width="100%" >}}

1. 「R」を選択して、スクリプトの実行を許可します。
  {{< figure src="../../img/tools/generator-3.png" width="100%" >}}

1. スクリプトが正常に実行されると、フォルダーに 2 つの新しいファイルが保存されます。一部の情報は、アクションプランに基づいて自動的に入力されます。
{{< hint type=important >}}
Updates will need to be made prior to presenting to any audience.
{{< /hint >}}
