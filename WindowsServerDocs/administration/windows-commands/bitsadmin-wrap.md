---
title: bitsadmin wrap
description: Tema de los comandos de Windows para **bitsadmin encapsulado** -ajusta cualquier línea de texto de salida extender más allá del borde más a la derecha de la ventana de comandos a la línea siguiente.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4834a8a17c72394b6ee8f051ec76919af9880124
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881676"
---
# <a name="bitsadmin-wrap"></a>bitsadmin wrap

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ajusta para que quepa en una ventana de comandos de salida.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /Wrap Job
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------|--------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|

## <a name="remarks"></a>Comentarios

Especifique antes de otros modificadores. De forma predeterminada, todos los modificadores, excepto el [bitsadmin monitor](bitsadmin-monitor.md) modificador, ajustar el resultado.

## <a name="BKMK_examples"></a>Ejemplos

En el ejemplo siguiente se recupera información del trabajo denominado *myDownloadJob* y ajusta la salida.

```
C:\>bitsadmin /Wrap /Info myDownloadJob /verbose
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
