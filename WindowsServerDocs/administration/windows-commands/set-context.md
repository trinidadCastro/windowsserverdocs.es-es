---
title: Establecer contexto
description: Artículo de referencia para set context, que establece el contexto para la creación de instantáneas.
ms.topic: article
ms.assetid: fc16c7dd-e8f0-4c2a-8742-0bddb2848bfd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3506a79ec713f26b16f58cd8cda3903ce6503adf
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87882707"
---
# <a name="set-contex"></a>Establecer contextual.

Establece el contexto para la creación de la instantánea. Si se usa sin parámetros, **set context** muestra la ayuda en el símbolo del sistema.



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

## <a name="remarks"></a>Observaciones

-   El contexto *clientaccessible* es persistente de forma predeterminada.

## <a name="examples"></a>Ejemplos

Para evitar que se eliminen las instantáneas al salir de DiskShadow, escriba:
```
set context persistent
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)