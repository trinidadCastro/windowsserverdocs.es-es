---
title: gpupdate
description: Artículo de referencia del comando gpupdate, que actualiza la configuración directiva de grupo.
ms.topic: article
ms.assetid: 2fd4e567-2ce1-4637-b611-c2f0895e5708
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bc17be77c17ad45dfa1ce8d8d112f86a46f67262
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87888529"
---
# <a name="gpupdate"></a>gpupdate

Actualiza la configuración de directiva de grupo.

## <a name="syntax"></a>Sintaxis

```
gpupdate [/target:{computer | user}] [/force] [/wait:<VALUE>] [/logoff] [/boot] [/sync] [/?]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- |------------ |
| /target:`{computer|user}` | Especifica que solo se actualiza la configuración de directiva de equipo o usuario. De forma predeterminada, se actualiza la configuración de directiva de usuario y equipo. |
| /force | Vuelve a aplicar todas las configuraciones de directiva. De forma predeterminada, solo se aplican las configuraciones de directivas que han cambiado. |
| /Wait`<VALUE>` | Establece el número de segundos que se esperará a que finalice el procesamiento de la Directiva antes de volver al símbolo del sistema. Cuando se supera el límite de tiempo, aparece el símbolo del sistema, pero continúa el procesamiento de directivas. El valor predeterminado es 600 segundos. El valor **0** significa no esperar. El valor **-1** significa esperar indefinidamente.<p>En un script, con este comando con un límite de tiempo especificado, puede ejecutar **gpupdate** y continuar con los comandos que no dependen de la finalización de **gpupdate**. Como alternativa, puede usar este comando sin ningún límite de tiempo especificado para permitir que **gpupdate** termine de ejecutarse antes de que se ejecuten otros comandos que dependen de él. |
| /logoff | Provoca un cierre de sesión después de que se actualice la configuración de directiva de grupo. Esto es necesario para los directiva de grupo extensiones del lado cliente que no procesan la Directiva en un ciclo de actualización en segundo plano, pero que procesan la Directiva cuando un usuario inicia sesión. Entre los ejemplos se incluyen la instalación de software de destino del usuario y el redireccionamiento de carpetas. Esta opción no tiene ningún efecto si no hay extensiones llamadas que requieran un cierre de sesión. |
| /boot | Hace que se reinicie el equipo después de aplicar la configuración de directiva de grupo. Esto es necesario para los directiva de grupo extensiones del lado cliente que no procesan la Directiva en un ciclo de actualización en segundo plano, pero sí en la Directiva de procesos durante el inicio del equipo. Algunos ejemplos incluyen la instalación de software de destino del equipo. Esta opción no tiene ningún efecto si no hay extensiones llamadas que requieran un reinicio. |
| ocasiona | Hace que la siguiente aplicación de directiva de primer plano se realice sincrónicamente. La Directiva de primer plano se aplica durante el arranque del equipo y el inicio de sesión del usuario. Puede especificar esto para el usuario, el equipo o ambos mediante el parámetro **/target** . Los parámetros **/Force** y **/Wait** se omiten si se especifican. |
| /? | Muestra la Ayuda en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

Para forzar una actualización en segundo plano de todos los valores de directiva de grupo, independientemente de si han cambiado, escriba:

```
gpupdate /force
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)