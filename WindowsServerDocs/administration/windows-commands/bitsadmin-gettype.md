---
title: bitsadmin gettype
description: 'Tema de comandos de Windows para **bitsadmin GetType** : recupera el tipo de trabajo del trabajo especificado.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bec16f04-3e95-4587-889e-3de6ad03c9c8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ca46cb813809621f4fa79b3265198206729a392c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381341"
---
# <a name="bitsadmin-gettype"></a>bitsadmin gettype



Recupera el tipo de trabajo del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetType <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|

## <a name="remarks"></a>Comentarios

El tipo puede ser DOWNLOAD, UPLOAD, UPLOAD-RESPONse o UNKNOWN.

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se recupera el tipo de trabajo para el trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /GetType myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)