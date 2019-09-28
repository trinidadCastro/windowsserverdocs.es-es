---
title: bitsadmin getpeercachingflags
description: 'Temas de comandos de Windows para **bitsadmin getpeercachingflags** : recupera marcas que determinan si los archivos del trabajo se pueden almacenar en caché y servir a los elementos del mismo nivel, y si bits puede descargar el contenido del trabajo desde los elementos del mismo nivel.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3c3c9f28-4c04-4c49-a23a-dee5bbcc8981
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6b86214b5289a59e8db2ecff065ab3b8cd17007e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381439"
---
# <a name="bitsadmin-getpeercachingflags"></a>bitsadmin getpeercachingflags

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera marcas que determinan si los archivos del trabajo se pueden almacenar en caché y servir a los elementos del mismo nivel, y si BITS puede descargar el contenido del trabajo de los elementos del mismo nivel.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetPeerCachingFlags <Job> 
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------|--------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|

## <a name="BKMK_examples"></a>Example
En el ejemplo siguiente se recuperan las marcas del trabajo denominado *myJob*.

```
C:\>bitsadmin /GetPeerCachingFlags myJob
```

## <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)


