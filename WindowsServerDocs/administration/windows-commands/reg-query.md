---
title: reg query
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 1e239184cc5d118a858d012528fd8135f0b834e5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827756"
---
# <a name="reg-query"></a>reg query



Devuelve una lista de las subclaves y entradas que se encuentran bajo la subclave especificada en el registro del siguiente nivel.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
reg query <KeyName> [{/v <ValueName> | /ve}] [/s] [/se <Separator>] [/f <Data>] [{/k | /d}] [/c] [/e] [/t <Type>] [/z]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<KeyName>|Especifica la ruta de acceso completa de la subclave. Para especificar los equipos remotos, incluya el nombre del equipo (con el formato \\ \\ComputerName\) como parte de la *KeyName*. Si se omite \\ \\ComputerName\ hace que la operación de forma predeterminada en el equipo local. El *KeyName* debe incluir una clave raíz válida. Son claves raíz válido para el equipo local: HKLM, HKCU, HKCR, HKU y HKCC. Si se especifica un equipo remoto, las claves raíz válidas son: HKLM y HKU.|
|/v \<ValueName>|Especifica el nombre del valor del registro que se van a consultar. Si se omite, el valor de todos los nombres para *KeyName* se devuelven. *ValueName* para este parámetro es opcional si el **/f** también se usa la opción.|
|/ve|Ejecuta una consulta para los nombres de los valores que están vacías.|
|/s|Especifica que todas las subclaves y valor de forma recursiva de nombres de la consulta.|
|/se \<separador >|Especifica el separador de valor único que se va a buscar en el tipo de nombre de valor REG_MULTI_SZ. Si *separador* no se especifica, **\0** se utiliza.|
|/f \<datos >|Especifica los datos o un patrón para buscar. Si una cadena contiene espacios, utilice comillas dobles. Si no se especifica, un carácter comodín (**&#42;**) se utiliza como el patrón de búsqueda.|
|/k|Especifica que solo los nombres de clave de búsqueda.|
|/d|Especifica que la búsqueda sólo en los datos.|
|/c|Especifica que la consulta distingue mayúsculas de minúsculas. De forma predeterminada, las consultas no distinguen mayúsculas de minúsculas.|
|/e|Especifica que se devuelvan a solamente coincidencias exactas. De forma predeterminada, se devuelven todas las coincidencias.|
|/t \<tipo >|Especifica los tipos de registro para buscar. Los tipos válidos son: REG_SZ, REG_MULTI_SZ, REG_EXPAND_SZ, REG_DWORD, REG_BINARY, REG_NONE. Si no se especifica, se buscan todos los tipos.|
|/z|Especifica que se incluya el equivalente numérico para el tipo de registro en los resultados de búsqueda.|
|/?|Muestra la Ayuda de **reg query** en el símbolo del sistema.|

## <a name="remarks-optional-section"></a>Comentarios \<sección opcional >

En la tabla siguiente se enumera los valores devueltos por la **reg query** operación.

|Valor|Descripción|
|-----|-----------|
|0|Correcto|
|1|Error|

## <a name="BKMK_examples"></a>Ejemplos

Para mostrar el valor del valor de nombre de la versión de la clave HKLM\Software\Microsoft\ResKit, escriba:
```
REG QUERY HKLM\Software\Microsoft\ResKit /v Version
```
Para mostrar todas las subclaves y valores en la clave \\ABC\HKLM\Software\Microsoft\ResKit\Nt\Setup en un equipo remoto denominado ABC, escriba:
```
REG QUERY \\ABC\HKLM\Software\Microsoft\ResKit\Nt\Setup /s
```
Para mostrar todas las subclaves y valores del tipo REG_MULTI_SZ mediante **#** como separador, escriba:
```
REG QUERY HKLM\Software\Microsoft\ResKit\Nt\Setup /se #
```
Para mostrar la clave, valor y datos para exacta y distingue mayúsculas de minúsculas compara del sistema bajo la raíz HKLM del tipo REG_SZ, tipo de datos:
```
REG QUERY HKLM /f SYSTEM /t REG_SZ /c /e
```
Para mostrar la clave, valor y los datos que coinciden con **0F** en los datos en la clave de raíz HKCU de datos de tipo REG_BINARY.
```
REG QUERY HKCU /f 0F /d /t REG_BINARY
```
Para mostrar el valor y los datos para los nombres de valor es null (valor predeterminado) en HKLM\SOFTWARE, escriba:
```
REG QUERY HKLM\SOFTWARE /ve
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)