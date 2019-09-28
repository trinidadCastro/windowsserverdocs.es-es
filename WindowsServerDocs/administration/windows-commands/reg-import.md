---
title: REG Import
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0be103de-08fc-4f02-b590-361782680b3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2e1c7920a64469717c30cfcddda7b8002db5ba10
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384732"
---
# <a name="reg-import"></a>REG Import



Copia el contenido de un archivo que contiene las subclaves del registro exportadas, las entradas y los valores en el registro del equipo local.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
Reg import FileName
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<Nombre de archivo >|Especifica el nombre y la ruta de acceso del archivo que tiene el contenido que se va a copiar en el registro del equipo local. Este archivo debe crearse de antemano mediante **reg Export**.|
|/?|Muestra ayuda para **reg Import** en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

En la tabla siguiente se enumeran los valores devueltos para la operación de **importación de reg** .

|Valor|Descripción|
|-----|-----------|
|0|Correcto|
|1|Error|

## <a name="BKMK_examples"></a>Example

Para importar las entradas del registro desde el archivo llamado CopiaAp. reg, escriba:
```
reg import AppBkUp.reg
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)