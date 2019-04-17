---
title: Usar la directiva DNS para la ubicación geográfica en función de la administración de tráfico con las implementaciones de principal secundario
description: En este tema es parte de la DNS directiva escenario guía para Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: a9ee7a56-f062-474f-a61c-9387ff260929
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4c78a0198e29fb59f30fd8ad776c7f200312d014
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-geo-location-based-traffic-management-with-primary-secondary-deployments"></a>Usar la directiva DNS para la ubicación geográfica en función de la administración de tráfico con las implementaciones de principal secundario

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para aprender a crear la directiva DNS para la administración de tráfico en función de ubicación geográfica cuando la implementación de DNS incluye servidores DNS principal y secundarios.  

El escenario anterior, [usar la directiva de DNS para la administración de tráfico en función de ubicación geográfica con los servidores principal](primary-geo-location.md), proporciona instrucciones para configurar la directiva DNS para la administración de tráfico en función de ubicación geográfica de un servidor DNS principal. En la infraestructura de Internet, sin embargo, los servidores DNS se implementan ampliamente en un modelo de principal secundario, donde se almacena la copia de una zona de escritura en los servidores principales seleccionados y seguros, y copias de solo lectura de la zona se mantienen en varios servidores secundarios.   
  
Los servidores secundarios usan los protocolos de transferencia de zona transferencia autorizado (AXFR) y transferencia de zona Incremental (IXFR) para pedir y recibir actualizaciones de la zona que incluyen nuevos cambios a las zonas de los servidores DNS principales.   
  
>[!NOTE]
>Para obtener más información sobre AXFR, consulta el grupo de trabajo de ingeniería de Internet (IETF) [solicitudes de comentarios 5936](https://tools.ietf.org/rfc/rfc5936.txt). Para obtener más información acerca de IXFR, consulta el grupo de trabajo de ingeniería de Internet (IETF) [solicitudes de comentarios 1995](https://tools.ietf.org/html/rfc1995).  
  
## <a name="bkmk_example"></a>Ubicación geográfica principal secundario en función de ejemplo de administración de tráfico  
Siguiente es un ejemplo de cómo puedes usar la directiva de DNS en una implementación principal secundario para lograr el redireccionamiento del tráfico basándose en la ubicación física del cliente que realiza una consulta DNS.  
  
Este ejemplo usa dos empresas ficticias - Contoso servicios en la nube, que proporciona la web y dominio que aloja soluciones; ejemplo de Woodgrove alimentos servicios, que proporciona servicios de entrega de alimentos de numerosas ciudades en todo el mundo y que tiene un sitio Web denominado woodgrove.com.  
  
Para garantizar que los clientes woodgrove.com obtendrán una experiencia con capacidad de respuesta de su sitio Web, Woodgrove quiere que los clientes europeos dirigidos al centro de datos Europea y americano dirige al centro de datos de Estados Unidos. Los clientes situados en otro lugar en el mundo pueden dirigirse a cualquiera de los centros de datos.  
  
Servicios en la nube Contoso tiene dos centros de datos, uno de los Estados Unidos y otro en Europa, en el que Contoso hospeda su orden portal para woodgrove.com de alimentos.  
  
La implementación de DNS de Contoso incluye dos servidores secundarios: **SecondaryServer1**, con la dirección IP 10.0.0.2; y **SecondaryServer2**, con la dirección IP 10.0.0.3. Estos servidores secundarios actúan como servidores de nombres de los dos diferentes regiones, con SecondaryServer1 ubicada en Europa y SecondaryServer2 que se encuentra en Estados Unidos
  
Hay una copia de la zona de escritura principal en **PrimaryServer** (dirección IP 10.0.0.1), donde se realizan los cambios de zona. Con transferencias normal a los servidores secundarios, los servidores secundarios siempre están actualizados con los nuevos cambios en la zona en la PrimaryServer.
  
La ilustración siguiente muestra este escenario.
  
![Ubicación geográfica principal secundario en función de ejemplo de administración de tráfico](../../media/Dns-Policy_PS1/dns_policy_primarysecondary1.jpg)  
   
## <a name="bkmk_works"></a>Cómo funciona el sistema DNS principal y secundario

Cuando implementas administración del tráfico en función de ubicación geográfica en una implementación de DNS principal secundaria, es importante comprender cómo normal zona de principal secundario las transferencias se producen antes de obtener información sobre las transferencias de nivel de ámbito de zona. Las secciones siguientes proporcionan información en la zona y transferencias de nivel de ámbito.  
  
- [Transferencias en una implementación de DNS principal y secundario](#bkmk_zone)  
- [Ámbito nivel transferencias en una implementación de DNS principal y secundario](#bkmk_scope)  
  
### <a name="bkmk_zone"></a>Transferencias en una implementación de DNS principal y secundario

Puedes crear una implementación de DNS principal secundario y sincronizar las zonas con los siguientes pasos.  
1. Cuando instalas DNS, la zona principal se crea en el servidor DNS principal.  
2. En el servidor secundario, crea las zonas y especifica los servidores principales.   
3. En los servidores principales, puedes agregar los servidores secundarios como secundarios confianza en la zona principal.   
4. Las zonas secundarias realizar una solicitud de transferencia de zona completa (AXFR) y reciben la copia de la zona.   
5. Cuando sea necesario, los servidores principales envían notificaciones a los servidores secundarios acerca de las actualizaciones de la zona.  
6. Servidores secundarios realizar una solicitud de transferencia de zona incremental (IXFR). Por este motivo, los servidores secundarios permanecen sincronizados con el servidor principal.   
  
### <a name="bkmk_scope"></a>Ámbito nivel transferencias en una implementación de DNS principal y secundario

El escenario de administración de tráfico requiere pasos adicionales para particionar las zonas en ámbitos zona diferente. Por este motivo, se necesitan para transferir los datos dentro de los ámbitos de zona a los servidores secundarios y para transferir las directivas y las subredes del cliente DNS a los servidores secundarios pasos adicionales.   
  
Después de configurar la infraestructura DNS con los servidores principales y secundarios, transferencias de nivel de ámbito se realizan automáticamente mediante DNS, con los siguientes procesos.  
  
Para garantizar a la transferencia de nivel de ámbito de zona, servidores DNS usan los mecanismos de extensión para DNS (EDNS0) participar RR. Todas las solicitudes de transferencia (AXFR o IXFR) de la zona de las zonas con ámbitos proceden con una participación de EDNS0 RR, cuyo identificador de la opción se establece en "65433" de manera predeterminada. Para obtener más información sobre EDNSO, consulta el IETF [solicitudes de comentarios 6891](https://tools.ietf.org/html/rfc6891).  
  
El valor del RR OPC es el nombre del ámbito zona para que se envía la solicitud. Cuando un servidor DNS principal recibe este paquete desde un servidor de confianza secundario, interpreta la solicitud como procedentes de dicho ámbito de la zona.   
  
Si el servidor principal tiene ese ámbito zona responde con los datos de transferencia (XFR) desde ese ámbito. La respuesta contiene un RR participación con el mismo identificador de la opción "65433" y el valor establecido en el mismo ámbito de la zona. Los servidores secundarios recibirán esta respuesta, recuperan la información de ámbito de la respuesta y actualización ese ámbito en concreto de la zona.  
  
Después de este proceso, el servidor principal mantiene una lista de confianza secundarios que has enviado tales una solicitud de ámbito de zona para las notificaciones.   
  
Para cualquier otra actualización en un ámbito de la zona, se envía una notificación de IXFR a los servidores secundarios, con el mismo RR participar. El ámbito de la zona recibir notificación de que realiza la solicitud IXFR que contenga ese RR participar y sigue el mismo proceso como se describió anteriormente.  
  
## <a name="bkmk_config"></a>Cómo configurar la directiva de DNS para la administración de tráfico en función de ubicación geográfica de principal secundario

Antes de comenzar, asegúrate de que has completado todos los pasos en el tema [usar la directiva de DNS para la administración de tráfico en función de ubicación geográfica con los servidores principal](../../dns/deploy/Scenario--Use-DNS-Policy-for-Geo-Location-Based-Traffic-Management-with-Primary-Servers.md), y el servidor DNS principal está configurado con las zonas, ámbitos zona, subredes del cliente DNS y directiva DNS.  
  
>[!NOTE]
> Las instrucciones de este tema para copiar subredes del cliente DNS, los ámbitos de la zona y directivas DNS de los servidores DNS principales a los servidores DNS secundarios son de la configuración inicial de DNS y la validación. En el futuro es posible que quieras cambiar la configuración de directivas en el servidor principal, los ámbitos de la zona y los subredes del cliente DNS. En este caso, puedes crear scripts de automatización para mantener los servidores secundarios sincronizados con el servidor principal.  
  
Para configurar la directiva DNS para las respuestas de consulta principal secundario geolocalización en función, debes realizar los siguientes pasos.  
  
- [Crear las zonas secundario](#bkmk_secondary)  
- [Configurar las opciones de transferencia de la zona en la zona principal](#bkmk_zonexfer)  
- [Copia las subredes del cliente DNS](#bkmk_client)  
- [Crear los ámbitos de la zona en el servidor secundario](#bkmk_zonescopes)  
- [Configurar la directiva de DNS](#bkmk_dnspolicy)  
  
Las siguientes secciones proporcionan instrucciones de configuración detallada.  
  
>[!IMPORTANT]
>Las secciones siguientes incluyen ejemplos de comandos de Windows PowerShell que contienen los valores de ejemplo para el número de parámetros. Asegúrate de que los valores de ejemplo en estos comandos, se reemplaza con valores que son adecuados para su implementación antes de ejecutar estos comandos.  
><br>Pertenencia a **grupo Administradores DNS**, o equivalente, es necesario para realizar los siguientes procedimientos.  
  
### <a name="bkmk_secondary"></a>Crear las zonas secundario

Puedes crear la copia secundaria de la zona que desea replicar SecondaryServer1 y SecondaryServer2 (suponiendo que los cmdlets se ejecutan remotamente desde un cliente solo administración).   
  
Por ejemplo, puedes crear la copia secundaria de www.woodgrove.com en SecondaryServer1 y SecondarySesrver2.  
  
Puedes usar los siguientes comandos de Windows PowerShell para crear las zonas secundarias.  
  
    
    Add-DnsServerSecondaryZone -Name "woodgrove.com" -ZoneFile "woodgrove.com.dns" -MasterServers 10.0.0.1 -ComputerName SecondaryServer1  
      
    Add-DnsServerSecondaryZone -Name "woodgrove.com" -ZoneFile "woodgrove.com.dns" -MasterServers 10.0.0.1 -ComputerName SecondaryServer2  
      

Para obtener más información, consulta [agregar DnsServerSecondaryZone](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserversecondaryzone?view=win10-ps).  
  
### <a name="bkmk_zonexfer"></a>Configurar las opciones de transferencia de la zona en la zona principal

Debes configurar la configuración de zona principal para que:

1. Se permiten transferencias desde el servidor principal a los servidores secundarios especificados.  
2. Notificaciones de actualización de zona enviadas por el servidor principal a los servidores secundarios.  
  
Puedes usar los siguientes comandos de Windows PowerShell para configurar las opciones de transferencia de la zona en la zona principal.
  
>[!NOTE]
>En el siguiente comando de ejemplo, el parámetro **-notificar** especifica que el servidor principal enviará notificaciones sobre las actualizaciones a la lista de selección de secundarios.  
  
    
    Set-DnsServerPrimaryZone -Name "woodgrove.com" -Notify Notify -SecondaryServers "10.0.0.2,10.0.0.3" -SecureSecondaries TransferToSecureServers -ComputerName PrimaryServer  
     
  
Para obtener más información, consulta [conjunto DnsServerPrimaryZone](https://https://docs.microsoft.com/powershell/module/dnsserver/set-dnsserverprimaryzone?view=win10-ps).  
  
  
### <a name="bkmk_client"></a>Copia las subredes del cliente DNS

Las subredes del cliente DNS tienes que copiar desde el servidor principal en los servidores secundarios.
  
Puedes usar los siguientes comandos de Windows PowerShell para copiar las subredes en los servidores secundarios.
  
    
    Get-DnsServerClientSubnet -ComputerName PrimaryServer | Add-DnsServerClientSubnet -ComputerName SecondaryServer1  
      
    Get-DnsServerClientSubnet -ComputerName PrimaryServer | Add-DnsServerClientSubnet -ComputerName SecondaryServer2  
      

Para obtener más información, consulta [agregar DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).  
  
### <a name="bkmk_zonescopes"></a>Crear los ámbitos de la zona en el servidor secundario

Debes crear los ámbitos de la zona en los servidores secundarios. En DNS, los ámbitos zona también iniciar la solicitud de XFR desde el servidor principal. Con los cambios realizados en los ámbitos de la zona en el servidor principal, se envía una notificación que contiene la información de ámbito de la zona en los servidores secundarios. Los servidores secundarios, a continuación, actualizar sus ámbitos zona con los cambios incrementales.  
  
Puedes usar los siguientes comandos de Windows PowerShell para crear los ámbitos de la zona en los servidores secundarios.  
  
    
    Get-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName PrimaryServer|Add-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName SecondaryServer1 -ErrorAction Ignore  
      
    Get-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName PrimaryServer|Add-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName SecondaryServer2 -ErrorAction Ignore  
  

>[!NOTE]
>En estos comandos de ejemplo, el **- ErrorAction Ignorar** parámetro se incluye, ya existe un ámbito de la zona de forma predeterminada en todas las zonas. El ámbito de la zona predeterminado no se puede crear ni eliminar. La canalización, se generará un intento de crear ese ámbito y se producirá un error. Como alternativa, puedes crear los ámbitos zona no predeterminado en dos zonas secundarias.  
  
Para obtener más información, consulta [agregar DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).  
  
### <a name="bkmk_dnspolicy"></a>Configurar la directiva de DNS

Después de haber creado las subredes, las particiones (ámbitos zona) y se han agregado registros, debes crear directivas que conectan las subredes y las particiones, para que cuando se trata de una consulta de un origen en una de las subredes del cliente DNS, se devuelve la respuesta de consulta desde el ámbito correcto de la zona. No hay directivas son necesarias para asignar el ámbito de la zona de forma predeterminada.  
  
Puedes usar los siguientes comandos de Windows PowerShell para crear una directiva DNS que se vincula las subredes del cliente DNS y los ámbitos de la zona.   
    
    $policy = Get-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName PrimaryServer  
      
    $policy | Add-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName SecondaryServer1  
      
    $policy | Add-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName SecondaryServer2  
      

Para obtener más información, consulta [agregar DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  
  
Ahora, los servidores DNS secundarios están configurados con las directivas necesarias de DNS para redirigir el tráfico en función de la ubicación geográfica.  
  
Cuando el servidor DNS recibe las consultas de resolución de nombre, el servidor DNS evalúa los campos de la solicitud DNS en las directivas DNS configuradas. Si la dirección IP de origen en la solicitud de resolución de nombre coincide con ninguna de las directivas, se usa el ámbito de la zona asociada para responder a la consulta y el usuario se dirige al recurso que geográficamente más cercano a ellos.   
  
Puede crear miles de directivas DNS según el tráfico de requisitos de administración y todas las nuevas directivas se aplican de forma dinámica - sin reiniciar el servidor DNS, en las consultas entrantes.
