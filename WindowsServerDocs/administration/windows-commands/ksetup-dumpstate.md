---
title: ksetup dumpstate
description: Artículo de referencia de ksetup dumpstate comandos, que muestra el estado actual de la configuración de dominio Kerberos para todos los territorios definidos en el equipo.
ms.topic: reference
ms.assetid: 3ef2f7b8-97af-4f42-9542-cff324840637
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 5f9d16a094dd3d3bbcdd5e9cb740e1bca5203a6d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639665"
---
# <a name="ksetup-dumpstate"></a>ksetup dumpstate

Muestra el estado actual de la configuración de dominio Kerberos para todos los territorios definidos en el equipo. Este comando muestra el mismo resultado que el comando **ksetup** .

## <a name="syntax"></a>Sintaxis

```
ksetup /dumpstate
```

### <a name="remarks"></a>Observaciones

- La salida de este comando incluye el dominio Kerberos predeterminado (el dominio del que el equipo es miembro) y todos los territorios definidos en este equipo. Se incluye lo siguiente para cada dominio Kerberos:

  - Todos los centros de distribución de claves (KDC) asociados a este dominio Kerberos.

  - Todas las marcas de **dominio Kerberos establecidas** para este dominio Kerberos.

  - Contraseña del KDC.

- Este comando no muestra el nombre de dominio especificado por la detección de DNS o por el comando `ksetup /domain` .

- Este comando no muestra el conjunto de contraseñas del equipo mediante el comando `ksetup /setcomputerpassword` .

## <a name="examples"></a>Ejemplos

Para buscar las configuraciones de dominio Kerberos en un equipo, escriba:

```
ksetup /dumpstate
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [ksetup, comando](ksetup.md)