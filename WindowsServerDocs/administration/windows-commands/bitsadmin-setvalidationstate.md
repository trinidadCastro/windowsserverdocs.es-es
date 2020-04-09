---
title: bitsadmin setvalidationstate
description: Tema de comandos de Windows para bitsadmin setvalidationstate, que establece el estado de validación de contenido del archivo especificado en el trabajo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e8fc8e8c-171c-4681-8057-6986b018e576
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: de6480596b55b3a483076297f32ce52a975915db
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849118"
---
# <a name="bitsadmin-setvalidationstate"></a>bitsadmin setvalidationstate

Establece el estado de validación del contenido del archivo especificado en el trabajo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /SetValidationState <Job> <file index> <true|false> 
```

### <a name="parameters"></a>Parámetros

| Parámetro  |          Descripción           |
|------------|--------------------------------|
|    Trabajo     | El nombre para mostrar del trabajo o el GUID |
| Índice de archivo |         Comienza en 0          |
|    True    |             False              |

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se establece el estado de validación de contenido del archivo 2 en TRUE para el trabajo denominado *myJob*.
```
C:\>bitsadmin /SetValidationState myJob 2 TRUE 
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)