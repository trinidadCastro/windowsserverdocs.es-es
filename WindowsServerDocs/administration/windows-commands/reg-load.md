---
title: carga de reg
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3b0b2b1b-f510-4108-9e9d-7057e924aa6e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2523876e2ea2305ede3289226c934c9352825ba9
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722538"
---
# <a name="reg-load"></a>carga de reg



Escribe las subclaves y entradas guardadas en una subclave diferente del registro. Diseñado para su uso con archivos temporales que se usan para solucionar problemas o editar entradas del registro.



## <a name="syntax"></a>Sintaxis

```
reg load KeyName FileName
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<> KeyName|Especifica la ruta de acceso completa de la subclave que se va a cargar. Para especificar equipos remotos, incluya el nombre del equipo (en el \\ \\formato\) COMPUTERNAME como parte del *keyName*). Si \\ \\se omite COMPUTERNAME \, la operación se realizará de forma predeterminada en el equipo local. *KeyName* debe incluir una clave raíz válida. Las claves raíz válidas para el equipo local son: HKLM, HKCU, HKCR, HKU y HKCC. Si se especifica un equipo remoto, las claves raíz válidas son: HKLM y HKU.|
|\<Nombre de archivo>|Especifica el nombre y la ruta de acceso del archivo que se va a cargar. Este archivo debe crearse de antemano mediante la operación de **guardado del registro** y la extensión. HIV.|
|/?|Muestra la ayuda de la **carga del registro** en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

En la tabla siguiente se enumeran los valores devueltos para la operación de **carga del registro** .

|Value|Descripción|
|-----|-----------|
|0|Correcto|
|1|Error|

## <a name="examples"></a>Ejemplos

Para cargar el archivo denominado TempHive. HIV en la clave HKLM\TempHive, escriba:
```
REG LOAD HKLM\TempHive TempHive.hiv
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)