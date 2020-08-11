---
title: Introducción a DNS de difusión por proximidad
description: En este tema se proporciona una breve introducción a DNS de difusión por proximidad
manager: laurawi
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: f9c313ac-bb86-4e48-b9b9-de5004393e06
ms.author: greglin
author: greg-lindsay
ms.openlocfilehash: 2f91ace398cf236967fadde21db7ea0957640995
ms.sourcegitcommit: c4f30b1617571fe434c7fe054695d163e73506b8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/10/2020
ms.locfileid: "88048915"
---
# <a name="anycast-dns-overview"></a>Introducción a DNS de difusión por proximidad

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2019

En este tema se proporciona información sobre cómo funciona el DNS de difusión por proximidad.

## <a name="what-is-anycast"></a>¿Qué es Anycast?

Anycast es una tecnología que proporciona varias rutas de acceso de enrutamiento a un grupo de puntos de conexión a los que se asigna la misma dirección IP. Cada dispositivo del grupo anuncia la misma dirección en una red y los protocolos de enrutamiento se usan para elegir cuál es el mejor destino.

Difusión por proximidad le permite escalar un servicio sin estado, como DNS o HTTP, colocando varios nodos detrás de la misma dirección IP y usando el enrutamiento multiruta de acceso de igual costo (ECMP) para dirigir el tráfico entre estos nodos. La difusión por proximidad es diferente de la unidifusión, en la que cada punto de conexión tiene su propia dirección IP independiente. 

## <a name="why-use-anycast-with-dns"></a>¿Por qué usar Anycast con DNS?

Con DNS de difusión por proximidad, puede habilitar un servidor DNS, o un grupo de servidores, para responder a las consultas DNS en función de la ubicación geográfica de un cliente DNS. Esto puede mejorar el tiempo de respuesta de DNS y simplificar la configuración del cliente DNS. El DNS de difusión por proximidad también proporciona una capa adicional de redundancia y puede ayudar a protegerse frente a ataques de denegación de servicio DNS. 

### <a name="how-anycast-dns-works"></a>Cómo funciona el DNS de difusión por proximidad

El DNS de difusión por proximidad funciona mediante protocolos de enrutamiento como Protocolo de puerta de enlace de borde (BGP) para enviar consultas de DNS a un servidor DNS o un grupo de servidores DNS preferidos (por ejemplo: un grupo de servidores DNS administrados por un equilibrador de carga). Esto puede optimizar las comunicaciones DNS mediante la obtención de respuestas DNS de un servidor DNS que sea más cercano a un cliente.

Con Anycast, los servidores que se encuentran en varias ubicaciones geográficas anuncian una única dirección IP idéntica a su puerta de enlace local (enrutador). Cuando un cliente DNS inicia una consulta a la dirección de difusión por proximidad, se evalúan las rutas disponibles y la consulta DNS se envía a la ubicación preferida. En general, se trata de la ubicación más cercana en función de la topología de red. Consulte el ejemplo siguiente.

![DNS de difusión por proximidad](../../media/Anycast/anycast.png)

**Figura 1**: cuatro servidores DNS ubicados en diferentes sitios de una red anuncian la misma dirección IP de difusión por proximidad (flechas negras) a la red. Un dispositivo cliente DNS envía una solicitud a la dirección IP de difusión por proximidad. Los dispositivos de red analizan las rutas disponibles y envían la consulta DNS del cliente a la ubicación más cercana (flecha azul). 

El DNS de difusión por proximidad se usa normalmente hoy para enrutar el tráfico DNS de muchos servicios DNS globales. Por ejemplo, el sistema del servidor DNS raíz depende en gran medida del DNS de difusión por proximidad. Anycast también funciona con una variedad de protocolos de enrutamiento y se puede usar exclusivamente en intranets.

## <a name="windows-server-native-bgp-anycast-demo"></a>Demostración de la difusión nativa de BGP en Windows Server

El siguiente procedimiento muestra cómo se puede usar BGP nativo en Windows Server con DNS de difusión por proximidad.  

### <a name="requirements"></a>Requisitos

- Un dispositivo físico con el rol de Hyper-V instalado.
  - Windows Server 2012 R2, Windows 10 o posterior.
- 2 máquinas virtuales de cliente (cualquier sistema operativo).
  - Se recomienda la instalación de herramientas de enlace para DNS como Dig.
- 3 máquinas virtuales de servidor (Windows Server 2016 o Windows Server 2019).
  - Si el módulo LoopbackAdapter de Windows PowerShell aún no está instalado en las máquinas virtuales de servidor (DC001, DC002), el acceso a Internet es necesario temporalmente para instalar este módulo.

### <a name="hyper-v-setup"></a>Configuración de Hyper-V

Configure el servidor de Hyper-V de la siguiente manera:

- 2 redes de conmutador virtual privado configuradas
  - Una red de Internet ficticia 131.253.1.0/24
  - Una red de intranet ficticia red 10.10.10.0/24
- 2 las máquinas virtuales de cliente están conectadas a la red 131.253.1.0/24
- 2 las máquinas virtuales de servidor están conectadas a la red Red 10.10.10.0/24
- 1 el servidor es de host dual y está conectado a las redes 131.253.1.0/24 y red 10.10.10.0/24.

### <a name="virtual-machine-network-configuration"></a>Configuración de red de máquina virtual

Configure las opciones de red en las máquinas virtuales con la siguiente configuración:

1.  Client1, cliente2
  - Client1:131.253.1.1
  - Cliente2:131.253.1.2
  - Máscara de subred: 255.255.255.0
  - DNS: 51.51.51.51
  - Puerta de enlace: 131.253.1.254
2.  Puerta de enlace (Windows Server)
  - NIC1:131.253.1.254, subred 255.255.255.0
  - NIC2:10.10.10.254, subred 255.255.255.0
  - DNS: 51.51.51.51
  - Puerta de enlace: 131.253.1.100 (se puede omitir para la demostración)
3.  DC001 (Windows Server)
  - NIC1:10.10.10.1
  - Subred: 255.255.255.0
  - DNS: 10.10.10.1
4.  DC002 (Windows Server)
  - NIC1:10.10.10.2
  - Subred 255.255.255.0
  - DNS: 10.10.10.2

### <a name="configure-dns"></a>Configurar el DNS

Use Administrador del servidor y la consola de administración de DNS o Windows PowerShell para instalar los siguientes roles de servidor y crear una zona DNS estática en cada uno de los dos servidores.

1.  DC001, DC002
  - Instalar Active Directory Domain Services y promover a controlador de dominio (opcional)
  - Instalar el rol DNS (obligatorio)
  - Cree una zona estática (no integrada en AD) denominada **Zone. TST** en DC001 y DC002
    - Agregue el único **servidor** de nombres de registro estático en la zona de tipo "txt"
    - Datos (texto) para el registro TXT en DC001 = **DC001**
    - Datos (texto) para el registro TXT en DC002 = **DC002**

### <a name="configure-loopback-adapters"></a>Configuración de adaptadores de bucle invertido

Escriba los siguientes comandos en un símbolo del sistema de Windows PowerShell con privilegios elevados en DC001 y DC002 para configurar los adaptadores de bucle invertido. 

> [!NOTE]
> El comando **install-Module** requiere acceso a Internet. Esto puede realizarse mediante la asignación temporal de la máquina virtual a una red externa en Hyper-V.

```PowerShell
$primary_interface = (Get-NetAdapter |?{$_.Status -eq "Up" -and !$_.Virtual}).Name
$loopback_ipv4 = '51.51.51.51'
$loopback_ipv4_length = '32'
$loopback_name = 'Loopback'
Install-Module -Name LoopbackAdapter -MinimumVersion 1.2.0.0 -Force
Import-Module -Name LoopbackAdapter
New-LoopbackAdapter -Name $loopback_name -Force
$interface_loopback = Get-NetAdapter -Name $loopback_name
$interface_main = Get-NetAdapter -Name $primary_interface
Set-NetIPInterface -InterfaceIndex $interface_loopback.ifIndex -InterfaceMetric "254" -WeakHostReceive Enabled -WeakHostSend Enabled -DHCP Disabled
Set-NetIPInterface -InterfaceIndex $interface_main.ifIndex -WeakHostReceive Enabled -WeakHostSend Enabled
Set-NetIPAddress -InterfaceIndex $interface_loopback.ifIndex -SkipAsSource $True
Get-NetAdapter $loopback_name | Set-DNSClient –RegisterThisConnectionsAddress $False
New-NetIPAddress -InterfaceAlias $loopback_name -IPAddress $loopback_ipv4 -PrefixLength $loopback_ipv4_length -AddressFamily ipv4
Disable-NetAdapterBinding -Name $loopback_name -ComponentID ms_msclient
Disable-NetAdapterBinding -Name $loopback_name -ComponentID ms_pacer
Disable-NetAdapterBinding -Name $loopback_name -ComponentID ms_server
Disable-NetAdapterBinding -Name $loopback_name -ComponentID ms_lltdio
Disable-NetAdapterBinding -Name $loopback_name -ComponentID ms_rspndr
```

### <a name="virtual-machine-routing-configuration"></a>Configuración de enrutamiento de máquina virtual

Use los siguientes comandos de Windows PowerShell en máquinas virtuales para configurar el enrutamiento.

1.  Puerta de enlace
```PowerShell
Install-RemoteAccess -VpnType RoutingOnly
Add-BgpRouter -BgpIdentifier “10.10.10.254” -LocalASN 8075
```

2.  DC001
```PowerShell
Install-RemoteAccess -VpnType RoutingOnly
Add-BgpRouter -BgpIdentifier “10.10.10.1” -LocalASN 65511
Add-BgpPeer -Name "Labgw" -LocalIPAddress 10.10.10.1 -PeerIPAddress 10.10.10.254 -PeerASN 8075 –LocalASN 65511
Add-BgpCustomRoute -Network 51.51.51.0/24
```

3.  DC002
```PowerShell
Install-RemoteAccess -VpnType RoutingOnly
Add-BgpRouter -BgpIdentifier "10.10.10.2" -LocalASN 65511
Add-BgpPeer -Name "Labgw" -LocalIPAddress 10.10.10.2 -PeerIPAddress 10.10.10.254 -PeerASN 8075 –LocalASN 65511
Add-BgpCustomRoute -Network 51.51.51.0/24
```

### <a name="summary-diagram"></a>Diagrama de Resumen

![DNS de difusión por proximidad](../../media/Anycast/anycast-lab.png)

**Figura 2**: demostración de la configuración de laboratorio para DNS de multidifusión nativa de BGP

## <a name="anycast-dns-demonstration"></a>Demostración de DNS de difusión por proximidad


1.  Comprobar el enrutamiento de BGP en el servidor de puerta de enlace

    PS C: \> Get-BgpRouteInformation

    DestinationNetwork NextHop LearnedFromPeer State LocalPref MED<br>
    ------------------ -------    --------------- ----- --------- ---<br>
    51.51.51.0/24 10.10.10.1 DC001 Best<br>
    51.51.51.0/24 10.10.10.2 DC002 mejor<br>

2.  En client1 y cliente2, compruebe que puede llegar a 51.51.51.51

    PS C: \> ping 51.51.51.51

    Haciendo ping a 51.51.51.51 con 32 bytes de datos:<br>
    Respuesta de 51.51.51.51: bytes = 32 tiempo<1 ms TTL = 126<br>
    Respuesta de 51.51.51.51: bytes = 32 tiempo<1 ms TTL = 126<br>
    Respuesta de 51.51.51.51: bytes = 32 tiempo<1 ms TTL = 126<br>
    Respuesta de 51.51.51.51: bytes = 32 tiempo<1 ms TTL = 126

    Estadísticas de ping para 51.51.51.51:<br>
    Paquetes: enviados = 4, recibidos = 4, perdidos = 0 (0% de pérdida),<br>
    Tiempos de ida y vuelta aproximados en milisegundos:<br>
    Mínimo = 0 MS, máximo = 0 MS, promedio = 0 ms

3.  En client1 y cliente2, use nslookup o Dig para consultar el registro TXT. A continuación se muestran ejemplos de ambos.

    PS C: \> dig Server. Zone. TST txt + Short<br>
    PS C: \> nslookup-type = txt Server. Zone. TST 51.51.51.51

    Un cliente mostrará "DC001" y el otro cliente mostrará "DC002" comprobando que Anycast funciona correctamente.  También puede consultar desde el servidor de puerta de enlace.

4.  A continuación, deshabilite el adaptador Ethernet en DC001.

    PS C: \> (get-NetAdapter). Name<br>
    Bucle invertido<br>
    Ethernet 2<br>
    PS C: \> Disable-NetAdapter "Ethernet 2"<br>
    Confirm<br>
    ¿Está seguro de que desea realizar esta acción?<br>
    Disable-NetAdapter ' Ethernet 2 '<br>
    [Y] sí [A] sí a todos [N] no [L] no a todas [S] suspender [?] Ayuda (el valor predeterminado es "Y"):<br>
    PS C: \> (get-NetAdapter). Estatus<br>
    Arriba<br>
    Deshabilitada

5.  Confirme que los clientes DNS que estaban recibiendo respuestas de DC001 han cambiado a DC002.

    PS C: \> nslookup-type = txt Server. Zone. TST 51.51.51.51<br>
    Servidor: desconocido<br>
    Dirección: 51.51.51.51<br>

    texto Server. Zone. TST =

    "DC001"<br>
    PS C: \> nslookup-type = txt Server. Zone. TST 51.51.51.51<br>
    Servidor: desconocido<br>
    Dirección: 51.51.51.51<br>

    texto Server. Zone. TST =

    "DC002"

6.  Confirme que la sesión de BGP está inactiva en DC001 mediante Get-BgpStatistics en el servidor de puerta de enlace.
7.  Vuelva a habilitar el adaptador Ethernet en DC001 y confirme que se ha restaurado la sesión BGP y que los clientes reciben las respuestas DNS de DC001 de nuevo.

> [!NOTE]
> Si no se usa un equilibrador de carga, un cliente individual usará el mismo servidor DNS de back-end si está disponible. Esto crea una ruta de acceso BGP coherente para el cliente. Para obtener más información, consulte la sección 4.4.3 de RFC4786: rutas de acceso de [igual costo](https://tools.ietf.org/html/rfc4786#page-10).

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

P: ¿DNS de difusión por proximidad es una buena solución para usar en un entorno DNS local?<br>
R: DNS de difusión por proximidad funciona sin problemas con un servicio DNS local. Sin embargo, la difusión por proximidad no es *necesaria* para que el servicio DNS se escale.
 
P: ¿Cuál es el impacto de la implementación de DNS de difusión por proximidad en un entorno con un número grande (por ejemplo, >50) de controladores de dominio? <br>
R: no hay ningún impacto directo en la funcionalidad. Si se usa un equilibrador de carga, no se requiere ninguna configuración adicional en los controladores de dominio.
 
P: ¿es una configuración de DNS de difusión por proximidad compatible con el servicio de atención al cliente de Microsoft?<br>
R: Si usa un equilibrador de carga que no es de Microsoft para reenviar consultas DNS, Microsoft admitirá problemas relacionados con el servicio servidor DNS. Consulte al proveedor del equilibrador de carga para ver los problemas relacionados con el reenvío de DNS. 
 
P: ¿Cuál es el procedimiento recomendado para el DNS de difusión por proximidad con un número grande (por ejemplo, >50) de controladores de dominio?<br>
R: el procedimiento recomendado es usar un equilibrador de carga en cada ubicación geográfica. Normalmente, un proveedor externo proporciona los equilibradores de carga. 

P: ¿los DNS de difusión por proximidad y Azure DNS tienen una funcionalidad similar?<br>
R: Azure DNS usa la difusión por proximidad. Para usar Anycast con Azure DNS, configure el equilibrador de carga para reenviar las solicitudes al servidor de Azure DNS. 