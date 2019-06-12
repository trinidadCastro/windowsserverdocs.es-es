---
title: Información general de las directivas DNS
description: En este tema forma parte de las DNS directiva escenario guía para Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 566bc270-81c7-48c3-a904-3cba942ad463
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6c4d8f9bb6c56e8f90a90cd4e77565a39211f719
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446431"
---
# <a name="dns-policies-overview"></a>Información general de las directivas DNS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para obtener información acerca de la directiva de DNS, que es nuevo en Windows Server 2016. Puede usar la directiva de DNS para la ubicación geográfica en función de administración del tráfico, las respuestas DNS inteligentes que según la hora del día, y administrar un solo servidor DNS configurado para la división\-implementación cerebro, aplicar filtros en las consultas de DNS y mucho más. Los elementos siguientes proporcionan más detalles sobre estas capacidades.

-   **Equilibrio de carga de aplicación.** Cuando implemente varias instancias de una aplicación en diferentes ubicaciones, puede usar la directiva de DNS para equilibrar la carga de tráfico entre las instancias de aplicación diferente, asignar dinámicamente la carga de tráfico de la aplicación.

-   **Geográfica\-ubicación en función de administración del tráfico.** Puede usar Directiva de DNS para permitir que los servidores DNS principal y secundario responder a consultas de cliente DNS basadas en la ubicación geográfica del cliente y el recurso al que está intentando conectarse, el cliente proporciona al cliente con la dirección IP de la más cercana recurso. 

-   **Dividir el DNS de cerebro.** Con división\-cerebral DNS, los registros DNS se dividen en distintos ámbitos de zona en el mismo servidor DNS y los clientes DNS reciben una respuesta basándose en si los clientes son los clientes internos o externos. Puede configurar división\-cerebral DNS para las zonas integradas en Active Directory o para las zonas en los servidores DNS independientes.

-   **El filtrado.** Puede configurar la directiva DNS para crear filtros de consulta que se basan en criterios que suministre. Filtros de consulta en la directiva DNS permiten configurar el servidor DNS para responder de manera personalizada basada en la consulta DNS y el cliente DNS que envía la consulta DNS. 
-   **Análisis forense.** Puede usar Directiva de DNS para redirigir los clientes malintencionados de DNS a un no\-dirección IP existente en lugar de dirigiéndolas al equipo que están intentando alcanzar.

-   **Hora del día en función de redirección.** Puede usar la directiva de DNS para distribuir el tráfico de aplicación a través de distintas instancias distribuidas geográficamente de una aplicación mediante el uso de las directivas DNS que se basan en la hora del día.

## <a name="new-concepts"></a>Nuevos conceptos  
Para crear directivas para admitir los escenarios mencionados anteriormente, es necesario poder identificar grupos de registros en una zona, grupos de clientes en una red, entre otros elementos. Estos elementos se representan mediante los siguientes nuevos objetos DNS:  

- **Subred de cliente:** un objeto de subred de cliente representa una subred IPv4 o IPv6 desde el que las consultas se envían a un servidor DNS. Puede crear subredes para definir las directivas que se aplican en función de la subred que proceden las solicitudes de más adelante. Por ejemplo, en un escenario de DNS de cerebro dividido, la solicitud para la resolución para un nombre, como <em>www.microsoft.com</em> se pueden responder con una dirección IP interna a los clientes de subredes internas y una dirección IP diferente a los clientes de externo subredes.

- **Ámbito de recursividad:** ámbitos de recursividad son instancias únicas de un grupo de configuraciones que controlan la recursividad en un servidor DNS. Un ámbito de recursividad contiene una lista de reenviadores y especifica si está habilitada la recursividad. Un servidor DNS puede tener muchos ámbitos de recursividad. Las directivas de recursividad del servidor DNS le permiten elegir un ámbito de recursividad para un conjunto de consultas. Si el servidor DNS no está autorizado para determinadas consultas, las directivas de recursividad DNS server permiten controlar cómo resolver las consultas. Puede especificar que los reenviadores que se usará y si se debe usar la recursividad.

- **Los ámbitos de la zona:** una zona DNS puede tener varios ámbitos de zona, con cada ámbito de la zona que contiene su propio conjunto de registros DNS. El mismo registro puede estar presente en varios ámbitos, con direcciones IP diferentes. Además, las transferencias de zona se realizan en el nivel de ámbito de la zona. Esto significa que se transferirán los registros de un ámbito de la zona en una zona principal para el ámbito de la misma zona en una zona secundaria.

## <a name="types-of-policy"></a>Tipos de directiva

Las directivas DNS se dividen por nivel y tipo. Puede usar directivas de resolución de consulta para definir cómo se procesan las consultas y las directivas de transferencia de zona para definir cómo se producen transferencias de zona. Se puede aplicar a cada tipo de directiva en el nivel de servidor o el nivel de zona.

### <a name="query-resolution-policies"></a>Directivas de resolución de consulta

Puede usar directivas de resolución de consultas de DNS para especificar la resolución entrante las consultas se controlan mediante un servidor DNS. Cada directiva de resolución de DNS consulta contiene los siguientes elementos:  

|Campo|Descripción|Posibles valores|  
|---------|---------------|-------------------|  
|**Name**|Nombre de directiva|-Hasta 256 caracteres<br />-Puede contener cualquier carácter válido para un nombre de archivo|  
|**Estado**|Estado de la directiva|-Habilitar (predeterminado)<br />-Deshabilitado|  
|**Level**|Nivel de directiva|-Servidor<br />-Zona|  
|**Orden de procesamiento**|Una vez que se clasifica según el nivel y se aplica en una consulta, el servidor busca la primera directiva para el que coincide con los criterios de la consulta y aplica a la consulta|: Valor numérico<br />-Valor de unique por directiva que contiene el mismo nivel y se aplica en el valor|  
|**Acción**|Acción que se realizará por el servidor DNS|-Permitir (valor predeterminado para el nivel de zona)<br />-Denegar (valor predeterminado en el nivel de servidor)<br />-Se omite|  
|**Criterios**|Condición de directiva (o) y la lista de criterios que debe cumplirse para que aplicar directiva|-Operador de condición (o)<br />-Lista de criterios (consulte la siguiente tabla se criterio)|  
|**Ámbito**|Lista de ámbitos de zona y los valores ponderados por ámbito. Se usan valores ponderados de distribución de equilibrio de carga. Por ejemplo, si esta lista incluye centrodedatos1 con un peso de 3 y centrodedatos2 con un peso de 5 el servidor responderá con un registro de datacentre1 tres veces fuera de ocho solicitudes|-Lista de ámbitos de zona (por nombre) y pesos|  

> [!NOTE]
> Directivas de nivel de servidor solo pueden tener los valores **Deny** o **omitir** como una acción.

El campo de criterios de directiva DNS se compone de dos elementos:


|              Nombre               |                                         Descripción                                          |                                                                                                                               Valores de ejemplo                                                                                                                               |
|---------------------------------|----------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        **Subred de cliente**        | Nombre de una subred de cliente predefinido. Se usa para comprobar la subred desde la que se envió la consulta. |                             -   **EQ, España, Francia** -se resuelve como true si la subred se identifica como España o Francia<br />-   **NE, Canadá, México** -se resuelve como true si la subred del cliente es una subred que no sea de Canadá y México                             |
|     **Protocolo de transporte**      |        Transporte de protocolo usado en la consulta. Las entradas posibles son **UDP** y **TCP**        |                                                                                                                    -   **EQ, TCP**<br />-   **EQ, UDP**                                                                                                                     |
|      **Protocolo de Internet**      |        Protocolo de red utilizado en la consulta. Las entradas posibles son **IPv4** y **IPv6**        |                                                                                                                   -   **EQ,IPv4**<br />-   **EQ,IPv6**                                                                                                                    |
| **Dirección IP de interfaz del servidor** |                   Dirección IP de la interfaz de red entrante del servidor DNS                   |                                                                                                              -   **EQ, 10.0.0.1**<br />-   **EQ, 192.168.1.1**                                                                                                              |
|            **FQDN**             |            FQDN del registro de la consulta, con la posibilidad de usar un carácter comodín            | -   **EQ, www.contoso.com** -resuelve tot rue solo la si la consulta está intentando resolver el <em>www.contoso.com</em> FQDN<br />-   **EQ,\*. contoso.com,\*. woodgrove.com** -se resuelve como true si la consulta es cualquier registro termina en *contoso.com***o***woodgrove.com* |
|         **Tipo de consulta**          |                          Tipo de registro que se está consultando (A, SVR, TXT)                          |                                                  -   **EQ, TXT, SRV** -se resuelve como true si la consulta solicita un TXT **o** registro SRV<br />-   **EQ, MX** -se resuelve como true si la consulta solicita un registro MX                                                   |
|         **Hora del día**         |                              Hora del día en que se recibe la consulta                               |                                                                    -   **EQ, 10:00-12:00, 22:00-23:00** -se resuelve como true si se recibe la consulta entre 10 a. M. y a mediodía, **o** entre 10 p. M. y las 11 P.M.                                                                    |

Uso de la tabla anterior como punto de partida, en la tabla siguiente podría usarse para definir un criterio que se utiliza para comparar las consultas para cualquier tipo de registros, pero los registros SRV en el dominio contoso.com procedentes de un cliente en la subred 10.0.0.0/24 a través de TCP entre 8 y 10 P.M. a través de interfaz Lollipop 10.0.0.3:  

|Nombre|Valor|  
|--------|---------|  
|Subred de cliente|EQ,10.0.0.0/24|  
|Protocolo de transporte|EQ, TCP|  
|Dirección IP de interfaz del servidor|EQ, 10.0.0.3|  
|FQDN|EQ,*.contoso.com|  
|Tipo de consulta|NE, SRV|  
|Hora del día|EQ,-20:00-22:00|  

Puede crear consulta de varias directivas de resolución del mismo nivel, siempre tengan un valor diferente para el orden de procesamiento. Cuando hay varias directivas, el servidor DNS procesa las consultas entrantes de la siguiente manera:  

![Procesamiento de directiva de DNS](../../media/DNS-Policies-Overview/DNSQueryResolutionPolicyFlowchart.png)  

### <a name="recursion-policies"></a>Directivas de recursividad  
Las directivas de recursividad son un especial **tipo** de directivas de nivel de servidor. Las directivas de recursividad controlan cómo el servidor DNS realiza recursividad para una consulta. Las directivas de recursividad aplican solo cuando el procesamiento de consultas alcanza la ruta de acceso de recursividad. Puede elegir un valor de denegar u omitir la recursividad para un conjunto de consultas. Como alternativa, puede elegir un conjunto de reenviadores para un conjunto de consultas.  

Puede usar las directivas de recursividad para implementar una configuración de DNS Split-brain. En esta configuración, el servidor DNS realiza recursividad para un conjunto de clientes para una consulta, mientras que el servidor DNS no realiza recursividad para otros clientes para esa consulta.  

Las directivas de recursividad contiene los mismos elementos que contiene una directiva de resolución de consultas DNS normal, junto con los elementos en la tabla siguiente:  

|Nombre|Descripción|  
|--------|---------------|  
|**Se aplican de la repetición**|Especifica que esta directiva solo debe usarse para la recursividad.|  
|**Ámbito de recursividad**|Nombre del ámbito de recursividad.|  

> [!NOTE]  
> Solo se pueden crear directivas de recursividad en el nivel de servidor.  

### <a name="zone-transfer-policies"></a>Directivas de transferencia de zona  
Directivas de transferencia de zona controlan si se permite una transferencia de zona o no por el servidor DNS. Puede crear directivas para la transferencia de zona en el nivel de servidor o en el nivel de zona. Directivas de nivel de servidor se aplican en cada consulta de transferencia de zona que se produce en el servidor DNS. Directivas de nivel de zona solo se aplican en las consultas en una zona hospedada en el servidor DNS. El uso más común para las directivas de nivel de zona es implementar las listas bloqueadas o seguras.  

> [!NOTE]  
> Solo pueden usar directivas de transferencia de zona DENY u omitir como acciones.  

Puede usar la directiva de transferencia de zona de nivel de servidor siguiente para denegar a una transferencia de zona para el dominio contoso.com de una subred determinada:  

```  
Add-DnsServerZoneTransferPolicy -Name DenyTransferOfContosoToFabrikam -Zone contoso.com -Action DENY -ClientSubnet "EQ,192.168.1.0/24"  
```  

Puede crear a la transferencia de zona varias directivas del mismo nivel, siempre tengan un valor diferente para el orden de procesamiento. Cuando hay varias directivas, el servidor DNS procesa las consultas entrantes de la siguiente manera:  

![Proceso de DNS para varias directivas de transferencia de zona](../../media/DNS-Policies-Overview/DNSPolicyZone.png)  

## <a name="managing-dns-policies"></a>Administración de directivas DNS  
Puede crear y administrar las directivas DNS mediante PowerShell. Los ejemplos siguientes se vaya a través de escenarios de ejemplo diferentes que se pueden configurar a través de las directivas DNS:  

### <a name="traffic-management"></a>Administración del tráfico  
Puede dirigir el tráfico en función de un FQDN a diferentes servidores según la ubicación del cliente DNS. El ejemplo siguiente muestra cómo crear directivas de administración para dirigir a los clientes de una subred determinada en un centro de datos de América del Norte y de otra subred en un centro de datos Europeo de tráfico.  

```  
Add-DnsServerClientSubnet -Name "NorthAmericaSubnet" -IPv4Subnet "172.21.33.0/24"  
Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "172.17.44.0/24"  
Add-DnsServerZoneScope -ZoneName "Contoso.com" -Name "NorthAmericaZoneScope"  
Add-DnsServerZoneScope -ZoneName "Contoso.com" -Name "EuropeZoneScope"  
Add-DnsServerResourceRecord -ZoneName "Contoso.com" -A -Name "www" -IPv4Address "172.17.97.97" -ZoneScope "EuropeZoneScope"  
Add-DnsServerResourceRecord -ZoneName "Contoso.com" -A -Name "www" -IPv4Address "172.21.21.21" -ZoneScope "NorthAmericaZoneScope"  
Add-DnsServerQueryResolutionPolicy -Name "NorthAmericaPolicy" -Action ALLOW -ClientSubnet "eq,NorthAmericaSubnet" -ZoneScope "NorthAmericaZoneScope,1" -ZoneName "Contoso.com"  
Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "EuropeZoneScope,1" -ZoneName contoso.com  
```  

Las dos primeras líneas del script crean objetos de subred para América del Norte y Europa de cliente. Las dos líneas después de crean un ámbito de zona en el dominio contoso.com, uno por cada región. Las dos líneas después de crean un registro en cada zona que asocia ww.contoso.com a otra dirección IP, uno para Europa, otra para América del Norte. Por último, las últimas líneas del script crean dos directivas de resolución de consultas de DNS, una que se aplicará a la subred de América del Norte, otro para la subred de Europa.  

### <a name="block-queries-for-a-domain"></a>Consultas de bloqueo para un dominio  
Puede usar una directiva de resolución de consultas de DNS para bloquear las consultas a un dominio. El ejemplo siguiente bloquea todas las consultas a treyresearch.net:  

```  
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicy" -Action IGNORE -FQDN "EQ,*.treyresearch.com"  
```  

### <a name="block-queries-from-a-subnet"></a>Consultas de bloqueo de una subred  
También puede bloquear consultas procedentes de una subred específica. El script siguiente crea una subred para 172.0.33.0/24 y, a continuación, crea una directiva para omitir todas las consultas procedentes de esa subred:  

```  
Add-DnsServerClientSubnet -Name "MaliciousSubnet06" -IPv4Subnet 172.0.33.0/24  
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06"  
```  

### <a name="allow-recursion-for-internal-clients"></a>Permite la recursividad para clientes internos  
Puede controlar la recursión mediante el uso de una directiva de resolución de consultas de DNS. El ejemplo siguiente puede utilizarse para habilitar la recursión para clientes internos, mientras se deshabilitan para los clientes externos en un escenario de cerebro dividido.  

```  
Set-DnsServerRecursionScope -Name . -EnableRecursion $False   
Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True  
Add-DnsServerQueryResolutionPolicy -Name "SplitBrainPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP  "EQ,10.0.0.34"  
```  

La primera línea en la secuencia de comandos cambia el ámbito de recursividad de forma predeterminada, se denomina simplemente como "." (punto) para deshabilitar la recursividad. La segunda línea crea un ámbito de recursividad llamado *InternalClients* con recursividad habilitada. Y la tercera línea crea una directiva para aplicar el recién crear ámbito de recursividad a las consultas que llegan a través de una interfaz de servidor que tiene 10.0.0.34 como una dirección IP.  

### <a name="create-a-server-level-zone-transfer-policy"></a>Crear una directiva de transferencia de zona de nivel de servidor  
Puede controlar la transferencia de zona de forma más granular con las directivas de transferencia de zona DNS. El siguiente script de ejemplo puede usarse para permitir transferencias de zona para cualquier servidor en una subred determinada:  

```  
Add-DnsServerClientSubnet -Name "AllowedSubnet" -IPv4Subnet 172.21.33.0/24  
Add-DnsServerZoneTransferPolicy -Name "NorthAmericaPolicy" -Action IGNORE -ClientSubnet "ne,AllowedSubnet"  
```  

La primera línea en el script crea un objeto de subred denominado *AllowedSubnet* con la dirección IP bloquear 172.21.33.0/24. La segunda línea crea una directiva de transferencia de zona para permitir las transferencias de zona a cualquier servidor DNS en la subred que creó anteriormente.  

### <a name="create-a-zone-level-zone-transfer-policy"></a>Crear una directiva de transferencia de zona de nivel de zona  
También puede crear directivas de transferencia de zona de nivel de zona. El ejemplo siguiente omite cualquier solicitud para una transferencia de zona para "contoso.com" procedentes de una interfaz de servidor que tiene una dirección IP de 10.0.0.33:  

```  
Add-DnsServerZoneTransferPolicy -Name "InternalTransfers" -Action IGNORE -ServerInterfaceIP "ne,10.0.0.33" -PassThru -ZoneName "contoso.com"  
```  

## <a name="dns-policy-scenarios"></a>Escenarios de directiva DNS

Para obtener información sobre cómo usar la directiva de DNS para escenarios específicos, vea los temas siguientes en esta guía.

- [Uso de directiva DNS para la ubicación geográfica en función de administración del tráfico con servidores principales](primary-geo-location.md)  
- [Uso de directiva DNS para la ubicación geográfica en función de administración del tráfico con implementaciones primarias-secundarias](primary-secondary-geo-location.md)  
- [Uso de directiva DNS para las respuestas DNS inteligentes basadas en la hora del día](dns-tod-intelligent.md)
- [Las respuestas DNS según la hora del día con una instancia de Azure en la nube del servidor de aplicaciones](dns-tod-azure-cloud-app-server.md)
- [Uso de directiva DNS para la implementación de DNS de cerebro dividido](split-brain-DNS-deployment.md)
- [Uso de directiva DNS para DNS de cerebro dividido en Active Directory](dns-sb-with-ad.md)
- [Usar la directiva de DNS para aplicar filtros en las consultas de DNS](apply-filters-on-dns-queries.md)
- [Uso de directiva DNS para equilibrio de carga de aplicación](app-lb.md)
- [Uso de directiva DNS para la aplicación equilibrio de carga con reconocimiento de ubicación geográfica](app-lb-geo.md)

## <a name="using-dns-policy-on-read-only-domain-controllers"></a>Mediante la directiva de DNS en controladores de dominio de solo lectura

Directiva de DNS es compatible con los controladores de dominio de sólo lectura. Tenga en cuenta que un reinicio del servicio servidor DNS es necesario para nuevas directivas de DNS que se cargue en los controladores de dominio de sólo lectura. Esto no es necesario en los controladores de dominio de escritura.
