---
title: bitsadmin getnotifyflags
description: 'El tema comandos de Windows para **bitsadmin getnotifyflags** : recupera las marcas de notificación para el trabajo especificado.'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 56ee3a30050b6cc934b35bab24e9508911ea250e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381479"
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
|Trabajo|El nombre para mostrar del trabajo o el GUID|

## <a name="remarks"></a>Comentarios

El trabajo puede contener una o varias de las siguientes marcas de notificación.

|-----|-----| | 0x001 | Generar un evento cuando se hayan transferido todos los archivos del trabajo. | | 0x002 | Genera un evento cuando se produce un error. | | 0x004 | Deshabilitar notificaciones. | | 0x008 | Genera un evento cuando se modifica el trabajo o se realiza el progreso de la transferencia. |

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se recuperan las marcas de notificación del trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /GetNotifyFlags myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)