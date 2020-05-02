---
title: bitsadmin addfileset
description: Tema de referencia del comando bitsadmin addfileset, que agrega uno o varios archivos al trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 75466994-262f-4724-b14d-f813c5397675
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d610c1330818cf820923b6d4f2e3555dc477444b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718474"
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
| textfile | Un archivo de texto, cada línea de que contiene un nombre de archivo remoto y otro local. **Nota:** Los nombres deben estar delimitados por espacios. Las líneas que comienzan `#` con un carácter se tratan como comentario. |

## <a name="examples"></a>Ejemplos

```
bitsadmin /addfileset files.txt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
