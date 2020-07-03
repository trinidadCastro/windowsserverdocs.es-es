---
title: bitsadmin setpriority
description: Artículo de referencia del comando bitsadmin SetPriority, que establece la prioridad del trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 90788363-01a2-4d7c-a560-a3eba45b5e9e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 07afd9c636a5dbcd4e70de71b3a6f515e7e02bae
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927595"
---
# <a name="bitsadmin-setpriority"></a>bitsadmin setpriority

Establece la prioridad del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /setpriority <job> <priority>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| priority | Establece la prioridad del trabajo, incluidos:<ul><li>FOREGROUND</li><li>HIGH</li><li>NORMAL</li><li>LOW</li></ul> |

## <a name="examples"></a>Ejemplos

Para establecer la prioridad del trabajo denominado *myDownloadJob* en normal:

```
bitsadmin /setpriority myDownloadJob NORMAL
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
