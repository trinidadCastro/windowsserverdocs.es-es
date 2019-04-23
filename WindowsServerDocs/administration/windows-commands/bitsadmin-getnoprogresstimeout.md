---
title: bitsadmin getnoprogresstimeout
description: Tema de los comandos de Windows para **getnoprogresstimeout bitsadmin** -recupera el período de tiempo, en segundos, que el servicio intenta transferir el archivo después de que se produce un error transitorio.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9cd9b19b-cbb4-4352-8419-978080f016b6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9563b68b8012a49471b56e3b8f2fbd60d1c69756
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850806"
---
# <a name="bitsadmin-getnoprogresstimeout"></a>bitsadmin getnoprogresstimeout



Recupera el período de tiempo, en segundos, que el servicio intenta transferir el archivo después de que se produce un error transitorio.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetNoProgressTimeout <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|

## <a name="BKMK_examples"></a>Ejemplos

En el ejemplo siguiente se recupera el valor de tiempo de espera del progreso del trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /GetNoProgressTimeout myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)