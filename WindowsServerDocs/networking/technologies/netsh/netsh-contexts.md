---
title: Sintaxis, contextos y formatos de comandos netsh
description: Puedes usar este tema para aprender cómo especificar contextos y subcontextos de netsh, comprender la sintaxis de netsh y el formato de los comandos, y cómo ejecutar comandos de netsh en equipos locales y remotos que ejecutan Windows Server 2016 o Windows 10.
ms.topic: article
ms.assetid: 8cb9b59f-0255-4261-b49a-562c5ea50ee0
manager: brianlic
ms.author: lizross
author: eross-msft
ms.openlocfilehash: f72d3dfc3cd6f54b123cb00baf9ba75e4faeb906
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969472"
---
# <a name="netsh-command-syntax-contexts-and-formatting"></a>Sintaxis, contextos y formatos de comandos netsh

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puedes usar este tema para aprender cómo especificar contextos y subcontextos de netsh, comprender la sintaxis de netsh y el formato de los comandos, y cómo ejecutar comandos netsh en equipos locales y remotos.

Netsh es una utilidad de scripting de línea de comandos que permite mostrar o modificar la configuración de red de un equipo actualmente en ejecución. Los comandos netsh se pueden ejecutar escribiendo comandos en el símbolo del sistema de netsh y se pueden usar en archivos por lotes o scripts. Los equipos remotos y locales se pueden configurar mediante los comandos netsh.

Netsh también proporciona una característica de scripting que permite ejecutar un grupo de comandos en modo de lotes con un equipo especificado. Con netsh se puede guardar un script de configuración en un archivo de texto para archivarlo y así configurar otros equipos.

## <a name="netsh-contexts"></a>Contextos de Netsh

Netsh interactúa con otros componentes del sistema operativo mediante archivos de biblioteca de vínculos dinámicos \(DLL\).

Cada archivo DLL de la aplicación auxiliar netsh proporciona un amplio conjunto de características denominado *contexto*, que es un grupo de comandos específicos de un rol o característica del servidor de red. Estos contextos amplían la funcionalidad de netsh al proporcionar compatibilidad con la configuración y la supervisión de uno o varios servicios, utilidades o protocolos. Por ejemplo, Dhcpmon.dll da a netsh el contexto y el conjunto de comandos necesarios para configurar y administrar los servidores DHCP.

### <a name="obtain-a-list-of-contexts"></a>Obtención de una lista de contextos

Para obtener una lista de contextos de netsh, puedes abrir el símbolo del sistema o Windows PowerShell en un equipo que ejecute Windows Server 2016 o Windows 10. Escribe el comando **netsh** y presiona ENTRAR. Escribe **/?** y presiona ENTRAR.

A continuación tienes una salida de ejemplo para estos comandos en un equipo que ejecuta Windows Server 2016 Datacenter.

>    ```
>   PS C:\Windows\system32> netsh
>   netsh>/?
>
>    The following commands are available:
>
>    Commands in this context:
>    ..            - Goes up one context level.
>    ?             - Displays a list of commands.
>    abort         - Discards changes made while in offline mode.
>    add           - Adds a configuration entry to a list of entries.
>    advfirewall   - Changes to the `netsh advfirewall' context.
>    alias         - Adds an alias.
>    branchcache   - Changes to the `netsh branchcache' context.
>    bridge        - Changes to the `netsh bridge' context.
>    bye           - Exits the program.
>    commit        - Commits changes made while in offline mode.
>    delete        - Deletes a configuration entry from a list of entries.
>    dhcpclient    - Changes to the `netsh dhcpclient' context.
>    dnsclient     - Changes to the `netsh dnsclient' context.
>    dump          - Displays a configuration script.
>    exec          - Runs a script file.
>    exit          - Exits the program.
>    firewall      - Changes to the `netsh firewall' context.
>    help          - Displays a list of commands.
>    http          - Changes to the `netsh http' context.
>    interface     - Changes to the `netsh interface' context.
>    ipsec         - Changes to the `netsh ipsec' context.
>    ipsecdosprotection - Changes to the `netsh ipsecdosprotection' context.
>    lan           - Changes to the `netsh lan' context.
>    namespace     - Changes to the `netsh namespace' context.
>    netio         - Changes to the `netsh netio' context.
>    offline       - Sets the current mode to offline.
>    online        - Sets the current mode to online.
>    popd          - Pops a context from the stack.
>    pushd         - Pushes current context on stack.
>    quit          - Exits the program.
>    ras           - Changes to the `netsh ras' context.
>    rpc           - Changes to the `netsh rpc' context.
>    set           - Updates configuration settings.
>    show          - Displays information.
>    trace         - Changes to the `netsh trace' context.
>    unalias       - Deletes an alias.
>    wfp           - Changes to the `netsh wfp' context.
>    winhttp       - Changes to the `netsh winhttp' context.
>    winsock       - Changes to the `netsh winsock' context.
>
>    The following sub-contexts are available:
>     advfirewall branchcache bridge dhcpclient dnsclient firewall http interface ipsec ipsecdosprotection lan namespace netio ras rpc trace wfp winhttp winsock
>
>    To view help for a command, type the command, followed by a space, and then type ?.
>    ```

### <a name="subcontexts"></a>Subcontextos

Los contextos netsh pueden contener comandos y contextos adicionales, denominados *subcontextos*. Por ejemplo, en el contexto Routing, puedes cambiar a los subcontextos IP e IPv6.

Para mostrar una lista de los comandos y subcontextos que puedes usar en un contexto, en el símbolo del sistema de netsh, escribe el nombre del contexto y, luego, escribe **/?** o **help**. Por ejemplo, para mostrar una lista de subcontextos y comandos que puedes usar en el contexto Routing, en el símbolo del sistema de netsh \(es decir, **netsh&gt;** \), escribe uno de los siguientes:

**routing /?**

**routing help**

Para realizar tareas en otro contexto sin cambiar el contexto actual, escribe la ruta de acceso de contexto del comando que quieres usar en el símbolo del sistema de netsh. Por ejemplo, para agregar una interfaz denominada "Conexión de área local" en el contexto de IGMP sin cambiar primero al contexto de IGMP, en el símbolo del sistema de Netsh, escribe:

**routing ip igmp add interface "Conexión de área local" startupqueryinterval=21**

## <a name="running-netsh-commands"></a>Ejecución de comandos netsh

Para ejecutar un comando netsh, debes iniciar netsh desde el símbolo del sistema; para ello, escribe **netsh** y presiona Entrar. Luego, puedes cambiar al contexto que contiene el comando que quieres usar. Los contextos que están disponibles dependen de los componentes de red que hayas instalado. Por ejemplo, si escribes **dhcp** en el símbolo del sistema de netsh y presionas Entrar, netsh cambia al contexto del servidor DHCP. Sin embargo, si no tienes DHCP instalado, aparece el siguiente mensaje:

**No se encuentra el comando: dhcp.**

## <a name="formatting-legend"></a>Leyenda de formato

Puedes usar la siguiente leyenda de formato para interpretar y usar la sintaxis de comandos netsh correcta al ejecutar el comando en el símbolo del sistema de netsh o en un archivo por lotes o script.

- El texto en *cursiva* es información que debes proporcionar mientras escribes el comando. Por ejemplo, si un comando tiene un parámetro denominado -*NombreDeUsuario*, tienes que escribir el nombre de usuario real.
- El texto en **negrita** es información que debes escribir tal cual se muestra mientras escribes el comando.
- El texto seguido de tres puntos \(…\) es un parámetro que se puede repetir varias veces en una línea de comandos.
- El texto entre corchetes [&nbsp;] es un elemento opcional.
- El texto entre llaves {&nbsp;} con opciones separadas por una barra vertical proporciona un conjunto de opciones de las que debes seleccionar solo una, como `{enable|disable}`.
- El texto con formato de fuente Courier es código o la salida del programa.

## <a name="running-netsh-commands-from-the-command-prompt-or-windows-powershell"></a>Ejecución de comandos netsh desde el símbolo del sistema o Windows PowerShell

Para iniciar el shell de red y escribir netsh en el símbolo del sistema o en Windows PowerShell, puedes usar el siguiente comando.

### <a name="netsh"></a>netsh

Netsh es una utilidad de scripting de línea de comandos que permite mostrar o modificar, local o remotamente, la configuración de red de un equipo actualmente en ejecución. Si se usa sin parámetros, **netsh** abre el símbolo del sistema Netsh.exe \(es decir, **netsh&gt;** \).

#### <a name="syntax"></a>Sintaxis

**netsh**\[ **-a**&nbsp;*ArchivoDeAlias*\] \[ **-c**&nbsp;*Contexto* \] \[ **-r**&nbsp;*EquipoRemoto*\] \[ **-u** \[ *NombreDeDominio\\* \] *NombreDeUsuario* \] \[ **-p**&nbsp;*Contraseña* | \*\] \[{*ComandoNetsh* |  **-f**&nbsp;*ArchivoDeScript*}\]

##### <a name="parameters"></a>Parámetros

**`-a`**

Opcional. Especifica que se te devuelve al símbolo del sistema **netsh** después de ejecutar *ArchivoDeAlias*.

**`AliasFile`**

Opcional. Especifica el nombre del archivo de texto que contiene uno o más comandos **netsh**.

**`-c`**

Opcional. Especifica que netsh entra en el contexto de **netsh** especificado.

**`Context`**

Opcional. Especifica el contexto de **netsh** que quieres especificar.

**`-r`**

Opcional. Especifica que quieres que el comando se ejecute en un equipo remoto.

> [!IMPORTANT]
> Si usas algunos comandos netsh de forma remota en otro equipo con el parámetro **netsh –r**, el servicio Registro remoto debe estar en ejecución en el equipo remoto. Si no está en ejecución, Windows muestra un mensaje de error "No se encontró la ruta de red".

***`RemoteComputer`***

Opcional. Especifica el equipo remoto que quieres configurar.

**`-u`**

Opcional. Especifica que quieres ejecutar el comando netsh en una cuenta de usuario.

***`DomainName\\`***

Opcional. Especifica el dominio donde se encuentra la cuenta de usuario. El valor predeterminado es el dominio local si no se especifica *NombreDeDominio\\* .

***`UserName`***

Opcional. Especifica el nombre de la cuenta de usuario.

**`-p`**

Opcional. Especifica que quieres proporcionar una contraseña para la cuenta de usuario.

***`Password`***

Opcional. Especifica la contraseña de la cuenta de usuario que especificaste con **-u** *NombreDeUsuario*.

***`NetshCommand`***

Opcional. Especifica el comando **netsh** que quieres ejecutar.

**`-f`**

Opcional. Sale de **netsh** después de ejecutar el script que se designa con *ArchivoDeScript*.

***`ScriptFile`***

Opcional. Especifica el script que quieres ejecutar.

**`/?`**

Opcional. Muestra la ayuda en el símbolo del sistema de netsh.

> [!NOTE]
> Si especificas **`-r`** seguido de otro comando, **netsh** ejecuta el comando en el equipo remoto y, luego, vuelve al símbolo del sistema Cmd.exe. Si especificas **`-r`** sin otro comando, **netsh** se abre en modo remoto. El proceso es similar al uso de **set machine** en el símbolo del sistema de netsh. Cuando usas **`-r`** , estableces el equipo de destino para la instancia actual de **netsh** únicamente. Después de salir de **netsh** y volver a entrar, el equipo de destino se restablece como equipo local. Puedes ejecutar comandos **netsh** en un equipo remoto si especificas un nombre de equipo almacenado en WINS, un nombre de UNC, un nombre de Internet que deba resolver el servidor DNS, o una dirección IP.

**Escritura de valores de cadena de parámetros para comandos netsh**

A lo largo de la referencia de los comandos netsh, hay comandos que contienen parámetros para los que se requiere un valor de cadena.

En el caso donde un valor de cadena contenga espacios entre caracteres, como los valores de cadena que se componen de más de una palabra, es necesario escribir el valor de cadena entre comillas. Por ejemplo, para un parámetro denominado **interface** con un valor de cadena de **Wireless Network Connection**, usa comillas alrededor del valor de cadena:

**`interface="Wireless Network Connection"`**

