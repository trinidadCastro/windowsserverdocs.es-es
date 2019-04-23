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
ms.openlocfilehash: 3d3eba4f62842fa7f862cd4eaea6830e6a08397a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868136"
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

## <a name="BKMK_examples"></a>Ejemplos

En el ejemplo siguiente se cambia todos los archivos de trabajo denominado *myDownloadJob* cuya dirección URL remota comienza con *http://stageserver* a *http://prodserver*.
```
C:\>bitsadmin /ReplaceRemotePrefix myDownloadJob http://stageserver http://prodserver
```

## <a name="additional-information"></a>Información adicional

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)