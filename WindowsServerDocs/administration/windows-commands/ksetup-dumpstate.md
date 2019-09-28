---
title: 'ksetup: dumpstate'
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3ef2f7b8-97af-4f42-9542-cff324840637
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 625d05b2fea9ae58681648c64e309aa8b2a201ed
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375002"
---
# <a name="ksetupdumpstate"></a>ksetup: dumpstate



Muestra el estado actual de la configuración de dominio Kerberos para todos los territorios definidos en el equipo. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /dumpstate
```

### <a name="parameters"></a>Parámetros

Ninguno

## <a name="remarks"></a>Comentarios

La salida de este comando incluye el dominio Kerberos predeterminado (el dominio del que el equipo es miembro) y todos los territorios definidos en este equipo. Se incluye lo siguiente para cada dominio Kerberos:
-   Todos los centros de distribución de claves (KDC) asociados a este dominio Kerberos
-   Todas las marcas de **dominio Kerberos establecidas** para este dominio Kerberos
-   La contraseña del KDC

Este comando no muestra el nombre de dominio especificado por la detección de DNS o por el comando **ksetup/Domain**.

Este comando no muestra la contraseña del equipo establecida mediante el comando **ksetup/setcomputerpassword**.

**Ksetup** produce la misma salida que **ksetup/dumpstate**.

## <a name="BKMK_Examples"></a>Example

Busque la mayoría de las configuraciones de dominio Kerberos en un equipo:
```
ksetup /dumpstate
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Ksetup](ksetup.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)