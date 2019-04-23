---
title: Bitsadmin setmaxdownloadtime
description: Tema de los comandos de Windows para **setmaxdownloadtime bitsadmin** -establece el tiempo de espera de descarga en segundos.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 16b96cf1-5738-415c-9b9d-c4ea8d5e4fec
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f13b44429bec2718af1a648f273fead18d4e9e08
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830996"
---
# <a name="bitsadmin-setmaxdownloadtime"></a>Bitsadmin setmaxdownloadtime



Establece el tiempo de espera de descarga en segundos.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetMaxDownloadTime <Job> <Timeout>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|
|tiempoDeEspera|El tiempo de espera en segundos|

## <a name="remarks"></a>Comentarios

-   N/D

## <a name="BKMK_examples"></a>Ejemplos

En el ejemplo siguiente se establece el tiempo de espera del trabajo denominado *myDownloadJob* en 10 segundos.
```
C:\>bitsadmin /SetMaxDownloadTime myDownloadJob 10
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)