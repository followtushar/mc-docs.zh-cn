## <a name="sign-in-to-azure"></a>登录 Azure

将使用 Azure CLI 创建在 Azure 中托管应用所需的资源。 使用 [az login](https://docs.microsoft.com/cli/azure/#login) 命令登录到 Azure 订阅，并按照屏幕上的说明进行操作。

```azurecli
az login
```

> [!NOTE]
> 在 Azure China 中使用 Azure CLI 2.0 之前，请首先运行 `az cloud set -n AzureChinaCloud` 更改云环境。 如果要切换回全局 Azure，请再次运行 `az cloud set -n AzureCloud`。

<!-- ms.date: 08/15/2018 -->