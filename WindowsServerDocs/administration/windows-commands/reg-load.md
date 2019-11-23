---
title: carga de reg
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3b0b2b1b-f510-4108-9e9d-7057e924aa6e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: db661e311e3fe8c393750716de5dab375e7817f4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384700"
---
# <a name="reg-load"></a>carga de reg



Escribe las subclaves y entradas guardadas en una subclave diferente del registro. Diseñado para su uso con archivos temporales que se usan para solucionar problemas o editar entradas del registro.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
reg load KeyName FileName
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<KeyName >|Especifica la ruta de acceso completa de la subclave que se va a cargar. Para especificar equipos remotos, incluya el nombre del equipo (en el formato \\\\ComputerName\) como parte del nombre de *clave*. Si se omite \\\\ComputerName \, la operación se realiza de forma predeterminada en el equipo local. *KeyName* debe incluir una clave raíz válida. Las claves raíz válidas para el equipo local son: HKLM, HKCU, HKCR, HKU y HKCC. Si se especifica un equipo remoto, las claves raíz válidas son: HKLM y HKU.|
|\<nombre de archivo >|Especifica el nombre y la ruta de acceso del archivo que se va a cargar. Este archivo debe crearse de antemano mediante la operación de **guardado del registro** y la extensión. HIV.|
|/?|Muestra la ayuda de la **carga del registro** en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

En la tabla siguiente se enumeran los valores devueltos para la operación de **carga del registro** .

|Valor|Descripción|
|-----|-----------|
|0|Correcto|
|1|Error|

## <a name="BKMK_examples"></a>Example

Para cargar el archivo denominado TempHive. HIV en la clave HKLM\TempHive, escriba:
```
REG LOAD HKLM\TempHive TempHive.hiv
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)