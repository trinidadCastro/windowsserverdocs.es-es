---
title: convert mbr
description: Temas de comandos de Windows para convertir MBR, que convierte un disco básico vacío con el estilo de partición de tabla de particiones GUID (GPT) en un disco básico con el estilo de partición de registro de arranque maestro (MBR).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a635a4c0-af73-4330-b021-51d483424537
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eeaf79a380fb5f1074d2bbef004537804caa0d8d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847208"
---
# <a name="convert-mbr"></a>convert mbr

Convierte un disco básico vacío con el estilo de partición de tabla de particiones GUID (GPT) en un disco básico con el estilo de partición de registro de arranque maestro (MBR).

> [!IMPORTANT]
> El disco debe estar vacío para poder convertirlo en un disco MBR. Haga una copia de seguridad de los datos y, a continuación, elimine todas las particiones o volúmenes antes de convertir el disco.

Para obtener instrucciones sobre cómo usar este comando, consulte [cambiar un disco de tabla de particiones GUID a un disco de registro de arranque maestro](https://go.microsoft.com/fwlink/?LinkId=207050) (https://go.microsoft.com/fwlink/?LinkId=207050).

## <a name="syntax"></a>Sintaxis

```
convert mbr [noerr]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|noerr|Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error.|

## <a name="remarks"></a>Comentarios

-   Se debe seleccionar un disco básico para que esta operación se realice correctamente. Use el comando **Seleccionar disco** para seleccionar un disco básico y desplazar el foco a él.

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para convertir un disco básico de estilo de partición GPT a estilo de partición MBR, escriba >:
```
convert mbr
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

