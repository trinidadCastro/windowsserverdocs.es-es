---
title: reset
description: Artículo de referencia del comando Reset, que restablece DiskShadow.exe al estado predeterminado.
ms.topic: reference
ms.assetid: afbdab44-199c-4e11-884f-e96804965c21
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9ca1b0574fae1e0d00bc1f2cbec17ff9572ed253
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038343"
---
# <a name="reset"></a>reset

Restablece DiskShadow.exe al estado predeterminado. Este comando es especialmente útil para separar las operaciones de DiskShadow compuestas, como **creación**, **importación**, **copia de seguridad**o **restauración**.

> [! IMPORTANTE: después de ejecutar este comando, se perderá la información de estado de los comandos, como **Agregar**, **establecer**, **cargar**o **escritor**. Este comando también libera interfaces IVssBackupComponent y pierde instantáneas no persistentes.

## <a name="syntax"></a>Sintaxis

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
