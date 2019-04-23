---
title: bitsadmin getreplyprogress
description: Tema de los comandos de Windows para **getreplyprogress bitsadmin** -recupera el tamaño y el progreso de la respuesta del servidor.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7f7cb0b4-ad95-44fd-a35d-0ddf5fc0b0d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aafecfb5873392ef86e6f7cceb139091b15e3b99
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852936"
---
# <a name="bitsadmin-getreplyprogress"></a>bitsadmin getreplyprogress

Recupera el tamaño y el progreso de la respuesta del servidor.

**BITS 1.2 y versiones anteriores**: No compatible.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetReplyProgress <Job>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|

## <a name="remarks"></a>Comentarios

Válido solo en los trabajos de respuesta de carga.

## <a name="BKMK_examples"></a>Ejemplos

En el ejemplo siguiente se recupera el progreso de la respuesta del trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /GetReplyProgress myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)