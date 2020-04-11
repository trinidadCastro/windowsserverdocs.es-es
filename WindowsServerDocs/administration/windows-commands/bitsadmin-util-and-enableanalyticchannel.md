---
title: bitsadmin util y enableanalyticchannel
description: Tema de comandos de Windows para **bitsadmin util y enableanalyticchannel**, que habilita o deshabilita el canal de análisis de cliente de bits.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0d7645aa-b91b-4ed7-b630-a1e1be6f6ae9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8ff1f835415979036fdc0f8aa637fe693e57d46
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122684"
---
# <a name="bitsadmin-util-and-enableanalyticchannel"></a>bitsadmin util y enableanalyticchannel

Habilita o deshabilita el canal de análisis de cliente de BITS.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /util /enableanalyticchannel TRUE|FALSE
```

| Parámetro | Descripción |
| --------- | ---------- |
| TRUE o FALSE | **True** activa la validación de contenido para el archivo especificado, mientras que **false** lo desactiva. |

## <a name="examples"></a>Ejemplos

En el ejemplo siguiente se habilita el canal de análisis de cliente de BITS.

```
C:\>bitsadmin /util / enableanalyticchannel TRUE
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)