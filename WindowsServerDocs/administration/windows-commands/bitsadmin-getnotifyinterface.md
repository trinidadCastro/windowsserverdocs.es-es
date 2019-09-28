---
title: bitsadmin getnotifyinterface
description: 'Temas de comandos de Windows para **bitsadmin getnotifyinterface** : determina si otro programa ha registrado una interfaz de devolución de llamada com para el trabajo especificado.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 40bf9dd8-b167-406a-80a6-a5a6f1b8cf7f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 826e13cf8a3e54935ceb5a72ff82647cacfc3be5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381466"
---
# <a name="bitsadmin-getnotifyinterface"></a>bitsadmin getnotifyinterface

Determina si otro programa ha registrado una interfaz de devolución de llamada COM (la interfaz de notificación) para el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetNotifyInterface <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|

## <a name="remarks"></a>Comentarios

Muestra registrado o ANULAdo su registro.

> [!NOTE]
> No es posible determinar el programa que registró la interfaz de devolución de llamada.

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se recupera la interfaz Notify del trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /GetNotifyInterface myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)