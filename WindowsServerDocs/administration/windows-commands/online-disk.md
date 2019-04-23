---
title: disco en línea
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bc44a783-eaa4-40ca-be01-5703b5bf4eb3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4c30d0853ff0ae065f02c0ee198c8cdcb90c950b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858066"
---
# <a name="online-disk"></a>disco en línea



Aporta discos que están actualmente sin conexión a un estado en línea.

> [!IMPORTANT]
> Este comando no está disponible en cualquier edición de Windows Vista.

> [!IMPORTANT]
> Este comando generará un error si se usa en un disco de solo lectura.

Para obtener instrucciones sobre cómo usar este comando, consulte [reactivar un disco dinámico sin conexión que falta o](https://go.microsoft.com/fwlink/?LinkId=207046) (https://go.microsoft.com/fwlink/?LinkId=207046).

## <a name="syntax"></a>Sintaxis

```
online disk [noerr]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|noerr|sólo para scripting. Cuando se produce un error, DiskPart sigue procesando comandos como si no hubiera habido ningún error. Sin este parámetro, un error provoca que DiskPart se cierre con un código de error.|

## <a name="remarks"></a>Comentarios

-   Cuando se usa sin parámetros en Windows Vista, este comando funciona en un grupo de discos. Para los discos básicos, nunca hay más de un disco por grupo. Para discos dinámicos, el grupo incluye todos los discos dinámicos no externo.
-   Para los discos básicos, este comando intentará poner en línea el disco seleccionado y todos los volúmenes en ese disco.
-   Para discos dinámicos, este comando intentará poner en línea todos los discos que no están marcados como externos en el equipo local. También intentará poner en línea todos los volúmenes en el conjunto de discos dinámicos.
-   Si se conecta un disco dinámico en un grupo de discos y es el único disco en el grupo, a continuación, se vuelve a crear el grupo original y el disco se traslade a ese grupo. Si hay otros discos en el grupo y estén en línea, se agrega simplemente el disco en el grupo.
-   Si el grupo de un disco seleccionado contiene volúmenes reflejados o RAID-5, este comando también vuelve a sincronizar estos volúmenes.
-   Debe seleccionarse un disco para que este comando se ejecute correctamente. Use la **seleccione disco** comando para seleccionar un disco y cambiar el foco a ella.

## <a name="BKMK_examples"></a>Ejemplos

Para que el disco con el foco en línea, escriba:
```
online disk
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

