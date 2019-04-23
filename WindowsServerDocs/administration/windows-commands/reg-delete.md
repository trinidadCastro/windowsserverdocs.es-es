---
title: reg delete
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cee05071-1607-4ab1-b8ab-65caebeb85c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 369ef3bda37ab8e143a14f0f9707b9bbf14bd5f8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877086"
---
# <a name="reg-delete"></a>reg delete



Elimina una subclave o las entradas del registro.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
Reg delete <KeyName> [{/v ValueName | /ve | /va}] [/f]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<KeyName>|Especifica la ruta de acceso completa de la subclave o entrada va a eliminar. Para especificar un equipo remoto, incluya el nombre del equipo (con el formato \\ \\ComputerName\) como parte de la *KeyName*. Si se omite \\ \\ComputerName\ hace que la operación de forma predeterminada en el equipo local. El *KeyName* debe incluir una clave raíz válida. Son claves raíz válido para el equipo local: HKLM, HKCU, HKCR, HKU y HKCC. Si se especifica un equipo remoto, las claves raíz válidas son: HKLM y HKU.|
|/v \<ValueName>|Elimina una entrada específica bajo la subclave. Si no se especifica ninguna entrada, a continuación, todas las entradas y las subclaves bajo la subclave se eliminarán.|
|/ve|Especifica que se eliminarán solo las entradas que no tienen ningún valor.|
|/va|Elimina todas las entradas bajo la subclave especificada. No se eliminan las subclaves bajo la subclave especificada.|
|/f|Elimina la subclave del registro existente o la entrada sin pedir confirmación.|
|/?|Muestra la Ayuda de **reg delete** en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

En la tabla siguiente se enumera los valores devueltos por la **reg delete** operación.

|Valor|Descripción|
|-----|-----------|
|0|Correcto|
|1|Error|

## <a name="BKMK_examples"></a>Ejemplos

Para eliminar la clave del registro del tiempo de espera y su todas las subclaves y valores, escriba:
```
REG DELETE HKLM\Software\MyCo\MyApp\Timeout
```
Para eliminar el valor del registro MTU HKLM\Software\MyCo en el equipo llamado ZODIAC, escriba:
```
REG DELETE \\ZODIAC\HKLM\Software\MyCo /v MTU
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)