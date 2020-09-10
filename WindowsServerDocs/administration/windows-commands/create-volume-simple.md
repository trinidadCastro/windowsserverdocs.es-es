---
title: create volume simple
description: Artículo de referencia para el comando CREATE Volume simple, que crea un volumen simple en el disco dinámico especificado.
ms.topic: reference
ms.assetid: da0f208d-7fda-471a-9db2-5de5ba5207c6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 473ee42e996960af3e0f27579283114ca0b3e7f3
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89629063"
---
# <a name="create-volume-simple"></a>create volume simple

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Crea un volumen simple en el disco dinámico especificado. Después de crear el volumen, el foco cambiará automáticamente al nuevo volumen.

## <a name="syntax"></a>Sintaxis

```
create volume simple [size=<n>] [disk=<n>] [align=<n>] [noerr]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| tamaño =`<n>`  | Tamaño del volumen en megabytes (MB). Si no se especifica tamaño, el nuevo volumen ocupará todo el espacio que quede libre en el disco. |
| disco =`<n>`  | El disco dinámico en el que se crea el volumen. Si no se especifica ningún disco, se usa el disco actual. |
| align =`<n>` | Alinea todas las extensiones de volumen con el límite de alineación más cercano. Normalmente se usa con matrices de número de unidad lógica (LUN) RAID de hardware para mejorar el rendimiento. `<n>` es el número de kilobytes (KB) desde el principio del disco hasta el límite de alineación más cercano. |
| noerr | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

## <a name="examples"></a>Ejemplos

Para crear un volumen de 1000 megabytes de tamaño, en el disco 1, escriba:

```
create volume simple size=1000 disk=1
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [crear comando](create.md)
