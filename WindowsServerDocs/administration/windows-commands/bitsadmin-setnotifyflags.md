---
title: bitsadmin setnotifyflags
description: Tema de los comandos de Windows para **setnotifyflags bitsadmin** -establece el evento de marcas de notificación para el trabajo especificado.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: bc817e03e0f1916ea392830d14985a7a1377d69a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868796"
---
# <a name="bitsadmin-setnotifyflags"></a>bitsadmin setnotifyflags

Establece el evento de marcas de notificación para el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetNotifyFlags <Job> <NotifyFlags>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|
|NotifyFlags|Vea la sección Comentarios|

## <a name="remarks"></a>Comentarios

El **NotifyFlags** parámetro puede contener una o varias de las siguientes marcas de notificación.

|---|---| | 1 | Generar un evento cuando se han transferido todos los archivos en el trabajo. | | 2 | Generar un evento cuando se produce un error. | | 4 | Deshabilitar las notificaciones. |

## <a name="BKMK_examples"></a>Ejemplos

El ejemplo siguiente establece las marcas de notificación para transferir y los eventos de error del trabajo de trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /SetNotifyFlags myDownloadJob 3
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)