---
title: create partition efi
description: Artículo de referencia para el comando CREATE Partition EFI, que crea una partición del sistema Extensible Firmware Interface (EFI) en un disco de tabla de particiones GUID (GPT) en equipos basados en Itanium.
ms.topic: article
ms.assetid: 3cfc1fca-6515-4a4d-bfae-615fa8045ea9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e0307410648453a42c66e7327b5c671a702017e2
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87880003"
---
# <a name="create-partition-efi"></a>create partition efi

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Crea una partición del sistema de Extensible Firmware Interface (EFI) en un disco de tabla de particiones GUID (GPT) en equipos basados en Itanium. Una vez creada la partición, se asigna el foco a la nueva partición.

>[!NOTE]
> Se debe seleccionar un disco GPT para que esta operación se realice correctamente. Use el comando [Seleccionar disco](select-disk.md) para seleccionar un disco y desplazar el foco a él.

## <a name="syntax"></a>Sintaxis

```
create partition efi [size=<n>] [offset=<n>] [noerr]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| tamaño =`<n>` | Tamaño de la partición en megabytes (MB). Si no se proporciona ningún tamaño, la partición continúa hasta que no haya más espacio libre en la región actual. |
| desplazamiento =`<n>` | Desplazamiento en kilobytes (KB), en el que se crea la partición. Si no se indica un desplazamiento, la partición se colocará en la primera zona del disco que sea lo suficientemente grande como para albergarla. |
| noerr | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

#### <a name="remarks"></a>Observaciones

- Debe agregar al menos un volumen con el comando **Add Volume** para poder usar el comando **Create** .

- Después de ejecutar el comando **Create** , puede usar el comando **exec** para ejecutar un script de duplicación para la copia de seguridad de la instantánea.

- Puede usar el comando **Begin backup** para especificar una copia de seguridad completa, en lugar de una copia de seguridad de copia.

## <a name="examples"></a>Ejemplos

Para crear una partición EFI de 1000 megabytes en el disco seleccionado, escriba:

```
create partition efi size=1000
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [crear comando](create.md)

- [select disk](select-disk.md)
