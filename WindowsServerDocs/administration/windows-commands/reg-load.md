---
title: carga de registro
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ebc75ad78b7334f4d48a085f6870a443b31fa2a9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852196"
---
# <a name="reg-load"></a>carga de registro



Operaciones de escritura guardan las subclaves y entradas en una subclave diferente en el registro. Diseñado para su uso con los archivos temporales que se usan para solucionar problemas o modificar entradas del registro.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
reg load KeyName FileName
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<KeyName>|Especifica la ruta de acceso completa de la subclave que se va a cargar. Para especificar los equipos remotos, incluya el nombre del equipo (con el formato \\ \\ComputerName\) como parte de la *KeyName*. Si se omite \\ \\ComputerName\ hace que la operación de forma predeterminada en el equipo local. El *KeyName* debe incluir una clave raíz válida. Son claves raíz válido para el equipo local: HKLM, HKCU, HKCR, HKU y HKCC. Si se especifica un equipo remoto, las claves raíz válidas son: HKLM y HKU.|
|\<FileName>|Especifica el nombre y ruta de acceso del archivo que se va a cargar. Este archivo debe crearse de antemano con el **guardar reg** operación y una extensión hiv.|
|/?|Muestra la Ayuda de **reg carga** en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

En la tabla siguiente se enumera los valores devueltos por la **reg carga** operación.

|Valor|Descripción|
|-----|-----------|
|0|Correcto|
|1|Error|

## <a name="BKMK_examples"></a>Ejemplos

Para cargar el archivo denominado Subarbtemp.hiv a la clave HKLM\Subarbtemp, escriba:
```
REG LOAD HKLM\TempHive TempHive.hiv
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)