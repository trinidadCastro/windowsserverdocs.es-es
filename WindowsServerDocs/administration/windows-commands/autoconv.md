---
title: autoconv
description: Comando de comandos de Windows para **autoconv**, que convierte volúmenes de tabla de asignación de archivos (FAT) y FAT32 en el sistema de archivos NTFS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 17281e54-0b18-4e84-94ac-24586c82df4e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fe0e388a1d4fd79567ef0562197e3181bbbc46f4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851118"
---
# <a name="autoconv"></a>autoconv

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Convierte los volúmenes de tabla de asignación de archivos (FAT) y FAT32 en el sistema de archivos NTFS, de forma que los archivos y directorios existentes permanecen intactos al iniciarse después de ejecutar **Autochk** . los volúmenes convertidos en el sistema de archivos NTFS no se pueden volver a convertir a FAT o FAT32.

## <a name="remarks"></a>Comentarios

No se puede ejecutar **autoconv** en la línea de comandos. Solo se ejecutará durante el inicio, si se establece mediante **Convert. exe**.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [autochk](autochk.md)

- [convert](convert.md)

- [Trabajar con sistemas de archivos](https://go.microsoft.com/fwlink/?LinkId=4509)
