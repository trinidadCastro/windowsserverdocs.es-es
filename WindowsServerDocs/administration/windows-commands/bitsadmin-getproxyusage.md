---
title: bitsadmin getproxyusage
description: Artículo de referencia para el comando bitsadmin getproxyusage, que recupera la configuración de uso del proxy para el trabajo especificado.
ms.topic: reference
ms.assetid: f940a70e-3b02-497e-a47f-b37b905c299e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: bdfcfa92a5886857920d56a0028a450ee9a3e521
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631730"
---
# <a name="bitsadmin-getproxyusage"></a>bitsadmin getproxyusage

Recupera la configuración de uso del proxy para el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getproxyusage <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

#### <a name="output"></a>Resultados

Los valores de uso del proxy devueltos pueden ser:

- **Configuración** predeterminada: Use los valores predeterminados de Internet Explorer del propietario.

- **No_Proxy** : no use un servidor proxy.

- **Invalidar** : usa una lista de proxy explícita.

- **Detección** automática: detecta automáticamente la configuración de proxy.

## <a name="examples"></a>Ejemplos

Para recuperar el uso de proxy para el trabajo denominado *myDownloadJob*:

```
bitsadmin /getproxyusage myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
