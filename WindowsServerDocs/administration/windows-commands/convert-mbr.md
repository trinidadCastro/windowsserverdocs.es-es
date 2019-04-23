---
title: convert mbr
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: da8d62567863bc38a5aa0b35a8f3fe4ee24cc888
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834616"
---
# <a name="convert-mbr"></a>convert mbr



Convierte un disco básico vacío con el estilo de partición de tabla de particiones GUID (GPT) en un disco básico con el estilo de partición (MBR) registro de arranque maestro.

> [!IMPORTANT]
> El disco debe estar vacío para poder convertirlo en un disco MBR. Realizar una copia de seguridad de los datos y, a continuación, elimine todas las particiones o volúmenes antes de convertir el disco.

Para obtener instrucciones sobre cómo usar este comando, consulte [cambiar un disco de tabla de particiones GUID a un disco de registro de arranque maestro](https://go.microsoft.com/fwlink/?LinkId=207050) (https://go.microsoft.com/fwlink/?LinkId=207050).

## <a name="syntax"></a>Sintaxis

```
convert mbr [noerr]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|noerr|sólo para scripting. Cuando se produce un error, DiskPart sigue procesando comandos como si no hubiera habido ningún error. Sin este parámetro, un error provoca que DiskPart se cierre con un código de error.|

## <a name="remarks"></a>Comentarios

-   Debe seleccionarse un disco básico para que esta operación se realice correctamente. Use la **seleccione disco** comando para seleccionar un disco básico y desplace el foco a ella.

## <a name="BKMK_examples"></a>Ejemplos

Para convertir un disco básico de estilo de partición GPT para el estilo de partición MBR, escriba >:
```
convert mbr
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

