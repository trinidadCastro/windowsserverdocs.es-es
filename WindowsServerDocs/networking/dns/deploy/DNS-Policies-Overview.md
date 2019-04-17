---
title: Introducción a las directivas DNS
description: En este tema es parte de la DNS directiva escenario guía para Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 566bc270-81c7-48c3-a904-3cba942ad463
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 06086bbd7edc2fa489805eb5075062332e002ab4
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="dns-policies-overview"></a>Introducción a las directivas DNS

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener información acerca de la directiva de DNS, que es nuevo en Windows Server 2016. Directiva de DNS para la administración de tráfico en función de ubicación geográfica, las respuestas de DNS inteligentes en función de la hora del día, puedes usar para administrar un único servidor DNS configurado para la implementación de split\ cerebro, la aplicación de filtros en las consultas DNS y mucho más. Los elementos siguientes proporcionan más detalles sobre estas funcionalidades.

-   **Equilibrio de carga de aplicación.** Cuando se implementaron varias instancias de una aplicación en distintas ubicaciones, puedes usar la directiva de DNS para equilibrar la carga de tráfico entre las instancias de la aplicación diferente, asignar dinámicamente la carga de tráfico de la aplicación.

-   **Administración de tráfico de basada en ubicación Geo\.** Puedes usar la directiva de DNS para permitir que los servidores DNS principal y secundario responder a las consultas del cliente DNS según la ubicación geográfica del cliente y el recurso al que intenta conectarse, el cliente proporciona al cliente con la dirección IP del recurso más cercana. 

-   **Dividir cerebro DNS.** Con split\ cerebro DNS, registros DNS se dividen en distintos ámbitos zona en el mismo servidor DNS y clientes DNS reciban una respuesta en función de si los clientes son interna o externa. Puedes configurar split\ cerebro DNS para las zonas integradas de Active Directory o para las zonas en los servidores DNS independiente.

-   **Filtrado.** Puedes configurar la directiva DNS para crear filtros de consulta que se basan en los criterios que proporciones. Filtros de consultas en la directiva de DNS te permiten configurar el servidor DNS para responder de manera personalizada, en función de la consulta DNS y el cliente DNS que envía la consulta DNS. 
-   **Análisis.** Puedes usar la directiva de DNS para redirigir a malintencionados clientes DNS a una dirección IP de non\ existente en lugar de dirigiéndolos al equipo que está intentando llegar a.

-   **Hora del día en función de redirección.** Puedes usar la directiva de DNS para distribuir el tráfico de la aplicación en diferentes instancias geográficamente de una aplicación mediante las directivas DNS que se basan en el momento del día.

## <a name="new-concepts"></a>Nuevos conceptos  
Para crear directivas para admitir los escenarios mencionados anteriormente, es necesario ser capaz de identificar los grupos de registros en una zona, grupos de clientes en una red, entre otros elementos. Estos elementos se representan mediante los siguientes objetos DNS nuevo:  

- **Subred del cliente:** un objeto de subred de cliente representa una subred IPv4 o IPv6 desde el que se envían consultas a un servidor DNS. Puedes crear subredes para definir más tarde en función de qué subred provienen de las solicitudes de aplicar directivas. Por ejemplo, en un escenario DNS cerebro dividida, la solicitud de resolución para un nombre como *www.microsoft.com* pueda responderse con una dirección IP interna a los clientes de subredes internas y una dirección IP diferentes a los clientes de subredes externos.

- **Ámbito de recursión:** ámbitos recursividad son instancias únicas de un grupo de configuración que controla recursividad en un servidor DNS. Un ámbito de recursión contiene una lista de servidores de reenvío y especifica si está habilitada recursividad. Un servidor DNS puede tener muchos de los ámbitos de recursión. Las directivas de recursividad de servidor DNS le permiten elegir un ámbito de recursión para un conjunto de consultas. Si el servidor DNS no está autorizado para determinados consultas, las directivas de recursividad de servidor DNS permiten controlar cómo resolver las consultas. Puedes especificar qué servidores de reenvío de usar y si vas a usar recursividad.

- **Zona ámbitos:** una zona DNS puede tener varios ámbitos zona, con cada ámbito de la zona que contiene su propio conjunto de registros de DNS. El mismo registro puede encontrarse en varios ámbitos, con diferentes direcciones IP. Además, las transferencias de zona se realizan en el nivel de ámbito de la zona. Esto significa que los registros de un ámbito de la zona en una zona principal se transferirá al mismo ámbito de la zona en una zona secundaria.

## <a name="types-of-policy"></a>Tipos de directiva

Las directivas de DNS se dividen según el nivel y el tipo. Puedes usar directivas de resolución de consulta para definir cómo se procesan las consultas y directivas de transferencia de zona para definir cómo se producen las transferencias de zona. Puedes aplicar cada tipo de directiva en el nivel de servidor o el nivel de la zona.
  
### <a name="query-resolution-policies"></a>Directivas de resolución de consulta

Puedes usar directivas de resolución de consulta de DNS para especificar la resolución de entrada consultas se controlan mediante un servidor DNS. Todas las directivas de resolución de DNS consulta contiene los siguientes elementos:  
  
|Campo|Descripción|Valores posibles|  
|---------|---------------|-------------------|  
|**Nombre**|Nombre de la directiva|-Hasta 256 caracteres<br />: Puede contener cualquier carácter válido para un nombre de archivo|  
|**Estado**|Estado de la directiva|-Habilitar (predeterminado)<br />-Deshabilitado|  
|**Nivel**|Nivel de directiva|-Server<br />-Zone|  
|**Orden de procesamiento**|Una vez que una consulta se clasifica por el nivel y se aplica en, el servidor busca la primera directiva para que coincida con los criterios la consulta y lo aplica para consultar|-Valor numérico<br />-Único valor por directiva que contiene el mismo nivel y se aplica de valor|  
|**Acción**|Acción que debe realizarse al servidor DNS|-Permitir (valor predeterminado para el nivel de la zona)<br />-Denegar (valor predeterminado en el nivel de servidor)<br />-Omitir|  
|**Criterios**|Condición de directiva (o) y lista de criterios que deben cumplir para que aplicar directiva|-Operador condición (o)<br />-Lista de criterios (consulta la siguiente tabla se criterio)|  
|**Ámbito**|Lista de ámbitos de la zona y valores ponderados por ámbito. Valores ponderados se usan para la distribución de equilibrio de carga. Por ejemplo, si esta lista incluye datacenter1 con un grosor de 3 y datacenter2 con un grosor de 5 el servidor responde con un registro de datacentre1 tres veces de ocho solicitudes|-Lista de ámbitos de zona (por su nombre) y grosores de tipos|  

> [!NOTE]
> Las directivas de nivel de servidor solo pueden tener los valores **denegar** o **Ignorar** como una acción.

El campo de criterios de la directiva DNS se compone de dos elementos:

|Nombre|Descripción|Valores de ejemplo|
|--------|---------------|-----------------|
|**Subred del cliente**|Transporte Protocolo usado en la consulta. Entradas posibles son **UDP** y **TCP**|-   **EQ, España, Francia** -resuelve en true, si la subred se identifica como España o Francia<br />-   **NE, Canadá, México** -resuelve en "true" si la subred del cliente es cualquier equipo que no es de Canadá y México|  
|**Protocolo de transporte**|Transporte Protocolo usado en la consulta. Entradas posibles son **UDP** y **TCP**|-   **EQ, TCP**<br />-   **EQ, UDP**|  
|**Protocolo de Internet**|Protocolo de red que se usa en la consulta. Entradas posibles son **IPv4** y **IPv6**|-   **EQ, IPv4**<br />-   **EQ, IPv6**|  
|**Dirección IP de la interfaz del servidor**|Dirección IP de la interfaz de red entrante del servidor DNS|-   **EQ, 10.0.0.1**<br />-   **EQ, 192.168.1.1**|  
|**FQDN**|Nombre completo del registro de la consulta, con la posibilidad de utilizar un carácter comodín|-   **EQ, www.contoso.com** -resuelve ventana true solo if la consulta intentar resolver la *www.contoso.com* FQDN<br />-   **EQ,\*.contoso.com,\*.woodgrove.com** -resuelve en "true" si la consulta es para cualquier registro finaliza en *contoso.com***o***woodgrove.com*|  
|**Tipo de consulta**|Tipo de registro que se está consultando (A, SVR, TXT)|-   **EQ, TXT, SRV** -resuelve ventana True si la consulta está solicitando TXT **o** registro SRV<br />-   **EQ, MX** -resuelve ventana True si la consulta solicita un registro MX|  
|**Hora del día**|Hora del día en que se recibe la consulta|-   **EQ, 10:00-12:00, 22:00-23:00** -resuelve ventana True si se recibe la consulta entre 10A.M. y las horas, **o** entre PM 10 y 11 P.M.|  
  
Con la tabla anterior como punto de partida, la siguiente tabla se podría usarse para definir un criterio que se usa para que coincida con las consultas de cualquier tipo de registros, pero los registros SRV del dominio contoso.com procedentes de un cliente en la subred 10.0.0.0/24a través de TCP entre 8 y CP de 10a través de la interfaz 10.0.0.3:  
  
|Nombre|Valor|  
|--------|---------|  
|Subred del cliente|EQ, 10.0.0.0/24|  
|Protocolo de transporte|EQ, TCP|  
|Dirección IP de la interfaz del servidor|EQ, 10.0.0.3|  
|FQDN|EQ, *. contoso.com|  
|Tipo de consulta|NE, SRV|  
|Hora del día|EQ, 20:00-22:00|  
  
Puedes crear consulta varias directivas de resolución del mismo nivel, siempre y cuando tienen un valor diferente para el orden de procesamiento. Cuando hay varias directivas disponibles, el servidor DNS procesa consultas entrantes en la siguiente manera:  
  
![Procesamiento de directivas DNS](../../media/DNS-Policies-Overview/DNSQueryResolutionPolicyFlowchart.png)  
  
### <a name="recursion-policies"></a>Directivas de recursividad  
Las directivas de recursión son un especial **tipo** de las directivas de nivel de servidor. Las directivas de recursión controlan cómo el servidor DNS realiza recursividad para una consulta. Directivas de recursión, aplican solo cuando el procesamiento de consultas llega a la ruta de acceso de recursión. Puedes elegir un valor de denegación o ignorar para recursividad para un conjunto de consultas. Como alternativa, puedes elegir un conjunto de servidores de reenvío para un conjunto de consultas.  
  
Puedes usar las directivas de recursividad para implementar un Split-brain configuración DNS. En esta configuración, el servidor DNS realiza recursividad para un conjunto de clientes para una consulta, mientras que el servidor DNS no realiza recursividad para otros clientes buscar esa consulta.  
  
Las directivas de recursividad contiene los mismos elementos que contiene una directiva de resolución de consulta DNS normal, junto con los elementos en la siguiente tabla:  
  
|Nombre|Descripción|  
|--------|---------------|  
|**Aplicar en recursividad**|Especifica que esta directiva debe usarse solo para recursividad.|  
|**Ámbito de recursión**|Nombre del ámbito de recursión.|  
  
> [!NOTE]  
> Las directivas de recursividad solo pueden crearse en el nivel de servidor.  
  
### <a name="zone-transfer-policies"></a>Directivas de transferencia de zona  
Directivas de transferencia de zona controlan si se permite una transferencia de zona o no el servidor DNS. Puedes crear directivas de transferencia de zonas en el nivel de servidor o en el nivel de la zona. Aplican directivas de nivel de servidor en cada consulta de transferencia de zona que se produce en el servidor DNS. Las directivas de nivel de zona se aplican únicamente en las consultas en una zona alojada en el servidor DNS. El uso más común para las directivas de nivel de zona es implementar listas bloqueados o seguros.  
  
> [!NOTE]  
> Solo pueden usar directivas de transferencia de zona denegar o ignorar como acciones.  
  
Puedes usar la directiva de transferencia de zona de nivel de servidor siguiente denegar una transferencia de zona para el dominio contoso.com de una subred determinada:  
  
```  
Add-DnsServerZoneTransferPolicy -Name DenyTransferOfCOnsotostoFabrikam -Zone contoso.com -Action DENY -ClientSubnet "EQ,192.168.1.0/24"  
```  
  
Puedes crear a varios transferencia de zona directivas del mismo nivel, siempre y cuando tienen un valor diferente para el orden de procesamiento. Cuando hay varias directivas disponibles, el servidor DNS procesa consultas entrantes en la siguiente manera:  
  
![Proceso DNS para varias directivas de transferencia de zona](../../media/DNS-Policies-Overview/DNSPolicyZone.png)  
  
## <a name="managing-dns-policies"></a>Administración de directivas DNS  
Puedes crear y administrar directivas de DNS mediante PowerShell. Los siguientes ejemplos se vaya a través de los escenarios de ejemplo diferentes que se pueden configurar mediante DNS directivas:  
  
### <a name="traffic-management"></a>Administración de tráfico  
Puedes dirigir el tráfico en función de un FQDN a servidores diferentes según la ubicación del cliente DNS. El siguiente ejemplo muestra cómo crear directivas de administración para dirigir a los clientes de una determinada subred en un centro de datos de Estados Unido y de subred otro en un centro de datos Europeo de tráfico.  
  
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
  
Las dos primeras líneas de la secuencia de crean objetos de subred de Norteamérica y Europa de cliente. Las dos líneas después de crean un ámbito de la zona dentro del dominio contoso.com, uno para cada región. Las dos líneas después de crean un registro en cada zona que asocia ww.contoso.com otra dirección IP, uno para Europa, otra para los Estados Unidos. Por último, las últimas líneas del script crean dos directivas de resolución de consulta de DNS, uno para aplicarse a la subred Norteamérica, otro para la subred de Europa.  
  
### <a name="block-queries-for-a-domain"></a>Consultas de bloqueo para un dominio  
Puedes usar una directiva de resolución de la consulta de DNS para bloquear las consultas a un dominio. El siguiente ejemplo bloquea todas las consultas a treyresearch.net:  
  
```  
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicy" -Action IGNORE -FQDN "EQ,*.treyresearch.com"  
```  
  
### <a name="block-queries-from-a-subnet"></a>Consultas de bloque de una subred  
También puedes bloquear consultas procedentes de una subred específica. El siguiente script crea una subred para 172.0.33.0/24 y, a continuación, crea una directiva para omitir todas las consultas procedentes de subred:  
  
```  
Add-DnsServerClientSubnet -Name "MaliciousSubnet06" -IPv4Subnet 172.0.33.0/24  
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06"  
```  
  
### <a name="allow-recursion-for-internal-clients"></a>Permite la recursividad para clientes internos  
Puedes controlar recursividad mediante una directiva de resolución de consulta de DNS. El ejemplo siguiente puede usarse para habilitar recursividad para clientes internos, mientras se deshabilitan para los clientes externos en un escenario de cerebro dividida.  
  
```  
Set-DnsServerRecursionScope -Name . -EnableRecursion $False   
Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True  
Add-DnsServerQueryResolutionPolicy -Name "SplitBrainPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP  "EQ,10.0.0.34"  
```  
  
La primera línea en el script cambia el ámbito de recursión predeterminado, simplemente se denomina "." (punto) para deshabilitar la recursividad. La segunda línea crea un ámbito de recursión denominado *InternalClients* con recursividad habilitada. Y la tercera línea crea una directiva para aplicar el recién crear el ámbito de recursión para cualquier consulta que llegan a través de una interfaz de servidor que tiene 10.0.0.34 como una dirección IP.  
  
### <a name="create-a-server-level-zone-transfer-policy"></a>Crear una directiva de transferencia de zona de nivel de servidor  
Puedes controlar la transferencia de zona de forma más granular mediante las directivas de transferencia de zona DNS. El script de ejemplo siguiente puede usarse para permitir las transferencias de cualquier servidor en una subred determinada:  
  
```  
Add-DnsServerClientSubnet -Name "AllowedSubnet" -IPv4Subnet 172.21.33.0/24  
Add-DnsServerZoneTransferPolicy -Name "NorthAmericaPolicy" -Action IGNORE -ClientSubnet "ne,AllowedSubnet"  
```  
  
La primera línea en el script crea un objeto de subred denominado *AllowedSubnet* con la dirección IP bloquear 172.21.33.0/24. La segunda línea crea una directiva de transferencia de zona para permitir las transferencias a cualquier servidor DNS en la subred creado anteriormente.  
  
### <a name="create-a-zone-level-zone-transfer-policy"></a>Crear una directiva de transferencia de zona de nivel de zona  
También puedes crear directivas de transferencia de zona de nivel de zona. El siguiente ejemplo pasa por alto cualquier solicitud de una transferencia de zona para contoso.com procedentes de una interfaz de servidor que tiene una dirección IP de 10.0.0.33:  
  
```  
Add-DnsServerZoneTransferPolicy -Name "InternalTransfers" -Action IGNORE -ServerInterfaceIP "ne,10.0.0.33" -PassThru -ZoneName "contoso.com"  
```  
  
## <a name="dns-policy-scenarios"></a>Escenarios de la directiva DNS

Para obtener información sobre cómo usar la directiva DNS para escenarios específicos, consulta los siguientes temas de esta guía.

- [Usar la directiva DNS para la ubicación geográfica en función de la administración de tráfico con servidores principales](primary-geo-location.md)  
- [Usar la directiva DNS para la ubicación geográfica en función de la administración de tráfico con las implementaciones de principal secundario](primary-secondary-geo-location.md)  
- [Usar la directiva de DNS para las respuestas DNS inteligente en función de la hora del día](dns-tod-intelligent.md)
- [Las respuestas DNS en función de la hora del día con una Azure servidor de aplicaciones en la nube](dns-tod-azure-cloud-app-server.md)
- [Usar la directiva de DNS para la implementación de DNS produzca](split-brain-DNS-deployment.md)
- [Usar la directiva de DNS para produzca DNS en Active Directory](dns-sb-with-ad.md)
- [Usar la directiva de DNS para la aplicación de filtros en las consultas DNS](apply-filters-on-dns-queries.md)
- [Usar la directiva de DNS para equilibrio de carga de aplicación](app-lb.md)
- [Usar la directiva de DNS para la aplicación equilibrio de carga con reconocimiento de ubicación geográfica](app-lb-geo.md)


