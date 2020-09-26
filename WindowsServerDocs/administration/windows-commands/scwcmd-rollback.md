---
title: scwcmd rollback
description: Artículo de referencia del comando scwcmd rollback, que aplica la Directiva de reversión más reciente disponible y, a continuación, se elimina esa Directiva de reversión.
ms.topic: reference
ms.assetid: 4fd9f89b-0420-420a-ad20-4a328636b1e7
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e8f50995131ca01ea2bb58ee9371b12d9ec9a917
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388915"
---
# <a name="scwcmd-rollback"></a>scwcmd rollback

> Se aplica a: Windows Server 2012 R2 y Windows Server 2012

Aplica la Directiva de reversión más reciente disponible y, a continuación, elimina esa Directiva de reversión.

## <a name="syntax"></a>Sintaxis

```
scwcmd rollback /m:<computername> [/u:<username>] [/pw:<password>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /m`<computername>` | Especifica el nombre NetBIOS, el nombre DNS o la dirección IP de un equipo en el que se debe realizar la operación de reversión. |
| /u`<username>` | Especifica la cuenta de usuario alternativa que se va a usar al realizar una reversión remota. El valor predeterminado es el usuario que ha iniciado sesión. |
| /PW`<password>` | Especifica una credencial de usuario alternativa para usar al realizar una reversión remota. El valor predeterminado es el usuario que ha iniciado sesión. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para revertir la Directiva de seguridad en un equipo con la dirección IP *172.16.0.0*, escriba:

```
scwcmd rollback /m:172.16.0.0
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando scwcmd ANALYZE](scwcmd-analyze.md)

- [comando scwcmd configure](scwcmd-configure.md)

- [comando scwcmd Register](scwcmd-register.md)

- [scwcmd transform (comando)](scwcmd-transform.md)

- [comando scwcmd View](scwcmd-view.md)