---
title: Usar la directiva DNS para la ubicación geográfica en función de la administración de tráfico con servidores principales
description: En este tema es parte de la DNS directiva escenario guía para Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: ef9828f8-c0ad-431d-ae52-e2065532e68f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bd72e13cd3d2d7f3ca886afcdcf97e824ef227f5
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-geo-location-based-traffic-management-with-primary-servers"></a>Usar la directiva DNS para la ubicación geográfica en función de la administración de tráfico con servidores principales

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para aprender a configurar la directiva de DNS para permitir que los servidores DNS principales responder a las consultas del cliente DNS según la ubicación geográfica del cliente y el recurso al que intenta conectarse, el cliente proporciona al cliente con la dirección IP del recurso más cercana.  
  
>[!IMPORTANT]  
>Este escenario muestra cómo implementar la directiva DNS para la administración de tráfico en función de ubicación geográfica cuando se usan los servidores DNS principales solo. También puede realizar administración ubicación geográfica en función del tráfico cuando tiene servidores DNS principales y secundarios. Si tienes una implementación principal secundario, primero completar los pasos de este tema y, a continuación, realiza los pasos que se proporcionan en el tema [usar la directiva de DNS para la administración de tráfico en función de ubicación geográfica con las implementaciones de principal secundario](primary-secondary-geo-location.md).

Con las nuevas directivas DNS, puedes crear una directiva DNS que permite que el servidor DNS responder a la dirección IP de un servidor Web pide una consulta del cliente. Instancias del servidor Web pueden encontrarse en los centros de datos distintos en distintas ubicaciones físicas. DNS puedes evaluar el cliente y ubicaciones del servidor Web, y luego responder a la solicitud de cliente proporcionando el cliente con un servidor Web dirección IP de un servidor Web que se encuentra físicamente cerca al cliente.  

Puedes usar los siguientes parámetros de la directiva DNS para controlar las respuestas del servidor DNS a las consultas de los clientes DNS. 

- **Cliente subred**. Nombre de una subred cliente predefinidos. Se utiliza para comprobar la subred desde el que se envió la consulta.
- **Protocolo de transporte**. Transporte Protocolo usado en la consulta. Entradas posibles son **UDP** y **TCP**.
- **Protocolo de Internet**. Protocolo de red que se usa en la consulta. Entradas posibles son **IPv4** y **IPv6**.
- **Dirección IP de la interfaz del servidor**. Dirección IP de la interfaz de red del servidor DNS que recibe la solicitud DNS.
- **FQDN**. El dominio nombre completo (FQDN) del registro de la consulta, con la posibilidad de utilizar un carácter comodín.
- **Tipo de consulta**. Tipo de registro que se está consultando (A, SRV, TXT, etcetera).
- **Hora del día**. Hora del día que se recibe la consulta. 
  
Los siguientes criterios se pueden combinar con un operador lógico (o) formular las expresiones de directiva. Cuando estas expresiones coinciden, se espera a que las directivas para realizar una de las siguientes acciones.
 
- **Omitir**. El servidor DNS silenciosamente disminuye la consulta.          
- **Denegar**. El servidor DNS responde esa consulta con una respuesta de error.          
- **Permitir**. El servidor DNS responde con tráfico que administrados respuesta.          
  
##  <a name="bkmk_example"></a>Ubicación geográfica en función de ejemplo de administración de tráfico

Siguiente es un ejemplo de cómo puedes usar la directiva DNS para lograr el redireccionamiento del tráfico basándose en la ubicación física del cliente que realiza una consulta DNS.   
  
Este ejemplo usa dos empresas ficticias - Contoso servicios en la nube, que proporciona la web y dominio que aloja soluciones; ejemplo de Woodgrove alimentos servicios, que proporciona servicios de entrega de alimentos de numerosas ciudades en todo el mundo y que tiene un sitio Web denominado woodgrove.com.  
  
Servicios en la nube Contoso tiene dos centros de datos, uno de los Estados Unidos y otro en Europa. El centro de datos Europeo hospeda un alimento orden portal para woodgrove.com.   
  
Para garantizar que los clientes woodgrove.com obtendrán una experiencia con capacidad de respuesta de su sitio Web, Woodgrove quiere que los clientes europeos dirigidos al centro de datos Europea y americano dirige al centro de datos de Estados Unidos. Los clientes situados en otro lugar en el mundo pueden dirigirse a cualquiera de los centros de datos.   
  
La ilustración siguiente muestra este escenario.  
  
![Ubicación geográfica en función de ejemplo de administración de tráfico](../../media/DNS-Policy-Geo1/dns_policy_geo1.png)  
  
##  <a name="bkmk_works"></a>Proceso de resolución de nombres DNS funciona  
  
Durante el proceso de resolución de nombre, el usuario intenta conectarse a www.woodgrove.com. Esto da como resultado una solicitud de resolución de nombre DNS que se envía al servidor DNS que está configurado en las propiedades de conexión de red en el equipo del usuario. Por lo general, esto es el servidor DNS proporcionado por el ISP local actúa como una resolución de almacenamiento en caché y se conoce como el LDNS.   
  
Si el nombre DNS no está presente en la memoria caché local de LDNS, el servidor LDNS reenvía la consulta al servidor DNS que está autorizado para woodgrove.com. El servidor DNS responde con el registro solicitado (www.woodgrove.com) en el servidor LDNS, que a su vez se almacena en caché el registro localmente antes de enviarlo al equipo del usuario.  
  
Debido a los servicios de nube de Contoso usa las directivas de servidor DNS, el servidor DNS que hospeda contoso.com está configurada para devolver la ubicación geográfica según las respuestas de tráfico que administrados. Esto da como resultado en la dirección de los clientes Europea en el centro de datos Europea y la dirección de los clientes americano en el centro de datos de Estados Unidos, como se muestra en la ilustración.  
  
En este escenario, el servidor DNS normalmente ve la solicitud de resolución de nombre procedente del servidor LDNS y, con poca frecuencia, desde el equipo del usuario. Por este motivo, la dirección IP de origen en la solicitud de resolución de nombre, tal como se muestra en el servidor DNS autorizado es que del servidor LDNS y no del equipo del usuario. Sin embargo, la dirección IP del servidor LDNS cuando se configura la ubicación geográfica en función con consulta las respuestas proporciona una estimación razonable de la ubicación geográfica del usuario, porque el usuario es consultar el servidor DNS de su ISP local.  
  
>[!NOTE]  
>Las directivas DNS utilizan la dirección IP de remitente en el paquete TCP o UDP que contiene la consulta DNS. Si la consulta alcanza el servidor a través de varios saltos de resolución/LDNS principal, la directiva considera solo la dirección IP de la última resolución desde el que el servidor DNS recibe la consulta.  
  
##  <a name="bkmk_config"></a>Cómo configurar la directiva de DNS para las respuestas de la consulta en función de ubicación geográfica  
Para configurar la directiva DNS para las respuestas de consulta en función de ubicación geográfica, debes realizar los siguientes pasos.  
  
1. [Crear las subredes del cliente DNS](#bkmk_subnets)  
2. [Crear los ámbitos de la zona](#bkmk_scopes)  
3. [Agregar registros a los ámbitos zona](#bkmk_records)  
4. [Crear las directivas](#bkmk_policies)  
  
>[!NOTE]  
>Debes realizar estos pasos en el servidor DNS que está autorizado para la zona que deseas configurar. Pertenencia a **grupo Administradores DNS**, o equivalente, es necesario para realizar los siguientes procedimientos.  
  
Las siguientes secciones proporcionan instrucciones de configuración detallada.  
  
>[!IMPORTANT]  
>Las secciones siguientes incluyen ejemplos de comandos de Windows PowerShell que contienen los valores de ejemplo para el número de parámetros. Asegúrate de que los valores de ejemplo en estos comandos, se reemplaza con valores que son adecuados para su implementación antes de ejecutar estos comandos.  
  
### <a name="bkmk_subnets"></a>Crear las subredes del cliente DNS  
  
El primer paso es identificar las subredes o el espacio de direcciones IP de las regiones para el que quieras redireccionar tráfico. Por ejemplo, si quieres redirigir el tráfico para los Estados Unidos y Europa, deberás identificar las subredes o espacios de direcciones IP de estas regiones.  
  
Puedes obtener esta información de mapas geográficos IP. En función de estas distribuciones geográfica IP, debes crear las "subredes del cliente DNS". Una subred del cliente DNS es una agrupación lógica de subredes IPv4 o IPv6 desde el que se envían consultas a un servidor DNS.  
  
Puedes usar los siguientes comandos de Windows PowerShell para crear subredes del cliente DNS.  
  
      
    Add-DnsServerClientSubnet -Name "USSubnet" -IPv4Subnet "192.0.0.0/24"  
      
    Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "141.1.0.0/24"  
      
  
Para obtener más información, consulta [agregar DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).  
  
### <a name="bkmk_scopes"></a>Crear ámbitos zona  
Después de configuran las subredes del cliente, se debe dividir la zona cuyo tráfico que desee redirigir en dos ámbitos zona diferente, un ámbito para cada una de las subredes del cliente DNS que hayas configurado.   
  
Por ejemplo, si quieres redirigir el tráfico para el nombre DNS www.woodgrove.com, debes crear dos ámbitos zona diferente en la zona woodgrove.com, uno para los Estados Unidos y otro para Europa.  
  
Un ámbito de la zona es una instancia única de la zona. Una zona DNS puede tener varios ámbitos zona, con cada ámbito de la zona que contiene su propio conjunto de registros de DNS. El mismo registro puede encontrarse en varios ámbitos, con diferentes direcciones IP o las mismas direcciones IP.  
  
>[!NOTE]  
>De manera predeterminada, existe un ámbito de la zona en las zonas DNS. El ámbito de esta zona tiene el mismo nombre que la zona y las operaciones de DNS heredadas funcionarán en este ámbito.   
  
Puedes usar los siguientes comandos de Windows PowerShell para crear ámbitos zona.  
  
      
    Add-DnsServerZoneScope -ZoneName "woodgrove.com" -Name "USZoneScope"  
      
    Add-DnsServerZoneScope -ZoneName "woodgrove.com" -Name "EuropeZoneScope"  

Para obtener más información, consulta [agregar DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).  
  
### <a name="bkmk_records"></a>Agregar registros a los ámbitos zona  
Ahora debes agregar los registros que representa el host del servidor web en los ámbitos dos zona.   
  
Por ejemplo, **USZoneScope** y **EuropeZoneScope**. En USZoneScope, puedes agregar el registro www.woodgrove.com con la dirección IP 192.0.0.1, que se encuentra en un centro de datos de EE. UU.; y en EuropeZoneScope puedes agregar el mismo registro (www.woodgrove.com) con la dirección IP 141.1.0.1 en el centro de datos Europeo.   
  
Puedes usar los siguientes comandos de Windows PowerShell para agregar registros a los ámbitos de la zona.  
  
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "USZoneScope  
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "EuropeZoneScope"  
  
  
  
En este ejemplo, también debes usar los siguientes comandos de Windows PowerShell para agregar los registros en el ámbito de la zona de forma predeterminada para garantizar que el resto del mundo todavía puede servidor web de acceso a la woodgrove.com desde cualquiera de los dos centros de datos.  
    
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "192.0.0.1"   
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "141.1.0.1"
      
 
La **zonaámbito** parámetro no se incluye Cuando agregas un registro en el ámbito de forma predeterminada. Esto es lo mismo que agregar los registros en una zona DNS estándar.  
  
Para obtener más información, consulta [agregar DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).  
  
### <a name="bkmk_policies"></a>Crear las directivas  
Después de haber creado las subredes, las particiones (ámbitos zona) y se han agregado registros, debes crear directivas que conectan las subredes y las particiones, para que cuando se trata de una consulta de un origen en una de las subredes del cliente DNS, se devuelve la respuesta de consulta desde el ámbito correcto de la zona. No hay directivas son necesarias para asignar el ámbito de la zona de forma predeterminada.   
  
Puedes usar los siguientes comandos de Windows PowerShell para crear una directiva DNS que se vincula las subredes del cliente DNS y los ámbitos de la zona.   
  
      
    Add-DnsServerQueryResolutionPolicy -Name "USPolicy" -Action ALLOW -ClientSubnet "eq,USSubnet" -ZoneScope "USZoneScope,1" -ZoneName "woodgrove.com"  
      
    Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "EuropeZoneScope,1" -ZoneName "woodgrove.com"  
     
Para obtener más información, consulta [agregar DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  
  
Ahora, el servidor DNS está configurado con las directivas necesarias de DNS para redirigir el tráfico en función de la ubicación geográfica.  
  
Cuando el servidor DNS recibe las consultas de resolución de nombre, el servidor DNS evalúa los campos de la solicitud DNS en las directivas DNS configuradas. Si la dirección IP de origen en la solicitud de resolución de nombre coincide con ninguna de las directivas, se usa el ámbito de la zona asociada para responder a la consulta y el usuario se dirige al recurso que geográficamente más cercano a ellos.   
  
Puede crear miles de directivas DNS según el tráfico de requisitos de administración y todas las nuevas directivas se aplican de forma dinámica - sin reiniciar el servidor DNS, en las consultas entrantes.  
  
  
