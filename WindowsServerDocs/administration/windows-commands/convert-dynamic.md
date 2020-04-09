---
title: convert dynamic
description: Tema de comandos de Windows para convertir dinámico, que convierte un disco básico en un disco dinámico.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7b8fa4b1-850f-4e48-b05f-871c883ea33c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6ad84cf2ecb6808b68110187b52f3fc13590b491
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847328"
---
# <a name="convert-dynamic"></a>convert dynamic

Convierte un disco básico en un disco dinámico.

Para obtener instrucciones sobre cómo usar este comando, consulte [cambiar un disco básico a un disco dinámico](https://go.microsoft.com/fwlink/?LinkId=207047) (https://go.microsoft.com/fwlink/?LinkId=207047).

## <a name="syntax"></a>Sintaxis

```
convert dynamic [noerr]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|noerr|Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error.|

## <a name="remarks"></a>Comentarios

-   Todas las particiones existentes en el disco básico se convertirán en volúmenes simples.
-   Se debe seleccionar un disco básico para que esta operación se realice correctamente. Use el comando **Seleccionar disco** para seleccionar un disco básico y desplazar el foco a él.

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para convertir un disco básico en un disco dinámico, escriba:
```
convert dynamic
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

