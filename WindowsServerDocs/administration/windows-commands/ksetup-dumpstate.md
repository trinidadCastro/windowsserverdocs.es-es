---
title: ksetup dumpstate
description: Artículo de referencia de ksetup dumpstate comandos, que muestra el estado actual de la configuración de dominio Kerberos para todos los territorios definidos en el equipo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3ef2f7b8-97af-4f42-9542-cff324840637
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 86e3761af14da9e1b8f52f4ce6859128fcda7bb7
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85929169"
---
# <a name="ksetup-dumpstate"></a>ksetup dumpstate

Muestra el estado actual de la configuración de dominio Kerberos para todos los territorios definidos en el equipo. Este comando muestra el mismo resultado que el comando **ksetup** .

## <a name="syntax"></a>Sintaxis

```
ksetup /dumpstate
```

### <a name="remarks"></a>Comentarios

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