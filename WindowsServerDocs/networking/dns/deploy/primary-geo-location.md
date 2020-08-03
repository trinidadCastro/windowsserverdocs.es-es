---
title: Uso de directiva DNS para la administración del tráfico basada en la ubicación geográfica con servidores principales
description: Este tema forma parte de la guía del escenario de la Directiva DNS para Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: ef9828f8-c0ad-431d-ae52-e2065532e68f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 47124531c3e516efeceda57574bd6a648667f90f
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/03/2020
ms.locfileid: "87518281"
---
# <a name="use-dns-policy-for-geo-location-based-traffic-management-with-primary-servers"></a>Uso de directiva DNS para la administración del tráfico basada en la ubicación geográfica con servidores principales

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información sobre cómo configurar la Directiva de DNS para permitir que los servidores DNS principales respondan a las consultas de cliente DNS en función de la ubicación geográfica del cliente y el recurso al que el cliente intenta conectarse, proporcionando al cliente la dirección IP del recurso más cercano.

>[!IMPORTANT]
>En este escenario se muestra cómo implementar la Directiva DNS para la administración del tráfico basada en la ubicación geográfica cuando solo se usan servidores DNS principales. También puede realizar la administración de tráfico basada en la ubicación geográfica cuando tenga servidores DNS principales y secundarios. Si tiene una implementación primaria-secundaria, complete primero los pasos descritos en este tema y, a continuación, complete los pasos que se proporcionan en el tema [uso de la Directiva DNS para la administración del tráfico basado en la ubicación geográfica con las implementaciones principales y secundarias](primary-secondary-geo-location.md).

Con las nuevas directivas DNS, puede crear una directiva DNS que permita que el servidor DNS responda a una consulta de cliente que solicite la dirección IP de un servidor Web. Las instancias del servidor Web podrían estar ubicadas en distintos centros de recursos en diferentes ubicaciones físicas. DNS puede evaluar las ubicaciones del cliente y del servidor Web y, a continuación, responder a la solicitud de cliente proporcionando al cliente una dirección IP de servidor web para un servidor Web que se encuentra físicamente más cerca del cliente.

Puede usar los siguientes parámetros de la Directiva DNS para controlar las respuestas del servidor DNS a las consultas de los clientes DNS.

- **Subred de cliente**. Nombre de una subred de cliente predefinida. Se utiliza para comprobar la subred desde la que se envió la consulta.
- **Protocolo de transporte**. Protocolo de transporte utilizado en la consulta. Las entradas posibles son **UDP** y **TCP**.
- **Protocolo de Internet**. Protocolo de red utilizado en la consulta. Las entradas posibles son **IPv4** e **IPv6**.
- **Dirección IP**de la interfaz del servidor. Dirección IP de la interfaz de red del servidor DNS que recibió la solicitud DNS.
- **FQDN**. El nombre de dominio completo (FQDN) del registro en la consulta, con la posibilidad de usar un carácter comodín.
- **Tipo de consulta**. Tipo de registro que se consulta (A, SRV, TXT, etc.).
- **Hora del día**. Hora del día a la que se recibe la consulta.

Puede combinar los criterios siguientes con un operador lógico (y/o) para formular expresiones de directiva. Cuando estas expresiones coinciden, se espera que las directivas realicen una de las acciones siguientes.

- **Omitir**. El servidor DNS quita la consulta de forma silenciosa.
- **Denegar** El servidor DNS responde a esa consulta con una respuesta de error.
- **Permitir** El servidor DNS responde con la respuesta administrada por el tráfico.

##  <a name="geo-location-based-traffic-management-example"></a><a name="bkmk_example"></a>Ejemplo de administración de tráfico basada en la ubicación geográfica

A continuación se ofrece un ejemplo de cómo se puede usar la Directiva de DNS para lograr el redireccionamiento del tráfico en función de la ubicación física del cliente que realiza una consulta DNS.

En este ejemplo se usan dos compañías ficticias: contoso Cloud Services, que proporciona soluciones de hospedaje web y de dominio. y Woodgrove Food Services, que proporciona servicios de entrega de alimentos en varias ciudades en todo el mundo y que tiene un sitio web denominado woodgrove.com.

Contoso Cloud Services tiene dos centros de recursos, uno en Estados Unidos y otro en Europa. El centro de recursos europeo hospeda un portal de pedidos de comida para woodgrove.com.

Para asegurarse de que los clientes de woodgrove.com obtienen una experiencia de respuesta de su sitio web, Woodgrove desea que los clientes europeos se dirijan al centro de centros de usuarios Europeo y a los clientes americanos dirigidos al centro de Estados Unidos. Los clientes ubicados en cualquier lugar del mundo pueden dirigirse a cualquiera de los centros de recursos.

En la ilustración siguiente se muestra este escenario.

![Ejemplo de administración de tráfico basada en la ubicación geográfica](../../media/DNS-Policy-Geo1/dns_policy_geo1.png)

##  <a name="how-the-dns-name-resolution-process-works"></a><a name="bkmk_works"></a>Cómo funciona el proceso de resolución de nombres DNS

Durante el proceso de resolución de nombres, el usuario intenta conectarse a www.woodgrove.com. Esto da como resultado una solicitud de resolución de nombres DNS que se envía al servidor DNS que está configurado en las propiedades de conexión de red en el equipo del usuario. Normalmente, se trata del servidor DNS proporcionado por el ISP local actuando como un solucionador de almacenamiento en caché y se conoce como LDNS.

Si el nombre DNS no está presente en la caché local de LDNS, el servidor de LDNS reenvía la consulta al servidor DNS que es autoritativo para woodgrove.com. El servidor DNS autoritativo responde con el registro solicitado (www.woodgrove.com) al servidor de LDNS, que, a su vez, almacena en caché el registro localmente antes de enviarlo al equipo del usuario.

Dado que contoso Cloud Services usa directivas de servidor DNS, el servidor DNS autoritativo que hospeda contoso.com está configurado para devolver respuestas administradas por tráfico basadas en la ubicación geográfica. Esto da como resultado la dirección de los clientes europeos al centro de centros de Estados de Europa y la dirección de los clientes americanos al centro de usuarios de EE. UU., tal como se muestra en la ilustración.

En este escenario, el servidor DNS autoritativo suele ver la solicitud de resolución de nombres procedente del servidor de LDNS y, en raras ocasiones, del equipo del usuario. Por este motivo, la dirección IP de origen de la solicitud de resolución de nombres tal como la detecta el servidor DNS autoritativo es la del servidor de LDNS y no la del equipo del usuario. Sin embargo, el uso de la dirección IP del servidor de LDNS cuando se configuran las respuestas de consultas basadas en la ubicación geográfica proporciona una estimación justa de la ubicación geográfica del usuario, ya que el usuario está consultando el servidor DNS de su ISP local.

>[!NOTE]
>Las directivas DNS usan la dirección IP del remitente en el paquete UDP/TCP que contiene la consulta DNS. Si la consulta alcanza el servidor principal a través de varios saltos de resolución/LDNS, la Directiva solo tendrá en cuenta la dirección IP del último solucionador desde el que el servidor DNS recibe la consulta.

##  <a name="how-to-configure-dns-policy-for-geo-location-based-query-responses"></a><a name="bkmk_config"></a>Configuración de la Directiva de DNS para las respuestas de consultas basadas en la ubicación geográfica
Para configurar la Directiva de DNS para las respuestas de consultas basadas en la ubicación geográfica, debe realizar los pasos siguientes.

1. [Crear las subredes de cliente DNS](#bkmk_subnets)
2. [Crear los ámbitos de la zona](#bkmk_scopes)
3. [Agregar registros a los ámbitos de zona](#bkmk_records)
4. [Crear las directivas](#bkmk_policies)

>[!NOTE]
>Debe realizar estos pasos en el servidor DNS que sea autoritativo para la zona que desea configurar. La pertenencia a **DnsAdmins**, o equivalente, es necesaria para realizar los siguientes procedimientos.

En las secciones siguientes se proporcionan instrucciones de configuración detalladas.

>[!IMPORTANT]
>En las secciones siguientes se incluyen comandos de Windows PowerShell de ejemplo que contienen valores de ejemplo para muchos parámetros. Asegúrese de reemplazar los valores de ejemplo de estos comandos por los valores adecuados para su implementación antes de ejecutar estos comandos.

### <a name="create-the-dns-client-subnets"></a><a name="bkmk_subnets"></a>Crear las subredes de cliente DNS

El primer paso consiste en identificar las subredes o el espacio de direcciones IP de las regiones para las que desea redirigir el tráfico. Por ejemplo, si desea redirigir el tráfico para los Estados Unidos y Europa, debe identificar las subredes o los espacios de direcciones IP de estas regiones.

Puede obtener esta información de mapas de IP geográfica. En función de estas distribuciones de IP geográfica, debe crear las "subredes de cliente DNS". Una subred de cliente DNS es una agrupación lógica de subredes IPv4 o IPv6 desde la que se envían las consultas a un servidor DNS.

Puede usar los siguientes comandos de Windows PowerShell para crear subredes de cliente DNS.

```powershell
Add-DnsServerClientSubnet -Name "USSubnet" -IPv4Subnet "192.0.0.0/24"
Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "141.1.0.0/24"
```

Para obtener más información, consulte [Add-DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).

### <a name="create-zone-scopes"></a><a name="bkmk_scopes"></a>Crear ámbitos de zona
Una vez configuradas las subredes de cliente, debe particionar la zona cuyo tráfico desea redirigir en dos ámbitos de zona diferentes, un ámbito para cada una de las subredes de cliente DNS que ha configurado.

Por ejemplo, si desea redirigir el tráfico para el nombre DNS www.woodgrove.com, debe crear dos ámbitos de zona diferentes en la zona woodgrove.com, uno para los Estados Unidos y otro para Europa.

Un ámbito de zona es una instancia única de la zona. Una zona DNS puede tener varios ámbitos de zona, con cada ámbito de zona que contenga su propio conjunto de registros DNS. El mismo registro puede estar presente en varios ámbitos, con direcciones IP diferentes o con las mismas direcciones IP.

>[!NOTE]
>De forma predeterminada, existe un ámbito de zona en las zonas DNS. Este ámbito de zona tiene el mismo nombre que la zona y las operaciones DNS heredadas funcionan en este ámbito.

Puede usar los siguientes comandos de Windows PowerShell para crear ámbitos de zona.

```powershell
Add-DnsServerZoneScope -ZoneName "woodgrove.com" -Name "USZoneScope"
Add-DnsServerZoneScope -ZoneName "woodgrove.com" -Name "EuropeZoneScope"
```

Para obtener más información, consulte [Add-DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).

### <a name="add-records-to-the-zone-scopes"></a><a name="bkmk_records"></a>Agregar registros a los ámbitos de zona
Ahora debe agregar los registros que representan el host del servidor Web en los dos ámbitos de zona.

Por ejemplo, **USZoneScope** y **EuropeZoneScope**. En USZoneScope, puede Agregar el registro www.woodgrove.com con la dirección IP 192.0.0.1, que se encuentra en un centro de centros de Estados Unidos; y en EuropeZoneScope puede Agregar el mismo registro (www.woodgrove.com) con la dirección IP 141.1.0.1 en el centro de centros de recursos europeo.

Puede usar los siguientes comandos de Windows PowerShell para agregar registros a los ámbitos de zona.

```powershell
Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "USZoneScope
Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "EuropeZoneScope"
```

En este ejemplo, también debe usar los siguientes comandos de Windows PowerShell para agregar registros en el ámbito de zona predeterminado con el fin de asegurarse de que el resto del mundo todavía puede tener acceso al servidor Web woodgrove.com desde cualquiera de los dos centros de recursos.

```powershell
Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "192.0.0.1"
Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "141.1.0.1"
```

El parámetro **ZoneScope** no se incluye al agregar un registro en el ámbito predeterminado. Esto es lo mismo que agregar registros a una zona DNS estándar.

Para obtener más información, consulte [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).

### <a name="create-the-policies"></a><a name="bkmk_policies"></a>Crear las directivas
Después de crear las subredes, las particiones (ámbitos de zona) y los registros agregados, debe crear directivas que conecten las subredes y las particiones, de modo que cuando una consulta provenga de un origen en una de las subredes de cliente DNS, la respuesta de la consulta se devuelva desde el ámbito correcto de la zona. No se requieren directivas para asignar el ámbito de zona predeterminado.

Puede usar los siguientes comandos de Windows PowerShell para crear una directiva DNS que vincule las subredes de cliente DNS y los ámbitos de zona.

```powershell
Add-DnsServerQueryResolutionPolicy -Name "USPolicy" -Action ALLOW -ClientSubnet "eq,USSubnet" -ZoneScope "USZoneScope,1" -ZoneName "woodgrove.com"
Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "EuropeZoneScope,1" -ZoneName "woodgrove.com"
```

Para obtener más información, consulte [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).

Ahora el servidor DNS está configurado con las directivas DNS necesarias para redirigir el tráfico en función de la ubicación geográfica.

Cuando el servidor DNS recibe consultas de resolución de nombres, el servidor DNS evalúa los campos de la solicitud DNS con las directivas DNS configuradas. Si la dirección IP de origen de la solicitud de resolución de nombres coincide con alguna de las directivas, el ámbito de la zona asociada se usa para responder a la consulta y se dirige al usuario al recurso que está geográficamente más cercano.

Puede crear miles de directivas DNS según los requisitos de administración del tráfico y todas las directivas nuevas se aplican de forma dinámica, sin necesidad de reiniciar el servidor DNS, en las consultas entrantes.
