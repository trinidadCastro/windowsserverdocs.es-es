---
title: Vssadmin cambiar el tamaño de shadowstorage
description: Descripción del comando vssadmin Resize shadowstorage
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 03/05/2020
ms.localizationpriority: medium
ms.openlocfilehash: e0d2adbb11320bd6a28ae9d83e357d5ec4b89de9
ms.sourcegitcommit: fc900eb19ac26c3d6bc2de179cc4b2c1e971043e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/10/2020
ms.locfileid: "79039655"
---
# <a name="vssadmin-resize-shadowstorage"></a>Vssadmin cambiar el tamaño de shadowstorage

>Se aplica a: Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

Cambia el tamaño de la cantidad máxima de espacio de almacenamiento que se puede usar para el almacenamiento de instantáneas.

La cantidad mínima de espacio de almacenamiento que se puede usar para el almacenamiento de instantáneas se puede especificar mediante el valor del registro **MinDiffAreaFileSize** . Para obtener más información, vea [MinDiffAreaFileSize](https://docs.microsoft.com/windows/win32/backup/registry-keys-for-backup-and-restore#mindiffareafilesize).

> [!WARNING]
> Cambiar el tamaño de la Asociación de almacenamiento puede hacer desaparecer las instantáneas.

## <a name="syntax"></a>Sintaxis

```cmd
vssadmin resize shadowstorage /for=<ForVolumeSpec> /on=<OnVolumeSpec> [/maxsize=<MaxSizeSpec>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---|---|
`/for=<ForVolumeSpec>`  | Especifica el volumen para el que se va a cambiar el tamaño máximo del espacio de almacenamiento.
`/on=<OnVolumeSpec>` | Especifica el volumen de almacenamiento.
`[/maxsize=<MaxSizeSpec>]` |  Especifica la cantidad máxima de espacio que se puede utilizar para almacenar las instantáneas. Si no se especifica ningún valor para/MAXSIZE, no se aplica ningún límite en la cantidad de espacio de almacenamiento que se puede usar.  <br> <br> El valor de tamañoMáxEspec debe ser 1 MB o superior y debe expresarse en una de las siguientes unidades: KB, MB, GB, TB, PB o EB. Si no se especifica ninguna unidad, tamañoMáxEspec usa bytes de forma predeterminada.

## <a name="examples"></a>Ejemplos

```cmd
vssadmin Resize ShadowStorage /For=C: /On=D: /MaxSize=900MB
vssadmin Resize ShadowStorage /For=C: /On=D: /MaxSize=UNBOUNDED
vssadmin Resize ShadowStorage /For=C: /On=C: /MaxSize=20%
```

## <a name="additional-references"></a>Referencias adicionales

* [Clave de sintaxis de línea de comandos](https://docs.microsoft.com/windows-server/administration/windows-commands/command-line-syntax-key)
* [List](vssadmin.md)