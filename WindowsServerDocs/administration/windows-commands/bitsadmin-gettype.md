---
title: bitsadmin gettype
description: Tema de los comandos de Windows para **bitsadmin gettype** -recupera el tipo de trabajo del trabajo especificado.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ff0118f14acbf4e9f37c02e660bd9c7f6e8d0f70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879436"
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
|Trabajo|Nombre para mostrar o el GUID del trabajo|

## <a name="remarks"></a>Comentarios

El tipo se puede descargar, cargar, respuesta de carga o desconocido.

## <a name="BKMK_examples"></a>Ejemplos

En el ejemplo siguiente se recupera el tipo de trabajo del trabajo denominado *myDownloadJob*.
```
C:\>bitsadmin /GetType myDownloadJob
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)