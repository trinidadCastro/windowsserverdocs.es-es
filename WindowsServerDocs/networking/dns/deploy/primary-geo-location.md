---
title: Uso de directiva DNS para la administración del tráfico basada en la ubicación geográfica con servidores principales
description: En este tema forma parte de las DNS directiva escenario guía para Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: ef9828f8-c0ad-431d-ae52-e2065532e68f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 110014adf1e23be574f23efc01e8a4d69397e547
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831986"
---
# <a name="use-dns-policy-for-geo-location-based-traffic-management-with-primary-servers"></a>Uso de directiva DNS para la administración del tráfico basada en la ubicación geográfica con servidores principales

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para aprender a configurar la directiva de DNS para permitir que los servidores DNS principales responder a consultas de cliente DNS basadas en la ubicación geográfica del cliente y el recurso al que está intentando conectarse, el cliente proporciona al cliente con el anuncio IP vestido de los recursos más cercano.  
  
>[!IMPORTANT]  
>Este escenario muestra cómo implementar la directiva DNS para la administración del tráfico según la ubicación geográfica cuando se usa servidores DNS principales única. También puede realizar administración del tráfico según la ubicación geográfica cuando hay servidores DNS principales y secundarios. Si tiene una implementación de principal secundario, complete primero los pasos descritos en este tema y, a continuación, complete los pasos que se proporcionan en el tema [uso de directiva DNS para la administración de tráfico en función de la ubicación geográfica con implementaciones primarias-secundarias](primary-secondary-geo-location.md).

Con las nuevas directivas DNS, puede crear una directiva DNS que permite que el servidor DNS responder a una consulta de cliente que solicita la dirección IP de un servidor Web. Las instancias del servidor Web pueden estar ubicadas en distintos centros de datos en diferentes ubicaciones físicas. DNS puede evaluar el cliente y las ubicaciones del servidor Web y luego responder a la solicitud de cliente al proporcionar al cliente con un servidor Web de dirección IP para un servidor Web que está ubicado físicamente más cercano al cliente.  

Puede usar los siguientes parámetros de directiva DNS para controlar las respuestas del servidor DNS a las consultas de los clientes DNS. 

- **Subred de cliente**. Nombre de una subred de cliente predefinido. Se usa para comprobar la subred desde la que se envió la consulta.
- **Protocolo de transporte**. Transporte de protocolo usado en la consulta. Las entradas posibles son **UDP** y **TCP**.
- **Protocolo de Internet**. Protocolo de red utilizado en la consulta. Las entradas posibles son **IPv4** y **IPv6**.
- **Dirección IP de interfaz del servidor**. Dirección IP de la interfaz de red del servidor DNS que recibió la solicitud DNS.
- **FQDN**. El completo dominio nombre (FQDN) del registro en la consulta, con la posibilidad de usar un carácter comodín.
- **Tipo de consulta**. Tipo de registro que se está consultando (A, SRV, TXT, etcetera).
- **Hora del día**. Hora del día en que se recibe la consulta. 
  
Los siguientes criterios se pueden combinar con un operador lógico (o) para formular las expresiones de directiva. Cuando estas expresiones coinciden, las directivas se esperan que realice una de las siguientes acciones.
 
- **Omitir**. El servidor DNS silenciosamente la consulta.          
- **Denegar** El servidor DNS responde a esa consulta con una respuesta de error.          
- **Permitir** El servidor DNS responde con respuesta de traffic Manager.          
  
##  <a name="bkmk_example"></a>Ubicación geográfica en función de ejemplo de administración de tráfico

La siguiente es un ejemplo de cómo se puede usar la directiva DNS para lograr el redireccionamiento del tráfico basándose en la ubicación física del cliente que realiza una consulta DNS.   
  
En este ejemplo utiliza dos empresas ficticios: servicios de nube de Contoso, que proporciona soluciones de hospedaje de dominios y web y servicios de alimentación de Woodgrove, que proporciona servicios de entrega de alimentos en varias ciudades en todo el mundo, y que tiene un sitio Web denominado woodgrove.com.  
  
Servicios en la nube de Contoso tiene dos centros de datos, uno en Estados Unidos y otros en Europa. El centro de datos Europeo hospeda un alimento portal para woodgrove.com de ordenación.   
  
Para asegurarse de que los clientes de woodgrove.com obtengan una capacidad de respuesta desde su sitio Web, Woodgrove desea europeos clientes dirigidos al centro de datos Europeo y American dirige al centro de datos de Estados Unidos. Los clientes ubicados en otro lugar del mundo se pueden dirigir a cualquiera de los centros de datos.   
  
La siguiente ilustración muestra este escenario.  
  
![Ubicación geográfica en función de ejemplo de administración de tráfico](../../media/DNS-Policy-Geo1/dns_policy_geo1.png)  
  
##  <a name="bkmk_works"></a>Proceso de resolución de nombres DNS funciona  
  
Durante el proceso de resolución de nombres, el usuario intenta conectarse a www.woodgrove.com. Esto da como resultado una solicitud de resolución de nombre DNS que se envía al servidor DNS que está configurado en las propiedades de conexión de red en el equipo del usuario. Normalmente, esto es el servidor DNS proporcionado por el ISP local actúa como una resolución de caché y se conoce como el LDNS.   
  
Si el nombre DNS no está presente en la caché local de LDNS, el servidor LDNS reenvía la consulta al servidor DNS que sea autoritativo para woodgrove.com. El servidor DNS autoritativo responde con el registro solicitado (www.woodgrove.com) para el servidor LDNS, que a su vez se almacena en caché el registro localmente antes de enviarlo al equipo del usuario.  
  
Dado que los servicios de nube de Contoso usa directivas de servidor DNS, el servidor DNS autoritativo que hospeda contoso.com está configurado para devolver las respuestas de traffic Manager según la ubicación geográfica. Esto da como resultado la dirección de los clientes europeos en el centro de datos Europeo y la dirección de los clientes American al centro de datos de EE. UU., como se muestra en la ilustración.  
  
En este escenario, el servidor DNS autoritativo normalmente ve la solicitud de resolución de nombres que provienen desde el servidor LDNS y, con poca frecuencia, desde el equipo del usuario. Por este motivo, la dirección IP de origen en la solicitud de resolución de nombres, tal como se muestra en el servidor DNS autoritativo es que las del servidor LDNS y no que el del equipo del usuario. Sin embargo, utilizando la dirección IP del servidor LDNS al configurar la ubicación geográfica basadas en consulta respuestas proporciona una estimación razonable de la ubicación geográfica del usuario, ya que el usuario está consultando el servidor DNS de su ISP local.  
  
>[!NOTE]  
>Las directivas DNS usan la dirección IP de remitente en el paquete UDP o TCP que contiene la consulta DNS. Si la consulta alcanza el servidor principal a través de varios saltos solucionador/LDNS, la directiva tendrá en cuenta solo la dirección IP de la última resolución desde el que el servidor DNS recibe la consulta.  
  
##  <a name="bkmk_config"></a>Cómo configurar la directiva de DNS para respuestas de consulta en función de la ubicación geográfica  
Para configurar la directiva DNS para las respuestas de consulta según la ubicación geográfica, debe realizar los pasos siguientes.  
  
1. [Crear las subredes del cliente DNS](#bkmk_subnets)  
2. [Creación de los ámbitos de la zona](#bkmk_scopes)  
3. [Agregar registros a los ámbitos de zona](#bkmk_records)  
4. [Cree las directivas](#bkmk_policies)  
  
>[!NOTE]  
>Debe realizar estos pasos en el servidor DNS que sea autoritativo para la zona que desea configurar. Pertenencia a **DnsAdmins**, o equivalente, es necesario para realizar los procedimientos siguientes.  
  
Las secciones siguientes proporcionan instrucciones de configuración detallada.  
  
>[!IMPORTANT]  
>Las secciones siguientes incluyen ejemplos de comandos de Windows PowerShell que contienen valores de ejemplo para muchos parámetros. Asegúrese de sustituir los valores de ejemplo de estos comandos con los valores adecuados para su implementación antes de ejecutar estos comandos.  
  
### <a name="bkmk_subnets"></a>Crear las subredes del cliente DNS  
  
El primer paso es identificar las subredes o espacio de direcciones IP de las regiones para la que desea redirigir el tráfico. Por ejemplo, si desea redirigir el tráfico de los Estados Unidos y Europa, debe identificar las subredes o espacios de direcciones IP de estas regiones.  
  
Puede obtener esta información de mapas de Geo-IP. En función de estas distribuciones de Geo-IP, debe crear las "subredes del cliente DNS". Una subred de cliente DNS es una agrupación lógica de subredes IPv4 o IPv6 desde el que las consultas se envían a un servidor DNS.  
  
Puede usar los siguientes comandos de Windows PowerShell para crear subredes de cliente DNS.  
  
      
    Add-DnsServerClientSubnet -Name "USSubnet" -IPv4Subnet "192.0.0.0/24"  
      
    Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "141.1.0.0/24"  
      
  
Para obtener más información, consulte [agregar DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).  
  
### <a name="bkmk_scopes"></a>Crear ámbitos de zona  
Después de configuran las subredes del cliente, debe dividir la zona de cuyo tráfico desea redirigir en dos ámbitos de zona diferente, un ámbito para cada una de las subredes del cliente DNS que haya configurado.   
  
Por ejemplo, si desea redirigir el tráfico de la www.woodgrove.com nombre DNS, debe crear dos ámbitos de zona diferente en la zona woodgrove.com, uno para Estados Unidos y otro para Europa.  
  
Un ámbito de la zona es una instancia única de la zona. Una zona DNS puede tener varios ámbitos de zona, con cada ámbito de la zona que contiene su propio conjunto de registros DNS. El mismo registro puede estar presente en varios ámbitos, con direcciones IP diferentes o las mismas direcciones IP.  
  
>[!NOTE]  
>De forma predeterminada, un ámbito de la zona existe en las zonas DNS. Este ámbito de la zona tiene el mismo nombre que la zona y las operaciones de DNS heredadas funcionan en este ámbito.   
  
Puede usar los siguientes comandos de Windows PowerShell para crear ámbitos de la zona.  
  
      
    Add-DnsServerZoneScope -ZoneName "woodgrove.com" -Name "USZoneScope"  
      
    Add-DnsServerZoneScope -ZoneName "woodgrove.com" -Name "EuropeZoneScope"  

Para obtener más información, consulte [agregar DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).  
  
### <a name="bkmk_records"></a>Agregar registros a los ámbitos de zona  
Ahora debe agregar los registros que representa el host del servidor web en los ámbitos de la zona de dos.   
  
Por ejemplo, **USZoneScope** y **EuropeZoneScope**. En USZoneScope, puede agregar el www.woodgrove.com registro con la dirección IP 192.0.0.1, que se encuentra en un centro de datos de EE. UU.; y en EuropeZoneScope puede agregar el mismo registro (www.woodgrove.com) con la dirección IP 141.1.0.1 en el centro de datos Europeo.   
  
Puede usar los siguientes comandos de Windows PowerShell para agregar registros a los ámbitos de la zona.  
  
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "USZoneScope  
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "EuropeZoneScope"  
  
  
  
En este ejemplo, también debe usar los siguientes comandos de Windows PowerShell para agregar registros en el ámbito de la zona predeterminada para asegurarse de que el resto del mundo todavía puede acceder al servidor de web de woodgrove.com desde cualquiera de los dos centros de datos.  
    
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "192.0.0.1"   
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "141.1.0.1"
      
 
El **zonaámbito** parámetro no se incluye cuando se agrega un registro en el ámbito predeterminado. Esto es igual que agregar registros a una zona DNS estándar.  
  
Para obtener más información, consulte [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).  
  
### <a name="bkmk_policies"></a>Cree las directivas  
Después de haber creado las subredes, las particiones (ámbitos de zona) y se han agregado registros, debe crear directivas que se conectan a las subredes y las particiones, por lo que cuando una consulta procede de un origen en una de las subredes de cliente DNS, se devuelve la respuesta de consulta desde el ámbito correcto de la zona. No hay directivas son necesarios para la asignación de ámbito de la zona predeterminada.   
  
Puede usar los siguientes comandos de Windows PowerShell para crear una directiva DNS que vincula las subredes del cliente DNS y los ámbitos de la zona.   
  
      
    Add-DnsServerQueryResolutionPolicy -Name "USPolicy" -Action ALLOW -ClientSubnet "eq,USSubnet" -ZoneScope "USZoneScope,1" -ZoneName "woodgrove.com"  
      
    Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "EuropeZoneScope,1" -ZoneName "woodgrove.com"  
     
Para obtener más información, consulte [agregar DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  
  
Ahora el servidor DNS está configurado con las directivas DNS necesarias para redirigir el tráfico según la ubicación geográfica.  
  
Cuando el servidor DNS recibe consultas de resolución de nombres, el servidor DNS se evalúa como los campos de la solicitud DNS con las directivas DNS configuradas. Si la dirección IP de origen en la solicitud de resolución de nombre coincide con cualquiera de las directivas, el ámbito de la zona asociada se utiliza para responder a la consulta y se dirige al usuario al recurso que está geográficamente más cercano a ellos.   
  
Puede crear miles de las directivas DNS según el tráfico de los requisitos de administración y todas las nuevas directivas se aplican dinámicamente - sin necesidad de reiniciar el servidor DNS, en las consultas entrantes.  
  
  
