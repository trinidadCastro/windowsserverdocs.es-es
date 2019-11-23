---
title: Sintaxis, contextos y formatos de comandos Netsh
description: Puede usar este tema para obtener información sobre cómo especificar contextos y subcontextos de Netsh, comprender la sintaxis de Netsh y el formato de los comandos, y cómo ejecutar comandos Netsh en equipos locales y remotos que ejecutan Windows Server 2016 o Windows 10.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 8cb9b59f-0255-4261-b49a-562c5ea50ee0
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 815e59ca00d6450b8ef09a034434c4209c928e78
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405575"
---
# <a name="netsh-command-syntax-contexts-and-formatting"></a>Sintaxis, contextos y formatos de comandos Netsh

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información sobre cómo especificar contextos y subcontextos de Netsh, comprender la sintaxis de Netsh y el formato de los comandos, y cómo ejecutar comandos Netsh en equipos locales y remotos.

Netsh es una utilidad de scripting de línea de comandos que permite mostrar o modificar la configuración de red de un equipo que se está ejecutando actualmente. Los comandos Netsh se pueden ejecutar escribiendo comandos en el símbolo del sistema de Netsh y se pueden usar en archivos por lotes o scripts. Los equipos remotos y el equipo local se pueden configurar mediante los comandos Netsh.

Netsh también proporciona una característica de scripting que permite ejecutar un grupo de comandos en modo de lotes con un equipo especificado. Con netsh se puede guardar un script de configuración en un archivo de texto para archivarlo y así configurar otros equipos.

## <a name="netsh-contexts"></a>Contextos de Netsh

Netsh interactúa con otros componentes del sistema operativo mediante Dynamic\-Link Library \(DLL\) archivos. 

Cada archivo DLL de la aplicación auxiliar netsh proporciona un amplio conjunto de características denominado *contexto*, que es un grupo de comandos específicos de un rol o característica del servidor de red. Estos contextos amplían la funcionalidad de Netsh al proporcionar compatibilidad con la configuración y la supervisión de uno o varios servicios, utilidades o protocolos. Por ejemplo, Dhcpmon. dll proporciona netsh con el contexto y el conjunto de comandos necesarios para configurar y administrar los servidores DHCP.

### <a name="obtain-a-list-of-contexts"></a>Obtener una lista de contextos

Para obtener una lista de contextos de Netsh, abra el símbolo del sistema o Windows PowerShell en un equipo que ejecute Windows Server 2016 o Windows 10. Escriba el comando **netsh** y presione Entrar. Escriba **/?** y, a continuación, presione Entrar.

A continuación se muestra una salida de ejemplo para estos comandos en un equipo que ejecuta Windows Server 2016 Datacenter.

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

Los contextos netsh pueden contener comandos y contextos adicionales, denominados *subcontextos*. Por ejemplo, en el contexto de enrutamiento, puede cambiar a los subcontextos IP e IPv6.

Para mostrar una lista de comandos y subcontextos que puede usar en un contexto, en el símbolo del sistema de Netsh, escriba el nombre del contexto y, a continuación, escriba **/?** o la **ayuda**de. Por ejemplo, para mostrar una lista de subcontextos y comandos que puede usar en el contexto de enrutamiento, en el símbolo del sistema de Netsh \(es decir, **netsh&gt;** \), escriba uno de los siguientes:

**¿enrutamiento/?**

**ayuda de enrutamiento**

Para realizar tareas en otro contexto sin cambiar el contexto actual, escriba la ruta de acceso de contexto del comando que desea utilizar en el símbolo del sistema de Netsh. Por ejemplo, para agregar una interfaz denominada "conexión de área local" en el contexto de IGMP sin cambiar primero al contexto de IGMP, en el símbolo del sistema de Netsh, escriba:

**IP de enrutamiento IGMP agregar interfaz "conexión de área local" startupqueryinterval = 21**

## <a name="running-netsh-commands"></a>Ejecutar comandos Netsh

Para ejecutar un comando netsh, debe iniciar Netsh desde el símbolo del sistema; para ello, escriba **netsh** y presione Entrar. A continuación, puede cambiar al contexto que contiene el comando que quiere usar. Los contextos que están disponibles dependen de los componentes de red que haya instalado. Por ejemplo, si escribe **DHCP** en el símbolo del sistema de Netsh y presiona entrar, netsh cambia al contexto del servidor DHCP. Sin embargo, si no tiene DHCP instalado, aparece el siguiente mensaje:

**No se encontró el comando siguiente: DHCP.**

## <a name="formatting-legend"></a>Aplicar formato a la leyenda

Puede usar la siguiente leyenda de formato para interpretar y utilizar la sintaxis del comando netsh correcta al ejecutar el comando en el símbolo del sistema de Netsh o en un archivo o script por lotes.

- El texto en *cursiva* es información que debe proporcionar mientras escribe el comando. Por ejemplo, si un comando tiene un parámetro denominado-*username*, debe escribir el nombre de usuario real.
- El texto en **negrita** es información que debe escribir exactamente como se muestra mientras escribe el comando.
- El texto seguido de un botón de puntos suspensivos \(...\) es un parámetro que se puede repetir varias veces en una línea de comandos.
- El texto entre corchetes [&nbsp;] es un elemento opcional.
- El texto entre llaves {&nbsp;} con opciones separadas por una canalización proporciona un conjunto de opciones de las que debe seleccionar solo una, como `{enable|disable}`.
- El texto al que se da formato con la fuente Courier es código o resultado del programa.

## <a name="running-netsh-commands-from-the-command-prompt-or-windows-powershell"></a>Ejecutar comandos Netsh desde el símbolo del sistema o Windows PowerShell

Para iniciar el shell de red y escribir Netsh en el símbolo del sistema o en Windows PowerShell, puede usar el siguiente comando.

### <a name="netsh"></a>netsh

Netsh es una utilidad de scripting de línea de comandos que permite mostrar o modificar la configuración de red de un equipo que se está ejecutando actualmente, ya sea de forma local o remota. Si se usa sin parámetros, **netsh** abre el símbolo del sistema netsh. exe \(es decir, **netsh&gt;** \).

#### <a name="syntax"></a>Sintaxis

**netsh**\[ **-a**&nbsp;*AliasFile*\] \[ **-c**&nbsp;*Context* \] **\[-r**&nbsp;*equiporemoto*\] \[ **-u** \[ *domainname\\* \] *username* \] \[ **-p**&nbsp;*password* | \*\] \[{*NetshCommand* |  **-f**&nbsp; *ScriptFile*}\]

#### <a name="parameters"></a>Parámetros

**`-a`**

Opcional. Especifica que se devuelve al símbolo del sistema de **netsh** después de ejecutar *AliasFile*.

**`AliasFile`**

Opcional. Especifica el nombre del archivo de texto que contiene uno o más comandos **netsh** .

**`-c`**

Opcional. Especifica que netsh entra en el contexto de **netsh** especificado.

**`Context`**

Opcional. Especifica el contexto de **netsh** que se desea escribir. 

**`-r`**

Opcional. Especifica que desea que el comando se ejecute en un equipo remoto.

> [!IMPORTANT]
> Si usa algunos comandos Netsh de forma remota en otro equipo con el parámetro **netsh – r** , el servicio de registro remoto debe estar en ejecución en el equipo remoto. Si no se está ejecutando, Windows muestra un mensaje de error "ruta de acceso de red no encontrada".

***`RemoteComputer`***

Opcional. Especifica el equipo remoto que desea configurar.

**`-u`**

Opcional. Especifica que desea ejecutar el comando netsh en una cuenta de usuario.

***`DomainName\\`***

Opcional. Especifica el dominio en el que se encuentra la cuenta de usuario. El valor predeterminado es el dominio local si no se especifica *DomainName\\* .

***`UserName`***

Opcional. Especifica el nombre de la cuenta de usuario.

**`-p`**

Opcional. Especifica que desea proporcionar una contraseña para la cuenta de usuario.

***`Password`***

Opcional. Especifica la contraseña de la cuenta de usuario que especificó con **-u** *nombreDeUsuario*.

***`NetshCommand`***

Opcional. Especifica el comando **netsh** que desea ejecutar.

**`-f`**

Opcional. Sale de **netsh** después de ejecutar el script que se designa con *ScriptFile*.

***`ScriptFile`***

Opcional. Especifica el script que desea ejecutar.

**`/?`**

Opcional. Muestra la ayuda en el símbolo del sistema de Netsh.

> [!NOTE]
> Si especifica **`-r`** seguido de otro comando, **netsh** ejecuta el comando en el equipo remoto y, a continuación, vuelve al símbolo del sistema cmd. exe. Si especifica **`-r`** sin otro comando, **netsh** se abre en modo remoto. El proceso es similar al uso de **set Machine** en el símbolo del sistema de Netsh. Cuando se usa **`-r`** , se establece el equipo de destino para la instancia actual de solo **netsh** . Después de salir y volver a escribir **netsh**, el equipo de destino se restablece como el equipo local. Los comandos **netsh** se pueden ejecutar en un equipo remoto mediante la especificación de un nombre de equipo almacenado en WINS, un nombre UNC, un nombre de Internet que debe resolver el servidor DNS o una dirección IP.

**Escribir valores de cadena de parámetros para comandos Netsh**

En la referencia del comando netsh hay comandos que contienen parámetros para los que se requiere un valor de cadena.

En el caso de que un valor de cadena contenga espacios entre caracteres, como los valores de cadena que se componen de más de una palabra, es necesario incluir el valor de cadena entre comillas. Por ejemplo, para un parámetro denominado **interface** con un valor de cadena de **conexión de red inalámbrica**, utilice comillas alrededor del valor de cadena:

**`interface="Wireless Network Connection"`**

