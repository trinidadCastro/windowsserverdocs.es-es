---
title: bitsadmin replaceremoteprefix
description: 'Tema de comandos de Windows para **bitsadmin replaceremoteprefix** : todos los archivos del trabajo cuya dirección URL remota comienza con *OldPrefix* se cambian para usar *NewPrefix*.'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: ee896a337b571487797967d3ce0bf1f1b17e7507
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380797"
---
# <a name="bitsadmin-replaceremoteprefix"></a>bitsadmin replaceremoteprefix

Todos los archivos del trabajo cuya dirección URL remota comienza con *OldPrefix* se cambian para usar *NewPrefix*.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /ReplaceRemotePrefix <Job> <OldPrefix> <NewPrefix
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|
|OldPrefix|Prefijo de dirección URL existente|
|NewPrefix|Nuevo prefijo de dirección URL|

## <a name="examples"></a>Ejemplos

En el ejemplo siguiente se cambian todos los archivos del trabajo denominado *myDownloadJob* cuya dirección URL remota comienza con *http://stageserver* a *http://prodserver* .

```
C:\>bitsadmin /ReplaceRemotePrefix myDownloadJob http://stageserver http://prodserver
```

## <a name="additional-information"></a>Información adicional

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)