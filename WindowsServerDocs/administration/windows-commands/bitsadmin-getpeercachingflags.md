---
title: Bitsadmin getpeercachingflags
description: Tema de los comandos de Windows para **getpeercachingflags bitsadmin** -recupera las marcas que determinan si los archivos del trabajo pueden almacenarse en caché y sirve a elementos del mismo nivel y BITS pueden descargar contenido para el trabajo de equipos del mismo nivel.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 28f248bab3e3cc3f5c7dd4f5f878f0b6d776029b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434924"
---
# <a name="bitsadmin-getpeercachingflags"></a>Bitsadmin getpeercachingflags

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera las marcas que determinan si los archivos del trabajo pueden almacenarse en caché y sirve a elementos del mismo nivel y BITS pueden descargar contenido para el trabajo de equipos del mismo nivel.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetPeerCachingFlags <Job> 
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------|--------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|

## <a name="BKMK_examples"></a>Ejemplos
El ejemplo siguiente recupera las marcas del trabajo denominado *myJob*.

```
C:\>bitsadmin /GetPeerCachingFlags myJob
```

## <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)


