---
title: aprovecha
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9b0a21cf-3bef-4ade-b8f1-ac42f9203947
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 500dc5cfcd5e2bba4cfbc3cb5ef81a9065ea53cf
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725676"
---
# <a name="expose"></a>aprovecha



Expone una instantánea persistente como letra de unidad, recurso compartido o punto de montaje.



## <a name="syntax"></a>Sintaxis

```
expose <ShadowID> {<Drive:> | <Share> | <MountPoint>}
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|ShadowID|Especifica el ID. de sombra de la instantánea que desea exponer.|
|\<Unidad: >|Expone la instantánea especificada como una letra de unidad (por ejemplo, P:).|
|\<Compartir>|Expone la instantánea especificada en un recurso compartido (por ejemplo, \\ \\ *MachineName*\).|
|\<> MountPoint|Expone la instantánea especificada en un punto de montaje (por ejemplo, C:\shadowcopy\).|

## <a name="remarks"></a>Observaciones

-   Puede usar un alias existente o una variable de entorno en lugar de *ShadowID*. Use **Agregar** sin parámetros para ver los alias existentes.

## <a name="examples"></a>Ejemplos

Para exponer la instantánea persistente asociada a la variable de entorno VSS_SHADOW_1 como unidad X, escriba:
```
expose %vss_shadow_1% x:
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)