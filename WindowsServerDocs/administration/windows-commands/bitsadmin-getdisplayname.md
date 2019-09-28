---
title: bitsadmin getdisplayname
description: 'Temas de comandos de Windows para **bitsadmin getDisplayName** : recupera el nombre para mostrar del trabajo especificado.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e5c0e76c-4cc6-42d8-ac30-30bf3dc11b9b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 229bd245f9e810fc6aeb856bbfba253b9ab8a9f0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381628"
---
# <a name="bitsadmin-getdisplayname"></a>bitsadmin getdisplayname



Recupera el nombre para mostrar del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetDisplayName <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se recupera el nombre para mostrar del trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /GetDisplayName myDownloadJob
```
Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)