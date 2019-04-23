---
title: ksetup:dumpstate
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 8e5e8f20188fc27cc08dfd37c5fdbd811925f476
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863126"
---
# <a name="ksetupdumpstate"></a>ksetup:dumpstate



Muestra el estado actual de la configuración del dominio para todos los territorios de que se definen en el equipo. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /dumpstate
```

### <a name="parameters"></a>Parámetros

Ninguno

## <a name="remarks"></a>Comentarios

El resultado de este comando incluye el dominio Kerberos predeterminado (el dominio que el equipo es un miembro de) y todos los territorios de la que se definen en este equipo. A continuación se incluyen para cada dominio de Kerberos:
-   Todas la clave centros de distribución (KDC) que están asociados con este dominio Kerberos
-   Todas la **conjunto territorio** marcas para este dominio
-   La contraseña KDC

Este comando no muestra el nombre de dominio que se especifica mediante la detección de DNS o el comando **/Domain ksetup**.

Este comando no muestra la contraseña de equipo que se establece mediante el comando **/setcomputerpassword ksetup**.

**Ksetup** genera el mismo resultado que **/dumpstate ksetup**.

## <a name="BKMK_Examples"></a>Ejemplos

Encontrar la mayoría de las configuraciones de dominio Kerberos de Kerberos en un equipo:
```
ksetup /dumpstate
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Ksetup](ksetup.md)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)