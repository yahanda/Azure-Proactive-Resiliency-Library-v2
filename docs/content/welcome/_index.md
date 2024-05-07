---
title: Welcome
weight: 0
---

---

Azure Proactive Resiliency Library v2 (APRL) のホームへようこそ。このサイトの目的は、Azure で実行されているワークロードの回復性に関する推奨事項のキュレーションされたカタログを提供することです。推奨事項の多くには、準拠していないリソースを特定するのに役立つ [Azure Resource Graph (ARG)](https://learn.microsoft.com/azure/governance/resource-graph/overview) クエリのサポートが含まれています。

サイトのコンテンツは、次の 4 つの主要なセクションに分かれています。

1. [**Azure Resources:**]({{< ref "azure-resources/_index.md">}}) このセクションでは、個々の Azure リソースに関する推奨事項について説明します。推奨事項は、Azure リソース プロバイダーとリソースの種類別に整理されています。

1. [**Specialized Workloads:**]({{< ref "azure-specialized-workloads/_index.md">}}) このセクションでは、一般的なワークロードの種類に関する推奨事項について説明します。推奨事項は、複数のリソースの種類をカバーし、ワークロード固有のガイダンスが含まれています。

1. [**Well-Architected Framework:**]({{< ref "azure-waf/_index.md">}}) このセクションでは、[Azure Well-Architected Framework](https://aka.ms/waf) からの回復性に関する推奨事項について説明します。

1. [**Tools:**]({{< ref "tools/_index.md">}}) このセクションでは、ワークロード評価用の自動化スクリプトについて説明します。スクリプトは ARG クエリを実行し、分析、レポート、およびトリアージ用のドキュメントを作成します。

### Get Started

開始するには、[Azure Resources section]({{< ref "azure-resources/_index.md">}}) に移動し、選択したリソース プロバイダーとリソースの種類に移動します。一覧表示された各リソースの種類には、推奨事項、サポート ドキュメント、および使用可能な場合は ARG クエリが提供されます。

{{< hint type=note >}}

また、このサイトに用意されている基本的な検索機能を使用して、探している Azure リソースを見つけることもできます。

{{< /hint >}}

### Contributing

[コントリビューションガイドはこちら]({{< ref "contributing/_index.md">}})をご覧ください。
