---
title: convert gpt
description: Temas de comandos de Windows para convertir GPT, que convierte un disco básico vacío con el estilo de partición de registro de arranque maestro (MBR) en un disco básico con el estilo de partición de tabla de particiones GUID (GPT).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b3b1b747-0a7a-4be2-8487-2c4be16ee190
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3c1ffe61245f7752ccc81d21d513fa00acd7b68b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847288"
---
# <a name="convert-gpt"></a>convert gpt

Convierte un disco básico vacío con el estilo de partición de registro de arranque maestro (MBR) en un disco básico con el estilo de partición de tabla de particiones GUID (GPT).

Para obtener instrucciones sobre cómo utilizar este comando, consulte [cambiar un disco de registro de arranque maestro a un disco de tabla de particiones GUID](https://go.microsoft.com/fwlink/?LinkId=207049) (https://go.microsoft.com/fwlink/?LinkId=207049).

## <a name="syntax"></a>Sintaxis

```
convert gpt [noerr]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|noerr|Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error.|

## <a name="remarks"></a>Comentarios

> [!IMPORTANT]
> El disco debe estar vacío para poder convertirlo en un disco GPT. Haga una copia de seguridad de los datos y, a continuación, elimine todas las particiones o volúmenes antes de convertir el disco.
> -   El tamaño de disco mínimo necesario para la conversión a GPT es de 128 megabytes.
> -   Se debe seleccionar un disco MBR básico para que esta operación se realice correctamente. Use el comando **Seleccionar disco** para seleccionar un disco básico y desplazar el foco a él.

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para convertir un disco básico de estilo de partición MBR a estilo de partición GPT, escriba:
```
convert gpt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

