---
title: prncnfg
description: Obtenga información sobre cómo configurar una impresora con el comando prncfg.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 38a4e8fa-3122-495b-a125-35b926bc6415
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 6426b053a5c56918768f82cbd7631631abcf0a6f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859156"
---
# <a name="prncnfg"></a>prncnfg

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configura o muestra información acerca de una impresora.

## <a name="syntax"></a>Sintaxis
```
cscript Prncnfg {-g | -t | -x | -?} [-S <ServerName>] [-P <printerName>] [-z <NewprinterName>] [-u <UserName>] [-w <Password>] [-r <PortName>] [-l <Location>] [-h <Sharename>] [-m <Comment>] [-f <SeparatorFileName>] [-y <Datatype>] [-st <starttime>] [-ut <Untiltime>] [-i <DefaultPriority>] [-o <Priority>] [<+|->shared] [<+|->direct] [<+|->hidden] [<+|->published] [<+|->rawonly] [<+|->queued] [<+|->enablebidi] [<+|->keepprintedjobs] [<+|->workoffline] [<+|->enabledevq] [<+|->docompletefirst]
```

## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|-g|Muestra información acerca de una impresora.|
|-t|Configura una impresora.|
|-x|cambia el nombre de una impresora.|
|-S \<ServerName\>|Especifica el nombre del equipo remoto que hospeda la impresora que desea administrar. Si no especifica un equipo, se usa el equipo local.|
|-P \<printerName\>|Especifica el nombre de la impresora que desea administrar. Obligatorio.|
|-z \<NewprinterName\>|Especifica el nuevo nombre de la impresora. Requiere el **- x** y **-P** parámetros.|
|-u \<UserName\> -w \<contraseña\>|Especifica una cuenta con permisos para conectarse al equipo que hospeda la impresora que desea administrar. Todos los miembros del grupo de administradores local del equipo de destino tienen estos permisos, pero también se pueden conceder los permisos a otros usuarios. Si no especifica una cuenta, debe haber iniciado sesión con una cuenta con estos permisos para que funcione el comando.|
|-r \<PortName\>|Especifica el puerto que está conectada la impresora. Si se trata de un paralelo o un puerto serie, a continuación, use el identificador del puerto (por ejemplo, LPT1 o COM1). Si se trata de un puerto TCP/IP, utilice el nombre del puerto que especificó cuando agregó el puerto.|
|-l \<ubicación\>|Especifica la ubicación de la impresora, como "Copiar sala".|
|-h \<Sharename\>|Especifica el nombre del recurso compartido de la impresora.|
|-m \<comentario\>|Especifica la cadena de comentario de la impresora.|
|-f \<SeparatorFileName\>|Especifica un archivo que contiene el texto que aparece en la página de separación.|
|-y \<Datatype\>|Especifica los tipos de datos que puede aceptar la impresora.|
|st - \<starttime\>|Configura la impresora para una disponibilidad limitada. Especifica la hora del día de que la impresora está disponible. Si envía un documento a una impresora cuando no está disponible, el documento se detiene (en cola) hasta que la impresora esté disponible. Debe especificar la hora como un reloj de 24 horas. Por ejemplo, para especificar las 11:00 P.M., escriba **2300**.|
|-ut \<Endtime\>|Configura la impresora para una disponibilidad limitada. Especifica la hora del día de que la impresora ya no está disponible. Si envía un documento a una impresora cuando no está disponible, el documento se detiene (en cola) hasta que la impresora esté disponible. Debe especificar la hora como un reloj de 24 horas. Por ejemplo, para especificar las 11:00 P.M., escriba **2300**.|
|-o \<prioridad\>|Especifica la prioridad que la cola de impresión se utiliza para enrutar los trabajos de impresión en la cola de impresión. Una cola de impresión con una prioridad más alta recibe todos sus trabajos antes de cualquier cola con una prioridad más baja.|
|-i \<DefaultPriority\>|Especifica la prioridad predeterminada asignada a cada trabajo de impresión.|
|{+&#124;-} compartido|Especifica si se comparte esta impresora en la red.|
|{+&#124;-} directa|Especifica si el documento debe enviarse directamente a la impresora sin que se coloca en la cola.|
|{+&#124;-} publicado|Especifica si se debe publicar esta impresora en active directory. Si publica la impresora, pueden buscar otros usuarios según su ubicación y funciones (como la impresión en color y grapado).|
|{+&#124;-} ocultos|Función reservado.|
|{+&#124;-} rawonly|Especifica si se pueden poner en cola solo datos sin procesar trabajos de impresión en esta cola.|
|{+ &#124; -}queued|Especifica que no debe empezar a la impresora imprimir hasta después de poner en cola la última página del documento. El programa de impresión no está disponible hasta que ha terminado de imprimir el documento. Sin embargo, con este parámetro garantiza que todo el documento está disponible para la impresora.|
|{+ &#124; -}keepprintedjobs|Especifica si la cola de impresión debe retener los documentos una vez que se imprimen. Al habilitar esta opción permite que un usuario a volver a enviar un documento a la impresora desde la cola de impresión en lugar de desde el programa de impresión.|
|{+ &#124; -} workoffline|Especifica si un usuario es capaz de enviar trabajos de impresión a la cola de impresión si el equipo no está conectado a la red.|
|{+ &#124; -}enabledevq|Especifica si deben mantenerse en la cola de trabajos de impresión que no coinciden con la configuración de la impresora (por ejemplo, archivos PostScript enviados a impresoras no PostScript) en lugar de que se va a imprimir.|
|{+ &#124; -}docompletefirst|Especifica si la cola de impresión debe enviar los trabajos de impresión con una prioridad más baja que se han completado la puesta en cola antes de enviar trabajos de impresión con una prioridad más alta que no se han completado la puesta en cola. Si esta opción está habilitada y documentos no han completado la puesta en cola, la cola de impresión enviará los documentos más grandes antes de otros más pequeños. Debe habilitar esta opción si desea maximizar la eficacia de la impresora a costa de la prioridad del trabajo. Si esta opción está deshabilitada, la cola de impresión siempre envía trabajos con prioridad superior a sus respectivas colas en primer lugar.|
|{+ &#124; -}enablebidi|Especifica si la impresora envía información de estado a la cola de impresión.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios
-   El **prncnfg** comando es un script de Visual Basic que se encuentra en la %WINdir%\System32\printing_Admin_Scripts\\ <language> directory. Para usar este comando, en un símbolo del sistema, escriba **cscript** seguido por la ruta de acceso completa al archivo prncnfg o cambie los directorios a la carpeta correspondiente. Por ejemplo:
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prncnfg
    ```
-   Si la información que se proporciona contiene espacios, utilice comillas alrededor del texto (por ejemplo, `"computer Name"`).

## <a name="BKMK_examples"></a>Ejemplos
Para mostrar información de configuración de la impresora llamada ImpresoraColor_2 con una cola de impresión hospedada por el equipo remoto ServidorRH, escriba:
```
cscript prncnfg -g -S HRServer -P colorprinter_2 
```

Para configurar una impresora llamada ImpresoraColor_2 para que la cola de impresión en el equipo remoto ServidorRH conserva los trabajos de impresión después de que se impriman, escriba:
```
cscript prncnfg -t -S HRServer -P colorprinter_2 +keepprintedjobs 
```

Para cambiar el nombre de una impresora en el equipo remoto ServidorRH de ImpresoraColor_2 a ImpresoraColor 3, tipo:
```
cscript prncnfg -x -S HRServer -P colorprinter_2 -z "colorprinter 3" 
```

#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[referencia de comandos de impresión](print-command-reference.md)
