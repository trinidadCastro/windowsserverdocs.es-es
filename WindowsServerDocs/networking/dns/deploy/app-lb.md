---
title: Usar la directiva de DNS para equilibrio de carga de aplicación
description: En este tema es parte de la DNS directiva escenario guía para Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: f9c313ac-bb86-4e48-b9b9-de5004393e06
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d156c258b971c45bf1c4c20739440bd5cc9e239f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-application-load-balancing"></a>Usar la directiva de DNS para equilibrio de carga de aplicación

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para aprender a configurar la directiva DNS para realizar el equilibrio de carga de la aplicación.

Las versiones anteriores de Windows Server DNS proporcionan solo mediante el uso de las respuestas por turnos; de equilibrio de carga pero con DNS en Windows Server 2016, puedes configurar la directiva DNS equilibrio de carga en la aplicación.

Cuando se implementaron varias instancias de una aplicación, puedes usar la directiva de DNS para equilibrar la carga de tráfico entre las instancias de la aplicación diferente, asignar lo dinámicamente la carga de tráfico de la aplicación.

## <a name="example-of-application-load-balancing"></a>Ejemplo de equilibrio de carga de la aplicación

Siguiente es un ejemplo de cómo puedes usar la directiva DNS equilibrio de carga en la aplicación.

Este ejemplo usa una empresa ficticia - servicios de regalo de Contoso - que proporciona servicios gifing en línea, y que tiene un sitio Web denominado **contosogiftservices.com**.

El sitio Web de contosogiftservices.com está hospedado en varios centros de datos que cada uno tiene diferentes direcciones IP.

En Estados Unidos, que es el mercado principal de servicios de regalo de Contoso, el sitio Web está hospedado en tres centros de datos: Chicago, IL, Dallas, transmisión y Seattle, Washington.

El servidor Web de Seattle tiene la mejor configuración de hardware y puede controlar el doble de carga como los otros dos sitios. Servicios de regalos de Contoso quiere tráfico de la aplicación dirigido de la siguiente manera.

- Dado que el servidor Web de Seattle incluye más recursos, la mitad de los clientes de la aplicación se dirige a este servidor
- Un cuarto de clientes de la aplicación se dirige al centro Dallas, transmisión de datos
- Un cuarto de clientes de la aplicación se dirige al centro de datos de Chicago, IL,

La ilustración siguiente muestra este escenario.

![DNS aplicación equilibrio de carga con directiva DNS](../../media/Dns-App-Lb/dns-app-lb.jpg)


### <a name="how-application-load-balancing-works"></a>Funciona Equilibrio de carga de la aplicación

Después de haber configurado el servidor DNS con la directiva DNS para la aplicación carga usando equilibrado como ejemplo este escenario, el servidor DNS responde 50% del tiempo con la dirección del servidor Web de Seattle, 25% del tiempo con la dirección del servidor Web Dallas y 25% del tiempo con la dirección del servidor Web de Chicago.

Por lo tanto, para cada cuatro consultas que recibe el servidor DNS, responde con dos respuestas probables de Seattle y uno para Dallas y Chicago.

Uno de los posibles problemas con equilibrio de carga con la directiva DNS es el almacenamiento en caché de registros DNS, el cliente DNS y la resolución o LDNS, que puede interferir con el equilibrio de carga porque el cliente o la resolución de no enviar una consulta al servidor DNS.

Puedes mitigar el efecto de este comportamiento mediante el uso de un valor bajo de Live para Time\ to\ \(TTL\) para los registros DNS que deberían estar equilibrada de carga.

### <a name="how-to-configure-application-load-balancing"></a>Cómo configurar el equilibrio de carga de la aplicación

Las siguientes secciones muestran cómo configurar la directiva DNS equilibrio de carga en la aplicación.

#### <a name="create-the-zone-scopes"></a>Crear los ámbitos zona

Primero debes crear los ámbitos de contosogiftservices.com de la zona de los centros de datos donde se hospedan.

Un ámbito de la zona es una instancia única de la zona. Una zona DNS puede tener varios ámbitos zona, con cada ámbito de la zona que contiene su propio conjunto de registros de DNS. El mismo registro puede encontrarse en varios ámbitos, con diferentes direcciones IP o las mismas direcciones IP.

>[!NOTE]
>De manera predeterminada, existe un ámbito de la zona en las zonas DNS. El ámbito de esta zona tiene el mismo nombre que la zona y las operaciones de DNS heredadas funcionan en este ámbito.

Puedes usar los siguientes comandos de Windows PowerShell para crear ámbitos zona.
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "SeattleZoneScope"
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DallasZoneScope"
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "ChicagoZoneScope"

Para obtener más información, consulta [agregar DnsServerZoneScope](https://technet.microsoft.com/library/mt126267.aspx)

####<a name="bkmk_records"></a>Agregar registros a los ámbitos zona

Ahora debes agregar los registros que representa el host del servidor web en los ámbitos de la zona.

En **SeattleZoneScope**, puedes agregar el registro www.contosogiftservices.com con la dirección IP 192.0.0.1, que se encuentra en el centro de datos de Seattle.

En **ChicagoZoneScope**, puedes agregar la misma \(www.contosogiftservices.com\) registro con la dirección IP 182.0.0.1 en el centro de datos de Chicago.

Del mismo modo en **DallasZoneScope**, puedes agregar un \(www.contosogiftservices.com\) registro con la dirección IP 162.0.0.1 en el centro de datos de Chicago.

Puedes usar los siguientes comandos de Windows PowerShell para agregar registros a los ámbitos de la zona.
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "SeattleZoneScope
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "182.0.0.1" -ZoneScope "ChicagoZoneScope"
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "162.0.0.1" -ZoneScope "DallasZoneScope"
    

Para obtener más información, consulta [agregar DnsServerResourceRecord](https://technet.microsoft.com/library/jj649925.aspx).

####<a name="bkmk_policies"></a>Crear las directivas DNS

Una vez que hayas creado las particiones (ámbitos zona) y se han agregado registros, debes crear directivas DNS que distribuyen las consultas entrantes a través de estos ámbitos para que se ha respondido a 50% de consultas para contosogiftservices.com con la dirección IP del servidor Web del centro de datos de Seattle y el resto se distribuya de igual manera entre los centros de datos de Chicago y Dallas.

Puedes usar los siguientes comandos de Windows PowerShell para crear una directiva DNS que equilibra el tráfico de la aplicación a través de estos tres centros de datos.

>[!NOTE]
>En el comando de ejemplo a continuación, la expresión: zonaámbito "SeattleZoneScope, 2; ChicagoZoneScope, 1; DallasZoneScope, 1" se configura el servidor DNS con una matriz que incluye la combinación de parámetro \ < ZoneScope\, > \ < weight\ >.
    
    Add-DnsServerQueryResolutionPolicy -Name "AmericaPolicy" -Action ALLOW – -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1;DallasZoneScope,1" -ZoneName "contosogiftservices.com"
    

Para obtener más información, consulta [agregar DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx).  

Ahora se ha creado correctamente una directiva DNS que proporciona equilibrio entre servidores Web en los centros de datos distintos tres carga de la aplicación.

Puede crear miles de directivas DNS según el tráfico de requisitos de administración y todas las nuevas directivas se aplican de forma dinámica - sin reiniciar el servidor DNS, en las consultas entrantes.