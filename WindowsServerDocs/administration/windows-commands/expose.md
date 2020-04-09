---
title: aprovecha
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9b0a21cf-3bef-4ade-b8f1-ac42f9203947
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 55cc7a292b81977a346f3f078a3b5623243ea46c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844808"
---
# <a name="expose"></a>aprovecha



expone una instantánea persistente como letra de unidad, recurso compartido o punto de montaje.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
expose <ShadowID> {<Drive:> | <Share> | <MountPoint>}
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|ShadowID|Especifica el ID. de sombra de la instantánea que desea exponer.|
|Unidad de \<: >|Expone la instantánea especificada como una letra de unidad (por ejemplo, P:).|
|\<compartir >|Expone la instantánea especificada en un recurso compartido (por ejemplo, \\\\*MachineName*\).|
|\<MountPoint >|Expone la instantánea especificada en un punto de montaje (por ejemplo, C:\shadowcopy\).|

## <a name="remarks"></a>Comentarios

-   Puede usar un alias existente o una variable de entorno en lugar de *ShadowID*. Use **Agregar** sin parámetros para ver los alias existentes.

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para exponer la instantánea persistente asociada a la variable de entorno VSS_SHADOW_1 como unidad X, escriba:
```
expose %vss_shadow_1% x:
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)