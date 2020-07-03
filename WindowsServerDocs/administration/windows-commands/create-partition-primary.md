---
title: create partition primary
description: Artículo de referencia para el comando CREATE Partition Primary, que crea una partición primaria en el disco básico con el foco.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6d652d8e-3935-4a91-8ced-b17c0e7937be
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9014b69528d4a67ba2c9c11b8ec53cf3a0f569f6
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85929628"
---
# <a name="create-partition-primary"></a>create partition primary

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Crea una partición primaria en el disco básico con el foco. Después de crear la partición, ésta recibe el foco automáticamente.

> [!IMPORTANT]
> Se debe seleccionar un disco básico para que esta operación se realice correctamente. Debe usar el comando [Seleccionar disco](select-disk.md) para seleccionar un disco básico y desplazar el foco a él.

## <a name="syntax"></a>Sintaxis

```
create partition primary [size=<n>] [offset=<n>] [id={ <byte> | <guid> }] [align=<n>] [noerr]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| tamaño =`<n>` | Especifica el tamaño de la partición en megabytes (MB). Si no indica un tamaño, la partición continuará mientras haya espacio no asignado en la región actual. |
| desplazamiento =`<n>` | Desplazamiento en kilobytes (KB), en el que se crea la partición. Si no se proporciona ningún desplazamiento, la partición se iniciará al principio de la mayor extensión de disco que sea lo suficientemente grande como para almacenarla. |
| align =`<n>` | Alinea todas las extensiones de partición con el límite de alineación más cercano. Normalmente se usa con matrices de número de unidad lógica (LUN) RAID de hardware para mejorar el rendimiento. `<n>`es el número de kilobytes (KB) desde el principio del disco hasta el límite de alineación más cercano. |
| ID. = { `<byte>  | <guid>` } | Especifica el tipo de partición. Este parámetro está pensado para uso exclusivo del fabricante de equipos originales (OEM). Se puede especificar cualquier tipo de partición byte o GUID con este parámetro. DiskPart no comprueba la validez del tipo de partición, excepto para asegurarse de que es un byte en formato hexadecimal o un GUID. **PRECAUCIÓN:** La creación de particiones con este parámetro puede provocar un error en el equipo o no poder iniciarse. A menos que sea un OEM o un profesional de TI con experiencia en discos GPT, no cree particiones en discos GPT con este parámetro. En su lugar, use siempre el comando [Create Partition EFI](create-partition-efi.md) para crear particiones del sistema EFI, el comando [Create Partition MSR](create-partition-msr.md) para crear particiones reservadas de Microsoft y el comando [Create Partition Primary](create-partition-primary.md)(sin el `id={ <byte>  | <guid>` parámetro) para crear particiones primarias en discos GPT.<p>En el **caso de los discos de registro de arranque maestro (MBR)**, debe especificar un byte de tipo de partición, en formato hexadecimal, para la partición. Si no se especifica este parámetro, el comando crea una partición de tipo `0x06` , que especifica que un sistema de archivos no está instalado. Algunos ejemplos son:<ul><li>**Partición de datos LDM:** 0x42</li><li>**Partición de recuperación:** 0x27</li><li>**Partición OEM reconocida:** 0X12, 0X84, 0XDE, 0Xfe, 0XA0</li></ul><p>**En el caso de los discos de tabla de particiones GUID (GPT)**, puede especificar un GUID de tipo de partición para la partición que desea crear. Los GUID reconocidos incluyen:<ul><li>**Partición del sistema EFI:** c12a7328-f81f-11D2-ba4b-00a0c93ec93b</li><li>**Partición reservada de Microsoft:** e3c9e316-0b5c-4db8-817d-f92df00215ae</li><li>**Partición de datos básica:** ebd0a0a2-b9e5-4433-87c0-68b6b72699c7</li><li>**Partición de metadatos LDM (disco dinámico):** 5808c8aa-7e8f-42e0-85d2-e1e90434cfb3</li><li>**Partición de datos LDM (disco dinámico):** af9b60a0-1431-4f62-bc68-3311714a69ad</li><li>**Partición de recuperación:** de94bba4-06d1-4d40-A16A-bfd50179d6ac<p>Si este parámetro no se especifica para un disco GPT, el comando crea una partición de datos básica.</li></ul> |
| noerr | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Si no se especifica el parámetro noerr, un error hará que DiskPart se cierre con un código de error. |

## <a name="examples"></a>Ejemplos

Para crear una partición primaria de 1000 megabytes de tamaño, escriba:

```
create partition primary size=1000
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando Assign](assign.md)

- [crear comando](create.md)

- [select disk](select-disk.md)
