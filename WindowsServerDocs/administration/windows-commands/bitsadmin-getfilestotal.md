---
title: bitsadmin getfilestotal
description: Tema de referencia del comando bitsadmin getfilestotal, que recupera el número de archivos del trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c5de113e-f29c-4cd3-9392-0e300018d516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5cf3b33c15bb18c8a141408f82fdd72a6e710817
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717977"
---
# <a name="bitsadmin-getfilestotal"></a>bitsadmin getfilestotal

Recupera el número de archivos del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getfilestotal <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar el número de archivos incluidos en el trabajo denominado *myDownloadJob*:

```
bitsadmin /getfilestotal myDownloadJob
```

## <a name="see-also"></a>Consulte también

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
