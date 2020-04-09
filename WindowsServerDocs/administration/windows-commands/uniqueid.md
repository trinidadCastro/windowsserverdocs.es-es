---
title: UniqueID
description: Windows Commands topic for UniqueID, que muestra o establece el identificador de la tabla de particiones GUID (GPT) o el registro de arranque maestro (MBR) del disco que tiene el foco.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 64235a4a-b91c-46da-b9b0-68ee90571c2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 29d7bf0498e76d5192e986aadabb77d575a8102b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832318"
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
| ID = {\<DWORD > |                                                                                               <GUID>}                                                                                                |
|    noerr     | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

## <a name="remarks"></a>Comentarios

-   Este comando funciona en discos básicos y dinámicos.
-   Se debe seleccionar un disco para que este comando se ejecute correctamente. Use el comando **Seleccionar disco** para seleccionar un disco y desplazar el foco a él.

## <a name="examples"></a><a name=BKMK_examples></a>Example

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

