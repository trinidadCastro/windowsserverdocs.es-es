---
title: bitsadmin setpriority
description: Tema de comandos de Windows para **bitsadmin SetPriority**, que establece la prioridad del trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 90788363-01a2-4d7c-a560-a3eba45b5e9e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9348680a61649b938267b3277de9aa5aa521361f
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122767"
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

En el ejemplo siguiente se establece la prioridad del trabajo denominado *myDownloadJob* en normal.

```
C:\>bitsadmin /setpriority myDownloadJob NORMAL
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)