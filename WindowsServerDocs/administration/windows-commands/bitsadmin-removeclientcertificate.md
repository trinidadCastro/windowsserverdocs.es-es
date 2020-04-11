---
title: bitsadmin removeclientcertificate
description: Windows Commands topic for **bitsadmin removeclientcertificate**, que quita el certificado de cliente del trabajo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b417c3e5-aadd-4fcc-968f-45d8b67ca516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 312226b73b91385436e15c4afbb49df161258768
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123108"
---
# <a name="bitsadmin-removeclientcertificate"></a>bitsadmin removeclientcertificate

Quita el certificado de cliente del trabajo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /removeclientcertificate <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

En el ejemplo siguiente se quita el certificado de cliente del trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /removeclientcertificate myDownloadJob 
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)