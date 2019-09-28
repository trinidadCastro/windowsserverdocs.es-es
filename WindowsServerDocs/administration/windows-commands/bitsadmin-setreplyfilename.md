---
title: bitsadmin setreplyfilename
description: 'Temas de comandos de Windows para **bitsadmin setreplyfilename** : especifique la ruta de acceso del archivo que contiene la respuesta del servidor.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c26d3342-0533-40b1-a13e-e09678232b25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a490b5bc565549d096b6f43f42758f77570fcb26
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380424"
---
# <a name="bitsadmin-setreplyfilename"></a>bitsadmin setreplyfilename

Especifique la ruta de acceso del archivo que contiene la respuesta del servidor.

**BITS 1,2 y versiones anteriores**: No compatible.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetReplyFileName <Job> <Path>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|
|Path|Ubicación para colocar la respuesta del servidor|

## <a name="remarks"></a>Comentarios

Válido solo para trabajos de carga y respuesta.

## <a name="BKMK_examples"></a>Example

En el ejemplo siguiente se establece el nombre de archivo de respuesta pathfor el trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /SetReplyFileName myDownloadJob c:\reply
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)