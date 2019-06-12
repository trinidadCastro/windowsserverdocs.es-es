---
title: Identificador único
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 64235a4a-b91c-46da-b9b0-68ee90571c2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d237f4d6d3562e3787efe28ca98f9dc553d74898
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440755"
---
# <a name="uniqueid"></a>Identificador único



Muestra o establece el GUID tabla (GPT) identificador o el patrón de arranque (MBR) del registro de partición para el disco con el foco.

> [!IMPORTANT]
> Este comando DiskPart no está disponible en cualquier edición de Windows Vista.

## <a name="syntax"></a>Sintaxis

```
uniqueid disk [id={<dword> | <GUID>}] [noerr]
```

## <a name="parameters"></a>Parámetros

|  Parámetro   |                                                                                             Descripción                                                                                              |
|--------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id={\<dword> |                                                                                               <GUID>}                                                                                                |
|    noerr     | sólo para scripting. Cuando se produce un error, DiskPart sigue procesando comandos como si no hubiera habido ningún error. Sin este parámetro, un error provoca que DiskPart se cierre con un código de error. |

## <a name="remarks"></a>Comentarios

-   Este comando funciona en discos básicos y dinámicos.
-   Debe seleccionarse un disco para que este comando se ejecute correctamente. Use la **seleccione disco** comando para seleccionar un disco y cambiar el foco a ella.

## <a name="BKMK_examples"></a>Ejemplos

Para mostrar la firma del disco MBR tiene el foco, escriba:
```
uniqueid disk
```
Para establecer la firma del disco MBR tiene el foco en 5f1b2c36, escriba:
```
uniqueid disk id=5f1b2c36
```
Para establecer el identificador del disco GPT con el foco en baf784e7 6bbd 4cfb aaac e86c96e166ee, escriba:
```
uniqueid disk id=baf784e7-6bbd-4cfb-aaac-e86c96e166ee
```

#### <a name="additional-references"></a>Referencias adicionales

