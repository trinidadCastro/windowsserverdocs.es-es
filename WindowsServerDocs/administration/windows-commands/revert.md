---
title: revertir
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 75ad40e4-502a-401e-b11e-8b31e00424b5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b967904c28661be82909ebcc0aa7cb42c73e418c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722332"
---
# <a name="revert"></a>revertir



Revierte un volumen a una instantánea especificada. Esto solo se admite para las instantáneas en el contexto CLIENTACCESSIBLE. Estas instantáneas son persistentes y solo las puede realizar el proveedor del sistema. Si se usa sin parámetros, **Revert** muestra la ayuda en el símbolo del sistema.

## <a name="syntax"></a>Sintaxis

```
revert <ShadowCopyID>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<> ShadowCopyID|Especifica el identificador de la instantánea a la que se revierte el volumen.|

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)