---
title: Eliminar registro
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 7156bf58b27da1602931f0dc1903de71d86764e7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384765"
---
# <a name="reg-delete"></a>Eliminar registro



Elimina una subclave o entradas del registro.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
Reg delete <KeyName> [{/v ValueName | /ve | /va}] [/f]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<KeyName >|Especifica la ruta de acceso completa de la subclave o entrada que se va a eliminar. Para especificar un equipo remoto, incluya el nombre del equipo (en el formato \\\\ComputerName\) como parte del nombre de *clave*. Si se omite \\\\ComputerName \, la operación se realiza de forma predeterminada en el equipo local. *KeyName* debe incluir una clave raíz válida. Las claves raíz válidas para el equipo local son: HKLM, HKCU, HKCR, HKU y HKCC. Si se especifica un equipo remoto, las claves raíz válidas son: HKLM y HKU.|
|/v \<ValueName >|Elimina una entrada específica bajo la subclave. Si no se especifica ninguna entrada, se eliminarán todas las entradas y subclaves de la subclave.|
|/ve|Especifica que solo se eliminarán las entradas que no tengan ningún valor.|
|/va|Elimina todas las entradas de la subclave especificada. Las subclaves de la subclave especificada no se eliminan.|
|/f|Elimina la subclave del registro existente o la entrada sin solicitar confirmación.|
|/?|Muestra la ayuda de **reg Delete** en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

En la tabla siguiente se enumeran los valores devueltos para la operación de **eliminación de registro** .

|Valor|Descripción|
|-----|-----------|
|0|Correcto|
|1|Error|

## <a name="BKMK_examples"></a>Example

Para eliminar el tiempo de espera de la clave del registro y sus subclaves y valores, escriba:
```
REG DELETE HKLM\Software\MyCo\MyApp\Timeout
```
Para eliminar la MTU del valor del registro en HKLM\Software\MyCo en el equipo denominado zodíaco, escriba:
```
REG DELETE \\ZODIAC\HKLM\Software\MyCo /v MTU
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)