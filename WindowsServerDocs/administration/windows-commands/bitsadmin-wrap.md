---
title: bitsadmin wrap
description: Tema de comandos de Windows para el ajuste de bitsadmin, que ajusta cualquier línea de texto de salida que se extienda más allá del borde derecho de la ventana de comandos a la línea siguiente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 14e57522-539d-4621-ad15-09f7a44ccab7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 009a0452f44c4944ae110ca6b9e0570793c32a72
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848758"
---
# <a name="bitsadmin-wrap"></a>bitsadmin wrap

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ajusta cualquier línea de texto de salida que se extienda más allá del borde derecho de la ventana de comandos a la línea siguiente.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /Wrap Job
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------|--------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|

## <a name="remarks"></a>Comentarios

Especifique antes que otros modificadores. De forma predeterminada, todos los modificadores, excepto el modificador de la [monitor bitsadmin](bitsadmin-monitor.md) , encapsulan la salida.

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se recupera información del trabajo denominado *myDownloadJob* y se incluye la salida.

```
C:\>bitsadmin /Wrap /Info myDownloadJob /verbose
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
