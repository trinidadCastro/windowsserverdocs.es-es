---
title: bitsadmin getdisplayname
description: Artículo de referencia del comando bitsadmin getDisplayName, que recupera el nombre para mostrar del trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e5c0e76c-4cc6-42d8-ac30-30bf3dc11b9b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4b0e768f377b9faa23eb59645cbecdc129f1e573
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923047"
---
# <a name="bitsadmin-getdisplayname"></a>bitsadmin getdisplayname

Recupera el nombre para mostrar del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getdisplayname <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar el nombre para mostrar del trabajo denominado *myDownloadJob*:

```
bitsadmin /getdisplayname myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
