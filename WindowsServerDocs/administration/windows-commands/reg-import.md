---
title: reg import
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 7d1dd1b61848671b528c62fd22fe656e14fda7b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861846"
---
# <a name="reg-import"></a>reg import



Copia el contenido de un archivo que contiene exporta las subclaves del registro, entradas y valores en el registro del equipo local.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
Reg import FileName
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<FileName>|Especifica el nombre y ruta de acceso del archivo que tiene contenido que se copiará en el registro del equipo local. Este archivo debe crearse de antemano con **reg export**.|
|/?|Muestra la Ayuda de **reg import** en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

En la tabla siguiente se enumera los valores devueltos por la **reg import** operación.

|Valor|Descripción|
|-----|-----------|
|0|Correcto|
|1|Error|

## <a name="BKMK_examples"></a>Ejemplos

Para importar las entradas del registro desde el archivo denominado AppBkUp.reg, escriba:
```
reg import AppBkUp.reg
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)