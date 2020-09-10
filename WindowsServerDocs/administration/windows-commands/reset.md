---
title: reset
description: Artículo de referencia del comando Reset, que restablece DiskShadow.exe al estado predeterminado.
ms.topic: reference
ms.assetid: afbdab44-199c-4e11-884f-e96804965c21
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 84f4aedee746e642e59a09055c3994160f503afa
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89626880"
---
# <a name="reset"></a>reset

Restablece DiskShadow.exe al estado predeterminado. Este comando es especialmente útil para separar las operaciones de DiskShadow compuestas, como **creación**, **importación**, **copia de seguridad**o **restauración**.

> [! IMPORTANTE: después de ejecutar este comando, se perderá la información de estado de los comandos, como **Agregar**, **establecer**, **cargar**o **escritor**. Este comando también libera interfaces IVssBackupComponent y pierde instantáneas no persistentes.

## <a name="syntax"></a>Syntax

```
reset
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [crear comando](create.md)

- [comando importar](import_1.md)

- [comando backup](begin-backup.md)

- [comando restore](begin-restore.md)

- [Agregar comando](add.md)

- [Set (comando)](set_2.md)

- [comando LOAD](reg-load.md)

- [escritor (comando)](writer.md)
