---
title: Revertir
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 75ad40e4-502a-401e-b11e-8b31e00424b5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3243f13a4997824d9fff7c874ce26d56325fefa4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371448"
---
# <a name="revert"></a>Revertir



Revierte un volumen a una instantánea especificada. Esto solo se admite para las instantáneas en el contexto CLIENTACCESSIBLE. Estas instantáneas son persistentes y solo las puede realizar el proveedor del sistema. Si se usa sin parámetros, **Revert** muestra la ayuda en el símbolo del sistema.

## <a name="syntax"></a>Sintaxis

```
revert <ShadowCopyID>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|@no__t 0ShadowCopyID >|Especifica el identificador de la instantánea a la que se revierte el volumen.|

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)