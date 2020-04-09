---
title: Establecer contexto
description: Tema de comandos de Windows para set context, que establece el contexto para la creación de instantáneas.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fc16c7dd-e8f0-4c2a-8742-0bddb2848bfd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 162fefc46bc3b8ae39dcb41a387e50ed98fefa70
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834568"
---
# <a name="set-contex"></a>Establecer contextual.

Establece el contexto para la creación de la instantánea. Si se usa sin parámetros, **set context** muestra la ayuda en el símbolo del sistema.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
set context {clientaccessible | persistent [nowriters] | volatile [nowriters]}
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|clientaccessible|Especifica que las versiones de cliente de Windows pueden usar la instantánea.|
|permanentes|Especifica que la instantánea se mantiene entre la salida del programa, el restablecimiento o el reinicio.|
|volatile|Elimina la instantánea al salir o restablecer.|
|nowriters|Especifica que todos los escritores están excluidos.|

## <a name="remarks"></a>Comentarios

-   El contexto *clientaccessible* es persistente de forma predeterminada.

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para evitar que se eliminen las instantáneas al salir de DiskShadow, escriba:
```
set context persistent
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)