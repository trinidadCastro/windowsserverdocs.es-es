---
title: bitsadmin setnotifyflags
description: 'Windows Commands topic for **bitsadmin setnotifyflags** : establece las marcas de notificación de eventos para el trabajo especificado.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d5763d95-94a6-45ca-9e03-891c20047e06
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d9cfabf05610cbbe8fa65fd16b0d33e161dcef9b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380449"
---
# <a name="bitsadmin-setnotifyflags"></a>bitsadmin setnotifyflags

Establece las marcas de notificación de eventos para el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetNotifyFlags <Job> <NotifyFlags>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|
|NotifyFlags|Ver comentarios|

## <a name="remarks"></a>Comentarios

El parámetro **NotifyFlags** puede contener una o varias de las siguientes marcas de notificación.

|-----|-----| | 1 | Generar un evento cuando se hayan transferido todos los archivos del trabajo. | | 2 | Genera un evento cuando se produce un error. | | 4 | Deshabilitar notificaciones. |

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se establecen las marcas de notificación para el trabajo de eventos transferidos y de error para el trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /SetNotifyFlags myDownloadJob 3
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)