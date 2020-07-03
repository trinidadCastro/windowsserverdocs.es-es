---
title: bitsadmin getcustomheaders
description: Artículo de referencia para el comando bitsadmin getcustomheaders, que recupera los encabezados HTTP personalizados del trabajo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1f0d38d3-e865-4474-81e8-773d65c3d1cc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7482c3eb4b259051ebd63677c70dbaabfb013314
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923078"
---
# <a name="bitsadmin-getcustomheaders"></a>bitsadmin getcustomheaders

Recupera los encabezados HTTP personalizados del trabajo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getcustomheaders <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para obtener los encabezados personalizados para el trabajo denominado *myDownloadJob*:

```
bitsadmin /getcustomheaders myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
