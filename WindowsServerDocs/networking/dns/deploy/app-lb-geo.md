---
title: Uso de la directiva de DNS para equilibrio de carga de aplicación con reconocimiento de ubicación geográfica
description: En este tema forma parte de las DNS directiva escenario guía para Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: b6e679c6-4398-496c-88bc-115099f3a819
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9f76163e6b064ac3225ab4d755afd548e1cb720b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446413"
---
# <a name="use-dns-policy-for-application-load-balancing-with-geo-location-awareness"></a>Uso de la directiva de DNS para equilibrio de carga de aplicación con reconocimiento de ubicación geográfica

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para aprender a configurar la directiva DNS para una aplicación de equilibrio de carga con reconocimiento de ubicación geográfica.

El tema anterior en esta guía, [uso de directiva DNS para el equilibrio de carga de aplicaciones](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/app-lb), usa un ejemplo de una compañía ficticia - servicios de regalo de Contoso - que proporciona regulan los regalos online services y que tiene un sitio Web denominado contosogiftservices.com. Equilibra la carga de los servicios de regalos de Contoso de su aplicación Web en línea entre los servidores en centros de datos de América del Norte ubicado en Seattle, WA, Chicago, IL y Dallas, TX.

>[!NOTE]
>Se recomienda familiarizarse con el tema [uso de directiva DNS para el equilibrio de carga de aplicaciones](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/app-lb) antes de realizar las instrucciones de este escenario.

En este tema se usa la misma empresa ficticia y la infraestructura de red como base para una nueva implementación de ejemplo que incluye el reconocimiento de ubicación geográfica.

En este ejemplo, servicios de regalos de Contoso está expandiendo correctamente su presencia en todo el mundo.

Al igual que América del Norte, ahora la compañía tiene servidores web hospedados en centros de datos Europeo.

Los administradores de Contoso regalo servicios DNS que desea configurar aplicación equilibrio de carga para los centros de datos europeos de forma similar a la implementación de directiva DNS en los Estados Unidos, con tráfico de aplicación que se distribuyen entre los servidores Web que se encuentran en Dublín, Irlanda, Amsterdam, Holanda y en otro lugar.

Los administradores de DNS también quieren todas las consultas de otras ubicaciones del mundo que se distribuye equitativamente entre todos sus centros de datos.

En las secciones siguientes puede aprender a lograr objetivos similares a las de los administradores de DNS de Contoso en su propia red.

## <a name="how-to-configure-application-load-balancing-with-geo-location-awareness"></a>Cómo configurar la aplicación equilibrio de carga con reconocimiento de ubicación geográfica

Las secciones siguientes muestran cómo configurar la directiva de DNS para la aplicación equilibrio de carga con reconocimiento de ubicación geográfica.

>[!IMPORTANT]
>Las secciones siguientes incluyen ejemplos de comandos de Windows PowerShell que contienen valores de ejemplo para muchos parámetros. Asegúrese de sustituir los valores de ejemplo de estos comandos con los valores adecuados para su implementación antes de ejecutar estos comandos.

### <a name="bkmk_clientsubnets"></a>Crear las subredes del cliente DNS

En primer lugar debe identificar las subredes o espacio de direcciones IP de las regiones de Norteamérica y Europa.

Puede obtener esta información de mapas de Geo-IP. En función de estas distribuciones de Geo-IP, debe crear las subredes del cliente DNS.

Una subred de cliente DNS es una agrupación lógica de subredes IPv4 o IPv6 desde el que las consultas se envían a un servidor DNS.

Puede usar los siguientes comandos de Windows PowerShell para crear subredes de cliente DNS. 

    
    Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet 192.0.0.0/24,182.0.0.0/24
    Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet 141.1.0.0/24,151.1.0.0/24
    
Para obtener más información, consulte [agregar DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).

### <a name="bkmk_zscopes2"></a>Creación de los ámbitos de zona

Después de las subredes del cliente están en su lugar, debe dividir el contosogiftservices.com de zona en los ámbitos de zona diferente, cada uno de un centro de datos.

Un ámbito de la zona es una instancia única de la zona. Una zona DNS puede tener varios ámbitos de zona, con cada ámbito de la zona que contiene su propio conjunto de registros DNS. El mismo registro puede estar presente en varios ámbitos, con direcciones IP diferentes o las mismas direcciones IP.

>[!NOTE]
>De forma predeterminada, un ámbito de la zona existe en las zonas DNS. Este ámbito de la zona tiene el mismo nombre que la zona y las operaciones de DNS heredadas funcionan en este ámbito.

El escenario anterior sobre el equilibrio de carga de aplicación muestra cómo configurar tres ámbitos de los centros de datos de zona en América del Norte.

Con los siguientes comandos, puede crear dos ámbitos zona más, uno de los centros de datos Dublín y Ámsterdam. 

Puede agregar estos ámbitos de zona sin realizar ningún cambio a los tres ámbitos de zona de América del Norte existentes en la misma zona. Además, después de crear estos ámbitos de zona, no es necesario reiniciar el servidor DNS.

Puede usar los siguientes comandos de Windows PowerShell para crear ámbitos de la zona.

    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AmsterdamZoneScope"
    

Para obtener más información, consulte [agregar DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

### <a name="bkmk_records2"></a>Agregar registros a los ámbitos de zona

Ahora debe agregar los registros que representa el host del servidor web en los ámbitos de la zona.

En el escenario anterior, se agregaron los registros de los centros de datos de América. Puede usar los siguientes comandos de Windows PowerShell para agregar registros a los ámbitos de zona para centros de datos Europeo.
 
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "151.1.0.1" -ZoneScope "DublinZoneScope”
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "AmsterdamZoneScope"
    

Para obtener más información, consulte [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).

### <a name="bkmk_policies2"></a>Cree las directivas DNS

Una vez que ha creado las particiones (ámbitos de zona) y se han agregado registros, debe crear las directivas DNS que distribución las consultas entrantes entre estos ámbitos.

En este ejemplo, la distribución de consultas a través de servidores de aplicaciones en distintos centros de datos cumple los criterios siguientes.

1. Cuando se recibe la consulta DNS desde un origen en una subred de cliente de América del Norte, 50% de las respuestas DNS apunte al centro de datos de Seattle, 25% de las respuestas hacia el centro de datos de Chicago y el 25% restante de las respuestas hacia el centro de datos de Dallas.
2. Cuando se recibe la consulta DNS desde un origen en una subred de cliente Europeo, 50% de las respuestas DNS hacia el centro de datos Dublín y 50% de las respuestas DNS hacia el centro de datos Ámsterdam.
3. Cuando la consulta procede de cualquier otro lugar del mundo, las respuestas DNS se distribuyen entre todos los centros de datos de cinco.

Puede usar los siguientes comandos de Windows PowerShell para implementar estas directivas DNS.

    
    Add-DnsServerQueryResolutionPolicy -Name "AmericaLBPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1; TexasZoneScope,1" -ZoneName "contosogiftservices.com" –ProcessingOrder 1
    
    Add-DnsServerQueryResolutionPolicy -Name "EuropeLBPolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 2
    
    Add-DnsServerQueryResolutionPolicy -Name "WorldWidePolicy" -Action ALLOW -FQDN "eq,*.contoso.com" -ZoneScope "SeattleZoneScope,1;ChicagoZoneScope,1; TexasZoneScope,1;DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3
    
    

Para obtener más información, consulte [agregar DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).

Ya ha creado correctamente una directiva DNS que proporciona aplicación equilibrio de carga entre los servidores Web que se encuentran en cinco distintos centros de datos en varios continentes.

Puede crear miles de las directivas DNS según el tráfico de los requisitos de administración y todas las nuevas directivas se aplican dinámicamente - sin necesidad de reiniciar el servidor DNS, en las consultas entrantes.
