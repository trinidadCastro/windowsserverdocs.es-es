---
title: bitsadmin getfilestransferred
description: Tema de referencia del comando bitsadmin getfilestransferred, que recupera el número de archivos transferidos para el trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e282815c-938b-4ac0-a09d-9baafb656dcb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ed11739029338ecce5fc4efbe1918873a7f37f62
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717912"
---
# <a name="bitsadmin-getfilestransferred"></a>bitsadmin getfilestransferred

Recupera el número de archivos transferidos para el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getfilestransferred <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar el número de archivos transferidos en el trabajo denominado *myDownloadJob*:

```
bitsadmin /getfilestransferred myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
