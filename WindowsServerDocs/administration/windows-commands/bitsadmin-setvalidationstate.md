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
ms.openlocfilehash: a832e8f3d21681f67a4486df33c387e5a8456718
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434873"
---
# <a name="bitsadmin-setvalidationstate"></a>Bitsadmin setvalidationstate



Establece el estado de validación del contenido del archivo especificado dentro del trabajo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetValidationState <Job> <file index> <true|false> 
```

## <a name="parameters"></a>Parámetros

| Parámetro  |          Descripción           |
|------------|--------------------------------|
|    Trabajo     | Nombre para mostrar o el GUID del trabajo |
| Índice de archivo |         Comienza por 0          |
|    True    |             False              |

## <a name="BKMK_examples"></a>Ejemplos

En el ejemplo siguiente se establece el estado de validación del contenido del archivo de 2 en TRUE para el trabajo denominado *myJob*.
```
C:\>bitsadmin /SetValidationState myJob 2 TRUE 
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)