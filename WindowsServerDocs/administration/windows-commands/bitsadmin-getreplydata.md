---
title: bitsadmin getreplydata
description: Tema de los comandos de Windows para **getreplydata bitsadmin** -recupera los datos de respuesta del servidor en formato hexadecimal.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 819f97d5-b255-4b2d-9f63-0daa73915434
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 78d70a44d6881568c8d92db145fdf22a260ee8af
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883826"
---
# <a name="bitsadmin-getreplydata"></a>bitsadmin getreplydata

Recupera los datos de respuesta del servidor en formato hexadecimal.

**BITS 1.2 y versiones anteriores**: No compatible.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetReplyData <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|

## <a name="remarks"></a>Comentarios

Válido solo en los trabajos de respuesta de carga.

## <a name="BKMK_examples"></a>Ejemplos

El ejemplo siguiente recupera los datos de respuesta del trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /GetReplyData myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)