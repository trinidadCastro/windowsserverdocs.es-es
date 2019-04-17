---
title: Usar la directiva de DNS para la aplicación equilibrio de carga con reconocimiento de ubicación geográfica
description: En este tema es parte de la DNS directiva escenario guía para Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: b6e679c6-4398-496c-88bc-115099f3a819
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7637d927c7b22b83053e7f9100b07581c11bafc0
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-application-load-balancing-with-geo-location-awareness"></a>Usar la directiva de DNS para la aplicación equilibrio de carga con reconocimiento de ubicación geográfica

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para aprender a configurar la directiva DNS para equilibrar la carga una aplicación con reconocimiento de ubicación geográfica.

El tema anterior en esta guía, [usar la directiva de DNS equilibrio de carga de aplicación](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/app-lb), usa un ejemplo de una empresa ficticia - servicios de regalo de Contoso - que proporciona regalar online services y que tiene un sitio Web llamada contosogiftservices.com. Carga de los servicios de regalo de Contoso saldos de su aplicación Web en línea entre los servidores de centros de datos norteamericano ubicados en Seattle, WA, Chicago, IL y Dallas, Texas.

>[!NOTE]
>Es recomendable que te familiarices con el tema [usar la directiva de DNS equilibrio de carga de aplicación](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/app-lb) antes de realizar las instrucciones de este escenario.

En este tema se usa la misma empresa ficticia y la infraestructura de red como base para una nueva implementación de ejemplo que incluye el reconocimiento de ubicación geográfica.

En este ejemplo, Contoso regalo servicios está creciendo correctamente su presencia en todo el mundo.

Al igual que Norteamérica, la empresa tiene ahora servidores web hospedados en Europeo centros de datos.

Los administradores de Contoso regalo servicios DNS desee configurar aplicación equilibrio de carga para Europeo centros de datos de forma similar a la implementación de directiva DNS en los Estados Unidos, con tráfico de la aplicación que se distribuyen entre servidores Web que se encuentran en Holland Dublín, Irlanda, Ámsterdam y en otros lugares.

Los administradores de DNS también te interese todas las consultas de otras ubicaciones en el mundo distribuir de forma equitativa entre todos sus centros de datos.

En las secciones siguientes puedes aprender a lograr los objetivos similares a las de los administradores de DNS de Contoso en su propia red.

## <a name="how-to-configure-application-load-balancing-with-geo-location-awareness"></a>Cómo configurar la aplicación equilibrio de carga con reconocimiento de ubicación geográfica

Las siguientes secciones muestran cómo configurar la directiva DNS para la aplicación equilibrio de carga con reconocimiento de ubicación geográfica.

>[!IMPORTANT]
>Las secciones siguientes incluyen ejemplos de comandos de Windows PowerShell que contienen los valores de ejemplo para el número de parámetros. Asegúrate de que los valores de ejemplo en estos comandos, se reemplaza con valores que son adecuados para su implementación antes de ejecutar estos comandos.

###<a name="bkmk_clientsubnets"></a>Crear las subredes del cliente DNS

Primero deben identificar las subredes o espacio de direcciones IP de las regiones de América del Norte y Europa.

Puedes obtener esta información de mapas geográficos IP. En función de estas distribuciones geográfica IP, debes crear las subredes del cliente DNS.

Una subred del cliente DNS es una agrupación lógica de subredes IPv4 o IPv6 desde el que se envían consultas a un servidor DNS.

Puedes usar los siguientes comandos de Windows PowerShell para crear subredes del cliente DNS. 

    
    Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet 192.0.0.0/24,182.0.0.0/24
    Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet 141.1.0.0/24,151.1.0.0/24
    
Para obtener más información, consulta [agregar DnsServerClientSubnet](https://technet.microsoft.com/library/mt126261.aspx).

###<a name="bkmk_zscopes2"></a>Crear los ámbitos zona

Después de las subredes del cliente en su lugar, debes dividir la contosogiftservices.com zona en ámbitos zona diferente, cada una para un centro de datos.

Un ámbito de la zona es una instancia única de la zona. Una zona DNS puede tener varios ámbitos zona, con cada ámbito de la zona que contiene su propio conjunto de registros de DNS. El mismo registro puede encontrarse en varios ámbitos, con diferentes direcciones IP o las mismas direcciones IP.

>[!NOTE]
>De manera predeterminada, existe un ámbito de la zona en las zonas DNS. El ámbito de esta zona tiene el mismo nombre que la zona y las operaciones de DNS heredadas funcionan en este ámbito.

El escenario anterior en equilibrio de carga de la aplicación muestra cómo configurar tres ámbitos de centros de datos de la zona de Norteamérica.

Con los siguientes comandos, puedes crear dos ámbitos zona más, uno para los centros de datos Dublin y Amsterdam. 

Puedes agregar estos ámbitos zona sin cambios para los tres ámbitos de zona de Norteamérica existentes en la misma zona. Además, después de crear estos ámbitos zona, no es necesario reiniciar el servidor DNS.

Puedes usar los siguientes comandos de Windows PowerShell para crear ámbitos zona.

    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AmsterdamZoneScope"
    

Para obtener más información, consulta [agregar DnsServerZoneScope](https://technet.microsoft.com/library/mt126267.aspx)

###<a name="bkmk_records2"></a>Agregar registros a los ámbitos zona

Ahora debes agregar los registros que representa el host del servidor web en los ámbitos de la zona.

En el escenario anterior, se agregaron los registros de los centros de datos de América. Puedes usar los siguientes comandos de Windows PowerShell para agregar registros a los ámbitos de centros de datos Europeo zona.
 
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "151.1.0.1" -ZoneScope "DublinZoneScope”
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "AmsterdamZoneScope"
    

Para obtener más información, consulta [agregar DnsServerResourceRecord](https://technet.microsoft.com/library/jj649925.aspx).

###<a name="bkmk_policies2"></a>Crear las directivas DNS

Una vez que hayas creado las particiones (ámbitos zona) y se han agregado registros, debes crear directivas DNS que distribuyen las consultas entrantes a través de estos ámbitos.

Para este ejemplo, la distribución de la consulta en servidores de aplicaciones en los centros de datos distintos cumple los criterios siguientes.

1. Cuando se recibe la consulta DNS de un origen en una subred cliente norteamericano, 50% de las respuestas DNS señala el centro de datos de Seattle, elija el centro de datos de Chicago 25% de respuestas y el 25% restante de respuestas señala el centro de datos Dallas.
2. Cuando se recibe la consulta DNS de un origen en una subred cliente Europea, seleccione el centro de datos de Dublin 50% de las respuestas DNS y 50% de las respuestas DNS señala el centro de datos de Amsterdam.
3. Cuando la consulta procede de cualquier lugar del mundo, las respuestas DNS se distribuyen entre todos los centros de datos de cinco.

Puedes usar los siguientes comandos de Windows PowerShell para implementar estas directivas DNS.

    
    Add-DnsServerQueryResolutionPolicy -Name "AmericaLBPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1; TexasZoneScope,1" -ZoneName "contosogiftservices.com" –ProcessingOrder 1
    
    Add-DnsServerQueryResolutionPolicy -Name "EuropeLBPolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 2
    
    Add-DnsServerQueryResolutionPolicy -Name "WorldWidePolicy" -Action ALLOW -FQDN "eq,*.contoso.com" -ZoneScope "SeattleZoneScope,1;ChicagoZoneScope,1; TexasZoneScope,1;DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3
    
    

Para obtener más información, consulta [agregar DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx).

Ahora se ha creado correctamente una directiva DNS que proporciona la aplicación equilibrio de carga en servidores Web que se encuentran en cinco centros de datos distintos en varias continentes.

Puede crear miles de directivas DNS según el tráfico de requisitos de administración y todas las nuevas directivas se aplican de forma dinámica - sin reiniciar el servidor DNS, en las consultas entrantes.
