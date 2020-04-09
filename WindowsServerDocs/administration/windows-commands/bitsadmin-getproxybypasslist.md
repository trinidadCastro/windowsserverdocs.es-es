---
title: bitsadmin getproxybypasslist
description: Tema de comandos de Windows para **bitsadmin getproxybypasslist**, que recupera la lista de omisión de proxy para el trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 50959be3-7014-4bc9-9a7b-68f1ff94a94a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9cd81aaef22c4173f198b765246b78b3d3bae136
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850538"
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

## <a name="remarks"></a>Comentarios

La lista de omisión contiene los nombres de host o direcciones IP, o ambos, que no se enrutarán a través de un proxy. La lista puede contener `<local>` para hacer referencia a todos los servidores de la misma LAN. La lista puede ser de punto y coma (;) o delimitado por espacios.

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se recupera la lista de omisión de proxy para el trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /getproxybypasslist myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)