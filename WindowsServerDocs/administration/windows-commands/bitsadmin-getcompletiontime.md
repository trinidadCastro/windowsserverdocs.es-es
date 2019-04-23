---
title: bitsadmin getcompletiontime
description: Tema de los comandos de Windows para **getcompletiontime bitsadmin** -recupera el tiempo que el trabajo ha finalizado la transferencia de datos.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7a4b3c1c-9832-4724-86b2-cce3c01bfa28
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a3790a91c4b347b982c0f0a023d5977a8d6cd1f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857386"
---
# <a name="bitsadmin-getcompletiontime"></a>bitsadmin getcompletiontime



Recupera el tiempo que el trabajo ha finalizado la transferencia de datos.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetCompletionTime <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|

## <a name="BKMK_examples"></a>Ejemplos

En el ejemplo siguiente se recupera el tiempo que el trabajo denominado *myDownloadJob* ha finalizado la transferencia de datos.
```
C:\>bitsadmin /GetCompletionTime myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)