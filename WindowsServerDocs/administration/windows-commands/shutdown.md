---
title: shutdown
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c432f5cf-c5aa-4665-83af-0ec52c87112e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5d03a8d35f3e56ec7829bc51c1499fddd13b86e8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837256"
---
# <a name="shutdown"></a>shutdown



Permite apagar o reiniciar los equipos locales o remotos de uno en uno.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
shutdown [/i | /l | /s | /r | /a | /p | /h | /e] [/f] [/m \\<ComputerName>] [/t <XXX>] [/d [p|u:]<XX>:<YY> [/c "comment"]] 
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/i|Muestra el **diálogo de apagado remoto** cuadro. El **/i** opción debe ser el primer parámetro después del comando. Si **/i** se especifica, se omiten todas las demás opciones.|
|/l|Cierra la sesión del usuario actual inmediatamente, con ningún período de tiempo de espera. No puede usar **/l** con **/m** o **/t**.|
|/s|Apaga el equipo.|
|/r|Reinicia el equipo tras el apagado.|
|/a|Anula un cierre del sistema. A partir del solo durante el período de tiempo de espera. Para usar **/a**, también debe usar el **/m** opción.|
|/p|Apaga el equipo local solo (no en un equipo remoto), sin tiempo de espera o advertencia. Puede usar **/p** sólo con **/d** o **/f**. Si el equipo no admite la funcionalidad de apagado, cerrará cuando se usa **/p**, pero la eficacia en el equipo permanecerá en.|
|/h|Coloca el equipo local en hibernación, si está habilitado el modo de hibernación. Puede usar **/h** sólo con **/f**.|
|/e|Le permite documentar el motivo del apagado inesperado en el equipo de destino.|
|/f|Obliga a las aplicaciones se cierren sin avisar a los usuarios de ejecución.</br>Precaución: Mediante el **/f** opción podría provocar la pérdida de datos no guardados.|
|/m \\ \\ \<nombreDeEquipo >|Especifica el equipo de destino. No se puede usar con el **/l** opción.|
|/t \<XXX>|Establece el período de tiempo de espera o retraso a *XXX* segundos antes de un reinicio o apagado. Esto hace que una advertencia que se muestra en la consola local. Puede especificar 0-600 segundos. Si no usas **/t**, el tiempo de espera es de 30 segundos de forma predeterminada.|
|/d [p\|u:]\<XX>:\<YY>|Indica el motivo del reinicio del sistema o el apagado. Los siguientes son los valores de parámetro:</br>**p** indica que el reinicio o apagado es planeado.</br>**u** indica que el motivo es definido por el usuario.</br>Nota: Si **p** o **u** no se especifican, el reinicio o apagado no estuviera planeado.</br>*XX* especifica el número correspondiente al motivo principal (número entero positivo inferior a 256).</br>*YY* especifica el número correspondiente al motivo secundario (número entero positivo menor que 65536).|
|/c "\<comentario >"|Le permite comentar con detalle el motivo del apagado. En primer lugar, debe proporcionar un motivo mediante la **/d** opción. Debe incluir comentarios entre comillas. Puede utilizar 511 caracteres como máximo.|
|/?|Muestra la Ayuda en el símbolo del sistema, incluida una lista de las razones principales y secundarias que se definen en el equipo local.|

## <a name="remarks"></a>Comentarios

-   Los usuarios deben asignarse el **apagar el sistema** derecho de usuario para que se cierre hacia abajo de una variable local o remota administra el equipo que está usando el **apagado** comando.
-   Los usuarios deben ser miembros del grupo Administradores para anotar un apagado inesperado de un equipo administrado de forma remota o local. Si el equipo de destino está unido a un dominio, los miembros del grupo Admins. del dominio podrían llevar a cabo este procedimiento. Para obtener más información, vea:  
    -   [Grupos locales predeterminados](https://technet.microsoft.com/library/cc785098(v=ws.10).aspx)
    -   [Grupos predeterminados](https://technet.microsoft.com/library/cc756898(v=ws.10).aspx)
-   Si desea apagar más de un equipo a la vez, puede llamar a **apagado** para cada equipo usando una secuencia de comandos, o bien puede usar **apagado** **/i** para mostrar el control remoto Cuadro de diálogo de apagado.
-   Si especifica los códigos de motivo principal y secundaria, primero debe definir estos códigos de motivo en cada equipo donde va a usar los motivos. Si no se definen los códigos de motivo en el equipo de destino, el Rastreador de eventos no se puede registrar el texto de motivo correcto.
-   Recuerde que indicar que un apagado es planeado mediante el uso de la **p:** parámetro. Si se omite **p:** indica que un cierre imprevisto. Si escribe **p:** seguido por el código de motivo de un apagado inesperado, el comando no llevará a cabo el apagado. Por el contrario, si se omite **p:** y tipo en el código de motivo de un cierre planeado, el comando no llevará a cabo el apagado.

## <a name="BKMK_examples"></a>Ejemplos

Para forzar que las aplicaciones que cerrar y reiniciar el equipo local después de un retraso de un minuto con el motivo "aplicación: Mantenimiento (planeado) "y el comentario"Volver a configurar myapp.exe"tipo:
```
shutdown /r /t 60 /c "Reconfiguring myapp.exe" /f /d p:4:1
```
Para reiniciar el equipo remoto \\ \\ServerName con los mismos parámetros, escriba:
```
shutdown /r /m \\servername /t 60 /c "Reconfiguring myapp.exe" /f /d p:4:1
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
