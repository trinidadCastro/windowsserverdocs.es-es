---
title: chkntfs
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 93eca810-8699-4716-8e9d-aecd54f704be
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 876c0e0c254216ac217aea7d165d5f4e3a7da9b4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861996"
---
# <a name="chkntfs"></a>chkntfs



Muestra o modifica automáticos de disco comprobación cuando se inicia el equipo. Si se usa sin opciones, **chkntfs** muestra el sistema de archivos del volumen especificado. Si la comprobación automática de archivos está programada para ejecutarse, **chkntfs** muestra si el volumen especificado está desfasado está programado para estar activa o la próxima vez que el equipo se inicia.

> [!NOTE]
> Para ejecutar **chkntfs**, debe ser miembro del grupo Administradores.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
chkntfs <Volume> [...]
chkntfs [/d]
chkntfs [/t[:<Time>]]
chkntfs [/x <Volume> [...]]
chkntfs [/c <Volume> [...]]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<Volume> [...]|Especifica uno o varios volúmenes para comprobar cuándo se inicia el equipo. Volúmenes válidos incluyen letras de unidad (seguidas de dos puntos), puntos de montaje, ni los nombres de volumen.|
|/d|Restaura todas **chkntfs** configuración, excepto el tiempo que resta para la comprobación automática de archivos predeterminada. De forma predeterminada, todos los volúmenes se comprueban cuando se inicia el equipo, y **chkdsk** se ejecuta en los que han cambiado.|
|/t [:\<tiempo >]|Cambia el tiempo que resta Autochk.exe iniciación a la cantidad de tiempo especificado en segundos. Si no especifica un tiempo, **/t** muestra la hora actual de la cuenta atrás.|
|/x \<Volume> [...]|Especifica uno o más volúmenes a excluir de comprobación cuando se inicia el equipo, incluso si el volumen está marcado como que requiere **chkdsk**.|
|/c \<Volume> [...]|Programa de uno o más volúmenes que se comprobarán cuando el equipo se inicia y ejecuta **chkdsk** en los que han cambiado.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="BKMK_examples"></a>Ejemplos

Para mostrar el tipo de sistema de archivos para la unidad C, escriba:
```
chkntfs c:
```
El siguiente resultado indica que un sistema de archivos NTFS:
```
The type of the file system is NTFS.
```

> [!NOTE]
> Si la comprobación automática de archivos está programada para ejecutarse, se mostrará la salida adicionales, que indica si la unidad está dañada o ha sido manualmente programados ser activado la próxima vez que el equipo se inicia.

Para mostrar el tiempo que resta Autochk.exe iniciación, escriba:
```
chkntfs /t
```
Por ejemplo, si el tiempo que resta se establece en 10 segundos, se muestra el siguiente resultado:
```
The AUTOCHK initiation countdown time is set to 10 second(s).
```
Para cambiar el tiempo que resta Autochk.exe iniciación a 30 segundos, escriba:
```
chkntfs /t:30
```

> [!NOTE]
> Aunque puede establecer el tiempo que resta Autochk.exe iniciación a cero, si lo hace así, no podrá de la cancelación de una comprobación automática de archivos puede requerir mucho tiempo.

El **/x** opción de línea de comandos no es acumulativa. Si se escribe más de una vez, la entrada más reciente reemplaza la entrada anterior. Para excluir de varios volúmenes, debe enumerar cada uno de ellos en un solo comando. Por ejemplo, para excluir volúmenes tanto la D y E, escriba:
```
chkntfs /x d: e:
```
El **/c** opción de línea de comandos es acumulativa. Si escribe **/c** más de una vez, cada entrada permanece. Para asegurarse de que se comprueba un volumen en particular, restablecer los valores predeterminados para borrar todos los comandos anteriores, excluir todos los volúmenes de la que se está comprobando y, a continuación, programar la comprobación en el volumen deseado automática de archivos.

Por ejemplo, para programar la comprobación en el volumen D, pero no los volúmenes de C o E automática de archivos, escriba los comandos siguientes en orden:
```
chkntfs /d
chkntfs /x c: d: e:
chkntfs /c d:
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)