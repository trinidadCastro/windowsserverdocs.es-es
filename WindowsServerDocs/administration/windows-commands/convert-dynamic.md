---
title: convertir dinámico
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7b8fa4b1-850f-4e48-b05f-871c883ea33c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 15c15d14aeb440c5d7862f0a304f223988f52bbe
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379101"
---
# <a name="convert-dynamic"></a>convertir dinámico



Convierte un disco básico en un disco dinámico.

Para obtener instrucciones sobre cómo usar este comando, consulte [cambiar un disco básico a un disco dinámico](https://go.microsoft.com/fwlink/?LinkId=207047) (https://go.microsoft.com/fwlink/?LinkId=207047).

## <a name="syntax"></a>Sintaxis

```
convert dynamic [noerr]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Noerr|Solo para scripting. Cuando se encuentra un error, DiskPart sigue procesando comandos como si no se hubiera producido el error. Sin este parámetro, un error hace que DiskPart salga con un código de error.|

## <a name="remarks"></a>Comentarios

-   Todas las particiones existentes en el disco básico se convertirán en volúmenes simples.
-   Se debe seleccionar un disco básico para que esta operación se realice correctamente. Use el comando **Seleccionar disco** para seleccionar un disco básico y desplazar el foco a él.

## <a name="BKMK_examples"></a>Example

Para convertir un disco básico en un disco dinámico, escriba:
```
convert dynamic
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

