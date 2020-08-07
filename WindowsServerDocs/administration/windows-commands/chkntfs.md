---
title: chkntfs
description: Artículo de referencia del comando chkntfs, que muestra o modifica la comprobación automática del disco cuando se inicia el equipo.
ms.topic: article
ms.assetid: 93eca810-8699-4716-8e9d-aecd54f704be
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d0f0c0a956cb2b286d4f5b1f34332dc01d984462
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87892735"
---
# <a name="chkntfs"></a>chkntfs

Muestra o modifica la comprobación automática del disco cuando se inicia el equipo. Si se usa sin opciones, **chkntfs** muestra el sistema de archivos del volumen especificado. Si la comprobación automática de archivos está programada para ejecutarse, **chkntfs** muestra si el volumen especificado está sucio o está programado para ser comprobado la próxima vez que se inicie el equipo.

> [!NOTE]
> Para ejecutar **chkntfs**, debe ser miembro del grupo administradores.

## <a name="syntax"></a>Sintaxis

```
chkntfs <volume> [...]
chkntfs [/d]
chkntfs [/t[:<time>]]
chkntfs [/x <volume> [...]]
chkntfs [/c <volume> [...]]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<volume>` [...] | Especifica uno o más volúmenes que se comprobarán cuando se inicie el equipo. Los volúmenes válidos incluyen Letras de unidad (seguidas de dos puntos), puntos de montaje o nombres de volumen. |
| /d | Restaura todos los valores predeterminados de **chkntfs** , excepto el tiempo de cuenta atrás para la comprobación automática de archivos. De forma predeterminada, todos los volúmenes se comprueban cuando se inicia el equipo y **CHKDSK** se ejecuta en los que están sucios. |
| /t [ `:<time>` ] | Cambia el tiempo de cuenta regresiva de inicio de Autochk.exe por la cantidad de tiempo especificada en segundos. Si no especifica una hora, **/t** muestra el tiempo de cuenta atrás actual. |
| /x `<volume>` [...] | Especifica uno o más volúmenes que se excluirán de la comprobación cuando se inicie el equipo, incluso si el volumen está marcado como que requiere **CHKDSK**. |
| /c `<volume>` [...] | Programa uno o más volúmenes para que se comprueben cuando se inicie el equipo y ejecute **CHKDSK** en los que han cambiado. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para mostrar el tipo de sistema de archivos de la unidad C, escriba:

```
chkntfs c:
```

> [!NOTE]
> Si la comprobación automática de archivos está programada para ejecutarse, se mostrará una salida adicional, que indica si la unidad está dañada o se ha programado manualmente para que se Compruebe la próxima vez que se inicie el equipo.

Para mostrar el tiempo de cuenta regresiva de inicio de Autochk.exe, escriba:

```
chkntfs /t
```

Para cambiar el tiempo de inactividad de inicio de Autochk.exe a 30 segundos, escriba:

```
chkntfs /t:30
```

> [!NOTE]
> Aunque puede establecer el tiempo de inactividad de la inicialización de Autochk.exe en cero, esto impedirá que cancele una comprobación de archivos automática que puede tardar mucho tiempo.

Para excluir la comprobación de varios volúmenes, debe enumerarlos en un solo comando. Por ejemplo, para excluir los volúmenes D e E, escriba:

```
chkntfs /x d: e:
```

> [!IMPORTANT]
> La opción de línea de comandos **/x** no es acumulativa. Si se escribe más de una vez, la entrada más reciente invalida la entrada anterior.

Para programar la comprobación automática de archivos en el volumen D, pero no los volúmenes C o E, escriba los siguientes comandos en orden:

```
chkntfs /d
chkntfs /x c: d: e:
chkntfs /c d:
```

> [!IMPORTANT]
> La opción de línea de comandos **/c** es acumulativa. Si escribe **/c** más de una vez, cada entrada permanece. Para asegurarse de que solo se comprueba un volumen determinado, restablezca los valores predeterminados para borrar todos los comandos anteriores, excluir todos los volúmenes de la comprobación y, a continuación, programe la comprobación automática de archivos en el volumen deseado.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
