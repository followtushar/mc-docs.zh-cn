---
title: 将 Docker 映像推送到专用 Azure 注册表
description: 使用 Docker CLI 在 Azure 的专用容器注册表中推送和拉取 Docker 映像
services: container-registry
author: rockboyfor
manager: digimobile
ms.service: container-registry
ms.topic: article
origin.date: 11/29/2017
ms.date: 08/27/2018
ms.author: v-yeche
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6e905d3f9ba46ef9d2a77b10f152384768b263d6
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52662395"
---
# <a name="push-your-first-image-to-a-private-docker-container-registry-using-the-docker-cli"></a>使用 Docker CLI 将第一个映像推送到专用 Docker 容器注册表

Azure 容器注册表存储和管理专用 [Docker](http://hub.docker.com) 容器映像，其方式类似于 [Docker Hub](https://hub.docker.com/) 存储公共 Docker 映像。 可以使用 [Docker 命令行接口](https://docs.docker.com/engine/reference/commandline/cli/) (Docker CLI) 对容器注册表执行[登录](https://docs.docker.com/engine/reference/commandline/login/)、[推送](https://docs.docker.com/engine/reference/commandline/push/)、[提取](https://docs.docker.com/engine/reference/commandline/pull/)和其他操作。

以下步骤从公共 Docker 中心注册表下载正式的 [Nginx 映像](https://store.docker.com/images/nginx)，为专用 Azure 容器注册表标记该映像，将其推入到注册表，然后从注册表提取。

## <a name="prerequisites"></a>先决条件

* **Azure 容器注册表** - 在 Azure 订阅中创建容器注册表。 例如，使用 [Azure 门户](container-registry-get-started-portal.md)或 [Azure CLI](container-registry-get-started-azure-cli.md)。
* **Docker CLI** - 要将本地计算机设置为 Docker 主机并访问 Docker CLI 命令，请安装 [Docker](https://docs.docker.com/engine/installation/)。

## <a name="log-in-to-a-registry"></a>登录到注册表

可[通过多种方式验证](container-registry-authentication.md)专用容器注册表。 在命令行中操作时，建议的方法是使用 Azure CLI 命令 [az acr login](https://docs.azure.cn/zh-cn/cli/acr?view=azure-cli-latest#az-acr-login)。 例如，若要登录到名为 *myregistry* 的注册表：

```azurecli
az acr login --name myregistry
```

<!-- Not Available on  [service principal](../active-directory/active-directory-application-objects.md)-->
<!-- Not Available on docker login myregistry.azurecr.cn -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword-->


完成后，命令将返回 `Login Succeeded`。

<!-- Not Available on docker login cmdlet-->

## <a name="pull-the-official-nginx-image"></a>提取正式的 Nginx 映像

首次将公共 Nginx 映像提取到本地计算机。

```Bash
docker pull nginx
```

## <a name="run-the-container-locally"></a>在本地运行容器

执行以下 [docker run](https://docs.docker.com/engine/reference/run/) 命令，在端口 8080 上以交互方式启动 Nginx 容器的本地实例 (`-it`)。 `--rm` 参数指定在停止容器时应将其删除。

```Bash
docker run -it --rm -p 8080:80 nginx
```

浏览到 [http://localhost:8080](http://localhost:8080)，查看由正在运行的容器中的 Nginx 提供服务的默认网页。 应看到类似于下面的页面：

![本地计算机上的 Nginx](./media/container-registry-get-started-docker-cli/nginx.png)

由于已使用 `-it` 以交互方式启动了容器，因此在浏览器中导航到该容器后，可在命令行中查看 Nginx 服务器的输出。

若要停止并删除容器，请按 `Control`+`C`。

## <a name="create-an-alias-of-the-image"></a>创建映像的别名

运行 [docker tag](https://docs.docker.com/engine/reference/commandline/tag/)，使用注册表的完全限定路径创建映像的别名。 此示例指定了 `samples` 命名空间，以免注册表根目录中出现混乱。

```Bash
docker tag nginx myregistry.azurecr.cn/samples/nginx
```

有关使用命名空间进行标记的详细信息，请参阅 [Azure 容器注册表的最佳做法](container-registry-best-practices.md)的[存储库命名空间](container-registry-best-practices.md#repository-namespaces)部分。

## <a name="push-the-image-to-your-registry"></a>将映像推送到注册表

使用专用注册表的完全限定路径标记映像后，可以使用 [docker push](https://docs.docker.com/engine/reference/commandline/push/) 将其推送到注册表：

```Bash
docker push myregistry.azurecr.cn/samples/nginx
```

## <a name="pull-the-image-from-your-registry"></a>从注册表中提取映像

使用 [docker pull](https://docs.docker.com/engine/reference/commandline/pull/) 命令从注册表提取映像：

```Bash
docker pull myregistry.azurecr.cn/samples/nginx
```

## <a name="start-the-nginx-container"></a>启动 Nginx 容器

使用 [docker run](https://docs.docker.com/engine/reference/run/) 命令运行已从注册表提取的映像：

```Bash
docker run -it --rm -p 8080:80 myregistry.azurecr.cn/samples/nginx
```

浏览到 [http://localhost:8080](http://localhost:8080) 查看正在运行的容器。

若要停止并删除容器，请按 `Control`+`C`。

## <a name="remove-the-image-optional"></a>删除映像（可选）

如果不再需要 Nginx 映像，可以使用 [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) 命令在本地将其删除。

```Bash
docker rmi myregistry.azurecr.cn/samples/nginx
```

若要从 Azure 容器注册表中删除映像，可以使用 Azure CLI 命令[az acr repository delete](https://docs.azure.cn/zh-cn/cli/acr/repository?view=azure-cli-latest#az-acr-repository-delete)。 例如，以下命令删除标记引用的清单、所有关联的层数据，以及引用清单的其他所有标记。

```azurecli
az acr repository delete --name myregistry --repository samples/nginx --tag latest --manifest
```

## <a name="next-steps"></a>后续步骤

了解基础知识后，便可以开始使用注册表了！

<!-- Not Available on  Deploy container images from your registry to:-->
<!-- Not Available on * [Azure Kubernetes Service (AKS)](../aks/tutorial-kubernetes-prepare-app.md)-->
<!-- Not Available on * [Azure Container Instances](../container-instances/container-instances-tutorial-prepare-app.md)-->
<!-- Not Available on * [Service Fabric](../service-fabric/service-fabric-tutorial-create-container-images.md)-->
<!-- Update_Description: update meta properties, wording update -->