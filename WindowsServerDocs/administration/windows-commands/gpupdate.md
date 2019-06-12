---
title: gpupdate
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fd4e567-2ce1-4637-b611-c2f0895e5708
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dba8a0fb7d9a4e95f91ed1c1e140d965f5f9e2fb
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811118"
---
# <a name="gpupdate"></a>gpupdate

Actualiza la configuración de directiva de grupo. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#examples).

## <a name="syntax"></a>Sintaxis

```
gpupdate [/target:{Computer | User}] [/force] [/wait:<VALUE>] [/logoff] [/boot] [/sync] [/?]
```

### <a name="parameters"></a>Parámetros

|     Parámetro     |                                                                                                                                                                                                                                                                                                                             Descripción                                                                                                                                                                                                                                                                                                                             |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| / target: {Computer |                                                                                                                                                                                                                                                                                                                                Usuario}                                                                                                                                                                                                                                                                                                                                |
|      /force       |                                                                                                                                                                                                                                                                                   Vuelve a aplicar todas las configuraciones de directiva. De forma predeterminada, se aplica solo la configuración de directiva que ha cambiado.                                                                                                                                                                                                                                                                                    |
|  / wait:\<valor >   | Establece el número de segundos de espera para el procesamiento finalice antes de volver a la línea de comandos de directiva. Cuando se supera el límite de tiempo, aparece el símbolo del sistema, pero continúa el procesamiento de directiva. El valor predeterminado es 600 segundos. El valor **0** significa que no debe esperar. El valor **-1** significa que debe esperar indefinidamente.</br>En una secuencia de comandos, mediante el uso de este comando con un límite de tiempo especificado, puede ejecutar **gpupdate** y continúe con los comandos que no dependen de la finalización de **gpupdate**. Como alternativa, puede usar este comando sin límite de tiempo especificado para permitir que **gpupdate** termine de ejecutar antes de ejecutar otros comandos que dependen de él. |
|      /logoff      |                                                                                                                                   Hace que un cierre de sesión después de actualiza la configuración de directiva de grupo. Esto es necesario para las extensiones de lado cliente de directiva de grupo que no procesan la directiva en un ciclo de actualización en segundo plano, sino cuando un usuario inicia sesión. Algunos ejemplos son dirigidos a usuarios de instalación de Software y redirección de carpetas. Esta opción no tiene ningún efecto si se llama a extensiones que requieren un cierre de sesión.                                                                                                                                    |
|       /boot       |                                                                                                                                       Hace que un reinicio del equipo después de aplica la configuración de directiva de grupo. Esto es necesario para las extensiones del lado cliente de directiva de grupo que no procesan la directiva en un ciclo de actualización en segundo plano, sino la directiva al iniciar el equipo. Algunos ejemplos son la instalación de Software de equipo de destino. Esta opción no tiene ningún efecto si se llama a extensiones que requieren un reinicio.                                                                                                                                        |
|       /sync       |                                                                                                                                                                              Hace que la aplicación de directiva realizar de forma sincrónica. Se aplica la directiva de primer plano al inicio de sesión de usuario y de arranque del equipo. Puede especificar para el usuario, equipo o ambos, mediante la **/target** parámetro. El **/force** y **/wait** se omiten los parámetros si los especifica.                                                                                                                                                                               |
|        /?         |                                                                                                                                                                                                                                                                                                                Muestra la Ayuda en el símbolo del sistema.                                                                                                                                                                                                                                                                                                                 |

## <a name="remarks"></a>Comentarios

-   El **gpupdate** comando está disponible en Windows Server 2008 R2, Windows Server 2008, Windows 7 Ultimate, Windows 7 Professional, Windows Vista Ultimate, Windows Vista Enterprise y Windows Vista Business.

## <a name="examples"></a>Ejemplos

Forzar una actualización en segundo plano de todas las configuraciones de directiva de grupo, independientemente de si han cambiado.

```
gpupdate /force
```

#### <a name="additional-references"></a>Referencias adicionales

-   [TechCenter de directiva de grupo](https://go.microsoft.com/fwlink/?LinkID=145531)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)