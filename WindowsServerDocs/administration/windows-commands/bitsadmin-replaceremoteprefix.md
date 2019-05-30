---
title: bitsadmin replaceremoteprefix
description: Tema de los comandos de Windows para **bitsadmin replaceremoteprefix** -todos los archivos en el trabajo cuya dirección URL remota comienza con *OldPrefix* se cambió para usar *NewPrefix*.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d0e0abb1-bdb4-4c74-abbc-16c809f5fd81
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 848c57736c3530e296cffb970237149b4634de67
ms.sourcegitcommit: d84dc3d037911ad698f5e3e84348b867c5f46ed8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2019
ms.locfileid: "66266521"
---
# <a name="bitsadmin-replaceremoteprefix"></a>bitsadmin replaceremoteprefix



Todos los archivos en el trabajo cuya dirección URL remota comienza con *OldPrefix* se cambió para usar *NewPrefix*.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /ReplaceRemotePrefix <Job> <OldPrefix> <NewPrefix
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|
|OldPrefix|Prefijo de dirección URL existente|
|NewPrefix|Nuevo prefijo de dirección URL|

## <a name="examples"></a>Ejemplos

En el ejemplo siguiente se cambia todos los archivos de trabajo denominado *myDownloadJob* cuya dirección URL remota comienza con *http://stageserver* a *http://prodserver* .
```
C:\>bitsadmin /ReplaceRemotePrefix myDownloadJob http://stageserver http://prodserver
```

## <a name="additional-information"></a>Información adicional

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)