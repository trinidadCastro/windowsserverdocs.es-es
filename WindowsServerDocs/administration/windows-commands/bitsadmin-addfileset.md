---
title: bitsadmin addfileset
description: Artículo de referencia para el comando bitsadmin addfileset, que agrega uno o varios archivos al trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 75466994-262f-4724-b14d-f813c5397675
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 861186dfc7ba1a230e1df05c98378d27bfff26b1
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927080"
---
# <a name="bitsadmin-addfileset"></a>bitsadmin addfileset

Agrega uno o más archivos al trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /addfileset <job> <textfile>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| textfile | Un archivo de texto, cada línea de que contiene un nombre de archivo remoto y otro local. **Nota:** Los nombres deben estar delimitados por espacios. Las líneas que comienzan con un `#` carácter se tratan como comentario. |

## <a name="examples"></a>Ejemplos

```
bitsadmin /addfileset files.txt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
