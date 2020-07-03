---
title: bitsadmin getproxybypasslist
description: Artículo de referencia para el comando bitsadmin getproxybypasslist, que recupera la lista de omisión de proxy para el trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 50959be3-7014-4bc9-9a7b-68f1ff94a94a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 67e6e4a54bb8c4929acbbaa47c78e33bab95a283
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926838"
---
# <a name="bitsadmin-getproxybypasslist"></a>bitsadmin getproxybypasslist

Recupera la lista de omisión de proxy para el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getproxybypasslist <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

### <a name="remarks"></a>Comentarios

La lista de omisión contiene los nombres de host o direcciones IP, o ambos, que no se enrutarán a través de un proxy. La lista puede contener `<local>` para hacer referencia a todos los servidores de la misma LAN. La lista puede ser de punto y coma (;) o delimitado por espacios.

## <a name="examples"></a>Ejemplos

Para recuperar la lista de omisión de proxy para el trabajo denominado *myDownloadJob*:

```
bitsadmin /getproxybypasslist myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
