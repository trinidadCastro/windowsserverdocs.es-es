---
title: autochk
description: 'Tema de los comandos de Windows para **autochk** : se ejecuta cuando se inicia el equipo y antes de Windows Server a partir comprobar la integridad lógica de un sistema de archivos.'
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8787e6a3-f023-4ea5-b2d1-61c6876d8aff
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 023bd81b93106a091fb9f26d97cf7eda75f0f633
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888466"
---
# <a name="autochk"></a>autochk



Se ejecuta cuando se inicia el equipo y antes de comenzar a comprobar la integridad lógica de un sistema de archivos en Windows Server® 2008 R2.

**Autochk.exe** es una versión de **Chkdsk** que se ejecuta solo en los discos NTFS y solo antes de Windows Server 2008 R2. **Autochk** no se puede ejecutar directamente desde la línea de comandos. En su lugar, **Autochk** se ejecuta en las situaciones siguientes:
-   Si intenta ejecutar **Chkdsk** en el volumen de arranque
-   Si **Chkdsk** no se puede obtener el uso exclusivo del volumen
-   Si el volumen se marca como modificado

## <a name="remarks"></a>Comentarios

> -   [!WARNING]
>     El **Autochk** herramienta de línea de comandos no se puede ejecutar directamente desde la línea de comandos. En su lugar, use el **Chkntfs** herramienta de línea de comandos para configurar la forma que desee **Autochk** debe ejecutar al inicio.
-   Puede usar **Chkntfs** con el **/x** parámetro para evitar **Autochk** desde que se ejecuta en un volumen específico o varios volúmenes.
-   Use la **Chkntfs.exe** herramienta de línea de comandos con el **/t** parámetro para cambiar el retardo Autochk de 0 segundos a hasta 3 días (259.200 segundos). Sin embargo, un gran retraso significa que el equipo no se inicia hasta que transcurra el tiempo o hasta que presione una tecla para cancelar **Autochk**.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Chkdsk](chkdsk.md)

[Chkntfs](chkntfs.md)

[Solución de problemas de discos y sistemas de archivos](https://go.microsoft.com/fwlink/?LinkId=4527)