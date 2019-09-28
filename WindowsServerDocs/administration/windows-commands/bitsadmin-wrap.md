---
title: bitsadmin wrap
description: El tema comandos de Windows para **bitsadmin ajusta** -ajusta cualquier línea de texto de salida que se extienda más allá del borde derecho de la ventana de comandos a la línea siguiente.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 14e57522-539d-4621-ad15-09f7a44ccab7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5609fb6f38716795a545e0c7fe3939f893a8c8d5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380684"
---
# <a name="bitsadmin-wrap"></a>bitsadmin wrap

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ajusta la salida para que quepa en una ventana de comandos.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /Wrap Job
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------|--------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|

## <a name="remarks"></a>Comentarios

Especifique antes que otros modificadores. De forma predeterminada, todos los modificadores, excepto el modificador de la [monitor bitsadmin](bitsadmin-monitor.md) , encapsulan la salida.

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se recupera información del trabajo denominado *myDownloadJob* y se incluye la salida.

```
C:\>bitsadmin /Wrap /Info myDownloadJob /verbose
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
