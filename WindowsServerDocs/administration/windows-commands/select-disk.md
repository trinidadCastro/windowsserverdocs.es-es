---
title: select disk
description: Artículo de referencia para el comando Seleccionar disco, que selecciona el disco especificado y, a continuación, desplaza el foco a él.
ms.topic: reference
ms.assetid: a0da614b-09d9-433b-b4eb-9127f84431cb
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 194bd36305060c8adfec27435ef05bf87605b45c
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389248"
---
# <a name="select-disk"></a>select disk

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Selecciona el disco especificado y cambia el foco a ese disco.

## <a name="syntax"></a>Sintaxis

```
select disk={<n>|<disk path>|system|next}
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<n>` | Especifica el número del disco en el que se va a recibir el foco. Puede ver los números de todos los discos del equipo mediante el comando **List Disk** en Diskpart.<p>**NOTA**:<br>Al configurar sistemas con varios discos, no use **Select Disk = 0** para especificar el disco del sistema. El equipo puede reasignar los números de disco al reiniciar y los distintos equipos con la misma configuración de disco pueden tener números de disco diferentes. |
| `<disk path>` | Especifica la ubicación del disco para recibir el foco, por ejemplo, `PCIROOT(0)#PCI(0F02)#atA(C00T00L00)` . Para ver la ruta de acceso de ubicación de un disco, selecciónelo y, a continuación, escriba **detail Disk**. |
| sistema | En los equipos BIOS, esta opción especifica que el disco 0 recibe el foco. En los equipos EFI, el disco que contiene la partición del sistema EFI (ESP), que se usa para el arranque actual, recibe el foco. En los equipos EFI, el comando producirá un error si no hay ningún ESP, si hay más de un ESP, o si el equipo se arranca desde Entorno de preinstalación de Windows (Windows PE). |
| Siguiente | Una vez seleccionado un disco, esta opción recorre en iteración todos los discos de la lista de discos. Al ejecutar esta opción, el siguiente disco de la lista recibe el foco. |

## <a name="examples"></a>Ejemplos

Para desplazar el foco al disco 1, escriba:

```
select disk=1
```

Para seleccionar un disco mediante la ruta de acceso de ubicación, escriba:

```
select disk=PCIROOT(0)#PCI(0100)#atA(C00T00L01)
```

Para desplazar el foco al disco del sistema, escriba:

```
select disk=system
```

Para desplazar el foco al siguiente disco del equipo, escriba:

```
select disk=next
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [seleccionar partición (comando)](select-partition.md)

- [comando SELECT vDisk](select-vdisk.md)

- [comando Seleccionar volumen](select-volume.md)
