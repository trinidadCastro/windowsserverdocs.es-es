---
title: shutdown
description: Artículo de referencia del comando shutdown, que permite apagar o reiniciar equipos locales o remotos, de uno en uno.
ms.topic: reference
ms.assetid: c432f5cf-c5aa-4665-83af-0ec52c87112e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 12138ffda4a08f8ac902dbf7c838f0a36627d3cf
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718312"
---
# <a name="shutdown"></a>shutdown

Permite apagar o reiniciar equipos locales o remotos, de uno en uno.

## <a name="syntax"></a>Sintaxis

```
shutdown [/i | /l | /s | /r | /a | /p | /h | /e] [/f] [/m \\<computername>] [/t <XXX>] [/d [p|u:]<XX>:<YY> [/c "comment"]]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /i | Muestra el cuadro de **apagado remoto** . La opción **/i** debe ser el primer parámetro que sigue al comando. Si se especifica **/i** , se omiten todas las demás opciones. |
| /l | Cierra la sesión del usuario actual de inmediato, sin período de tiempo de espera. No se puede usar **/l** con **/m** o **/t**. |
| /s | Apaga el equipo. |
| /r | Reinicia el equipo tras el apagado. |
| /a | Anula el cierre del sistema. Efectivo solo durante el período de tiempo de espera. Para usar **/a**, también debe usar la opción **/m** . |
| /p | Desactiva solo el equipo local (no un equipo remoto), sin ningún período de tiempo de espera ni ADVERTENCIA. Puede usar **/p** solo con **/d** o **/f**. Si el equipo no es compatible con la funcionalidad de apagado, se apagará cuando se use **/p**, pero la energía del equipo permanecerá activada. |
| /h | Pone el equipo local en hibernación si está habilitada la hibernación. Puede usar **/h** solo con **/f**. |
| /e | Permite documentar el motivo del apagado inesperado en el equipo de destino. |
| /f | Obliga a las aplicaciones en ejecución a cerrarse sin avisar a los usuarios.<br>**PRECAUCIÓN:** El uso de la opción **/f** podría provocar la pérdida de datos no guardados. |
| /m `\\<computername>` | Especifica el equipo de destino. No se puede usar con la opción **/l** . |
| /t `<n>` | Establece el período de tiempo de espera o el retraso en *n* segundos antes de un reinicio o apagado. Esto hace que se muestre una advertencia en la consola local. Puede especificar 0-600 segundos. Si no usa **/t**, el período de tiempo de espera es de 30 segundos de forma predeterminada. |
| /d. `[p | u:]<XX>:<YY>` | Muestra el motivo del reinicio o apagado del sistema. Los valores de parámetro admitidos son:<ul><li>**p** : indica que el reinicio o el apagado están planeados.</li><li>**u** : indica que el motivo es definido por el usuario.<p>**NOTA**:<br>Si no se especifican **p** o **u** , el reinicio o el apagado no están planeados.</li><li>*XX* : especifica el número de motivo principal (un entero positivo, menor que 256).</li><li>*AA* Especifica el número de motivo secundario (un entero positivo, menor que 65536).</li></ul> |
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
