---
title: create partition extended
description: Artículo de referencia para el comando CREATE Partition Extended, que crea una partición extendida en el disco que tiene el foco.
ms.topic: reference
ms.assetid: 4ad7cb66-9c66-4153-b94e-1030a7225070
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0d60438d634309d93a2d8446e4d86ff909db27e4
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89030223"
---
# <a name="create-partition-extended"></a>create partition extended

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Crea una partición extendida en el disco que tiene el foco. Después de crear la partición, ésta recibe el foco automáticamente.

>[!IMPORTANT]
> Este comando solo se puede usar en discos de registro de arranque maestro (MBR). Debe usar el comando [Seleccionar disco](select-disk.md) para seleccionar un disco MBR básico y cambiarle el foco.
>
> Se debe crear una partición extendida para poder crear unidades lógicas. Sólo es posible crear una partición extendida por disco. Este comando produce un error si se intenta crear una partición extendida dentro de otra partición extendida.

## <a name="syntax"></a>Sintaxis

```
create partition extended [size=<n>] [offset=<n>] [align=<n>] [noerr]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| tamaño =`<n>` | Especifica el tamaño de la partición en megabytes (MB). Si no se proporciona ningún tamaño, la partición continuará hasta que no haya más espacio libre en la partición extendida. |
| desplazamiento =`<n>` | Especifica el desplazamiento en kilobytes (KB), en el que se crea la partición. Si no se proporciona ningún desplazamiento, la partición se iniciará al principio del espacio libre en el disco que sea lo suficientemente grande como para contener la nueva partición. |
| align =`<n>` | Alinea todas las extensiones de partición con el límite de alineación más cercano. Normalmente se usa con matrices de número de unidad lógica (LUN) RAID de hardware para mejorar el rendimiento. `<n>` es el número de kilobytes (KB) desde el principio del disco hasta el límite de alineación más cercano. |
| noerr | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

## <a name="examples"></a>Ejemplos

Para crear una partición extendida de 1000 megabytes de tamaño, escriba:

```
create partition extended size=1000
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [crear comando](create.md)

- [select disk](select-disk.md)
