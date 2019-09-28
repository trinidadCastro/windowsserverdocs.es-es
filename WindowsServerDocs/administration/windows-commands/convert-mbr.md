---
title: convert mbr
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a635a4c0-af73-4330-b021-51d483424537
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 47001415158b3bdb0b06af9114b995a6f0634da1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379070"
---
# <a name="convert-mbr"></a>convert mbr



Convierte un disco básico vacío con el estilo de partición de tabla de particiones GUID (GPT) en un disco básico con el estilo de partición de registro de arranque maestro (MBR).

> [!IMPORTANT]
> El disco debe estar vacío para poder convertirlo en un disco MBR. Realice una copia de seguridad de los datos y, a continuación, elimine todas las particiones o volúmenes antes de convertir el disco.

Para obtener instrucciones sobre cómo usar este comando, consulte [cambiar un disco de tabla de particiones GUID a un disco de registro de arranque maestro](https://go.microsoft.com/fwlink/?LinkId=207050) (https://go.microsoft.com/fwlink/?LinkId=207050).

## <a name="syntax"></a>Sintaxis

```
convert mbr [noerr]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Noerr|Solo para scripting. Cuando se encuentra un error, DiskPart sigue procesando comandos como si no se hubiera producido el error. Sin este parámetro, un error hace que DiskPart salga con un código de error.|

## <a name="remarks"></a>Comentarios

-   Se debe seleccionar un disco básico para que esta operación se realice correctamente. Use el comando **Seleccionar disco** para seleccionar un disco básico y desplazar el foco a él.

## <a name="BKMK_examples"></a>Example

Para convertir un disco básico de estilo de partición GPT a estilo de partición MBR, escriba >:
```
convert mbr
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

