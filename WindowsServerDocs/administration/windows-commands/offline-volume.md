---
title: Volumen desconectado
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b8f7192f-ea38-47d0-9d4e-58ef68336ae6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bd14492a40046472f43f37d79c393c9467fe4a88
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873746"
---
# <a name="offline-volume"></a>Volumen desconectado



Desconecta el volumen en línea con el foco al estado sin conexión.

> [!IMPORTANT]
> Este comando DiskPart no está disponible en cualquier edición de Windows Vista.

## <a name="syntax"></a>Sintaxis

```
offline volume [noerr]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|noerr|sólo para scripting. Cuando se produce un error, DiskPart sigue procesando comandos como si no hubiera habido ningún error. Sin este parámetro, un error provoca que DiskPart se cierre con un código de error.|

## <a name="remarks"></a>Comentarios

-   Debe seleccionarse un volumen para que se realice correctamente. Use la **seleccione volumen** comando para seleccionar un disco y cambiar el foco a ella.

## <a name="BKMK_examples"></a>Ejemplos

Para desconectar el disco con el foco, escriba:
```
offline volume
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

