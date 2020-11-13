---
title: shutdown
description: Artículo de referencia del comando shutdown, que permite apagar o reiniciar equipos locales o remotos, de uno en uno.
ms.topic: reference
ms.assetid: c432f5cf-c5aa-4665-83af-0ec52c87112e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e8bd887796dd69645a4238fbbe9316ab6aba8b9b
ms.sourcegitcommit: 6a245fefdf958bfc0aeb69f7a887d11a07bdcd23
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94570300"
---
# <a name="shutdown"></a>shutdown

Permite apagar o reiniciar equipos locales o remotos, de uno en uno.

## <a name="syntax"></a>Sintaxis

```
shutdown [/i | /l | /s | /sg | /r | /g | /a | /p | /h | /e | /o] [/hybrid] [/fw] [/f] [/m \\computer][/t xxx][/d [p|u:]xx:yy [/c "comment"]]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /i | Muestra el cuadro de **apagado remoto** . La opción **/i** debe ser el primer parámetro que sigue al comando. Si se especifica **/i** , se omiten todas las demás opciones. |
| /l | Cierra la sesión del usuario actual de inmediato, sin período de tiempo de espera. No se puede usar **/l** con **/m** o **/t**. |
| /s | Apaga el equipo. |
| /SG | Apaga el equipo. En el siguiente arranque, si el inicio de **sesión de reinicio automático** está habilitado, el dispositivo inicia sesión automáticamente y se bloquea según el último usuario interactivo. Después de iniciar sesión, se reinician las aplicaciones registradas. |
| /r | Reinicia el equipo tras el apagado. |
| /g | Apaga el equipo. En el siguiente reinicio, si el **Inicio de sesión de reinicio automático** está habilitado, el dispositivo inicia sesión automáticamente y se bloquea según el último usuario interactivo. Después de iniciar sesión, se reinician las aplicaciones registradas. |
| /a | Anula el cierre del sistema. Efectivo solo durante el período de tiempo de espera. Para usar **/a** , también debe usar la opción **/m** . |
| /p | Desactiva solo el equipo local (no un equipo remoto), sin ningún período de tiempo de espera ni ADVERTENCIA. Puede usar **/p** solo con **/d** o **/f**. Si el equipo no es compatible con la funcionalidad de apagado, se apagará cuando se use **/p** , pero la energía del equipo permanecerá activada. |
| /h | Pone el equipo local en hibernación si está habilitada la hibernación. Puede usar **/h** solo con **/f**. |
| híbrido | Apaga el dispositivo y lo prepara para el inicio rápido. Esta opción debe usarse con la opción **/s** . |
| /fw | La combinación de esta opción con una opción Shutdown hace que el siguiente reinicio vaya a la interfaz de usuario firmware. |
| /e | Permite documentar el motivo del apagado inesperado en el equipo de destino. |
| /o | Va al menú **Opciones de arranque avanzadas** y reinicia el dispositivo. Esta opción debe usarse con la opción **/r** . |
| /f | Obliga a las aplicaciones en ejecución a cerrarse sin avisar a los usuarios.<br>**PRECAUCIÓN:** El uso de la opción **/f** podría provocar la pérdida de datos no guardados. |
| /m `\\<computername>` | Especifica el equipo de destino. No se puede usar con la opción **/l** . |
| /t `<xxx>` | Establece el período de tiempo de espera antes del apagado en *XXX* segundos. El intervalo válido es 0-315360000 (10 años) y su valor predeterminado es 30. Si el período de tiempo de espera es mayor que 0, el parámetro **/f** es implícito. |
| /d. `[p | u:]<XX>:<YY>` | Muestra el motivo del reinicio o apagado del sistema. Los valores de parámetro admitidos son:<ul><li>**p** : indica que el reinicio o el apagado están planeados.</li><li>**u** : indica que el motivo es definido por el usuario.<p>**NOTA** :<br>Si no se especifican **p** o **u** , el reinicio o el apagado no están planeados.</li><li>*XX* : especifica el número de motivo principal (un entero positivo, menor que 256).</li><li>*AA* Especifica el número de motivo secundario (un entero positivo, menor que 65536).</li></ul> |
| /c `<comment>` | Le permite comentar con detalle el motivo del apagado. Primero debe proporcionar un motivo mediante la opción **/d** y debe incluir los comentarios entre comillas. Puede utilizar 511 caracteres como máximo. |
| /? | Muestra la ayuda en el símbolo del sistema, incluida una lista de las razones principales y secundarias que se definen en el equipo local. |

#### <a name="remarks"></a>Observaciones

- Los usuarios deben tener asignado el derecho de usuario **apagar el sistema** para apagar un equipo administrado de forma remota o local que use el comando **Shutdown** .

- Los usuarios deben ser miembros del grupo **administradores** para anotar un apagado inesperado de un equipo administrado de forma remota o local. Si el equipo de destino está unido a un dominio, los miembros del grupo **Admins** . del dominio podrían realizar este procedimiento. Para más información, consulte:

  - [Grupos locales predeterminados](/previous-versions/windows/it-pro/windows-server-2003/cc785098(v=ws.10))

  - [Grupos predeterminados](/previous-versions/windows/it-pro/windows-server-2003/cc756898(v=ws.10))

- Si desea apagar más de un equipo a la vez, puede llamar a **Shutdown** para cada equipo mediante un script, o bien puede usar **Shutdown** **/i** para mostrar el cuadro de **apagado remoto** .

- Si especifica códigos de motivo principales y secundarios, primero debe definir estos códigos de motivo en cada equipo en el que vaya a usar los motivos. Si los códigos de motivo no están definidos en el equipo de destino, el rastreador de eventos de apagado no puede registrar el texto de motivo correcto.

- Recuerde indicar que se ha planeado un apagado mediante el parámetro **p** . Si no se usa el parámetro **p** , indica que no se ha planeado el cierre.

  - Con el parámetro **p** , a lo largo del código de motivo de un apagado no planeado, se produce un error en el cierre.

  - Si no se usa el parámetro **p** y solo se proporciona el código de motivo para un apagado planeado, también se produce un error en el cierre

## <a name="examples"></a>Ejemplos

Para forzar el cierre de las aplicaciones y reiniciar el equipo local después de un retraso de un minuto, con el motivo *aplicación: mantenimiento (planeado)* y el comentario "reconfigurando myapp.exe", escriba:

```
shutdown /r /t 60 /c "Reconfiguring myapp.exe" /f /d p:4:1
```

Para reiniciar el equipo remoto *myremoteserver* con los mismos parámetros que en el ejemplo anterior, escriba:

```
shutdown /r /m \\myremoteserver /t 60 /c "Reconfiguring myapp.exe" /f /d p:4:1
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
