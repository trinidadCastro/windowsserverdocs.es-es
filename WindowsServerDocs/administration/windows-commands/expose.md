---
title: aprovecha
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 819484364e8375c4d58e4d022681eedeaa7084ab
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377282"
---
# <a name="expose"></a>aprovecha



Expone una instantánea persistente como letra de unidad, recurso compartido o punto de montaje.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
expose <ShadowID> {<Drive:> | <Share> | <MountPoint>}
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|ShadowID|Especifica el ID. de sombra de la instantánea que desea exponer.|
|\<Drive: >|Expone la instantánea especificada como una letra de unidad (por ejemplo, P:).|
|@no__t 0Share >|Expone la instantánea especificada en un recurso compartido (por ejemplo, \\ @ no__t-1*MachineName*\).|
|@no__t 0MountPoint >|Expone la instantánea especificada en un punto de montaje (por ejemplo, C:\shadowcopy @ no__t-0.|

## <a name="remarks"></a>Comentarios

-   Puede usar un alias existente o una variable de entorno en lugar de *ShadowID*. Use **Agregar** sin parámetros para ver los alias existentes.

## <a name="BKMK_examples"></a>Example

Para exponer la instantánea persistente asociada a la variable de entorno VSS_SHADOW_1 como unidad X, escriba:
```
expose %vss_shadow_1% x:
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)