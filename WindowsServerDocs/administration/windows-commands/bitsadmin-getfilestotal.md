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
ms.openlocfilehash: 21240cfa1f0fa1f8e8fc3d2acf83f5df80812816
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857466"
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

##

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md) Vea también