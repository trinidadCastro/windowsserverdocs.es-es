---
title: bitsadmin setnotifyflags
description: Windows Commands topic for bitsadmin setnotifyflags, que establece las marcas de notificación de eventos para el trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d5763d95-94a6-45ca-9e03-891c20047e06
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fd3001fa4ae7f51cab92556f4f2f498511cca5ae
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849288"
---
# <a name="bitsadmin-setnotifyflags"></a>bitsadmin setnotifyflags

Establece las marcas de notificación de eventos para el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetNotifyFlags <Job> <NotifyFlags>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|
|NotifyFlags|Ver comentarios|

## <a name="remarks"></a>Comentarios

El parámetro **NotifyFlags** puede contener una o varias de las siguientes marcas de notificación.

|-----|-----| | 1 | Generar un evento cuando se hayan transferido todos los archivos del trabajo. | | 2 | Genera un evento cuando se produce un error. | | 4 | Deshabilitar notificaciones. |

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se establecen las marcas de notificación para el trabajo de eventos transferidos y de error para el trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /SetNotifyFlags myDownloadJob 3
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)