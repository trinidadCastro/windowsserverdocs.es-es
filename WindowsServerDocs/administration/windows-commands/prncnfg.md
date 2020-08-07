---
title: prncnfg
description: Artículo de referencia del comando prncnfg, que configura o muestra información de configuración sobre una impresora.
ms.topic: article
ms.assetid: 38a4e8fa-3122-495b-a125-35b926bc6415
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 4f7329d4f5c7441232efffbc40dcc1177f083e1e
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87884762"
---
# <a name="prncnfg"></a>prncnfg

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Configura o muestra la información de configuración de una impresora. Este comando es un script de Visual Basic ubicado en el `%WINdir%\System32\printing_Admin_Scripts\<language>` directorio. Para usar este comando en un símbolo del sistema, escriba **cscript** seguido de la ruta de acceso completa al archivo prncnfg o cambie los directorios a la carpeta correspondiente. Por ejemplo: `cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prncnfg`.

## <a name="syntax"></a>Sintaxis

```
cscript prncnfg {-g | -t | -x | -?} [-S <Servername>] [-P <Printername>] [-z <newprintername>] [-u <Username>] [-w <password>] [-r <portname>] [-l <location>] [-h <sharename>] [-m <comment>] [-f <separatorfilename>] [-y <datatype>] [-st <starttime>] [-ut <untiltime>] [-i <defaultpriority>] [-o <priority>] [<+|->shared] [<+|->direct] [<+|->hidden] [<+|->published] [<+|->rawonly] [<+|->queued] [<+|->enablebidi] [<+|->keepprintedjobs] [<+|->workoffline] [<+|->enabledevq] [<+|->docompletefirst]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| -g | Muestra información de configuración acerca de una impresora. |
| -T | Configura una impresora. |
| -X | Cambia el nombre de una impresora. |
| -S`<Servername>` | Especifica el nombre del equipo remoto que hospeda la impresora que desea administrar. Si no especifica un equipo, se usa el equipo local. |
| -P`<Printername>` | Especifica el nombre de la impresora que desea administrar. Necesario. |
| -z`<newprintername>` | Especifica el nuevo nombre de la impresora. Requiere los parámetros **-x** y **-P** . |
| -u `<Username>` -w`<password>` | Especifica una cuenta con permisos para conectarse al equipo que hospeda la impresora que desea administrar. Todos los miembros del grupo de administradores locales del equipo de destino tienen estos permisos, pero también se pueden conceder los permisos a otros usuarios. Si no especifica una cuenta, debe iniciar sesión con una cuenta que tenga estos permisos para que el comando funcione. |
| -r`<portname>` | Especifica el puerto al que está conectada la impresora. Si se trata de un puerto paralelo o de serie, use el identificador del puerto (por ejemplo, LPT1 o COM1). Si se trata de un puerto TCP/IP, utilice el nombre de puerto que se especificó cuando se agregó el puerto. |
| -l`<location>` | Especifica la ubicación de la impresora, como **Copyroom**. Si la ubicación contiene espacios, utilice comillas alrededor del texto, como **"copiar salón"**.|
| -h`<sharename>` | Especifica el nombre del recurso compartido de la impresora. |
| -m `<comment>` | Especifica la cadena de comentario de la impresora. |
| -f`<separatorfilename>` | Especifica un archivo que contiene el texto que aparece en la página separador. |
| -y`<datatype>` | Especifica los tipos de datos que la impresora puede aceptar. |
| -St`<starttime>` | Configura la impresora para una disponibilidad limitada. Especifica la hora del día en la que la impresora está disponible. Si envía un documento a una impresora cuando no está disponible, el documento se mantiene (en cola) hasta que la impresora esté disponible. Debe especificar la hora como un reloj de 24 horas. Por ejemplo, para especificar 11:00 P.M., escriba **2300**. |
| -UT`<endtime>` | Configura la impresora para una disponibilidad limitada. Especifica la hora del día en que la impresora ya no está disponible. Si envía un documento a una impresora cuando no está disponible, el documento se mantiene (en cola) hasta que la impresora esté disponible. Debe especificar la hora como un reloj de 24 horas. Por ejemplo, para especificar 11:00 P.M., escriba **2300**. |
| -o`<priority>` | Especifica la prioridad que utiliza el administrador de trabajos de impresión para enrutar los trabajos de impresión en la cola de impresión. Una cola de impresión con una prioridad más alta recibe todos sus trabajos antes de cualquier cola con una prioridad más baja. |
| -i`<defaultpriority>` | Especifica la prioridad predeterminada asignada a cada trabajo de impresión. |
| `{+|-}`recurso | Especifica si esta impresora se comparte en la red. |
| `{+|-}`directo | Especifica si el documento se debe enviar directamente a la impresora sin ser puesto en cola. |
| `{+|-}`sin | Especifica si esta impresora debe publicarse en Active Directory. Si publica la impresora, otros usuarios pueden buscarla en función de su ubicación y capacidades (como la impresión en color y el grapado). |
| `{+|-}`plusvalía | Función reservada. |
| `{+|-}`rawonly | Especifica si solo se pueden poner en cola los trabajos de impresión de datos sin procesar en esta cola. |
| `{+|-}`} en cola | Especifica que la impresora no debe empezar a imprimir hasta que no se haya puesto en cola la última página del documento. El programa de impresión no está disponible hasta que el documento ha finalizado la impresión. Sin embargo, el uso de este parámetro garantiza que todo el documento esté disponible en la impresora. |
| `{+|-}`keepprintedjobs | Especifica si el administrador de trabajos de impresión debe conservar los documentos después de imprimirlos. Al habilitar esta opción, el usuario puede volver a enviar un documento a la impresora desde la cola de impresión en lugar de desde el programa de impresión. |
| `{+|-}`WorkOffline | Especifica si un usuario puede enviar trabajos de impresión a la cola de impresión si el equipo no está conectado a la red. |
| `{+|-}`enabledevq | Especifica si los trabajos de impresión que no coinciden con la configuración de la impresora (por ejemplo, archivos PostScript puestos en cola en impresoras que no son PostScript) se deben mantener en la cola en lugar de imprimirse. |
| `{+|-}`docompletefirst | Especifica si el administrador de trabajos de impresión debe enviar trabajos de impresión con una prioridad más baja que haya completado la puesta en cola antes de enviar trabajos de impresión con una prioridad más alta que no haya completado la puesta en cola. Si esta opción está habilitada y ningún documento ha completado la cola de impresión, el administrador de trabajos de impresión enviará documentos más grandes antes que los más pequeños. Debe habilitar esta opción si desea maximizar la eficacia de la impresora a costa de la prioridad del trabajo. Si esta opción está deshabilitada, el administrador de trabajos de impresión siempre envía primero los trabajos de mayor prioridad a sus respectivas colas. |
| `{+|-}`enablebidi | Especifica si la impresora envía información de estado al administrador de trabajos de impresión. |
| /? | Muestra la ayuda en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

Para mostrar la información de configuración de la impresora llamada *colorprinter_2* con una cola de impresión hospedada por el equipo remoto llamado *ServidorRH*, escriba:

```
cscript prncnfg -g -S HRServer -P colorprinter_2
```

Para configurar una impresora llamada *colorprinter_2* de modo que el administrador de trabajos de impresión en el equipo remoto llamado *ServidorRH* Mantenga los trabajos de impresión una vez que se hayan impreso, escriba:

```
cscript prncnfg -t -S HRServer -P colorprinter_2 +keepprintedjobs
```

Para cambiar el nombre de una impresora en el equipo remoto llamado *ServidorRH* desde *colorprinter_2* a *colorprinter 3*, escriba:

```
cscript prncnfg -x -S HRServer -P colorprinter_2 -z "colorprinter 3"
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Referencia de comandos de impresión](print-command-reference.md)
