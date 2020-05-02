---
title: 'ksetup: dumpstate'
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3ef2f7b8-97af-4f42-9542-cff324840637
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 27a7e3154b9dfa663b88b04857ea7650995613c6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724641"
---
# <a name="ksetupdumpstate"></a>ksetup: dumpstate



Muestra el estado actual de la configuración de dominio Kerberos para todos los territorios definidos en el equipo.

## <a name="syntax"></a>Sintaxis

```
ksetup /dumpstate
```

#### <a name="parameters"></a>Parámetros

None

## <a name="remarks"></a>Observaciones

La salida de este comando incluye el dominio Kerberos predeterminado (el dominio del que el equipo es miembro) y todos los territorios definidos en este equipo. Se incluye lo siguiente para cada dominio Kerberos:
-   Todos los centros de distribución de claves (KDC) asociados a este dominio Kerberos
-   Todas las marcas de **dominio Kerberos establecidas** para este dominio Kerberos
-   La contraseña del KDC

Este comando no muestra el nombre de dominio especificado por la detección de DNS o por el comando **ksetup/Domain**.

Este comando no muestra la contraseña del equipo establecida mediante el comando **ksetup/setcomputerpassword**.

**Ksetup** produce la misma salida que **ksetup/dumpstate**.

## <a name="examples"></a>Ejemplos

Busque la mayoría de las configuraciones de dominio Kerberos en un equipo:
```
ksetup /dumpstate
```

## <a name="additional-references"></a>Referencias adicionales

-   [Ksetup](ksetup.md)
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)