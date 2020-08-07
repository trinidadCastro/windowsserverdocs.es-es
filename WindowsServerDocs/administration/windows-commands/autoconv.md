---
title: autoconv
description: Artículo de referencia para el comando autoconv, que convierte volúmenes de tabla de asignación de archivos (FAT) y FAT32 en el sistema de archivos NTFS.
ms.topic: article
ms.assetid: 17281e54-0b18-4e84-94ac-24586c82df4e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9f4c7ed1a2c370de46e02130fd06e1b9326207e9
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87895268"
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
