---
title: create volume raid
description: Artículo de referencia para el comando CREATE Volume RAID, que crea un volumen RAID-5 con tres o más discos dinámicos especificados.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9f257950-9240-4d5f-9537-8ad653d48ebf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0c3b142e7fd678af04d1bc4e109ac399807da06e
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85929540"
---
# <a name="create-volume-raid"></a>create volume raid

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Crea un volumen RAID-5 con tres o más discos dinámicos especificados. Después de crear el volumen, el foco cambiará automáticamente al nuevo volumen.

## <a name="syntax"></a>Sintaxis

```
create volume raid [size=<n>] disk=<n>,<n>,<n>[,<n>,...] [align=<n>] [noerr]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| tamaño =`<n>` | Cantidad de espacio en disco, en megabytes (MB), que ocupará el volumen en cada disco. Si no se especifica ningún tamaño, se creará el volumen RAID-5 más grande posible. El disco con el menor espacio contiguo libre determina el tamaño del volumen RAID-5, pues se asigna la misma cantidad de espacio de cada disco. La cantidad real de espacio de disco que se puede utilizar en el volumen RAID-5 es inferior a la cantidad conjunta de espacio de disco porque parte del espacio del disco es necesario para la paridad. |
| disco =`<n>,<n>,<n>[,<n>,...]` | Los discos dinámicos en los que se va a crear el volumen RAID-5. Necesitará al menos tres discos dinámicos para crear un volumen RAID-5. En cada disco se asigna una cantidad de espacio igual a `size=<n>` . |
| align =`<n>` | Alinea todas las extensiones de volumen con el límite de alineación más cercano. Normalmente se usa con matrices de número de unidad lógica (LUN) RAID de hardware para mejorar el rendimiento. `<n>`es el número de kilobytes (KB) desde el principio del disco hasta el límite de alineación más cercano. |
| noerr | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

## <a name="examples"></a>Ejemplos

Para crear un volumen RAID-5 de 1000 megabytes de tamaño, con los discos 1, 2 y 3, escriba:

```
create volume raid size=1000 disk=1,2,3
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [crear comando](create.md)
