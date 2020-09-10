---
title: bitsadmin getclientcertificate
description: Artículo de referencia para el comando bitsadmin getclientcertificate, que recupera el certificado de cliente del trabajo.
ms.topic: reference
ms.assetid: 4fc8f408-085e-43a0-9fa8-3d798ef107b1
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 71f1ed8920ba2bd3aa0a0f48683d8841e08afb6d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632259"
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
