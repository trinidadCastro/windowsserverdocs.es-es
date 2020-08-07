---
title: bitsadmin getclientcertificate
description: Artículo de referencia para el comando bitsadmin getclientcertificate, que recupera el certificado de cliente del trabajo.
ms.topic: article
ms.assetid: 4fc8f408-085e-43a0-9fa8-3d798ef107b1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a3deb42c0b29f238bfd0a3b0fddbea14833d38b4
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894505"
---
# <a name="bitsadmin-getclientcertificate"></a>bitsadmin getclientcertificate

Recupera el certificado de cliente del trabajo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getclientcertificate <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar el certificado de cliente para el trabajo denominado *myDownloadJob*:

```
bitsadmin /getclientcertificate myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
