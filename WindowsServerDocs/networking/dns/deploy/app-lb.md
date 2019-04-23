---
title: Uso de la directiva de DNS para el equilibrio de carga de aplicación
description: En este tema forma parte de las DNS directiva escenario guía para Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: f9c313ac-bb86-4e48-b9b9-de5004393e06
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1bb3e6695a7ec8fc7d950873403df023b4def3d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881616"
---
# <a name="use-dns-policy-for-application-load-balancing"></a>Uso de la directiva de DNS para el equilibrio de carga de aplicación

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para aprender a configurar la directiva DNS para realizar el equilibrio de carga de aplicaciones.

Las versiones anteriores de Windows Server DNS solo proporcionan equilibrio de carga mediante el uso de las respuestas de operación por turnos; pero con DNS en Windows Server 2016, puede configurar la directiva DNS para el equilibrio de carga de aplicaciones.

Cuando implemente varias instancias de una aplicación, puede usar la directiva de DNS para equilibrar la carga de tráfico entre las instancias de aplicación diferente, asignar dinámicamente, por tanto, la carga de tráfico para la aplicación.

## <a name="example-of-application-load-balancing"></a>Ejemplo de equilibrio de carga de aplicación

La siguiente es un ejemplo de cómo puede usar Directiva de DNS para el equilibrio de carga de aplicación.

Este ejemplo usa una compañía ficticia - servicios de regalo de Contoso - que proporciona servicios en línea gifing y que tiene un sitio Web denominado **contosogiftservices.com**.

El sitio Web de contosogiftservices.com se hospeda en varios centros de datos que cada uno tiene distintas direcciones IP.

En Norteamérica, que es el mercado principal de servicios de regalo de Contoso, el sitio Web se hospeda en tres centros de datos: Chicago, IL, Dallas, TX and Seattle, WA.

El servidor Web de Seattle tiene la mejor configuración de hardware y puede controlar el doble de carga como los otros dos sitios. Servicios de regalos de Contoso quiere que el tráfico de aplicación dirigido de la siguiente manera.

- Dado que el servidor Web de Seattle incluye más recursos, la mitad de los clientes de la aplicación se dirigen a este servidor
- Un cuarto de los clientes de la aplicación se dirigen al centro de datos de Dallas, Texas
- Un cuarto de los clientes de la aplicación se dirigen al centro de datos de Chicago, IL,

La siguiente ilustración muestra este escenario.

![DNS aplicación equilibrio de carga con directiva de DNS](../../media/Dns-App-Lb/dns-app-lb.jpg)


### <a name="how-application-load-balancing-works"></a>Cómo la aplicación funciona Equilibrio de carga

Después de haber configurado el servidor DNS con la directiva DNS para su aplicación cargue equilibrio con este escenario de ejemplo, el servidor DNS responde el 50% del tiempo con la dirección del servidor Web de Seattle, 25% del tiempo con la dirección del servidor Web de Dallas y el 25% del tiempo con la dirección del servidor Web de Chicago.

Por lo tanto para cada cuatro consultas que recibe el servidor DNS, responde con las dos respuestas para Seattle y uno para cada uno de Dallas y Chicago.

Un posible problema con equilibrio de carga con la directiva de DNS es el almacenamiento en caché de registros DNS por el cliente DNS y la resolución/LDNS, que puede interferir con equilibrio de carga porque el cliente o la resolución no enviar una consulta al servidor DNS.

Puede mitigar el efecto de este comportamiento mediante el uso de un bajo tiempo\-a\-Live \(TTL\) el valor de los registros DNS que debe tener equilibrio de carga.

### <a name="how-to-configure-application-load-balancing"></a>Cómo configurar el equilibrio de carga de aplicación

Las secciones siguientes muestran cómo configurar la directiva de DNS para el equilibrio de carga de aplicación.

#### <a name="create-the-zone-scopes"></a>Creación de los ámbitos de zona

En primer lugar debe crear los ámbitos de la zona de contosogiftservices.com para los centros de datos donde estén hospedadas.

Un ámbito de la zona es una instancia única de la zona. Una zona DNS puede tener varios ámbitos de zona, con cada ámbito de la zona que contiene su propio conjunto de registros DNS. El mismo registro puede estar presente en varios ámbitos, con direcciones IP diferentes o las mismas direcciones IP.

>[!NOTE]
>De forma predeterminada, un ámbito de la zona existe en las zonas DNS. Este ámbito de la zona tiene el mismo nombre que la zona y las operaciones de DNS heredadas funcionan en este ámbito.

Puede usar los siguientes comandos de Windows PowerShell para crear ámbitos de la zona.
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "SeattleZoneScope"
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DallasZoneScope"
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "ChicagoZoneScope"

Para obtener más información, consulte [agregar DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

####<a name="bkmk_records"></a>Agregar registros a los ámbitos de zona

Ahora debe agregar los registros que representa el host del servidor web en los ámbitos de la zona.

En **SeattleZoneScope**, puede agregar el registro www.contosogiftservices.com con dirección IP 192.0.0.1, que se encuentra en el centro de datos de Seattle.

En **ChicagoZoneScope**, puede agregar el mismo registro \(www.contosogiftservices.com\) con dirección IP 182.0.0.1 en el centro de datos de Chicago.

De forma similar en **DallasZoneScope**, puede agregar un registro \(www.contosogiftservices.com\) con dirección IP 162.0.0.1 en el centro de datos de Chicago.

Puede usar los siguientes comandos de Windows PowerShell para agregar registros a los ámbitos de la zona.
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "SeattleZoneScope
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "182.0.0.1" -ZoneScope "ChicagoZoneScope"
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "162.0.0.1" -ZoneScope "DallasZoneScope"
    

Para obtener más información, consulte [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).

####<a name="bkmk_policies"></a>Cree las directivas DNS

Una vez que ha creado las particiones (ámbitos de zona) y se han agregado registros, debe crear las directivas DNS que distribución las consultas entrantes entre estos ámbitos para que el 50% de las consultas para contosogiftservices.com se responde con la dirección IP para la Web servidor en el centro de datos de Seattle y el resto se distribuyen equitativamente entre los centros de datos de Chicago y Dallas.

Puede usar los siguientes comandos de Windows PowerShell para crear una directiva DNS que equilibra el tráfico de la aplicación en estas tres centros de datos.

>[!NOTE]
>En el comando de ejemplo siguiente, la expresión – zonaámbito "SeattleZoneScope, 2; ChicagoZoneScope, 1; DallasZoneScope, 1" se configura el servidor DNS con una matriz que incluye la combinación de parámetros \<zonaámbito\>,\<peso\>.
    
    Add-DnsServerQueryResolutionPolicy -Name "AmericaPolicy" -Action ALLOW -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1;DallasZoneScope,1" -ZoneName "contosogiftservices.com"
    

Para obtener más información, consulte [agregar DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  

Ya ha creado correctamente una directiva DNS que se proporciona a través de servidores Web en tres centros de datos diferentes de equilibrio de carga de aplicación.

Puede crear miles de las directivas DNS según el tráfico de los requisitos de administración y todas las nuevas directivas se aplican dinámicamente - sin necesidad de reiniciar el servidor DNS, en las consultas entrantes.
