---
title: Azure IoT 中心设备管理入门 (Node)
description: 如何使用 IoT 中心设备管理进行远程设备重启。 使用 Azure IoT SDK for Node.js 实现包含直接方法的模拟设备应用和调用直接方法的服务应用。
author: juanjperez
manager: cberlin
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
origin.date: 08/25/2017
ms.date: 03/09/2020
ms.author: v-yiso
ms.openlocfilehash: c56047db686de07b89f475af29328d09c82dd46d
ms.sourcegitcommit: d202f6fe068455461c8756b50e52acd4caf2d095
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2020
ms.locfileid: "78154502"
---
# <a name="get-started-with-device-management-nodejs"></a>设备管理入门 (Node.js)

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

本教程演示如何：

* 使用 Azure 门户创建 IoT 中心，以及如何在 IoT 中心创建设备标识。
* 创建包含重新启动该设备的直接方法的模拟设备应用。 直接方法是从云中调用的。
* 创建一个 Node.js 控制台应用，其通过 IoT 中心直接重启模拟设备应用。

本教程结束时，会创建两个 Node.js 控制台应用：

* **dmpatterns_getstarted_device.js**，它使用先前创建的设备标识连接到 IoT 中心，接收重新启动直接方法，模拟物理重新启动，并报告上次重新启动的时间。

* **dmpatterns_getstarted_service.js**，它调用模拟设备应用中的直接方法，显示响应，并显示更新后的报告属性。

## <a name="prerequisites"></a>先决条件

* Node.js 版本 10.0.x 或更高版本。 [准备开发环境](https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md)介绍了如何在 Windows 或 Linux 上安装本教程所用的 Node.js。

* 有效的 Azure 帐户。 （如果没有帐户，只需几分钟即可创建一个[试用帐户][lnk-free-trial]。）
* 确保已在防火墙中打开端口 8883。 本文中的设备示例使用 MQTT 协议，该协议通过端口 8883 进行通信。 在某些公司和教育网络环境中，此端口可能被阻止。 有关解决此问题的更多信息和方法，请参阅[连接到 IoT 中心(MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub)。

## <a name="create-an-iot-hub"></a>创建 IoT 中心

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>在 IoT 中心内注册新设备

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>创建模拟设备应用程序

本部分的操作：

* 创建一个 Node.js 控制台应用，用于响应通过云调用的直接方法
* 触发模拟的设备重启
* 通过报告的属性，设备孪生查询可标识设备及设备上次重新启动的时间

1. 创建名为 **manageddevice** 的空文件夹。  在 **manageddevice** 文件夹的命令提示符处，使用以下命令创建 package.json 文件。  接受所有默认值：

    ```cmd/sh
    npm init
    ```

2. 在 **manageddevice** 文件夹的命令提示符处，运行下述命令以安装 **azure-iot-device** 设备 SDK 包和 **azure-iot-device-mqtt** 包：

    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

3. 在 **manageddevice** 文件夹中，使用文本编辑器创建 **dmpatterns_getstarted_device.js** 文件。

4. 在 **dmpatterns_getstarted_device.js** 文件开头添加以下“require”语句：

    ```javascript
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. 添加 **connectionString** 变量，并使用它创建一个**客户端**实例。  将 `{yourdeviceconnectionstring}` 占位符值替换为先前在[在 IoT 中心注册新设备](#register-a-new-device-in-the-iot-hub)中复制的设备连接字符串。  

    ```javascript
    var connectionString = '{yourdeviceconnectionstring}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. 添加以下函数，实现设备上的直接方法

    ```javascript
    var onReboot = function(request, response) {

        // Respond the cloud app for the direct method
        response.send(200, 'Reboot started', function(err) {
            if (err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });

        // Report the reboot before the physical restart
        var date = new Date();
        var patch = {
            iothubDM : {
                reboot : {
                    lastReboot : date.toISOString(),
                }
            }
        };

        // Get device Twin
        client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                console.log('twin acquired');
                twin.properties.reported.update(patch, function(err) {
                    if (err) throw err;
                    console.log('Device reboot twin state reported')
                });  
            }
        });

        // Add your device's reboot API for physical restart.
        console.log('Rebooting!');
    };
    ```
7. 打开与 IoT 中心的连接并启动直接方法侦听器：

    ```javascript
    client.open(function(err) {
        if (err) {
            console.error('Could not open IotHub client');
        }  else {
            console.log('Client opened.  Waiting for reboot method.');
            client.onDeviceMethod('reboot', onReboot);
        }
    });
    ```
8. 保存并关闭 **dmpatterns_getstarted_device.js** 文件。

   >[!NOTE]
   > 为简单起见，本教程不实现任何重试策略。 在生产代码中，应该按文章 [Transient Fault Handling][lnk-transient-faults]（暂时性故障处理）中所述实施重试策略（例如指数退避）。

## <a name="get-the-iot-hub-connection-string"></a>获取 IoT 中心连接字符串

[!INCLUDE [iot-hub-howto-device-management-shared-access-policy-text](../../includes/iot-hub-howto-device-management-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-service-connection-string](../../includes/iot-hub-include-find-service-connection-string.md)]

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>使用直接方法在设备上触发远程重新启动
本部分中会创建一个 Node.js 控制台应用，该应用使用直接方法在设备上初始化远程重启。 该应用使用设备孪生查询来搜索该设备的上次重新启动时间。

1. 创建一个名为 **triggerrebootondevice** 的空文件夹。  在 **triggerrebootondevice** 文件夹的命令提示符处，使用以下命令创建 package.json 文件。  接受所有默认值：

    ```cmd/sh
    npm init
    ```
2. 在 **triggerrebootondevice** 文件夹的命令提示符处，运行下述命令以安装 **azure-iothub** 设备 SDK 包和 **azure-iot-device-mqtt** 包：

    ```cmd/sh
    npm install azure-iothub --save
    ```

3. 在 **triggerrebootondevice** 文件夹中，使用文本编辑器创建 **dmpatterns_getstarted_service.js** 文件。

4. 在 **dmpatterns_getstarted_service.js** 文件开头添加以下“require”语句：

    ```javascript
    'use strict';

    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```

5. 添加以下变量声明，并将 `{iothubconnectionstring}` 占位符值替换为先前在[获取 IoT 中心连接字符串](#get-the-iot-hub-connection-string)中复制的 IoT 中心连接字符串：

    ```javascript
    var connectionString = '{iothubconnectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToReboot = 'myDeviceId';
    ```
6. 添加以下函数以调用设备方法来重新启动目标设备：

    ```javascript
    var startRebootDevice = function(twin) {

        var methodName = "reboot";

        var methodParams = {
            methodName: methodName,
            payload: null,
            timeoutInSeconds: 30
        };

        client.invokeDeviceMethod(deviceToReboot, methodParams, function(err, result) {
            if (err) { 
                console.error("Direct method error: "+err.message);
            } else {
                console.log("Successfully invoked the device to reboot.");  
            }
        });
    };
    ```
7. 添加以下函数以查询设备并获取上次重新启动时间：

    ```javascript
    var queryTwinLastReboot = function() {

        registry.getTwin(deviceToReboot, function(err, twin){

            if (twin.properties.reported.iothubDM != null)
            {
                if (err) {
                    console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
                } else {
                    var lastRebootTime = twin.properties.reported.iothubDM.reboot.lastReboot;
                    console.log('Last reboot time: ' + JSON.stringify(lastRebootTime, null, 2));
                }
            } else 
                console.log('Waiting for device to report last reboot time.');
        });
    };
    ```
8. 添加以下代码以调用函数，触发重新启动直接方法并查询上次重新启动时间：

    ```javascript
    startRebootDevice();
    setInterval(queryTwinLastReboot, 2000);
    ```

9. 保存并关闭 **dmpatterns_getstarted_service.js** 文件。

## <a name="run-the-apps"></a>运行应用

现已准备好运行应用。

1. 在 **manageddevice** 文件夹的命令提示符处，运行以下命令以开始侦听重新启动直接方法。

    ```cmd/sh
    node dmpatterns_getstarted_device.js
    ```

2. 在 **triggerrebootondevice** 文件夹的命令提示符处运行以下命令，以便触发远程重启并查询设备孪生了解上次重启时间。

    ```cmd/sh
    node dmpatterns_getstarted_service.js
    ```

3. 可以在控制台中看到设备对重新启动直接方法的响应和重新启动状态。

   下面显示了设备对服务发送的重新启动直接方法的响应：

   ![manageddevice 应用输出](./media/iot-hub-node-node-device-management-get-started/device.png)

   下面显示了触发重新启动并轮询设备孪生的上次重新启动时间的服务：

   ![triggerrebootondevice 应用输出](./media/iot-hub-node-node-device-management-get-started/service.png)
[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]

<!-- images and links -->
[img-output]: ./media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: ./media/iot-hub-get-started-with-dm/dmui.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md

[lnk-free-trial]: https://www.azure.cn/pricing/1rmb-trial/
[Azure portal]: https://portal.azure.cn/
[Using resource groups to manage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management

[lnk-devtwin]: ./iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: ./iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/zh-cn/library/hh680901(v=pandp.50).aspx