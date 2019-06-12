---
title: inactivo
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f4fb4695-4e66-4166-b4ab-2c86a4605580
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9c8ded732d984830c7892720f75938979f1abb67
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438167"
---
# <a name="inactive"></a>inactivo



En discos de arranque maestro (MBR) del registro, marca la partición del sistema o una partición de arranque con el foco como inactiva.

## <a name="syntax"></a>Sintaxis

```
inactive
```

## <a name="remarks"></a>Comentarios

> [!CAUTION]
> El equipo podría no iniciarse sin una partición activa. No marque una partición de sistema o de arranque como inactiva a menos que sea un usuario con experiencia con una descripción completa de la familia de Windows de los sistemas operativos.</br>> Si no puede iniciar el equipo después de marcar la partición de sistema o de arranque como inactiva, inserte el CD de instalación de Windows en la unidad de CD-ROM, reinicie el equipo y, a continuación, repare la partición mediante la **fixmbr** y **fixboot** comandos en la consola de recuperación.
> -   Después de marcar la partición del sistema o una partición de arranque como inactiva, el equipo se inicia desde la siguiente opción especificada en el BIOS, por ejemplo, la unidad de CD-ROM o una ejecución previo al arranque (PXE) de entorno.
> -   Debe seleccionarse una partición activa del sistema o de inicio para que esta operación se realice correctamente. Use la **seleccione partición** comando para seleccionar la partición activa y desplace el foco a ella.

## <a name="BKMK_examples"></a>Ejemplos

```
inactive
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

