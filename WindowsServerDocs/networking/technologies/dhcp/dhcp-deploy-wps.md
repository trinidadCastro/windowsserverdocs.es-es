---
title: Deploy DHCP Using Windows PowerShell (Implementar DHCP mediante Windows PowerShell)
description: Puede utilizar este tema para implementar un servidor DHCP de protocolo de Internet de Windows Server 2016 (IP) versión 4 que proporciona direcciones IP automática y las opciones de DHCP a los clientes de DHCP IPv4 conectados a una o varias subredes de la red.
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: article
ms.assetid: 7110ad21-a33e-48d5-bb3c-129982913bc8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8a53f6293067fa0f7014e7794696cf75f7179545
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849096"
---
# <a name="deploy-dhcp-using-windows-powershell"></a>Deploy DHCP Using Windows PowerShell (Implementar DHCP mediante Windows PowerShell)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Esta guía proporciona instrucciones sobre cómo usar Windows PowerShell para implementar un protocolo de Internet (IP) versión 4 Dynamic Host Configuration Protocol \(DHCP\) opciones de servidor que asigne automáticamente direcciones IP y DHCP a DHCP IPv4 clientes que están conectados a una o varias subredes de la red.

>[!NOTE]
>Para descargar este documento en formato de Word desde la Galería de TechNet, consulte [implementar DHCP con Windows PowerShell en Windows Server 2016](https://gallery.technet.microsoft.com/Deploy-DHCP-Using-Windows-246dd293).

Uso de servidores DHCP para asignar dirección IP direcciones que se guarda en sobrecarga administrativa porque no es necesario realizar manualmente la configuración TCP/IP v4 para cada adaptador de red en cada equipo de la red. Con DHCP, configuración de TCP/IP v4 se realiza automáticamente cuando un equipo u otro cliente DHCP está conectado a la red.

Puede implementar el servidor DHCP en un grupo de trabajo como un servidor independiente o como parte de un dominio de Active Directory.

En esta guía se incluyen las siguientes secciones.

- [Introducción a la implementación de DHCP](#bkmk_overview)
- [Introducción a las tecnologías](#bkmk_technologies)
- [Planear la implementación de DHCP](#bkmk_plan)
- [Uso de esta guía en un laboratorio de pruebas](#bkmk_lab)
- [Implementar DHCP](#bkmk_deploy)
- [Comprobar la funcionalidad de servidor](#bkmk_verify)
- [Comandos de Windows PowerShell para DHCP](#bkmk_dhcpwps)
- [Lista de comandos de Windows PowerShell en esta guía](#bkmk_list)

## <a name="bkmk_overview"></a>Introducción a la implementación de DHCP

La siguiente ilustración muestra el escenario que se puede implementar mediante el uso de esta guía. El escenario incluye un servidor DHCP en un dominio de Active Directory. El servidor está configurado para proporcionar direcciones IP a los clientes DHCP en dos subredes diferentes. Las subredes se separan mediante un enrutador que tiene habilitado el reenvío de DHCP.

![Introducción a la topología de red DHCP](../../media/Core-Network-Guide/cng16_overview.jpg)

## <a name="bkmk_technologies"></a>Introducción a las tecnologías

Las secciones siguientes proporcionan información general breve de DHCP y TCP/IP.

### <a name="dhcp-overview"></a>Introducción a DHCP

DHCP es un estándar IP que sirve para simplificar la administración de la configuración IP del host. El estándar DHCP ofrece el uso de servidores DHCP como una forma de administrar la asignación dinámica de direcciones IP y demás detalles de configuración relacionados para los clientes habilitados para DHCP de la red.

DHCP le permite utilizar un servidor DHCP para asignar dinámicamente una dirección IP a un equipo u otro dispositivo, como una impresora, en la red local, en lugar de configurar manualmente todos los dispositivos con una dirección IP estática.

Cada equipo de una red TCP/IP debe tener una dirección IP única, ya que la dirección IP y su máscara de subred relacionada identifican el equipo host y la subred a la cual está conectado el equipo. Si usa DHCP, puede estar seguro de que todos los equipos que están configurados como clientes DHCP reciben una dirección IP que sea apropiada para la ubicación de red y la subred; además, al usar las opciones de DHCP, como puertas de enlace y servidores DNS predeterminados, puede proporcionar de forma automática a los clientes de DHCP la información que necesitan para funcionar correctamente en la red.

Para las redes basadas en TCP/IP, DHCP reduce la complejidad y la cantidad de trabajo administrativa relacionada con la configuración de equipos.

### <a name="tcpip-overview"></a>Información general de TCP/IP

De forma predeterminada, todas las versiones de sistemas operativos cliente de Windows y Windows Server tienen la configuración de TCP/IP IP versión 4 las conexiones de red configuradas para obtener automáticamente una dirección IP y otra información, denominado opciones de DHCP, desde un servidor DHCP. Por este motivo, no es necesario configurar manualmente la configuración TCP/IP a menos que el equipo es un equipo de servidor u otro dispositivo que requiere una dirección IP estática configurada manualmente. 

Por ejemplo, se recomienda que configure manualmente la dirección IP del servidor DHCP y las direcciones IP de los servidores DNS y controladores de dominio que ejecutan Active Directory Domain Services \(AD DS\).

TCP/IP en Windows Server 2016 es la siguiente:

-   Software de red basado en protocolos de red estándar del sector.

-   Un protocolo de red corporativa enrutable que admite la conexión del equipo basado en Windows en entornos de red de área local (LAN) y de red de área extensa (WAN).

-   Tecnologías y utilidades principales para la conexión del equipo basado en Windows con sistemas distintos con el objetivo de compartir información.

-   Una base para obtener acceso a los servicios globales de Internet, como servidores Web y el protocolo de transferencia de archivos (FTP).

-   Un marco cliente/servidor entre plataformas escalable y sólido.

TCP/IP proporciona utilidades de TCP/IP básicas que permiten a los equipos basados en Windows conectarse y compartir información con otros sistemas de Microsoft y sistemas que no son de Microsoft, incluidos:

- Windows Server 2016

- Windows 10

- Windows Server 2012 R2

- Windows 8.1

- Windows Server 2012

- Windows 8

- Windows Server 2008 R2

- Windows 7

- Windows Server 2008

- Windows Vista

- Hosts de Internet

- Sistemas Apple Macintosh

- Grandes sistemas (mainframes) IBM

- Sistemas UNIX y Linux

- Sistemas OpenVMS

- Impresoras de red

- Tabletas y teléfonos celulares con Ethernet con cable o tecnología de 802.11 inalámbrica habilitada

## <a name="bkmk_plan"></a>Planear la implementación de DHCP

Estos son los pasos de planeación claves antes de instalar el rol de servidor DHCP.

### <a name="planning-dhcp-servers-and-dhcp-forwarding"></a>Planear servidores DHCP y reenvío DHCP

Como los mensajes DHCP son mensajes de difusión, los enrutadores no los reenvían entre subredes. Si tiene varias subredes y desea proporcionar el servicio DHCP para cada subred, realice una de las acciones siguientes:

-   Instalar un servidor DHCP en cada subred

-   Configurar los enrutadores para reenviar los mensajes de difusión DHCP entre subredes y configurar múltiples ámbitos en el servidor DHCP, un ámbito por subred.

En la mayoría de los casos, configurar los enrutadores para reenviar mensajes de difusión DHCP es más rentable que implementar un servidor DHCP en cada segmento físico de la red.

### <a name="planning-ip-address-ranges"></a>Planear intervalos de direcciones IP

Cada subred debe tener su propio intervalo de direcciones IP únicas. En un servidor DHCP, dichos intervalos se representan con ámbitos.

Un ámbito es una agrupación administrativa de direcciones IP para equipos de una subred que usa el servicio DHCP. El administrador crea primero un ámbito para cada subred física y, a continuación, lo usa para definir los parámetros usados por los clientes.

Un ámbito tiene las siguientes propiedades:

-   Un intervalo de direcciones IP desde el que incluir o excluir las direcciones usadas para las ofertas de concesión de servicio DHCP.

-   Una máscara de subred, que determina el prefijo de subred para una dirección IP determinada.

-   Un nombre de ámbito asignado al crearlo.

-   Valores de duración de la concesión, asignados a los clientes DHCP que reciben las direcciones IP asignadas dinámicamente.

-   Todas las opciones de ámbito DHCP configuradas para la asignación a clientes DHCP (por ejemplo, dirección IP del servidor DNS y dirección IP de la puerta de enlace predeterminada o enrutador).

-   Las reservas se usan opcionalmente para garantizar que un cliente DHCP reciba siempre la misma dirección IP.

Antes de implementar los servidores, cree una lista con las subredes y los intervalos de direcciones IP que desea usar para cada subred.

### <a name="planning-subnet-masks"></a>Planear máscaras de subred

Las máscaras de subred sirven para distinguir los identificadores de red de los identificadores de host dentro de una dirección IP. Cada máscara de subred es un número de 32 bits que usa grupos de bits consecutivos de todo unos (1) para reconocer el identificador de red y de todo ceros (0) para reconocer las partes del identificador de host de una dirección IP.

Por ejemplo, la máscara de subred que se usa normalmente con la dirección IP 131.107.16.200 es el siguiente número binario de 32 bits:

```
11111111 11111111 00000000 00000000
```

Este número de máscara de subred es 16 bits de uno seguidos de 16 bits cero, que indica que las secciones de Id. de identificador y el host de red de esta dirección IP tienen ambas 16 bits de longitud. Normalmente, esta máscara de subred se muestra en notación decimal con puntos como 255.255.0.0.

La siguiente tabla muestra máscaras de subred para las clases de direcciones de Internet.

|Clase de dirección|Bits para la máscara de subred|Máscara de subred|
|-----------------|------------------------|---------------|
|Clase A|11111111 00000000 00000000 00000000|255.0.0.0|
|Clase B|11111111 11111111 00000000 00000000|255.255.0.0|
|Clase C|11111111 11111111 11111111 00000000|255.255.255.0|

Cuando se crea un ámbito en DHCP y se escribe el intervalo de direcciones IP para el ámbito, DHCP proporciona estos valores predeterminados para las máscaras de subred. Por lo general, los valores de la máscara de subred predeterminados son aceptables para la mayoría de las redes que no tienen requisitos especiales y donde cada segmento de red IP corresponde a una sola red física.

En ciertos casos, se pueden usar máscaras de subred personalizadas para implementar las subredes IP. Con el establecimiento de subredes IP, se puede subdividir la parte del identificador de host predeterminada de una dirección IP para especificar subredes, que son subdivisiones del identificador de red basado en clases original.

Mediante la personalización de la longitud de la máscara de subred, se puede reducir el número de bits que se usan para el identificador de host real.

Para evitar problemas de direccionamiento y enrutamiento, debería asegurarse de que todos los equipos TCP/IP de un segmento de red usan la misma máscara de subred y de que cada equipo o dispositivo tenga una dirección IP única.

### <a name="planning-exclusion-ranges"></a>Planear intervalos de exclusión

Cuando se crea un ámbito en un servidor DHCP, se especifica un intervalo de direcciones IP que incluye todas las direcciones IP que el servidor DHCP está autorizado a conceder a clientes DHCP, como equipos y otros dispositivos. Si después configura manualmente algunos servidores y otros dispositivos con direcciones IP estáticas del mismo intervalo de direcciones IP que está usando el servidor DHCP, puede crear accidentalmente un conflicto de dirección IP en el que usted y el servidor DHCP tienen asignada la misma dirección IP para diferentes dispositivos.

Para solucionar este problema, puede crear un intervalo de exclusión para el ámbito DHCP. Un intervalo de exclusión es un contiguos intervalo de direcciones IP dentro del intervalo de direcciones IP del ámbito que no se puede usar el servidor DHCP. Si crea un intervalo de exclusión, el servidor DHCP no asigna las direcciones en ese intervalo, lo cual le permite asignar manualmente estas direcciones sin crear un conflicto de direcciones IP.

El servidor DHCP puede excluir direcciones IP de la distribución creando un intervalo de exclusión para cada ámbito. Las exclusiones se deberían usar para todos los dispositivos que están configurados con una dirección IP estática. Las direcciones excluidas deberían incluir todas las direcciones IP asignadas manualmente a otros servidores, clientes no DHCP, estaciones de trabajo sin disco o clientes PPP y de Enrutamiento y acceso remoto.

Se recomienda configurar el intervalo de exclusión con direcciones adicionales en previsión de una futura ampliación de la red. En la tabla siguiente proporciona un intervalo de exclusión de ejemplo para un ámbito con un intervalo de direcciones IP de 10.0.0.1 - 10.0.0.254 y una máscara de subred de 255.255.255.0.

|Elementos de configuración|Valores de ejemplo|
|-----------------------|------------------|
|Dirección IP inicial del intervalo de exclusión|10.0.0.1|
|Dirección IP final del intervalo de exclusión|10.0.0.25|

### <a name="planning-tcpip-static-configuration"></a>Planear la configuración estática de TCP/IP

Algunos dispositivos, como enrutadores, servidores DHCP y servidores DNS, se deben configurar con una dirección IP estática. Además, es posible que tenga dispositivos adicionales, como impresoras, para los que desee asegurarse de que tengan siempre la misma dirección IP. Reúna en una lista los dispositivos que desee configurar estáticamente para cada subred y, a continuación, planee el intervalo de exclusión que desea usar en el servidor DHCP para asegurarse de que el servidor DHCP no conceda la dirección IP de un dispositivo configurado estáticamente. Un intervalo de exclusión es una secuencia limitada de direcciones IP dentro de un ámbito que está excluida de las ofertas del servicio DHCP. Los intervalos de exclusión garantizan que el servidor no ofrece ninguna de las direcciones incluidas en esos intervalos a los clientes DHCP de la red.

Por ejemplo, si el intervalo de direcciones IP para una subred va de 192.168.0.1 a 192.168.0.254 y tiene diez dispositivos que desea configurar con una dirección IP estática, puede crear un intervalo de exclusión para el ámbito 192.168.0.*x* que incluya diez o más direcciones IP: 192.168.0.1 a 192.168.0.15.

En este ejemplo, se usan diez de las direcciones IP excluidas para configurar servidores y otros dispositivos con direcciones IP estáticas, y quedan cinco direcciones IP más disponibles para la configuración estática de nuevos dispositivos que puede que quiera agregar en el futuro. Con este intervalo de exclusión, se ha dejado al servidor DHCP con un grupo de direcciones que oscila entre 192.168.0.16 y 192.168.0.254.

Elementos de configuración de ejemplo adicional para AD DS y DNS se proporcionan en la tabla siguiente.

|Elementos de configuración|Valores de ejemplo|
|-----------------------|------------------|
|Enlaces de conexión de red|Ethernet|
|Configuración del servidor DNS|DC1.corp.contoso.com|
|Dirección IP del servidor DNS preferido|10.0.0.2|
|Valores de ámbito<br /><br />1.  Nombre de ámbito<br />2.  Dirección IP inicial<br />3.  Dirección IP final<br />4.  Máscara de subred<br />5.  Puerta de enlace predeterminada (opcional)<br />6.  Duración de la concesión|1.  Subred principal<br />2.  10.0.0.1<br />3.  10.0.0.254<br />4.  255.255.255.0<br />5.  10.0.0.1<br />6. 8 días|
|Modo de funcionamiento del servidor DHCP IPv6|No habilitado|

## <a name="bkmk_lab"></a>Uso de esta guía en un laboratorio de pruebas

Puede usar a esta guía para implementar DHCP en un laboratorio de pruebas antes de implementar en un entorno de producción. 

>[!NOTE]
>Si no desea implementar DHCP en un laboratorio de pruebas, puede omitir la sección [implementar DHCP](#bkmk_deploy).

Los requisitos para el laboratorio difieren dependiendo de si usa servidores físicos o máquinas virtuales \(máquinas virtuales\), y si se usa un dominio de Active Directory o implementar un servidor independiente. 

Puede usar la siguiente información para determinar los recursos mínimos que necesita para probar con esta guía de implementación de DHCP.

### <a name="test-lab-requirements-with-vms"></a>Requisitos del laboratorio de pruebas con máquinas virtuales

Para implementar DHCP en un laboratorio de pruebas con máquinas virtuales, necesita los siguientes recursos.

Para una implementación de dominio o en la implementación independiente, necesita un servidor que está configurado como un Hyper\-host V.

**Implementación de dominio**

Esta implementación requiere un servidor físico, un conmutador virtual, dos servidores virtuales y un cliente virtual:

En el servidor físico, en el Administrador de Hyper-V, cree los siguientes elementos.

1. Una **interno** conmutador virtual. No cree un **externo** conmutador virtual, porque si su Hyper\-host V está en una subred que incluye un servidor DHCP, la prueba de las máquinas virtuales recibirán una dirección IP del servidor DHCP. Además, el servidor DHCP de prueba que implemente podría asignar direcciones IP a otros equipos de la subred donde Hyper\-host V está instalado.
1. Una VM que ejecute Windows Server 2016 está configurado como un controlador de dominio con Active Directory Domain Services que está conectado al conmutador virtual interno que creó. Para que coincida con esta guía, este servidor debe tener una dirección IP configurada estáticamente 10.0.0.2. Para obtener información sobre la implementación de AD DS, consulte la sección **implementar DC1** en Windows Server 2016 [Guía de red principal](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployADDNS01).
1. Una máquina virtual que ejecuta Windows Server 2016 que se configurará como un servidor DHCP mediante el uso de esta guía y que está conectado a la interna virtual cambie que ha creado. 
1. Una VM que ejecute un sistema operativo de cliente de Windows que está conectado a la interna virtual switch que ha creado y que utilizará para comprobar que el servidor DHCP es asignar dinámicamente direcciones IP y opciones de DHCP a los clientes DHCP.

**Implementación del servidor DHCP Standalone**

Esta implementación requiere un servidor físico, un conmutador virtual, un servidor virtual y un cliente virtual:

En el servidor físico, en el Administrador de Hyper-V, cree los siguientes elementos.

1. Una **interno** conmutador virtual. No cree un **externo** conmutador virtual, porque si su Hyper\-host V está en una subred que incluye un servidor DHCP, la prueba de las máquinas virtuales recibirán una dirección IP del servidor DHCP. Además, el servidor DHCP de prueba que implemente podría asignar direcciones IP a otros equipos de la subred donde Hyper\-host V está instalado.
1. Una máquina virtual que ejecuta Windows Server 2016 que se configurará como un servidor DHCP mediante el uso de esta guía y que está conectado a la interna virtual cambie que ha creado.
1. Una VM que ejecute un sistema operativo de cliente de Windows que está conectado a la interna virtual switch que ha creado y que utilizará para comprobar que el servidor DHCP es asignar dinámicamente direcciones IP y opciones de DHCP a los clientes DHCP.

### <a name="test-lab-requirements-with-physical-servers"></a>Requisitos del laboratorio de pruebas con servidores físicos

Para implementar DHCP en un laboratorio de pruebas con servidores físicos, necesita los siguientes recursos.

**Implementación de dominio**

Esta implementación requiere un concentrador o conmutador, dos servidores físicos y un cliente físico:

1. Un concentrador o conmutador Ethernet al que se pueden conectar los equipos físicos con cables Ethernet
1. Un equipo físico que ejecuta Windows Server 2016 está configurado como un controlador de dominio con Active Directory Domain Services. Para que coincida con esta guía, este servidor debe tener una dirección IP configurada estáticamente 10.0.0.2. Para obtener información sobre la implementación de AD DS, consulte la sección **implementar DC1** en Windows Server 2016 [Guía de red principal](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployADDNS01).
1. Un equipo físico que ejecuta Windows Server 2016 que configure como un servidor DHCP mediante el uso de esta guía. 
1. Un equipo físico que ejecuta un sistema operativo de cliente de Windows que va a utilizar para comprobar que el servidor DHCP es asignar dinámicamente direcciones IP y opciones de DHCP a los clientes DHCP.

>[!NOTE]
>Si no tiene suficientes máquinas de prueba para esta implementación, puede usar una máquina de prueba para AD DS y DHCP - sin embargo, esta configuración no se recomienda para un entorno de producción.

**Implementación del servidor DHCP Standalone**

Esta implementación requiere un concentrador o conmutador, un servidor físico y un cliente físico:

1. Un concentrador o conmutador Ethernet al que se pueden conectar los equipos físicos con cables Ethernet
2. Un equipo físico que ejecuta Windows Server 2016 que configure como un servidor DHCP mediante el uso de esta guía. 
3. Un equipo físico que ejecuta un sistema operativo de cliente de Windows que va a utilizar para comprobar que el servidor DHCP es asignar dinámicamente direcciones IP y opciones de DHCP a los clientes DHCP.


## <a name="bkmk_deploy"></a>Implementar DHCP

Esta sección proporciona los comandos de Windows PowerShell de ejemplo que puede usar para implementar DHCP en un servidor. Antes de ejecutar estos comandos de ejemplo en el servidor, debe modificar los comandos para que coincida con la red y el entorno. 

Por ejemplo, antes de ejecutar los comandos, debe reemplazar los valores de ejemplo en los comandos para los siguientes elementos:

- Nombres de equipo
- Intervalo de direcciones IP para cada ámbito que desea configurar (1 ámbito por subred)
- Máscara de subred para cada intervalo de direcciones IP que desea configurar
- Nombre de ámbito para cada ámbito
- Intervalo de exclusión para cada ámbito
- Valores de opción de DHCP, como puerta de enlace predeterminada, el nombre de dominio y servidores DNS o WINS
- Nombres de interfaz

>[!IMPORTANT]
>Examinar y modificar todos los comandos para su entorno antes de ejecutar el comando.

### <a name="where-to-install-dhcp---on-a-physical-computer-or-a-vm"></a>¿Dónde instalar DHCP - en un equipo físico o una máquina virtual?

Puede instalar el rol de servidor DHCP en un equipo físico o en una máquina virtual \(VM\) que está instalado en un Hyper\-host V. Si va a instalar DHCP en una máquina virtual y desea que el servidor DHCP para proporcionar asignaciones de direcciones IP a equipos en la red física a la que está conectado el host de Hyper-V, debe conectar el adaptador de red virtual de la máquina virtual a un conmutador Virtual de Hyper-V que está **Externo**.

Para obtener más información, consulte la sección **crear un conmutador Virtual con el Administrador de Hyper-V** en el tema [crear una red virtual](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/connect-to-network).

### <a name="run-windows-powershell-as-an-administrator"></a>Ejecutar Windows PowerShell como administrador

Puede usar el siguiente procedimiento para ejecutar Windows PowerShell con privilegios de administrador.

1. En un equipo que ejecuta Windows Server 2016, haga clic en **iniciar**, a continuación, haga clic en el icono de Windows PowerShell. Aparece un menú. 

2. En el menú, haga clic en **más**y, a continuación, haga clic en **ejecutar como administrador**. Si se le solicite, escriba las credenciales de una cuenta que tenga privilegios de administrador en el equipo. Si la cuenta de usuario con el que ha iniciado sesión el equipo es una cuenta de nivel de administrador, no recibirá una petición de credenciales.

3. Se abre Windows PowerShell con privilegios de administrador.

### <a name="rename-the-dhcp-server-and-configure-a-static-ip-address"></a>Cambie el nombre del servidor DHCP y configurar una dirección IP estática

Si aún no lo ha hecho, puede usar los siguientes comandos de Windows PowerShell para cambiar el nombre del servidor DHCP y configurar una dirección IP estática para el servidor.

**Configurar una dirección IP estática**

Puede usar los comandos siguientes para asignar una dirección IP estática para el servidor DHCP y para configurar las propiedades de TCP/IP del servidor DHCP con la dirección IP de servidor DNS correcta. También debe reemplazar los nombres de interfaz y las direcciones IP de este ejemplo por los valores que quiera usar para configurar el equipo.

 `New-NetIPAddress -IPAddress 10.0.0.3 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24`

 `Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 10.0.0.2`

Para obtener más información acerca de estos comandos, vea los temas siguientes.

- [New-NetIPAddress](https://technet.microsoft.com/itpro/powershell/windows/tcpip/new-netipaddress)
- [Set-DnsClientServerAddress](https://technet.microsoft.com/itpro/powershell/windows/dns-client/set-dnsclientserveraddress)

**El nombre del equipo**

Puede usar los comandos siguientes para cambiar el nombre y, a continuación, reinicie el equipo.

`Rename-Computer -Name DHCP1`

 `Restart-Computer`

Para obtener más información acerca de estos comandos, vea los temas siguientes.

- [Rename-Computer](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/rename-computer)
- [Restart-Computer](https://msdn.microsoft.com/powershell/reference/4.0/microsoft.powershell.management/restart-computer)

### <a name="join-the-computer-to-the-domain-optional"></a>Unir el equipo al dominio \(opcional\)

Si va a instalar al servidor DHCP en un entorno de dominio de Active Directory, debe unir el equipo al dominio. Abra Windows PowerShell con privilegios de administrador y, a continuación, ejecute el comando siguiente después de reemplazar el nombre NetBios del dominio **CORP** con un valor que sea adecuado para su entorno.

    Add-Computer CORP

Cuando se le solicite, escriba las credenciales de una cuenta de usuario de dominio que tenga permiso para unir un equipo al dominio. 

    Restart-Computer

Para obtener más información acerca del comando Add-Computer, vea el tema siguiente.

- [Add-Computer](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/add-computer)

### <a name="install-dhcp"></a>Instalar DHCP

Una vez reiniciado el equipo, abra Windows PowerShell con privilegios de administrador y, a continuación, instalar DHCP ejecutando el comando siguiente.

    Install-WindowsFeature DHCP -IncludeManagementTools

Para obtener más información sobre este comando, vea el tema siguiente.

- [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/server-manager/install-windowsfeature)

### <a name="create-dhcp-security-groups"></a>Crear grupos de seguridad DHCP

Para crear grupos de seguridad, debe ejecutar un Shell de red \(netsh\) comando en Windows PowerShell y, a continuación, reinicie el servicio DHCP para que los nuevos grupos de activarán.

Al ejecutar el siguiente comando netsh en el servidor DHCP, el **administradores de DHCP** y **usuarios de DHCP** grupos de seguridad se crean en **usuarios y grupos locales** en el protocolo DHCP servidor.

    netsh dhcp add securitygroups

El siguiente comando reinicia el servicio DHCP en el equipo local.

    Restart-service dhcpserver

Para obtener más información acerca de estos comandos, vea los temas siguientes.

- [Shell de red (Netsh)](../netsh/netsh.md)
- [Restart-Service](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/restart-service)

### <a name="authorize-the-dhcp-server-in-active-directory-optional"></a>Autorizar el servidor DHCP en Active Directory \(opcional\)

Si va a instalar DHCP en un entorno de dominio, debe realizar los pasos siguientes para autorizar el servidor DHCP para que funcione en el dominio.

>[!NOTE]
>Los servidores DHCP no autorizados que se instalan en los dominios de Active Directory no pueden funcionar correctamente y no conceder direcciones IP a los clientes DHCP. La desactivación automática de servidores DHCP no autorizados es una característica de seguridad que impide que los servidores DHCP no autorizados se asignen direcciones IP incorrectas a los clientes de la red.

Puede usar el siguiente comando para agregar el servidor DHCP a la lista de servidores DHCP autorizados en Active Directory. 

>[!NOTE]
>Si no tiene un entorno de dominio, no ejecute este comando.

 `Add-DhcpServerInDC -DnsName DHCP1.corp.contoso.com -IPAddress 10.0.0.3`

Para comprobar que el servidor DHCP está autorizado en Active Directory, puede usar el siguiente comando.

    Get-DhcpServerInDC

Estos son los resultados de ejemplo que se muestran en Windows PowerShell.

    
        IPAddress   DnsName
        ---------   -------
        10.0.0.3    DHCP1.corp.contoso.com
    

Para obtener más información acerca de estos comandos, vea los temas siguientes.

- [Add-DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverindc)
- [Get-DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/get-dhcpserverindc)

### <a name="notify-server-manager-that-post-install-dhcp-configuration-is-complete-optional"></a>Notificar al administrador del servidor que registre\-completada la configuración de DHCP de instalación \(opcional\)

Después de haber completado post\-tareas de instalación, como la creación de grupos de seguridad y autorizar el servidor DHCP en Active Directory, el administrador del servidor todavía podrían mostrar una alerta en la interfaz de usuario que indica que esa publicación\- mediante el Asistente para la configuración de instalación posterior de DHCP, se deben completar los pasos de instalación.

Puede evitarlo ahora\-innecesario e imprecisa mensaje aparezca en el administrador del servidor mediante la configuración de la siguiente clave del registro con este comando de Windows PowerShell.

    Set-ItemProperty –Path registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ServerManager\Roles\12 –Name ConfigurationState –Value 2

Para obtener más información sobre este comando, vea el tema siguiente.

- [Set-ItemProperty](https://msdn.microsoft.com/powershell/reference/4.0/microsoft.powershell.management/set-itemproperty?f=255&MSPPError=-2147217396)

### <a name="set-server-level-dns-dynamic-update-configuration-settings-optional"></a>Establecer opciones de configuración de actualización dinámica de servidor nivel DNS \(opcional\)

Si desea que el servidor DHCP para realizar actualizaciones dinámicas de DNS para clientes DHCP, puede ejecutar el comando siguiente para configurar esta opción. Se trata de un configuración, no un ámbito nivel establecer, lo que afectará a todos los ámbitos que se configuran en el servidor de nivel de servidor. Este comando de ejemplo también configura el servidor DHCP para eliminar registros de recursos DNS para los clientes cuando el cliente menos expira.

    Set-DhcpServerv4DnsSetting -ComputerName "DHCP1.corp.contoso.com" -DynamicUpdates "Always" -DeleteDnsRRonLeaseExpiry $True

Puede usar el comando siguiente para configurar las credenciales que utiliza el servidor DHCP para registrar o anular el registro de los registros de cliente en un servidor DNS. En este ejemplo se guarda una credencial en un servidor DHCP. El primer comando usa **Get-Credential** para crear un **PSCredential** objeto y, a continuación, almacena el objeto en el **$Credential** variable. El comando le pide el nombre de usuario y contraseña, así que asegúrese de que proporcione credenciales para una cuenta que tenga permiso para actualizar los registros de recursos en el servidor DNS.

    
    $Credential = Get-Credential
    Set-DhcpServerDnsCredential -Credential $Credential -ComputerName "DHCP1.corp.contoso.com"
    

Para obtener más información acerca de estos comandos, vea los temas siguientes.

- [Set-DhcpServerv4DnsSetting](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverv4dnssetting)
- [Set-DhcpServerDnsCredential](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverdnscredential)

### <a name="configure-the-corpnet-scope"></a>Configurar el ámbito de la red corporativa

Una vez completada la instalación de DHCP, puede usar los comandos siguientes para configurar y activar el ámbito de la red corporativa, crear un intervalo de exclusión para el ámbito y configurar la puerta de enlace de predeterminada de las opciones de DHCP, la dirección IP del servidor DNS y el nombre de dominio DNS.

    
    Add-DhcpServerv4Scope -name "Corpnet" -StartRange 10.0.0.1 -EndRange 10.0.0.254 -SubnetMask 255.255.255.0 -State Active`
    
    Add-DhcpServerv4ExclusionRange -ScopeID 10.0.0.0 -StartRange 10.0.0.1 -EndRange 10.0.0.15`
    
    Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.0.1 -ScopeID 10.0.0.0 -ComputerName DHCP1.corp.contoso.com`
    
    Set-DhcpServerv4OptionValue -DnsDomain corp.contoso.com -DnsServer 10.0.0.2
    

Para obtener más información acerca de estos comandos, vea los temas siguientes.

- [Add-DhcpServerv4Scope](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverv4scope)
- [Add-DhcpServerv4ExclusionRange](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverv4exclusionrange)
- [Set-DhcpServerv4OptionValue](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverv4optionvalue)

### <a name="configure-the-corpnet2-scope-optional"></a>Configurar el ámbito Corpnet2 \(opcional\)

Si tiene una segunda subred que está conectada a la primera subred con un enrutador que está habilitado el reenvío de DHCP, puede usar los comandos siguientes para agregar un segundo ámbito denominado Corpnet2 en este ejemplo. Este ejemplo también configura un intervalo de exclusión y la dirección IP de puerta de enlace predeterminada \(la dirección IP del enrutador en la subred\) de la subred Corpnet2.

 `Add-DhcpServerv4Scope -name "Corpnet2" -StartRange 10.0.1.1 -EndRange 10.0.1.254 -SubnetMask 255.255.255.0 -State Active`

 `Add-DhcpServerv4ExclusionRange -ScopeID 10.0.1.0 -StartRange 10.0.1.1 -EndRange 10.0.1.15`

 `Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.1.1 -ScopeID 10.0.1.0 -ComputerName DHCP1.corp.contoso.com`

Si tiene subredes adicionales que son atendidas por este servidor DHCP, puede repetir estos comandos, con diferentes valores para todos los parámetros de comando para agregar los ámbitos para cada subred.

>[!IMPORTANT]
>Asegúrese de que todos los enrutadores entre los clientes DHCP y el servidor DHCP están configurados para reenvío de mensajes DHCP. Consulte la documentación del enrutador para obtener información sobre cómo configurar el reenvío de DHCP.

## <a name="bkmk_verify"></a>Comprobar la funcionalidad de servidor

Para comprobar que el servidor DHCP proporciona la asignación dinámica de direcciones IP a los clientes DHCP, puede conectar a otro equipo a una subred con servicio. Después de conectar el cable Ethernet para el adaptador de red y encender el equipo, solicitará una dirección IP del servidor DHCP. Puede comprobar una configuración correcta mediante la **ipconfig/all** comando y revisar los resultados o mediante la realización de pruebas de conectividad, como intentar obtener acceso a recursos Web con los recursos compartidos de archivo o el explorador con Windows El explorador u otras aplicaciones.

Si el cliente no recibe una dirección IP del servidor DHCP, realice los pasos de solución de problemas siguientes.

1. Asegúrese de que el cable Ethernet esté enchufado tanto el equipo y la Ethernet conmutador, concentrador o enrutador.
1. Si conecta el equipo cliente en un segmento de red que está separado del servidor DHCP mediante un enrutador, asegúrese de que el enrutador está configurado para reenviar mensajes DHCP.
1. Asegúrese de que el servidor DHCP está autorizado en Active Directory ejecutando el siguiente comando para recuperar la lista de servidores DHCP autorizados de Active Directory. [Get-DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/get-dhcpserverindc).
1. Asegúrese de que se activan los ámbitos, abra la consola de DHCP \(administrador del servidor, **herramientas**, **DHCP**\), a continuación, expandir el árbol de servidores para revisar los ámbitos, derecha\-al hacer clic en cada ámbito. Si el menú resultante incluye la selección **activar**, haga clic en **activar**. \(Si el ámbito ya está activado, la selección de menú lee **Deactivate**.\) 

## <a name="bkmk_dhcpwps"></a>Comandos de Windows PowerShell para DHCP

La siguiente referencia proporciona descripciones de comandos y la sintaxis para todos los comandos de PowerShell de Windows del servidor de DHCP de Windows Server 2016. Este tema enumeran los comandos en orden alfabético según el verbo al principio de los comandos, como **obtener** o **establecer**.

>[!NOTE]
>No se pueden usar comandos de Windows Server 2016 en Windows Server 2012 R2.

- [Módulo DhcpServer](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/index)

La siguiente referencia proporciona descripciones de comandos y la sintaxis para todos los comandos de PowerShell de Windows del servidor de DHCP para Windows Server 2012 R2. Este tema enumeran los comandos en orden alfabético según el verbo al principio de los comandos, como **obtener** o **establecer**.

>[!NOTE]
>Puede usar los comandos de Windows Server 2012 R2 en Windows Server 2016.

- [Cmdlets del servidor DHCP en Windows PowerShell](https://technet.microsoft.com/library/jj590751.aspx)

## <a name="bkmk_list"></a>Lista de comandos de Windows PowerShell en esta guía

Siguiente es una simple lista de comandos y valores de ejemplo que se usan en esta guía.

    
    New-NetIPAddress -IPAddress 10.0.0.3 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24
    Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 10.0.0.2
    Rename-Computer -Name DHCP1
    Restart-Computer
    
    Add-Computer CORP
    Restart-Computer
    
    Install-WindowsFeature DHCP -IncludeManagementTools
    netsh dhcp add securitygroups
    Restart-service dhcpserver
    
    Add-DhcpServerInDC -DnsName DHCP1.corp.contoso.com -IPAddress 10.0.0.3
    Get-DhcpServerInDC
    
    Set-ItemProperty –Path registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ServerManager\Roles\12 –Name ConfigurationState –Value 2
    
    Set-DhcpServerv4DnsSetting -ComputerName "DHCP1.corp.contoso.com" -DynamicUpdates "Always" -DeleteDnsRRonLeaseExpiry $True
    
    $Credential = Get-Credential
    Set-DhcpServerDnsCredential -Credential $Credential -ComputerName "DHCP1.corp.contoso.com"
    
    rem At prompt, supply credential in form DOMAIN\user, password
    
    
    rem Configure scope Corpnet
    
    Add-DhcpServerv4Scope -name "Corpnet" -StartRange 10.0.0.1 -EndRange 10.0.0.254 -SubnetMask 255.255.255.0 -State Active
    
    Add-DhcpServerv4ExclusionRange -ScopeID 10.0.0.0 -StartRange 10.0.0.1 -EndRange 10.0.0.15
    
    Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.0.1 -ScopeID 10.0.0.0 -ComputerName DHCP1.corp.contoso.com
    
    Set-DhcpServerv4OptionValue -DnsDomain corp.contoso.com -DnsServer 10.0.0.2
    
    rem Configure scope Corpnet2
    
    Add-DhcpServerv4Scope -name "Corpnet2" -StartRange 10.0.1.1 -EndRange 10.0.1.254 -SubnetMask 255.255.255.0 -State Active
    
    Add-DhcpServerv4ExclusionRange -ScopeID 10.0.1.0 -StartRange 10.0.1.1 -EndRange 10.0.1.15
    
    Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.1.1 -ScopeID 10.0.1.0 -ComputerName DHCP1.corp.contoso.com
    


