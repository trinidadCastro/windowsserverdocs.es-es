---
title: bitsadmin takeownership
description: Tema de comandos de Windows para **bitsadmin TakeOwnerShip**, que permite a un usuario con privilegios administrativos tomar posesión del trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ea0ce7cb-440a-498f-a3ef-8368fa43e399
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a04f54747e3e06aa61166c2c9f9cedfdfbc8d42a
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122694"
---
# <a name="bitsadmin-takeownership"></a>bitsadmin takeownership

Permite a un usuario con privilegios administrativos tomar posesión del trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /takeownership <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ---------- |
| Trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

En el ejemplo siguiente se toma la propiedad del trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /takeownership myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)