---
title: bitsadmin getfilestransferred
description: Tema de los comandos de Windows para **getfilestransferred bitsadmin** -recupera el número de archivos transferido para el trabajo especificado.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e282815c-938b-4ac0-a09d-9baafb656dcb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5df7f2abfdad6780878b1f00da44c772eecf9fba
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822266"
---
# <a name="bitsadmin-getfilestransferred"></a>bitsadmin getfilestransferred



Recupera el número de archivos transferidos del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetFilesTransferred <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|

## <a name="BKMK_examples"></a>Ejemplos

El ejemplo siguiente recupera el número de archivos transferidos en el trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /GetFilesTransferred myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)