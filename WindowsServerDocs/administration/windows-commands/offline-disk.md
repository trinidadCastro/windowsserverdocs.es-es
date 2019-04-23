---
title: Disco sin conexión
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8fb9b3c3-0b2c-4192-a2e7-f706292653e3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 617371583a3f0cb3d0cb739845208e4216573d9c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834626"
---
# <a name="offline-disk"></a>Disco sin conexión



Toma el disco en línea con el foco al estado sin conexión.

> [!IMPORTANT]
> Este comando DiskPart no está disponible en cualquier edición de Windows Vista.

## <a name="syntax"></a>Sintaxis

```
offline disk [noerr]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|noerr|sólo para scripting. Cuando se produce un error, DiskPart sigue procesando comandos como si no hubiera habido ningún error. Sin este parámetro, un error provoca que DiskPart se cierre con un código de error.|

## <a name="remarks"></a>Comentarios

-   Este comando funciona en discos que están en modo en línea de SAN. Cambia el modo de SAN a sin conexión.
-   Si se desconecta un disco dinámico en un grupo de discos, el estado del disco cambia a **falta** y el grupo muestra un disco que está sin conexión. El disco que falta se mueve al grupo no válido. Si el disco dinámico es el último disco en el grupo, el estado del disco cambiará a **sin conexión**, y se eliminará el grupo vacío.
-   Debe seleccionarse un disco para el **disco sin conexión** comando se ejecute correctamente. Use la **seleccione disco** comando para seleccionar un disco y cambiar el foco a ella.

## <a name="BKMK_examples"></a>Ejemplos

Para desconectar el disco con el foco, escriba:
```
offline disk
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

