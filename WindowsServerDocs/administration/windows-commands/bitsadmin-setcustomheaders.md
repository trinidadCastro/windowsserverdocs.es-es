---
title: bitsadmin setcustomheaders
description: Windows Commands topic for **bitsadmin setcustomheaders**, que agrega un encabezado HTTP personalizado a una solicitud GET.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ed926410-80d0-46ed-9a90-f752c164bb9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b5b1a28f03815a22a3f8d10b2c3d1d4a3a2ae635
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123026"
---
# <a name="bitsadmin-setcustomheaders"></a>bitsadmin setcustomheaders

Agregue un encabezado HTTP personalizado a una solicitud GET enviada a un servidor HTTP.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /setcustomheaders <job> <header1> <header2> <...>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| `<header1> <header2>` etc. | Los encabezados personalizados para el trabajo. |

## <a name="examples"></a>Ejemplos

En el ejemplo siguiente se agrega un encabezado HTTP personalizado para el trabajo denominado *myDownloadJob*. Para obtener más información sobre las solicitudes GET, vea [definiciones de métodos](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html#sec9.3) y definiciones de [campos de encabezado](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).

```
C:\>bitsadmin /setcustomheaders myDownloadJob accept-encoding:deflate/gzip
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)