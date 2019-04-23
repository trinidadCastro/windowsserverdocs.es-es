---
title: convert basic
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 61329896-3b56-4959-8d58-45cbe18ba860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5a5aef930689e809b9b697e5f428177610ff723b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862406"
---
# <a name="convert-basic"></a>convert basic



Convierte un disco dinámico vacío en un disco básico.

Para obtener instrucciones sobre cómo usar este comando, consulte [cambiar un disco dinámico vuelve a un disco básico](https://go.microsoft.com/fwlink/?LinkId=207048) (https://go.microsoft.com/fwlink/?LinkId=207048).

## <a name="syntax"></a>Sintaxis

```
convert basic [noerr]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|noerr|sólo para scripting. Cuando se produce un error, DiskPart sigue procesando comandos como si no hubiera habido ningún error. Sin este parámetro, un error provoca que DiskPart se cierre con un código de error.|

## <a name="remarks"></a>Comentarios

> [!IMPORTANT]
> El disco debe estar vacío para poder convertirlo en un disco básico. Realizar una copia de seguridad de los datos y, a continuación, elimine todas las particiones o volúmenes antes de convertir el disco.
-   Debe seleccionarse un disco dinámico para que realizar esta operación correctamente. Use la **seleccione disco** comando para seleccionar un disco dinámico y cambiar el foco a ella.

## <a name="BKMK_examples"></a>Ejemplos

Para convertir el disco dinámico seleccionado en basic, escriba:
```
convert basic
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

