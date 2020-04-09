---
title: bitsadmin addfileset
description: Windows Commands topic for **bitsadmin addfileset**, que agrega uno o varios archivos al trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 75466994-262f-4724-b14d-f813c5397675
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1c4cff7dc8439fe8e1c54d1f5d231d1b487dc70c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850978"
---
# <a name="bitsadmin-addfileset"></a>bitsadmin addfileset

Agrega uno o más archivos al trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /addfileset <Job> <TextFile>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| Trabajo | El nombre para mostrar o el GUID del trabajo. |
| TextFile | Un archivo de texto, cada línea de que contiene un nombre de archivo remoto y otro local. **Nota:** Los nombres están delimitados por espacios. Las líneas que comienzan por un carácter `#` se tratan como comentario. |

## <a name="examples"></a><a name=BKMK_examples></a>Example

```
C:\>bitsadmin /addfileset files.txt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)