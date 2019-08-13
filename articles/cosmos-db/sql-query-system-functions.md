---
title: 系统函数
description: 了解 Azure Cosmos DB 中的 SQL 系统函数。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 05/31/2019
ms.date: 07/29/2019
ms.author: v-yeche
ms.openlocfilehash: 066f3ae0f6046ed425188e486bde395396bacbaa
ms.sourcegitcommit: 5a4a826eea3914911fd93592e0f835efc9173133
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/31/2019
ms.locfileid: "68672266"
---
# <a name="system-functions"></a>系统函数

 Cosmos DB 提供多个内置 SQL 函数。 下面列出了内置函数的类别。  

|函数|说明|  
|--------------|-----------------|  
|[数学函数](#mathematical-functions)|每个数学函数均执行一个计算，通常基于作为参数提供的输出值，并返回数值。|  
|[类型检查函数](#type-checking-functions)|类型检查函数允许检查 SQL 查询内表达式的类型。|  
|[字符串函数](#string-functions)|字符串函数对字符串输入值执行运算，并返回字符串、数值或布尔值。|  
|[数组函数](#array-functions)|该数组函数对数组输入值执行操作，并返回数值、布尔值或数组值。|
|[日期和时间函数](#date-time-functions)|使用日期和时间函数可以获取采用以下两种格式的当前 UTC 日期和时间：一个时间戳，其值为以毫秒为单位的 Unix 纪元；一个符合 ISO 8601 格式的字符串。|
|[空间函数](#spatial-functions)|该空间函数对控件对象输入值执行操作，并返回数值或布尔值。|  

下面列出了每个类别中的函数：

| 函数组 | 操作 |
|---------|----------|
| 数学函数 | ABS、CEILING、EXP、FLOOR、LOG、LOG10、POWER、ROUND、SIGN、SQRT、SQUARE、TRUNC、ACOS、ASIN、ATAN、ATN2、COS、COT、DEGREES、PI、RADIANS、SIN 和 TAN |
| 类型检查函数 | IS_ARRAY、IS_BOOL、IS_NULL、IS_NUMBER、IS_OBJECT、IS_STRING、IS_DEFINED 和 IS_PRIMITIVE |
| 字符串函数 | CONCAT、CONTAINS、ENDSWITH、INDEX_OF、LEFT、LENGTH、LOWER、LTRIM、REPLACE、REPLICATE、REVERSE、RIGHT、RTRIM、STARTSWITH、SUBSTRING 和 UPPER |
| 数组函数 | ARRAY_CONCAT、ARRAY_CONTAINS、ARRAY_LENGTH 和 ARRAY_SLICE |
| 日期和时间函数 | GETCURRENTDATETIME、GETCURRENTTIMESTAMP  |
| 空间函数 | ST_DISTANCE、ST_WITHIN、ST_INTERSECTS、ST_ISVALID 和 ST_ISVALIDDETAILED |

如果当前正在使用的用户定义的函数 (UDF) 有内置函数可用，则相应的内置函数会更快更有效地运行。

Cosmos DB 函数与 ANSI SQL 函数之间的主要差别在于，Cosmos DB 函数能够很好地处理无架构数据和混合架构数据。 例如，如果某个属性缺失或包含类似于 `unknown` 的非数字值，则会跳过该项，而不是返回错误。

<a name="mathematical-functions"></a>
## <a name="mathematical-functions"></a>数学函数  

每个数学函数均执行一个计算，基于作为参数提供的输出值，并返回数值。

可以运行以下示例所示的查询：

```sql
    SELECT VALUE ABS(-4)
```

结果为：

```json
    [4]
```

以下是支持的内置数学函数表。

||||  
|-|-|-|  
|[ABS](#bk_abs)|[ACOS](#bk_acos)|[ASIN](#bk_asin)|  
|[ATAN](#bk_atan)|[ATN2](#bk_atn2)|[CEILING](#bk_ceiling)|  
|[COS](#bk_cos)|[COT](#bk_cot)|[DEGREES](#bk_degrees)|  
|[EXP](#bk_exp)|[FLOOR](#bk_floor)|[LOG](#bk_log)|  
|[LOG10](#bk_log10)|[PI](#bk_pi)|[POWER](#bk_power)|  
|[RADIANS](#bk_radians)|[ROUND](#bk_round)|[SIN](#bk_sin)|  
|[SQRT](#bk_sqrt)|[SQUARE](#bk_square)|[SIGN](#bk_sign)|  
|[TAN](#bk_tan)|[TRUNC](#bk_trunc)||  

<a name="bk_abs"></a>
#### <a name="abs"></a>ABS  
返回指定数值表达式的绝对（正）值。  

**语法**

```  
ABS (<numeric_expression>)  
```  

**参数**

- `numeric_expression`  

    是一个数值表达式。  

返回类型 

  返回数值表达式。  

**示例**

  以下示例显示了对三个不同数字使用 ABS 函数的结果。  

```  
SELECT ABS(-1) AS abs1, ABS(0) AS abs2, ABS(1) AS abs3 
```  

 下面是结果集。  

```  
[{abs1: 1, abs2: 0, abs3: 1}]  
```  

<a name="bk_acos"></a>
#### <a name="acos"></a>ACOS  
 返回角度（弧度），其余弦是指定的数值表达式；也被称为反余弦。  

**语法**

```  
ACOS(<numeric_expression>)  
```  

**参数**

- `numeric_expression`  

   是一个数值表达式。  

返回类型 

  返回数值表达式。  

**示例**

  以下示例返回 -1 的 ACOS。 

```  
SELECT ACOS(-1) AS acos 
```  

 下面是结果集。  

```  
[{"acos": 3.1415926535897931}]  
```  

<a name="bk_asin"></a>
#### <a name="asin"></a>ASIN  
 返回角度（弧度），其正弦是指定的数值表达式。 也被称为反正弦。  

**语法**

```  
ASIN(<numeric_expression>)  
```  

**参数**

- `numeric_expression`  

   是一个数值表达式。  

返回类型 

  返回数值表达式。  

**示例**

  以下示例返回 -1 的 ASIN。 

```  
SELECT ASIN(-1) AS asin  
```  

 下面是结果集。  

```  
[{"asin": -1.5707963267948966}]  
```  

<a name="bk_atan"></a>
#### <a name="atan"></a>ATAN  
 返回角度（弧度），其正切是指定的数值表达式。 这也被称为反正切。  

**语法**

```  
ATAN(<numeric_expression>)  
```  

**参数**

- `numeric_expression`  

   是一个数值表达式。  

返回类型 

  返回数值表达式。  

**示例**

  以下示例返回指定值的 ATAN。  

```  
SELECT ATAN(-45.01) AS atan  
```  

 下面是结果集。  

```  
[{"atan": -1.5485826962062663}]  
```  

<a name="bk_atn2"></a>
#### <a name="atn2"></a>ATN2  
 返回 y/x 的反正切的主体值，以弧度表示。  

**语法**

```  
ATN2(<numeric_expression>, <numeric_expression>)  
```  

**参数**

- `numeric_expression`  

   是一个数值表达式。  

返回类型 

  返回数值表达式。  

**示例**

  以下示例为指定的 x 和 y 组件计算 ATN2。  

```  
SELECT ATN2(35.175643, 129.44) AS atn2  
```  

 下面是结果集。  

```  
[{"atn2": 1.3054517947300646}]  
```  

<a name="bk_ceiling"></a>
#### <a name="ceiling"></a>CEILING  
 返回大于或等于指定数值表达式的最小整数值。  

**语法**

```  
CEILING (<numeric_expression>)  
```  

**参数**

- `numeric_expression`  

   是一个数值表达式。  

返回类型 

  返回数值表达式。  

**示例**

  以下示例显示了如何对正值、负值和零值使用 CEILING 函数。  

```  
SELECT CEILING(123.45) AS c1, CEILING(-123.45) AS c2, CEILING(0.0) AS c3  
```  

 下面是结果集。  

```  
[{c1: 124, c2: -123, c3: 0}]  
```  

<a name="bk_cos"></a>
#### <a name="cos"></a>COS  
 返回指定表达式中指定角度的三角余弦（弧度）。  

**语法**

```  
COS(<numeric_expression>)  
```  

**参数**

- `numeric_expression`  

   是一个数值表达式。  

返回类型 

  返回数值表达式。  

**示例**

  以下示例计算指定角度的 COS。  

```  
SELECT COS(14.78) AS cos  
```  

 下面是结果集。  

```  
[{"cos": -0.59946542619465426}]  
```  

<a name="bk_cot"></a>
#### <a name="cot"></a>COT  
 返回指定数值表达式中指定角度的三角余切。  

**语法**

```  
COT(<numeric_expression>)  
```  

**参数**

- `numeric_expression`  

   是一个数值表达式。  

返回类型 

  返回数值表达式。  

**示例**

  以下示例计算指定角度的 COT。  

```  
SELECT COT(124.1332) AS cot  
```  

 下面是结果集。  

```  
[{"cot": -0.040311998371148884}]  
```  

<a name="bk_degrees"></a>
#### <a name="degrees"></a>DEGREES  
 返回指定角度（弧度）的相应角度（度）。  

**语法**

```  
DEGREES (<numeric_expression>)  
```  

**参数**

- `numeric_expression`  

   是一个数值表达式。  

返回类型 

  返回数值表达式。  

**示例**

  以下示例返回 PI/2 弧度表示的角度的度数。  

```  
SELECT DEGREES(PI()/2) AS degrees  
```  

 下面是结果集。  

```  
[{"degrees": 90}]  
```  

<a name="bk_floor"></a>
#### <a name="floor"></a>FLOOR  
 返回小于或等于指定数值表达式的最大整数。  

**语法**

```  
FLOOR (<numeric_expression>)  
```  

**参数**

- `numeric_expression`  

   是一个数值表达式。  

返回类型 

  返回数值表达式。  

**示例**

  以下示例显示了如何对正值、负值和零值使用 FLOOR 函数。  

```  
SELECT FLOOR(123.45) AS fl1, FLOOR(-123.45) AS fl2, FLOOR(0.0) AS fl3  
```  

 下面是结果集。  

```  
[{fl1: 123, fl2: -124, fl3: 0}]  
```  

<a name="bk_exp"></a>
#### <a name="exp"></a>EXP  
 返回指定数值表达式的指数值。  

**语法**

```  
EXP (<numeric_expression>)  
```  

**参数**

- `numeric_expression`  

   是一个数值表达式。  

返回类型 

  返回一个数值表达式。  

  **备注**  

  常量 **e** (2.718281…) 是自然对数的底。  

  某个数字的指数是对常量 **e** 执行该次数的乘幂计算。 例如 EXP(1.0) = e^1.0 = 2.71828182845905，EXP(10) = e^10 = 22026.4657948067。 

  某个数的自然对数的指数就是该数本身：EXP (LOG (n)) = n。 并且某个数的指数的自然对数也是该数本身：LOG (EXP (n)) = n。  

**示例**

  以下示例声明一个变量并返回指定变量 (10) 的指数值。  

```  
SELECT EXP(10) AS exp  
```  

 下面是结果集。  

```  
[{exp: 22026.465794806718}]  
```  

 以下示例返回 20 的自然对数的指数值和 20 的指数的自然对数。 因为这些函数是另一个的反函数，因此，在两个示例中，返回值在进行浮点算术舍入后都是 20。 

```  
SELECT EXP(LOG(20)) AS exp1, LOG(EXP(20)) AS exp2  
```  

 下面是结果集。  

```  
[{exp1: 19.999999999999996, exp2: 20}]  
```  

<a name="bk_log"></a>
#### <a name="log"></a>LOG  
 返回指定数值表达式的自然对数。  

**语法**

```  
LOG (<numeric_expression> [, <base>])  
```  

**参数**

- `numeric_expression`  

   为数值表达式。  

- `base`  

   可选的数值参数，用于设置对数的底。  

**返回类型**

  返回一个数值表达式。  

  **备注**  

  默认情况下，LOG() 返回自然对数。 可以通过使用可选的 base 参数将对数的底更改为其他值。  

  自然对数是以 **e** 为底的对数，其中，**e** 是一个无理常量，约等于 2.718281828。 

  某个数的指数的自然对数也是该数本身：LOG( EXP( n ) ) = n。 并且某个数的自然对数的指数就是该数本身：EXP( LOG( n ) ) = n。  

**示例**

  以下示例声明一个变量并返回指定变量 (10) 的对数值。  

```  
SELECT LOG(10) AS log  
```  

 下面是结果集。  

```  
[{log: 2.3025850929940459}]  
```  

 以下示例计算某个数字的指数的 LOG。  

```  
SELECT EXP(LOG(10)) AS expLog  
```  

 下面是结果集。  

```  
[{expLog: 10.000000000000002}]  
```  

<a name="bk_log10"></a>
#### <a name="log10"></a>LOG10  
 返回指定数值表达式以 10 为底的对数。  

**语法**

```  
LOG10 (<numeric_expression>)  
```  

**参数**

- `numeric_expression`  

   是一个数值表达式。  

返回类型 

  返回一个数值表达式。  

  **备注**  

  LOG10 和 POWER 函数互为反函数。 例如，10 ^ LOG10(n) = n。  

**示例**

  以下示例声明一个变量并返回指定变量 (100) 的 LOG10 值。  

```  
SELECT LOG10(100) AS log10 
```  

 下面是结果集。  

```  
[{log10: 2}]  
```  

<a name="bk_pi"></a>
#### <a name="pi"></a>PI  
 返回 PI 的常数值。  

**语法**

```  
PI ()  
```  

**参数**

- `numeric_expression`  

   是一个数值表达式。  

返回类型 

  返回数值表达式。  

**示例**

  以下示例返回 PI 的值。  

```  
SELECT PI() AS pi 
```  

 下面是结果集。  

```  
[{"pi": 3.1415926535897931}]  
```  

<a name="bk_power"></a>
#### <a name="power"></a>POWER  
 返回指定表达式的指定幂的值。  

**语法**

```  
POWER (<numeric_expression>, <y>)  
```  

**参数**

- `numeric_expression`  

   是一个数值表达式。  

- `y`  

   是要将 `numeric_expression` 提升到的幂次。  

**返回类型**

  返回数值表达式。  

**示例**

  以下数字演示了如何求某个数字的 3 次幂（数字的立方）。  

```  
SELECT POWER(2, 3) AS pow1, POWER(2.5, 3) AS pow2  
```  

 下面是结果集。  

```  
[{pow1: 8, pow2: 15.625}]  
```  

<a name="bk_radians"></a>
#### <a name="radians"></a>RADIANS  
 返回输入的数值表达式（度）的弧度。  

**语法**

```  
RADIANS (<numeric_expression>)  
```  

**参数**

- `numeric_expression`  

   是一个数值表达式。  

返回类型 

  返回数值表达式。  

**示例**

  以下示例采用几个角度作为输入并返回其对应的弧度值。  

```  
SELECT RADIANS(-45.01) AS r1, RADIANS(-181.01) AS r2, RADIANS(0) AS r3, RADIANS(0.1472738) AS r4, RADIANS(197.1099392) AS r5  
```  

  下面是结果集。  

```  
[{  
       "r1": -0.7855726963226477,  
       "r2": -3.1592204790349356,  
       "r3": 0,  
       "r4": 0.0025704127119236249,  
       "r5": 3.4402174274458375  
   }]  
```  

<a name="bk_round"></a>
#### <a name="round"></a>ROUND  
 返回一个数值，四舍五入到最接近的整数值。  

**语法**

```  
ROUND(<numeric_expression>)  
```  

**参数**

- `numeric_expression`  

   是一个数值表达式。  

返回类型 

  返回一个数值表达式。  

  **备注**

  执行的四舍五入运算遵循远离零的中点四舍五入。 如果输入是正好介于两个整数之间的数值表达式，则结果将是离零最近的整数值。  

  |<numeric_expression>|已四舍五入|
  |-|-|
  |-6.5000|-7|
  |-0.5|-1|
  |0.5|1|
  |6.5000|7||

**示例**

  以下示例将以下正数和负数舍入到最接近的整数。  

```  
SELECT ROUND(2.4) AS r1, ROUND(2.6) AS r2, ROUND(2.5) AS r3, ROUND(-2.4) AS r4, ROUND(-2.6) AS r5  
```  

  下面是结果集。  

```  
[{r1: 2, r2: 3, r3: 3, r4: -2, r5: -3}]  
```  

<a name="bk_sign"></a>
#### <a name="sign"></a>SIGN  
 返回指定数值表达式的正数 (+1)、零 (0) 或负数 (-1)。  

**语法**

```  
SIGN(<numeric_expression>)  
```  

**参数**

- `numeric_expression`  

   是一个数值表达式。  

返回类型 

  返回数值表达式。  

**示例**

  以下示例返回数字 -2 到 2 的 SIGN 值。 

```  
SELECT SIGN(-2) AS s1, SIGN(-1) AS s2, SIGN(0) AS s3, SIGN(1) AS s4, SIGN(2) AS s5  
```  

 下面是结果集。  

```  
[{s1: -1, s2: -1, s3: 0, s4: 1, s5: 1}]  
```  

<a name="bk_sin"></a>
#### <a name="sin"></a>SIN  
 返回指定表达式中指定角度的三角正弦（弧度）。  

**语法**

```  
SIN(<numeric_expression>)  
```  

**参数**

- `numeric_expression`  

   是一个数值表达式。  

返回类型 

  返回数值表达式。  

**示例**

  以下示例计算指定角度的 SIN。  

```  
SELECT SIN(45.175643) AS sin  
```  

 下面是结果集。  

```  
[{"sin": 0.929607286611012}]  
```  

<a name="bk_sqrt"></a>
#### <a name="sqrt"></a>SQRT  
 返回指定数值的平方根。  

**语法**

```  
SQRT(<numeric_expression>)  
```  

**参数**

- `numeric_expression`  

   是一个数值表达式。  

返回类型 

  返回数值表达式。  

**示例**

  以下示例返回数字 1-3 的平方根。 

```  
SELECT SQRT(1) AS s1, SQRT(2.0) AS s2, SQRT(3) AS s3  
```  

 下面是结果集。  

```  
[{s1: 1, s2: 1.4142135623730952, s3: 1.7320508075688772}]  
```  

<a name="bk_square"></a>
#### <a name="square"></a>SQUARE  
 返回指定数字值的平方。  

**语法**

```  
SQUARE(<numeric_expression>)  
```  

**参数**

- `numeric_expression`  

   是一个数值表达式。  

返回类型 

  返回数值表达式。  

**示例**

  以下示例返回数字 1-3 的平方。 

```  
SELECT SQUARE(1) AS s1, SQUARE(2.0) AS s2, SQUARE(3) AS s3  
```  

 下面是结果集。  

```  
[{s1: 1, s2: 4, s3: 9}]  
```  

<a name="bk_tan"></a>
#### <a name="tan"></a>TAN  
 返回在指定表达式中以弧度表示的指定角度的正切。  

**语法**

```  
TAN (<numeric_expression>)  
```  

**参数**

- `numeric_expression`  

   是一个数值表达式。  

返回类型 

  返回数值表达式。  

**示例**

  以下示例计算 PI()/2 的正切。 

```  
SELECT TAN(PI()/2) AS tan 
```  

 下面是结果集。  

```  
[{"tan": 16331239353195370 }]  
```  

<a name="bk_trunc"></a>
#### <a name="trunc"></a>TRUNC  
 返回一个数值，截断到最接近的整数值。  

**语法**

```  
TRUNC(<numeric_expression>)  
```  

**参数**

- `numeric_expression`  

   是一个数值表达式。  

返回类型 

  返回数值表达式。  

**示例**

  以下示例将以下正数和负数截断到最接近的整数值。  

```  
SELECT TRUNC(2.4) AS t1, TRUNC(2.6) AS t2, TRUNC(2.5) AS t3, TRUNC(-2.4) AS t4, TRUNC(-2.6) AS t5  
```  

 下面是结果集。  

```  
[{t1: 2, t2: 2, t3: 2, t4: -2, t5: -2}]  
```

<a name="type-checking-functions"></a>
## <a name="type-checking-functions"></a>类型检查函数

使用类型检查函数可以检查 SQL 查询中表达式的类型。 当项中的属性可变或未知时，可以使用类型检查函数即时确定这些属性的类型。 下表列出了支持的内置类型检查函数：

以下函数支持针对输入值执行类型检查，并且每个函数将返回一个布尔值。  

||||  
|-|-|-|  
|[IS_ARRAY](#bk_is_array)|[IS_BOOL](#bk_is_bool)|[IS_DEFINED](#bk_is_defined)|  
|[IS_NULL](#bk_is_null)|[IS_NUMBER](#bk_is_number)|[IS_OBJECT](#bk_is_object)|  
|[IS_PRIMITIVE](#bk_is_primitive)|[IS_STRING](#bk_is_string)||  

<a name="bk_is_array"></a>
#### <a name="is_array"></a>IS_ARRAY  
 返回一个布尔值，指示指定表达式类型是否为数组。  

**语法**

```  
IS_ARRAY(<expression>)  
```  

**参数**

- `expression`  

   是任何有效的表达式。  

**返回类型**

  返回一个布尔表达式。  

**示例**

  以下示例使用 IS_ARRAY 函数检查了 JSON 布尔值、数字、字符串、null、对象、数组和未定义类型的对象。  

```  
SELECT   
 IS_ARRAY(true) AS isArray1,   
 IS_ARRAY(1) AS isArray2,  
 IS_ARRAY("value") AS isArray3,  
 IS_ARRAY(null) AS isArray4,  
 IS_ARRAY({prop: "value"}) AS isArray5,   
 IS_ARRAY([1, 2, 3]) AS isArray6,  
 IS_ARRAY({prop: "value"}.prop2) AS isArray7  
```  

 下面是结果集。  

```  
[{"isArray1":false,"isArray2":false,"isArray3":false,"isArray4":false,"isArray5":false,"isArray6":true,"isArray7":false}]
```  

<a name="bk_is_bool"></a>
#### <a name="is_bool"></a>IS_BOOL  
 返回一个布尔值，指示指定表达式的类型是否为布尔表达式。  

**语法**

```  
IS_BOOL(<expression>)  
```  

**参数**

- `expression`  

   是任何有效的表达式。  

**返回类型**

  返回一个布尔表达式。  

**示例**

  以下示例使用 IS_BOOL 函数检查了 JSON 布尔值、数字、字符串、null、对象、数组和未定义类型的对象。  

```  
SELECT   
    IS_BOOL(true) AS isBool1,   
    IS_BOOL(1) AS isBool2,  
    IS_BOOL("value") AS isBool3,   
    IS_BOOL(null) AS isBool4,  
    IS_BOOL({prop: "value"}) AS isBool5,   
    IS_BOOL([1, 2, 3]) AS isBool6,  
    IS_BOOL({prop: "value"}.prop2) AS isBool7  
```  

 下面是结果集。  

```  
[{"isBool1":true,"isBool2":false,"isBool3":false,"isBool4":false,"isBool5":false,"isBool6":false,"isBool7":false}]
```  

<a name="bk_is_defined"></a>
#### <a name="is_defined"></a>IS_DEFINED  
 返回一个布尔，它指示属性是否已经分配了值。  

**语法**

```  
IS_DEFINED(<expression>)  
```  

**参数**

- `expression`  

   是任何有效的表达式。  

**返回类型**

  返回一个布尔表达式。  

**示例**

  以下示例检查指定的 JSON 文档中是否存在某个属性。 第一个示例返回 true，因为“a” 存在；第二个示例返回 false，因为“b”不存在。  

```  
SELECT IS_DEFINED({ "a" : 5 }.a) AS isDefined1, IS_DEFINED({ "a" : 5 }.b) AS isDefined2 
```  

 下面是结果集。  

```  
[{"isDefined1":true,"isDefined2":false}]  
```  

<a name="bk_is_null"></a>
#### <a name="is_null"></a>IS_NULL  
 返回一个布尔值，指示指定表达式的类型是否为 null。  

**语法**

```  
IS_NULL(<expression>)  
```  

**参数**

- `expression`  

   是任何有效的表达式。  

**返回类型**

  返回一个布尔表达式。  

**示例**

  以下示例使用 IS_NULL 函数检查了 JSON 布尔值、数字、字符串、null、对象、数组和未定义类型的对象。  

```  
SELECT   
    IS_NULL(true) AS isNull1,   
    IS_NULL(1) AS isNull2,  
    IS_NULL("value") AS isNull3,   
    IS_NULL(null) AS isNull4,  
    IS_NULL({prop: "value"}) AS isNull5,   
    IS_NULL([1, 2, 3]) AS isNull6,  
    IS_NULL({prop: "value"}.prop2) AS isNull7  
```  

 下面是结果集。  

```  
[{"isNull1":false,"isNull2":false,"isNull3":false,"isNull4":true,"isNull5":false,"isNull6":false,"isNull7":false}]
```  

<a name="bk_is_number"></a>
#### <a name="is_number"></a>IS_NUMBER  
 返回一个布尔值，指示指定表达式的类型是否为数字。  

**语法**

```  
IS_NUMBER(<expression>)  
```  

**参数**

- `expression`  

   是任何有效的表达式。  

**返回类型**

  返回一个布尔表达式。  

**示例**

  以下示例使用 IS_NULL 函数检查了 JSON 布尔值、数字、字符串、null、对象、数组和未定义类型的对象。  

```  
SELECT   
    IS_NUMBER(true) AS isNum1,   
    IS_NUMBER(1) AS isNum2,  
    IS_NUMBER("value") AS isNum3,   
    IS_NUMBER(null) AS isNum4,  
    IS_NUMBER({prop: "value"}) AS isNum5,   
    IS_NUMBER([1, 2, 3]) AS isNum6,  
    IS_NUMBER({prop: "value"}.prop2) AS isNum7  
```  

 下面是结果集。  

```  
[{"isNum1":false,"isNum2":true,"isNum3":false,"isNum4":false,"isNum5":false,"isNum6":false,"isNum7":false}]  
```  

<a name="bk_is_object"></a>
#### <a name="is_object"></a>IS_OBJECT  
 返回一个布尔值，指示指定表达式的类型是否为 JSON 对象。  

**语法**

```  
IS_OBJECT(<expression>)  
```  

**参数**

- `expression`  

   是任何有效的表达式。  

**返回类型**

  返回一个布尔表达式。  

**示例**

  以下示例使用 IS_OBJECT 函数检查了 JSON 布尔值、数字、字符串、null、对象、数组和未定义类型的对象。  

```  
SELECT   
    IS_OBJECT(true) AS isObj1,   
    IS_OBJECT(1) AS isObj2,  
    IS_OBJECT("value") AS isObj3,   
    IS_OBJECT(null) AS isObj4,  
    IS_OBJECT({prop: "value"}) AS isObj5,   
    IS_OBJECT([1, 2, 3]) AS isObj6,  
    IS_OBJECT({prop: "value"}.prop2) AS isObj7  
```  

 下面是结果集。  

```  
[{"isObj1":false,"isObj2":false,"isObj3":false,"isObj4":false,"isObj5":true,"isObj6":false,"isObj7":false}]
```  

<a name="bk_is_primitive"></a>
#### <a name="is_primitive"></a>IS_PRIMITIVE  
 返回一个布尔值，指示指定表达式的类型是否为一个（字符串、布尔、数值或 null）。  

**语法**

```  
IS_PRIMITIVE(<expression>)  
```  

**参数**

- `expression`  

   是任何有效的表达式。  

**返回类型**

  返回一个布尔表达式。  

**示例**

  以下示例使用 IS_PRIMITIVE 函数检查 JSON 布尔、数字、字符串、null、对象、数组和 undefined 类型的对象。  

```  
SELECT   
           IS_PRIMITIVE(true) AS isPrim1,   
           IS_PRIMITIVE(1) AS isPrim2,  
           IS_PRIMITIVE("value") AS isPrim3,   
           IS_PRIMITIVE(null) AS isPrim4,  
           IS_PRIMITIVE({prop: "value"}) AS isPrim5,   
           IS_PRIMITIVE([1, 2, 3]) AS isPrim6,  
           IS_PRIMITIVE({prop: "value"}.prop2) AS isPrim7  
```  

 下面是结果集。  

```  
[{"isPrim1": true, "isPrim2": true, "isPrim3": true, "isPrim4": true, "isPrim5": false, "isPrim6": false, "isPrim7": false}]  
```  

<a name="bk_is_string"></a>
#### <a name="is_string"></a>IS_STRING  
 返回一个布尔值，指示指定表达式的类型是否为字符串。  

**语法**

```  
IS_STRING(<expression>)  
```  

**参数**

- `expression`  

   是任何有效的表达式。  

**返回类型**

  返回一个布尔表达式。  

**示例**

  以下示例使用 IS_STRING 函数检查了 JSON 布尔值、数字、字符串、null、对象、数组和未定义类型的对象。  

```  
SELECT   
       IS_STRING(true) AS isStr1,   
       IS_STRING(1) AS isStr2,  
       IS_STRING("value") AS isStr3,   
       IS_STRING(null) AS isStr4,  
       IS_STRING({prop: "value"}) AS isStr5,   
       IS_STRING([1, 2, 3]) AS isStr6,  
       IS_STRING({prop: "value"}.prop2) AS isStr7  
```  

 下面是结果集。  

```  
[{"isStr1":false,"isStr2":false,"isStr3":true,"isStr4":false,"isStr5":false,"isStr6":false,"isStr7":false}] 
```  

<a name="string-functions"></a>
## <a name="string-functions"></a>字符串函数

以下标量函数对字符串输入值执行操作，并返回字符串、数字或布尔值：

||||  
|-|-|-|  
|[CONCAT](#bk_concat)|[CONTAINS](#bk_contains)|[ENDSWITH](#bk_endswith)|  
|[INDEX_OF](#bk_index_of)|[LEFT](#bk_left)|[LENGTH](#bk_length)|  
|[LOWER](#bk_lower)|[LTRIM](#bk_ltrim)|[REPLACE](#bk_replace)|  
|[REPLICATE](#bk_replicate)|[REVERSE](#bk_reverse)|[RIGHT](#bk_right)|  
|[RTRIM](#bk_rtrim)|[STARTSWITH](#bk_startswith)|[StringToArray](#bk_stringtoarray)|
|[StringToBoolean](#bk_stringtoboolean)|[StringToNull](#bk_stringtonull)|[StringToNumber](#bk_stringtonumber)|
|[StringToObject](#bk_stringtoobject)|[SUBSTRING](#bk_substring)|[ToString](#bk_tostring)|
|[TRIM](#bk_trim)|[UPPER](#bk_upper)||

<a name="bk_concat"></a>
#### <a name="concat"></a>CONCAT  
 返回一个字符串，该字符串是连接两个或多个字符串值的结果。  

**语法**

```  
CONCAT(<str_expr>, <str_expr> [, <str_expr>])  
```  

**参数**

- `str_expr`  

   是任何有效的字符串表达式。  

**返回类型**

  返回一个字符串表达式。  

**示例**

  以下示例返回将指定值串联后形成的字符串。  

```  
SELECT CONCAT("abc", "def") AS concat  
```  

 下面是结果集。  

```  
[{"concat": "abcdef"}  
```  

<a name="bk_contains"></a>
#### <a name="contains"></a>CONTAINS  
 返回一个布尔值，该值指示第一个字符串表达式是否包含第二个字符串表达式。  

**语法**

```  
CONTAINS(<str_expr>, <str_expr>)  
```  

**参数**

- `str_expr`  

   是任何有效的字符串表达式。  

**返回类型**

  返回一个布尔表达式。  

**示例**

  以下示例检查“abc”是否包含“ab”以及是否包含“d”。  

```  
SELECT CONTAINS("abc", "ab") AS c1, CONTAINS("abc", "d") AS c2 
```  

 下面是结果集。  

```  
[{"c1": true, "c2": false}]  
```  

<a name="bk_endswith"></a>
#### <a name="endswith"></a>ENDSWITH  
 返回一个布尔值，指示第一个字符串表达式是否以第二个字符串表达式结尾。  

**语法**

```  
ENDSWITH(<str_expr>, <str_expr>)  
```  

**参数**

- `str_expr`  

   是任何有效的字符串表达式。  

**返回类型**

  返回一个布尔表达式。  

**示例**

  以下示例的返回结果指示“abc”是否以“b”和“bc”结尾。  

```  
SELECT ENDSWITH("abc", "b") AS e1, ENDSWITH("abc", "bc") AS e2 
```  

 下面是结果集。  

```  
[{"e1": false, "e2": true}]  
```  

<a name="bk_index_of"></a>
#### <a name="index_of"></a>INDEX_OF  
 返回第一个指定的字符串表达式中第一次出现第二个字符串表达式的起始位置，如果未找到字符串，则返回 -1。  

**语法**

```  
INDEX_OF(<str_expr>, <str_expr>)  
```  

**参数**

- `str_expr`  

   是任何有效的字符串表达式。  

**返回类型**

  返回数值表达式。  

**示例**

  以下示例返回“abc”内的各个子字符串的索引。  

```  
SELECT INDEX_OF("abc", "ab") AS i1, INDEX_OF("abc", "b") AS i2, INDEX_OF("abc", "c") AS i3 
```  

 下面是结果集。  

```  
[{"i1": 0, "i2": 1, "i3": -1}]  
```  

<a name="bk_left"></a>
#### <a name="left"></a>LEFT  
 返回具有指定字符数的字符串的左侧部分。  

**语法**

```  
LEFT(<str_expr>, <num_expr>)  
```  

**参数**

- `str_expr`  

   是任何有效的字符串表达式。  

- `num_expr`  

   是任何有效的数值表达式。  

**返回类型**

  返回一个字符串表达式。  

**示例**

  以下示例根据不同的长度值返回“abc”的左侧部分。  

```  
SELECT LEFT("abc", 1) AS l1, LEFT("abc", 2) AS l2 
```  

 下面是结果集。  

```  
[{"l1": "a", "l2": "ab"}]  
```  

<a name="bk_length"></a>
#### <a name="length"></a>LENGTH  
 返回指定字符串表达式的字符数。  

**语法**

```  
LENGTH(<str_expr>)  
```  

**参数**

- `str_expr`  

   是任何有效的字符串表达式。  

**返回类型**

  返回一个字符串表达式。  

**示例**

  以下示例返回某个字符串的长度。  

```  
SELECT LENGTH("abc") AS len 
```  

 下面是结果集。  

```  
[{"len": 3}]  
```  

<a name="bk_lower"></a>
#### <a name="lower"></a>LOWER  
 返回在将大写字符数据转换为小写后的字符串表达式。  

**语法**

```  
LOWER(<str_expr>)  
```  

**参数**

- `str_expr`  

   是任何有效的字符串表达式。  

**返回类型**

  返回一个字符串表达式。  

**示例**

  以下示例演示了如何在查询中使用 LOWER。  

```  
SELECT LOWER("Abc") AS lower
```  

 下面是结果集。  

```  
[{"lower": "abc"}]  

```  

<a name="bk_ltrim"></a>
#### <a name="ltrim"></a>LTRIM  
 返回删除前导空格后的字符串表达式。  

**语法**

```  
LTRIM(<str_expr>)  
```  

**参数**

- `str_expr`  

   是任何有效的字符串表达式。  

**返回类型**

  返回一个字符串表达式。  

**示例**

  以下示例演示了如何在查询中使用 LTRIM。  

```  
SELECT LTRIM("  abc") AS l1, LTRIM("abc") AS l2, LTRIM("abc   ") AS l3 
```  

 下面是结果集。  

```  
[{"l1": "abc", "l2": "abc", "l3": "abc   "}]  
```  

<a name="bk_replace"></a>
#### <a name="replace"></a>REPLACE  
 将出现的所有指定字符串值替换为另一个字符串值。  

**语法**

```  
REPLACE(<str_expr>, <str_expr>, <str_expr>)  
```  

**参数**

- `str_expr`  

   是任何有效的字符串表达式。  

**返回类型**

  返回一个字符串表达式。  

**示例**

  以下示例演示了如何在查询中使用 REPLACE。  

```  
SELECT REPLACE("This is a Test", "Test", "desk") AS replace 
```  

 下面是结果集。  

```  
[{"replace": "This is a desk"}]  
```  

<a name="bk_replicate"></a>
#### <a name="replicate"></a>REPLICATE  
 将一个字符串值重复指定的次数。  

**语法**

```  
REPLICATE(<str_expr>, <num_expr>)  
```  

**参数**

- `str_expr`  

   是任何有效的字符串表达式。  

- `num_expr`  

   是任何有效的数值表达式。 如果 num_expr 为负或非有限，则结果未定义。

  > [!NOTE]
  > 结果的最大长度为 10,000 个字符，即 (length(str_expr)  *  num_expr) <= 10,000。

**返回类型**

  返回一个字符串表达式。  

**示例**

  以下示例演示了如何在查询中使用 REPLICATE。  

```  
SELECT REPLICATE("a", 3) AS replicate  
```  

 下面是结果集。  

```  
[{"replicate": "aaa"}]  
```  

<a name="bk_reverse"></a>
#### <a name="reverse"></a>REVERSE  
 返回字符串值的逆序排序形式。  

**语法**

```  
REVERSE(<str_expr>)  
```  

**参数**

- `str_expr`  

   是任何有效的字符串表达式。  

**返回类型**

  返回一个字符串表达式。  

**示例**

  以下示例演示了如何在查询中使用 REVERSE。  

```  
SELECT REVERSE("Abc") AS reverse  
```  

 下面是结果集。  

```  
[{"reverse": "cbA"}]  
```  

<a name="bk_right"></a>
#### <a name="right"></a>RIGHT  
 返回具有指定字符数的字符串的右侧部分。  

**语法**

```  
RIGHT(<str_expr>, <num_expr>)  
```  

**参数**

- `str_expr`  

   是任何有效的字符串表达式。  

- `num_expr`  

   是任何有效的数值表达式。  

**返回类型**

  返回一个字符串表达式。  

**示例**

  以下示例根据不同的长度值返回“abc”的右侧部分。  

```  
SELECT RIGHT("abc", 1) AS r1, RIGHT("abc", 2) AS r2 
```  

 下面是结果集。  

```  
[{"r1": "c", "r2": "bc"}]  
```  

<a name="bk_rtrim"></a>
#### <a name="rtrim"></a>RTRIM  
 返回删除尾随空格后的字符串表达式。  

**语法**

```  
RTRIM(<str_expr>)  
```  

**参数**

- `str_expr`  

   是任何有效的字符串表达式。  

**返回类型**

  返回一个字符串表达式。  

**示例**

  以下示例演示了如何在查询中使用 RTRIM。  

```  
SELECT RTRIM("  abc") AS r1, RTRIM("abc") AS r2, RTRIM("abc   ") AS r3  
```  

 下面是结果集。  

```  
[{"r1": "   abc", "r2": "abc", "r3": "abc"}]  
```  

<a name="bk_startswith"></a>
#### <a name="startswith"></a>STARTSWITH  
 返回一个布尔值，指示第一个字符串表达式是否以第二个字符串表达式开头。  

**语法**

```  
STARTSWITH(<str_expr>, <str_expr>)  
```  

**参数**

- `str_expr`  

   是任何有效的字符串表达式。  

**返回类型**

  返回一个布尔表达式。  

**示例**

  以下示例检查字符串“abc”是否以“b”和“a”开头。  

```  
SELECT STARTSWITH("abc", "b") AS s1, STARTSWITH("abc", "a") AS s2  
```  

 下面是结果集。  

```  
[{"s1": false, "s2": true}]  
```  

  <a name="bk_stringtoarray"></a>
#### <a name="stringtoarray"></a>StringToArray  
 返回已转换为数组的表达式。 如果表达式无法转换，则返回未定义的表达式。  

**语法**

```  
StringToArray(<expr>)  
```  

**参数**

- `expr`  

   是否会将任何有效的标量表达式作为 JSON 数组表达式来计算？ 请注意，嵌套字符串值必须使用双引号编写，否则无效。 有关 JSON 格式的详细信息，请参阅 [json.org](https://json.org/)

**返回类型**

  返回一个数组表达式或未定义的表达式。  

**示例**

  以下示例演示 StringToArray 在不同类型中的行为方式。 

 下面是输入有效的示例。

```
SELECT 
    StringToArray('[]') AS a1, 
    StringToArray("[1,2,3]") AS a2,
    StringToArray("[\"str\",2,3]") AS a3,
    StringToArray('[["5","6","7"],["8"],["9"]]') AS a4,
    StringToArray('[1,2,3, "[4,5,6]",[7,8]]') AS a5
```

下面是结果集。

```
[{"a1": [], "a2": [1,2,3], "a3": ["str",2,3], "a4": [["5","6","7"],["8"],["9"]], "a5": [1,2,3,"[4,5,6]",[7,8]]}]
```

下面是输入无效的示例。 

 在数组中使用单引号不是有效的 JSON。
即使它们在查询中有效，系统也不会将其解析为有效数组。 必须将数组字符串中的字符串转义为 "[\\"\\"]"，否则其引号必须为单个 '[""]'。

```
SELECT
    StringToArray("['5','6','7']")
```

下面是结果集。

```
[{}]
```

下面是输入无效的示例。

 传递的表达式将会解析为 JSON 数组；下面的示例不会计算为类型数组，因此返回未定义。

```
SELECT
    StringToArray("["),
    StringToArray("1"),
    StringToArray(NaN),
    StringToArray(false),
    StringToArray(undefined)
```

下面是结果集。

```
[{}]
```

<a name="bk_stringtoboolean"></a>
#### <a name="stringtoboolean"></a>StringToBoolean  
 返回已转换为布尔值的表达式。 如果表达式无法转换，则返回未定义的表达式。  

**语法**

```  
StringToBoolean(<expr>)  
```  

**参数**

- `expr`  

   是否会将任何有效的标量表达式作为布尔表达式来计算？  

**返回类型**

  返回一个布尔表达式或未定义的表达式。  

**示例**

  以下示例演示 StringToBoolean 在不同类型中的行为方式。 

 下面是输入有效的示例。

只能在 "true"/"false" 之前或之后使用空格。

```  
SELECT 
    StringToBoolean("true") AS b1, 
    StringToBoolean("    false") AS b2,
    StringToBoolean("false    ") AS b3
```  

 下面是结果集。  

```  
[{"b1": true, "b2": false, "b3": false}]
```  

下面是输入无效的示例。

 布尔值区分大小写，必须全用小写字符（即 "true" 和 "false"）来表示。

```  
SELECT 
    StringToBoolean("TRUE"),
    StringToBoolean("False")
```  

下面是结果集。  

```  
[{}]
``` 

传递的表达式将会解析为布尔表达式；以下输入不会计算为布尔类型，因此会返回未定义。

```  
SELECT 
    StringToBoolean("null"),
    StringToBoolean(undefined),
    StringToBoolean(NaN), 
    StringToBoolean(false), 
    StringToBoolean(true)
```  

下面是结果集。  

```  
[{}]
```  

<a name="bk_stringtonull"></a>
#### <a name="stringtonull"></a>StringToNull  
 返回已转换为 Null 的表达式。 如果表达式无法转换，则返回未定义的表达式。  

**语法**

```  
StringToNull(<expr>)  
```  

**参数**

- `expr`  

   是否会将任何有效的标量表达式作为 Null 表达式来计算？

**返回类型**

  返回一个 Null 表达式或未定义的表达式。  

**示例**

  以下示例演示 StringToNull 在不同类型中的行为方式。 

下面是输入有效的示例。

 只能在 "null" 之前或之后使用空格。

```  
SELECT 
    StringToNull("null") AS n1, 
    StringToNull("  null ") AS n2,
    IS_NULL(StringToNull("null   ")) AS n3
```  

 下面是结果集。  

```  
[{"n1": null, "n2": null, "n3": true}]
```  

下面是输入无效的示例。

Null 值区分大小写，必须全用小写字符（即 "null"）来表示。

```  
SELECT    
    StringToNull("NULL"),
    StringToNull("Null")
```  

 下面是结果集。  

```  
[{}]
```  

传递的表达式将会解析为 null 表达式；以下输入不会计算为 null 类型，因此会返回未定义。

```  
SELECT    
    StringToNull("true"), 
    StringToNull(false), 
    StringToNull(undefined),
    StringToNull(NaN) 
```  

 下面是结果集。  

```  
[{}]
```  

<a name="bk_stringtonumber"></a>
#### <a name="stringtonumber"></a>StringToNumber  
 返回已转换为数字值的表达式。 如果表达式无法转换，则返回未定义的表达式。  

**语法**

```  
StringToNumber(<expr>)  
```  

**参数**

- `expr`  

   是否会将任何有效的标量表达式作为 JSON 数字表达式来计算？ JSON 中的数字必须是整数或浮点数。 有关 JSON 格式的详细信息，请参阅 [json.org](https://json.org/)  

**返回类型**

  返回一个数字表达式或未定义的表达式。  

**示例**

  以下示例演示 StringToNumber 在不同类型中的行为方式。 

只能在 Number 之前或之后使用空格。

```  
SELECT 
    StringToNumber("1.000000") AS num1, 
    StringToNumber("3.14") AS num2,
    StringToNumber("   60   ") AS num3, 
    StringToNumber("-1.79769e+308") AS num4
```  

 下面是结果集。  

```  
{{"num1": 1, "num2": 3.14, "num3": 60, "num4": -1.79769e+308}}
```  

在 JSON 中，有效的 Number 必须是整数或浮点数。

```  
SELECT   
    StringToNumber("0xF")
```  

 下面是结果集。  

```  
{{}}
```  

传递的表达式将会解析为 Number 表达式；以下输入不会计算为 Number 类型，因此会返回未定义。 

```  
SELECT 
    StringToNumber("99     54"),   
    StringToNumber(undefined),
    StringToNumber("false"),
    StringToNumber(false),
    StringToNumber(" "),
    StringToNumber(NaN)
```  

 下面是结果集。  

```  
{{}}
```  

<a name="bk_stringtoobject"></a>
#### <a name="stringtoobject"></a>StringToObject  
 返回已转换为对象的表达式。 如果表达式无法转换，则返回未定义的表达式。  

**语法**

```  
StringToObject(<expr>)  
```  

**参数**

- `expr`  

   是否会将任何有效的标量表达式作为 JSON 对象表达式来计算？ 请注意，嵌套字符串值必须使用双引号编写，否则无效。 有关 JSON 格式的详细信息，请参阅 [json.org](https://json.org/)  

**返回类型**

  返回一个对象表达式或未定义的表达式。  

**示例**

  以下示例演示 StringToObject 在不同类型中的行为方式。 

 下面是输入有效的示例。

``` 
SELECT 
    StringToObject("{}") AS obj1, 
    StringToObject('{"A":[1,2,3]}') AS obj2,
    StringToObject('{"B":[{"b1":[5,6,7]},{"b2":8},{"b3":9}]}') AS obj3, 
    StringToObject("{\"C\":[{\"c1\":[5,6,7]},{\"c2\":8},{\"c3\":9}]}") AS obj4
``` 

下面是结果集。

```
[{"obj1": {}, 
  "obj2": {"A": [1,2,3]}, 
  "obj3": {"B":[{"b1":[5,6,7]},{"b2":8},{"b3":9}]},
  "obj4": {"C":[{"c1":[5,6,7]},{"c2":8},{"c3":9}]}}]
```

 下面是输入无效的示例。
即使它们在查询中有效，系统也不会将其解析为有效对象。 必须将对象字符串中的字符串转义为 "{\\"a\\":\\"str\\"}"，否则其引号必须为单个 '{"a": "str"}'。

属性名称的单引号不是有效的 JSON。

``` 
SELECT 
    StringToObject("{'a':[1,2,3]}")
```

下面是结果集。

```  
[{}]
```  

没有引号的属性名称不是有效的 JSON。

``` 
SELECT 
    StringToObject("{a:[1,2,3]}")
```

下面是结果集。

```  
[{}]
``` 

下面是输入无效的示例。

 传递的表达式将会解析为 JSON 对象；以下输入不会计算为对象类型，因此会返回未定义。

``` 
SELECT 
    StringToObject("}"),
    StringToObject("{"),
    StringToObject("1"),
    StringToObject(NaN), 
    StringToObject(false), 
    StringToObject(undefined)
``` 

 下面是结果集。

```
[{}]
```

<a name="bk_substring"></a>
#### <a name="substring"></a>SUBSTRING  
 返回字符串表达式的部分内容，该内容起于指定字符从零开始的位置，继续到指定长度或字符串结尾。  

**语法**

```  
SUBSTRING(<str_expr>, <num_expr>, <num_expr>)  
```  

**参数**

- `str_expr`  

   是任何有效的字符串表达式。  

- `num_expr`  

   是表示开始和结束字符的任何有效数字表达式。    

**返回类型**

  返回一个字符串表达式。  

**示例**

  以下示例返回“abc”中从位置 1 开始且长度为 1 个字符的子字符串。  

```  
SELECT SUBSTRING("abc", 1, 1) AS substring  
```  

 下面是结果集。  

```  
[{"substring": "b"}]  
```  
<a name="bk_tostring"></a>
#### <a name="tostring"></a>ToString  
 返回标量表达式的字符串表示形式。 

**语法**

```  
ToString(<expr>)
```  

**参数**

- `expr`  

   为任何有效的标量表达式。  

**返回类型**

  返回一个字符串表达式。  

**示例**

  以下示例演示 ToString 在不同类型中的行为方式。   

```  
SELECT 
    ToString(1.0000) AS str1, 
    ToString("Hello World") AS str2, 
    ToString(NaN) AS str3, 
    ToString(Infinity) AS str4,
    ToString(IS_STRING(ToString(undefined))) AS str5, 
    ToString(0.1234) AS str6, 
    ToString(false) AS str7, 
    ToString(undefined) AS str8
```  

 下面是结果集。  

```  
[{"str1": "1", "str2": "Hello World", "str3": "NaN", "str4": "Infinity", "str5": "false", "str6": "0.1234", "str7": "false"}]  
```  
 给定以下输入：
```  
{"Products":[{"ProductID":1,"Weight":4,"WeightUnits":"lb"},{"ProductID":2,"Weight":32,"WeightUnits":"kg"},{"ProductID":3,"Weight":400,"WeightUnits":"g"},{"ProductID":4,"Weight":8999,"WeightUnits":"mg"}]}
```    
 以下示例演示 ToString 如何与其他字符串函数（如 CONCAT）一起使用。   

```  
SELECT 
CONCAT(ToString(p.Weight), p.WeightUnits) 
FROM p in c.Products 
```  

下面是结果集。  

```  
[{"$1":"4lb" },
{"$1":"32kg"},
{"$1":"400g" },
{"$1":"8999mg" }]

```  
给定以下输入。
```
{"id":"08259","description":"Cereals ready-to-eat, KELLOGG, KELLOGG'S CRISPIX","nutrients":[{"id":"305","description":"Caffeine","units":"mg"},{"id":"306","description":"Cholesterol, HDL","nutritionValue":30,"units":"mg"},{"id":"307","description":"Sodium, NA","nutritionValue":612,"units":"mg"},{"id":"308","description":"Protein, ABP","nutritionValue":60,"units":"mg"},{"id":"309","description":"Zinc, ZN","nutritionValue":null,"units":"mg"}]}
```
以下示例演示 ToString 如何与其他字符串函数（如 REPLACE）一起使用。   
```
SELECT 
    n.id AS nutrientID,
    REPLACE(ToString(n.nutritionValue), "6", "9") AS nutritionVal
FROM food 
JOIN n IN food.nutrients
```
下面是结果集。  
 ```
[{"nutrientID":"305"},
{"nutrientID":"306","nutritionVal":"30"},
{"nutrientID":"307","nutritionVal":"912"},
{"nutrientID":"308","nutritionVal":"90"},
{"nutrientID":"309","nutritionVal":"null"}]
``` 

<a name="bk_trim"></a>
#### <a name="trim"></a>TRIM  
 返回删除前导空格和尾随空格后的字符串表达式。  

**语法**

```  
TRIM(<str_expr>)  
```  

**参数**

- `str_expr`  

   是任何有效的字符串表达式。  

**返回类型**

  返回一个字符串表达式。  

**示例**

  以下示例介绍了如何在查询中使用 TRIM。  

```  
SELECT TRIM("   abc") AS t1, TRIM("   abc   ") AS t2, TRIM("abc   ") AS t3, TRIM("abc") AS t4
```  

 下面是结果集。  

```  
[{"t1": "abc", "t2": "abc", "t3": "abc", "t4": "abc"}]  
``` 
<a name="bk_upper"></a>
#### <a name="upper"></a>UPPER  
 返回在将小写字符数据转换为大写后的字符串表达式。  

**语法**

```  
UPPER(<str_expr>)  
```  

**参数**

- `str_expr`  

   是任何有效的字符串表达式。  

**返回类型**

  返回一个字符串表达式。  

**示例**

  以下示例演示了如何在查询中使用 UPPER。  

```  
SELECT UPPER("Abc") AS upper  
```  

 下面是结果集。  

```  
[{"upper": "ABC"}]  
```

<a name="array-functions"></a>
## <a name="array-functions"></a>数组函数

以下标量函数对数组输入值执行操作，并返回数值、布尔值或数组值：

||||  
|-|-|-|  
|[ARRAY_CONCAT](#bk_array_concat)|[ARRAY_CONTAINS](#bk_array_contains)|[ARRAY_LENGTH](#bk_array_length)|  
|[ARRAY_SLICE](#bk_array_slice)|||  

<a name="bk_array_concat"></a>
#### <a name="array_concat"></a>ARRAY_CONCAT  
 返回一个数组，该数组是连接两个或更多数组值的结果。  

**语法**

```  
ARRAY_CONCAT (<arr_expr>, <arr_expr> [, <arr_expr>])  
```  

**参数**

- `arr_expr`  

   是任何有效的数组表达式。  

**返回类型**

  返回一个数组表达式。  

**示例**

  以下示例演示了如何连接两个数组。  

```  
SELECT ARRAY_CONCAT(["apples", "strawberries"], ["bananas"]) AS arrayConcat 
```  

 下面是结果集。  

```  
[{"arrayConcat": ["apples", "strawberries", "bananas"]}]  
```  

<a name="bk_array_contains"></a>
#### <a name="array_contains"></a>ARRAY_CONTAINS  
返回一个布尔，它指示数组是否包含指定的值。 可以通过在命令中使用布尔表达式来检查对象的部分匹配或完全匹配。 

**语法**

```  
ARRAY_CONTAINS (<arr_expr>, <expr> [, bool_expr])  
```  

**参数**

- `arr_expr`  

   是任何有效的数组表达式。  

- `expr`  

   是任何有效的表达式。  

- `bool_expr`  

   为任何布尔表达式。 如果将其设置为“true”，并且指定的搜索值是对象，则该命令将检查部分匹配（搜索对象是其中一个对象的子集）。 如果将其设置为“false”，则该命令将检查数组中所有对象的完全匹配。 如果未指定，默认值为“false”。 

**返回类型**

  返回一个布尔值。  

**示例**

  以下示例演示了如何使用 ARRAY_CONTAINS 检查数组中的成员身份。  

```  
SELECT   
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "apples") AS b1,  
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "mangoes") AS b2  
```  

 下面是结果集。  

```  
[{"b1": true, "b2": false}]  
```  

以下示例介绍了如何使用 ARRAY_CONTAINS 检查数组内是否存在 JSON 字符串的部分匹配字符串。  

```  
SELECT  
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "apples"}, true) AS b1, 
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "apples"}) AS b2,
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "mangoes"}, true) AS b3 
```  

 下面是结果集。  

```  
[{
  "b1": true,
  "b2": false,
  "b3": false
}] 
```  

<a name="bk_array_length"></a>
#### <a name="array_length"></a>ARRAY_LENGTH  
 返回指定数组表达式的元素数。  

**语法**

```  
ARRAY_LENGTH(<arr_expr>)  
```  

**参数**

- `arr_expr`  

   是任何有效的数组表达式。  

**返回类型**

  返回数值表达式。  

**示例**

  以下示例演示了如何使用 ARRAY_LENGTH 获取数组的长度。  

```  
SELECT ARRAY_LENGTH(["apples", "strawberries", "bananas"]) AS len  
```  

 下面是结果集。  

```  
[{"len": 3}]  
```  

<a name="bk_array_slice"></a>
#### <a name="array_slice"></a>ARRAY_SLICE  
 返回部分数组表达式。

**语法**

```  
ARRAY_SLICE (<arr_expr>, <num_expr> [, <num_expr>])  
```  

**参数**

- `arr_expr`  

   是任何有效的数组表达式。  

- `num_expr`  

   用于开始数组的从零开始的数字索引。 负值可用于指定相对于数组最后一个元素的起始索引，即 -1 引用数组中最后一个元素。  

- `num_expr`  

   结果数组中的最大元素数。    

**返回类型**

  返回一个数组表达式。  

**示例**

  以下示例展示了如何使用 ARRAY_SLICE 获取数组的不同切片。  

```  
SELECT
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1) AS s1,  
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 1) AS s2,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], -2, 1) AS s3,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], -2, 2) AS s4,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 0) AS s5,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 1000) AS s6,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, -100) AS s7      

```  

 下面是结果集。  

```  
[{  
           "s1": ["strawberries", "bananas"],   
           "s2": ["strawberries"],
           "s3": ["strawberries"],  
           "s4": ["strawberries", "bananas"], 
           "s5": [],
           "s6": ["strawberries", "bananas"],
           "s7": [] 
}]  
```  
<a name="date-time-functions"></a>
## <a name="date-and-time-function"></a>日期和时间函数

使用以下标量函数可以获取采用以下两种格式的当前 UTC 日期和时间：一个时间戳，其值为以毫秒为单位的 Unix 纪元；一个符合 ISO 8601 格式的字符串。 

|||
|-|-|
|[GETCURRENTDATETIME](#bk_get_current_date_time)|[GETCURRENTTIMESTAMP](#bk_get_current_timestamp)||

<a name="bk_get_current_date_time"></a>
#### <a name="getcurrentdatetime"></a>GETCURRENTDATETIME
 以 ISO 8601 字符串形式返回当前 UTC 日期和时间。

**语法**

```
GETCURRENTDATETIME ()
```

  **返回类型**

  以 ISO 8601 字符串值形式返回当前 UTC 日期和时间。 

  此值以 YYYY-MM-DDThh:mm:ss.sssZ 格式表示，其中：

  |||
  |-|-|
  |YYYY|四位数的年份|
  |MM|两位数的月份（01 = 1 月，依此类推。）|
  |DD|两位数的月份日期（01 到 31）|
  |T|时间元素开头的符号|
  |hh|两位数小时（00 到 23）|
  |MM|两位数分钟（00 到 59）|
  |ss|两位数秒（00 到 59）|
  |.sss|三位数的秒小数部分|
  |Z|UTC（协调世界时）指示符||

  有关 ISO 8601 格式的更多详细信息，请参阅 [ISO_8601](https://en.wikipedia.org/wiki/ISO_8601)

  **备注**

  GETCURRENTDATETIME 是非确定性的函数。 

  返回的结果采用 UTC（协调世界时）格式。

**示例**

  以下示例演示如何使用 GetCurrentDateTime 内置函数获取当前 UTC 日期时间。

```  
SELECT GETCURRENTDATETIME() AS currentUtcDateTime
```  

 下面是示例结果集。

```  
[{
  "currentUtcDateTime": "2019-05-03T20:36:17.784Z"
}]  
```  

<a name="bk_get_current_timestamp"></a>
#### <a name="getcurrenttimestamp"></a>GETCURRENTTIMESTAMP
 返回自 1970 年 1 月 1 日星期四 00:00:00 开始消逝的毫秒数。 

**语法**

```  
GETCURRENTTIMESTAMP ()  
```  

**返回类型**

  返回一个数字值，表示自 Unix 纪元开始消逝的秒数，即自 1970 年 1 月 1 日星期四 00:00:00 开始消逝的毫秒数。

  **备注**

  GETCURRENTTIMESTAMP 是非确定性的函数。

  返回的结果采用 UTC（协调世界时）格式。

**示例**

  以下示例演示如何使用 GetCurrentTimestamp 内置函数获取当前时间戳。

```  
SELECT GETCURRENTTIMESTAMP() AS currentUtcTimestamp
```  

 下面是示例结果集。

```  
[{
  "currentUtcTimestamp": 1556916469065
}]  
```

<a name="spatial-functions"></a>
## <a name="spatial-functions"></a>空间函数

Cosmos DB 支持以下用于查询地理空间的开放地理空间信息联盟 (OGC) 内置函数。 以下标量函数对标量对象输入值执行操作，并返回数值或布尔值。  

|||||
|-|-|-|-|
|[ST_DISTANCE](#bk_st_distance)|[ST_WITHIN](#bk_st_within)|[ST_INTERSECTS](#bk_st_intersects)|[ST_ISVALID](#bk_st_isvalid)|
|[ST_ISVALIDDETAILED](#bk_st_isvaliddetailed)||||

<a name="bk_st_distance"></a>
#### <a name="st_distance"></a>ST_DISTANCE  
 返回两个 GeoJSON 点、多边形或 LineString 表达式之间的距离。  

**语法**

```  
ST_DISTANCE (<spatial_expr>, <spatial_expr>)  
```  

**参数**

- `spatial_expr`  

   是任何有效的 GeoJSON 点、多边形或 LineString 对象表达式。  

**返回类型**

  返回包含距离的一个数字表达式。 这是根据默认参考系统以米为单位表示的。  

**示例**

  以下示例演示了如何使用 ST_DISTANCE 内置函数返回与指定位置的距离在 30 公里内的所有家族文档。 上获取。  

```  
SELECT f.id   
FROM Families f   
WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000  
```  

 下面是结果集。  

```  
[{  
  "id": "WakefieldFamily"  
}]  
```  

<a name="bk_st_within"></a>
#### <a name="st_within"></a>ST_WITHIN  
 返回一个布尔表达式，指示在第一个参数中指定的 GeoJSON 对象（点、多边形或 LineString）是否位于第二个参数中的 GeoJSON（点、多边形或 LineString）内。  

**语法**

```  
ST_WITHIN (<spatial_expr>, <spatial_expr>)  
```  

**参数**

- `spatial_expr`  

   是任何有效的 GeoJSON 点、多边形或 LineString 对象表达式。  

- `spatial_expr`  

   为任何有效的 GeoJSON 点、多边形或 LineString 对象表达式。  

**返回类型**

  返回一个布尔值。  

**示例**

  以下示例演示了如何使用 ST_WITHIN 查找某个多边形内的所有家族文档。  

```  
SELECT f.id   
FROM Families f   
WHERE ST_WITHIN(f.location, {  
    'type':'Polygon',   
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  

 下面是结果集。  

```  
[{ "id": "WakefieldFamily" }]  
```  

<a name="bk_st_intersects"></a>
#### <a name="st_intersects"></a>ST_INTERSECTS  
 返回一个布尔表达式，指示在第一个参数中指定的 GeoJSON 对象（点、多边形或 LineString）是否与第二个参数中的 GeoJSON（点、多边形或 LineString）相交。  

**语法**

```  
ST_INTERSECTS (<spatial_expr>, <spatial_expr>)  
```  

**参数**

- `spatial_expr`  

   是任何有效的 GeoJSON 点、多边形或 LineString 对象表达式。  

- `spatial_expr`  

   为任何有效的 GeoJSON 点、多边形或 LineString 对象表达式。  

**返回类型**

  返回一个布尔值。  

**示例**

  以下示例介绍了如何查找与给定多边形相交的所有区域。  

```  
SELECT a.id
FROM Areas a
WHERE ST_INTERSECTS(a.location, {  
    'type':'Polygon',
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  

 下面是结果集。  

```  
[{ "id": "IntersectingPolygon" }]  
```  

<a name="bk_st_isvalid"></a>
#### <a name="st_isvalid"></a>ST_ISVALID  
 返回一个布尔值，该值指示指定的 GeoJSON 点、多边形或 LineString 表达式是否有效。  

**语法**

```  
ST_ISVALID(<spatial_expr>)  
```  

 **参数**

- `spatial_expr`  

   是任何有效的 GeoJSON 点、多边形或 LineString 表达式。  

**返回类型**

  返回一个布尔表达式。  

**示例**

  以下示例演示了如何使用 ST_VALID 检查某个点是否有效。  

  例如，此点具有不在有效的值范围 [-90, 90] 中的纬度值，因此，该查询返回 false。  

  对于多边形，若要创建闭合的形状，GeoJSON 规范要求所提供的最后一个坐标对应该与第一个坐标对相同。 多边形内的点必须以逆时针顺序指定。 以顺时针顺序指定的多边形表示其中的区域倒转。  

```  
SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] }) AS b 
```  

 下面是结果集。  

```  
[{ "b": false }]  
```  

<a name="bk_st_isvaliddetailed"></a>
#### <a name="st_isvaliddetailed"></a>ST_ISVALIDDETAILED  
 如果指定的 GeoJSON 点、多边形或 LineString 表达式有效，则返回包含布尔值的 JSON 值；如果无效，则额外加上作为字符串值的原因。  

**语法**

```  
ST_ISVALIDDETAILED(<spatial_expr>)  
```  

**参数**

- `spatial_expr`  

   是任何有效的 GeoJSON 点或多边形表达式。  

**返回类型**

  如果指定的 GeoJSON 点或多边形表达式有效，则返回包含布尔值的 JSON 值；如果无效，则额外以字符串值提供原因。  

**示例**

  以下示例演示了如何使用 ST_ISVALIDDETAILED 检查有效性（带详细信息）。  

```  
SELECT ST_ISVALIDDETAILED({   
  "type": "Polygon",   
  "coordinates": [[ [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] ]]  
}) AS b 
```  

 下面是结果集。  

```  
[{  
  "b": {
    "valid": false,
    "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points."   
  }  
}]  
```  

## <a name="next-steps"></a>后续步骤

- [Azure Cosmos DB 简介](introduction.md)
- [UDF](sql-query-udfs.md)
- [聚合](sql-query-aggregates.md)

<!-- Update_Description: wording update, update link -->