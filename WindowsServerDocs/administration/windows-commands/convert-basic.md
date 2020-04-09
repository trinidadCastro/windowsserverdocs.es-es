---
title: convert basic
description: Tema de comandos de Windows para convertir básico, que convierte un disco dinámico vacío en un disco básico.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 61329896-3b56-4959-8d58-45cbe18ba860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8e0f6f5f04373042956d83bc9136c884c268e591
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847318"
---
# <a name="convert-basic"></a>convert basic

Convierte un disco dinámico vacío en un disco básico.

Para obtener instrucciones sobre cómo usar este comando, consulte [cambiar un disco dinámico a un disco básico](https://go.microsoft.com/fwlink/?LinkId=207048) (https://go.microsoft.com/fwlink/?LinkId=207048).

## <a name="syntax"></a>Sintaxis

```
convert basic [noerr]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|noerr|Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error.|

## <a name="remarks"></a>Comentarios

> [!IMPORTANT]
> El disco debe estar vacío para poder convertirlo en un disco básico. Haga una copia de seguridad de los datos y, a continuación, elimine todas las particiones o volúmenes antes de convertir el disco.

-   Se debe seleccionar un disco dinámico para que esta operación se realice correctamente. Use el comando **Seleccionar disco** para seleccionar un disco dinámico y cambiar el foco a él.

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para convertir el disco dinámico seleccionado en básico, escriba:
```
convert basic
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

