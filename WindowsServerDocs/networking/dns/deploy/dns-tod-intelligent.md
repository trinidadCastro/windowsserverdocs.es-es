---
title: Uso de la directiva de DNS para las respuestas DNS inteligentes basadas en la hora del día
description: Este tema forma parte de la guía del escenario de la Directiva DNS para Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: 161446ff-a072-4cc4-b339-00a04857ff3a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e497b0d73c816f0295588aa77a21c49d376c0dcf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406189"
---
# <a name="use-dns-policy-for-intelligent-dns-responses-based-on-the-time-of-day"></a>Uso de la directiva de DNS para las respuestas DNS inteligentes basadas en la hora del día

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para aprender a distribuir el tráfico de aplicaciones entre diferentes instancias distribuidas geográficamente de una aplicación mediante el uso de directivas DNS basadas en la hora del día.  
  
Este escenario es útil en situaciones en las que desea dirigir el tráfico de una zona horaria a servidores de aplicaciones alternativos, como servidores Web, que se encuentran en otra zona horaria. Esto le permite equilibrar la carga del tráfico entre las instancias de la aplicación durante períodos de tiempo máximos cuando los servidores principales están sobrecargados con el tráfico.   
  
### <a name="bkmk_example1"></a>Ejemplo de respuestas DNS inteligentes basadas en la hora del día  
A continuación se describe un ejemplo de cómo se puede usar la Directiva de DNS para equilibrar el tráfico de la aplicación en función de la hora del día.  
  
En este ejemplo se usa una empresa ficticia, servicios de regalos de Contoso, que proporciona soluciones de regalos en línea en todo el mundo a través de su sitio web, contosogiftservices.com.   
  
El sitio web de contosogiftservices.com se hospeda en dos centros de recursos, uno en Seattle (Norteamérica) y otro en Dublín (Europa). Los servidores DNS se configuran para enviar respuestas con reconocimiento de ubicación geográfica mediante la Directiva DNS. Con un aumento reciente en la empresa, contosogiftservices.com tiene un número mayor de visitantes cada día, y algunos de los clientes han comunicado problemas de disponibilidad del servicio.  
  
Contoso Gift Services realiza un análisis de sitio y detecta que cada noche entre la hora local de las 6 PM y las 9 P.M., hay un aumento en el tráfico a los servidores Web. Los servidores web no se pueden escalar para controlar el aumento del tráfico en estas horas punta, lo que da lugar a la denegación de servicio a los clientes. Se produce la misma sobrecarga de tráfico de horas punta en los centros de recursos europeos y americanos. En otras horas del día, los servidores controlan los volúmenes de tráfico que están por debajo de su capacidad máxima.  
  
Para asegurarse de que los clientes de contosogiftservices.com obtienen una experiencia de respuesta en el sitio web, los servicios de regalos de Contoso desean redirigir algún tráfico de Dublín a los servidores de aplicaciones de Seattle entre las 6 PM y las 9 PM en Dublín; y desean redirigir cierto tráfico de Seattle a los servidores de aplicaciones de Dublin entre 6 PM y 9 PM en Seattle.  
  
En la ilustración siguiente se muestra este escenario.  
  
![Ejemplo de directiva de DNS de hora del día](../../media/DNS-Policy-Tod1/dns_policy_tod1.jpg)  
  
### <a name="bkmk_works1"></a>Funcionamiento de las respuestas de DNS inteligentes basadas en la hora del día  
  
Cuando el servidor DNS está configurado con la Directiva de DNS de hora del día, entre 6 PM y 9 P.M. en cada ubicación geográfica, el servidor DNS hace lo siguiente.  
  
- Responde a las cuatro primeras consultas que recibe con la dirección IP del servidor Web en el centro de recursos local.  
- Responde a la quinta consulta que recibe con la dirección IP del servidor Web en el centro de recursos remoto.   
  
Este comportamiento basado en directivas descarga el veinte por ciento de la carga de tráfico del servidor Web local en el servidor Web remoto, lo que facilita la presión en el servidor de aplicaciones local y mejora el rendimiento del sitio para los clientes.  
  
Durante las horas de poca actividad, los servidores DNS realizan la administración de tráfico basada en ubicaciones geográficas normales. Además, los clientes DNS que envían consultas desde ubicaciones distintas de Norteamérica o Europa, el servidor DNS equilibra la carga del tráfico entre los centros de recursos de Seattle y Dublín.  
  
Cuando se configuran varias directivas DNS en DNS, son un conjunto ordenado de reglas y son procesadas por DNS de la prioridad más alta a la más baja. DNS usa la primera Directiva que coincida con las circunstancias, incluida la hora del día. Por este motivo, las directivas más específicas deben tener una prioridad más alta. Si crea directivas de hora del día y les da prioridad alta en la lista de directivas, los procesos de DNS y las usan primero si coinciden con los parámetros de la consulta de cliente DNS y los criterios definidos en la Directiva. Si no coinciden, el DNS se desplaza hacia abajo en la lista de directivas para procesar las directivas predeterminadas hasta que encuentre una coincidencia.  
  
Para obtener más información sobre los tipos de directivas y los criterios, consulte [información general sobre directivas DNS](../../dns/deploy/DNS-Policies-Overview.md).  
  
### <a name="bkmk_how1"></a>Configuración de la Directiva de DNS para respuestas de DNS inteligentes basadas en la hora del día  
Para configurar la Directiva de DNS para las respuestas de consulta basadas en el equilibrio de carga de la aplicación de la hora del día, debe realizar los pasos siguientes.  
  
- [Crear las subredes de cliente DNS](#bkmk_subnets)  
- [Crear los ámbitos de zona](#bkmk_zscopes)  
- [Agregar registros a los ámbitos de zona](#bkmk_records)  
- [Crear las directivas de DNS](#bkmk_policies)  
  
>[!NOTE]
>Debe realizar estos pasos en el servidor DNS que sea autoritativo para la zona que desea configurar. La pertenencia a **DnsAdmins**, o equivalente, es necesaria para realizar los siguientes procedimientos.  
  
En las secciones siguientes se proporcionan instrucciones de configuración detalladas.  
  
>[!IMPORTANT]
>En las secciones siguientes se incluyen comandos de Windows PowerShell de ejemplo que contienen valores de ejemplo para muchos parámetros. Asegúrese de reemplazar los valores de ejemplo de estos comandos por los valores adecuados para su implementación antes de ejecutar estos comandos.  
  
#### <a name="bkmk_subnets"></a>Crear las subredes de cliente DNS  
El primer paso consiste en identificar las subredes o el espacio de direcciones IP de las regiones para las que desea redirigir el tráfico. Por ejemplo, si desea redirigir el tráfico para los Estados Unidos y Europa, debe identificar las subredes o los espacios de direcciones IP de estas regiones.  
  
Puede obtener esta información de mapas de IP geográfica. En función de estas distribuciones de IP geográfica, debe crear las "subredes de cliente DNS". Una subred de cliente DNS es una agrupación lógica de subredes IPv4 o IPv6 desde la que se envían las consultas a un servidor DNS.  
  
Puede usar los siguientes comandos de Windows PowerShell para crear subredes de cliente DNS.  
  
```  
Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet "192.0.0.0/24, 182.0.0.0/24"  
  
Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "141.1.0.0/24, 151.1.0.0/24"  
  
```  
Para obtener más información, consulte [Add-DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).  
  
#### <a name="bkmk_zscopes"></a>Crear los ámbitos de zona  
Una vez configuradas las subredes de cliente, debe particionar la zona cuyo tráfico desea redirigir en dos ámbitos de zona diferentes, un ámbito para cada una de las subredes de cliente DNS que ha configurado.  
  
Por ejemplo, si desea redirigir el tráfico para el nombre DNS www.contosogiftservices.com, debe crear dos ámbitos de zona diferentes en la zona contosogiftservices.com, uno para los Estados Unidos y otro para Europa.  
  
Un ámbito de zona es una instancia única de la zona. Una zona DNS puede tener varios ámbitos de zona, con cada ámbito de zona que contenga su propio conjunto de registros DNS. El mismo registro puede estar presente en varios ámbitos, con direcciones IP diferentes o con las mismas direcciones IP.  
  
>[!NOTE]
>De forma predeterminada, existe un ámbito de zona en las zonas DNS. Este ámbito de zona tiene el mismo nombre que la zona y las operaciones DNS heredadas funcionan en este ámbito.  
  
Puede usar los siguientes comandos de Windows PowerShell para crear ámbitos de zona.  
  
```  
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "SeattleZoneScope"  
  
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"  
  
```  
Para obtener más información, consulte [Add-DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).  
  
#### <a name="bkmk_records"></a>Agregar registros a los ámbitos de zona  
Ahora debe agregar los registros que representan el host del servidor Web en los dos ámbitos de zona.  
  
Por ejemplo, en **SeattleZoneScope**, el registro <strong>www.contosogiftservices.com</strong> se agrega con la dirección IP 192.0.0.1, que se encuentra en un centro de centros de recursos de Seattle. Del mismo modo, en **DublinZoneScope**, el registro <strong>www.contosogiftservices.com</strong> se agrega con la dirección IP 141.1.0.3 en el centro de recursos de Dublín  
  
Puede usar los siguientes comandos de Windows PowerShell para agregar registros a los ámbitos de zona.  
  
```  
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "SeattleZoneScope  
  
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.3" -ZoneScope "DublinZoneScope"  
  
```  
El parámetro ZoneScope no se incluye al agregar un registro en el ámbito predeterminado. Esto es lo mismo que agregar registros a una zona DNS estándar.  
  
Para obtener más información, consulte [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).  
  
#### <a name="bkmk_policies"></a>Crear las directivas de DNS  
Después de crear las subredes, las particiones (ámbitos de zona) y los registros agregados, debe crear directivas que conecten las subredes y las particiones, de modo que cuando una consulta provenga de un origen en una de las subredes de cliente DNS, la respuesta de la consulta se devuelva desde el ámbito correcto de la zona. No se requieren directivas para asignar el ámbito de zona predeterminado.  
  
Después de configurar estas directivas DNS, el comportamiento del servidor DNS es el siguiente:  
  
1. Los clientes DNS europeos reciben la dirección IP del servidor Web en el centro de recursos de Dublin en su respuesta de consulta DNS.  
2. Los clientes DNS americanos reciben la dirección IP del servidor Web en el centro de recursos de Seattle en su respuesta de consulta DNS.  
3. Entre 6 PM y 9 P.M. en Dublín, el 20% de las consultas de los clientes europeos reciben la dirección IP del servidor Web en el centro de recursos de Seattle en su respuesta de consulta DNS.  
4. Entre 6 PM y 9 P.M. en Seattle, el 20% de las consultas de los clientes americanos reciben la dirección IP del servidor Web en el centro de usuarios de Dublin en su respuesta de consulta DNS.  
5. La mitad de las consultas del resto del mundo reciben la dirección IP del centro de recursos de Seattle y la otra mitad reciben la dirección IP del centro de recursos de Dublín.  
  
  
Puede usar los siguientes comandos de Windows PowerShell para crear una directiva DNS que vincule las subredes de cliente DNS y los ámbitos de zona.  
  
>[!NOTE]
>En este ejemplo, el servidor DNS se encuentra en la zona horaria GMT, por lo que los períodos de tiempo máximo se deben expresar en la hora GMT equivalente.  
  
```  
Add-DnsServerQueryResolutionPolicy -Name "America6To9Policy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,4;DublinZoneScope,1" -TimeOfDay "EQ,01:00-04:00" -ZoneName "contosogiftservices.com" -ProcessingOrder 1  
  
Add-DnsServerQueryResolutionPolicy -Name "Europe6To9Policy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "SeattleZoneScope,1;DublinZoneScope,4" -TimeOfDay "EQ,17:00-20:00" -ZoneName "contosogiftservices.com" -ProcessingOrder 2  
  
Add-DnsServerQueryResolutionPolicy -Name "AmericaPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3  
  
Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 4  
  
Add-DnsServerQueryResolutionPolicy -Name "RestOfWorldPolicy" -Action ALLOW --ZoneScope "DublinZoneScope,1;SeattleZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 5  
  
```  
Para obtener más información, consulte [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  
  
Ahora el servidor DNS está configurado con las directivas DNS necesarias para redirigir el tráfico en función de la ubicación geográfica y la hora del día.  
  
Cuando el servidor DNS recibe consultas de resolución de nombres, el servidor DNS evalúa los campos de la solicitud DNS con las directivas DNS configuradas. Si la dirección IP de origen de la solicitud de resolución de nombres coincide con alguna de las directivas, el ámbito de la zona asociada se usa para responder a la consulta y se dirige al usuario al recurso que está geográficamente más cercano.  
  
Puede crear miles de directivas DNS según los requisitos de administración del tráfico y todas las directivas nuevas se aplican de forma dinámica, sin necesidad de reiniciar el servidor DNS, en las consultas entrantes.


