---
title: recover
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8cc3a73d-9456-41a0-b375-2b4cc37c3992
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a83bb7502145cc09116241ea255e31b5f9981791
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384741"
---
# <a name="recover"></a>recover



Actualiza el estado de todos los discos de un grupo de discos, intenta recuperar discos en un grupo de discos no válido y vuelve a sincronizar los volúmenes reflejados y los volúmenes RAID-5 que tienen datos obsoletos.

> [!IMPORTANT]
> Este comando DiskPart no está disponible en ninguna edición de Windows Vista.

## <a name="syntax"></a>Sintaxis

```
recover [noerr]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Noerr|Solo para scripting. Cuando se encuentra un error, DiskPart sigue procesando comandos como si no se hubiera producido el error. Sin este parámetro, un error hace que DiskPart salga con un código de error.|

## <a name="remarks"></a>Comentarios

-   Este comando funciona en un grupo de discos.
-   Este comando solo se aplica a grupos de discos dinámicos. Si este comando se usa en un grupo con un disco básico, no devolverá un error, pero no se realizará ninguna acción.
-   Este comando funciona en discos con errores o con errores. También funciona en los volúmenes con errores, con errores o con errores de redundancia.
-   Se debe seleccionar un disco que forme parte de un grupo de discos para que este comando se ejecute correctamente. Use el comando **Seleccionar disco** para seleccionar un disco y desplazar el foco a él.

## <a name="BKMK_examples"></a>Example

Para recuperar el grupo de discos que contiene el disco que tiene el foco, escriba:
```
recover
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

