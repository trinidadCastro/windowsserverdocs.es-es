---
title: bitsadmin sethttpmethod
description: Tema de comandos de Windows para **bitsadmin sethttpmethod**, que establece el verbo http que se va a usar.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: d349dcad7bdf6a6fc566ed961c3160836d7f49da
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122967"
---
# <a name="bitsadmin-sethttpmethod"></a>bitsadmin sethttpmethod

Establece el verbo HTTP que se va a usar.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /sethttpmethod <job> <httpmethod>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| httpmethod | Verbo HTTP que se va a usar. Para obtener información sobre los verbos disponibles, vea [definiciones de método](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)