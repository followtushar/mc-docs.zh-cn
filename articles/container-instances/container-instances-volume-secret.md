---
title: 将机密卷装载到容器组
description: 了解如何装载机密卷以存储供容器实例访问的敏感信息
ms.topic: article
origin.date: 07/19/2018
ms.date: 01/15/2020
ms.author: v-yeche
ms.openlocfilehash: 9323da4baf7fc43f6df035028f9e4f34d2ffc113
ms.sourcegitcommit: 2b4507745b98b45f1ce3f3d30f397521148ef35a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2020
ms.locfileid: "78213734"
---
<!--Verified successfully-->
# <a name="mount-a-secret-volume-in-azure-container-instances"></a>在 Azure 容器实例中装载机密卷

可以使用机密  卷向容器组中的容器提供敏感信息。 机密  卷将机密存储在该卷内的文件中，然后容器组中的容器可以访问这些机密。 将机密存储在机密  卷中，可以避免将敏感数据（例如，SSH 密钥或数据库凭据）添加到应用程序代码中。

所有机密  卷均由 [tmpfs][tmpfs] 提供支持，后者是一个支持 RAM 的文件系统，其内容永远不会写入到非易失性存储。

> [!NOTE]
> 机密  卷目前仅限于 Linux 容器。 在[设置环境变量](container-instances-environment-variables.md)中了解如何为 Linux 容器传递安全环境变量。

<!--Not Available on both Windows and Linux container-->
<!--Not Available on  While we're working to bring all features to Windows containers, you can find current platform differences in the [overview](container-instances-overview.md#linux-and-windows-containers)-->

## <a name="mount-secret-volume---azure-cli"></a>装载机密卷 - Azure CLI

若要使用 Azure CLI 部署包含一个或多个机密的容器，请在 [az container create][az-container-create] 命令中包含 `--secrets` 和 `--secrets-mount-path` 参数。 此示例在 `/mnt/secrets` 处装载一个机密  卷，其中包含两个机密 “mysecret1”和“mysecret2”：

```azurecli
az container create \
    --resource-group myResourceGroup \
    --name secret-volume-demo \
    --image mcr.microsoft.com/azuredocs/aci-helloworld \
    --secrets mysecret1="My first secret FOO" mysecret2="My second secret BAR" \
    --secrets-mount-path /mnt/secrets
```

以下 [az container exec][az-container-exec] 输出演示在运行的容器中打开 shell，列出机密卷中的文件，然后显示其内容：

```console
$ az container exec --resource-group myResourceGroup --name secret-volume-demo --exec-command "/bin/sh"
/usr/src/app # ls -1 /mnt/secrets
mysecret1
mysecret2
/usr/src/app # cat /mnt/secrets/mysecret1
My first secret FOO
/usr/src/app # cat /mnt/secrets/mysecret2
My second secret BAR
/usr/src/app # exit
Bye.
```

## <a name="mount-secret-volume---yaml"></a>装载机密卷 - YAML

还可以使用 Azure CLI 和 [YAML 模板](container-instances-multi-container-yaml.md)部署容器组。 在部署由多个容器组成的容器组时，通过 YAML 模板进行部署是首选方法。

使用 YAML 模板进行部署时，模板中的机密值必须已进行 **Base64 编码**。 但是，机密值会以明文形式显示在容器的文件中。

以下 YAML 模板定义了一个容器组，其中包含一个容器，该容器在 `/mnt/secrets` 处装载了一个机密  卷。 机密卷有两个机密：“mysecret1”和“mysecret2”。

```yaml
apiVersion: '2018-10-01'
location: chinaeast2
name: secret-volume-demo
properties:
  containers:
  - name: aci-tutorial-app
    properties:
      environmentVariables: []
      image: mcr.microsoft.com/azuredocs/aci-helloworld:latest
      ports: []
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
      volumeMounts:
      - mountPath: /mnt/secrets
        name: secretvolume1
  osType: Linux
  restartPolicy: Always
  volumes:
  - name: secretvolume1
    secret:
      mysecret1: TXkgZmlyc3Qgc2VjcmV0IEZPTwo=
      mysecret2: TXkgc2Vjb25kIHNlY3JldCBCQVIK
tags: {}
type: Microsoft.ContainerInstance/containerGroups
```

若要使用 YAML 模板进行部署，请将前面的 YAML 保存到名为 `deploy-aci.yaml` 的文件中，然后使用 `--file` 参数执行 [az container create][az-container-create] 命令：

```azurecli
# Deploy with YAML template
az container create --resource-group myResourceGroup --file deploy-aci.yaml
```

## <a name="mount-secret-volume---resource-manager"></a>装载机密卷 - 资源管理器

除了 CLI 和 YAML 部署外，还可以使用 Azure [资源管理器模板](https://docs.microsoft.com/azure/templates/microsoft.containerinstance/containergroups)部署容器组。

首先，在模板的容器组 `properties` 节中填充 `volumes` 数组。 使用资源管理器模板进行部署时，模板中的机密值必须已进行 **Base64 编码**。 但是，机密值会以明文形式显示在容器的文件中。

接下来，针对容器组中希望装载*机密*卷的每个容器，在容器定义的 `properties` 节中填充 `volumeMounts` 数组。

以下资源管理器模板定义了一个容器组，其中包含一个容器，该容器在 `/mnt/secrets` 处装载了一个机密  卷。 机密卷有两个机密：“mysecret1”和“mysecret2”。

<!-- https://github.com/Azure/azure-docs-json-samples/blob/master/container-instances/aci-deploy-volume-secret.json -->

```json
apiVersion: '2018-10-01'
location: eastus
name: secret-volume-demo
properties:
  containers:
  - name: aci-tutorial-app
    properties:
      environmentVariables: []
      image: mcr.microsoft.com/azuredocs/aci-helloworld:latest
      ports: []
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
      volumeMounts:
      - mountPath: /mnt/secrets
        name: secretvolume1
  osType: Linux
  restartPolicy: Always
  volumes:
  - name: secretvolume1
    secret:
      mysecret1: TXkgZmlyc3Qgc2VjcmV0IEZPTwo=
      mysecret2: TXkgc2Vjb25kIHNlY3JldCBCQVIK
tags: {}
type: Microsoft.ContainerInstance/containerGroups
```

若要使用资源管理器模板进行部署，请将前面的 JSON 保存到名为 `deploy-aci.json` 的文件中，然后使用 `--template-file` 参数执行 [az group deployment create][az-group-deployment-create] 命令：

```azurecli
# Deploy with Resource Manager template
az group deployment create --resource-group myResourceGroup --template-file deploy-aci.json
```

## <a name="next-steps"></a>后续步骤

### <a name="volumes"></a>卷

了解如何在 Azure 容器实例中装载其他卷类型：

* [在 Azure 容器实例中装载 Azure 文件共享](container-instances-volume-azure-files.md)
* [在 Azure 容器实例中装载 emptyDir 卷](container-instances-volume-emptydir.md)
* [在 Azure 容器实例中装载 gitRepo 卷](container-instances-volume-gitrepo.md)

### <a name="secure-environment-variables"></a>安全环境变量

向容器（包括 Windows 容器）提供敏感信息的另一种方法是通过使用[安全环境变量](container-instances-environment-variables.md#secure-values)。

<!-- LINKS - External -->

[tmpfs]: https://wikipedia.org/wiki/Tmpfs

<!-- LINKS - Internal -->

[az-container-create]: https://docs.microsoft.com/cli/azure/container?view=azure-cli-latest#az-container-create
[az-container-exec]: https://docs.microsoft.com/cli/azure/container?view=azure-cli-latest#az-container-exec
[az-group-deployment-create]: https://docs.azure.cn/cli/group/deployment?view=azure-cli-latest#az-group-deployment-create

<!-- Update_Description: new article about container instances volume secret -->
<!--NEW.date: 01/15/2020-->