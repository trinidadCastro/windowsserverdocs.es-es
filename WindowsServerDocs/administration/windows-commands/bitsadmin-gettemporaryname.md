---
title: bitsadmin gettemporaryname
description: Comando comandos de Windows para **bitsadmin gettemporaryname**, que notifica el nombre de archivo temporal del archivo especificado dentro del trabajo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 68925edc-a801-4292-a812-7471c4f60fdd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6c331ecf12cb02d34c76692158c79eafbe5691c5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850458"
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

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se notifica el nombre de archivo temporal del archivo 2 para el trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /gettemporaryname myDownloadJob 1
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)