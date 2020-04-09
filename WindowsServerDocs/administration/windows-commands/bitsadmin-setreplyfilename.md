---
title: bitsadmin setreplyfilename
description: Windows Commands topic for bitsadmin setreplyfilename, que especifica la ruta de acceso del archivo que contiene la respuesta del servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c26d3342-0533-40b1-a13e-e09678232b25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fd45174a7deac89cc943fb19d544e372c0198139
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849188"
---
# <a name="bitsadmin-setreplyfilename"></a>bitsadmin setreplyfilename

Especifica la ruta de acceso del archivo que contiene la respuesta del servidor.

**BITS 1,2 y versiones anteriores**: no se admiten.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetReplyFileName <Job> <Path>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|El nombre para mostrar del trabajo o el GUID|
|Ruta de acceso|Ubicación para colocar la respuesta del servidor|

## <a name="remarks"></a>Comentarios

Válido solo para trabajos de carga y respuesta.

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se establece el nombre de archivo de respuesta pathfor el trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /SetReplyFileName myDownloadJob c:\reply
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)