---
title: create partition msr
description: Artículo de referencia para Create Partition MSR, que crea una partición reservada de Microsoft (MSR) en un disco de tabla de particiones GUID (GPT).
ms.topic: reference
ms.assetid: 04fba033-23cb-4521-bd5d-db96131f2e73
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1df792001cf48d9d5fce69de6dc9bc6bdd09a1f8
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033233"
---
# <a name="create-partition-msr"></a>create partition msr

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Crea una partición reservada de Microsoft (MSR) en un disco de tabla de particiones GUID (GPT). Se requiere una partición reservada de Microsoft en cada disco GPT. El tamaño de esta partición depende del tamaño total del disco GPT. El tamaño del disco GPT debe ser de al menos 32 MB para crear una partición reservada de Microsoft.

> [!IMPORTANT]
> Tenga mucho cuidado al usar este comando. Dado que los discos GPT requieren un diseño de partición específico, la creación de particiones reservadas de Microsoft puede hacer que el disco sea ilegible.
>
> Se debe seleccionar un disco GPT básico para que esta operación se realice correctamente. Debe usar el comando [Seleccionar disco](select-disk.md) para seleccionar un disco GPT básico y cambiarle el foco.

## <a name="syntax"></a>Sintaxis

```
create partition msr [size=<n>] [offset=<n>] [noerr]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| tamaño =`<n>` | Tamaño de la partición en megabytes (MB). La partición es, como mínimo, en bytes como el número especificado por `<n>` . Si no se proporciona ningún tamaño, la partición continúa hasta que no haya más espacio libre en la región actual. |
| desplazamiento =`<n>` | Especifica el desplazamiento en kilobytes (KB), en el que se crea la partición. El desplazamiento se redondea hacia arriba para llenar completamente el tamaño del sector que se utiliza. Si no se indica un desplazamiento, la partición se colocará en la primera zona del disco que sea lo suficientemente grande como para albergarla. |
| noerr | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

#### <a name="remarks"></a>Observaciones

- En los discos GPT que se utilizan para arrancar el sistema operativo Windows, la partición del sistema Extensible Firmware Interface (EFI) es la primera partición del disco, seguida de la partición reservada de Microsoft. los discos GPT que se usan únicamente para el almacenamiento de datos no tienen una partición del sistema EFI, en cuyo caso la partición reservada de Microsoft es la primera partición.

- Windows no monta las particiones reservadas de Microsoft. No se puede almacenar datos en ellas y no se pueden eliminar.

## <a name="examples"></a>Ejemplos

Para crear una partición reservada de Microsoft de 1000 megabytes de tamaño, escriba:

```
create partition msr size=1000
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [crear comando](create.md)

- [select disk](select-disk.md)
