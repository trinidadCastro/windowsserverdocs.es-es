---
title: Bitsadmin setvalidationstate
description: Tema de los comandos de Windows para **setvalidationstate bitsadmin** -establece el estado de validación del contenido del archivo especificado dentro del trabajo.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e8fc8e8c-171c-4681-8057-6986b018e576
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 647389aaac06d1eb109052548c1b24f7579bde2f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851246"
---
# <a name="bitsadmin-setvalidationstate"></a>Bitsadmin setvalidationstate



Establece el estado de validación del contenido del archivo especificado dentro del trabajo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetValidationState <Job> <file index> <true|false> 
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|Trabajo|Nombre para mostrar o el GUID del trabajo|
|Índice de archivo|Comienza por 0|
|True|False|Establecida en TRUE si el contenido del archivo es válido, en caso contrario, establezca en FALSE|

## <a name="BKMK_examples"></a>Ejemplos

En el ejemplo siguiente se establece el estado de validación del contenido del archivo de 2 en TRUE para el trabajo denominado *myJob*.
```
C:\>bitsadmin /SetValidationState myJob 2 TRUE 
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)