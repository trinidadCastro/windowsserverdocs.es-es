---
title: volumen sin conexión
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b8f7192f-ea38-47d0-9d4e-58ef68336ae6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d17295f3367fed054a7f6a245bae44ea3494a4a8
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723451"
---
# <a name="offline-volume"></a>volumen sin conexión



Toma el volumen en línea que tiene el foco en el estado sin conexión.

> [!IMPORTANT]
> Este comando DiskPart no está disponible en ninguna edición de Windows Vista.

## <a name="syntax"></a>Sintaxis

```
offline volume [noerr]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|noerr|Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error.|

## <a name="remarks"></a>Observaciones

-   Se debe seleccionar un volumen para que esto se realice correctamente. Use el comando **seleccionar volumen** para seleccionar un disco y desplazar el foco a él.

## <a name="examples"></a>Ejemplos

Para desconectar el disco con el foco, escriba:
```
offline volume
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

