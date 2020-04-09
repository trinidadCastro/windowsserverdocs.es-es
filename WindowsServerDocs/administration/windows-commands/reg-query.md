---
title: REG (consulta)
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0e6a0d7c-ed9b-4318-833d-33f265a81f39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bf21933e1ce9928048f0f07ed502dfcab75d1783
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836398"
---
# <a name="reg-query"></a>REG (consulta)



Devuelve una lista del siguiente nivel de subclaves y entradas que se encuentran en una subclave especificada del registro.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
reg query <KeyName> [{/v <ValueName> | /ve}] [/s] [/se <Separator>] [/f <Data>] [{/k | /d}] [/c] [/e] [/t <Type>] [/z]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<KeyName >|Especifica la ruta de acceso completa de la subclave. Para especificar equipos remotos, incluya el nombre del equipo (en el formato \\\\ComputerName\) como parte del nombre de *clave*. Si se omite \\\\ComputerName \, la operación se realiza de forma predeterminada en el equipo local. *KeyName* debe incluir una clave raíz válida. Las claves raíz válidas para el equipo local son: HKLM, HKCU, HKCR, HKU y HKCC. Si se especifica un equipo remoto, las claves raíz válidas son: HKLM y HKU.|
|/v \<ValueName >|Especifica el nombre del valor del registro que se va a consultar. Si se omite, se devuelven todos los nombres de valor para *keyName* . *ValueName* para este parámetro es opcional si también se usa la opción **/f** .|
|/ve|Ejecuta una consulta para los nombres de valor que están vacíos.|
|/s|Especifica que se consulten todas las subclaves y los nombres de valor de forma recursiva.|
|separador de \</se >|Especifica el separador de valor único que se va a buscar en el tipo de nombre de valor REG_MULTI_SZ. Si no se especifica *separator* , se usa **\ 0** .|
|/f \<> de datos|Especifica los datos o el patrón que se va a buscar. Use comillas dobles si una cadena contiene espacios. Si no se especifica, se usa **&#42;** un carácter comodín () como patrón de búsqueda.|
|/k|Especifica que se busque solo en los nombres de clave.|
|/d|Especifica que solo se busque en los datos.|
|/c|Especifica que la consulta distingue mayúsculas de minúsculas. De forma predeterminada, las consultas no distinguen mayúsculas de minúsculas.|
|/e|Especifica que solo se devuelvan las coincidencias exactas. De forma predeterminada, se devuelven todas las coincidencias.|
|/t \<tipo >|Especifica los tipos de registro que se van a buscar. Los tipos válidos son: REG_SZ, REG_MULTI_SZ, REG_EXPAND_SZ, REG_DWORD, REG_BINARY REG_NONE. Si no se especifica, se busca en todos los tipos.|
|/z|Especifica que se incluya el equivalente numérico para el tipo de registro en los resultados de la búsqueda.|
|/?|Muestra la ayuda de **reg Query** en el símbolo del sistema.|

## <a name="remarks-optional-section"></a>Comentarios \<sección opcional >

En la tabla siguiente se enumeran los valores devueltos para la operación **reg Query** .

|Valor|Descripción|
|-----|-----------|
|0|Correcto|
|1|Error|

## <a name="examples"></a><a name=BKMK_examples></a>Example

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