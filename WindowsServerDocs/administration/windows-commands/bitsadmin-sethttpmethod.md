---
title: bitsadmin sethttpmethod
description: Artículo de referencia para el comando bitsadmin sethttpmethod, que establece el verbo HTTP que se va a usar.
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 03/01/2019
ms.openlocfilehash: 0c8d2357e35831db89365eabbe0b97cdec88b6aa
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630874"
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

- [bitsadmin (comando)](bitsadmin.md)
