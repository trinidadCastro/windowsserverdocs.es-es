---
title: bitsadmin gettemporaryname
description: Artículo de referencia del comando bitsadmin gettemporaryname, que notifica el nombre de archivo temporal del archivo especificado en el trabajo.
ms.topic: reference
ms.assetid: 68925edc-a801-4292-a812-7471c4f60fdd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d9e03b00fa1885b89f25dab2781ce988aa0c5df8
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89034813"
---
# <a name="bitsadmin-gettemporaryname"></a>bitsadmin gettemporaryname

Notifica el nombre de archivo temporal del archivo especificado en el trabajo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /gettemporaryname <job> <file_index>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| file_index | Comienza en 0. |

## <a name="examples"></a>Ejemplos

Para notificar el nombre de archivo temporal del archivo 2 para el trabajo denominado *myDownloadJob*:

```
bitsadmin /gettemporaryname myDownloadJob 1
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
