---
title: 使用 Open Service Broker for Azure (OSBA) 与 Azure 托管服务进行集成
description: 使用 Open Service Broker for Azure (OSBA) 与 Azure 托管服务进行集成
author: rockboyfor
ms.topic: overview
ms.date: 03/09/2020
ms.author: v-yeche
ms.openlocfilehash: 107bffa0153482e45c147baaf0d4c5e8f75a0277
ms.sourcegitcommit: 3c98f52b6ccca469e598d327cd537caab2fde83f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79290839"
---
<!--IMPORT: NOT AVAILABLE ON MOONCAKE-->
<!--PLEASE BE KINDLY NOTICE MOONCAKE COMMENTS-->
# <a name="integrate-with-azure-managed-services-using-open-service-broker-for-azure-osba"></a>使用 Open Service Broker for Azure (OSBA) 与 Azure 托管服务进行集成

结合使用 [Kubernetes 服务目录][kubernetes-service-catalog]和 Open Service Broker for Azure (OSBA) 时，开发人员可利用 Kubernetes 中的 Azure 托管服务。 本指南重点介绍如何使用 Kubernetes 部署 Kubernetes 服务目录、Open Service Broker for Azure (OSBA) 和使用 Azure 托管服务的应用程序。

## <a name="prerequisites"></a>先决条件
* Azure 订阅

* Azure CLI：[在本地安装它][azure-cli-install]。

    <!--Not Available on , or use it in the [Azure local Shell][azure-cloud-shell]-->

* Helm CLI 2.7+：[在本地安装它][helm-cli-install]。

    <!--Not Available on , or use it in the [Azure local Shell][azure-cloud-shell]-->
    
* 使用 Azure 订阅上的参与者角色创建服务主体的权限

* 现有 Azure Kubernetes 服务 (AKS) 群集。 如果需要 AKS 群集，请参照[创建 AKS 群集][create-aks-cluster]操作，以便快速入门。

## <a name="install-service-catalog"></a>安装服务目录

第一步是使用 Helm 图表在 Kubernetes 群集中安装服务目录。 按照以下步骤升级群集中的 Tiller（Helm 服务器）安装：

<!--MOONCAKE: TO ADD --tiller-image gcr.azk8s.cn/kubernetes-helm/tiller:v2.13.0-->

```azurecli
helm init --upgrade --tiller-image gcr.azk8s.cn/kubernetes-helm/tiller:v2.13.0
```

<!--MOONCAKE: TO ADD --tiller-image gcr.azk8s.cn/kubernetes-helm/tiller:v2.13.0-->

现在，将服务目录图表添加到 Helm 存储库：

```azurecli
helm repo add svc-cat https://svc-catalog-charts.storage.googleapis.com
```

最后，使用 Helm Chart 安装服务目录。 如果群集启用了 RBAC，请运行此命令。

```azurecli
helm install svc-cat/catalog --name catalog --namespace catalog --set apiserver.storage.etcd.persistence.enabled=true --set apiserver.healthcheck.enabled=false --set controllerManager.healthcheck.enabled=false --set apiserver.verbosity=2 --set controllerManager.verbosity=2
```

如果群集未启用 RBAC，请运行以下命令。

```azurecli
helm install svc-cat/catalog --name catalog --namespace catalog --set rbacEnable=false --set apiserver.storage.etcd.persistence.enabled=true --set apiserver.healthcheck.enabled=false --set controllerManager.healthcheck.enabled=false --set apiserver.verbosity=2 --set controllerManager.verbosity=2
```

运行 Helm 图表后，验证 `servicecatalog` 是否出现在以下命令的输出中：

```azurecli
kubectl get apiservice
```

例如，应看到与下面（此处显示的为节选）类似的输出：

```
NAME                                 AGE
v1.                                  10m
v1.authentication.k8s.io             10m
...
v1beta1.servicecatalog.k8s.io        34s
v1beta1.storage.k8s.io               10
```

## <a name="install-open-service-broker-for-azure"></a>安装 Open Service Broker for Azure

下一步是安装 [Open Service Broker for Azure][open-service-broker-azure]，其中包括 Azure 托管服务目录。 可用的 Azure 服务示例包括 Azure Database for PostgreSQL、Azure Database for MySQL 和 Azure SQL 数据库。

首先添加 Open Service Broker for Azure Helm 存储库：

```azurecli
helm repo add azure https://kubernetescharts.blob.core.chinacloudapi.cn/azure
```

使用以下 Azure CLI 命令创建[服务主体][create-service-principal]：

```azurecli
az ad sp create-for-rbac
```

输出应如下所示。 记下 `appId`、`password` 和 `tenant` 值，下一步会用到这些值。

```JSON
{
  "appId": "7248f250-0000-0000-0000-dbdeb8400d85",
  "displayName": "azure-cli-2017-10-15-02-20-15",
  "name": "http://azure-cli-2017-10-15-02-20-15",
  "password": "77851d2c-0000-0000-0000-cb3ebc97975a",
  "tenant": "72f988bf-0000-0000-0000-2d7cd011db47"
}
```

使用上述值设置以下环境变量：

<!--MOONCAKE: Add AZURE_ENVIRONMENT-->

```azurecli
AZURE_CLIENT_ID=<appId>
AZURE_CLIENT_SECRET=<password>
AZURE_TENANT_ID=<tenant>
AZURE_ENVIRONMENT=AzureChinaCloud
```

<!--MOONCAKE: Add AZURE_ENVIRONMENT-->

现在获取 Azure 订阅 ID：

```azurecli
az account show --query id --output tsv
```

再次使用上述值设置以下环境变量：

```azurecli
AZURE_SUBSCRIPTION_ID=[your Azure subscription ID from above]
```

填充完这些环境变量后，现在请执行下列命令，以使用 Helm 图表安装 Open Service Broker for Azure：

<!--MOONCAKE: Correct on --set azure.environment=AzureChinaCloud-->

```azurecli
helm install azure/open-service-broker-azure --name osba --namespace osba \
    --set azure.subscriptionId=$AZURE_SUBSCRIPTION_ID \
    --set azure.tenantId=$AZURE_TENANT_ID \
    --set azure.clientId=$AZURE_CLIENT_ID \
    --set azure.clientSecret=$AZURE_CLIENT_SECRET \
    --set azure.environment=AzureChinaCloud
```

<!--MOONCAKE: Correct on --set azure.environment=AzureChinaCloud-->

OSBA 部署完成后，请安装[服务目录 CLI][service-catalog-cli]，这是易于使用的命令行接口，可用于查询服务代理、服务类、服务计划等。

执行以下命令以安装服务目录 CLI 二进制：

<!--MOONCAKE: CORRECT ON https://servicecatalogcli.blob.core.windows.net/cli/latest-->

```azurecli
curl -sLO https://servicecatalogcli.blob.core.windows.net/cli/latest/$(uname -s)/$(uname -m)/svcat
chmod +x ./svcat
```

<!--MOONCAKE: CORRECT ON https://servicecatalogcli.blob.core.windows.net/cli/latest-->

现在，列出已安装的服务代理：

```azurecli
./svcat get brokers
```

应该会看到与下面类似的输出：

```
  NAME                               URL                                STATUS
+------+--------------------------------------------------------------+--------+
  osba   http://osba-open-service-broker-azure.osba.svc.cluster.local   Ready
```

接下来，列出可用的服务类。 显示的服务类是可通过 Open Service Broker for Azure 预配的可用 Azure 托管服务。

```azurecli
./svcat get classes
```

最后，列出所有可用的服务计划。 服务计划是 Azure 托管服务的服务层级。 例如，对于 Azure Database for MySQL，计划范围为 `basic50`（具有 50 个数据传输单位 (DTU) 的基本层）到 `standard800`（具有 800 个 DTU 的标准层）。

```azurecli
./svcat get plans
```

## <a name="install-wordpress-from-helm-chart-using-azure-database-for-mysql"></a>使用 Azure Database for MySQL 从 Helm 图表安装 WordPress

在此步骤中，使用 Helm 为 WordPress 安装更新的 Helm 图表。 该图表预配 WordPress 可以使用的外部 Azure Database for MySQL 实例。 此过程可能需要几分钟。

<!--MOONCAKE: CORRECT ON --set externalDatabase.azure.location=chinaeast2-->

```azurecli
helm install azure/wordpress --name wordpress --namespace wordpress --set resources.requests.cpu=0 --set replicaCount=1 --set externalDatabase.azure.location=chinaeast2
```

<!--MOONCAKE: CORRECT ON --set externalDatabase.azure.location=chinaeast2-->

为了验证安装是否已预配适当的资源，请列出已安装的服务实例和绑定：

```azurecli
./svcat get instances -n wordpress
./svcat get bindings -n wordpress
```

列出已安装的机密：

```azurecli
kubectl get secrets -n wordpress -o yaml
```

## <a name="next-steps"></a>后续步骤

遵照此文章的说明，已将服务目录部署到 Azure Kubernetes 服务 (AKS) 群集。 并使用 Open Service Broker for Azure 部署了 WordPress 安装，后者使用 Azure 托管服务（在此例中为 Azure Database for MySQL）。

请参阅 [Azure/helm-charts][helm-charts] 存储库，以访问其他已更新的基于 OSBA 的 Helm 图表。 如需了解创建适用于 OSBA 的图表，请参阅[创建新图表][helm-create-new-chart]。

<!-- LINKS - external -->

[helm-charts]: https://github.com/Azure/helm-charts
[helm-cli-install]: https://docs.helm.sh/helm/#helm-install
[helm-create-new-chart]: https://github.com/Azure/helm-charts#creating-a-new-chart
[kubernetes-service-catalog]: https://github.com/kubernetes-incubator/service-catalog
[open-service-broker-azure]: https://github.com/Azure/open-service-broker-azure
[service-catalog-cli]: https://github.com/Azure/service-catalog-cli

<!-- LINKS - internal -->

[azure-cli-install]: https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest

<!--Not Available on [azure-cloud-shell]: ../cloud-shell/overview.md-->

[create-aks-cluster]: ./kubernetes-walkthrough.md
[create-service-principal]: ./kubernetes-service-principal.md

<!--IMPORT: NOT AVAILABLE ON MOONCAKE-->
<!--PLEASE BE KINDLY NOTICE MOONCAKE COMMENTS-->