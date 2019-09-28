---
title: bitsadmin setpriority
description: 'Temas de comandos de Windows para **bitsadmin SetPriority** : establece la prioridad del trabajo especificado.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 90788363-01a2-4d7c-a560-a3eba45b5e9e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 60564350928f917ca1861684e042304d5d380426
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380443"
---
# <a name="bitsadmin-setpriority"></a>bitsadmin setpriority



Establece la prioridad del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetPriority <Job> <Priority>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|
|Prioridad|Uno de los siguientes valores:</br>-FOREGROUND</br>-ALTO</br>-NORMAL</br>-LOW|

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se establece la prioridad del trabajo denominado *myDownloadJob* en normal.
```
C:\>bitsadmin /SetPriority myDownloadJob NORMAL
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)