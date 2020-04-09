---
title: 'ksetup: dumpstate'
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3ef2f7b8-97af-4f42-9542-cff324840637
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 46f827d26d867392db4cbef92cf5be496aee8d74
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841518"
---
# <a name="ksetupdumpstate"></a>ksetup: dumpstate



Muestra el estado actual de la configuración de dominio Kerberos para todos los territorios definidos en el equipo. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
ksetup /dumpstate
```

#### <a name="parameters"></a>Parámetros

Ninguno

## <a name="remarks"></a>Comentarios

La salida de este comando incluye el dominio Kerberos predeterminado (el dominio del que el equipo es miembro) y todos los territorios definidos en este equipo. Se incluye lo siguiente para cada dominio Kerberos:
-   Todos los centros de distribución de claves (KDC) asociados a este dominio Kerberos
-   Todas las marcas de **dominio Kerberos establecidas** para este dominio Kerberos
-   La contraseña del KDC

Este comando no muestra el nombre de dominio especificado por la detección de DNS o por el comando **ksetup/Domain**.

Este comando no muestra la contraseña del equipo establecida mediante el comando **ksetup/setcomputerpassword**.

**Ksetup** produce la misma salida que **ksetup/dumpstate**.

## <a name="examples"></a><a name=BKMK_Examples></a>Example

Busque la mayoría de las configuraciones de dominio Kerberos en un equipo:
```
ksetup /dumpstate
```

## <a name="additional-references"></a>Referencias adicionales

-   [Ksetup](ksetup.md)
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)