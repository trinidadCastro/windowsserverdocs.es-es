---
title: bitsadmin getbytestotal
description: Tema de los comandos de Windows para **getbytestotal bitsadmin** -recupera el tamaño del trabajo especificado.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 784e0bfa-7b09-4262-9104-adbc9beb479b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d7a33b02dbdea63e87bd8ce56a2d50e15ca19d05
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882866"
---
# <a name="bitsadmin-getbytestotal"></a>bitsadmin getbytestotal



Recupera el tamaño del trabajo especificado

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetBytesTotal <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|

## <a name="BKMK_examples"></a>Ejemplos

El ejemplo siguiente recupera el tamaño del trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /GetBytesTotal myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)