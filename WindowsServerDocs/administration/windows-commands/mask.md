---
title: Máscara
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bf301474-d74a-44e7-9fad-c8a11e7ca3bd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 353e6080d1f6c548bc907b58655f31d0bce6de8b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858026"
---
# <a name="mask"></a>Máscara



Quita las instantáneas de hardware que se importaron mediante el uso de la **importar** comando.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
mask <ShadowSetID>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|ShadowSetID|Quita instantáneas que pertenecen al identificador especificado de conjunto de copia sombra.|

## <a name="remarks"></a>Comentarios

-   Puede usar un alias existente o una variable de entorno en lugar de *ShadowSetID*. Use **agregar** sin parámetros para ver los alias existentes.

## <a name="BKMK_examples"></a>Ejemplos

Para quitar el % de copia sombra importados Import_1, escriba:
```
mask %Import_1%
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)