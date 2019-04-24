---
title: Establecer el subcomando-servidor
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da55c29d-a94a-4d73-877b-af480f906ca0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: abc3fe23558f077e0ba9ac69f2641e3b8c9cde4f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858386"
---
# <a name="subcommand-set-server"></a>Subcomando: set-Server

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configura las opciones para un servidor de servicios de implementación de Windows.
## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Set-Server [/Server:<Server name>]
    [/Authorize:{Yes | No}]
    [/RogueDetection:{Yes | No}]
    [/AnswerClients:{All | Known | None}]
    [/Responsedelay:<time in seconds>]
    [/AllowN12forNewClients:{Yes | No}]
    [/ArchitectureDiscovery:{Yes | No}]
    [/resetBootProgram:{Yes | No}]
    [/DefaultX86X64Imagetype:{x86 | x64 | Both}]
    [/UseDhcpPorts:{Yes | No}]
    [/DhcpOption60:{Yes | No}]
    [/RpcPort:<Port number>]
    [/PxepromptPolicy 
        [/Known:{OptIn | Noprompt | OptOut}]
        [/New:{OptIn | Noprompt | OptOut}] 
    [/BootProgram:<Relative path>]
         /Architecture:{x86 | ia64 | x64}
    [/N12BootProgram:<Relative path>]
         /Architecture:{x86 | ia64 | x64}
    [/BootImage:<Relative path>]
         /Architecture:{x86 | ia64 | x64}
    [/PreferredDC:<DC Name>]
    [/PreferredGC:<GC Name>]
    [/PrestageUsingMAC:{Yes | No}]
    [/NewMachineNamingPolicy:<Policy>]
    [/NewMachineOU]
         [/type:{Serverdomain | Userdomain | UserOU | Custom}]
         [/OU:<Domain name of OU>]
    [/DomainSearchOrder:{GCOnly | DCFirst}]
    [/NewMachineDomainJoin:{Yes | No}]
    [/OSCMenuName:<Name>]
    [/WdsClientLogging]
         [/Enabled:{Yes | No}]
         [/LoggingLevel:{None | Errors | Warnings | Info}]
    [/WdsUnattend]
         [/Policy:{Enabled | Disabled}]
         [/CommandlinePrecedence:{Yes | No}]
         [/File:<path>]
             /Architecture:{x86 | ia64 | x64}
    [/AutoaddPolicy]
         [/Policy:{AdminApproval | Disabled}]
         [/PollInterval:{time in seconds}]
         [/MaxRetry:{Retries}]
         [/Message:<Message>]
         [/RetentionPeriod]
             [/Approved:<time in days>]
             [/Others:<time in days>]
    [/AutoaddSettings]
         /Architecture:{x86 | ia64 | x64}
         [/BootProgram:<Relative path>]
         [/ReferralServer:<Server name>
         [/WdsClientUnattend:<Relative path>]
         [/BootImage:<Relative path>]
         [/User:<Owner>]
         [/JoinRights:{JoinOnly | Full}]
         [/JoinDomain:{Yes | No}]
    [/BindPolicy]
         [/Policy:{Include | Exclude}]
         [/add]
              /address:<IP or MAC address>
              /addresstype:{IP | MAC}
         [/remove]
              /address:<IP or MAC address>
              /addresstype:{IP | MAC}
    [/RefreshPeriod:<time in seconds>]
    [/BannedGuidPolicy]
         [/add]
              /Guid:<GUID>
         [/remove]
             /Guid:<GUID>
    [/BcdRefreshPolicy]
         [/Enabled:{Yes | No}]
         [/RefreshPeriod:<time in minutes>]
    [/Transport]
         [/ObtainIpv4From:{Dhcp | Range}]
             [/start:<start IP address>]
             [/End:<End IP address>]
         [/ObtainIpv6From:Range]
             [/start:<start IP address>]
             [/End:<End IP address>]
         [/startPort:<start Port>
             [/EndPort:<start Port>
        [/Profile:{10Mbps | 100Mbps | 1Gbps | Custom}]
        [/MulticastSessionPolicy]
             [/Policy:{None | AutoDisconnect | Multistream}]
                 [/Threshold:<Speed in KBps>]
                 [/StreamCount:{2 | 3}]
                 [/Fallback:{Yes | No}]    
        [/forceNative]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se usará el servidor local.|
|[/Authorize:{Yes &#124; No}]|Especifica si se debe autorizar este servidor en el protocolo de Control dinámico de Host (DHCP).|
|[/ RogueDetection: {Sí &#124; n}]|Habilita o deshabilita la detección rogue DHCP.|
|[/ AnswerClients: {todos &#124; conocido &#124; None}]|Especifica que los clientes responderá a este servidor. Si establece este valor en **conocido**, debe preconfigurar un equipo en servicios de dominio de active directory (AD DS) antes de que se responderá por el servidor de servicios de implementación de Windows.|
|[/ Responsedelay:<time in seconds>]|La cantidad de tiempo que esperará el servidor antes de responder a un cliente de arranque. Esta configuración no se aplica a los equipos preconfigurados.|
|[/AllowN12forNewClients:{Yes &#124; No}]|para Windows Server 2008, especifica que los clientes desconocidos no tendrá que presionar la tecla F12 para iniciar un arranque de red. Sabe que los clientes recibirán el programa de arranque especificado para el equipo o, si no se especifica, el programa de arranque especificado para la arquitectura.<br /><br />para Windows Server 2008 R2, esta opción se ha reemplazado por el siguiente comando: wdsutil /Set-Server /PxepromptPolicy /New:Noprompt|
|[/ ArchitectureDiscovery: {Sí &#124; n}]|Habilita o deshabilita la detección de arquitectura. Esto facilita la detección de clientes basados en x64 64 que no se difunden su arquitectura correctamente.|
|[/resetBootProgram:{Yes &#124; No}]|Determina si se borrará la ruta de acceso de inicio para un cliente que solo haya arrancado sin necesidad de una pulsación de tecla F12.|
|[/ DefaultX86X64Imagetype: {x86 &#124; x64 &#124; ambos}]|Controla qué imágenes de arranque se mostrará a los clientes basados en x64 64.|
|[/UseDhcpPorts:{Yes &#124; No}]|Especifica si el servidor PXE debe intentar enlazar con el puerto DHCP, 67 de puerto TCP. Si DHCP y servicios de implementación de Windows se ejecutan en el mismo equipo, debe establecer esta opción en **No** para habilitar el servidor DHCP usar el puerto y establecer el **/DhcpOption60** parámetro **Sí**. La configuración predeterminada para este valor es **Sí**.|
|[/ DhcpOption60: {Sí &#124; n}]|Especifica si se debe configurar la opción 60 de DHCP para la compatibilidad de PXE. Si DHCP y servicios de implementación de Windows se ejecutan en el mismo servidor, establezca esta opción en **Sí** y establezca el **/UseDhcpPorts** opción **No**. La configuración predeterminada para este valor es **No**.|
|[/RpcPort:<Port number>]|Especifica el número de puerto TCP que se usará para atender las solicitudes de cliente.|
|[/PxepromptPolicy]|Configura cómo conocidos (preconfigurados) y los clientes de nuevo iniciar un arranque PXE. Esta opción solo se aplica a Windows Server 2008 R2. Establezca la configuración mediante las siguientes opciones:<br /><br />-[Conocidos: {OptIn&#124;OptOut&#124;Noprompt}]-establece la directiva para los clientes preconfigurados.<br />-[/ Nuevo: {OptIn&#124;OptOut&#124;Noprompt}]-establece la directiva para los nuevos clientes.<br /><br />**OptIn** significa que el cliente tiene que presionar una tecla en el orden de arranque de PXE, en caso contrario, recurre al siguiente dispositivo de arranque.<br /><br />**Noprompt** significa el cliente siempre se arranque PXE.<br /><br />**OptOut** significa el cliente se arranque PXE a menos que se presiona la tecla Esc.|
|[/BootProgram:<Relative path>] /Architecture:{x86 &#124; ia64 &#124; x64}|Especifica la ruta de acceso relativa del programa de arranque en la carpeta remoteInstall (por ejemplo, **boot\x86\pxeboot.n12**) y especifica la arquitectura del programa de arranque.|
|[/ N12BootProgram:<Relative path>] / Architecture: {x86 &#124; ia64 &#124; x64}|Especifica la ruta de acceso relativa del programa de arranque que no requiere que se presione la tecla F12 (por ejemplo, **boot\x86\pxeboot.n12**) y especifica la arquitectura del programa de arranque.|
|[/BootImage:<Relative path>] /Architecture:{x86 &#124; ia64 &#124; x64}|Especifica la ruta de acceso relativa a la imagen de arranque que deben recibir los clientes que arrancan y especifica la arquitectura de la imagen de arranque. Puede especificar esto para cada arquitectura.|
|[/ PreferredDC:<DC Name>]|Especifica el nombre del controlador de dominio que debe usar servicios de implementación de Windows. Esto puede ser el nombre NetBIOS o el FQDN.|
|[/ PreferredGC:<GC Name>]|Especifica el nombre del servidor de catálogo global que se debe usar servicios de implementación de Windows. Esto puede ser el nombre NetBIOS o el FQDN.|
|[/PrestageUsingMAC:{Yes &#124; No}]|Especifica si Windows Deployment Services, al crear cuentas de equipo en AD DS, debe usar la dirección MAC en lugar de con el GUID/UUID para identificar el equipo.|
|[/NewMachineNamingPolicy:<Policy>]|Especifica el formato que se utilizará al generar nombres de equipo para los clientes. Para obtener información sobre el formato que se usará para <policy>, haga clic en el servidor en el complemento de mmc, haga clic en **propiedades**y ver el **servicios de directorio** ficha. Por ejemplo, **/NewMachineNamingPolicy: % 61Username % #**.|
|[/NewMachineOU]|Se utiliza para especificar la ubicación en AD DS donde se crearán las cuentas de equipo cliente. Especifique la ubicación mediante las siguientes opciones.<br /><br />-   [/type: Serverdomain &#124; Userdomain &#124; UserOU &#124; personalizado] Especifica el tipo de ubicación. **Serverdomain** crea cuentas en el mismo dominio que el servidor de servicios de implementación de Windows. **USERDOMAIN** crea cuentas en el mismo dominio que el usuario que realiza la instalación. **UserOU** crea cuentas en la unidad organizativa del usuario que realiza la instalación. **Personalizado** le permite especificar una ubicación personalizada (también debe especificar un valor para **/OU** con esta opción).<br />-[/ OU:<Domain name of OU>]: si especifica **personalizado** para el **/tipo** opción, esta opción especifica la unidad organizativa donde se deben crear cuentas de equipo.|
|[/DomainSearchOrder:{GCOnly &#124; DCFirst}]|Especifica la directiva para la búsqueda de cuentas de equipo en AD DS (controlador de dominio o catálogo global).|
|[/NewMachineDomainJoin:{Yes &#124; No}]|Especifica si un equipo que no esté ya preconfigurado en AD DS debe ser unido al dominio durante la instalación. El valor predeterminado es **Sí**.|
|[/WdsClientLogging]|Especifica el nivel de registro para el servidor.<br /><br />-[/ Enabled: {Sí &#124; n}]: habilita o deshabilita el registro de acciones de cliente de servicios de implementación de Windows.<br />-[/ LoggingLevel: {None &#124; errores &#124; advertencias &#124; Info}-establece el nivel de registro. **Ninguno** es equivalente a la desactivación del registro. **Errores** es el nivel más bajo de registro e indica que solo los errores se registrarán. **Advertencias** incluye errores y advertencias. **Información** es el más alto nivel de registro e incluye eventos informativos, advertencias y errores.|
|[/WdsUnattend]|Esta configuración controla el comportamiento de instalación desatendida del cliente de servicios de implementación de Windows. Establezca la configuración mediante las siguientes opciones:<br /><br />-[/ Directiva: {habilitado &#124; deshabilitado}]: Especifica si se usa una instalación desatendida.<br />-[/ CommandlinePrecedence: {Sí &#124; n}]: Especifica si se utilizará un archivo Autounattend.xml (si está presente en el cliente) o un archivo de instalación desatendida que se pasa directamente al cliente de servicios de implementación de Windows con la opción/Unattend, en lugar de un archivo de instalación desatendida de imagen durante una instalación de cliente. El valor predeterminado es **No**.<br />-[/ File:<Relative path> /Architecture: {x86 &#124; ia64 &#124; x64}]: especifica el nombre de archivo, la ruta de acceso y la arquitectura de archivo de instalación desatendida.|
|[/AutoaddPolicy]|Estos valores controlan la directiva de adición automática. Definir la configuración mediante las siguientes opciones:<br /><br />-[/ Directiva: {AdminApproval &#124; deshabilitado}]- **AdminApprove** hace que todos los equipos desconocidos para agregarse a una cola pendiente, donde el administrador puede, a continuación, revise la lista de equipos y aprobar o rechazar cada solicitud, como adecuado. **Deshabilitado** indica que se realiza ninguna acción adicional cuando se trata de un equipo desconocido arranca con el servidor.<br />-[/ PollInterval: {tiempo en segundos}]: especifica el intervalo (en segundos) en el que el programa de arranque de red debe sondear el servidor de servicios de implementación de Windows.<br />-[/ MaxRetry: <Number>]: especifica el número de veces que el programa de arranque de red debe sondear el servidor de servicios de implementación de Windows. Este valor, junto con **/PollInterval**, determina cuánto tiempo esperará el programa de arranque de red para que un administrador debe aprobar o rechazar el equipo antes de agotar el tiempo. Por ejemplo, un **MaxRetry** valor de 10 y un **PollInterval** vlue 60 indicaría que el cliente debe sondear el servidor 10 veces, esperar 60 segundos entre intentos. Por lo tanto, el cliente tendría tiempo de espera después de 10 minutos (10 x 60 segundos = 10 minutos).<br />-[/ Message: <Message>]: especifica el mensaje que se muestra al cliente en la página de diálogo del programa de arranque de red.<br />-[/RetentionPeriod]: especifica el número de días que un equipo puede estar en un estado pendiente antes de que se purguen automáticamente.<br />-[/ Aprobado: <time in days>]: especifica el período de retención para los equipos aprobados. Debe utilizar este parámetro con el **/RetentionPeriod** opción.<br />-[/ Otras personas: <time in days>]: especifica el período de retención para los equipos no aprobados (rechazado o pendiente). Debe utilizar este parámetro con el **/RetentionPeriod** opción.|
|[/AutoaddSettings]|Especifica la configuración predeterminada que se aplicará a cada equipo. Definir la configuración mediante las siguientes opciones:<br /><br />-/ Architecture: {x86 &#124; ia64 &#124; x64}-especifica la arquitectura.<br />-[/ BootProgram: <Relative path>]: especifica el programa de arranque que se envían al equipo aprobado. Si no se especifica ningún programa de arranque, se usará el valor predeterminado para la arquitectura del equipo (como se especifica en el servidor).<br />-[/ WdsClientUnattend: <Relative path>]-establece la ruta de acceso relativa al archivo de instalación desatendida que debería recibir el cliente aprobadas.<br />-[/ ReferralServer: <Server name>]: especifica el servidor de servicios de implementación de Windows que utilizará el cliente para descargar las imágenes.<br />-[/ Imagendearranque: <Relative path>]: especifica la imagen de arranque que va a recibir el cliente aprobadas.<br />-[/ Usuario: < DOMINIO\nombre de usuario &#124; User@Domain>]-establece permisos en el objeto de cuenta de equipo para proporcionar al usuario especificado los derechos necesarios para unir el equipo al dominio.<br />-[JoinRights: {JoinOnly &#124; completa}]: especifica el tipo de derechos que se asignará al usuario. **JoinOnly** requiere que el administrador restablecer la cuenta de equipo antes de que el usuario puede unir el equipo al dominio. **Completa** proporciona acceso total al usuario, incluido el derecho de unir el equipo al dominio.<br />-[/ JoinDomain: {Sí &#124; n}]: Especifica si el equipo debe estar unido al dominio como esta cuenta de equipo durante una instalación de servicios de implementación de Windows. El valor predeterminado es **Sí**.|
|[/BindPolicy]|Configura las interfaces de red para el proveedor PXE para que escuche en. Definir la directiva mediante las siguientes opciones:<br /><br />-[/ Directiva: {Include &#124; excluir}]-establece la directiva de enlace de la interfaz para incluir o excluir las direcciones en la lista de interfaces.<br />-[/ Agregar] - agrega una interfaz a la lista. También debe especificar/addressType y/Address.<br />-[Remove]: quita una interfaz de la lista. También debe especificar/addressType y/Address.<br />-direcciones /:<IP or MAC address> -especifica la dirección MAC o IP de la interfaz para agregar o quitar.<br />-/ addressType: {IP &#124; MAC}-indica el tipo de dirección especificada en el **/dirección** opción.|
|[/ RefreshPeriod: <seconds>]|Especifica la frecuencia (en segundos) en el servidor actualiza a su configuración.|
|[/BannedGuidPolicy]|Administra la lista de GUID prohibidos mediante las siguientes opciones:<br /><br />-[/ Agregar] / GUID:<GUID> -agrega el GUID especificado a la lista de GUID no permitidas. Cualquier cliente con este GUID se identifica mediante su dirección MAC en su lugar.<br />-/ GUID [Remove]:<GUID> -quita el GUID especificado de la lista de GUID no permitidas.|
|[/BcdRefreshPolicy]|Configura las opciones para actualizar los archivos de Bcd mediante las siguientes opciones:<br /><br />-[/ Enabled: {Sí &#124; n}]: especifica la actualización de directiva de Bcd. Cuando **/habilitada** está establecido en **Sí**, se actualizan los archivos de Bcd en el intervalo de tiempo especificado.<br />-[/ RefreshPeriod\:<time in minutes>]: especifica el intervalo de tiempo en el Bcd se actualizan los archivos.|
|[/Transport]|Configura las siguientes opciones\:<br /><br /><ul><li>[/ ObtainIpv4From: {Dhcp &#124; intervalo}]: especifica el origen de direcciones IPv4.<br /><br /><ul><li>[/ iniciar\: <starting Ipv4 address>]: especifica el inicio del intervalo de direcciones IP. Esta opción es necesaria y sólo es válido si **/ObtainIpv4From** está establecido en **intervalo**</li><li>[/ End\: <Ending Ipv4 address>]: especifica el final del intervalo de direcciones IP. Esta opción es necesaria y sólo es válido si **/ObtainIpv4From** está establecido en **intervalo**.</li></ul></li><li>[/ ObtainIpv6From:Range] [/ iniciar\:<start IP address>] [/ End<End IP address>] Especifica el origen de las direcciones IPv6. Esta opción solo se aplica a Windows Server 2008 R2 y el único valor admitido es el intervalo.</li><li>[/ puerto inicial <starting port>] especifica el inicio del intervalo de puertos.</li><li>[/ El puerto final <Ending port>] especifica el final del intervalo de puertos.</li><li>[/ Profile: {10Mbps &#124; 100Mbps &#124; 1 Gbps &#124; Custom}]: especifica el perfil de red que se usará. Esta opción es sólo compatible forservers que ejecuta Windows Server 2008.</li><li>[/MulticastSessionPolicy]  Configura las opciones de transferencia para transmisiones por multidifusión. Este comando solo está disponible para Windows Server 2008 R2.<br /><br /><ul><li>[/ Directiva: {None &#124; desconexión automática &#124; transmisión múltiple}]-determina cómo se controlan los clientes lentos. Ninguno significa mantener todos los clientes en una sesión de la misma velocidad. Desconexión automática significa que se desconectarán todos los clientes que caen por debajo de la /Threshold especificado. Transmisión múltiple significa que los clientes se dividirá en varias sesiones según lo especificado por /StreamCount.</li><li>[/ Umbral:<Speed in KBps>]: como /Policy:AutoDisconnect, este conjuntos de opciones la transferencia mínima velocidad en KBps. Los clientes que caen por debajo de esta velocidad se desconectará de las transmisiones por multidifusión.</li><li>[/ StreamCount: {2 &#124; 3}] [/ Reserva: {Sí &#124; n}]-/Policy:Multistream, esta opción determina el número de sesiones. 2 significa en dos sesiones (rápido y lento) 3 significa tres sesiones (lento, medio, rápido).</li><li>[/ Reserva: {Sí&#124; n}]-determina si los clientes que están desconectados continuará la transferencia mediante otro método (si se admite por el cliente). Si usa el cliente WDS, el equipo retrocederá para unidifusión. Wdsmcast.exe no admite un mecanismo de reserva. Esta opción también se aplica a los clientes que no admiten la transmisión múltiple. En ese caso, el equipo se revertirá a otro método en lugar de mover a una sesión de transferencia más lenta.</li></ul></li></ul>|
## <a name="BKMK_examples"></a>Ejemplos
Para configurar el servidor para responder a sólo clientes conocidos, con un retraso de respuesta de 4 minutos, escriba:
```
wdsutil /Set-Server /AnswerClients:Known /Responsedelay:4
```
Para establecer el programa de arranque y la arquitectura para el servidor, escriba:
```
wdsutil /Set-Server /BootProgram:boot\x86\pxeboot.n12 /Architecture:x86
```
Para habilitar el registro en el servidor, escriba:
```
wdsutil /Set-Server /WdsClientLogging /Enabled:Yes /LoggingLevel:Warnings
```
Para habilitar la instalación desatendida en el servidor, así como la arquitectura y el archivo de instalación desatendida de cliente, escriba:
```
wdsutil /Set-Server /WdsUnattend /Policy:Enabled /File:WDSClientUnattend \unattend.xml /Architecture:x86
```
Para establecer el servidor de entorno (PXE) de ejecución previo al arranque intentan enlazarse a los puertos TCP 67 y 60, escriba:
```
wdsutil /Set-server /UseDhcpPorts:No /DhcpOption60:Yes
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[mediante el comando disable-servidor](using-the-disable-server-command.md)
[mediante el comando enable-servidor](using-the-enable-server-command.md)
[mediante el Comando Get-Server](using-the-get-server-command.md)
[con el comando Initialize-Server](using-the-initialize-server-command.md)
[subcomando: iniciar el servidor](subcommand-start-server.md) 
 [ Subcomando: detención del servidor](subcommand-stop-server.md)
[la opción de servidor uninitialize](the-uninitialize-server-option.md)
