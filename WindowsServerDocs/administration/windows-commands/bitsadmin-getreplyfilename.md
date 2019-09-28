---
title: bitsadmin getreplyfilename
description: 'Temas de comandos de Windows para **bitsadmin getreplyfilename** : obtiene la ruta de acceso del archivo que contiene la respuesta del servidor.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 85447184-1732-4816-a365-2e3599551bf8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 96b77e9bd19cdc094e6b025e143b05aff7bc60d5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381267"
---
# <a name="bitsadmin-getreplyfilename"></a>bitsadmin getreplyfilename

Obtiene la ruta de acceso del archivo que contiene la respuesta del servidor.

**BITS 1,2 y versiones anteriores**: No compatible.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetReplyFileName <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|

## <a name="remarks"></a>Comentarios

Válido solo para trabajos de carga y respuesta.

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se recupera el nombre de archivo de respuesta del trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /GetReplyFileName myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)