---
title: ksetup dumpstate
description: Tema de referencia de ksetup dumpstate comandos, que muestra el estado actual de la configuración de dominio Kerberos para todos los territorios definidos en el equipo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3ef2f7b8-97af-4f42-9542-cff324840637
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4ccb75ac143239d97b823fb7030f9a8020b4b4f6
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817745"
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