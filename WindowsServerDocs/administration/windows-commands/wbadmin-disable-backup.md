---
title: wbadmin disable backup
description: Artículo de referencia para el comando Wbadmin deshabilitar copia de seguridad, que detiene la ejecución de las copias de seguridad diarias programadas existentes.
ms.topic: reference
ms.assetid: 5176cbd9-0696-4b3f-9c35-272dd84f7898
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: f490ede3b6f48285291b1658458d8c8c176bb2c8
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156048"
---
# <a name="wbadmin-disable-backup"></a>wbadmin disable backup

Detiene la ejecución de las copias de seguridad diarias programadas existentes.

Para deshabilitar una copia de seguridad diaria programada con este comando, debe ser miembro del grupo **administradores** o tener delegados los permisos adecuados. Además, debe ejecutar **Wbadmin** desde un símbolo del sistema con privilegios elevados, haciendo clic con el botón secundario en **símbolo del sistema**y, a continuación, seleccionando **Ejecutar como administrador**.

## <a name="syntax"></a>Sintaxis

```
wbadmin disable backup [-quiet]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| -quiet | Ejecuta el comando sin preguntar al usuario. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando Wbadmin](wbadmin.md)

- [comando Wbadmin para habilitar copia de seguridad](wbadmin-enable-backup.md)