---
title: autochk
description: Artículo de referencia del comando Autochk, que se ejecuta cuando se inicia el equipo y antes de Windows Server a partir de la comprobación de la integridad lógica de un sistema de archivos.
ms.topic: reference
ms.assetid: 8787e6a3-f023-4ea5-b2d1-61c6876d8aff
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: bb360c4207371d8056d3a3840951b5eaa10eb9bf
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633027"
---
# <a name="autochk"></a>autochk

Se ejecuta cuando se inicia el equipo y antes de que Windows Server empiece a comprobar la integridad lógica de un sistema de archivos.

**Autochk.exe** es una versión de **CHKDSK** que solo se ejecuta en discos NTFS y solo antes de que se inicie Windows Server. **Autochk** no se puede ejecutar directamente desde la línea de comandos. En su lugar, **Autochk** se ejecuta en las siguientes situaciones:

- Si intenta ejecutar **CHKDSK** en el volumen de arranque.

- Si **CHKDSK** no puede obtener el uso exclusivo del volumen.

- Si el volumen se marca como sucio.

## <a name="remarks"></a>Observaciones

> [!WARNING]
> La herramienta de línea de comandos **Autochk** no se puede ejecutar directamente desde la línea de comandos. En su lugar, use la herramienta de línea de comandos **chkntfs** para configurar la forma en que se desea que **Autochk** se ejecute en el inicio.
>
> - Puede usar **chkntfs** con el parámetro **/x** para impedir que **Autochk** se ejecute en un volumen específico o en varios volúmenes.
>
> - Use la herramienta de línea de comandos de **chkntfs.exe** con el parámetro **/t** para cambiar el retraso de AUTOCHK de 0 a 3 segundos a (259.200 segundos). Sin embargo, un retraso largo significa que el equipo no se inicia hasta que transcurre el tiempo o hasta que se presiona una tecla para cancelar el **Autochk**.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [CHKDSK (comando)](chkdsk.md)

- [chkntfs (comando)](chkntfs.md)
