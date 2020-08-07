---
title: create volume stripe
description: Artículo de referencia para el comando CREATE Volume Stripe, que crea un volumen seccionado con dos o más discos dinámicos especificados.
ms.topic: article
ms.assetid: 20dce735-5f7c-4f83-a580-d087e2913a00
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e9f0133783178002e35b32b665dc64dfb3144a19
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87891616"
---
# <a name="create-volume-stripe"></a>create volume stripe

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Crea un volumen seccionado mediante dos o más discos dinámicos especificados. Después de crear el volumen, el foco cambiará automáticamente al nuevo volumen.

## <a name="syntax"></a>Sintaxis

```
create volume stripe [size=<n>] disk=<n>,<n>[,<n>,...] [align=<n>] [noerr]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- |  -----------|
| tamaño =`<n>` | Cantidad de espacio en disco, en megabytes (MB), que ocupará el volumen en cada disco. Si no se especifica tamaño, el nuevo volumen ocupará todo el espacio que quede libre en el disco más pequeño y cantidades equivalentes de espacio en los discos sucesivos. |
| disco =`<n>,<n>[,<n>,...]` | Los discos dinámicos en los que se crea el volumen seccionado. Necesitará al menos dos discos dinámicos para crear un volumen seccionado. En cada disco se asigna una cantidad de espacio igual a `size=<n>` . |
| align =`<n>` | Alinea todas las extensiones de volumen con el límite de alineación más cercano. Normalmente se usa con matrices de número de unidad lógica (LUN) RAID de hardware para mejorar el rendimiento. `<n>`es el número de kilobytes (KB) desde el principio del disco hasta el límite de alineación más cercano. |
| noerr | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

## <a name="examples"></a>Ejemplos

Para crear un volumen seccionado de 1000 megabytes de tamaño, en los discos 1 y 2, escriba:

```
create volume stripe size=1000 disk=1,2
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [crear comando](create.md)
