---
title: recover
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 261bfd79d74323ad324246e21b84a5eb798ebcdf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867186"
---
# <a name="recover"></a>recover



Actualiza el estado de todos los discos en un grupo de discos, intenta recuperar los discos en un grupo de discos no válido y vuelve a sincronizar los volúmenes reflejados y RAID-5 que tiene datos obsoletos.

> [!IMPORTANT]
> Este comando DiskPart no está disponible en cualquier edición de Windows Vista.

## <a name="syntax"></a>Sintaxis

```
recover [noerr]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|noerr|sólo para scripting. Cuando se produce un error, DiskPart sigue procesando comandos como si no hubiera habido ningún error. Sin este parámetro, un error provoca que DiskPart se cierre con un código de error.|

## <a name="remarks"></a>Comentarios

-   Este comando funciona en un grupo de discos.
-   Este comando se aplica únicamente a grupos de discos dinámicos. Si este comando se utiliza en un grupo con un disco básico, no devolverá un error, pero no se realizará ninguna acción.
-   Este comando funciona en discos con error o con errores. También funciona en los volúmenes que son erróneos, con errores o en estado de redundancia con errores.
-   Debe seleccionarse un disco que forma parte de un grupo de discos para que este comando se ejecute correctamente. Use la **seleccione disco** comando para seleccionar un disco y cambiar el foco a ella.

## <a name="BKMK_examples"></a>Ejemplos

Para recuperar el grupo de discos que contiene el disco con el foco, escriba:
```
recover
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

