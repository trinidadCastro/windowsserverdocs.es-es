---
title: Uso de la directiva de DNS para equilibrio de carga de aplicación con reconocimiento de ubicación geográfica
description: Obtenga información sobre cómo configurar la Directiva de DNS para equilibrar la carga de una aplicación con reconocimiento de ubicación geográfica.
manager: brianlic
ms.topic: article
ms.assetid: b6e679c6-4398-496c-88bc-115099f3a819
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 05ba48daff7a1d1f313108f803d4165d2fb1b79f
ms.sourcegitcommit: 029b1e19ce11160d5f988046e04a83e8ab5a60dc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/05/2021
ms.locfileid: "97904600"
---
# <a name="use-dns-policy-for-application-load-balancing-with-geo-location-awareness"></a>Uso de la directiva de DNS para equilibrio de carga de aplicación con reconocimiento de ubicación geográfica

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información sobre cómo configurar la Directiva de DNS para equilibrar la carga de una aplicación con reconocimiento de ubicación geográfica.

En el tema anterior de esta guía, uso de la [Directiva de DNS para el equilibrio de carga de aplicaciones](./app-lb.md), se usa un ejemplo de una empresa ficticia-servicios de regalo de Contoso que proporciona servicios de regalos en línea y que tiene un sitio Web denominado contosogiftservices.com. La carga de servicios de regalos de Contoso equilibra su aplicación web en línea entre servidores de centros de seguridad de Norteamérica ubicados en Seattle, WA, Chicago, IL y Dallas, TX.

>[!NOTE]
>Se recomienda que se familiarice con el tema [uso de la Directiva de DNS para el equilibrio de carga de la aplicación](./app-lb.md) antes de realizar las instrucciones de este escenario.

En este tema se utiliza la misma infraestructura ficticia de la empresa y la red como base para una nueva implementación de ejemplo que incluye el reconocimiento de la ubicación geográfica.

En este ejemplo, contoso Gift Services está expandiendo correctamente su presencia en todo el mundo.

De forma similar a Norteamérica, la compañía ahora tiene servidores web hospedados en centros de recursos europeos.

Los administradores de DNS de los servicios de regalos de Contoso desean configurar el equilibrio de carga de la aplicación para los centros de centros de recursos de forma similar a la implementación de directivas DNS en el Estados Unidos, con el tráfico de la aplicación distribuido entre los servidores web que se encuentran en Dublín, Irlanda, Ámsterdam, Holanda y otros.

Los administradores de DNS también quieren que todas las consultas de otras ubicaciones del mundo se distribuyan equitativamente entre todos sus centros de recursos.

En las secciones siguientes puede obtener información sobre cómo lograr objetivos similares a los de los administradores de DNS de Contoso en su propia red.

## <a name="how-to-configure-application-load-balancing-with-geo-location-awareness"></a>Cómo configurar el equilibrio de carga de la aplicación con conocimiento de Geo-Location

En las secciones siguientes se muestra cómo configurar la Directiva de DNS para el equilibrio de carga de aplicaciones con reconocimiento de ubicación geográfica.

>[!IMPORTANT]
>En las secciones siguientes se incluyen comandos de Windows PowerShell de ejemplo que contienen valores de ejemplo para muchos parámetros. Asegúrese de reemplazar los valores de ejemplo de estos comandos por los valores adecuados para su implementación antes de ejecutar estos comandos.

### <a name="create-the-dns-client-subnets"></a><a name="bkmk_clientsubnets"></a>Crear las subredes de cliente DNS

En primer lugar, debe identificar las subredes o el espacio de direcciones IP de las regiones Norteamérica y Europa.

Puede obtener esta información de mapas de IP geográfica. En función de estas distribuciones de IP geográfica, debe crear las subredes de cliente DNS.

Una subred de cliente DNS es una agrupación lógica de subredes IPv4 o IPv6 desde la que se envían las consultas a un servidor DNS.

Puede usar los siguientes comandos de Windows PowerShell para crear subredes de cliente DNS.

```powershell
Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet 192.0.0.0/24,182.0.0.0/24
Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet 141.1.0.0/24,151.1.0.0/24
```

Para obtener más información, consulte [Add-DnsServerClientSubnet](/powershell/module/dnsserver/add-dnsserverclientsubnet).

### <a name="create-the-zone-scopes"></a><a name="bkmk_zscopes2"></a>Crear los ámbitos de zona

Una vez que las subredes de cliente están en su lugar, debe particionar la zona contosogiftservices.com en distintos ámbitos de zona, cada uno para un centro de recursos.

Un ámbito de zona es una instancia única de la zona. Una zona DNS puede tener varios ámbitos de zona, con cada ámbito de zona que contenga su propio conjunto de registros DNS. El mismo registro puede estar presente en varios ámbitos, con direcciones IP diferentes o con las mismas direcciones IP.

>[!NOTE]
>De forma predeterminada, existe un ámbito de zona en las zonas DNS. Este ámbito de zona tiene el mismo nombre que la zona y las operaciones DNS heredadas funcionan en este ámbito.

En el escenario anterior del equilibrio de carga de la aplicación se muestra cómo configurar los ámbitos de tres zonas para los centros de recursos en Norteamérica.

Con los siguientes comandos, puede crear dos ámbitos de zona más, uno para los centros de los centros de información de Dublín y Amsterdam.

Puede agregar estos ámbitos de zona sin realizar ningún cambio en los tres ámbitos de zona Norteamérica existentes en la misma zona. Además, después de crear estos ámbitos de zona, no es necesario reiniciar el servidor DNS.

Puede usar los siguientes comandos de Windows PowerShell para crear ámbitos de zona.

```powershell
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AmsterdamZoneScope"
```

Para obtener más información, consulte [Add-DnsServerZoneScope](/powershell/module/dnsserver/add-dnsserverzonescope)

### <a name="add-records-to-the-zone-scopes"></a><a name="bkmk_records2"></a>Agregar registros a los ámbitos de zona

Ahora debe agregar los registros que representan el host del servidor Web en los ámbitos de zona.

Los registros de los centros de recursos de América se agregaron en el escenario anterior. Puede usar los siguientes comandos de Windows PowerShell para agregar registros a los ámbitos de zona para los centros de recursos europeos.

```powershell
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "151.1.0.1" -ZoneScope "DublinZoneScope”
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "AmsterdamZoneScope"
```

Para obtener más información, consulte [Add-DnsServerResourceRecord](/powershell/module/dnsserver/add-dnsserverresourcerecord).

### <a name="create-the-dns-policies"></a><a name="bkmk_policies2"></a>Crear las directivas de DNS

Después de crear las particiones (ámbitos de zona) y de haber agregado registros, debe crear directivas DNS que distribuyan las consultas entrantes en estos ámbitos.

En este ejemplo, la distribución de consultas entre servidores de aplicaciones en distintos centros de recursos cumple los siguientes criterios.

1. Cuando se recibe la consulta DNS de un origen en una subred de cliente estadounidense, el 50% de las respuestas DNS apuntan al centro de datos de Seattle, el 25% de las respuestas señalan al centro de datos de Chicago y el 25% de las respuestas restantes apuntan al centro de datos de Dallas.
2. Cuando se recibe la consulta DNS de un origen en una subred de cliente Europea, el 50% de las respuestas DNS apuntan al centro de recursos de Dublín y el 50% de las respuestas DNS apuntan al centro de recursos de Amsterdam.
3. Cuando la consulta proviene de cualquier otra parte del mundo, las respuestas DNS se distribuyen entre los cinco centros de recursos.

Puede usar los siguientes comandos de Windows PowerShell para implementar estas directivas DNS.

```powershell
Add-DnsServerQueryResolutionPolicy -Name "AmericaLBPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1; TexasZoneScope,1" -ZoneName "contosogiftservices.com" –ProcessingOrder 1
Add-DnsServerQueryResolutionPolicy -Name "EuropeLBPolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 2
Add-DnsServerQueryResolutionPolicy -Name "WorldWidePolicy" -Action ALLOW -FQDN "eq,*.contoso.com" -ZoneScope "SeattleZoneScope,1;ChicagoZoneScope,1; TexasZoneScope,1;DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3
```

Para obtener más información, consulte [Add-DnsServerQueryResolutionPolicy](/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy).

Ahora ha creado correctamente una directiva DNS que proporciona equilibrio de carga de aplicaciones entre servidores web que se encuentran en cinco centros de recursos diferentes en varios continentes.

Puede crear miles de directivas DNS según los requisitos de administración del tráfico y todas las directivas nuevas se aplican de forma dinámica, sin necesidad de reiniciar el servidor DNS, en las consultas entrantes.
