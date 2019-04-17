---
title: Implementar DHCP mediante Windows PowerShell
description: Puedes usar este tema para implementar un servidor DHCP versión 4 de protocolo de Internet de Windows Server 2016 (IP) que proporciona opciones de DHCP y direcciones IP automáticas a los clientes de DHCP IPv4 conectados a una o varias subredes de la red.
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: article
ms.assetid: 7110ad21-a33e-48d5-bb3c-129982913bc8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2be3c02f32229c9b9ee411f5e97305776f9825d8
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-dhcp-using-windows-powershell"></a>Implementar DHCP mediante Windows PowerShell

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Esta guía brinda instrucciones sobre cómo usar Windows PowerShell para implementar un servidor de protocolo de configuración dinámica de Host \(DHCP\) versión 4 de protocolo de Internet (IP) que asigna automáticamente direcciones IP y opciones de DHCP a los clientes de DHCP de IPv4 que están conectados a una o varias subredes de la red.

>[!NOTE]
>Para descargar este documento en formato de Word desde la Galería de TechNet, consulta [implementar DHCP usando Windows PowerShell en Windows Server 2016](https://gallery.technet.microsoft.com/Deploy-DHCP-Using-Windows-246dd293).

Uso de servidores DHCP para asignar IP direcciones que se guarda en la sobrecarga administrativa porque no tienes que configurar manualmente la configuración de TCP/IP v4 para todos los adaptadores de red en cada equipo de la red. Con DHCP, configuración de TCP/IP v4 se realiza automáticamente cuando un equipo u otro cliente DHCP está conectado a la red.

Puedes implementar el servidor DHCP en un grupo de trabajo como un servidor independiente, o como parte de un dominio de Active Directory.

Esta guía contiene las siguientes secciones.

- [Introducción a la implementación DHCP](#bkmk_overview)
- [Información general de la tecnología](#bkmk_technologies)
- [Planear la implementación de DHCP](#bkmk_plan)
- [Uso de esta guía en un laboratorio de pruebas](#bkmk_lab)
- [Implementar DHCP](#bkmk_deploy)
- [Comprueba la funcionalidad del servidor](#bkmk_verify)
- [Comandos de Windows PowerShell para DHCP](#bkmk_dhcpwps)
- [Lista de los comandos de PowerShell de Windows en esta guía](#bkmk_list)

## <a name="bkmk_overview"></a>Introducción a la implementación DHCP

La ilustración siguiente muestra el escenario que se puede implementar mediante el uso de esta guía. El escenario incluye un servidor DHCP en un dominio de Active Directory. El servidor está configurado para proporcionar direcciones IP a los clientes DHCP de dos subredes diferentes. Las subredes se separan con un enrutador que tenga habilitado el reenvío de DHCP.

![Introducción a topología de red DHCP](../../media/Core-Network-Guide/cng16_overview.jpg)

## <a name="bkmk_technologies"></a>Información general de la tecnología

Las secciones siguientes proporcionan breve introducción DHCP y TCP/IP.

### <a name="dhcp-overview"></a>Introducción a DHCP

DHCP es un estándar IP para simplificar la administración de configuración IP del host. El estándar DHCP permite el uso de servidores DHCP como una forma de administrar la asignación dinámica de direcciones IP y otros detalles de configuración para los clientes DHCP en la red.

DHCP te permite usar un servidor DHCP para asignar una dirección IP de dinámicamente en un equipo u otro dispositivo, como una impresora, en la red local, en lugar de configurar manualmente todos los dispositivos con una dirección IP estática.

Todos los equipos en una red TCP/IP deben tener una dirección IP única, porque la dirección IP y su máscara de subred relacionada identifican el equipo host y la subred a la que está conectado el equipo. Mediante el uso de DHCP, puedes asegurarte de que todos los equipos que están configurados como clientes DHCP reciban una dirección IP que sea apropiada para su ubicación de red y la subred y mediante opciones de DHCP, como la puerta de enlace predeterminada y los servidores DNS, puede proporcionar automáticamente a los clientes DHCP con la información que necesitan para funcionar correctamente en la red.

Redes TCP/IP, DHCP reduce la complejidad y la cantidad de trabajo administrativo necesarios para configurar los equipos.

### <a name="tcpip-overview"></a>Información general de TCP/IP

De manera predeterminada, todas las versiones de sistemas operativos Windows Server y el cliente de Windows tienen una configuración de TCP/IP IP versión 4 para conexiones de red configuradas para obtener automáticamente una dirección IP y otra información, denominado opciones de DHCP, desde un servidor DHCP. Por este motivo, no es necesario configurar manualmente las opciones de TCP/IP a menos que el equipo es un equipo de servidor u otro dispositivo que requiere una dirección IP estática configurada manualmente. 

Por ejemplo, se recomienda configurar manualmente la dirección IP del servidor DHCP y las direcciones IP de los servidores DNS y controladores de dominio que ejecutan \(AD DS\) los servicios de dominio de Active Directory.

TCP/IP en Windows Server 2016 es la siguiente:

-   En función de los protocolos de redes estándar del sector de software de red.

-   Protocolo de red de empresa pueden enrutar que admite la conexión de un equipo basado en Windows para la red de área local (LAN) y entornos de área extensa (WAN) de la red.

-   Tecnologías fundamentales y utilidades para conectar un equipo basado en Windows con diferentes sistemas con el fin de compartir información.

-   Una base para obtener acceso a los servicios de Internet globales, como los servidores Web y el protocolo de transferencia de archivos (FTP).

-   Un marco sólida, escalable y plataformas de cliente y servidor.

TCP/IP proporciona utilidades de TCP/IP básicas que permiten a los equipos basados en Windows conectar y compartir información con otros Microsoft y sistemas no son de Microsoft, incluidos:

- Windows Server 2016

- Windows 10

- Windows Server 2012 R2

- Windows 8.1

- Windows Server 2012

- Windows 8

- Windows Server 2008 R2

- Windows 7

- Windows Server 2008

- Windows Vista

- Hosts de Internet

- Sistemas de Apple Macintosh

- Grandes de sistemas de IBM

- Sistemas de UNIX y Linux

- Sistemas abiertos de máquinas virtuales

- Impresoras de red

- Tabletas y teléfonos móviles con Ethernet por cable o inalámbrica tecnología 802.11 habilitado

## <a name="bkmk_plan"></a>Planear la implementación de DHCP

Siguiente es los pasos de programación claves antes de instalar el rol de servidor DHCP.

### <a name="planning-dhcp-servers-and-dhcp-forwarding"></a>Planeación de servidores DHCP y reenvío de DHCP

Dado que los mensajes de difusión mensajes DHCP, no se reenvían entre subredes enrutadores. Si tienes varias subredes y desea proporcionar el servicio DHCP para cada subred, debe hacer lo siguiente:

-   Instalar a un servidor DHCP en cada subred

-   Configurar los enrutadores para reenviar los mensajes de difusión DHCP en subredes y configurar varios ámbitos en el servidor DHCP, un ámbito por cada subred.

En la mayoría de los casos, la configuración de enrutadores para reenviar mensajes de difusión DHCP es más rentable que la implementación de un servidor DHCP en cada segmento físico de la red.

### <a name="planning-ip-address-ranges"></a>Planeación de intervalos de direcciones IP

Cada subred debe tener su propio intervalo de direcciones IP único. Estos intervalos se representan en un servidor DHCP con ámbitos.

Un ámbito es una agrupación administrativa de direcciones IP para equipos de una subred que usan el servicio DHCP. El administrador crea primero un ámbito para cada subred física y, a continuación, usa el ámbito para definir los parámetros usados por los clientes.

Un ámbito tiene las siguientes propiedades:

-   Un intervalo de direcciones IP desde el que se incluya o excluya direcciones usadas para ofertas de concesión de servicio DHCP.

-   Máscara de subred, que determina el prefijo de subred para una dirección IP especificada.

-   Un nombre de ámbito que se asigna cuando se crea.

-   Valores de duración de concesión, que se asignan a los clientes DHCP que reciban asignado de manera dinámica direcciones IP.

-   Las opciones de ámbito DHCP configuradas para la asignación a los clientes DHCP, como la dirección IP del servidor DNS y dirección IP de puerta de enlace de enrutador de forma predeterminada.

-   Opcionalmente, se usan las reservas para garantizar que un cliente DHCP reciba siempre la misma dirección IP.

Antes de implementar los servidores de la lista de las subredes y el intervalo de direcciones IP que quieras usar para cada subred.

### <a name="planning-subnet-masks"></a>Planeación de máscaras

Identificadores de red y de host en una dirección IP se distinguen mediante una máscara de subred. Cada máscara de subred es un número de 32 bits que se usan los grupos de bits consecutivos de todos los (1) para identificar la red Id. y cero (0) para identificar las partes del identificador de host de una dirección IP.

Por ejemplo, la máscara de subred que normalmente se utiliza con la dirección IP 131.107.16.200 es el siguiente número binario de 32 bits:

```
11111111 11111111 00000000 00000000
```

Este número de máscara de subred es 16 bits uno seguidos de 16 bits cero, que indica que las secciones de Id. de identificador y host de red de esta dirección IP son 16 bits de longitud. Normalmente, esta máscara de subred se muestra en notación decimal como 255.255.0.0.

La siguiente tabla muestra las máscaras de subred para las clases de direcciones de Internet.

|Clase de dirección|Bits de máscara de subred|Máscara de subred|
|-----------------|------------------------|---------------|
|Clase A|11111111 00000000 00000000 00000000|255.0.0.0|
|Clase B|11111111 11111111 00000000 00000000|255.255.0.0|
|Clase C|11111111 11111111 11111111 00000000|255.255.255.0|

Cuando creas un ámbito en DHCP y especificar el intervalo de direcciones IP para el ámbito, DHCP proporciona estos valores de máscara de subred de forma predeterminada. Por lo general, valores de máscara de subred predeterminados son aceptables para la mayoría de las redes sin requisitos especiales y donde cada segmento de red IP corresponde a una sola red física.

En algunos casos, puedes usar máscaras de subred personalizadas para implementar subredes IP. Con las subredes IP, puede subdivide la parte del identificador de host predeterminado de una dirección IP para especificar subredes que son subdivisiones de identificador de red basada en la clase original.

Al personalizar la longitud de máscara de subred, puedes reducir el número de bits que se usan para el Id.

Para evitar problemas de direcciones y enrutamiento, debes asegurarte de que todos los equipos de TCP/IP en un segmento de red utilizan la misma máscara de subred y que cada equipo o dispositivo tiene una dirección IP única.

### <a name="planning-exclusion-ranges"></a>Planeación de intervalos de exclusión

Cuando creas un ámbito en un servidor DHCP, especifican un intervalo de direcciones IP que incluye todas las direcciones IP que el servidor DHCP puede conceder a los clientes DHCP, como equipos y otros dispositivos. Si, a continuación, ir y configurar manualmente algunos servidores y direcciones IP de otros dispositivos con estático desde el mismo intervalo de direcciones IP que está usando el servidor DHCP, puedes crear accidentalmente un conflicto de dirección IP, donde puedes y el servidor DHCP tienen les asigna la misma dirección IP a diferentes dispositivos.

Para resolver este problema, puedes crear un intervalo de exclusión para el ámbito DHCP. Un intervalo de exclusión es un contiguos intervalo de direcciones IP dentro del intervalo de direcciones IP del ámbito que el servidor DHCP no está permitido usar. Si creas un intervalo de exclusión, el servidor DHCP no asigna las direcciones en ese intervalo, que le permite asignar manualmente estas direcciones sin tener que crear un conflicto de dirección IP.

Puede excluir direcciones IP de distribución por el servidor DHCP mediante la creación de un intervalo de exclusión para cada ámbito. Debes usar las exclusiones para todos los dispositivos que están configurados con una dirección IP estática. Las direcciones excluidas deben incluir todas las direcciones IP que se asignen manualmente a otros servidores, los clientes no DHCP, estaciones de trabajo sin disco o clientes de enrutamiento y acceso remoto o de PPP.

Se recomienda que configures el intervalo de exclusión con las direcciones adicionales para acomodar el crecimiento futura de la red. La siguiente tabla proporciona un intervalo de exclusión de ejemplo para un ámbito con un intervalo de direcciones IP de 10.0.0.1 - 10.0.0.254 y una máscara de subred de 255.255.255.0.

|Elementos de configuración|Valores de ejemplo|
|-----------------------|------------------|
|Dirección IP de inicio del intervalo de exclusión|10.0.0.1|
|Dirección IP final del intervalo de exclusión|10.0.0.25|

### <a name="planning-tcpip-static-configuration"></a>Planeación de la configuración de TCP/IP estática

Algunos dispositivos, como los enrutadores, los servidores DHCP y servidores DNS, deben estar configurados con una dirección IP estática. Además, puede que dispositivos adicionales, como impresoras, lo que quieres asegurarte de que siempre tienen la misma dirección IP. Enumerar los dispositivos que desea configurar estáticamente para cada subred y, a continuación, planear el intervalo de exclusión que quieras usar en el servidor DHCP para asegurarse de que el servidor DHCP no conceder a la dirección IP de un dispositivo configurado estáticamente. Un intervalo de exclusión es una secuencia limitada de direcciones IP en un ámbito, excluidas de las ofertas de servicio DHCP. Los intervalos de exclusión debe asegurarse de que el servidor no ofrece todas las direcciones de estos intervalos a los clientes DHCP en la red.

Por ejemplo, si el intervalo de direcciones IP para una subred es 192.168.0.1a través de 192.168.0.254 y tienes diez dispositivos que desea configurar con una dirección IP estática, puedes crear un intervalo de exclusión para la 192.168.0. *x* ámbito que incluya diez o más direcciones IP: 192.168.0.1a través de 192.168.0.15.

En este ejemplo, usan diez de las direcciones IP excluidas para configurar servidores y otros dispositivos con direcciones IP estáticas y cinco direcciones IP adicionales se encuentran disponibles para una configuración estática de nuevos dispositivos que desea agregar en el futuro. Con este intervalo de exclusión, el servidor DHCP quedo con un conjunto de direcciones de 192.168.0.16a través de 192.168.0.254.

En la siguiente tabla, se proporcionan los elementos de configuración de ejemplo adicional de AD DS y DNS.

|Elementos de configuración|Valores de ejemplo|
|-----------------------|------------------|
|Enlaces de conexión de red|Ethernet|
|Configuración del servidor DNS|DC1.corp.contoso.com|
|Dirección IP del servidor DNS preferido|10.0.0.2|
|Valores de ámbito<br /><br />1. Nombre del ámbito de<br />2. A partir de la dirección IP<br />3. La dirección IP final<br />4. Máscara de subred de<br />5. (opcional) de la puerta de enlace predeterminada de<br />6. Duración de la concesión de|1. Principal subred<br />2.  10.0.0.1<br />3.  10.0.0.254<br />4.  255.255.255.0<br />5.  10.0.0.1<br />6. 8 días|
|Modo de operación de servidor DHCP IPv6|No está habilitado|

## <a name="bkmk_lab"></a>Uso de esta guía en un laboratorio de pruebas

Puedes usar a esta guía para implementar DHCP en un laboratorio de prueba antes de implementar en un entorno de producción. 

>[!NOTE]
>Si no quieres implementar DHCP en un laboratorio de pruebas, puedes omitir la sección [implementar DHCP](#bkmk_deploy).

Los requisitos para el laboratorio varían según si usas servidores físicos o máquinas virtuales \(VMs\), y si está usando un dominio de Active Directory o implementar un servidor independiente. 

Puedes usar la siguiente información para determinar los recursos mínimos que necesitas para probar la implementación de DHCP con esta guía.

### <a name="test-lab-requirements-with-vms"></a>Requisitos de laboratorio de prueba con máquinas virtuales

Para implementar DHCP en un laboratorio de prueba con máquinas virtuales, debes los siguientes recursos.

Para una implementación de dominio o en la implementación independiente, necesitará un servidor que esté configurado como un host Hyper\-V.

**Implementación de dominio**

Esta implementación requiere un servidor físico, un conmutador virtual, dos servidores virtuales y un cliente virtual:

En el servidor físico, en el Administrador de Hyper-V, crear los elementos siguientes.

1. Una **interna** conmutador virtual. No crees un **externo** virtual cambiar, ya que si el host Hyper\-V está en una subred que incluye un servidor DHCP, la prueba de máquinas virtuales recibirá una dirección IP del servidor DHCP. Además, el servidor DHCP de prueba que implementas podría asignar direcciones IP a otros equipos de la subred donde está instalado el host Hyper\-V.
1. Una VM que ejecute Windows Server 2106 configurado como un controlador de dominio con servicios de dominio de Active Directory que está conectada al conmutador virtual interno que creaste. Para que coincida con esta guía, este servidor debe tener una dirección IP estáticas de 10.0.0.2. Para obtener información sobre la implementación de AD DS, consulta la sección **implementar DC1** en el equipo con Windows Server 2016 [guía básica de red](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployADDNS01).
1. Una VM que se ejecute Windows Server 2106 que va a configurar como un servidor DHCP mediante el uso de esta guía y que está conectado a la interna virtual cambiar que has creado. 
1. Una VM que ejecute un sistema operativo cliente de Windows que está conectado a la interna virtual conmutador que has creado y que usarás para comprobar que el servidor DHCP dinámicamente asigna direcciones IP y opciones de DHCP a los clientes DHCP.

**Implementación del servidor DHCP Standalone**

Esta implementación requiere un servidor físico, un conmutador virtual, un servidor virtual y un cliente virtual:

En el servidor físico, en el Administrador de Hyper-V, crear los elementos siguientes.

1. Una **interna** conmutador virtual. No crees un **externo** virtual cambiar, ya que si el host Hyper\-V está en una subred que incluye un servidor DHCP, la prueba de máquinas virtuales recibirá una dirección IP del servidor DHCP. Además, el servidor DHCP de prueba que implementas podría asignar direcciones IP a otros equipos de la subred donde está instalado el host Hyper\-V.
1. Una VM que se ejecute Windows Server 2106 que va a configurar como un servidor DHCP mediante el uso de esta guía y que está conectado a la interna virtual cambiar que has creado.
1. Una VM que ejecute un sistema operativo cliente de Windows que está conectado a la interna virtual conmutador que has creado y que usarás para comprobar que el servidor DHCP dinámicamente asigna direcciones IP y opciones de DHCP a los clientes DHCP.

### <a name="test-lab-requirements-with-physical-servers"></a>Requisitos de laboratorio de prueba con servidores físicos

Para implementar DHCP en un laboratorio de prueba con servidores físicos, debes los siguientes recursos.

**Implementación de dominio**

Esta implementación requiere un concentrador o conmutador, dos servidores físicos y un cliente físico:

1. Un concentrador o conmutador Ethernet a la que se puedan conectar los equipos físicos con cables Ethernet
1. Un equipo físico que ejecuta Windows Server 2106 configurado como un controlador de dominio con los servicios de dominio de Active Directory. Para que coincida con esta guía, este servidor debe tener una dirección IP estáticas de 10.0.0.2. Para obtener información sobre la implementación de AD DS, consulta la sección **implementar DC1** en el equipo con Windows Server 2016 [guía básica de red](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployADDNS01).
1. Un equipo físico que ejecuta Windows Server 2106 que va a configurar como un servidor DHCP mediante el uso de esta guía. 
1. Un equipo físico con un sistema operativo cliente de Windows que se usará para comprobar que el servidor DHCP dinámicamente va a asignar direcciones IP y opciones de DHCP a los clientes DHCP.

>[!NOTE]
>Si no tienes suficiente equipos de prueba para esta implementación, puedes usar un equipo de prueba de AD DS y DHCP - sin embargo, esta configuración no se recomienda para un entorno de producción.

**Implementación del servidor DHCP Standalone**

Esta implementación requiere un concentrador o conmutador, un servidor físico y un cliente físico:

1. Un concentrador o conmutador Ethernet a la que se puedan conectar los equipos físicos con cables Ethernet
2. Un equipo físico que ejecuta Windows Server 2106 que va a configurar como un servidor DHCP mediante el uso de esta guía. 
3. Un equipo físico con un sistema operativo cliente de Windows que se usará para comprobar que el servidor DHCP dinámicamente va a asignar direcciones IP y opciones de DHCP a los clientes DHCP.


## <a name="bkmk_deploy"></a>Implementar DHCP

Esta sección proporciona ejemplos de comandos de Windows PowerShell que puedes usar para implementar DHCP en un servidor. Antes de ejecutar estos comandos de ejemplo en el servidor, debes modificar los comandos para que coincida con la red y el entorno. 

Por ejemplo, antes de ejecutar los comandos, deberás reemplazar los valores de ejemplo en los comandos de los siguientes elementos:

- Nombres de equipo
- Intervalo de direcciones IP para cada ámbito que deseas configurar (1 ámbito por cada subred)
- Máscara de subred de cada intervalo de direcciones IP que deseas configurar
- Nombre del ámbito de cada ámbito
- Intervalo de exclusión para cada ámbito
- Valores de opción de DHCP, como la puerta de enlace predeterminada, el nombre de dominio y servidores DNS o WINS
- Nombres de interfaz

>[!IMPORTANT]
>Examinar y modificar todos los comandos para el entorno antes de ejecutar el comando.

### <a name="where-to-install-dhcp---on-a-physical-computer-or-a-vm"></a>¿Dónde instalar DHCP - en un equipo físico o en una máquina virtual?

Puedes instalar el rol de servidor DHCP en un equipo físico o en una máquina virtual \(VM\) que está instalada en un host Hyper\-V. Si vas a instalar DHCP en una máquina virtual y quieres que el servidor DHCP para proporcionar asignaciones de direcciones IP a los equipos en la red física al que está conectado el host de Hyper-V, debe conectarse a un conmutador Virtual de Hyper-V que es el adaptador de red virtual de máquina virtual **externo**.

Para obtener más información, consulta la sección **cree un conmutador Virtual con el Administrador de Hyper-V** en el tema [crear una red virtual](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/connect-to-network).

### <a name="run-windows-powershell-as-an-administrator"></a>Ejecutar Windows PowerShell como administrador

Puedes usar el siguiente procedimiento para ejecutar Windows PowerShell con privilegios de administrador.

1. En un equipo con Windows Server 2016, haz clic en **inicio**, a continuación, haz clic en el icono de Windows PowerShell. Aparece un menú. 

2. En el menú, haz clic en **más**y, a continuación, haz clic en **ejecutar como administrador**. Si se te pide, escribe las credenciales para una cuenta con privilegios de administrador en el equipo. Si la cuenta de usuario con la que ha iniciado sesión en el equipo es una cuenta de nivel de administrador, no recibirá una petición de credenciales.

3. Abre Windows PowerShell con privilegios de administrador.

### <a name="rename-the-dhcp-server-and-configure-a-static-ip-address"></a>Cambiar el nombre del servidor DHCP y configurar una dirección IP estática

Si aún no lo has hecho, puedes usar los siguientes comandos de Windows PowerShell para cambiar el nombre del servidor DHCP y configurar una dirección IP estática del servidor.

**Configurar una dirección IP estática**

Puedes usar los siguientes comandos para asignar una dirección IP estática en el servidor DHCP y para configurar las propiedades de TCP/IP del servidor DHCP con la dirección IP del servidor DNS correcta. También debes reemplazar los nombres de interfaz y direcciones IP en este ejemplo con los valores que quieres usar para configurar el equipo.

 `New-NetIPAddress -IPAddress 10.0.0.3 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24`

 `Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 10.0.0.2`

Para obtener más información sobre estos comandos, consulta los siguientes temas.

- [New-NetIPAddress](https://technet.microsoft.com/itpro/powershell/windows/tcpip/new-netipaddress)
- [Set-DnsClientServerAddress](https://technet.microsoft.com/itpro/powershell/windows/dns-client/set-dnsclientserveraddress)

**Cambiar el nombre del equipo**

Puedes usar los siguientes comandos para cambiar el nombre y, a continuación, reinicia el equipo.

`Rename-Computer -Name DHCP1`

 `Restart-Computer`

Para obtener más información sobre estos comandos, consulta los siguientes temas.

- [Rename-Computer](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/rename-computer)
- [Restart-Computer](https://msdn.microsoft.com/powershell/reference/4.0/microsoft.powershell.management/restart-computer)

### <a name="join-the-computer-to-the-domain-optional"></a>Unir el equipo a la \(Optional\) dominio

Si vas a instalar al servidor DHCP en un entorno de dominio de Active Directory, debes unir el equipo al dominio. Abre Windows PowerShell con privilegios de administrador y, a continuación, ejecuta el siguiente comando después de cambiar el nombre NetBios del dominio **CORP** con un valor que sea apropiado para su entorno.

    Add-Computer CORP

Cuando se te pide, escribe las credenciales para una cuenta de usuario de dominio que tenga permisos para unir un equipo al dominio. 

    Restart-Computer

Para obtener más información acerca del comando Add-Computer, consulta el tema siguiente.

- [Add-Computer](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/add-computer)

### <a name="install-dhcp"></a>Instalar DHCP

Una vez reiniciado el equipo, abre Windows PowerShell con privilegios de administrador y, a continuación, instalar DHCP ejecutando el siguiente comando.

    Install-WindowsFeature DHCP -IncludeManagementTools

Para obtener más información acerca de este comando, consulta el tema siguiente.

- [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/server-manager/install-windowsfeature)

### <a name="create-dhcp-security-groups"></a>Crear grupos de seguridad DHCP

Para crear grupos de seguridad, debes ejecutar un comando de \(netsh\) de Shell de red en Windows PowerShell y, a continuación, reinicia el servicio DHCP para que los nuevos grupos se volverán activos.

Cuando se ejecuta el siguiente comando netsh en el servidor DHCP, el **Administradores DHCP** y **usuarios DHCP** grupos de seguridad se crean en **usuarios y grupos locales** en el servidor DHCP.

    netsh dhcp add securitygroups

El siguiente comando reinicia el servicio DHCP en el equipo local.

    Restart-service dhcpserver

Para obtener más información sobre estos comandos, consulta los siguientes temas.

- [Shell de red (Netsh)](../netsh/netsh.md)
- [Restart-Service](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/restart-service)

### <a name="authorize-the-dhcp-server-in-active-directory-optional"></a>Autorizar el servidor DHCP en Active Directory \(Optional\)

Si vas a instalar DHCP en un entorno de dominio, debes realizar los siguientes pasos para autorizar el servidor DHCP para que funcione en el dominio.

>[!NOTE]
>Los servidores DHCP no autorizados que están instalados en los dominios de Active Directory no pueden funcionar correctamente y no arrendar direcciones IP a los clientes DHCP. La desactivación automática de servidores DHCP no autorizados es una característica de seguridad que impide que los servidores DHCP no autorizados asignar direcciones IP incorrectas a los clientes de la red.

Puedes usar el siguiente comando para agregar el servidor DHCP a la lista de servidores DHCP autorizados en Active Directory. 

>[!NOTE]
>Si no tiene un entorno de dominio, no ejecutes este comando.

 `Add-DhcpServerInDC -DnsName DHCP1.corp.contoso.com -IPAddress 10.0.0.3`

Para comprobar que el servidor DHCP está autorizado en Active Directory, puedes usar el siguiente comando.

    Get-DhcpServerInDC

A continuación se muestran los resultados de ejemplo que se muestran en Windows PowerShell.

    
        IPAddress   DnsName
        ---------   -------
        10.0.0.3    DHCP1.corp.contoso.com
    

Para obtener más información sobre estos comandos, consulta los siguientes temas.

- [Add-DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverindc)
- [Get-DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/get-dhcpserverindc)

### <a name="notify-server-manager-that-post-install-dhcp-configuration-is-complete-optional"></a>Notificar a ese DHCP post\ instalar el administrador del servidor configuración \(Optional\) completo

Después de completar las tareas de instalación post\, como crear grupos de seguridad y autorizar el servidor DHCP en Active Directory, el administrador del servidor puede seguir mostrando una alerta en la interfaz de usuario que indica que se deben completar los pasos de instalación de post\ mediante el Asistente de configuración de instalación de publicación de DHCP.

Este mensaje now\ innecesarios e incorrectos puede impedir que aparecen en el administrador del servidor mediante la configuración de la siguiente clave del registro con el comando de Windows PowerShell.

    Set-ItemProperty –Path registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ServerManager\Roles\12 –Name ConfigurationState –Value 2

Para obtener más información acerca de este comando, consulta el tema siguiente.

- [Set-ItemProperty](https://msdn.microsoft.com/powershell/reference/4.0/microsoft.powershell.management/set-itemproperty?f=255&MSPPError=-2147217396)

### <a name="set-server-level-dns-dynamic-update-configuration-settings-optional"></a>Configurar el servidor nivel DNS actualización dinámica configuración configuración \(Optional\)

Si quieres que el servidor DHCP para realizar las actualizaciones dinámicas para los equipos cliente DHCP, puede ejecutar el comando siguiente para configurar esta configuración. Se trata de un nivel de servidor, configuración, no un ámbito nivel estableciendo, por lo que afectará a todos los ámbitos que configuran en el servidor. Este comando de ejemplo también configura el servidor DHCP para eliminar registros de recursos DNS de los clientes cuando el cliente menos expira.

    Set-DhcpServerv4DnsSetting -ComputerName "DHCP1.corp.contoso.com" -DynamicUpdates "Always" -DeleteDnsRRonLeaseExpiry $True

Puedes usar el siguiente comando para configurar las credenciales que usa el servidor DHCP para registrar y anular el registro de los registros de cliente en un servidor DNS. Este ejemplo, guarda una credencial en un servidor DHCP. Usa el primer comando **Get-Credential** para crear un **PSCredential** objeto y, a continuación, almacena el objeto en la **$Credential** variable. El comando solicita el nombre de usuario y contraseña, por lo tanto, asegúrese de que proporcionar credenciales para una cuenta que tiene permiso para actualizar los registros de recursos en el servidor DNS.

    
    $Credential = Get-Credential
    Set-DhcpServerDnsCredential -Credential $Credential -ComputerName "DHCP1.corp.contoso.com"
    

Para obtener más información sobre estos comandos, consulta los siguientes temas.

- [Conjunto DhcpServerv4DnsSetting](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverv4dnssetting)
- [Set-DhcpServerDnsCredential](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverdnscredential)

### <a name="configure-the-corpnet-scope"></a>Configurar el ámbito de la red corporativa

Una vez completada la instalación de DHCP, puedes usar los siguientes comandos para configurar y activar el ámbito de la red corporativa, crear un intervalo de exclusión para el ámbito y configurar la puerta de enlace predeterminada DHCP opciones, la dirección IP del servidor DNS y el nombre de dominio DNS.

    
    Add-DhcpServerv4Scope -name "Corpnet" -StartRange 10.0.0.1 -EndRange 10.0.0.254 -SubnetMask 255.255.255.0 -State Active`
    
    Add-DhcpServerv4ExclusionRange -ScopeID 10.0.0.0 -StartRange 10.0.0.1 -EndRange 10.0.0.15`
    
    Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.0.1 -ScopeID 10.0.0.0 -ComputerName DHCP1.corp.contoso.com`
    
    Set-DhcpServerv4OptionValue -DnsDomain corp.contoso.com -DnsServer 10.0.0.2
    

Para obtener más información sobre estos comandos, consulta los siguientes temas.

- [DhcpServerv4Scope agregar](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverv4scope)
- [DhcpServerv4ExclusionRange agregar](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverv4exclusionrange)
- [Conjunto DhcpServerv4OptionValue](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverv4optionvalue)

### <a name="configure-the-corpnet2-scope-optional"></a>Configurar la Corpnet2 ámbito \(Optional\)

Si tienes una segunda subred que está conectada a la primera subred con un enrutador donde el reenvío de DHCP está habilitado, puedes usar los siguientes comandos para agregar un segundo ámbito, denominado Corpnet2 para este ejemplo. Este ejemplo también configura un intervalo de exclusión y la dirección IP para la puerta de enlace predeterminada \ (dirección IP del enrutador en la subnet\) de la subred Corpnet2.

 `Add-DhcpServerv4Scope -name "Corpnet2" -StartRange 10.0.1.1 -EndRange 10.0.1.254 -SubnetMask 255.255.255.0 -State Active`

 `Add-DhcpServerv4ExclusionRange -ScopeID 10.0.1.0 -StartRange 10.0.1.1 -EndRange 10.0.1.15`

 `Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.1.1 -ScopeID 10.0.1.0 -ComputerName DHCP1.corp.contoso.com`

Si tienes subredes adicionales que son mantenidas por este servidor DHCP, puedes repetir estos comandos, usan los diferentes valores para todos los parámetros de comando, agregar ámbitos para cada subred.

>[!IMPORTANT]
>Asegúrate de que todos los enrutadores entre los clientes DHCP y el servidor DHCP están configurados para el reenvío de mensajes DHCP. Consulta la documentación del enrutador para obtener información sobre cómo configurar el reenvío de DHCP.

## <a name="bkmk_verify"></a>Comprueba la funcionalidad del servidor

Para comprobar que el servidor DHCP proporciona una asignación dinámica de direcciones IP a los clientes DHCP, puede conectar a una subred revisada otro equipo. Después de conectar el cable Ethernet para el adaptador de red y energía en el equipo, solicitará una dirección IP del servidor DHCP. Puede comprobar la configuración correcta mediante la **ipconfig /all** comando y revisar los resultados o mediante la realización de pruebas de conectividad, como intentar obtener acceso a recursos Web con los recursos compartidos de explorador o archivo con el Explorador de Windows u otras aplicaciones.

Si el cliente recibe una dirección IP del servidor DHCP, realiza los siguientes pasos de solución de problemas.

1. Asegúrate de que el cable Ethernet esté enchufado tanto el equipo y el Ethernet conmutador, concentrador o enrutador.
1. Si conecta el equipo cliente en un segmento de red es independiente del servidor DHCP por un enrutador, asegúrate de que el enrutador está configurado para reenviar mensajes DHCP.
1. Asegúrate de que el servidor DHCP está autorizado en Active Directory ejecutando el siguiente comando para recuperar la lista de servidores DHCP autorizados de Active Directory. [Get-DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/get-dhcpserverindc).
1. Asegúrate de que se activan los ámbitos, abre la consola DHCP \ (Administrador del servidor, **herramientas**, **DHCP**\), expande el árbol de servidor para revisar los ámbitos, a continuación, haz clic en el right\ cada ámbito. Si el menú resultante incluye la selección **activar**, haz clic en **activar**. \ (Si ya está activado el ámbito, lee la selección del menú **Deactivate**. \) 

## <a name="bkmk_dhcpwps"></a>Comandos de Windows PowerShell para DHCP

La siguiente referencia proporciona descripciones de comando y la sintaxis para todos los comandos de PowerShell de Windows de servidor DHCP para Windows Server 2016. El tema enumeran los comandos en orden alfabético según el verbo al principio de los comandos, como **obtener** o **establecer**.

>[!NOTE]
>No puedes usar comandos de Windows Server 2016 en Windows Server 2012 R2.

- [Módulo DhcpServer](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/index)

La siguiente referencia proporciona descripciones de comando y la sintaxis para todos los comandos de PowerShell de Windows de servidor DHCP para Windows Server 2012 R2. El tema enumeran los comandos en orden alfabético según el verbo al principio de los comandos, como **obtener** o **establecer**.

>[!NOTE]
>Puedes usar comandos de Windows Server 2012 R2 en Windows Server 2016.

- [Cmdlets de servidor DHCP en Windows PowerShell](https://technet.microsoft.com/library/jj590751.aspx)

## <a name="bkmk_list"></a>Lista de los comandos de PowerShell de Windows en esta guía

Siguiente es una lista sencilla de comandos y los valores de ejemplo que se usan en esta guía.

    
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
    


