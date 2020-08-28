---
title: autoconv
description: Artículo de referencia para el comando autoconv, que convierte volúmenes de tabla de asignación de archivos (FAT) y FAT32 en el sistema de archivos NTFS.
ms.topic: reference
ms.assetid: 17281e54-0b18-4e84-94ac-24586c82df4e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3d69d70200b4885404486bc4956903a17b329630
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028943"
---
# <a name="autoconv"></a>autoconv

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Convierte los volúmenes de tabla de asignación de archivos (FAT) y FAT32 en el sistema de archivos NTFS, de forma que los archivos y directorios existentes permanecen intactos al iniciarse después de ejecutar **Autochk** . los volúmenes convertidos en el sistema de archivos NTFS no se pueden volver a convertir a FAT o FAT32.

> [!IMPORTANT]
> No se puede ejecutar **autoconv** desde la línea de comandos. Solo se puede ejecutar durante el inicio, si se establece a través de **convert.exe**.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando Autochk](autochk.md)

- [Convert (comando)](convert.md)
