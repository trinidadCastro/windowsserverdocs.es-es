---
title: disco sin conexión
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: f28d473cdb557d6adb3aaf235bebdfbc4e78b24a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372599"
---
# <a name="offline-disk"></a>disco sin conexión



Toma el disco en línea con el foco en el estado sin conexión.

> [!IMPORTANT]
> Este comando DiskPart no está disponible en ninguna edición de Windows Vista.

## <a name="syntax"></a>Sintaxis

```
offline disk [noerr]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Noerr|Solo para scripting. Cuando se encuentra un error, DiskPart sigue procesando comandos como si no se hubiera producido el error. Sin este parámetro, un error hace que DiskPart salga con un código de error.|

## <a name="remarks"></a>Comentarios

-   Este comando funciona en los discos que están en modo SAN online. Cambia el modo de SAN a sin conexión.
-   Si se desconecta un disco dinámico de un grupo de discos, el estado del disco cambia a **ausente** y el grupo muestra un disco sin conexión. El disco que falta se mueve al grupo no válido. Si el disco dinámico es el último disco del grupo, el estado del disco cambiará a **sin conexión**y se quitará el grupo vacío.
-   Se debe seleccionar un disco para que el comando de **disco sin conexión** se realice correctamente. Use el comando **Seleccionar disco** para seleccionar un disco y desplazar el foco a él.

## <a name="BKMK_examples"></a>Example

Para desconectar el disco con el foco, escriba:
```
offline disk
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

