---
title: bitsadmin setproxysettings
description: Windows Commands topic for **bitsadmin setproxysettings**, que establece la configuración de proxy para el trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: be8edb1b-614e-4d0b-a8f8-64b4bde3e64b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ea92383d9bd09372d21d3c1da84db060b0a9958
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122753"
---
# <a name="bitsadmin-setproxysettings"></a>bitsadmin setproxysettings

Establece la configuración de proxy para el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /setproxysettings <job> <usage> [list] [bypass]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| usage | Establece el uso del proxy, incluido:<ul><li>**PRECONFIG.** Use los valores predeterminados de Internet Explorer del propietario.</li><li>**NO_PROXY.** No utilice un servidor proxy.</li><li>**Estima.** Use una lista de proxy explícita y una lista de omisión. La lista de proxy y la información de omisión de proxy deben seguir.</li><li>**Detección automática.** Detecta automáticamente la configuración de proxy.</li></ul> |
| lista | Se utiliza cuando el parámetro *Usage* está establecido en override. Debe contener una lista delimitada por comas de servidores proxy para usar. |
| Alto | Se utiliza cuando el parámetro *Usage* está establecido en override. Debe contener una lista delimitada por espacios de nombres de host o direcciones IP, o ambos, para las que las transferencias no se van a enrutar a través de un proxy. Esto se puede `<local>` para hacer referencia a todos los servidores de la misma LAN. Los valores NULL se pueden usar para una lista de omisión de proxy vacía. |

## <a name="examples"></a>Ejemplos

En el ejemplo siguiente se establece la configuración de proxy para el trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /setproxysettings myDownloadJob PRECONFIG
```

```
bitsadmin /setproxysettings myDownloadJob NO_PROXY
```
```
bitsadmin /setproxysettings myDownloadJob OVERRIDE proxy1:80
```

```
bitsadmin /setproxysettings myDownloadJob OVERRIDE proxy1,proxy2,proxy3 NULL
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)