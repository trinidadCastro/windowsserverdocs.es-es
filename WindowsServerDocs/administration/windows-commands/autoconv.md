---
title: autoconv
description: 'Tema de los comandos de Windows para **autoconv** : convierte archivos (Fat) de la tabla de asignación y el sistema de archivos de los volúmenes Fat32 a NTFS.'
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 1d135da085558f12a51c8febfd72aa805e1d12f1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872496"
---
# <a name="autoconv"></a>autoconv

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Convierte archivos (Fat) de la tabla de asignación y los volúmenes Fat32 al sistema de archivos NTFS, dejando intacto archivos y directorios existentes en el inicio después de **autochk** se ejecuta. no se puede convertir los volúmenes que se convierte en el sistema de archivos NTFS a Fat o Fat32.
## <a name="remarks"></a>Comentarios
No se puede ejecutar **autoconv** en la línea de comandos. Esto solo se ejecutará en el inicio, si establece a través de **convert.exe**.
## <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[autochk](autochk.md)
[convertir](convert.md)
[trabajar con sistemas de archivos](https://go.microsoft.com/fwlink/?LinkId=4509)
