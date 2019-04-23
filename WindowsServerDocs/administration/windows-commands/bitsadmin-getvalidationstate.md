---
title: Bitsadmin getvalidationstate
description: 'Tema de los comandos de Windows para **getvalidationstate bitsadmin** -notifica el estado de validación del contenido del archivo especificado dentro del trabajo. '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6ada3f1f-9967-4262-9d22-ed641e23f516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8abff3fc9fddb9cff1758739fdc540a9c945efe2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879166"
---
# <a name="bitsadmin-getvalidationstate"></a>Bitsadmin getvalidationstate



Notifica el estado de validación del contenido del archivo especificado dentro del trabajo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /GetValidationState <Job> <file index> 
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|
|Índice de archivo|Comienza por 0|

## <a name="BKMK_examples"></a>Ejemplos

En el ejemplo siguiente se obtiene el estado de validación del contenido del archivo 2 dentro del trabajo denominado *myJob*.
```
C:\>bitsadmin /GetValidationState myJob 1
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)