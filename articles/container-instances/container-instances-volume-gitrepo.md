---
title: 将 gitRepo 卷装载到容器组
description: 了解如何在容器实例中装载 gitRepo 卷以克隆 Git 存储库
ms.topic: article
origin.date: 06/15/2018
ms.date: 01/15/2020
ms.author: v-yeche
ms.openlocfilehash: 5d9bc646bf080f1f8df1ad1a98085cd7a002258d
ms.sourcegitcommit: ada94ca4685855f58616e4bf1dd5ca757878dfdc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/18/2020
ms.locfileid: "77428292"
---
<!--Verified successfully-->
# <a name="mount-a-gitrepo-volume-in-azure-container-instances"></a>在 Azure 容器实例中装载 gitRepo 卷

了解如何在容器实例中装载 *gitRepo* 卷以克隆 Git 存储库。

> [!NOTE]
> 当前只有 Linux 容器能装载 *gitRepo* 卷。 虽然我们正致力于为 Windows 容器提供全部功能，但你可在[概述](container-instances-overview.md#linux-and-windows-containers)中了解当前的平台差异。

## <a name="gitrepo-volume"></a>gitRepo 卷

*gitRepo* 卷可在容器启动时装载目录，并将指定的 Git 存储库克隆到该目录。 在容器实例中使用 *gitRepo* 卷，可避免在应用程序中为执行此操作添加代码。

装载 *gitRepo* 卷时，可以设置三个属性以对卷进行配置：

| 属性 | 必须 | 说明 |
| -------- | -------- | ----------- |
| `repository` | 是 | 要克隆的 Git 存储库的完整 URL，包括 `http://` 或 `https://`。|
| `directory` | 否 | 存储库应克隆到的目录。 路径不得包含“`..`”，也不能以其开头。  如果指定“`.`”，存储库将克隆到卷的目录。 否则，Git 存储库将克隆到卷目录中给定名称的子目录。 |
| `revision` | 否 | 要克隆的修订的提交哈希。 如果未指定，则克隆 `HEAD` 修订。 |

## <a name="mount-gitrepo-volume-azure-cli"></a>装载 gitRepo 卷：Azure CLI

若要在使用 [Azure CLI](https://docs.azure.cn/cli/index?view=azure-cli-latest) 部署容器实例时装载 gitRepo 卷，请在 [az container create][az-container-create] 命令中提供 `--gitrepo-url` 和 `--gitrepo-mount-path` 参数。 还可以指定要将卷克隆到其中的目录 (`--gitrepo-dir`) 和要克隆的修订版的提交哈希 (`--gitrepo-revision`)。

此示例命令将 Microsoft [aci-helloworld][aci-helloworld] 示例应用程序克隆到容器实例中的 `/mnt/aci-helloworld`：

<!--Not Avaialble on Microsoft [aci-helloworld]-->

```azurecli
az container create \
    --resource-group myResourceGroup \
    --name hellogitrepo \
    --image mcr.microsoft.com/azuredocs/aci-helloworld \
    --dns-name-label aci-demo \
    --ports 80 \
    --gitrepo-url https://github.com/Azure-Samples/aci-helloworld \
    --gitrepo-mount-path /mnt/aci-helloworld
```

若要验证 gitRepo 卷是否已装载，请使用 [az container exec][az-container-exec] 在该容器中启动 shell 并列出目录：

```console
$ az container exec --resource-group myResourceGroup --name hellogitrepo --exec-command /bin/sh
/usr/src/app # ls -l /mnt/aci-helloworld/
total 16
-rw-r--r--    1 root     root           144 Apr 16 16:35 Dockerfile
-rw-r--r--    1 root     root          1162 Apr 16 16:35 LICENSE
-rw-r--r--    1 root     root          1237 Apr 16 16:35 README.md
drwxr-xr-x    2 root     root          4096 Apr 16 16:35 app
```

## <a name="mount-gitrepo-volume-resource-manager"></a>装载 gitRepo 卷：Resource Manager

若要在使用 Azure 资源管理器模板部署容器实例时装载 gitRepo 卷，请首先填充模板的容器组 `properties` 节中的 `volumes` 数组。 然后，针对容器组中希望装载 *gitRepo* 卷的每个容器，在容器定义的 `properties` 节中填充 `volumeMounts` 数组。

<!--Not Avaiable on [Azure Resource Manager template](https://docs.microsoft.com/azure/templates/microsoft.containerinstance/containergroups)-->

例如，以下资源管理器模板创建了一个包含单个容器的容器组。 该容器克隆由 *gitRepo* 卷块指定的两个 GitHub 存储库。 第二个卷包括其他属性以指定要克隆到的目录和要克隆的特定修订的提交哈希。

<!-- https://github.com/Azure/azure-docs-json-samples/blob/master/container-instances/aci-deploy-volume-gitrepo.json -->

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "variables": {
    "container1name": "aci-tutorial-app",
    "container1image": "microsoft/aci-helloworld:latest"
  },
  "resources": [
    {
      "name": "volume-demo-gitrepo",
      "type": "Microsoft.ContainerInstance/containerGroups",
      "apiVersion": "2018-02-01-preview",
      "location": "[resourceGroup().location]",
      "properties": {
        "containers": [
          {
            "name": "[variables('container1name')]",
            "properties": {
              "image": "[variables('container1image')]",
              "resources": {
                "requests": {
                  "cpu": 1,
                  "memoryInGb": 1.5
                }
              },
              "ports": [
                {
                  "port": 80
                }
              ],
              "volumeMounts": [
                {
                  "name": "gitrepo1",
                  "mountPath": "/mnt/repo1"
                },
                {
                  "name": "gitrepo2",
                  "mountPath": "/mnt/repo2"
                }
              ]
            }
          }
        ],
        "osType": "Linux",
        "ipAddress": {
          "type": "Public",
          "ports": [
            {
              "protocol": "tcp",
              "port": "80"
            }
          ]
        },
        "volumes": [
          {
            "name": "gitrepo1",
            "gitRepo": {
              "repository": "https://github.com/Azure-Samples/aci-helloworld"
            }
          },
          {
            "name": "gitrepo2",
            "gitRepo": {
              "directory": "my-custom-clone-directory",
              "repository": "https://github.com/Azure-Samples/aci-helloworld",
              "revision": "d5ccfcedc0d81f7ca5e3dbe6e5a7705b579101f1"
            }
          }
        ]
      }
    }
  ]
}
```

前面的模板中定义的两个克隆存储库的生成目录结构如下：

```
/mnt/repo1/aci-helloworld
/mnt/repo2/my-custom-clone-directory
```

若要查看使用 Azure 资源管理器模板进行的容器实例部署示例，请参阅[在 Azure 容器实例中部署多容器组](container-instances-multi-container-group.md)。

## <a name="private-git-repo-authentication"></a>专用 Git 存储库身份验证

若要为专用 Git 存储库装载 gitRepo 卷，请在存储库 URL 中指定凭据。 通常，凭据采用用户名和个人访问令牌 (PAT) 的形式，授予对存储库的范围访问权限。

例如，专用 GitHub 存储库的 Azure CLI `--gitrepo-url` 参数将类似于以下内容（其中“gituser”是 GitHub 用户名，“abcdef1234fdsa4321abcdef”是用户的个人访问令牌）：

```azurecli
--gitrepo-url https://gituser:abcdef1234fdsa4321abcdef@github.com/GitUser/some-private-repository
```

对于 Azure Repos Git 存储库，请指定任何用户名（可以使用“azurereposuser”，如下例所示）并结合有效的 PAT：

```azurecli
--gitrepo-url https://azurereposuser:abcdef1234fdsa4321abcdef@dev.azure.com/your-org/_git/some-private-repository
```

有关 GitHub 和 Azure Repos 的个人访问令牌的详细信息，请参阅以下内容：

GitHub：[创建命令行的个人访问令牌][pat-github]

<!--Not Available on Azure Repos: [Create personal access tokens to authenticate access][pat-repos]-->

## <a name="next-steps"></a>后续步骤

了解如何在 Azure 容器实例中装载其他卷类型：

* [在 Azure 容器实例中装载 Azure 文件共享](container-instances-volume-azure-files.md)
* [在 Azure 容器实例中装载 emptyDir 卷](container-instances-volume-emptydir.md)
* [在 Azure 容器实例中装载机密卷](container-instances-volume-secret.md)

<!-- LINKS - External -->

[aci-helloworld]: https://github.com/Azure-Samples/aci-helloworld
[pat-github]: https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/

<!--Not Available on [pat-repos]: /devops/organizations/accounts/use-personal-access-tokens-to-authenticate-->

<!-- LINKS - Internal -->

[az-container-create]: https://docs.microsoft.com/cli/azure/container?view=azure-cli-latest#az-container-create
[az-container-exec]: https://docs.microsoft.com/cli/azure/container?view=azure-cli-latest#az-container-exec

<!-- Update_Description: new article about container instances volume gitrepo -->
<!--NEW.date: 01/15/2020-->