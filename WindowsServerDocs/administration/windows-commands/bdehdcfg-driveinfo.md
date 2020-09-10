---
title: bdehdcfg driveinfo
description: Artículo de referencia para el comando bdehdcfg DriveInfo, que muestra la letra de unidad, el tamaño total, el espacio libre máximo y las características de la partición.
ms.topic: reference
ms.assetid: f2d065e7-eced-4509-a1a0-ee2521a7f02e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: fb474e40e92979f5f2cf73d90a553bbf785c0312
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632968"
---
# <a name="bdehdcfg-driveinfo"></a>bdehdcfg: DriveInfo

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra la letra de unidad, el tamaño total, el espacio libre máximo y las características de la partición. Solo se enumeran las particiones válidas. El espacio sin asignar no se enumera si ya existen cuatro particiones principales o extendidas.

>[!NOTE]
> Este comando es meramente informativo y no realiza ningún cambio en la unidad.

## <a name="syntax"></a>Sintaxis

```
bdehdcfg -driveinfo <drive_letter>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| <drive_letter> | Especifica una letra de unidad seguida de dos puntos. |

## <a name="example"></a>Ejemplo

Para mostrar la información de la unidad C:

```
bdehdcfg  driveinfo C:
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
