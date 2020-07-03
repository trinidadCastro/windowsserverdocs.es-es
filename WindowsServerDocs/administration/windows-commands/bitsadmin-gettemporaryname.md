---
title: bitsadmin gettemporaryname
description: Artículo de referencia del comando bitsadmin gettemporaryname, que notifica el nombre de archivo temporal del archivo especificado en el trabajo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 68925edc-a801-4292-a812-7471c4f60fdd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0b5dab756856f0c0905d7e3b523a2ec4f3d7cad6
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926654"
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
