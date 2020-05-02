---
title: bitsadmin getbytestotal
description: Tema de referencia del comando bitsadmin getbytestotal, que recupera el tamaño del trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 784e0bfa-7b09-4262-9104-adbc9beb479b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f844e1d3689c42a2c533921797d15dbb946b551e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718161"
---
# <a name="bitsadmin-getbytestotal"></a>bitsadmin getbytestotal

Recupera el tamaño del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getbytestotal <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar el tamaño del trabajo denominado *myDownloadJob*:

```
bitsadmin /getbytestotal myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
