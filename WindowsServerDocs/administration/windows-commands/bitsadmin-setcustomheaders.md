---
title: bitsadmin setcustomheaders
description: Artículo de referencia para el comando bitsadmin setcustomheaders, que agrega un encabezado HTTP personalizado a una solicitud GET.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ed926410-80d0-46ed-9a90-f752c164bb9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 74a8c5f4aa464aa9f362a9761c9f1c449fbaad00
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927842"
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
