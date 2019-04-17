---
title: Sintaxis de comandos Netsh, los contextos y formato
description: Puedes usar este tema para obtener información sobre cómo escribir subcontextos y contextos de netsh, comprender netsh sintaxis y el formato de comando y cómo ejecutar comandos netsh en los equipos locales y remotos que ejecutan Windows Server 2016 o Windows 10.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 8cb9b59f-0255-4261-b49a-562c5ea50ee0
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c7c16674b831431ff5bb1d0172cc2b2a939008f6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="netsh-command-syntax-contexts-and-formatting"></a>Sintaxis de comandos Netsh, los contextos y formato

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener información sobre cómo escribir subcontextos y contextos de netsh, comprender netsh sintaxis y el formato de comando y cómo ejecutar comandos netsh en los equipos locales y remotos.

Netsh es una utilidad de línea de comandos de scripting que te permite mostrar o modificar la configuración de red de un equipo que se está ejecutando. Se pueden ejecutar los comandos Netsh al escribir estos comandos en el símbolo del sistema de netsh y pueden usarse en archivos por lotes o scripts. Equipos remotos y el equipo local pueden configurarse mediante comandos netsh.

Netsh también proporciona una característica de scripting que te permite ejecutar un grupo de comandos en lotes en un equipo especificado. Con netsh, puede guardar un script de configuración en un archivo de texto para archivarlo o que te ayudarán a configurar otros equipos.

## <a name="netsh-contexts"></a>Contextos de Netsh

Netsh interactúa con otros componentes de sistema operativo mediante el uso de archivos de vínculo dynamic\ biblioteca \(DLL\). 

Cada estos archivos DLL proporciona un amplio conjunto de características denominado un *contexto*, que es un grupo de comandos específicos para un rol de servidor de red o característica. Estos contextos amplían la funcionalidad de netsh al proporcionar la configuración y supervisión de uno o más servicios, utilidades o protocolos. Por ejemplo, Dhcpmon.dll proporciona netsh con el contexto y el conjunto de comandos necesarios para configurar y administrar los servidores DHCP.

### <a name="obtain-a-list-of-contexts"></a>Obtener una lista de contextos

Puede obtener una lista de los contextos de netsh abriendo el símbolo del sistema o Windows PowerShell en un equipo que ejecute Windows Server 2016 o Windows 10. Escribe el comando **netsh** y presiona ENTRAR. ¿Tipo **/? **, y, a continuación, presione ENTRAR.

A continuación mostramos resultados de ejemplo para estos comandos en un equipo que ejecute Windows Server 2016 Datacenter.

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

Contextos de Netsh pueden contener comandos y otros contextos, llamados *subcontextos*. Por ejemplo, dentro del contexto de enrutamiento, puede cambiar los subcontextos IP e IPv6.

¿Para mostrar una lista de comandos y subcontextos que puedes usar en un contexto en el símbolo del sistema netsh, escribe el nombre de contexto y, a continuación, escriba uno **/?** O **ayuda **. Por ejemplo, para mostrar una lista de subcontextos y comandos que puedes usar en el contexto de enrutamiento, en el símbolo del sistema de netsh \ (es decir, **netsh&gt;**\), escribe una de las siguientes acciones:

**¿enrutamiento /?**

**Enrutamiento de ayuda**

Para realizar tareas en otro contexto sin cambiar el contexto actual, escriba la ruta del contexto del comando que quieras usar en el símbolo del sistema de netsh. Por ejemplo, para agregar una interfaz denominada "Conexión de área Local" en el contexto IGMP sin cambiar primero el contexto IGMP, en el símbolo del sistema netsh, escribe:

**Enrutamiento ip igmp agregar interfaz "Conexión de área Local" startupqueryinterval = 21**

## <a name="running-netsh-commands"></a>Ejecuta los comandos netsh

Para ejecutar un comando netsh, debe iniciar netsh desde el símbolo del sistema, escribe **netsh** y, a continuación, presione ENTRAR. A continuación, puedes cambiar el contexto que contiene el comando que quieras usar. Los contextos que están disponibles dependen de los componentes de red que haya instalado. Por ejemplo, si escribes **dhcp** en el símbolo del sistema de netsh y presione ENTRAR, netsh cambios en el contexto del servidor DHCP. Si no tienes instalado DHCP, sin embargo, aparece el siguiente mensaje:

**No se encontró el siguiente comando: dhcp.**

## <a name="formatting-legend"></a>Formato de leyenda

Puedes usar el siguiente formato de leyenda interpretar y usar la sintaxis del comando netsh correcto al ejecutar el comando en el símbolo del sistema de netsh o en un archivo por lotes o un script.

- Texto en *cursiva* es la información que proporcione mientras se ejecuta el comando. Por ejemplo, si un comando tiene un parámetro denominado -*nombre de usuario*, debes escribir el nombre de usuario real.
- Texto en **negrita** es la información que debes escribir exactamente como se muestra mientras se ejecuta el comando.
- Texto, seguido de un \(...\) puntos suspensivos es un parámetro que se puede repetir varias veces en una línea de comandos.
- Texto que se encuentra entre corchetes [&nbsp;] es un elemento opcional.
- Texto que se encuentra entre llaves {&nbsp;} con opciones separadas por una canalización proporciona un conjunto de opciones en el que debe seleccionar solo una como `{enable|disable}`.
- Texto que se da formato con la fuente Courier es código o salida de programa.

## <a name="running-netsh-commands-from-the-command-prompt-or-windows-powershell"></a>Ejecuta los comandos Netsh desde el símbolo del sistema o Windows PowerShell

Para iniciar el Shell de red y escribe netsh en el símbolo del sistema o en Windows PowerShell, puedes usar el siguiente comando.

### <a name="netsh"></a>netsh

Netsh es una utilidad de línea de comandos de scripting que te permite, ya sea local o remota, mostrar o modificar la configuración de red de un equipo en ejecución actualmente. Usa sin parámetros, **netsh** abre el símbolo del sistema de Netsh.exe \ (es decir, **netsh&gt;**\).

#### <a name="syntax"></a>Sintaxis

**netsh**\[ **-a**&nbsp;*AliasFile*\] \[ **-c**&nbsp;*Context* \] \[**-r**&nbsp;*RemoteComputer*\] \[ **-u** \[ *DomainName\\* \] *UserName* \] \[ **-p**&nbsp;*Password* | \*\] \[{*NetshCommand* | **-f**&nbsp;*ScriptFile*}\]

#### <a name="parameters"></a>Parámetros

**`-a`**

Opcional. Especifica que se devuelven a la **netsh** símbolo del sistema después de ejecutar *archivoAlias *.

**`AliasFile`**

Opcional. Especifica el nombre del archivo de texto que contiene uno o más **netsh** comandos.

**`-c`**

Opcional. Especifica que netsh entra en especificado **netsh** contexto.

**`Context`**

Opcional. Especifica el **netsh** contexto que quieres escribir. 

**`-r`**

Opcional. Especifica que desea que el comando para ejecutar en un equipo remoto.

>[!IMPORTANT]
>Cuando usas algunos comandos netsh remotamente en otro equipo con el **netsh – r** parámetro, el servicio de registro remoto debe estar ejecutándose en el equipo remoto. Si no se está ejecutando, Windows muestra un mensaje de error "Ruta de acceso de red no encontrado".

***`RemoteComputer`***

Opcional. Especifica el equipo remoto que quieras configurar.

**`-u`**

Opcional. Especifica que desea ejecutar el comando netsh en una cuenta de usuario.

***`DomainName\\`***

Opcional. Especifica el dominio donde se encuentra la cuenta de usuario. El valor predeterminado es el dominio local si *DomainName\\* no se especifica ninguna.

***`UserName`***

Opcional. Especifica el nombre de cuenta de usuario.

**`-p`**

Opcional. Especifica que desea proporcionar una contraseña para la cuenta de usuario.

***`Password`***

Opcional. Especifica la contraseña de la cuenta de usuario que especificaste con **-u***UserName *.

***`NetshCommand`***

Opcional. Especifica el **netsh** comando que quieres ejecutar.

**`-f`**

Opcional. Salidas **netsh** después de ejecutar el script que designes con *archivoDeComandos *.

***`ScriptFile`***

Opcional. Especifica el script que desea ejecutar.

**`/?`**

Opcional. Muestra Ayuda en el símbolo del sistema de netsh.

>[!NOTE]
>Si especificas **`-r`**seguido de otro comando **netsh** ejecuta el comando en el equipo remoto y, a continuación, se devuelve al símbolo del sistema de Cmd.exe. Si especificas **`-r`**sin otro comando **netsh** se abre en modo remoto. El proceso es similar al uso **conjunto máquina** en la línea de comandos Netsh. Cuando usas **`-r`**, configura el equipo de destino para la instancia actual de **netsh** solo. Después de salir y vuelve a escribir **netsh**, se restablece el equipo de destino que el equipo local. Puedes ejecutar **netsh** comandos en un equipo remoto mediante la especificación de un equipo nombre almacenado en WINS, un nombre UNC, un nombre de Internet que se resuelva el servidor DNS o una dirección IP.

**Escribir valores de cadena de parámetros para los comandos netsh**

A lo largo de la referencia de comandos Netsh hay comandos que contienen los parámetros para el que se requiere un valor de cadena.

En el caso de que un valor de cadena contiene espacios entre caracteres, como valores de cadena que constan de más de una palabra, es necesario que incluya el valor de cadena entre comillas. Por ejemplo, para un parámetro denominado **interfaz** con un valor de cadena de **conexión de red inalámbrica**, entre comillas el valor de cadena:

**`interface="Wireless Network Connection"`**

