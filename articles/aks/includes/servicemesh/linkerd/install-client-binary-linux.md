---
author: rockboyfor
ms.topic: include
origin.date: 10/09/2019
ms.date: 03/09/2020
ms.author: v-yeche
ms.openlocfilehash: 5b990bd2d57980e6e9c52450d736de4ce1d287ee
ms.sourcegitcommit: 3c98f52b6ccca469e598d327cd537caab2fde83f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79290698"
---
## <a name="download-and-install-the-linkerd-linkerd-client-binary"></a>下载并安装 Linkerd linkerd 客户端二进制文件

在 Linux 或[适用于 Linux 的 Windows 子系统][install-wsl]上的基于 bash 的 shell 中，使用 `curl` 下载 Linkerd 发行版，如下所示：

```bash
# Specify the Linkerd version that will be leveraged throughout these instructions
LINKERD_VERSION=stable-2.6.0

curl -sLO "https://github.com/linkerd/linkerd2/releases/download/$LINKERD_VERSION/linkerd2-cli-$LINKERD_VERSION-linux"
```

`linkerd` 客户端二进制文件在客户端计算机上运行，用来与 Linkerd 服务网格交互。 在 Linux 或[适用于 Linux 的 Windows 子系统][install-wsl]上的基于 bash 的 shell 中使用以下命令安装 Linkerd `linkerd` 客户端二进制文件。 这些命令可将 `linkerd` 客户端二进制文件复制到 `PATH` 中的标准用户程序位置。

```bash
sudo cp ./linkerd2-cli-$LINKERD_VERSION-linux /usr/local/bin/linkerd
sudo chmod +x /usr/local/bin/linkerd
```

如果想要通过命令行完成 Linkerd `linkerd` 客户端二进制文件的下载和安装，则按如下所示进行设置：

```bash
# Generate the bash completion file and source it in your current shell
mkdir -p ~/completions && linkerd completion bash > ~/completions/linkerd.bash
source ~/completions/linkerd.bash

# Source the bash completion file in your .bashrc so that the command-line completions
# are permanently available in your shell
echo "source ~/completions/linkerd.bash" >> ~/.bashrc
```

<!-- LINKS - external -->

[install-wsl]: https://docs.microsoft.com/windows/wsl/install-win10

<!-- Update_Description: update meta properties, wording update, update link -->