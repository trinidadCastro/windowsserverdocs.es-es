---
title: bitsadmin util y enableanalyticchannel
description: Tema de referencia del comando bitsadmin util y enableanalyticchannel, que habilita o deshabilita el canal de análisis de cliente de BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0d7645aa-b91b-4ed7-b630-a1e1be6f6ae9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c1f5c8c924d1011928aca6ec1bcebd4d71abb015
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82707761"
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

Para activar o desactivar el canal de análisis de cliente de BITS.

```
bitsadmin /util / enableanalyticchannel TRUE
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin util (comando)](bitsadmin-util.md)

- [bitsadmin (comando)](bitsadmin.md)
