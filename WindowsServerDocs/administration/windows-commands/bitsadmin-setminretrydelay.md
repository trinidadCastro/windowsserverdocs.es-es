---
title: bitsadmin setminretrydelay
description: Windows Commands topic for **bitsadmin setminretrydelay**, que establece el tiempo mínimo, en segundos, que bits espera después de encontrar un error transitorio antes de intentar transferir el archivo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ce8674ca-6cc5-4bb2-8dda-7dfbb1cd6830
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ddae9a62a49ca07bb03649f131a0a1ebad8ee3fe
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122871"
---
# <a name="bitsadmin-setminretrydelay"></a>bitsadmin setminretrydelay

Establece la cantidad mínima de tiempo, en segundos, que BITS espera después de encontrar un error transitorio antes de intentar transferir el archivo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /setminretrydelay <job> <retrydelay>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| retrydelay | Cantidad mínima de tiempo para que los BITS esperen después de un error durante la transferencia, en segundos. |

## <a name="examples"></a>Ejemplos

En el ejemplo siguiente se establece el intervalo mínimo de reintentos para el trabajo denominado *myDownloadJob* en 35 segundos.

```
C:\>bitsadmin /setminretrydelay myDownloadJob 35
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)