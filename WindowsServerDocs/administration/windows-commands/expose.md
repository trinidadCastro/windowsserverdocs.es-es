---
title: exponer
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9b0a21cf-3bef-4ade-b8f1-ac42f9203947
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 51cc744bc2b61862ed05ca2e7d0aaa8f70d38692
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886666"
---
# <a name="expose"></a>exponer



expone una instantánea persistentes como una letra de unidad, un recurso compartido o un punto de montaje.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
expose <ShadowID> {<Drive:> | <Share> | <MountPoint>}
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|ShadowID|Especifica el identificador de la sombra de la instantánea que desea exponer.|
|\<Drive:>|Expone la instantánea especificada como una letra de unidad (por ejemplo, P:).|
|\<Share>|Expone la instantánea especificada en un recurso compartido (por ejemplo, \\ \\ *MachineName*\).|
|\<MountPoint>|Expone la instantánea especificada a un punto de montaje (por ejemplo, C:\shadowcopy\).|

## <a name="remarks"></a>Comentarios

-   Puede usar un alias existente o una variable de entorno en lugar de *IdDeInstantánea*. Use **agregar** sin parámetros para ver los alias existentes.

## <a name="BKMK_examples"></a>Ejemplos

Para exponer la instantánea persistente asociada a la variable de entorno VSS_SHADOW_1 como unidad X, escriba:
```
expose %vss_shadow_1% x:
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)