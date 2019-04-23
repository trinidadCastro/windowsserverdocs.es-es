---
title: bitsadmin setreplyfilename
description: Tema de los comandos de Windows para **setreplyfilename bitsadmin** -especificar la ruta de acceso del archivo que contiene la respuesta del servidor.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: b86d4137f661e9953d6d397b2fbc890393bbd8a0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852876"
---
# <a name="bitsadmin-setreplyfilename"></a>bitsadmin setreplyfilename

Especifique la ruta de acceso del archivo que contiene la respuesta del servidor.

**BITS 1.2 y versiones anteriores**: No compatible.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetReplyFileName <Job> <Path>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|
|Ruta de acceso|Ubicación para colocar la respuesta del servidor|

## <a name="remarks"></a>Comentarios

Válido solo en los trabajos de respuesta de carga.

## <a name="BKMK_examples"></a>Ejemplos

El ejemplo siguiente establece la ruta de búsquedaPor del nombre de archivo de respuesta del trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /SetReplyFileName myDownloadJob c:\reply
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)