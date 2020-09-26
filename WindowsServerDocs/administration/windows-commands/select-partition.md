---
title: select partition
description: Artículo de referencia del comando SELECT Partition, que selecciona la partición especificada y desplaza el foco a ella.
ms.topic: reference
ms.assetid: 25f70083-b8f7-4a8e-9b34-4b3ffbe06670
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6418ff1a08bdc12355d2b2dc75bbb7151539ae02
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389238"
---
# <a name="select-partition"></a>select partition

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Selecciona la partición especificada y desplaza el foco a ella. Este comando también se puede usar para mostrar la partición que tiene actualmente el foco en el disco seleccionado.

## <a name="syntax"></a>Sintaxis

```
select partition=<n>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| partición =`<n>` | Número de la partición que va a recibir el foco. Puede ver los números de todas las particiones del disco seleccionado actualmente mediante el comando **List Partition** en Diskpart. |

#### <a name="remarks"></a>Observaciones

- Antes de poder seleccionar una partición, primero debe seleccionar un disco con el comando **Seleccionar disco** .

  - Si no se especifica ningún número de partición, esta opción muestra la partición que tiene actualmente el foco en el disco seleccionado.

  - Si se selecciona un volumen con una partición correspondiente, la partición se selecciona automáticamente.

  - Si se selecciona una partición con un volumen correspondiente, el volumen se selecciona automáticamente.

## <a name="examples"></a>Ejemplos

Para desplazar el foco a la *partición 3*, escriba:

```
select partitition=3
```

Para mostrar la partición que tiene actualmente el foco en el disco seleccionado, escriba:

```
select partition
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando CREATE Partition EFI](create-partition-efi.md)

- [comando de creación de partición extendida](create-partition-extended.md)

- [crear partición (comando lógico)](create-partition-logical.md)

- [comando CREATE Partition MSR](create-partition-msr.md)

- [comando crear partición principal](create-partition-primary.md)

- [comando DELETE Partition](delete-partition.md)

- [comando de partición de detalles](detail-partition.md)

- [Seleccionar disco (comando)](select-disk.md)

- [comando SELECT vDisk](select-vdisk.md)

- [comando Seleccionar volumen](select-volume.md)
