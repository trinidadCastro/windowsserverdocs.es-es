---
title: Sintaxis, contextos y formatos de comandos Netsh
description: Puede utilizar este tema para obtener información sobre cómo escribir subcontextos y contextos de netsh, entender la sintaxis de netsh y formato de comando y cómo ejecutar comandos de netsh en equipos locales y remotos que ejecutan Windows Server 2016 o Windows 10.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 8cb9b59f-0255-4261-b49a-562c5ea50ee0
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: adb77d841ba4d69b0d36bc7f19d4707638530c97
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823696"
---
# <a name="netsh-command-syntax-contexts-and-formatting"></a>Sintaxis, contextos y formatos de comandos Netsh

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para obtener información sobre cómo escribir subcontextos y contextos de netsh, entender la sintaxis de netsh y formato de comando y cómo ejecutar comandos de netsh en equipos locales y remotos.

Netsh es una utilidad de scripting de línea de comandos que le permite mostrar o modificar la configuración de red de un equipo que se está ejecutando actualmente. Se pueden ejecutar los comandos Netsh para ello, escriba los comandos en el símbolo del sistema de netsh y que pueden utilizarse en archivos por lotes o secuencias de comandos. Los equipos remotos y el equipo local pueden configurarse mediante el uso de comandos netsh.

Netsh también proporciona una característica de scripting que permite ejecutar un grupo de comandos en modo de lotes con un equipo especificado. Con netsh se puede guardar un script de configuración en un archivo de texto para archivarlo y así configurar otros equipos.

## <a name="netsh-contexts"></a>Contextos de Netsh

Netsh interactúa con otros componentes del sistema operativo mediante el uso dinámicos\-biblioteca de vínculos \(DLL\) archivos. 

Cada estos archivos DLL proporciona un amplio conjunto de características denominadas un *contexto*, que es un grupo de comandos específicos de un rol o característica de red. Estos contextos amplían la funcionalidad de netsh por lo que proporciona configuración y supervisión de uno o varios servicios, utilidades o protocolos. Por ejemplo, Dhcpmon.dll proporciona netsh con el contexto y el conjunto de comandos necesarios para configurar y administrar servidores DHCP.

### <a name="obtain-a-list-of-contexts"></a>Obtener una lista de contextos

Puede obtener una lista de contextos de netsh abriendo el símbolo del sistema o Windows PowerShell en un equipo que ejecuta Windows Server 2016 o Windows 10. Escriba el comando **netsh** y presione ENTRAR. Tipo **/?**, y, a continuación, presione ENTRAR.

La siguiente es la salida de ejemplo de estos comandos en un equipo que ejecuta Windows Server 2016 Datacenter.

    PS C:\Windows\system32> netsh
    netsh>/?
    
    The following commands are available:
    
    Commands in this context:
    ..            - Goes up one context level.
    ?             - Displays a list of commands.
    abort         - Discards changes made while in offline mode.
    add           - Adds a configuration entry to a list of entries.
    advfirewall   - Changes to the `netsh advfirewall' context.
    alias         - Adds an alias.
    branchcache   - Changes to the `netsh branchcache' context.
    bridge        - Changes to the `netsh bridge' context.
    bye           - Exits the program.
    commit        - Commits changes made while in offline mode.
    delete        - Deletes a configuration entry from a list of entries.
    dhcpclient    - Changes to the `netsh dhcpclient' context.
    dnsclient     - Changes to the `netsh dnsclient' context.
    dump          - Displays a configuration script.
    exec          - Runs a script file.
    exit          - Exits the program.
    firewall      - Changes to the `netsh firewall' context.
    help          - Displays a list of commands.
    http          - Changes to the `netsh http' context.
    interface     - Changes to the `netsh interface' context.
    ipsec         - Changes to the `netsh ipsec' context.
    ipsecdosprotection - Changes to the `netsh ipsecdosprotection' context.
    lan           - Changes to the `netsh lan' context.
    namespace     - Changes to the `netsh namespace' context.
    netio         - Changes to the `netsh netio' context.
    offline       - Sets the current mode to offline.
    online        - Sets the current mode to online.
    popd          - Pops a context from the stack.
    pushd         - Pushes current context on stack.
    quit          - Exits the program.
    ras           - Changes to the `netsh ras' context.
    rpc           - Changes to the `netsh rpc' context.
    set           - Updates configuration settings.
    show          - Displays information.
    trace         - Changes to the `netsh trace' context.
    unalias       - Deletes an alias.
    wfp           - Changes to the `netsh wfp' context.
    winhttp       - Changes to the `netsh winhttp' context.
    winsock       - Changes to the `netsh winsock' context.
    
    The following sub-contexts are available:
     advfirewall branchcache bridge dhcpclient dnsclient firewall http interface ipsec ipsecdosprotection lan namespace netio ras rpc trace wfp winhttp winsock
    
    To view help for a command, type the command, followed by a space, and then
     type ?.


### <a name="subcontexts"></a>Subcontextos

Contextos de Netsh pueden contener comandos y en otros contextos, llamados *subcontextos*. Por ejemplo, en el contexto de enrutamiento, puede cambiar los subcontextos IP e IPv6.

¿Para mostrar una lista de comandos y subcontextos que puede usar dentro de un contexto en el símbolo del sistema netsh, escriba el nombre de contexto y, a continuación, escriba **/?** o **ayuda**. Por ejemplo, para mostrar una lista de subcontextos y comandos que puede usar en el contexto de enrutamiento, en el símbolo del sistema de netsh \(es decir, **netsh&gt;**\), escriba uno de los siguientes:

**¿enrutamiento /?**

**Ayuda de enrutamiento**

Para realizar tareas en otro contexto sin cambiar el contexto actual, escriba la ruta de acceso del contexto del comando que desea usar en el símbolo del sistema de netsh. Por ejemplo, para agregar una interfaz denominada "Local Area Connection" en el contexto IGMP sin cambiar primero el contexto de IGMP, en el símbolo del sistema netsh, escriba:

**enrutamiento ip igmp agregar interfaz "Conexión de área Local" startupqueryinterval = 21**

## <a name="running-netsh-commands"></a>Ejecutar comandos de netsh

Para ejecutar un comando netsh, debe iniciar netsh desde el símbolo del sistema escribiendo **netsh** y, a continuación, presione ENTRAR. A continuación, puede cambiar al contexto que contiene el comando que desea usar. Los contextos disponibles dependen de los componentes de red que ha instalado. Por ejemplo, si escribe **dhcp** en el símbolo de sistema netsh y presione ENTRAR, netsh, los cambios en el contexto del servidor DHCP. Si no tiene instalado DHCP, sin embargo, aparece el mensaje siguiente:

**No se encontró el siguiente comando: dhcp.**

## <a name="formatting-legend"></a>Leyenda de formato

Puede utilizar la leyenda de formato siguiente al interpretar y usar la sintaxis del comando netsh correcto al ejecutar el comando en el símbolo del sistema de netsh o en un archivo por lotes o secuencias de comandos.

- Texto en *cursiva* es información que debe suministrar mientras escribe el comando. Por ejemplo, si un comando tiene un parámetro denominado -*UserName*, debe escribir el nombre de usuario real.
- Texto en **negrita** es información que debe escribir exactamente como se muestra mientras se escribe el comando.
- Texto seguido de puntos suspensivos \(... \) es un parámetro que se puede repetir varias veces en una línea de comandos.
- Texto que se encuentra entre corchetes [&nbsp;] es un elemento opcional.
- Texto que está entre llaves {&nbsp;} con opciones separadas por una barra vertical proporciona un conjunto de opciones en el que debe seleccionar sólo uno, como `{enable|disable}`.
- Texto que se da formato con la fuente Courier es código o resultado del programa.

## <a name="running-netsh-commands-from-the-command-prompt-or-windows-powershell"></a>Ejecutar comandos de Netsh desde el símbolo del sistema o de Windows PowerShell

Para iniciar el Shell de red y escriba netsh en el símbolo del sistema o en Windows PowerShell, puede usar el siguiente comando.

### <a name="netsh"></a>netsh

Netsh es una utilidad de scripting de línea de comandos que le permite, ya sea de forma local o remota, mostrar o modificar la configuración de red de un equipo que se está ejecutando. Se utiliza sin parámetros, **netsh** abre el símbolo del sistema de Netsh.exe \(es decir, **netsh&gt;**\).

#### <a name="syntax"></a>Sintaxis

**netsh** \[ **- a**&nbsp;*archivoAlias* \] \[ **- c** &nbsp;  *Contexto* \] \[ **- r**&nbsp;*equipoRemoto* \] \[ **- u** \[ *DomainName\\*  \] *UserName* \] \[ **-p** &nbsp; *Contraseña*  |  \* \] \[{*comandoNetsh* | **-f** &nbsp; *ScriptFile*}\]

#### <a name="parameters"></a>Parámetros

**`-a`**

Opcional. Especifica que se le redirigirá a la **netsh** símbolo del sistema después de ejecutar *archivoAlias*.

**`AliasFile`**

Opcional. Especifica el nombre del archivo de texto que contiene uno o varios **netsh** comandos.

**`-c`**

Opcional. Especifica que netsh entra en el especificado **netsh** contexto.

**`Context`**

Opcional. Especifica el **netsh** contexto que se desea escribir. 

**`-r`**

Opcional. Especifica que desea que el comando se ejecute en un equipo remoto.

>[!IMPORTANT]
>Cuando usa algunos comandos de netsh remotamente en otro equipo con el **netsh – r** parámetro, el servicio Registro remoto debe ejecutar en el equipo remoto. Si no está en ejecución, Windows muestra un mensaje de error "Ruta de acceso de red no encontrado".

***`RemoteComputer`***

Opcional. Especifica el equipo remoto que desea configurar.

**`-u`**

Opcional. Especifica que desea ejecutar el comando netsh bajo una cuenta de usuario.

***`DomainName\\`***

Opcional. Especifica el dominio donde se encuentra la cuenta de usuario. El valor predeterminado es el dominio local si *DomainName\\*  no se especifica.

***`UserName`***

Opcional. Especifica el nombre de cuenta de usuario.

**`-p`**

Opcional. Especifica que desea proporcionar una contraseña para la cuenta de usuario.

***`Password`***

Opcional. Especifica la contraseña de la cuenta de usuario que especificó con **-u** *UserName*.

***`NetshCommand`***

Opcional. Especifica el **netsh** comando que desea ejecutar.

**`-f`**

Opcional. Se cierra **netsh** después de ejecutar el script que se designa con *ScriptFile*.

***`ScriptFile`***

Opcional. Especifica el script que desea ejecutar.

**`/?`**

Opcional. Muestra la Ayuda en el símbolo del sistema de netsh.

>[!NOTE]
>Si especifica **`-r`** seguido de otro comando, **netsh** se ejecuta el comando en el equipo remoto y, a continuación, se devuelve a la línea de comandos de Cmd.exe. Si especifica **`-r`** sin otro comando **netsh** se abre en modo remoto. El proceso es similar a usar **conjunto máquina** en la línea de comandos de Netsh. Cuando usas **`-r`**, configura el equipo de destino para la instancia actual de **netsh** solo. Después de salir y volver a escribir **netsh**, se restablece el equipo de destino que el equipo local. Puede ejecutar **netsh** comandos en un equipo remoto mediante la especificación de un equipo el nombre almacenado en WINS, un nombre UNC, un nombre de Internet pueden resolverse mediante el servidor DNS o una dirección IP.

**Escribir valores de cadena de parámetros para los comandos de netsh**

A lo largo de la referencia de comandos de Netsh hay comandos que contienen parámetros para el que se requiere un valor de cadena.

En el caso de que un valor de cadena contiene espacios en blanco entre caracteres, como los valores de cadena que se componen de más de una palabra, es necesario que escriba el valor de cadena entre comillas. Por ejemplo, para un parámetro denominado **interfaz** con un valor de cadena de **conexión de red inalámbrica**, utilice comillas alrededor del valor de cadena:

**`interface="Wireless Network Connection"`**

