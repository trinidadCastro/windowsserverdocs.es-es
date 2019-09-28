---
title: gpupdate
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 994e37ebd972d881e06bdb99d5256e75096ccd81
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375645"
---
# <a name="gpupdate"></a>gpupdate

Actualiza la configuración de directiva de grupo. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#examples).

## <a name="syntax"></a>Sintaxis

```
gpupdate [/target:{Computer | User}] [/force] [/wait:<VALUE>] [/logoff] [/boot] [/sync] [/?]
```

### <a name="parameters"></a>Parámetros

|     Parámetro     |                                                                                                                                                                                                                                                                                                                             Descripción                                                                                                                                                                                                                                                                                                                             |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /target: {equipo |                                                                                                                                                                                                                                                                                                                                Usuario                                                                                                                                                                                                                                                                                                                                |
|      /Force.       |                                                                                                                                                                                                                                                                                   Vuelve a aplicar todas las configuraciones de directiva. De forma predeterminada, solo se aplican las configuraciones de directivas que han cambiado.                                                                                                                                                                                                                                                                                    |
|  /Wait: @no__t 0VALUE >   | Establece el número de segundos que se esperará a que finalice el procesamiento de la Directiva antes de volver al símbolo del sistema. Cuando se supera el límite de tiempo, aparece el símbolo del sistema, pero continúa el procesamiento de directivas. El valor predeterminado es 600 segundos. El valor **0** significa no esperar. El valor **-1** significa esperar indefinidamente.</br>En un script, con este comando con un límite de tiempo especificado, puede ejecutar **gpupdate** y continuar con los comandos que no dependen de la finalización de **gpupdate**. Como alternativa, puede usar este comando sin ningún límite de tiempo especificado para permitir que **gpupdate** termine de ejecutarse antes de que se ejecuten otros comandos que dependen de él. |
|      /logoff      |                                                                                                                                   Provoca un cierre de sesión después de que se actualice la configuración de directiva de grupo. Esto es necesario para los directiva de grupo extensiones del lado cliente que no procesan la Directiva en un ciclo de actualización en segundo plano, pero que procesan la Directiva cuando un usuario inicia sesión. Entre los ejemplos se incluyen la instalación de software de destino del usuario y el redireccionamiento de carpetas. Esta opción no tiene ningún efecto si no hay extensiones llamadas que requieran un cierre de sesión.                                                                                                                                    |
|       /boot       |                                                                                                                                       Hace que se reinicie el equipo después de aplicar la configuración de directiva de grupo. Esto es necesario para los directiva de grupo extensiones del lado cliente que no procesan la Directiva en un ciclo de actualización en segundo plano, pero sí en la Directiva de procesos durante el inicio del equipo. Algunos ejemplos incluyen la instalación de software de destino del equipo. Esta opción no tiene ningún efecto si no hay extensiones llamadas que requieran un reinicio.                                                                                                                                        |
|       ocasiona       |                                                                                                                                                                              Hace que la siguiente aplicación de directiva de primer plano se realice sincrónicamente. La Directiva de primer plano se aplica durante el arranque del equipo y el inicio de sesión del usuario. Puede especificar esto para el usuario, el equipo o ambos mediante el parámetro **/target** . Los parámetros **/Force** y **/Wait** se omiten si se especifican.                                                                                                                                                                               |
|        /?         |                                                                                                                                                                                                                                                                                                                Muestra la Ayuda en el símbolo del sistema.                                                                                                                                                                                                                                                                                                                 |

## <a name="remarks"></a>Comentarios

-   El comando **gpupdate** está disponible en windows Server 2008 R2, windows Server 2008, Windows 7 Ultimate, Windows 7 Professional, Windows Vista Ultimate, Windows Vista Enterprise y Windows Vista Business.

## <a name="examples"></a>Ejemplos

Forzar una actualización en segundo plano de todas las configuraciones de directiva de grupo, independientemente de si han cambiado.

```
gpupdate /force
```

#### <a name="additional-references"></a>Referencias adicionales

-   [TechCenter de directiva de grupo](https://go.microsoft.com/fwlink/?LinkID=145531)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)