---
title: create volume mirror
description: Artículo de referencia para el comando crear reflejo de volumen, que crea un reflejo de volumen mediante los dos discos dinámicos especificados.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 48776917-783a-47ff-8da4-1cab77cea34b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 91f718aab181db7d3cbeb0e4255a43f2924ecd33
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85929557"
---
# <a name="create-volume-mirror"></a>create volume mirror

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Crea un reflejo de volumen mediante el uso de los dos discos dinámicos especificados. Una vez creado el volumen, el foco se desplaza automáticamente al nuevo volumen.

## <a name="syntax"></a>Sintaxis

```
create volume mirror [size=<n>] disk=<n>,<n>[,<n>,...] [align=<n>] [noerr]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| tamaño =`<n>` | Especifica la cantidad de espacio en disco, en megabytes (MB), que ocupará el volumen en cada disco. Si no se especifica tamaño, el nuevo volumen ocupará todo el espacio que quede libre en el disco más pequeño y cantidades equivalentes de espacio en los discos sucesivos. |
| disco = `<n>` , `<n>` [ `,<n>,...` ] | Especifica los discos dinámicos en los que se crea el volumen reflejado. Necesita dos discos dinámicos para crear un volumen reflejado. En cada disco se asigna una cantidad de espacio igual al tamaño especificado con el parámetro de **tamaño** . |
| align =`<n>` | Alinea todas las extensiones de volumen con el límite de alineación más cercano. Este parámetro se utiliza normalmente con matrices de número de unidad lógica (LUN) RAID de hardware para mejorar el rendimiento. `<n>`es el número de kilobytes (KB) desde el principio del disco hasta el límite de alineación más cercano. |
| noerr | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart se cierre con un error. |

## <a name="examples"></a>Ejemplos

Para crear un volumen reflejado de 1000 megabytes de tamaño, en los discos 1 y 2, escriba:

```
create volume mirror size=1000 disk=1,2
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [crear comando](create.md)
