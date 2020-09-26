---
title: secedit validate
description: Artículo de referencia para el comando secedit Validate, que valida la configuración de seguridad almacenada en una plantilla de seguridad.
ms.topic: reference
ms.assetid: 9fb06354-f55a-4ca4-9fbc-9a872eb9b9cf
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 057573cf5bc28a47f5140fb58e23ee1ed4bbc4c1
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388107"
---
# <a name="secedit-validate"></a>secedit/Validate

Valida la configuración de seguridad almacenada en una plantilla de seguridad (archivo. inf). La validación de plantillas de seguridad puede ayudarle a determinar si una está dañada o configurada de forma inapropiada. No se aplican plantillas de seguridad dañadas o configuradas incorrectamente.

## <a name="syntax"></a>Sintaxis

```
secedit /validate <configuration file name>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<configuration file name>` | Obligatorio. Especifica la ruta de acceso y el nombre de archivo de la plantilla de seguridad que se validará. Este comando no actualiza los archivos de registro. |

## <a name="examples"></a>Ejemplos

Para comprobar que el archivo rollback. inf, *secRBKcontoso. inf*, sigue siendo válido después de la reversión, escriba:

```
secedit /validate secRBKcontoso.inf
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [secedit/ANALYZE](secedit-analyze.md)

- [secedit/configure](secedit-configure.md)

- [secedit/Export](secedit-export.md)

- [secedit/generaterollback](secedit-generaterollback.md)

- [secedit/Import](secedit-import.md)