---
title: clean
description: Tema de referencia del comando Clean de DiskPart, que quita todas las particiones o el formato de volumen del disco que tiene el foco.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9bbe6fd3-e07e-487b-9035-910957a1d326
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 45a23919dc07c8c1525808859471fdcb9f9e9403
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82712877"
---
# <a name="clean"></a>clean

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Quita todas las particiones o el formato de volumen del disco que tiene el foco.

>[!NOTE]
> Para obtener una versión de PowerShell de este comando, consulte [comando CLEAR-Disk](https://docs.microsoft.com/powershell/module/storage/clear-disk).

## <a name="syntax"></a>Sintaxis

```
clean [all]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| all | Especifica que cada sector y cada sector del disco se establezcan en cero, lo que elimina completamente todos los datos contenidos en el disco. |

#### <a name="remarks"></a>Observaciones

- En los discos de registro de arranque maestro (MBR), solo se sobrescribe la información de particiones MBR y la información de sectores ocultos.

- En los discos de tabla de particiones GUID (GPT), se sobrescribe la información de particiones GPT, incluido el MBR de protección. No hay información de sectores ocultos.

- Se debe seleccionar un disco para que esta operación se realice correctamente. Use el comando **Seleccionar disco** para seleccionar un disco y desplazar el foco a él.

## <a name="examples"></a>Ejemplos

Para quitar todo el formato del disco seleccionado, escriba:

```
clean
```

## <a name="additional-references"></a>Referencias adicionales

- [comando CLEAR-Disk](https://docs.microsoft.com/powershell/module/storage/clear-disk)

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
