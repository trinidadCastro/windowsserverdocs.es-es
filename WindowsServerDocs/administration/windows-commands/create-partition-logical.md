---
title: create partition logical
description: Artículo de referencia del comando lógico Create Partition, que crea una partición lógica en una partición extendida existente.
ms.topic: article
ms.assetid: 1f59b79a-d690-4d0e-ad38-40df5a0ce38e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 26adcb50c43f859d312dadc16328bc65aed29cee
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87879904"
---
# <a name="create-partition-logical"></a>create partition logical

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Crea una partición lógica en una partición extendida existente. Después de crear la partición, ésta recibe el foco automáticamente.

>[!IMPORTANT]
> Este comando solo se puede usar en discos de registro de arranque maestro (MBR). Debe usar el comando [Seleccionar disco](select-disk.md) para seleccionar un disco MBR básico y cambiarle el foco.
>
> Debe crear una [partición extendida](create-partition-extended.md) para poder crear unidades lógicas.

## <a name="syntax"></a>Sintaxis

```
create partition logical [size=<n>] [offset=<n>] [align=<n>] [noerr]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| tamaño =`<n>` | Especifica el tamaño de la partición lógica en megabytes (MB), que debe ser menor que la partición extendida. Si no se proporciona ningún tamaño, la partición continuará hasta que no haya más espacio libre en la partición extendida. |
| desplazamiento =`<n>` | Especifica el desplazamiento en kilobytes (KB), en el que se crea la partición. El desplazamiento se redondea hacia arriba para llenar todo el tamaño del cilindro que se use. Si no se indica un desplazamiento, la partición se ubicará en la primera zona del disco que sea lo suficientemente grande como para albergarla. La partición es, como mínimo, en bytes como el número especificado por **size = `<n>` **. Si especifica un tamaño para la partición lógica, debe ser menor que la partición extendida. |
| align =`<n>` | Alinea todas las extensiones de volumen o partición con el límite de alineación más cercano. Normalmente se usa con matrices de número de unidad lógica (LUN) RAID de hardware para mejorar el rendimiento. `<n>`es el número de kilobytes (KB) desde el principio del disco hasta el límite de alineación más cercano. |
| noerr | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

#### <a name="remarks"></a>Observaciones

- Si no se especifican los parámetros de **tamaño** y **desplazamiento** , la partición lógica se crea en la extensión de disco más grande disponible en la partición extendida.

## <a name="examples"></a>Ejemplos

Para crear una partición lógica de 1000 megabytes de tamaño, en la partición extendida del disco seleccionado, escriba:

```
create partition logical size=1000
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [crear comando](create.md)

- [select disk](select-disk.md)
