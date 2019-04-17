---
title: Las respuestas DNS en función de la hora del día con una Azure servidor de aplicaciones en la nube
description: En este tema es parte de la DNS directiva escenario guía para Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 4846b548-8fbc-4a7f-af13-09e834acdec0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3255d3fe29f6a7dda821183fa4352964cc230a5f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="dns-responses-based-on-time-of-day-with-an-azure-cloud-app-server"></a>Las respuestas DNS en función de la hora del día con una Azure servidor de aplicaciones en la nube

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener información sobre cómo distribuir el tráfico de la aplicación en diferentes instancias geográficamente de una aplicación mediante las directivas DNS que se basan en el momento del día. 

Este escenario resulta útil en situaciones donde quieras dirigir el tráfico en una zona horaria a servidores de la aplicación alternativo, como los servidores Web que se hospedan en Microsoft Azure, que se encuentran en otra zona horaria. Esto permite la carga de tráfico de equilibrio entre instancias de la aplicación durante los períodos de cuando se sobrecargan tus servidores principales con el tráfico de períodos de tiempo. 

>[!NOTE]
>Para aprender a usar la directiva de DNS para las respuestas DNS inteligentes sin usar Azure, consulta [usar la directiva de DNS para inteligente DNS respuestas en función de la hora del día](Scenario--Use-DNS-Policy-for-Intelligent-DNS-Responses-Based-on-the-Time-of-Day.md). 

## <a name="bkmk_azureexample"></a>Ejemplo de respuestas DNS inteligente en función de la hora del día con el servidor de aplicaciones de la nube de Azure

Siguiente es un ejemplo de cómo puedes usar la directiva DNS para tráfico saldo según la hora del día.

Este ejemplo usa una empresa ficticia, servicios de regalo de Contoso, que proporciona soluciones en línea regalar en todo el mundo a través de su sitio Web, contosogiftservices.com. 

El sitio Web de contosogiftservices.com se hospeda solo en un centro de datos local solo en Seattle (con 192.68.30.2 IP pública). 

El servidor DNS también se encuentra en el centro de datos local. 

Con un aumento reciente de empresa, contosogiftservices.com tiene un mayor número de visitantes cada día y algunos de los clientes han informado de problemas de disponibilidad de servicio. 

Servicios de regalos de Contoso realiza un análisis de sitio y descubre que la cada noche entre 6 P.M. y 9 P.M. hora local, hay un aumento en el tráfico en el servidor Web de Seattle. El servidor Web no se puede escalar para controlar el incremento del tráfico a estas horas pico, lo que provocaría una denegación de servicio a los clientes. 

Para garantizar que los clientes contosogiftservices.com obtendrán una experiencia con capacidad de respuesta desde el sitio Web, servicios de regalos de Contoso decide durante estas horas se alquilar un virtual máquina \(VM\) en Microsoft Azure para hospedar una copia de su servidor Web.  

Servicios de regalos de Contoso, se obtiene una dirección IP pública de Azure para la máquina virtual (192.68.31.44) y desarrolla la automatización para implementar el servidor Web de cada día en Azure entre 5-10 PM, lo que permite durante un período de contingencia en una hora.

>[!NOTE]
>Para obtener más información acerca de máquinas virtuales de Azure, consulta [documentación de máquinas virtuales](https://azure.microsoft.com/documentation/services/virtual-machines/) 

Los servidores DNS están configurados con ámbitos de la zona y directivas DNS para que entre 5-9 h cada día, 30 por ciento de las consultas se envían a la instancia de servidor Web que se ejecuta en Azure.

La ilustración siguiente muestra este escenario.

![Directiva de DNS para tiempo de respuestas de día](../../media/DNS-Policy-Tod2/dns_policy_tod2.jpg)  

## <a name="bkmk_azurehow"></a>Cómo las respuestas de DNS inteligente en función de la hora del día con Azure funciona servidor de aplicaciones
 
En este artículo muestra cómo configurar el servidor DNS para responder a las consultas DNS con dos aplicaciones diferentes direcciones IP - un servidor web está en Seattle y la otra es en un centro de datos de Azure.

Después de la configuración de una nueva directiva DNS que se basa en las horas pico de 6 PM a 9 PM en Seattle, el servidor DNS envía setenta por ciento de las respuestas DNS a los clientes que contiene la dirección IP del servidor Web de Seattle y treinta por ciento de las respuestas DNS a los clientes que contiene la dirección IP del servidor Web de Azure, lo que dirigir el tráfico del cliente al servidor Web de Azure nuevo y evitar que el servidor Web de Seattle se sobrecarguen. 

En cualquier otro momento del día, tiene lugar el procesamiento normal de consulta y las respuestas se envían desde el ámbito de la zona de predeterminado que contiene un registro para el servidor web en el centro de datos local. 

El TTL de 10 minutos en el registro de Azure garantiza que el registro de expiración de la memoria caché LDNS antes de quita la máquina virtual de Azure. Una de las ventajas de este ajuste de tamaño es que puede conservar sus datos DNS en instalaciones y mantener el escalado a Azure como requiere la demanda.

## <a name="bkmk_azureconfigure"></a>Cómo configurar la directiva DNS para las respuestas DNS inteligente en función de la hora del día con el servidor de aplicaciones de Azure
Para configurar la directiva DNS para las respuestas de consulta de tiempo de equilibrio de carga de aplicación de día según, debes realizar los siguientes pasos.


- [Crear los ámbitos zona](#bkmk_zscopes)
- [Agregar registros a los ámbitos zona](#bkmk_records)
- [Crear las directivas DNS](#bkmk_policies)


>[!NOTE]
>Debes realizar estos pasos en el servidor DNS que está autorizado para la zona que deseas configurar. Pertenencia a grupo Administradores DNS o equivalente, es necesario para realizar los siguientes procedimientos. 

Las siguientes secciones proporcionan instrucciones de configuración detallada.

>[!IMPORTANT]
>Las secciones siguientes incluyen ejemplos de comandos de Windows PowerShell que contienen los valores de ejemplo para el número de parámetros. Asegúrate de que los valores de ejemplo en estos comandos, se reemplaza con valores que son adecuados para su implementación antes de ejecutar estos comandos. 


### <a name="bkmk_zscopes"></a>Crear los ámbitos zona
Un ámbito de la zona es una instancia única de la zona. Una zona DNS puede tener varios ámbitos zona, con cada ámbito de la zona que contiene su propio conjunto de registros de DNS. El mismo registro puede encontrarse en varios ámbitos, con diferentes direcciones IP o las mismas direcciones IP. 

>[!NOTE]
>De manera predeterminada, existe un ámbito de la zona en las zonas DNS. El ámbito de esta zona tiene el mismo nombre que la zona y las operaciones de DNS heredadas funcionan en este ámbito. 

Puedes usar el siguiente comando de ejemplo para crear un ámbito de la zona para hospedar los registros de Azure.

```
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AzureZoneScope"
```

Para obtener más información, consulta [agregar DnsServerZoneScope](https://technet.microsoft.com/library/mt126267.aspx)

### <a name="bkmk_records"></a>Agregar registros a los ámbitos zona
El siguiente paso es agregar los registros que representa el host del servidor Web en los ámbitos de la zona. 

En AzureZoneScope, se agrega el registro www.contosogiftservices.com con la dirección IP 192.68.31.44, que se encuentra en la nube pública de Azure. 

Del mismo modo, en el predeterminado zona ámbito \(contosogiftservices.com\), se agrega un \(www.contosogiftservices.com\) registro con la dirección IP 192.68.30.2 del servidor Web que se ejecuta en el centro de datos de Seattle local.

En el segundo cmdlet, no se incluye el parámetro: zonaámbito. Por este motivo, los registros se agregan en el valor predeterminado zonaámbito. 

Además, el TTL del registro para máquinas virtuales de Azure se mantiene en 600s (10 minutos) para que la LDNS no almacenarla en caché durante más tiempo - que pueden interferir con el equilibrio de carga. Además, las máquinas virtuales de Azure están disponibles para 1 hora adicional como contingencia para garantizar que los clientes incluso con registros almacenados en caché son capaces de resolver.

```
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.31.44" -ZoneScope "AzureZoneScope" –TimeToLive 600

Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.30.2"
```

Para obtener más información, consulta [agregar DnsServerResourceRecord](https://technet.microsoft.com/library/jj649925.aspx).  

### <a name="bkmk_policies"></a>Crear las directivas DNS 
Después de crean los ámbitos de la zona, puedes crear directivas DNS que distribuyen las consultas entrantes a través de estos ámbitos para que se produce lo siguiente.

1. De 6 P.M. al 9 P.M. diario, 30% de los clientes reciben la dirección IP del servidor Web del centro de datos de Azure en la respuesta DNS, mientras que 70% de los clientes reciben la dirección IP del servidor Web Seattle local.
2. En cualquier otro momento, todos los clientes reciben la dirección IP del servidor Web Seattle local.

La hora del día debe expresarse en la hora local del servidor DNS.

Puedes usar el siguiente comando de ejemplo para crear la directiva DNS.

```
Add-DnsServerQueryResolutionPolicy -Name "Contoso6To9Policy" -Action ALLOW –-ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00” -ZoneName "contosogiftservices.com" –ProcessingOrder 1
```

Para obtener más información, consulta [agregar DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx).  
  
Ahora, el servidor DNS está configurado con las directivas necesarias de DNS para redirigir el tráfico en el servidor Web de Azure según la hora del día. 

Ten en cuenta la expresión:

`
 -ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00” 
`

Esta expresión configura el servidor DNS con una combinación de zonaámbito y grosor que indica al servidor DNS para enviar la dirección IP del servidor Web de Seattle setenta por ciento de las veces, al enviar la dirección IP del servidor Web de Azure treinta por ciento del tiempo.

Puede crear miles de directivas DNS según el tráfico de requisitos de administración y todas las nuevas directivas se aplican de forma dinámica - sin reiniciar el servidor DNS, en las consultas entrantes.
