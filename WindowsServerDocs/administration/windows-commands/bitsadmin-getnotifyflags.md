---
title: bitsadmin getnotifyflags
description: Tema de los comandos de Windows para **getnotifyflags bitsadmin** -recupera las marcas de notificación para el trabajo especificado.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d4657e6c-8959-4db7-a4af-e73d3f80ecf8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 690e94805c5e61d96603e4ade102fb3a4bda409e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889286"
---
# <a name="bitsadmin-getnotifyflags"></a>bitsadmin getnotifyflags



Recupera las marcas de notificación para el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetNotifyFlags <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|

## <a name="remarks"></a>Comentarios

El trabajo puede contener una o varias de las siguientes marcas de notificación.

|---|---| | 0 x 001 | Generar un evento cuando se han transferido todos los archivos en el trabajo. | | 0 x 002 | Generar un evento cuando se produce un error. | | 0 x 004 | Deshabilitar las notificaciones. | | 0 x 008 | Generar un evento cuando el trabajo se modifica o se realiza un progreso de transferencia. |

## <a name="BKMK_examples"></a>Ejemplos

El ejemplo siguiente recupera las marcas de notificación del trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /GetNotifyFlags myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)