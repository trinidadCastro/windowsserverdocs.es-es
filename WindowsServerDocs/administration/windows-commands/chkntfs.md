---
title: chkntfs
description: Comandos de Windows tema para chkntfs, que muestra o modifica la comprobación automática del disco cuando se inicia el equipo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 93eca810-8699-4716-8e9d-aecd54f704be
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bb04022964b3c315c1003a9746f6551fc281dba3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847778"
---
# <a name="chkntfs"></a>chkntfs

Muestra o modifica la comprobación automática del disco cuando se inicia el equipo. Si se usa sin opciones, **chkntfs** muestra el sistema de archivos del volumen especificado. Si la comprobación automática de archivos está programada para ejecutarse, **chkntfs** muestra si el volumen especificado está sucio o está programado para ser comprobado la próxima vez que se inicie el equipo.

> [!NOTE]
> Para ejecutar **chkntfs**, debe ser miembro del grupo administradores.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
chkntfs <Volume> [...]
chkntfs [/d]
chkntfs [/t[:<Time>]]
chkntfs [/x <Volume> [...]]
chkntfs [/c <Volume> [...]]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<> de volumen [...]|Especifica uno o más volúmenes que se comprobarán cuando se inicie el equipo. Los volúmenes válidos incluyen Letras de unidad (seguidas de dos puntos), puntos de montaje o nombres de volumen.|
|/d|Restaura todos los valores predeterminados de **chkntfs** , excepto el tiempo de cuenta atrás para la comprobación automática de archivos. De forma predeterminada, todos los volúmenes se comprueban cuando se inicia el equipo y **CHKDSK** se ejecuta en los que están sucios.|
|/t [:\<hora >]|Cambia el tiempo de la cuenta de inicio de la inicialización de Autochk. exe a la cantidad de tiempo especificada en segundos. Si no especifica una hora, **/t** muestra el tiempo de cuenta atrás actual.|
|/x \<volumen > [...]|Especifica uno o más volúmenes que se excluirán de la comprobación cuando se inicie el equipo, incluso si el volumen está marcado como que requiere **CHKDSK**.|
|/c \<volumen > [...]|Programa uno o más volúmenes para que se comprueben cuando se inicie el equipo y ejecute **CHKDSK** en los que han cambiado.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para mostrar el tipo de sistema de archivos de la unidad C, escriba:
```
chkntfs c:
```
La siguiente salida indica un sistema de archivos NTFS:
```
The type of the file system is NTFS.
```

> [!NOTE]
> Si la comprobación automática de archivos está programada para ejecutarse, se mostrará una salida adicional, que indica si la unidad está dañada o se ha programado manualmente para que se Compruebe la próxima vez que se inicie el equipo.

Para mostrar el tiempo de la cuenta de inicio de la inicialización de Autochk. exe, escriba:
```
chkntfs /t
```
Por ejemplo, si el tiempo de cuenta atrás se establece en 10 segundos, se muestra el siguiente resultado:
```
The AUTOCHK initiation countdown time is set to 10 second(s).
```
Para cambiar el tiempo de la cuenta de inicio de Autochk. exe a 30 segundos, escriba:
```
chkntfs /t:30
```

> [!NOTE]
> Aunque puede establecer el tiempo de inicio de la cuenta de inicio de Autochk. exe en cero, esto impedirá que cancele una comprobación de archivo automática que puede tardar mucho tiempo.

La opción de línea de comandos **/x** no es acumulativa. Si se escribe más de una vez, la entrada más reciente invalida la entrada anterior. Para excluir la comprobación de varios volúmenes, debe enumerarlos en un solo comando. Por ejemplo, para excluir los volúmenes D e E, escriba:
```
chkntfs /x d: e:
```
La opción de línea de comandos **/c** es acumulativa. Si escribe **/c** más de una vez, cada entrada permanece. Para asegurarse de que solo se comprueba un volumen determinado, restablezca los valores predeterminados para borrar todos los comandos anteriores, excluir todos los volúmenes de la comprobación y, a continuación, programe la comprobación automática de archivos en el volumen deseado.

Por ejemplo, para programar la comprobación automática de archivos en el volumen D pero no en los volúmenes de C o E, escriba los siguientes comandos en orden:
```
chkntfs /d
chkntfs /x c: d: e:
chkntfs /c d:
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)