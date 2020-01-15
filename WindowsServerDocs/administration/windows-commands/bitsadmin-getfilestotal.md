---
title: bitsadmin getfilestotal
description: 'Temas de comandos de Windows para **bitsadmin getfilestotal** : recupera el número de archivos del trabajo especificado.'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: e5c28d8e970bd7db896073bf8cddb168ffe9deff
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75946846"
---
# <a name="bitsadmin-getfilestotal"></a>bitsadmin getfilestotal



Recupera el número de archivos del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetFilesTotal <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|

## <a name="BKMK_examples"></a>Ejemplos

En el ejemplo siguiente se recupera el número de archivos incluidos en el trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /GetFilesTotal myDownloadJob
```

## <a name="see-also"></a>Consulta también

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
