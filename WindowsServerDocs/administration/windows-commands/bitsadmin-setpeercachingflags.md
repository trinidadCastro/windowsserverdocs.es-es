---
title: Bitsadmin setpeercachingflags
description: Tema de los comandos de Windows para **setpeercachingflags bitsadmin** -establece marcas que determinan si los archivos del trabajo pueden almacenarse en caché y sirve a elementos del mismo nivel y el trabajo puede descargar el contenido de elementos del mismo nivel.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3f54a127-fb68-49a5-b843-664ec833df67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2d50a6ccd83a6251808ca3d66437e52f641c60a7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814256"
---
# <a name="bitsadmin-setpeercachingflags"></a>Bitsadmin setpeercachingflags



Establece marcas que determinan si los archivos del trabajo pueden almacenarse en caché y sirve a elementos del mismo nivel y si el trabajo se puede descargar contenido de elementos del mismo nivel.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetPeerCachingFlags <Job> <value> 
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|
|Valor|El valor es un entero sin signo con la siguiente interpretación de los bits de la representación binaria.</br>1 - el trabajo puede descargar contenido de elementos del mismo nivel.</br>2 - los archivos del trabajo se pueden almacenar en caché y sirve a elementos del mismo nivel.|

## <a name="BKMK_examples"></a>Ejemplos

El ejemplo siguiente establece las marcas del trabajo denominado *myJob* que le permite descargar el contenido de elementos del mismo nivel.
```
C:\>bitsadmin / SetPeerCachingFlags myJob 1 
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)