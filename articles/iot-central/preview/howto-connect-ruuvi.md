---
title: 在 Azure IoT Central 中连接 RuuviTag | Microsoft Docs
description: 了解如何将 RuuviTag 环境传感器连接到 IoT Central 应用程序。
services: iot-central
ms.service: iot-central
ms.topic: conceptual
ms.custom:
- iot-storeAnalytics-conditionMonitor
- iot-p0-scenario
ms.author: v-yiso
author: avneet723
origin.date: 10/19/2019
ms.date: 12/16/2019
ms.openlocfilehash: bf438d9e971a19c086362f70cc4d52ce202bfd93
ms.sourcegitcommit: 6ffa4d50cee80c7c0944e215ca917a248f2a4bcd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2019
ms.locfileid: "74883052"
---
# <a name="connect-a-ruuvitag-sensor-to-your-azure-iot-central-application"></a>将 RuuviTag 传感器连接到 Azure IoT Central 应用程序

本文为解决方案构建人员介绍如何将 RuuviTag 传感器连接到 Microsoft Azure IoT Central 应用程序。

什么是 RuuviTag？

RuuviTag 是一个高级开源传感器信标平台，旨在满足企业客户、开发人员、制作人员、学生和爱好者的需求。 该设备开箱即用，随时可部署到所需的任何位置。 它是一个 Bluetooth LE 信标，内置了环境传感器和加速度传感器。

RuuviTag 通过 BLE（低能耗蓝牙）通信，要求网关设备与 Azure IoT Central 通信。 请确保已设置好一个网关设备（例如 Rigado Cascade 500），使 RuuviTag 能够连接到 IoT Central。

若要设置 Rigado Cascade 500 网关设备，请按照[此处的说明](./howto-connect-rigado-cascade-500.md)操作。

## <a name="prerequisites"></a>先决条件

若要连接 RuuviTag 传感器，需准备好以下资源：

* 一个 RuuviTag 传感器。 有关详细信息，请访问 [RuuviTag](https://ruuvi.com/)。
* Rigado Cascade 500 设备或其他 BLE 网关。 有关详细信息，请访问 [Rigado](https://www.rigado.com/)。
* 从某个预览版应用程序模板创建的 Azure IoT Central 应用程序。 有关详细信息，请参阅[创建新应用程序](./quick-deploy-iot-central.md)。

## <a name="add-a-ruuvitag-device-template"></a>添加 RuuviTag 设备模板

若要将 RuuviTag 传感器加入到 Azure IoT Central 应用程序实例，需要在应用程序中配置相应的设备模板。

若要添加 RuuviTag 设备模板：

1. 在左侧窗格中导航到“设备模板”选项卡，选择“+ 新建”：  ![创建新设备模板](./media/howto-connect-ruuvi/devicetemplate-new.png) 页面中提供了“创建自定义模板”或“使用预配置的设备模板”选项
1. 按如下所示，从预配置的设备模板列表中选择 RuuviTag 设备模板：![选择 RuuviTag 设备模板](./media/howto-connect-ruuvi/devicetemplate-preconfigured.png)
1. 在完成时选择“下一步:自定义”以继续执行下一步。
1. 在下一个屏幕上，选择“创建”以将 C500 设备模板加入到 IoT Central 应用程序中。

## <a name="connect-a-ruuvitag-sensor"></a>连接 RuuviTag 传感器

如前所述，若要将 RuuviTag 连接到 IoT Central 应用程序，需要设置一个网关设备。 以下步骤假设已设置 Rigado Cascade 500 网关设备。  

1. 打开 Rigado Cascade 500 设备，并将其连接到网络连接（通过以太网或无线方式）
1. 弹开 RuuviTag 的护盖，拉动塑料扣环以稳固电池的连接。
1. 将 RuuviTag 放到已在 IoT Central 应用程序中配置的 Rigado Cascade 500 网关的旁边。
1. 几秒钟后，RuuviTag 就会出现在 IoT Central 中的设备列表内。  
    ![RuuviTag 设备列表](./media/howto-connect-ruuvi/ruuvi-devicelist.png)

现在，可以在 IoT Central 应用程序中使用此 RuuviTag 了。  

## <a name="create-a-simulated-ruuvitag"></a>创建模拟的 RuuviTag

如果没有 RuuviTag 物理设备，可以创建一个模拟的 RuuviTag 传感器用于在 Azure IoT Central 应用程序中进行测试。

若要创建模拟的 RuuviTag：

1. 选择“设备”>“RuuviTag”。 
1. 选择“+ 新建”  。
1. 指定唯一的**设备 ID** 和易记的**设备名称**。  
1. 启用“模拟”设置。 
1. 选择“创建”  。  

## <a name="next-steps"></a>后续步骤

了解如何将 RuuviTag 连接到 Azure IoT Central 应用程序后，建议接下来了解如何[自定义 IoT Central 应用程序](../retail/tutorial-in-store-analytics-customize-dashboard-pnp.md)来构建端到端的解决方案。
