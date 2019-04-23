---
title: convert gpt
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b3b1b747-0a7a-4be2-8487-2c4be16ee190
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e838f68162b6faabf2ecbc7dea2ce840235890c0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859116"
---
# <a name="convert-gpt"></a>convert gpt



Convierte un disco básico vacío con el estilo de partición (MBR) registro de arranque maestro en un disco básico con el estilo de particiones GUID (GPT) de la tabla de partición.

Para obtener instrucciones sobre cómo usar este comando, consulte [cambiar un disco de registro de arranque maestro a un disco de tabla de particiones GUID](https://go.microsoft.com/fwlink/?LinkId=207049) (https://go.microsoft.com/fwlink/?LinkId=207049).

## <a name="syntax"></a>Sintaxis

```
convert gpt [noerr]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|noerr|sólo para scripting. Cuando se produce un error, DiskPart sigue procesando comandos como si no hubiera habido ningún error. Sin este parámetro, un error provoca que DiskPart se cierre con un código de error.|

## <a name="remarks"></a>Comentarios

> [!IMPORTANT]
> El disco debe estar vacío para poder convertirlo en un disco GPT. Realizar una copia de seguridad de los datos y, a continuación, elimine todas las particiones o volúmenes antes de convertir el disco.
-   El tamaño de disco mínimo necesario para la conversión a GPT es 128 megabytes.
-   Debe seleccionarse un disco MBR básico para que esta operación se realice correctamente. Use la **seleccione disco** comando para seleccionar un disco básico y desplace el foco a ella.

## <a name="BKMK_examples"></a>Ejemplos

Para convertir un disco básico de estilo de partición MBR al estilo de partición GPT, escriba:
```
convert gpt
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

