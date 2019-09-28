---
title: REG Export
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0ad9526f-1e29-4fa5-9d2d-feaa92f12d7c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7fb3a779ffe5a4e7d513ca9a3afed8ee90901688
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384749"
---
# <a name="reg-export"></a>REG Export



Copia las subclaves, entradas y valores especificados del equipo local en un archivo para su transferencia a otros servidores.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
Reg export KeyName FileName [/y]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|@no__t 0KeyName >|Especifica la ruta de acceso completa de la subclave. La operación de exportación solo funciona con el equipo local. KeyName debe incluir una clave raíz válida. Las claves raíz válidas son: HKLM, HKCU, HKCR, HKU y HKCC.|
|\<Nombre de archivo >|Especifica el nombre y la ruta de acceso del archivo que se va a crear durante la operación. El archivo debe tener una extensión. reg.|
|/y|Sobrescribe cualquier archivo existente con el nombre *filename* sin pedir confirmación.|
|/?|Muestra ayuda para la **exportación reg** en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

En la tabla siguiente se enumeran los valores devueltos para la operación de **exportación del registro** .

|Valor|Descripción|
|-----|-----------|
|0|Correcto|
|1|Error|

## <a name="BKMK_examples"></a>Example

Para exportar el contenido de todas las subclaves y valores de la clave MyApp al archivo CopiaAp. reg, escriba:
```
reg export HKLM\Software\MyCo\MyApp AppBkUp.reg
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)