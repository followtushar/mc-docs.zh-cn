---
title: 快速入门：Python SDK
titleSuffix: Azure Cognitive Services
description: 本快速入门介绍如何使用 Python SDK 完成常见任务。
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
origin.date: 02/28/2019
ms.date: 03/26/2019
ms.author: v-junlch
ms.openlocfilehash: 5218ea4bfee93ffeb9ee31acc7eb07ed1b7e3b3f
ms.sourcegitcommit: c5599eb7dfe9fd5fe725b82a861c97605635a73f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2019
ms.locfileid: "58505556"
---
# <a name="azure-cognitive-services-computer-vision-sdk-for-python"></a>适用于 Python 的 Azure 认知服务计算机视觉 SDK

使用计算机视觉服务，开发人员可以访问用于处理图像并返回信息的高级算法。 计算机视觉算法根据你感兴趣的视觉特征，通过不同的方式分析图像的内容。 

* [分析图像](#analyze-an-image)
* [获取主题域列表](#get-subject-domain-list)
* [按域分析图像](#analyze-an-image-by-domain)
* [获取图像的文本说明](#get-text-description-of-an-image)
* [获取图像中的手写文本](#get-text-from-image)
* [生成缩略图](#generate-thumbnail)

有关此服务的详细信息，请参阅[什么是计算机视觉？][computervision_docs]。

想要更多文档？

* [SDK 参考文档](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision)
* [认知服务计算机视觉文档](/cognitive-services/computer-vision/)

## <a name="prerequisites"></a>先决条件

* [Python 3.6+][python]
* 免费的[计算机视觉密钥][computervision_resource]和关联的区域。 创建 `ComputerVisionAPI` 客户端对象的实例时需要使用这些值。 使用以下其中一种方法获取这些值。 

### <a name="if-you-dont-have-an-azure-subscription"></a>如果你没有 Azure 订阅

创建有效期为 7 天的免费密钥，获得计算机视觉服务的**[试用][computervision_resource]** 体验。 创建密钥后，复制密钥和区域名称。 需要这些来[创建客户端](#create-client)。

创建密钥后，保留以下项：

* 密钥值：32 个字符的字符串，格式为 `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx` 
* 密钥区域：终结点 URL https://api.cognitive.azure.cn 的子域

### <a name="if-you-have-an-azure-subscription"></a>如果你拥有 Azure 订阅

在订阅中创建资源的最简单方法是使用以下 [Azure CLI][azure_cli] 命令。 这样会创建一个认知服务密钥，该密钥可以在许多认知服务中使用。 需要选择现有的资源组名称（例如“my-cogserv-group”）和新的计算机视觉资源名称（例如“my-computer-vision-resource”）。 

```Bash
RES_REGION=chinanorth 
RES_GROUP=<resourcegroup-name>
ACCT_NAME=<computervision-account-name>

az cognitiveservices account create \
    --resource-group $RES_GROUP \
    --name $ACCT_NAME \
    --location $RES_REGION \
    --kind CognitiveServices \
    --sku S0 \
    --yes
```

<!--
## Installation

Install the Azure Cognitive Services Computer Vision SDK with [pip][pip], optionally within a [virtual environment][venv].

### Configure a virtual environment (optional)

Although not required, you can keep your base system and Azure SDK environments isolated from one another if you use a [virtual environment][virtualenv]. Execute the following commands to configure and then enter a virtual environment with [venv][venv], such as `cogsrv-vision-env`:

```Bash
python3 -m venv cogsrv-vision-env
source cogsrv-vision-env/bin/activate
```
-->

### <a name="install-the-sdk"></a>安装 SDK

安装包含 [pip][pip] 的适用于 Python 的 Azure 认知服务计算机视觉 SDK [包][pypi_computervision]：

```Bash
pip install azure-cognitiveservices-vision-computervision
```

## <a name="authentication"></a>身份验证

创建计算机视觉资源后，需要使用该资源的**区域**及其**帐户密钥**之一来实例化客户端对象。

创建 `ComputerVisionAPI` 客户端对象的实例时需要使用这些值。 

例如，使用 Bash 终端设置环境变量：

```Bash
ACCOUNT_REGION=<resourcegroup-name>
ACCT_NAME=<computervision-account-name>
```

### <a name="for-azure-subscription-users-get-credentials-for-key-and-region"></a>对于 Azure 订阅用户，请获取密钥和区域的凭据

如果忘记了区域和密钥，可以使用以下方法找到它们。 如需创建密钥和区域，则可使用适用于 [Azure 订阅持有人](#if-you-have-an-azure-subscription)的方法，或者使用适用于[没有 Azure 订阅的用户](#if-you-dont-have-an-azure-subscription)的方法。

使用 Azure powershell 在两个环境变量中填充计算机视觉帐户的**区域**及其**密钥**之一（也可以在 [Azure 门户][azure_portal]中找到这些值）。 此代码片段已针对 Bash shell 格式化。

```Bash
RES_GROUP=<resourcegroup-name>
ACCT_NAME=<computervision-account-name>

export ACCOUNT_REGION=$(az cognitiveservices account show \
    --resource-group $RES_GROUP \
    --name $ACCT_NAME \
    --query location \
    --output tsv)

export ACCOUNT_KEY=$(az cognitiveservices account keys list \
    --resource-group $RES_GROUP \
    --name $ACCT_NAME \
    --query key1 \
    --output tsv)
```


### <a name="create-client"></a>创建客户端

从环境变量获取区域和密钥，然后创建 `ComputerVisionAPI` 客户端对象。  

```Python
from azure.cognitiveservices.vision.computervision import ComputerVisionAPI
from azure.cognitiveservices.vision.computervision.models import VisualFeatureTypes
from msrest.authentication import CognitiveServicesCredentials

# Get region and key from environment variables
import os
region = os.environ['ACCOUNT_REGION']
key = os.environ['ACCOUNT_KEY']

# Set credentials
credentials = CognitiveServicesCredentials(key)

# Create client
client = ComputerVisionAPI(region, credentials)
```

## <a name="examples"></a>示例

在使用以下任何任务前，你需要 `ComputerVisionAPI` 客户端对象。

### <a name="analyze-an-image"></a>分析图像

可以使用 `analyze_image` 分析图像中的某些特征。 使用 [`visual_features`][ref_computervision_model_visualfeatures] 属性设置针对图像执行的分析类型。 常用值为 `VisualFeatureTypes.tags` 和 `VisualFeatureTypes.description`。

```Python
url = "https://upload.wikimedia.org/wikipedia/commons/thumb/1/12/Broadway_and_Times_Square_by_night.jpg/450px-Broadway_and_Times_Square_by_night.jpg"

image_analysis = client.analyze_image(url,visual_features=[VisualFeatureTypes.tags])

for tag in image_analysis.tags:
    print(tag)
```

### <a name="get-subject-domain-list"></a>获取主题域列表

使用 `list_models` 查看用于分析图像的主题域。 [按域分析图像](#analyze-an-image-by-domain)时将使用这些域名。 域的示例是 `landmarks`。

```Python
models = client.list_models()

for x in models.models_property:
    print(x)
```

### <a name="analyze-an-image-by-domain"></a>按域分析图像

可以使用 `analyze_image_by_domain` 按主题域分析图像。 获取[支持的主题域列表](#get-subject-domain-list)，以使用正确的域名。  

```Python
# type of prediction
domain = "landmarks"

# Public domain image of Eiffel tower
url = "https://images.pexels.com/photos/338515/pexels-photo-338515.jpeg"

# English language response
language = "en"

analysis = client.analyze_image_by_domain(domain, url, language)

for landmark in analysis.result["landmarks"]:
    print(landmark["name"])
    print(landmark["confidence"])
```

### <a name="get-text-description-of-an-image"></a>获取图像的文本说明

可以使用 `describe_image` 获取图像的基于语言的文本说明。 如果你要针对与图像关联的关键字执行文本分析，请使用 `max_description` 属性请求多个说明。 以下图像的文本说明示例包括 `a train crossing a bridge over a body of water`、`a large bridge over a body of water` 和 `a train crossing a bridge over a large body of water`。

```Python
domain = "landmarks"
url = "http://www.public-domain-photos.com/free-stock-photos-4/travel/san-francisco/golden-gate-bridge-in-san-francisco.jpg"
language = "en"
max_descriptions = 3

analysis = client.describe_image(url, max_descriptions, language)

for caption in analysis.captions:
    print(caption.text)
    print(caption.confidence)
```

### <a name="get-text-from-image"></a>获取图像中的文本

可以从图像中获取任何手写或打印的文本。 这需要对 SDK 进行两次调用：`recognize_text` 和 `get_text_operation_result`。 对 recognize_text 的调用是异步的。 在 get_text_operation_result 调用的结果中，需要先使用 [`TextOperationStatusCodes`][ref_computervision_model_textoperationstatuscodes] 检查第一次调用是否已完成，然后提取文本数据。 结果包括文本以及该文本的边框坐标。 

```Python
# import models
from azure.cognitiveservices.vision.computervision.models import TextRecognitionMode
from azure.cognitiveservices.vision.computervision.models import TextOperationStatusCodes

url = "https://azurecomcdn.azureedge.net/cvt-1979217d3d0d31c5c87cbd991bccfee2d184b55eeb4081200012bdaf6a65601a/images/shared/cognitive-services-demos/read-text/read-1-thumbnail.png"
mode = TextRecognitionMode.handwritten
raw = True
custom_headers = None
numberOfCharsInOperationId = 36

# Async SDK call
rawHttpResponse = client.recognize_text(url, mode, custom_headers,  raw)

# Get ID from returned headers
operationLocation = rawHttpResponse.headers["Operation-Location"]
idLocation = len(operationLocation) - numberOfCharsInOperationId
operationId = operationLocation[idLocation:]

# SDK call
while result.status in ['NotStarted', 'Running']:
    time.sleep(1)
    result = client.get_text_operation_result(operationId)

# Get data
if result.status == TextOperationStatusCodes.succeeded:

    for line in result.recognition_result.lines:
        print(line.text)
        print(line.bounding_box)
```

### <a name="generate-thumbnail"></a>生成缩略图

可以使用 `generate_thumbnail` 生成图像的缩略图 (JPG)。 缩略图的比例不需要与原始图像相同。 

安装 Pillow 以使用此示例：

```bash
pip install Pillow
``` 

Pillow 安装后，使用以下代码示例中的包来生成缩略图。

```Python
# Pillow package
from PIL import Image

# IO package to create local image
import io

width = 50
height = 50
url = "http://www.public-domain-photos.com/free-stock-photos-4/travel/san-francisco/golden-gate-bridge-in-san-francisco.jpg"

thumbnail = client.generate_thumbnail(width, height, url)

for x in thumbnail:
    image = Image.open(io.BytesIO(x))

image.save('thumbnail.jpg')
```

## <a name="troubleshooting"></a>故障排除

### <a name="general"></a>常规

使用 Python SDK 与 `ComputerVisionAPI` 客户端对象交互时，将使用 [`ComputerVisionErrorException`][ref_computervision_computervisionerrorexception] 类返回错误。 服务返回的错误对应于返回给 REST API 请求的相同 HTTP 状态代码。

例如，如果你尝试使用无效的密钥分析图像，则会返回 `401` 错误。 以下代码片段通过捕获异常并显示有关错误的其他信息来妥善处理该[错误][ref_httpfailure]。

```Python

domain = "landmarks"
url = "http://www.public-domain-photos.com/free-stock-photos-4/travel/san-francisco/golden-gate-bridge-in-san-francisco.jpg"
language = "en"
max_descriptions = 3

try:
    analysis = client.describe_image(url, max_descriptions, language)

    for caption in analysis.captions:
        print(caption.text)
        print(caption.confidence)
except HTTPFailure as e:
    if e.status_code == 401:
        print("Error unauthorized. Make sure your key and region are correct.")
    else:
        raise
```

### <a name="handle-transient-errors-with-retries"></a>使用重试处理暂时性错误

使用 `ComputerVisionAPI` 客户端时，可能会遇到服务强制实施的[速率限制][computervision_request_units]所导致的暂时性错误，或者网络中断等其他暂时性问题。 

### <a name="more-sample-code"></a>更多示例代码

SDK 的 GitHub 存储库中提供了多个计算机视觉 Python SDK 示例。 这些示例提供了在使用计算机视觉时经常遇到的其他场景的示例代码：

* [recognize_text][recognize-text]

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [将内容标记应用于图像](../concept-tagging-images.md)

<!-- LINKS -->
[pip]: https://pypi.org/project/pip/
[python]: https://www.python.org/downloads/

[azure_cli]: https://docs.microsoft.com/en-us/cli/azure/cognitiveservices/account?view=azure-cli-latest#az-cognitiveservices-account-create
[azure_portal]: https://portal.azure.cn
[azure_sub]: https://www.azure.cn/pricing/1rmb-trial/

[venv]: https://docs.python.org/3/library/venv.html
[virtualenv]: https://virtualenv.pypa.io

[source_code]: https://github.com/Azure/azure-sdk-for-python/tree/master/azure-cognitiveservices-vision-computervision

[pypi_computervision]:https://pypi.org/project/azure-cognitiveservices-vision-computervision/
[pypi_pillow]:https://pypi.org/project/Pillow/

[ref_computervision_sdk]: https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision?view=azure-python
[ref_computervision_computervisionerrorexception]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.models.computervisionerrorexception?view=azure-python
[ref_httpfailure]: https://docs.microsoft.com/python/api/msrest/msrest.exceptions.httpoperationerror?view=azure-python


[computervision_resource]: https://azure.microsoft.com/en-us/try/cognitive-services/?

[computervision_docs]: /cognitive-services/computer-vision/home


[ref_computervision_model_visualfeatures]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.models.visualfeaturetypes?view=azure-python

[ref_computervision_model_textoperationstatuscodes]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.models.textoperationstatuscodes?view=azure-python

[computervision_request_units]:https://www.azure.cn/pricing/details/cognitive-services

[recognize-text]:https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/vision/computer_vision_samples.py

