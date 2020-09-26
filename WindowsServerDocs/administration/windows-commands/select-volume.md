---
title: select volume
description: Artículo de referencia del comando SELECT Volume, que selecciona el volumen especificado y desplaza el foco a él.
ms.topic: reference
ms.assetid: 5d70d776-80ad-4f20-8288-a7997fb1df28
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 643db6484842e8a67462b0460a1ecdeac653e577
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389193"
---
# <a name="select-volume"></a>select volume

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Selecciona el volumen especificado y cambia el foco a ese volumen. Este comando también se puede usar para mostrar el volumen que tiene actualmente el foco en el disco seleccionado.

## <a name="syntax"></a>Sintaxis

```
select volume={<n>|<d>}
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<n>` | Número del volumen en el que se va a recibir el foco. Puede ver los números de todos los volúmenes del disco seleccionado actualmente mediante el comando **List Volume** en Diskpart. |
| `<d> `| La letra de unidad o la ruta de acceso del punto de montaje del volumen que va a recibir el foco. |

## <a name="remarks"></a>Observaciones

- Si no se especifica ningún volumen, este comando muestra el volumen que tiene actualmente el foco en el disco seleccionado.

- En un disco básico, la selección de un volumen también proporciona el foco a la partición correspondiente.

  - Si se selecciona un volumen con una partición correspondiente, la partición se seleccionará automáticamente.

  - Si se selecciona una partición con un volumen correspondiente, el volumen se seleccionará automáticamente.

## <a name="examples"></a>Ejemplos

Para desplazar el foco al *volumen 2*, escriba:

```
select volume=2
```

Para desplazar el foco a la *unidad C*, escriba:

```
select volume=c
```

Para desplazar el foco al volumen montado en una carpeta denominada *c:\mountpath*, escriba:

```
select volume=c:\mountpath
```

Para mostrar el volumen que tiene actualmente el foco en el disco seleccionado, escriba:

```
select volume
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando Add Volume](add-volume.md)

- [Attributes Volume (comando)](attributes-volume.md)

- [comando crear reflejo de volumen](create-volume-mirror.md)

- [comando CREATE Volume RAID](create-volume-raid.md)

- [comando CREATE Volume simple](create-volume-simple.md)

- [comando CREATE Volume stripe](create-volume-stripe.md)

- [comando Eliminar volumen](delete-volume.md)

- [comando detail volume](detail-volume.md)

- [comando fsutil Volume](fsutil-volume.md)

- [comando list Volume](list-volume.md)

- [comando Volume sin conexión](offline-volume.md)

- [Onine Volume (comando)](online-volume.md)

- [Seleccionar disco (comando)](select-disk.md)

- [seleccionar partición (comando)](select-partition.md)

- [comando SELECT vDisk](select-vdisk.md)
