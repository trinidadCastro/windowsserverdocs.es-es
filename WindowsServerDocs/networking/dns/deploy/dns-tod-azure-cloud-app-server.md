---
title: Respuestas DNS basadas en la hora del día con un servidor de aplicaciones en la nube de Azure
description: En este tema forma parte de las DNS directiva escenario guía para Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 4846b548-8fbc-4a7f-af13-09e834acdec0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 68f30973ef58b64006181990425e6ca84c39c059
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812038"
---
# <a name="dns-responses-based-on-time-of-day-with-an-azure-cloud-app-server"></a>Respuestas DNS basadas en la hora del día con un servidor de aplicaciones en la nube de Azure

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para aprender a distribuir el tráfico de aplicación entre diferentes instancias distribuidas geográficamente de una aplicación mediante el uso de las directivas DNS que se basan en la hora del día. 

Este escenario es útil en situaciones donde desea dirigir el tráfico de una zona horaria a servidores de aplicaciones alternativo, como los servidores Web que se hospedan en Microsoft Azure, que se encuentran en otra zona horaria. Esto le permite equilibrar la carga entre instancias de la aplicación durante los períodos cuando los servidores principales están sobrecargados con tráfico de períodos de tiempo. 

> [!NOTE]
> Para obtener información sobre cómo utilizar Directiva DNS para las respuestas DNS inteligentes sin usar Azure, consulte [usar Directiva de DNS para las respuestas de DNS inteligentes basados en la hora del día](Scenario--Use-DNS-Policy-for-Intelligent-DNS-Responses-Based-on-the-Time-of-Day.md). 

## <a name="example-of-intelligent-dns-responses-based-on-the-time-of-day-with-azure-cloud-app-server"></a>Ejemplo de las respuestas DNS inteligentes basadas en la hora del día con el servidor de aplicaciones de nube de Azure

La siguiente es un ejemplo de cómo puede usar la directiva DNS para equilibrar la aplicación tráfico según la hora del día.

Este ejemplo usa una compañía ficticia, Contoso regalo servicios, que proporciona soluciones de regalos en línea en todo el mundo a través de su sitio Web, contosogiftservices.com. 

El sitio web de contosogiftservices.com se hospeda en un único centro de datos local en Seattle (con la dirección IP pública 192.68.30.2). 

El servidor DNS también se encuentra en el centro de datos local. 

Con un aumento reciente en los negocios, contosogiftservices.com tiene un mayor número de visitantes diariamente y algunos clientes han informado de problemas de disponibilidad del servicio. 

Servicios de regalos de Contoso realiza un análisis de sitio y descubre que la cada noche entre las 6 P.M. y 9 p. M. hora local, hay un aumento en el tráfico al servidor Web de Seattle. El servidor Web no se puede escalar para controlar el incremento del tráfico a estas horas punta, lo que provocará denegación de servicio a los clientes. 

Para asegurarse de que los clientes de contosogiftservices.com obtengan una capacidad de respuesta desde el sitio Web, servicios de regalos de Contoso decide que durante estas horas alquila una máquina virtual \(VM\) en Microsoft Azure para hospedar una copia de su servidor Web .  

Servicios de regalos de Contoso se obtiene una dirección IP pública de Azure para la máquina virtual (192.68.31.44) y desarrolla la automatización para implementar el servidor Web cada día en Azure entre 5-10 PM, lo que permite un período de contingencia de una hora.

> [!NOTE]
> Para obtener más información acerca de las máquinas virtuales de Azure, consulte [documentación sobre máquinas virtuales](https://azure.microsoft.com/documentation/services/virtual-machines/) 

Los servidores DNS se configuran con los ámbitos de la zona y las directivas DNS para que entre 5 a 9 P.M. todos los días, 30% de las consultas se envían a la instancia del servidor Web que se ejecuta en Azure.

La siguiente ilustración muestra este escenario.

![Directiva de DNS para el tiempo de respuestas de día](../../media/DNS-Policy-Tod2/dns_policy_tod2.jpg)  

## <a name="how-intelligent-dns-responses-based-on-time-of-day-with-azure-app-server-works"></a>Cómo las respuestas DNS inteligentes según la hora del día con Azure funciona del servidor de aplicaciones
 
En este artículo se muestra cómo configurar el servidor DNS para responder a las consultas DNS con dos aplicaciones diferentes direcciones IP - un servidor web está en Seattle y el otro está en un centro de datos de Azure.

Después de la configuración de una nueva directiva DNS que se basa en las horas punta de 18: 00 a 21: 00 en Seattle, el servidor DNS envía setenta por ciento de las respuestas DNS a los clientes que contiene la dirección IP del servidor Web de Seattle y treinta por ciento de las respuestas DNS a clien nts que contiene la dirección IP del servidor Web de Azure, con lo que dirigir el tráfico de cliente al nuevo servidor Web de Azure e impide que se sobrecargue el servidor Web de Seattle. 

En todas las demás horas del día, tiene lugar el procesamiento de las consultas normales y las respuestas se envían desde el ámbito de zona predeterminada que contiene un registro para el servidor web en el centro de datos local. 

El valor de TTL de 10 minutos en el registro de Azure garantiza que ha expirado el registro de la caché LDNS antes de quita la máquina virtual de Azure. Una de las ventajas de este ajuste de tamaño es que puede mantener sus DNS datos locales y mantener el escalado en Azure según exija la demanda.

## <a name="how-to-configure-dns-policy-for-intelligent-dns-responses-based-on-time-of-day-with-azure-app-server"></a>Cómo configurar la directiva de DNS para las respuestas DNS inteligentes según la hora del día con el servidor de aplicaciones de Azure

Para configurar la directiva DNS para las respuestas de consulta basado en el tiempo de equilibrio de carga de aplicación de día, debe realizar los pasos siguientes.

- [Creación de los ámbitos de zona](#create-the-zone-scopes)
- [Agregar registros a los ámbitos de zona](#add-records-to-the-zone-scopes)
- [Cree las directivas DNS](#create-the-dns-policies)

> [!NOTE]
> Debe realizar estos pasos en el servidor DNS que sea autoritativo para la zona que desea configurar. Pertenencia al grupo DnsAdmins, o equivalente, es necesario para realizar los procedimientos siguientes. 

Las secciones siguientes proporcionan instrucciones de configuración detallada.

> [!IMPORTANT]
> Las secciones siguientes incluyen ejemplos de comandos de Windows PowerShell que contienen valores de ejemplo para muchos parámetros. Asegúrese de sustituir los valores de ejemplo de estos comandos con los valores adecuados para su implementación antes de ejecutar estos comandos. 


### <a name="create-the-zone-scopes"></a>Creación de los ámbitos de zona

Un ámbito de la zona es una instancia única de la zona. Una zona DNS puede tener varios ámbitos de zona, con cada ámbito de la zona que contiene su propio conjunto de registros DNS. El mismo registro puede estar presente en varios ámbitos, con direcciones IP diferentes o las mismas direcciones IP. 

> [!NOTE]
> De forma predeterminada, un ámbito de la zona existe en las zonas DNS. Este ámbito de la zona tiene el mismo nombre que la zona y las operaciones de DNS heredadas funcionan en este ámbito. 

Puede usar el siguiente comando de ejemplo para crear un ámbito de la zona para hospedar los registros de Azure.

```
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AzureZoneScope"
```

Para obtener más información, consulte [agregar DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

### <a name="add-records-to-the-zone-scopes"></a>Agregar registros a los ámbitos de zona
El siguiente paso es agregar los registros que representa el host del servidor Web en los ámbitos de la zona. 

En AzureZoneScope, se agrega el registro www.contosogiftservices.com con la dirección IP 192.68.31.44, que se encuentra en la nube pública de Azure. 

De forma similar, en el ámbito de la zona predeterminada \(contosogiftservices.com\), un registro \(www.contosogiftservices.com\) se agrega con la dirección IP 192.68.30.2 del servidor Web que se ejecutan en Seattle local Centro de datos.

En el segundo cmdlet, no se incluye el parámetro – zonaámbito. Por este motivo, los registros se agregan en el valor predeterminado zonaámbito. 

Además, el TTL del registro para las máquinas virtuales de Azure se mantiene en 600s (10 minutos) para que el LDNS no lo almacena en caché durante más tiempo - lo que interferiría con equilibrio de carga. Además, las máquinas virtuales de Azure están disponibles durante 1 hora adicional como medida de contingencia para asegurarse de que los clientes incluso con los registros almacenados en caché son capaces de resolver.

```
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.31.44" -ZoneScope "AzureZoneScope" –TimeToLive 600

Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.30.2"
```

Para obtener más información, consulte [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).  

### <a name="create-the-dns-policies"></a>Cree las directivas DNS 
Una vez creados los ámbitos de zona, puede crear las directivas DNS que distribución las consultas entrantes entre estos ámbitos, por lo que ocurre lo siguiente.

1. De 18: 00 a 9 P.M. diaria, 30% de los clientes recibir la dirección IP del servidor Web en el centro de datos de Azure en la respuesta DNS, mientras que el 70% de los clientes reciben la dirección IP del servidor Web Seattle en el entorno local.
2. En cualquier otro momento, todos los clientes reciben la dirección IP del servidor Web Seattle en el entorno local.

La hora del día debe expresarse en hora local del servidor DNS.

Puede usar el siguiente comando de ejemplo para crear la directiva DNS.

```
Add-DnsServerQueryResolutionPolicy -Name "Contoso6To9Policy" -Action ALLOW -ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00” -ZoneName "contosogiftservices.com" –ProcessingOrder 1
```

Para obtener más información, consulte [agregar DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  
  
Ahora el servidor DNS está configurado con las directivas DNS necesarias para redirigir el tráfico al servidor Web de Azure según la hora del día. 

Tenga en cuenta la expresión:

`
 -ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00” 
`

Esta expresión configura el servidor DNS con una combinación zonaámbito y peso que indica al servidor DNS para enviar la dirección IP del servidor Web de Seattle setenta por ciento del tiempo, al enviar la dirección IP del servidor Web de Azure del treinta por ciento del tiempo.

Puede crear miles de las directivas DNS según el tráfico de los requisitos de administración y todas las nuevas directivas se aplican dinámicamente - sin necesidad de reiniciar el servidor DNS, en las consultas entrantes.
