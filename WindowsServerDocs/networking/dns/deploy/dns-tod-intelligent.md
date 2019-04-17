---
title: Usar la directiva de DNS para las respuestas DNS inteligente en función de la hora del día
description: En este tema es parte de la DNS directiva escenario guía para Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 161446ff-a072-4cc4-b339-00a04857ff3a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 46821daff4a0046bf78d7f56dc7c5deabcc437e4
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-intelligent-dns-responses-based-on-the-time-of-day"></a>Usar la directiva de DNS para las respuestas DNS inteligente en función de la hora del día

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener información sobre cómo distribuir el tráfico de la aplicación en diferentes instancias geográficamente de una aplicación mediante las directivas DNS que se basan en el momento del día.  
  
Este escenario resulta útil en situaciones donde quieras dirigir el tráfico en una zona horaria a servidores de aplicaciones alternativos, como los servidores Web, que se encuentran en otra zona horaria. Esto permite la carga de tráfico de equilibrio entre instancias de la aplicación durante los períodos de cuando se sobrecargan tus servidores principales con el tráfico de períodos de tiempo.   
  
### <a name="bkmk_example1"></a>Ejemplo de respuestas DNS inteligente en función de la hora del día  
Siguiente es un ejemplo de cómo puedes usar la directiva DNS para tráfico saldo según la hora del día.  
  
Este ejemplo usa una empresa ficticia, servicios de regalo de Contoso, que proporciona soluciones en línea regalar en todo el mundo a través de su sitio Web, contosogiftservices.com.   
  
El sitio Web de contosogiftservices.com está hospedado en dos centros de datos, que se muestra en Seattle (Estados Unidos) y otro en Dublin (Europa). Los servidores DNS están configurados para el envío de ubicación geográfica respuestas cuenta mediante la directiva de DNS. Con un aumento reciente de empresa, contosogiftservices.com tiene un mayor número de visitantes cada día y algunos de los clientes han informado de problemas de disponibilidad de servicio.  
  
Servicios de regalos de Contoso realiza un análisis de sitio y descubre que la cada noche entre 6 P.M. y 9 P.M. hora local, hay un aumento en el tráfico a los servidores Web. Los servidores Web no se pueden escalar para controlar el incremento del tráfico a estas horas pico, lo que provocaría una denegación de servicio a los clientes. La sobrecarga de tráfico de horas pico mismo sucede en los centros de datos europeas y estadounidense. En otras ocasiones del día, los servidores controlan los volúmenes de tráfico que se encuentran por debajo de su capacidad máxima.  
  
Para garantizar que los clientes contosogiftservices.com obtendrán una experiencia con capacidad de respuesta desde el sitio Web, servicios de regalos de Contoso quiere redirigir algunas tráfico Dublin a los servidores de aplicaciones de Seattle entre 6 P.M. y 9 P.M. en Dublin; y quieren redirigir algunas tráfico Seattle a los servidores de aplicaciones de Dublin entre 6 P.M. y 9 P.M. en Seattle.  
  
La ilustración siguiente muestra este escenario.  
  
![Hora de ejemplo de directiva de DNS de día](../../media/DNS-Policy-Tod1/dns_policy_tod1.jpg)  
  
### <a name="bkmk_works1"></a>Las respuestas de DNS inteligentes en función de la hora del día funciona  
  
Cuando el servidor DNS está configurado con un tiempo de la directiva DNS de día, entre 6 P.M. y 9 P.M. en cada ubicación geográfica, el servidor DNS hace lo siguiente.  
  
- Responde a las consultas de cuatro primeros recibe con la dirección IP del servidor Web del centro de datos local.  
- Responde a la consulta quinta recibe con la dirección IP del servidor Web del centro de datos remoto.   
  
Este comportamiento basada en directivas descarga veinte por ciento de carga de tráfico del servidor Web local en el servidor Web remoto, aceleración de la carga del servidor de aplicaciones locales y mejorar el rendimiento del sitio para que los clientes.  
  
Durante las horas, los servidores DNS realizan administración ubicaciones geográfica normal en función del tráfico. Además, los clientes DNS que envían consultas desde ubicaciones que no sean los Estados Unidos o en Europa, la carga del servidor DNS equilibra el tráfico a través de los centros de datos de Seattle y Dublín.  
  
Cuando hay varias directivas DNS configuradas en DNS, son un conjunto de reglas ordenado y se procesan por DNS de prioridad mayor a menor prioridad. DNS usa la primera directiva que coincida con las circunstancias, como la hora del día. Por este motivo, las directivas más específicas deben tener prioridad más alta. Si creas el tiempo de directivas de día y dar prioridad alta en la lista de directivas, DNS procesa y usa estas directivas primero si coinciden con los parámetros de la consulta del cliente DNS y los criterios definidos en la directiva. Si no coinciden, DNS se mueve hacia abajo de la lista de directivas para procesar las directivas de forma predeterminada hasta que encuentra a una coincidencia.  
  
Para obtener más información sobre los tipos de directivas y los criterios, consulta [introducción de las directivas de DNS](../../dns/deploy/DNS-Policies-Overview.md).  
  
### <a name="bkmk_how1"></a>Cómo configurar la directiva DNS para las respuestas DNS inteligente en función de la hora del día  
Para configurar la directiva DNS para las respuestas de consulta de tiempo de equilibrio de carga de aplicación de día según, debes realizar los siguientes pasos.  
  
- [Crear las subredes del cliente DNS](#bkmk_subnets)  
- [Crear los ámbitos zona](#bkmk_zscopes)  
- [Agregar registros a los ámbitos zona](#bkmk_records)  
- [Crear las directivas DNS](#bkmk_policies)  
  
>[!NOTE]
>Debes realizar estos pasos en el servidor DNS que está autorizado para la zona que deseas configurar. Pertenencia a **grupo Administradores DNS**, o equivalente, es necesario para realizar los siguientes procedimientos.  
  
Las siguientes secciones proporcionan instrucciones de configuración detallada.  
  
>[!IMPORTANT]
>Las secciones siguientes incluyen ejemplos de comandos de Windows PowerShell que contienen los valores de ejemplo para el número de parámetros. Asegúrate de que los valores de ejemplo en estos comandos, se reemplaza con valores que son adecuados para su implementación antes de ejecutar estos comandos.  
  
#### <a name="bkmk_subnets"></a>Crear las subredes del cliente DNS  
El primer paso es identificar las subredes o el espacio de direcciones IP de las regiones para el que quieras redireccionar tráfico. Por ejemplo, si quieres redirigir el tráfico para los Estados Unidos y Europa, deberás identificar las subredes o espacios de direcciones IP de estas regiones.  
  
Puedes obtener esta información de mapas geográficos IP. En función de estas distribuciones geográfica IP, debes crear las "subredes del cliente DNS". Una subred del cliente DNS es una agrupación lógica de subredes IPv4 o IPv6 desde el que se envían consultas a un servidor DNS.  
  
Puedes usar los siguientes comandos de Windows PowerShell para crear subredes del cliente DNS.  
  
```  
Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet "192.0.0.0/24, 182.0.0.0/24"  
  
Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "141.1.0.0/24, 151.1.0.0/24"  
  
```  
Para obtener más información, consulta [agregar DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).  
  
#### <a name="bkmk_zscopes"></a>Crear los ámbitos zona  
Después de configuran las subredes del cliente, se debe dividir la zona cuyo tráfico que desee redirigir en dos ámbitos zona diferente, un ámbito para cada una de las subredes del cliente DNS que hayas configurado.  
  
Por ejemplo, si quieres redirigir el tráfico para el nombre DNS www.contosogiftservices.com, debes crear dos ámbitos zona diferente en la zona contosogiftservices.com, uno para los Estados Unidos y otro para Europa.  
  
Un ámbito de la zona es una instancia única de la zona. Una zona DNS puede tener varios ámbitos zona, con cada ámbito de la zona que contiene su propio conjunto de registros de DNS. El mismo registro puede encontrarse en varios ámbitos, con diferentes direcciones IP o las mismas direcciones IP.  
  
>[!NOTE]
>De manera predeterminada, existe un ámbito de la zona en las zonas DNS. El ámbito de esta zona tiene el mismo nombre que la zona y las operaciones de DNS heredadas funcionan en este ámbito.  
  
Puedes usar los siguientes comandos de Windows PowerShell para crear ámbitos zona.  
  
```  
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "SeattleZoneScope"  
  
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"  
  
```  
Para obtener más información, consulta [agregar DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).  
  
#### <a name="bkmk_records"></a>Agregar registros a los ámbitos zona  
Ahora debes agregar los registros que representa el host del servidor web en los ámbitos dos zona.  
  
Por ejemplo, en **SeattleZoneScope**, el registro **www.contosogiftservices.com** se agrega con la dirección IP 192.0.0.1, que se encuentra en un centro de datos de Seattle. De forma similar, en **DublinZoneScope**, el registro **www.contosogiftservices.com** se agrega con la dirección IP 141.1.0.3 en el centro de datos de Dublin  
  
Puedes usar los siguientes comandos de Windows PowerShell para agregar registros a los ámbitos de la zona.  
  
```  
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "SeattleZoneScope  
  
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.3" -ZoneScope "DublinZoneScope"  
  
```  
El parámetro zonaámbito no está incluido Cuando agregas un registro en el ámbito de forma predeterminada. Esto es lo mismo que agregar los registros en una zona DNS estándar.  
  
Para obtener más información, consulta [agregar DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).  
  
#### <a name="bkmk_policies"></a>Crear las directivas DNS  
Después de haber creado las subredes, las particiones (ámbitos zona) y se han agregado registros, debes crear directivas que conectan las subredes y las particiones, para que cuando se trata de una consulta de un origen en una de las subredes del cliente DNS, se devuelve la respuesta de consulta desde el ámbito correcto de la zona. No hay directivas son necesarias para asignar el ámbito de la zona de forma predeterminada.  
  
Después de configurar estas directivas DNS, el comportamiento del servidor DNS es la siguiente:  
  
1. Los clientes de DNS Europeo recibir la dirección IP del servidor Web del centro de datos de Dublin en respuesta a sus consultas DNS.  
2. Los clientes de DNS americano recibir la dirección IP del servidor Web del centro de datos de Seattle en su respuesta de la consulta DNS.  
3. Entre 6 P.M. y 9 P.M. en Dublín, 20 por ciento de las consultas de los clientes europeos recibir la dirección IP del servidor Web del centro de datos de Seattle en su respuesta de la consulta DNS.  
4. Entre 6 P.M. y 9 P.M. en Seattle, 20 por ciento de las consultas de los clientes americanos recibir la dirección IP del servidor Web del centro de datos de Dublin en respuesta a sus consultas DNS.  
5. La mitad de las consultas del resto del mundo recibe la dirección IP del centro de datos de Seattle y la otra mitad recibir la dirección IP del centro de datos Dublin.  
  
  
Puedes usar los siguientes comandos de Windows PowerShell para crear una directiva DNS que se vincula las subredes del cliente DNS y los ámbitos de la zona.  
  
>[!NOTE]
>En este ejemplo, el servidor DNS está en la zona horaria GMT, por lo que las horas pico períodos de tiempo deben estar expresados en la hora de GMT equivalente.  
  
```  
Add-DnsServerQueryResolutionPolicy -Name "America6To9Policy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,4;DublinZoneScope,1" -TimeOfDay "EQ,01:00-04:00" -ZoneName "contosogiftservices.com" -ProcessingOrder 1  
  
Add-DnsServerQueryResolutionPolicy -Name "Europe6To9Policy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "SeattleZoneScope,1;DublinZoneScope,4" -TimeOfDay "EQ,17:00-20:00" -ZoneName "contosogiftservices.com" -ProcessingOrder 2  
  
Add-DnsServerQueryResolutionPolicy -Name "AmericaPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3  
  
Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 4  
  
Add-DnsServerQueryResolutionPolicy -Name "RestOfWorldPolicy" -Action ALLOW --ZoneScope "DublinZoneScope,1;SeattleZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 5  
  
```  
Para obtener más información, consulta [agregar DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  
  
Ahora, el servidor DNS está configurado con las directivas necesarias de DNS para redirigir el tráfico en función de la ubicación geográfica y la hora del día.  
  
Cuando el servidor DNS recibe las consultas de resolución de nombre, el servidor DNS evalúa los campos de la solicitud DNS en las directivas DNS configuradas. Si la dirección IP de origen en la solicitud de resolución de nombre coincide con ninguna de las directivas, se usa el ámbito de la zona asociada para responder a la consulta y el usuario se dirige al recurso que geográficamente más cercano a ellos.  
  
Puede crear miles de directivas DNS según el tráfico de requisitos de administración y todas las nuevas directivas se aplican de forma dinámica - sin reiniciar el servidor DNS, en las consultas entrantes.


