---
title: Establecer contexto
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fc16c7dd-e8f0-4c2a-8742-0bddb2848bfd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 16f71d831f374f495abf2239cb8e694eee69efdf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370977"
---
# <a name="set-contex"></a>Establecer contextual.



Establece el contexto para la creación de la instantánea. Si se usa sin parámetros, **set context** muestra la ayuda en el símbolo del sistema.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
set context {clientaccessible | persistent [nowriters] | volatile [nowriters]}
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|clientaccessible|Especifica que las versiones de cliente de Windows pueden usar la instantánea.|
|permanentes|Especifica que la instantánea se mantiene entre la salida del programa, el restablecimiento o el reinicio.|
|volatil|Elimina la instantánea al salir o restablecer.|
|nowriters|Especifica que todos los escritores están excluidos.|

## <a name="remarks"></a>Comentarios

-   El contexto *clientaccessible* es persistente de forma predeterminada.

## <a name="BKMK_examples"></a>Example

Para evitar que se eliminen las instantáneas al salir de DiskShadow, escriba:
```
set context persistent
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)