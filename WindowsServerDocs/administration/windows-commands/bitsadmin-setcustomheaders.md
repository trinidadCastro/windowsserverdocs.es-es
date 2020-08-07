---
title: bitsadmin setcustomheaders
description: Artículo de referencia para el comando bitsadmin setcustomheaders, que agrega un encabezado HTTP personalizado a una solicitud GET.
ms.topic: article
ms.assetid: ed926410-80d0-46ed-9a90-f752c164bb9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 407e4aa85d8413167add716cd63fe620f73b0e12
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893189"
---
# <a name="bitsadmin-setcustomheaders"></a>bitsadmin setcustomheaders

Agregue un encabezado HTTP personalizado a una solicitud GET enviada a un servidor HTTP. Para obtener más información sobre las solicitudes GET, vea [definiciones de métodos](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html#sec9.3) y definiciones de [campos de encabezado](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).

## <a name="syntax"></a>Sintaxis

```
bitsadmin /setcustomheaders <job> <header1> <header2> <...>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| `<header1> <header2>`etc. | Los encabezados personalizados para el trabajo. |

## <a name="examples"></a>Ejemplos

Para agregar un encabezado HTTP personalizado para el trabajo denominado *myDownloadJob*:

```
bitsadmin /setcustomheaders myDownloadJob accept-encoding:deflate/gzip
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
