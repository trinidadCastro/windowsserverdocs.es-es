---
title: Uso de la directiva de DNS para las respuestas DNS inteligentes basadas en la hora del día
description: En este tema forma parte de las DNS directiva escenario guía para Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 161446ff-a072-4cc4-b339-00a04857ff3a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c36475dacb8664352f4ab270878357118d281c60
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446419"
---
# <a name="use-dns-policy-for-intelligent-dns-responses-based-on-the-time-of-day"></a>Uso de la directiva de DNS para las respuestas DNS inteligentes basadas en la hora del día

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para aprender a distribuir el tráfico de aplicación entre diferentes instancias distribuidas geográficamente de una aplicación mediante el uso de las directivas DNS que se basan en la hora del día.  
  
Este escenario es útil en situaciones donde desea dirigir el tráfico de una zona horaria a servidores de aplicaciones alternativo, como servidores Web, que se encuentran en otra zona horaria. Esto le permite equilibrar la carga entre instancias de la aplicación durante los períodos cuando los servidores principales están sobrecargados con tráfico de períodos de tiempo.   
  
### <a name="bkmk_example1"></a>Ejemplo de las respuestas DNS inteligentes basadas en la hora del día  
La siguiente es un ejemplo de cómo puede usar la directiva DNS para equilibrar la aplicación tráfico según la hora del día.  
  
Este ejemplo usa una compañía ficticia, Contoso regalo servicios, que proporciona soluciones de regalos en línea en todo el mundo a través de su sitio Web, contosogiftservices.com.   
  
El sitio Web de contosogiftservices.com se hospeda en dos centros de datos, en Seattle (Estados Unidos) y otra en Dublín (Europa). Los servidores DNS están configurados para el envío de ubicación geográfica compatible con respuestas mediante la directiva de DNS. Con un aumento reciente en los negocios, contosogiftservices.com tiene un mayor número de visitantes diariamente y algunos clientes han informado de problemas de disponibilidad del servicio.  
  
Servicios de regalos de Contoso realiza un análisis de sitio y descubre que la cada noche entre las 6 P.M. y 9 p. M. hora local, hay un aumento en el tráfico a los servidores Web. Los servidores Web no se pueden escalar para controlar el incremento del tráfico a estas horas punta, lo que provocará denegación de servicio a los clientes. Se produce la misma sobrecarga de tráfico de horas pico en los centros de datos europeos y estadounidenses. En otros momentos del día, los servidores de administrar volúmenes de tráfico que están por debajo de su capacidad máxima.  
  
Para asegurarse de que los clientes de contosogiftservices.com obtengan una capacidad de respuesta desde el sitio Web, servicios de regalos de Contoso desea redirigir el tráfico de algunos Dublín a los servidores de aplicaciones de Seattle entre las 6 P.M. y 9 P.M. en Dublín; y desean redirigir parte del tráfico de Seattle a los servidores de aplicaciones de Dublín entre las 6 P.M. y 9 P.M. en Seattle.  
  
La siguiente ilustración muestra este escenario.  
  
![Tiempo de ejemplo de directiva de DNS de día](../../media/DNS-Policy-Tod1/dns_policy_tod1.jpg)  
  
### <a name="bkmk_works1"></a>Cómo Intelligent respuestas DNS según la hora del día funciona  
  
Cuando el servidor DNS está configurado con un tiempo de la directiva de DNS del día, entre las 6 P.M. y 9 P.M. en cada ubicación geográfica, el servidor DNS hace lo siguiente.  
  
- Responde a las cuatro primeras consultas que recibe con la dirección IP del servidor Web en el centro de datos local.  
- Responde a la consulta quinta que recibe con la dirección IP del servidor Web en el centro de datos remoto.   
  
Este comportamiento basada en directivas descarga el veinte por ciento de carga de tráfico del servidor Web local para el servidor Web remoto, facilitando la presión sobre el servidor de aplicaciones locales y mejorar el rendimiento del sitio para los clientes.  
  
Durante las horas, los servidores DNS realizan administración ubicaciones geográficas normal en función del tráfico. Además, los clientes DNS que envían consultas desde ubicaciones distintas de América del Norte o Europa, la carga del servidor DNS equilibra el tráfico entre los centros de datos de Seattle y Dublín.  
  
Cuando se configuran varias directivas DNS en DNS, son un conjunto ordenado de reglas y se procesan por DNS de prioridad más alta a prioridad más baja. DNS usa la primera directiva que coincida con las circunstancias, incluida la hora del día. Por este motivo, las directivas más específicas deben tener prioridad más alta. Si crea el tiempo de las directivas de día y darles prioridad alta en la lista de directivas, DNS procesa y usa estas directivas en primer lugar si coinciden con los parámetros de la consulta del cliente DNS y los criterios definidos en la directiva. Si no coinciden, DNS se mueve hacia abajo de la lista de directivas para procesar las directivas predeterminadas hasta que encuentra a una coincidencia.  
  
Para obtener más información sobre los tipos de directivas y los criterios, vea [Introducción a las directivas DNS](../../dns/deploy/DNS-Policies-Overview.md).  
  
### <a name="bkmk_how1"></a>Cómo configurar la directiva de DNS para las respuestas DNS inteligentes según la hora del día  
Para configurar la directiva DNS para las respuestas de consulta basado en el tiempo de equilibrio de carga de aplicación de día, debe realizar los pasos siguientes.  
  
- [Crear las subredes del cliente DNS](#bkmk_subnets)  
- [Creación de los ámbitos de zona](#bkmk_zscopes)  
- [Agregar registros a los ámbitos de zona](#bkmk_records)  
- [Cree las directivas DNS](#bkmk_policies)  
  
>[!NOTE]
>Debe realizar estos pasos en el servidor DNS que sea autoritativo para la zona que desea configurar. Pertenencia a **DnsAdmins**, o equivalente, es necesario para realizar los procedimientos siguientes.  
  
Las secciones siguientes proporcionan instrucciones de configuración detallada.  
  
>[!IMPORTANT]
>Las secciones siguientes incluyen ejemplos de comandos de Windows PowerShell que contienen valores de ejemplo para muchos parámetros. Asegúrese de sustituir los valores de ejemplo de estos comandos con los valores adecuados para su implementación antes de ejecutar estos comandos.  
  
#### <a name="bkmk_subnets"></a>Crear las subredes del cliente DNS  
El primer paso es identificar las subredes o espacio de direcciones IP de las regiones para la que desea redirigir el tráfico. Por ejemplo, si desea redirigir el tráfico de los Estados Unidos y Europa, debe identificar las subredes o espacios de direcciones IP de estas regiones.  
  
Puede obtener esta información de mapas de Geo-IP. En función de estas distribuciones de Geo-IP, debe crear las "subredes del cliente DNS". Una subred de cliente DNS es una agrupación lógica de subredes IPv4 o IPv6 desde el que las consultas se envían a un servidor DNS.  
  
Puede usar los siguientes comandos de Windows PowerShell para crear subredes de cliente DNS.  
  
```  
Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet "192.0.0.0/24, 182.0.0.0/24"  
  
Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "141.1.0.0/24, 151.1.0.0/24"  
  
```  
Para obtener más información, consulte [agregar DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).  
  
#### <a name="bkmk_zscopes"></a>Creación de los ámbitos de zona  
Después de configuran las subredes del cliente, debe dividir la zona de cuyo tráfico desea redirigir en dos ámbitos de zona diferente, un ámbito para cada una de las subredes del cliente DNS que haya configurado.  
  
Por ejemplo, si desea redirigir el tráfico de la www.contosogiftservices.com nombre DNS, debe crear dos ámbitos de zona diferente en la zona contosogiftservices.com, uno para Estados Unidos y otro para Europa.  
  
Un ámbito de la zona es una instancia única de la zona. Una zona DNS puede tener varios ámbitos de zona, con cada ámbito de la zona que contiene su propio conjunto de registros DNS. El mismo registro puede estar presente en varios ámbitos, con direcciones IP diferentes o las mismas direcciones IP.  
  
>[!NOTE]
>De forma predeterminada, un ámbito de la zona existe en las zonas DNS. Este ámbito de la zona tiene el mismo nombre que la zona y las operaciones de DNS heredadas funcionan en este ámbito.  
  
Puede usar los siguientes comandos de Windows PowerShell para crear ámbitos de la zona.  
  
```  
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "SeattleZoneScope"  
  
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"  
  
```  
Para obtener más información, consulte [agregar DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).  
  
#### <a name="bkmk_records"></a>Agregar registros a los ámbitos de zona  
Ahora debe agregar los registros que representa el host del servidor web en los ámbitos de la zona de dos.  
  
Por ejemplo, en **SeattleZoneScope**, el registro <strong>www.contosogiftservices.com</strong> se agrega con dirección IP 192.0.0.1, que se encuentra en un centro de datos de Seattle. De forma similar, en **DublinZoneScope**, el registro <strong>www.contosogiftservices.com</strong> se agrega con la dirección IP 141.1.0.3 en el centro de datos Dublín  
  
Puede usar los siguientes comandos de Windows PowerShell para agregar registros a los ámbitos de la zona.  
  
```  
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "SeattleZoneScope  
  
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.3" -ZoneScope "DublinZoneScope"  
  
```  
El parámetro zonaámbito no se incluye cuando se agrega un registro en el ámbito predeterminado. Esto es igual que agregar registros a una zona DNS estándar.  
  
Para obtener más información, consulte [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).  
  
#### <a name="bkmk_policies"></a>Cree las directivas DNS  
Después de haber creado las subredes, las particiones (ámbitos de zona) y se han agregado registros, debe crear directivas que se conectan a las subredes y las particiones, por lo que cuando una consulta procede de un origen en una de las subredes de cliente DNS, se devuelve la respuesta de consulta desde el ámbito correcto de la zona. No hay directivas son necesarios para la asignación de ámbito de la zona predeterminada.  
  
Después de configurar estas directivas DNS, el comportamiento del servidor DNS es como sigue:  
  
1. Los clientes europeos DNS reciben la dirección IP del servidor Web en el centro de datos Dublín su respuesta a consultas DNS.  
2. Los clientes DNS American reciben la dirección IP del servidor Web en el centro de datos de Seattle en su respuesta a consultas DNS.  
3. Entre las 6 P.M. y 9 P.M. en Dublín, 20% de las consultas de los clientes europeos recibe la dirección IP del servidor Web en el centro de datos de Seattle en su respuesta a consultas DNS.  
4. Entre las 6 P.M. y 9 P.M. en Seattle, 20% de las consultas de los clientes American recibir la dirección IP del servidor Web en el centro de datos Dublín su respuesta a consultas DNS.  
5. La mitad de las consultas del resto del mundo recibir la dirección IP del centro de datos de Seattle y la otra mitad recibir la dirección IP del centro de datos Dublín.  
  
  
Puede usar los siguientes comandos de Windows PowerShell para crear una directiva DNS que vincula las subredes del cliente DNS y los ámbitos de la zona.  
  
>[!NOTE]
>En este ejemplo, el servidor DNS está en la zona horaria GMT, por lo que la hora punta períodos de tiempo se deben expresar en la hora GMT equivalente.  
  
```  
Add-DnsServerQueryResolutionPolicy -Name "America6To9Policy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,4;DublinZoneScope,1" -TimeOfDay "EQ,01:00-04:00" -ZoneName "contosogiftservices.com" -ProcessingOrder 1  
  
Add-DnsServerQueryResolutionPolicy -Name "Europe6To9Policy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "SeattleZoneScope,1;DublinZoneScope,4" -TimeOfDay "EQ,17:00-20:00" -ZoneName "contosogiftservices.com" -ProcessingOrder 2  
  
Add-DnsServerQueryResolutionPolicy -Name "AmericaPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3  
  
Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 4  
  
Add-DnsServerQueryResolutionPolicy -Name "RestOfWorldPolicy" -Action ALLOW --ZoneScope "DublinZoneScope,1;SeattleZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 5  
  
```  
Para obtener más información, consulte [agregar DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  
  
Ahora el servidor DNS está configurado con las directivas DNS necesarias para redirigir el tráfico según la ubicación geográfica y la hora del día.  
  
Cuando el servidor DNS recibe consultas de resolución de nombres, el servidor DNS se evalúa como los campos de la solicitud DNS con las directivas DNS configuradas. Si la dirección IP de origen en la solicitud de resolución de nombre coincide con cualquiera de las directivas, el ámbito de la zona asociada se utiliza para responder a la consulta y se dirige al usuario al recurso que está geográficamente más cercano a ellos.  
  
Puede crear miles de las directivas DNS según el tráfico de los requisitos de administración y todas las nuevas directivas se aplican dinámicamente - sin necesidad de reiniciar el servidor DNS, en las consultas entrantes.


