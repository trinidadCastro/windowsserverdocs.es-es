---
title: bitsadmin setproxysettings
description: Artículo de referencia para el comando bitsadmin setproxysettings, que establece la configuración de proxy para el trabajo especificado.
ms.topic: reference
ms.assetid: be8edb1b-614e-4d0b-a8f8-64b4bde3e64b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 199cb3f4b4259a52a8960cac23b9e408e71ded23
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630686"
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
| list | Se utiliza cuando el parámetro *Usage* está establecido en override. Debe contener una lista delimitada por comas de servidores proxy para usar. |
| omitir | Se utiliza cuando el parámetro *Usage* está establecido en override. Debe contener una lista delimitada por espacios de nombres de host o direcciones IP, o ambos, para las que las transferencias no se van a enrutar a través de un proxy. Esto puede ser `<local>` hacer referencia a todos los servidores de la misma LAN. Los valores NULL se pueden usar para una lista de omisión de proxy vacía. |

## <a name="examples"></a>Ejemplos

Para establecer la configuración de proxy con las distintas opciones de uso del trabajo denominado *myDownloadJob*:

```
bitsadmin /setproxysettings myDownloadJob PRECONFIG
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

- [bitsadmin (comando)](bitsadmin.md)
