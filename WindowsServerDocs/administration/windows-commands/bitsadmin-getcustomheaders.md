---
title: bitsadmin getcustomheaders
description: Tema de referencia del comando bitsadmin getcustomheaders, que recupera los encabezados HTTP personalizados del trabajo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1f0d38d3-e865-4474-81e8-773d65c3d1cc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7fe839cd0e629af88b3ee3642abcce339442d03a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718096"
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
