---
title: UniqueID
description: Artículo de referencia de UniqueID, que muestra o establece el identificador de la tabla de particiones GUID (GPT) o la firma del registro de arranque maestro (MBR) del disco que tiene el foco.
ms.topic: reference
ms.assetid: 64235a4a-b91c-46da-b9b0-68ee90571c2a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: de379b25edf83212e25b34e7c8594ef03090ca9a
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89626571"
---
# <a name="uniqueid"></a>UniqueID

Muestra o establece el identificador de la tabla de particiones GUID (GPT) o la firma del registro de arranque maestro (MBR) del disco que tiene el foco.

> [!IMPORTANT]
> Este comando DiskPart no está disponible en ninguna edición de Windows Vista.

## <a name="syntax"></a>Sintaxis

```
uniqueid disk [id={<dword> | <GUID>}] [noerr]
```

### <a name="parameters"></a>Parámetros

|  Parámetro   |                                                                                             Descripción                                                                                              |
|--------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ID. = {\<dword> |                                                                                               <GUID>}                                                                                                |
|    noerr     | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

## <a name="remarks"></a>Observaciones

-   Este comando funciona en discos básicos y dinámicos.
-   Se debe seleccionar un disco para que este comando se ejecute correctamente. Use el comando **Seleccionar disco** para seleccionar un disco y desplazar el foco a él.

## <a name="examples"></a>Ejemplos

Para mostrar la firma del disco MBR con el foco, escriba:
```
uniqueid disk
```
Para establecer la firma del disco MBR con el foco en 5f1b2c36, escriba:
```
uniqueid disk id=5f1b2c36
```
Para establecer el identificador del disco GPT con el foco en baf784e7-6bbd-4cfb-AAAC-e86c96e166ee, escriba:
```
uniqueid disk id=baf784e7-6bbd-4cfb-aaac-e86c96e166ee
```

## <a name="additional-references"></a>Referencias adicionales

