---
title: inactive
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f4fb4695-4e66-4166-b4ab-2c86a4605580
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 38a4f731bef9515a387b0343eaf4cb06142e6620
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842128"
---
# <a name="inactive"></a>inactive



En discos de registro de arranque maestro (MBR) básicos, marca como inactiva la partición de sistema o la partición de arranque que tiene el foco.

## <a name="syntax"></a>Sintaxis

```
inactive
```

## <a name="remarks"></a>Comentarios

> [!CAUTION]
> Si no existe una partición activa, puede que el equipo no se inicie. No marque una partición de sistema o de arranque como inactiva a menos que sea un usuario con experiencia que comprenda bien la familia de sistemas operativos Windows.</br>> Si no puede iniciar el equipo después de marcar la partición de sistema o de arranque como inactiva, inserte el CD de instalación de Windows en la unidad de CD-ROM, reinicie el equipo y, a continuación, repare la partición mediante los comandos **fixmbr** y **fixboot** de la consola de recuperación.
> -   Después de marcar la partición del sistema o la partición de arranque como inactiva, el equipo se inicia con la siguiente opción especificada en el BIOS, como la unidad de CD-ROM o un entorno de ejecución previo al arranque (PXE).
> -   Se debe seleccionar una partición de arranque o del sistema activa para que esta operación se realice correctamente. Use el comando **seleccionar partición** para seleccionar la partición activa y cambiar el foco a ella.

## <a name="examples"></a><a name=BKMK_examples></a>Example

```
inactive
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

