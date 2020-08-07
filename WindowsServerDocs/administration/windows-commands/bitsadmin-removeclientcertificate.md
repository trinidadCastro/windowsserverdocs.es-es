---
title: bitsadmin removeclientcertificate
description: Artículo de referencia para el comando bitsadmin removeclientcertificate, que quita el certificado de cliente del trabajo.
ms.topic: article
ms.assetid: b417c3e5-aadd-4fcc-968f-45d8b67ca516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f4b2a17a08f3c2b224d90237975841644ea16ecd
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893379"
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

Para quitar el certificado de cliente del trabajo denominado *myDownloadJob*:

```
bitsadmin /removeclientcertificate myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
