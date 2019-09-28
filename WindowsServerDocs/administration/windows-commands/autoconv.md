---
title: autoconv
description: 'El tema comandos de Windows para **autoconv** : convierte volúmenes de tabla de asignación de archivos (FAT) y FAT32 en el sistema de archivos NTFS.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 17281e54-0b18-4e84-94ac-24586c82df4e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bf36be6bcf3dd8f6c61c6ab0d8780ed77dd8903a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383449"
---
# <a name="autoconv"></a>autoconv

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

convierte los volúmenes de tabla de asignación de archivos (FAT) y FAT32 en el sistema de archivos NTFS, de forma que los archivos y directorios existentes permanecen intactos al iniciarse después de ejecutar **Autochk** . los volúmenes convertidos en el sistema de archivos NTFS no se pueden volver a convertir a FAT o FAT32.
## <a name="remarks"></a>Comentarios
No se puede ejecutar **autoconv** en la línea de comandos. Solo se ejecutará durante el inicio, si se establece mediante **Convert. exe**.
## <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[Autochk](autochk.md)
[convertir](convert.md)
[trabajar con sistemas de archivos](https://go.microsoft.com/fwlink/?LinkId=4509)
