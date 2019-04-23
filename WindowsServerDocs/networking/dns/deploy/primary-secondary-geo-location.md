---
title: Uso de directiva DNS para la administración del tráfico basada en la ubicación geográfica con implementaciones primarias-secundarias
description: En este tema forma parte de las DNS directiva escenario guía para Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: a9ee7a56-f062-474f-a61c-9387ff260929
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b11064e6b3bd2590d5712afdb7afc69de1ed83f4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889706"
---
# <a name="use-dns-policy-for-geo-location-based-traffic-management-with-primary-secondary-deployments"></a>Uso de directiva DNS para la administración del tráfico basada en la ubicación geográfica con implementaciones primarias-secundarias

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para aprender a crear la directiva DNS para la administración del tráfico según la ubicación geográfica cuando la implementación de DNS incluye servidores DNS principal y secundaria.  

El escenario anterior, [uso de directiva DNS para la administración de tráfico en función de la ubicación geográfica con servidores principales](primary-geo-location.md), proporciona instrucciones para configurar la directiva DNS para la administración del tráfico según la ubicación geográfica en un servidor DNS principal. En la infraestructura de Internet, sin embargo, los servidores DNS están ampliamente implementados en un modelo de principal secundario, donde la copia grabable de una zona se almacena en servidores principales de selectos y seguros, y copias de solo lectura de la zona se mantienen en varios servidores secundarios.   
  
Los servidores secundarios usan los protocolos de transferencia de zona transferencia autoritativo (AXFR) y transferencia de zona Incremental (IXFR) para solicitar y recibir las actualizaciones de zona que incluyen los nuevos cambios en las zonas de los servidores DNS principales.   
  
>[!NOTE]
>Para obtener más información acerca de AXFR, vea Internet Engineering Task Force (IETF) [de solicitud de comentarios 5936](https://tools.ietf.org/rfc/rfc5936.txt). Para obtener más información acerca de IXFR, consulte Internet Engineering Task Force (IETF) [de solicitud de comentarios de 1995](https://tools.ietf.org/html/rfc1995).  
  
## <a name="bkmk_example"></a>Ubicación geográfica secundaria principal en función de ejemplo de administración de tráfico  
La siguiente es un ejemplo de cómo se puede usar la directiva DNS en una implementación de principal secundario para lograr el redireccionamiento del tráfico basándose en la ubicación física del cliente que realiza una consulta DNS.  
  
En este ejemplo utiliza dos empresas ficticios: servicios de nube de Contoso, que proporciona soluciones de hospedaje de dominios y web y servicios de alimentación de Woodgrove, que proporciona servicios de entrega de alimentos en varias ciudades en todo el mundo, y que tiene un sitio Web denominado woodgrove.com.  
  
Para asegurarse de que los clientes de woodgrove.com obtengan una capacidad de respuesta desde su sitio Web, Woodgrove desea europeos clientes dirigidos al centro de datos Europeo y American dirige al centro de datos de Estados Unidos. Los clientes ubicados en otro lugar del mundo se pueden dirigir a cualquiera de los centros de datos.  
  
Servicios en la nube de Contoso tiene dos centros de datos, uno en Estados Unidos y otros en Europa, en el que Contoso hospeda su comida ordenación portal para woodgrove.com.  
  
La implementación de DNS de Contoso incluye dos servidores secundarios: **SecondaryServer1**, con la dirección IP 10.0.0.2; y **SecondaryServer2**, con la dirección IP 10.0.0.3. Estos servidores secundarios actúan como servidores de nombres de las dos regiones diferentes con SecondaryServer1 ubicado en Europa y SecondaryServer2 ubicados en Estados Unidos.
  
Hay una copia de la zona principal de escritura en **servidorprincipal** (dirección IP 10.0.0.1), donde se realizan los cambios de zona. Con las transferencias de zona normal a los servidores secundarios, los servidores secundarios son siempre al día con los nuevos cambios a la zona en el servidor principal.
  
La siguiente ilustración muestra este escenario.
  
![Ubicación geográfica secundaria principal en función de ejemplo de administración de tráfico](../../media/Dns-Policy_PS1/dns_policy_primarysecondary1.jpg)  
   
## <a name="bkmk_works"></a>Cómo funciona el sistema DNS principal secundario

Al implementar la administración del tráfico según la ubicación geográfica en una implementación de DNS de principal secundario, es importante comprender cómo normal zona principal secundario las transferencias se producen antes de obtener más información sobre las transferencias de nivel de ámbito de zona. Las secciones siguientes proporcionan información de zona y las transferencias de nivel de ámbito de zona.  
  
- [Transferencias de zona en una implementación de DNS principal secundario](#bkmk_zone)  
- [Transfiere el nivel de ámbito de la zona en una implementación de DNS principal secundario](#bkmk_scope)  
  
### <a name="bkmk_zone"></a>Transferencias de zona en una implementación de DNS principal secundario

Puede crear una implementación de DNS principal secundario y sincronizar las zonas con los pasos siguientes.  
1. Al instalar DNS, se crea la zona principal en el servidor DNS principal.  
2. En el servidor secundario, cree las zonas y especifique los servidores principales.   
3. En los servidores principales, puede agregar los servidores secundarios como elementos secundarios de confianza en la zona principal.   
4. Las zonas secundarias realizar una solicitud de transferencia de zona completa (AXFR) y recibirán la copia de la zona.   
5. Cuando sea necesario, los servidores principales envían notificaciones a los servidores secundarios sobre las actualizaciones de zona.  
6. Los servidores secundarios realice una solicitud de transferencia de zona incremental (IXFR). Por este motivo, los servidores secundarios sigan estando sincronizados con el servidor principal.   
  
### <a name="bkmk_scope"></a>Transfiere el nivel de ámbito de la zona en una implementación de DNS principal secundario

El escenario de administración de tráfico requiere pasos adicionales para dividir las zonas en ámbitos diferentes de zona. Por este motivo, se requieren para transferir los datos dentro de los ámbitos de zona a los servidores secundarios y para transferir las directivas y las subredes de cliente DNS a los servidores secundarios pasos adicionales.   
  
Después de configurar la infraestructura de DNS con servidores principales y secundarios, las transferencias de nivel de ámbito de zona se realizan automáticamente mediante DNS, con los siguientes procesos.  
  
Para garantizar a la transferencia de nivel de ámbito de zona, los servidores DNS usan los mecanismos de extensión para DNS (EDNS0) OPT RR. Todas las solicitudes de transferencia (AXFR o IXFR) zona de las zonas con ámbitos que se originan con una participación de EDNS0 RR, cuyo identificador de opción se establece en "65433" de forma predeterminada. Para obtener más información sobre EDNSO, vea IETF [de solicitud de comentarios 6891](https://tools.ietf.org/html/rfc6891).  
  
El valor de la participación RR es el nombre de ámbito de zona para el que se está enviando la solicitud. Cuando un servidor DNS principal recibe este paquete desde un servidor secundario de confianza, interpreta la solicitud como procedente de ese ámbito de la zona.   
  
Si el servidor principal tiene ese ámbito de la zona responde con los datos de transferencia (XFR) de ese ámbito. La respuesta contiene un RR participación con el mismo identificador de la opción "65433" y el valor establecido en el mismo ámbito de la zona. Los servidores secundarios recibirán esta respuesta, recuperan la información de ámbito de la respuesta y actualización de ese ámbito determinado de la zona.  
  
Después de este proceso, el servidor principal mantiene una lista de elementos secundarios de confianza que se ha enviado este tipo una solicitud de ámbito de zona para las notificaciones.   
  
Cualquier actualización adicional en un ámbito de la zona, se envía una notificación de IXFR a los servidores secundarios, con el mismo RR participar. El ámbito de la zona recibir dicha notificación realiza la solicitud IXFR que contiene ese RR participar y sigue el mismo proceso tal como se describió anteriormente.  
  
## <a name="bkmk_config"></a>Cómo configurar la directiva de DNS para la administración del tráfico en función de principal secundario ubicación geográfica

Antes de comenzar, asegúrese de que ha completado todos los pasos en el tema [uso de directiva DNS para la administración de tráfico en función de la ubicación geográfica con servidores principales](../../dns/deploy/Scenario--Use-DNS-Policy-for-Geo-Location-Based-Traffic-Management-with-Primary-Servers.md), y el servidor DNS principal se configura con zonas, ámbitos de zona, el cliente DNS Las subredes y directiva de DNS.  
  
>[!NOTE]
> Las instrucciones de este tema para copiar las subredes de cliente DNS, los ámbitos de zona y las directivas DNS de los servidores DNS principales en los servidores DNS secundarios son para la instalación inicial de DNS y la validación. En el futuro que desea cambiar las subredes del cliente DNS, los ámbitos de zona y configuración de directivas en el servidor principal. En este caso, puede crear scripts de automatización para mantener los servidores secundarios sincronizados con el servidor principal.  
  
Para configurar la directiva DNS para las respuestas de consulta basada primarias-secundarias la ubicación geográfica, debe realizar los pasos siguientes.  
  
- [Crear las zonas de la base de datos secundaria](#bkmk_secondary)  
- [Configurar las opciones de transferencia de zona en la zona principal](#bkmk_zonexfer)  
- [Copie las subredes del cliente DNS](#bkmk_client)  
- [Crear ámbitos de la zona en el servidor secundario](#bkmk_zonescopes)  
- [Configurar la directiva de DNS](#bkmk_dnspolicy)  
  
Las secciones siguientes proporcionan instrucciones de configuración detallada.  
  
>[!IMPORTANT]
>Las secciones siguientes incluyen ejemplos de comandos de Windows PowerShell que contienen valores de ejemplo para muchos parámetros. Asegúrese de sustituir los valores de ejemplo de estos comandos con los valores adecuados para su implementación antes de ejecutar estos comandos.  
><br>Pertenencia a **DnsAdmins**, o equivalente, es necesario para realizar los procedimientos siguientes.  
  
### <a name="bkmk_secondary"></a>Crear las zonas de la base de datos secundaria

Puede crear la copia secundaria de la zona que desea replicar en SecondaryServer1 y SecondaryServer2 (suponiendo que los cmdlets se ejecutan remotamente desde un cliente de administración único).   
  
Por ejemplo, puede crear la copia secundaria de www.woodgrove.com en SecondaryServer1 y SecondarySesrver2.  
  
Puede usar los siguientes comandos de Windows PowerShell para crear las zonas secundarias.  
  
    
    Add-DnsServerSecondaryZone -Name "woodgrove.com" -ZoneFile "woodgrove.com.dns" -MasterServers 10.0.0.1 -ComputerName SecondaryServer1  
      
    Add-DnsServerSecondaryZone -Name "woodgrove.com" -ZoneFile "woodgrove.com.dns" -MasterServers 10.0.0.1 -ComputerName SecondaryServer2  
      

Para obtener más información, consulte [agregar DnsServerSecondaryZone](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserversecondaryzone?view=win10-ps).  
  
### <a name="bkmk_zonexfer"></a>Configurar las opciones de transferencia de zona en la zona principal

Debe configurar la configuración de zona principal para que:

1. Se permiten las transferencias de zona desde el servidor principal a los servidores secundarios especificados.  
2. Notificaciones de actualización de zona se envían por el servidor principal a los servidores secundarios.  
  
Puede usar los siguientes comandos de Windows PowerShell para configurar las opciones de transferencia de zona en la zona principal.
  
>[!NOTE]
>En el siguiente comando de ejemplo, el parámetro **-notificar** especifica que el servidor principal enviará notificaciones sobre actualizaciones a la lista de selección de bases de datos secundarias.  
  
    
    Set-DnsServerPrimaryZone -Name "woodgrove.com" -Notify Notify -SecondaryServers "10.0.0.2,10.0.0.3" -SecureSecondaries TransferToSecureServers -ComputerName PrimaryServer  
     
  
Para obtener más información, consulte [Set-DnsServerPrimaryZone](https://docs.microsoft.com/powershell/module/dnsserver/set-dnsserverprimaryzone?view=win10-ps).  
  
  
### <a name="bkmk_client"></a>Copie las subredes del cliente DNS

Debe copiar las subredes del cliente DNS desde el servidor principal en los servidores secundarios.
  
Puede usar los siguientes comandos de Windows PowerShell para copiar las subredes a los servidores secundarios.
  
    
    Get-DnsServerClientSubnet -ComputerName PrimaryServer | Add-DnsServerClientSubnet -ComputerName SecondaryServer1  
      
    Get-DnsServerClientSubnet -ComputerName PrimaryServer | Add-DnsServerClientSubnet -ComputerName SecondaryServer2  
      

Para obtener más información, consulte [agregar DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).  
  
### <a name="bkmk_zonescopes"></a>Crear ámbitos de la zona en el servidor secundario

Debe crear los ámbitos de zona en los servidores secundarios. En DNS, los ámbitos de zona también iniciar la solicitud de XFR desde el servidor principal. Con cualquier cambio en los ámbitos de zona en el servidor principal, se envía una notificación que contiene la información de ámbito de la zona a los servidores secundarios. Los servidores secundarios, a continuación, pueden actualizar sus ámbitos de zona con cambios incrementales.  
  
Puede usar los siguientes comandos de Windows PowerShell para crear los ámbitos de zona en los servidores secundarios.  
  
    
    Get-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName PrimaryServer|Add-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName SecondaryServer1 -ErrorAction Ignore  
      
    Get-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName PrimaryServer|Add-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName SecondaryServer2 -ErrorAction Ignore  
  

>[!NOTE]
>En estos comandos de ejemplo, el **- ErrorAction omitir** parámetro se incluye, porque existe un ámbito de la zona de forma predeterminada en todas las zonas. El ámbito de la zona predeterminada no se pueden crear ni eliminar. La canalización dará como resultado un intento de crear ese ámbito y se producirá un error. Como alternativa, puede crear los ámbitos de la zona no predeterminada en dos zonas secundarias.  
  
Para obtener más información, consulte [agregar DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).  
  
### <a name="bkmk_dnspolicy"></a>Configurar la directiva de DNS

Después de haber creado las subredes, las particiones (ámbitos de zona) y se han agregado registros, debe crear directivas que se conectan a las subredes y las particiones, por lo que cuando una consulta procede de un origen en una de las subredes de cliente DNS, se devuelve la respuesta de consulta desde el ámbito correcto de la zona. No hay directivas son necesarios para la asignación de ámbito de la zona predeterminada.  
  
Puede usar los siguientes comandos de Windows PowerShell para crear una directiva DNS que vincula las subredes del cliente DNS y los ámbitos de la zona.   
    
    $policy = Get-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName PrimaryServer  
      
    $policy | Add-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName SecondaryServer1  
      
    $policy | Add-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName SecondaryServer2  
      

Para obtener más información, consulte [agregar DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  
  
Ahora, los servidores DNS secundarios se configuran con las directivas DNS necesarias para redirigir el tráfico según la ubicación geográfica.  
  
Cuando el servidor DNS recibe consultas de resolución de nombres, el servidor DNS se evalúa como los campos de la solicitud DNS con las directivas DNS configuradas. Si la dirección IP de origen en la solicitud de resolución de nombre coincide con cualquiera de las directivas, el ámbito de la zona asociada se utiliza para responder a la consulta y se dirige al usuario al recurso que está geográficamente más cercano a ellos.   
  
Puede crear miles de las directivas DNS según el tráfico de los requisitos de administración y todas las nuevas directivas se aplican dinámicamente - sin necesidad de reiniciar el servidor DNS, en las consultas entrantes.
