---
title: Establecer el contexto
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6f24e795f2d7c92d462cf822e70e4830b53827e5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845856"
---
# <a name="set-contex"></a>Conjunto contextual



Establece el contexto de creación de instantáneas. Si se utiliza sin parámetros, **establecer el contexto de** muestra la Ayuda en el símbolo del sistema.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
set context {clientaccessible | persistent [nowriters] | volatile [nowriters]}
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|clientaccessible|Especifica que la instantánea se puede utilizar en las versiones de cliente de Windows.|
|Persistente|Especifica que la instantánea se mantiene en el reinicio, restablecimiento o cierra el programa.|
|volatile|Elimina la instantánea en sale o restablece.|
|NoWriters|Especifica que se excluyen todos los escritores.|

## <a name="remarks"></a>Comentarios

-   El *clientaccessible* contexto es persistente de forma predeterminada.

## <a name="BKMK_examples"></a>Ejemplos

Para evitar que las instantáneas que se eliminen al salir de DiskShadow, escriba:
```
set context persistent
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)