---
title: bitsadmin setmaxdownloadtime
description: Tema de comandos de Windows para **bitsadmin setmaxdownloadtime**, que establece el tiempo de espera de descarga en segundos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 16b96cf1-5738-415c-9b9d-c4ea8d5e4fec
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f07931dfb9fabaec272384dced6d60f1335b6a94
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122913"
---
# <a name="bitsadmin-setmaxdownloadtime"></a>bitsadmin setmaxdownloadtime

Establece el tiempo de espera de descarga en segundos.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /setmaxdownloadtime <job> <timeout>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| timeout | La longitud del tiempo de espera de descarga, en segundos. |

## <a name="examples"></a>Ejemplos

En el ejemplo siguiente se establece el tiempo de espera para el trabajo denominado *myDownloadJob* en 10 segundos.

```
C:\>bitsadmin /setmaxdownloadtime myDownloadJob 10
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)