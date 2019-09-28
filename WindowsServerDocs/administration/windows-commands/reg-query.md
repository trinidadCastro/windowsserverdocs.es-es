---
title: REG (consulta)
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0e6a0d7c-ed9b-4318-833d-33f265a81f39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d2f616fb33974df4327c7b2536b3143b75d116be
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371718"
---
# <a name="reg-query"></a>REG (consulta)



Devuelve una lista del siguiente nivel de subclaves y entradas que se encuentran en una subclave especificada del registro.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
reg query <KeyName> [{/v <ValueName> | /ve}] [/s] [/se <Separator>] [/f <Data>] [{/k | /d}] [/c] [/e] [/t <Type>] [/z]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|@no__t 0KeyName >|Especifica la ruta de acceso completa de la subclave. Para especificar equipos remotos, incluya el nombre del equipo (en el formato \\ @ no__t-1ComputerName @ no__t-2 como parte del nombre de *clave*. Si se omite \\ @ no__t-1ComputerName, la operación se realiza de forma predeterminada en el equipo local. *KeyName* debe incluir una clave raíz válida. Las claves raíz válidas para el equipo local son: HKLM, HKCU, HKCR, HKU y HKCC. Si se especifica un equipo remoto, las claves raíz válidas son: HKLM y HKU.|
|/v \<ValueName >|Especifica el nombre del valor del registro que se va a consultar. Si se omite, se devuelven todos los nombres de valor para *keyName* . *ValueName* para este parámetro es opcional si también se usa la opción **/f** .|
|/ve|Ejecuta una consulta para los nombres de valor que están vacíos.|
|/s|Especifica que se consulten todas las subclaves y los nombres de valor de forma recursiva.|
|/se \<Separator >|Especifica el separador de valor único que se va a buscar en el nombre de valor tipo REG_MULTI_SZ. Si no se especifica *separator* , se usa **\ 0** .|
|/f \<Data >|Especifica los datos o el patrón que se va a buscar. Use comillas dobles si una cadena contiene espacios. Si no se especifica, se usa **&#42;** un carácter comodín () como patrón de búsqueda.|
|/k|Especifica que se busque solo en los nombres de clave.|
|/d|Especifica que solo se busque en los datos.|
|/c|Especifica que la consulta distingue mayúsculas de minúsculas. De forma predeterminada, las consultas no distinguen mayúsculas de minúsculas.|
|/e|Especifica que solo se devuelvan las coincidencias exactas. De forma predeterminada, se devuelven todas las coincidencias.|
|/t \<Type >|Especifica los tipos de registro que se van a buscar. Los tipos válidos son: REG_SZ, REG_MULTI_SZ, REG_EXPAND_SZ, REG_DWORD, REG_BINARY, REG_NONE. Si no se especifica, se busca en todos los tipos.|
|/z|Especifica que se incluya el equivalente numérico para el tipo de registro en los resultados de la búsqueda.|
|/?|Muestra la ayuda de **reg Query** en el símbolo del sistema.|

## <a name="remarks-optional-section"></a>Notas @no__t la sección 0optional >

En la tabla siguiente se enumeran los valores devueltos para la operación **reg Query** .

|Valor|Descripción|
|-----|-----------|
|0|Correcto|
|1|Error|

## <a name="BKMK_examples"></a>Example

Para mostrar el valor de la versión del valor de nombre en la clave HKLM\Software\Microsoft\ResKit, escriba:
```
REG QUERY HKLM\Software\Microsoft\ResKit /v Version
```
Para mostrar todas las subclaves y valores de la clave HKLM\Software\Microsoft\ResKit\Nt\Setup en un equipo remoto llamado ABC, escriba:
```
REG QUERY \\ABC\HKLM\Software\Microsoft\ResKit\Nt\Setup /s
```
Para mostrar todas las subclaves y valores del tipo REG_MULTI_SZ usando **#** como separador, escriba:
```
REG QUERY HKLM\Software\Microsoft\ResKit\Nt\Setup /se #
```
Para mostrar la clave, el valor y los datos de las coincidencias exactas y con distinción de mayúsculas y minúsculas del sistema en la raíz HKLM del tipo de datos REG_SZ, escriba:
```
REG QUERY HKLM /f SYSTEM /t REG_SZ /c /e
```
Para mostrar la clave, el valor y los datos que coinciden con **0F** en los datos bajo la clave raíz HKCU del tipo de datos REG_BINARY.
```
REG QUERY HKCU /f 0F /d /t REG_BINARY
```
Para mostrar el valor y los datos de los nombres de valor null (valor predeterminado) en HKLM\SOFTWARE, escriba:
```
REG QUERY HKLM\SOFTWARE /ve
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)