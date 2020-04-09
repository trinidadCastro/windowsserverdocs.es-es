---
title: bitsadmin getvalidationstate
description: Tema de comandos de Windows para **bitsadmin getvalidationstate**, que notifica el estado de validación del contenido del archivo especificado en el trabajo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6ada3f1f-9967-4262-9d22-ed641e23f516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 52d7d983cc7858607c350483ed81223d107cee25
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850438"
---
# <a name="bitsadmin-getvalidationstate"></a>bitsadmin getvalidationstate

Notifica el estado de validación del contenido del archivo especificado en el trabajo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getvalidationstate <job> <file_index>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| file_index | Comienza en 0. |

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se obtiene el estado de validación del contenido del archivo 2 en el trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /getvalidationstate myDownloadJob 1
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)