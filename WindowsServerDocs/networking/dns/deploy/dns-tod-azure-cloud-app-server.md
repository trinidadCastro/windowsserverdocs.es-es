---
title: Respuestas DNS basadas en la hora del día con un servidor de aplicaciones en la nube de Azure
description: Aprenda a usar las respuestas DNS basadas en la hora del día con un servidor de aplicaciones en la nube de Azure.
manager: brianlic
ms.topic: article
ms.assetid: 4846b548-8fbc-4a7f-af13-09e834acdec0
ms.author: lizross
author: eross-msft
ms.openlocfilehash: b90af11d4bb6f5c1f52699669a8eee4562a97d48
ms.sourcegitcommit: 029b1e19ce11160d5f988046e04a83e8ab5a60dc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/05/2021
ms.locfileid: "97904930"
---
# <a name="dns-responses-based-on-time-of-day-with-an-azure-cloud-app-server"></a>Respuestas DNS basadas en la hora del día con un servidor de aplicaciones en la nube de Azure

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para aprender a distribuir el tráfico de aplicaciones entre diferentes instancias distribuidas geográficamente de una aplicación mediante el uso de directivas DNS basadas en la hora del día.

Este escenario es útil en situaciones en las que desea dirigir el tráfico de una zona horaria a servidores de aplicaciones alternativos, como los servidores web que se hospedan en Microsoft Azure, que se encuentran en otra zona horaria. Esto le permite equilibrar la carga del tráfico entre las instancias de la aplicación durante períodos de tiempo máximos cuando los servidores principales están sobrecargados con el tráfico.

> [!NOTE]
> Para obtener información sobre cómo usar la Directiva de DNS para las respuestas de DNS inteligentes sin usar Azure, consulte [uso de la Directiva de DNS para respuestas de DNS inteligentes en función de la hora del día](./dns-tod-intelligent.md).

## <a name="example-of-intelligent-dns-responses-based-on-the-time-of-day-with-azure-cloud-app-server"></a>Ejemplo de respuestas DNS inteligentes basadas en la hora del día con el servidor de aplicaciones en la nube de Azure

A continuación se describe un ejemplo de cómo se puede usar la Directiva de DNS para equilibrar el tráfico de la aplicación en función de la hora del día.

En este ejemplo se usa una empresa ficticia, servicios de regalos de Contoso, que proporciona soluciones de regalos en línea en todo el mundo a través de su sitio web, contosogiftservices.com.

El sitio web de contosogiftservices.com se hospeda solo en un centro de centros de recursos local en Seattle (con IP pública 192.68.30.2).

El servidor DNS también se encuentra en el centro de recursos local.

Con un aumento reciente en la empresa, contosogiftservices.com tiene un número mayor de visitantes cada día, y algunos de los clientes han comunicado problemas de disponibilidad del servicio.

Contoso Gift Services realiza un análisis de sitio y detecta que cada noche entre la hora local de las 6 PM y las 9 P.M. hay un aumento del tráfico en el servidor Web de Seattle. El servidor Web no se puede escalar para controlar el aumento del tráfico en estas horas punta, lo que da lugar a la denegación de servicio a los clientes.

Para asegurarse de que los clientes de contosogiftservices.com obtienen una experiencia de respuesta en el sitio web, los servicios de regalo de Contoso deciden que, durante estas horas, alquilará una máquina virtual de máquina virtual \( \) en Microsoft Azure para hospedar una copia de su servidor Web.

Contoso Gift Services obtiene una dirección IP pública de Azure para la máquina virtual (192.68.31.44) y desarrolla la automatización para implementar el servidor web cada día en Azure entre 5-10 PM, lo que permite un período de contingencia de una hora.

> [!NOTE]
> Para más información sobre las máquinas virtuales de Azure, consulte la [documentación de virtual machines](https://azure.microsoft.com/documentation/services/virtual-machines/)

Los servidores DNS se configuran con ámbitos de zona y directivas DNS, de modo que entre 5-9 P.M. todos los días, el 30% de las consultas se envían a la instancia del servidor Web que se ejecuta en Azure.

En la ilustración siguiente se muestra este escenario.

![Directiva DNS para respuestas de hora del día](../../media/DNS-Policy-Tod2/dns_policy_tod2.jpg)

## <a name="how-intelligent-dns-responses-based-on-time-of-day-with-azure-app-server-works"></a>Funcionamiento de las respuestas de DNS inteligentes basadas en la hora del día con App de Azure Server

En este artículo se muestra cómo configurar el servidor DNS para responder a las consultas DNS con dos direcciones IP diferentes del servidor de aplicaciones: un servidor web está en Seattle y el otro está en un centro de recursos de Azure.

Después de la configuración de una nueva Directiva de DNS que se basa en las horas punta de 6 PM a 9 PM en Seattle, el servidor DNS envía 70 por ciento de las respuestas DNS a los clientes que contienen la dirección IP del servidor Web de Seattle y treinta por ciento de las respuestas DNS a los clientes que contienen la dirección IP del servidor Web de Azure. , con lo que se dirige el tráfico de cliente al nuevo servidor Web de Azure y se impide que el servidor Web de Seattle se sobrecargue.

En todas las demás horas del día, se realiza el procesamiento de consultas normal y se envían las respuestas desde el ámbito de zona predeterminado que contiene un registro para el servidor Web en el centro de recursos local.

El TTL de 10 minutos en el registro de Azure garantiza que el registro ha expirado en la memoria caché de LDNS antes de que se quite la máquina virtual de Azure. Una de las ventajas de este escalado es que puede mantener sus datos DNS en el entorno local y seguir escalando horizontalmente a Azure a medida que la demanda lo requiere.

## <a name="how-to-configure-dns-policy-for-intelligent-dns-responses-based-on-time-of-day-with-azure-app-server"></a>Configuración de la Directiva de DNS para respuestas DNS inteligentes basadas en la hora del día con App de Azure Server

Para configurar la Directiva de DNS para las respuestas de consulta basadas en el equilibrio de carga de la aplicación de la hora del día, debe realizar los pasos siguientes.

- [Crear los ámbitos de zona](#create-the-zone-scopes)
- [Agregar registros a los ámbitos de zona](#add-records-to-the-zone-scopes)
- [Crear las directivas de DNS](#create-the-dns-policies)

> [!NOTE]
> Debe realizar estos pasos en el servidor DNS que sea autoritativo para la zona que desea configurar. La pertenencia a DnsAdmins, o equivalente, es necesaria para realizar los siguientes procedimientos.

En las secciones siguientes se proporcionan instrucciones de configuración detalladas.

> [!IMPORTANT]
> En las secciones siguientes se incluyen comandos de Windows PowerShell de ejemplo que contienen valores de ejemplo para muchos parámetros. Asegúrese de reemplazar los valores de ejemplo de estos comandos por los valores adecuados para su implementación antes de ejecutar estos comandos.


### <a name="create-the-zone-scopes"></a>Crear los ámbitos de zona

Un ámbito de zona es una instancia única de la zona. Una zona DNS puede tener varios ámbitos de zona, con cada ámbito de zona que contenga su propio conjunto de registros DNS. El mismo registro puede estar presente en varios ámbitos, con direcciones IP diferentes o con las mismas direcciones IP.

> [!NOTE]
> De forma predeterminada, existe un ámbito de zona en las zonas DNS. Este ámbito de zona tiene el mismo nombre que la zona y las operaciones DNS heredadas funcionan en este ámbito.

Puede usar el siguiente comando de ejemplo para crear un ámbito de zona para hospedar los registros de Azure.

```
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AzureZoneScope"
```

Para obtener más información, consulte [Add-DnsServerZoneScope](/powershell/module/dnsserver/add-dnsserverzonescope)

### <a name="add-records-to-the-zone-scopes"></a>Agregar registros a los ámbitos de zona
El siguiente paso consiste en agregar los registros que representan el host del servidor Web en los ámbitos de zona.

En AzureZoneScope, el registro www.contosogiftservices.com se agrega con la dirección IP 192.68.31.44, que se encuentra en la nube pública de Azure.

Del mismo modo, en el ámbito de zona predeterminado \( contosogiftservices.com \) , \( se agrega un registro www.contosogiftservices.com \) con la dirección IP 192.68.30.2 del servidor Web que se ejecuta en el centro de recursos local de Seattle.

En el segundo cmdlet siguiente, el parámetro – ZoneScope no se incluye. Por este motivo, los registros se agregan en el valor predeterminado de ZoneScope.

Además, el TTL del registro para las máquinas virtuales de Azure se mantiene en 600S (10 minutos), por lo que el LDNS no lo almacena en caché durante más tiempo, lo que interferiría con el equilibrio de carga. Además, las máquinas virtuales de Azure están disponibles durante 1 hora extra como contingencia para asegurarse de que incluso los clientes con los registros almacenados en caché pueden resolver.

```
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.31.44" -ZoneScope "AzureZoneScope" –TimeToLive 600

Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.30.2"
```

Para obtener más información, consulte [Add-DnsServerResourceRecord](/powershell/module/dnsserver/add-dnsserverresourcerecord).

### <a name="create-the-dns-policies"></a>Crear las directivas de DNS
Una vez creados los ámbitos de zona, puede crear directivas DNS que distribuyan las consultas entrantes en estos ámbitos para que se produzca lo siguiente.

1. De 6 PM a 9 PM diariamente, el 30% de los clientes reciben la dirección IP del servidor Web en el centro de recursos de Azure en la respuesta DNS, mientras que el 70% de los clientes reciben la dirección IP del servidor Web local de Seattle.
2. En el resto de los casos, todos los clientes reciben la dirección IP del servidor Web local de Seattle.

La hora del día se debe expresar en la hora local del servidor DNS.

Puede usar el siguiente comando de ejemplo para crear la Directiva DNS.

```
Add-DnsServerQueryResolutionPolicy -Name "Contoso6To9Policy" -Action ALLOW -ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00” -ZoneName "contosogiftservices.com" –ProcessingOrder 1
```

Para obtener más información, consulte [Add-DnsServerQueryResolutionPolicy](/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy).

Ahora el servidor DNS está configurado con las directivas DNS necesarias para redirigir el tráfico al servidor Web de Azure en función de la hora del día.

Observe la expresión:

`
 -ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00”
`

Esta expresión configura el servidor DNS con una combinación de ZoneScope y Weight que indica al servidor DNS que envíe la dirección IP del servidor Web de Seattle 70 por ciento de tiempo, al tiempo que envía la dirección IP del servidor Web de Azure el 30 por ciento del tiempo.

Puede crear miles de directivas DNS según los requisitos de administración del tráfico y todas las directivas nuevas se aplican de forma dinámica, sin necesidad de reiniciar el servidor DNS, en las consultas entrantes.
