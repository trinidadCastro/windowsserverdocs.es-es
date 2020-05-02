---
title: bitsadmin gethttpmethod
description: Tema de referencia del comando bitsadmin gethttpmethod, que obtiene el verbo HTTP que se va a usar con el trabajo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: a458322a5ace69df74df054a537a7365da9e7329
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717883"
---
# <a name="bitsadmin-gethttpmethod"></a>bitsadmin gethttpmethod

Obtiene el verbo HTTP que se va a usar con el trabajo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /gethttpmethod <Job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar el verbo HTTP que se va a usar con el trabajo denominado *myDownloadJob*:

```
bitsadmin /gethttpmethod myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
