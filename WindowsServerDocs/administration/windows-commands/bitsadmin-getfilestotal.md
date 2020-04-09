---
title: bitsadmin getfilestotal
description: Tema de comandos de Windows para **bitsadmin getfilestotal**, que recupera el número de archivos del trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c5de113e-f29c-4cd3-9392-0e300018d516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bad42a8bef57ca4c4a1411a12f20979e4a95d178
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850688"
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

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se recupera el número de archivos incluidos en el trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /getfilestotal myDownloadJob
```

## <a name="see-also"></a>Consulta también

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
