---
title: bitsadmin getminretrydelay
description: Tema de los comandos de Windows para **getminretrydelay bitsadmin** -recupera la longitud de tiempo, en segundos, que espera el servicio después de encontrar un error transitorio antes de intentar transferir el archivo.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 54f0abab-c129-40ed-a603-50f464d26011
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2a6df9faab8340994ad9219a863ad8e50186ccd1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832206"
---
# <a name="bitsadmin-getminretrydelay"></a>bitsadmin getminretrydelay



Recupera la longitud de tiempo, en segundos, que espera el servicio después de encontrar un error transitorio antes de intentar transferir el archivo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetMinRetryDelay <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|

## <a name="BKMK_examples"></a>Ejemplos

En el ejemplo siguiente se recupera el retraso mínimo de reintentos del trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /GetMinRetryDelay myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)