---
title: REG (consulta)
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0e6a0d7c-ed9b-4318-833d-33f265a81f39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ea66a7d96435309a3b30b67f45bc68200f30dd2f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722528"
---
# <a name="reg-query"></a>REG (consulta)



Devuelve una lista del siguiente nivel de subclaves y entradas que se encuentran en una subclave especificada del registro.



## <a name="syntax"></a>Sintaxis

```
reg query <KeyName> [{/v <ValueName> | /ve}] [/s] [/se <Separator>] [/f <Data>] [{/k | /d}] [/c] [/e] [/t <Type>] [/z]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<> KeyName|Especifica la ruta de acceso completa de la subclave. Para especificar equipos remotos, incluya el nombre del equipo (en el \\ \\formato\) COMPUTERNAME como parte del *keyName*). Si \\ \\se omite COMPUTERNAME \, la operación se realizará de forma predeterminada en el equipo local. *KeyName* debe incluir una clave raíz válida. Las claves raíz válidas para el equipo local son: HKLM, HKCU, HKCR, HKU y HKCC. Si se especifica un equipo remoto, las claves raíz válidas son: HKLM y HKU.|
|/v \<ValueName>|Especifica el nombre del valor del registro que se va a consultar. Si se omite, se devuelven todos los nombres de valor para *keyName* . *ValueName* para este parámetro es opcional si también se usa la opción **/f** .|
|/ve|Ejecuta una consulta para los nombres de valor que están vacíos.|
|/s|Especifica que se consulten todas las subclaves y los nombres de valor de forma recursiva.|
|> \<separador de/se|Especifica el separador de valor único que se va a buscar en el tipo de nombre de valor REG_MULTI_SZ. Si no se especifica *separator* , se usa **\ 0** .|
|/f \<> de datos|Especifica los datos o el patrón que se va a buscar. Use comillas dobles si una cadena contiene espacios. Si no se especifica, se usa un carácter comodín (**&#42;**) como patrón de búsqueda.|
|/k|Especifica que se busque solo en los nombres de clave.|
|/d|Especifica que solo se busque en los datos.|
|/C|Especifica que la consulta distingue mayúsculas de minúsculas. De forma predeterminada, las consultas no distinguen mayúsculas de minúsculas.|
|/e|Especifica que solo se devuelvan las coincidencias exactas. De forma predeterminada, se devuelven todas las coincidencias.|
|/t \<> tipo|Especifica los tipos de registro que se van a buscar. Los tipos válidos son: REG_SZ, REG_MULTI_SZ, REG_EXPAND_SZ, REG_DWORD, REG_BINARY REG_NONE. Si no se especifica, se busca en todos los tipos.|
|/z|Especifica que se incluya el equivalente numérico para el tipo de registro en los resultados de la búsqueda.|
|/?|Muestra la ayuda de **reg Query** en el símbolo del sistema.|

## <a name="remarks-optional-section"></a>Comentario \<sección opcional>

En la tabla siguiente se enumeran los valores devueltos para la operación **reg Query** .

|Value|Descripción|
|-----|-----------|
|0|Correcto|
|1|Error|

## <a name="examples"></a>Ejemplos

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
Para mostrar la clave, el valor y los datos que coinciden con **0F** en los datos de la clave raíz HKCU del tipo de datos REG_BINARY.
```
REG QUERY HKCU /f 0F /d /t REG_BINARY
```
Para mostrar el valor y los datos de los nombres de valor null (valor predeterminado) en HKLM\SOFTWARE, escriba:
```
REG QUERY HKLM\SOFTWARE /ve
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)