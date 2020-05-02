---
title: bitsadmin info
description: Tema de referencia del comando bitsadmin info, que muestra información de resumen sobre el trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5c306677-0d64-41c0-8276-5bba7750cecb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 904f70c82ab4bcc4fb25f759898674cc719b1954
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717440"
---
# <a name="bitsadmin-info"></a>bitsadmin info

Muestra información de resumen sobre el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /info <job> [/verbose]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| /verbose | Opcional. Proporciona información detallada acerca de cada trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar información sobre el trabajo denominado *myDownloadJob*:

```
bitsadmin /info myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin info](bitsadmin-info.md)
