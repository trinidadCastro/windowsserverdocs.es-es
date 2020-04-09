---
title: shutdown
description: 'Comandos de Windows: tema de apagado, que permite apagar o reiniciar equipos locales o remotos de uno en uno.'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c432f5cf-c5aa-4665-83af-0ec52c87112e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 649695fb8ec936375057cf730eb215047c97e19e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834188"
---
# <a name="shutdown"></a>shutdown

Permite apagar o reiniciar los equipos locales o remotos de uno en uno.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
shutdown [/i | /l | /s | /r | /a | /p | /h | /e] [/f] [/m \\<ComputerName>] [/t <XXX>] [/d [p|u:]<XX>:<YY> [/c comment]] 
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/i|Muestra el cuadro de **diálogo apagado remoto** . La opción **/i** debe ser el primer parámetro que sigue al comando. Si se especifica **/i** , se omiten todas las demás opciones.|
|/l|Cierra la sesión del usuario actual de inmediato, sin período de tiempo de espera. No se puede usar **/l** con **/m** o **/t**.|
|/s|Apaga el equipo.|
|/r|Reinicia el equipo tras el apagado.|
|/a|Anula el cierre del sistema. Solo surte efecto durante el tiempo de espera. Para usar **/a**, también debe usar la opción **/m** .|
|/p|Desactiva solo el equipo local (no un equipo remoto), sin ningún período de tiempo de espera ni ADVERTENCIA. Puede usar **/p** solo con **/d** o **/f**. Si el equipo no admite la funcionalidad de apagado, se apagará cuando se use **/p**, pero la energía del equipo permanecerá activada.|
|/h|Pone el equipo local en hibernación si está habilitada la hibernación. Puede usar **/h** solo con **/f**.|
|/e|Permite documentar el motivo del apagado inesperado en el equipo de destino.|
|/f|Obliga a las aplicaciones en ejecución a cerrarse sin avisar a los usuarios.</br>PRECAUCIÓN: el uso de la opción **/f** podría provocar la pérdida de datos no guardados.|
|/m \\\\\<ComputerName >|Especifica el equipo de destino. No se puede usar con la opción **/l** .|
|/t \<XXX >|Establece el período de tiempo de espera o el retraso en *XXX* segundos antes de un reinicio o apagado. Esto hace que se muestre una advertencia en la consola local. Puede especificar 0-600 segundos. Si no usa **/t**, el período de tiempo de espera es de 30 segundos de forma predeterminada.|
|/d [p\|u:]\<XX >:\<YY >|Muestra el motivo del reinicio o apagado del sistema. Estos son los valores de parámetro:</br>**p** indica que el reinicio o el apagado están planeados.</br>**u** indica que el motivo es definido por el usuario.</br>Nota: Si no se especifican **p** o **u** , el reinicio o el apagado no están planeados.</br>*XX* especifica el número de motivo principal (entero positivo inferior a 256).</br>*AA* Especifica el número de motivo secundario (entero positivo inferior a 65536).|
|/c \<comentario >|Le permite comentar con detalle el motivo del apagado. Primero debe proporcionar un motivo mediante la opción **/d** . Los comentarios deben ir entre comillas. Puede utilizar 511 caracteres como máximo.|
|/?|Muestra la ayuda en el símbolo del sistema, incluida una lista de las razones principales y secundarias que se definen en el equipo local.|

## <a name="remarks"></a>Comentarios

-   Los usuarios deben tener asignado el derecho de usuario **apagar el sistema** para apagar un equipo administrado de forma remota o local que use el comando **Shutdown** .
-   Los usuarios deben ser miembros del grupo administradores para anotar un apagado inesperado de un equipo administrado de forma remota o local. Si el equipo de destino está unido a un dominio, los miembros del grupo Admins. del dominio podrían realizar este procedimiento. Para más información, consulta lo siguiente:  
    -   [Grupos locales predeterminados](https://technet.microsoft.com/library/cc785098(v=ws.10).aspx)
    -   [Grupos predeterminados](https://technet.microsoft.com/library/cc756898(v=ws.10).aspx)
-   Si desea apagar más de un equipo a la vez, puede llamar a **Shutdown** para cada equipo mediante un script, o bien puede usar **Shutdown** **/i** para mostrar el cuadro de diálogo apagado remoto.
-   Si especifica códigos de motivo principales y secundarios, primero debe definir estos códigos de motivo en cada equipo en el que vaya a usar los motivos. Si los códigos de motivo no están definidos en el equipo de destino, el rastreador de eventos de apagado no puede registrar el texto de motivo correcto.
-   Recuerde indicar que se ha planeado un apagado mediante el parámetro **p:** . La omisión de **p:** indica que un cierre no está planeado. Si escribe **p:** seguido del código de motivo para un apagado no planeado, el comando no llevará a cabo el cierre. Por el contrario, si se omite **p:** y se escribe el código de motivo para un cierre planeado, el comando no llevará a cabo el cierre.

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para obligar a las aplicaciones a cerrar y reiniciar el equipo local después de un retraso de un minuto con el motivo de la aplicación: mantenimiento (planeado) y el comentario volver a configurar el tipo MyApp. exe:
```
shutdown /r /t 60 /c Reconfiguring myapp.exe /f /d p:4:1
```
Para reiniciar el equipo remoto \\\\ServerName con los mismos parámetros, escriba:
```
shutdown /r /m \\servername /t 60 /c Reconfiguring myapp.exe /f /d p:4:1
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
