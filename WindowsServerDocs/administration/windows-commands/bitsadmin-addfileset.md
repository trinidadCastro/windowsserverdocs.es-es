---
title: bitsadmin addfileset
description: Artículo de referencia para el comando bitsadmin addfileset, que agrega uno o varios archivos al trabajo especificado.
ms.topic: reference
ms.assetid: 75466994-262f-4724-b14d-f813c5397675
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2e3e736fe6dcc96b7f81b3b249257f0d23dd78c9
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632713"
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
