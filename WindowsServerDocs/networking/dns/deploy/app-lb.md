---
title: Uso de la directiva de DNS para el equilibrio de carga de aplicación
description: Obtenga información sobre cómo configurar la Directiva de DNS para realizar el equilibrio de carga de la aplicación.
manager: brianlic
ms.topic: article
ms.assetid: f9c313ac-bb86-4e48-b9b9-de5004393e06
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 849baf9e95af51d5bd3c3bd4460f181fa8836691
ms.sourcegitcommit: 029b1e19ce11160d5f988046e04a83e8ab5a60dc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/05/2021
ms.locfileid: "97904920"
---
# <a name="use-dns-policy-for-application-load-balancing"></a>Uso de la directiva de DNS para el equilibrio de carga de aplicación

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información sobre cómo configurar la Directiva de DNS para realizar el equilibrio de carga de la aplicación.

Las versiones anteriores de DNS de Windows Server solo proporcionaban equilibrio de carga mediante el uso de round robin respuestas; pero con DNS en Windows Server 2016, puede configurar la Directiva de DNS para el equilibrio de carga de la aplicación.

Una vez que haya implementado varias instancias de una aplicación, puede usar la Directiva de DNS para equilibrar la carga de tráfico entre las distintas instancias de aplicación, lo que asigna dinámicamente la carga de tráfico para la aplicación.

## <a name="example-of-application-load-balancing"></a>Ejemplo de equilibrio de carga de la aplicación

A continuación se describe un ejemplo de cómo se puede usar la Directiva de DNS para el equilibrio de carga de la aplicación.

En este ejemplo se usa una empresa ficticia-servicios de regalos de Contoso, que proporciona servicios de regalos en línea y que tiene un sitio web denominado **contosogiftservices.com**.

El sitio web de contosogiftservices.com se hospeda en varios centros de recursos, cada uno con distintas direcciones IP.

En Norteamérica, que es el mercado principal de los servicios de regalos de Contoso, el sitio web se hospeda en tres centros de recursos: Chicago, IL, Dallas, TX y Seattle, WA.

El servidor Web de Seattle tiene la mejor configuración de hardware y puede controlar el doble de carga que los otros dos sitios. Contoso Gift Services quiere que el tráfico de la aplicación se dirija de la manera siguiente.

- Dado que el servidor Web de Seattle incluye más recursos, la mitad de los clientes de la aplicación se dirige a este servidor
- Un cuarto de los clientes de la aplicación se dirige al centro de centros de usuarios de Dallas y TX
- Un cuarto de los clientes de la aplicación se dirige a Chicago, IL, Datacenter

En la ilustración siguiente se muestra este escenario.

![Equilibrio de carga de aplicaciones DNS con Directiva DNS](../../media/Dns-App-Lb/dns-app-lb.jpg)


### <a name="how-application-load-balancing-works"></a>Cómo funciona el equilibrio de carga de la aplicación

Después de configurar el servidor DNS con la Directiva de DNS para el equilibrio de carga de la aplicación mediante este escenario de ejemplo, el servidor DNS responde el 50% del tiempo con la dirección del servidor Web de Seattle, el 25% del tiempo con la dirección del servidor Web de Dallas y el 25% del tiempo con la dirección del servidor Web de Chicago.

Por lo tanto, para cada cuatro consultas que recibe el servidor DNS, responde con dos respuestas para Seattle y otra para Dallas y Chicago.

Un posible problema con el equilibrio de carga con la Directiva de DNS es el almacenamiento en caché de los registros DNS por parte del cliente DNS y el solucionador/LDNS, que pueden interferir con el equilibrio de carga porque el cliente o el solucionador no envían una consulta al servidor DNS.

Puede mitigar el efecto de este comportamiento mediante el uso de un valor de TTL de bajo período \- \- de vida \( \) para los registros DNS cuya carga se debe equilibrar.

### <a name="how-to-configure-application-load-balancing"></a>Cómo configurar el equilibrio de carga de la aplicación

En las secciones siguientes se muestra cómo configurar la Directiva de DNS para el equilibrio de carga de la aplicación.

#### <a name="create-the-zone-scopes"></a>Crear los ámbitos de zona

En primer lugar, debe crear los ámbitos de la zona contosogiftservices.com para los centros de recursos en los que se hospedan.

Un ámbito de zona es una instancia única de la zona. Una zona DNS puede tener varios ámbitos de zona, con cada ámbito de zona que contenga su propio conjunto de registros DNS. El mismo registro puede estar presente en varios ámbitos, con direcciones IP diferentes o con las mismas direcciones IP.

>[!NOTE]
>De forma predeterminada, existe un ámbito de zona en las zonas DNS. Este ámbito de zona tiene el mismo nombre que la zona y las operaciones DNS heredadas funcionan en este ámbito.

Puede usar los siguientes comandos de Windows PowerShell para crear ámbitos de zona.

```powershell
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "SeattleZoneScope"
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DallasZoneScope"
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "ChicagoZoneScope"
```

Para obtener más información, consulte [Add-DnsServerZoneScope](/powershell/module/dnsserver/add-dnsserverzonescope)

#### <a name="add-records-to-the-zone-scopes"></a><a name="bkmk_records"></a>Agregar registros a los ámbitos de zona

Ahora debe agregar los registros que representan el host del servidor Web en los ámbitos de zona.

En **SeattleZoneScope**, puede Agregar el registro www.contosogiftservices.com con la dirección IP 192.0.0.1, que se encuentra en el centro de recursos de Seattle.

En **ChicagoZoneScope**, puede Agregar el mismo registro \( www.contosogiftservices.com \) con la dirección IP 182.0.0.1 en el centro de recursos de Chicago.

Del mismo modo en **DallasZoneScope**, puede Agregar un registro \( www.contosogiftservices.com \) con la dirección IP 162.0.0.1 en el centro de los centros de ti.

Puede usar los siguientes comandos de Windows PowerShell para agregar registros a los ámbitos de zona.

```powershell
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "SeattleZoneScope"
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "182.0.0.1" -ZoneScope "ChicagoZoneScope"
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "162.0.0.1" -ZoneScope "DallasZoneScope"
```

Para obtener más información, consulte [Add-DnsServerResourceRecord](/powershell/module/dnsserver/add-dnsserverresourcerecord).

#### <a name="create-the-dns-policies"></a><a name="bkmk_policies"></a>Crear las directivas de DNS

Después de haber creado las particiones (ámbitos de zona) y de haber agregado registros, debe crear directivas DNS que distribuyan las consultas entrantes en estos ámbitos para que el 50% de las consultas de contosogiftservices.com se respondan con la dirección IP del servidor Web en el centro de centros de recursos de Seattle y el resto se distribuya equitativamente entre los centros de recursos de Chicago y Dallas.

Puede usar los siguientes comandos de Windows PowerShell para crear una directiva DNS que equilibre el tráfico de la aplicación en estos tres centros de recursos.

>[!NOTE]
>En el siguiente comando de ejemplo, la expresión-ZoneScope "SeattleZoneScope, 2; ChicagoZoneScope, 1; DallasZoneScope, 1 "configura el servidor DNS con una matriz que incluye la combinación de parámetros `<ZoneScope>` , `<weight>` .

```powershell
Add-DnsServerQueryResolutionPolicy -Name "AmericaPolicy" -Action ALLOW -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1;DallasZoneScope,1" -ZoneName "contosogiftservices.com"
```

Para obtener más información, consulte [Add-DnsServerQueryResolutionPolicy](/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy).

Ahora ha creado correctamente una directiva DNS que proporciona equilibrio de carga de aplicaciones entre servidores Web en tres centros de recursos diferentes.

Puede crear miles de directivas DNS según los requisitos de administración del tráfico y todas las directivas nuevas se aplican de forma dinámica, sin necesidad de reiniciar el servidor DNS, en las consultas entrantes.
