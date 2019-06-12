---
title: bitsadmin getfilestotal
description: Tema de los comandos de Windows para **getfilestotal bitsadmin** -recupera el número de archivos en el trabajo especificado.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c5de113e-f29c-4cd3-9392-0e300018d516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b5f6d32b3410b182c510cf40b9def5370efafdc4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435121"
---
# <a name="bitsadmin-getfilestotal"></a>bitsadmin getfilestotal



Recupera el número de archivos en el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetFilesTotal <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|

## <a name="BKMK_examples"></a>Ejemplos

El ejemplo siguiente recupera el número de archivos incluidos en el trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /GetFilesTotal myDownloadJob
```

# #

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md) Vea también