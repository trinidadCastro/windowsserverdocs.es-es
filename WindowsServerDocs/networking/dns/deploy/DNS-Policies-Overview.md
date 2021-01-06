---
title: Información general de las directivas DNS
description: Aprenda a usar la Directiva de DNS para la administración del tráfico basada en Geo-Location, las respuestas de DNS inteligentes basadas en la hora del día, para administrar un solo servidor DNS configurado para la \- implementación de cerebro dividido, aplicar filtros en consultas DNS, etc.
manager: brianlic
ms.topic: article
ms.assetid: 566bc270-81c7-48c3-a904-3cba942ad463
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d913905fe5b40e4e3e24828a0c6b392c1691043d
ms.sourcegitcommit: 029b1e19ce11160d5f988046e04a83e8ab5a60dc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/05/2021
ms.locfileid: "97904670"
---
# <a name="dns-policies-overview"></a>Información general de las directivas DNS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información acerca de la Directiva de DNS, que es nueva en Windows Server 2016. Puede usar la Directiva de DNS para la administración del tráfico basada en Geo-Location, las respuestas de DNS inteligentes basadas en la hora del día, para administrar un solo servidor DNS configurado para la \- implementación de cerebro dividido, aplicar filtros en consultas DNS, etc. Los elementos siguientes proporcionan más detalles acerca de estas capacidades.

-   **Equilibrio de carga de la aplicación.** Cuando haya implementado varias instancias de una aplicación en diferentes ubicaciones, puede usar la Directiva de DNS para equilibrar la carga de tráfico entre las distintas instancias de aplicación, asignando dinámicamente la carga de tráfico para la aplicación.

-   **\-Administración del tráfico basada en la ubicación geográfica.** Puede usar la Directiva de DNS para permitir que los servidores DNS principal y secundario respondan a las consultas de cliente DNS en función de la ubicación geográfica del cliente y el recurso al que el cliente intenta conectarse, proporcionando al cliente la dirección IP del recurso más cercano.

-   **Divida DNS de cerebro.** Con \- DNS de cerebro dividido, los registros DNS se dividen en diferentes ámbitos de zona en el mismo servidor DNS y los clientes DNS reciben una respuesta basada en si los clientes son internos o externos. Puede configurar \- DNS de cerebro dividido para zonas integradas de Active Directory o para zonas en servidores DNS independientes.

-   **Filtra.** Puede configurar la Directiva de DNS para crear filtros de consulta basados en los criterios que proporcione. Los filtros de consulta de la Directiva de DNS permiten configurar el servidor DNS para responder de forma personalizada en función de la consulta DNS y el cliente DNS que envía la consulta DNS.
-   **Análisis forense.** Puede usar la Directiva de DNS para redirigir a los clientes DNS malintencionados a una dirección IP que no existe, en \- lugar de dirigirlos al equipo al que intentan tener acceso.

-   **Hora del redireccionamiento basado en el día.** Puede usar la Directiva de DNS para distribuir el tráfico de aplicaciones entre diferentes instancias distribuidas geográficamente de una aplicación mediante el uso de directivas DNS basadas en la hora del día.

## <a name="new-concepts"></a>Nuevos conceptos
Con el fin de crear directivas para admitir los escenarios enumerados anteriormente, es necesario poder identificar grupos de registros en una zona, grupos de clientes de una red, entre otros elementos. Estos elementos se representan mediante los siguientes objetos DNS nuevos:

- **Subred de cliente:** un objeto de subred de cliente representa una subred IPv4 o IPv6 desde la que se envían las consultas a un servidor DNS. Puede crear subredes para definir posteriormente las directivas que se van a aplicar en función de la subred de la que proceden las solicitudes. Por ejemplo, en un escenario de DNS de cerebro dividido, la solicitud de resolución de un nombre como <em>www.Microsoft.com</em> se puede responder con una dirección IP interna a los clientes de subredes internas y otra dirección IP a los clientes en subredes externas.

- **Ámbito de recursividad:** los ámbitos de recursividad son instancias únicas de un grupo de valores de configuración que controlan la recursividad en un servidor DNS. Un ámbito de recursividad contiene una lista de reenviadores y especifica si está habilitada la recursividad. Un servidor DNS puede tener muchos ámbitos de recursividad. Las directivas de recursividad de servidor DNS permiten elegir un ámbito de recursividad para un conjunto de consultas. Si el servidor DNS no es autoritativo para determinadas consultas, las directivas de recursividad del servidor DNS le permiten controlar cómo resolver esas consultas. Puede especificar los reenviadores que se van a usar y si se va a utilizar la recursividad.

- **Ámbitos de zona:** una zona DNS puede tener varios ámbitos de zona, donde cada ámbito de zona contiene su propio conjunto de registros DNS. El mismo registro puede estar presente en varios ámbitos, con direcciones IP diferentes. Además, las transferencias de zona se realizan en el nivel de ámbito de zona. Esto significa que los registros de un ámbito de zona de una zona principal se transferirán al mismo ámbito de zona en una zona secundaria.

## <a name="types-of-policy"></a>Tipos de directivas

Las directivas de DNS se dividen por nivel y tipo. Puede usar las directivas de resolución de consultas para definir cómo se procesan las consultas y las directivas de transferencia de zona para definir cómo se producen las transferencias de zona. Puede aplicar cada tipo de directiva en el nivel de servidor o de zona.

### <a name="query-resolution-policies"></a>Directivas de resolución de consultas

Puede usar las directivas de resolución de consultas DNS para especificar cómo se administran las consultas de resolución de entrada por un servidor DNS. Cada directiva de resolución de consultas DNS contiene los siguientes elementos:

|Campo|Description|Valores posibles|
|---------|---------------|-------------------|
|**Nombre**|Nombre de la directiva|-Hasta 256 caracteres<br />-Puede contener cualquier carácter válido para un nombre de archivo|
|**Estado**|Estado de directiva|-Enable (valor predeterminado)<br />-Deshabilitado|
|**Level**|Nivel de Directiva|-Servidor<br />-Zona|
|**Orden de procesamiento**|Una vez que una consulta se clasifica por nivel y se aplica en, el servidor encuentra la primera Directiva para la que la consulta coincide con los criterios y la aplica a la consulta.|-Valor numérico<br />-Valor único por directiva que contiene el mismo nivel y se aplica al valor|
|**Acción**|Acción que va a realizar el servidor DNS|-Allow (valor predeterminado para el nivel de zona)<br />-Deny (valor predeterminado en el nivel de servidor)<br />-Ignore|
|**Criterios**|Condición de directiva (o) y lista de criterios que se deben cumplir para que se aplique la Directiva|: Operador de condición (y/o)<br />-Lista de criterios (vea la tabla de criterios siguiente)|
|**Ámbito**|Lista de ámbitos de zona y valores ponderados por ámbito. Los valores ponderados se usan para la distribución del equilibrio de carga. Por ejemplo, si esta lista incluye centrodedatos1 con un peso de 3 y centrodedatos2 con un peso de 5, el servidor responderá con un registro de datacentre1 tres veces de ocho solicitudes|-Lista de ámbitos de zona (por nombre) y pesos|

> [!NOTE]
> Las directivas de nivel de servidor solo pueden tener los valores **denegar** u **omitir** como acción.

El campo criterios de la Directiva de DNS se compone de dos elementos:


|              Nombre               |                                         Descripción                                          |                                                                                                                               Valores de ejemplo                                                                                                                               |
|---------------------------------|----------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        **Subred de cliente**        | Nombre de una subred de cliente predefinida. Se utiliza para comprobar la subred desde la que se envió la consulta. |                             -   **EQ, España, Francia** : se resuelve como true si la subred se identifica como España o Francia<br />-   **NE, Canadá, México** : se resuelve como true si la subred del cliente es cualquier subred que no sea Canadá y México.                             |
|     **Protocolo de transporte**      |        Protocolo de transporte utilizado en la consulta. Las entradas posibles son **UDP** y **TCP**        |                                                                                                                    -   **EQ, TCP**<br />-   **EQ, UDP**                                                                                                                     |
|      **Protocolo de Internet**      |        Protocolo de red utilizado en la consulta. Las entradas posibles son **IPv4** e **IPv6**        |                                                                                                                   -   **EQ, IPv4**<br />-   **EQ, IPv6**                                                                                                                    |
| **Dirección IP de la interfaz de servidor** |                   Dirección IP de la interfaz de red del servidor DNS entrante                   |                                                                                                              -   **EQ, 10.0.0.1**<br />-   **EQ, 192.168.1.1**                                                                                                              |
|            **FQDN**             |            FQDN del registro en la consulta, con la posibilidad de usar un carácter comodín            | -   **EQ, www. contoso. com** : se resuelve como true solo si la consulta está intentando resolver el FQDN de <em>www.contoso.com</em><br />-   **EQ, \* . contoso.com, \* . Woodgrove.com** : se resuelve como true si la consulta es para cualquier registro que termine en * contoso.com ***o**_Woodgrove.com_ |
|         **Tipo de consulta**          |                          Tipo de registro que se consulta (A, SRV, TXT)                          |                                                  -   **EQ, txt, SRV** : se resuelve como true si la consulta solicita un registro TXT **o** SRV<br />-   **EQ, mx** : se resuelve como true si la consulta solicita un registro MX                                                   |
|         **Hora del día**         |                              Hora del día en que se recibe la consulta                               |                                                                    -   **EQ, 10:00-12:00, 22:00-23:00** -se resuelve como true si la consulta se recibe entre 10 AM y mediodía, **o** entre 10PM y 11 p.m.                                                                    |

Con la tabla anterior como punto de partida, la tabla siguiente podría usarse para definir un criterio que se usa para hacer coincidir las consultas para cualquier tipo de registro, pero los registros SRV del dominio contoso.com proceden de un cliente en la subred 10.0.0.0/24 a través de TCP entre 8 y 10 PM a través de la interfaz 10.0.0.3:

|Nombre|Valor|
|--------|---------|
|Subred de cliente|EQ, 10.0.0.0/24|
|Protocolo de transporte|EQ, TCP|
|Dirección IP de la interfaz de servidor|EQ, 10.0.0.3|
|FQDN|EQ, *. contoso. com|
|Tipo de consulta|NE, SRV|
|Hora del día|EQ, 20:00-22:00|

Puede crear varias directivas de resolución de consulta del mismo nivel, siempre que tengan un valor diferente para el orden de procesamiento. Cuando hay varias directivas disponibles, el servidor DNS procesa las consultas entrantes de la siguiente manera:

![Procesamiento de directivas DNS](../../media/DNS-Policies-Overview/DNSQueryResolutionPolicyFlowchart.png)

### <a name="recursion-policies"></a>Directivas de recursividad
Las directivas de recursividad son un **tipo** especial de directivas de nivel de servidor. Las directivas de recursividad controlan cómo el servidor DNS realiza la recursividad para una consulta. Las directivas de recursividad solo se aplican cuando el procesamiento de consultas alcanza la ruta de recursividad. Puede elegir un valor de denegar u OMITIr para recursividad para un conjunto de consultas. Como alternativa, puede elegir un conjunto de reenviadores para un conjunto de consultas.

Puede usar directivas de recursividad para implementar una configuración de DNS de cerebro dividido. En esta configuración, el servidor DNS realiza la recursividad para un conjunto de clientes para una consulta, mientras que el servidor DNS no realiza recursividad para otros clientes de la consulta.

Las directivas de recursividad contienen los mismos elementos que contiene una directiva de resolución de consultas DNS normal, junto con los elementos de la tabla siguiente:

|Nombre|Descripción|
|--------|---------------|
|**Aplicar en la recursividad**|Especifica que esta directiva solo se debe usar para la recursividad.|
|**Ámbito de recursividad**|Nombre del ámbito de recursividad.|

> [!NOTE]
> Las directivas de recursividad solo se pueden crear en el nivel de servidor.

### <a name="zone-transfer-policies"></a>Directivas de transferencia de zona
Las directivas de transferencia de zona controlan si el servidor DNS permite o no una transferencia de zona. Puede crear directivas para la transferencia de zona en el nivel de servidor o de zona. Las directivas de nivel de servidor se aplican a todas las consultas de transferencia de zona que se producen en el servidor DNS. Las directivas de nivel de zona solo se aplican en las consultas de una zona hospedada en el servidor DNS. El uso más común de las directivas de nivel de zona es implementar listas bloqueadas o seguras.

> [!NOTE]
> Las directivas de transferencia de zona solo pueden usar las acciones denegar u OMITIr como.

Puede usar la Directiva de transferencia de zona de nivel de servidor siguiente para denegar una transferencia de zona para el dominio contoso.com de una subred determinada:

```
Add-DnsServerZoneTransferPolicy -Name DenyTransferOfContosoToFabrikam -Zone contoso.com -Action DENY -ClientSubnet "EQ,192.168.1.0/24"
```

Puede crear varias directivas de transferencia de zona del mismo nivel, siempre y cuando tengan un valor diferente para el orden de procesamiento. Cuando hay varias directivas disponibles, el servidor DNS procesa las consultas entrantes de la siguiente manera:

![Proceso DNS para varias directivas de transferencia de zona](../../media/DNS-Policies-Overview/DNSPolicyZone.png)

## <a name="managing-dns-policies"></a>Administración de directivas DNS
Puede crear y administrar directivas DNS mediante PowerShell. Los ejemplos siguientes se refieren a diferentes escenarios de ejemplo que se pueden configurar a través de directivas DNS:

### <a name="traffic-management"></a>Administración del tráfico
Puede dirigir el tráfico basado en un FQDN a distintos servidores en función de la ubicación del cliente DNS. En el ejemplo siguiente se muestra cómo crear directivas de administración de tráfico para dirigir a los clientes de una determinada subred a un centro de centros de seguridad de América del norte y de otra subred a un centro de recursos europeo.

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

Las dos primeras líneas del script crean objetos de subred de cliente para Norteamérica y Europa. Las dos líneas siguientes crean un ámbito de zona dentro del dominio contoso.com, uno para cada región. Las dos líneas siguientes crean un registro en cada zona que asocia ww.contoso.com a una dirección IP diferente, una para Europa, otra para Norteamérica. Por último, las últimas líneas del script crean dos directivas de resolución de consultas DNS, una que se aplica a la subred de Norteamérica, otra a la subred de Europa.

### <a name="block-queries-for-a-domain"></a>Bloquear consultas para un dominio
Puede usar una directiva de resolución de consultas DNS para bloquear las consultas a un dominio. En el ejemplo siguiente se bloquean todas las consultas a treyresearch.net:

```
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicy" -Action IGNORE -FQDN "EQ,*.treyresearch.com"
```

### <a name="block-queries-from-a-subnet"></a>Bloquear consultas desde una subred
También puede bloquear las consultas procedentes de una subred específica. El script siguiente crea una subred para 172.0.33.0/24 y, a continuación, crea una directiva para omitir todas las consultas procedentes de esa subred:

```
Add-DnsServerClientSubnet -Name "MaliciousSubnet06" -IPv4Subnet 172.0.33.0/24
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06"
```

### <a name="allow-recursion-for-internal-clients"></a>Permitir recursividad para clientes internos
Puede controlar la recursividad mediante una directiva de resolución de consultas DNS. El ejemplo siguiente se puede usar para habilitar la recursividad para clientes internos, al deshabilitarlo para clientes externos en un escenario de cerebro dividido.

```
Set-DnsServerRecursionScope -Name . -EnableRecursion $False
Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True
Add-DnsServerQueryResolutionPolicy -Name "SplitBrainPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP  "EQ,10.0.0.34"
```

La primera línea del script cambia el ámbito de recursividad predeterminado, simplemente denominado "". (punto) para deshabilitar la recursividad. La segunda línea crea un ámbito de recursividad denominado *InternalClients* con recursividad habilitada. Y la tercera línea crea una directiva para aplicar el ámbito de recursividad recién creado a cualquier consulta que llegue a través de una interfaz de servidor que tenga 10.0.0.34 como dirección IP.

### <a name="create-a-server-level-zone-transfer-policy"></a>Crear una directiva de transferencia de zona de nivel de servidor
Puede controlar la transferencia de zona de forma más granular mediante directivas de transferencia de zona DNS. El script de ejemplo siguiente se puede usar para permitir transferencias de zona para cualquier servidor en una subred determinada:

```
Add-DnsServerClientSubnet -Name "AllowedSubnet" -IPv4Subnet 172.21.33.0/24
Add-DnsServerZoneTransferPolicy -Name "NorthAmericaPolicy" -Action IGNORE -ClientSubnet "ne,AllowedSubnet"
```

La primera línea del script crea un objeto de subred denominado *AllowedSubnet* con el bloque IP 172.21.33.0/24. La segunda línea crea una directiva de transferencia de zona para permitir las transferencias de zona a cualquier servidor DNS en la subred creada anteriormente.

### <a name="create-a-zone-level-zone-transfer-policy"></a>Crear una directiva de transferencia de zona de nivel de zona
También puede crear directivas de transferencia de zona de nivel de zona. En el ejemplo siguiente se omiten las solicitudes de transferencia de zona de contoso.com que provienen de una interfaz de servidor que tiene una dirección IP de 10.0.0.33:

```
Add-DnsServerZoneTransferPolicy -Name "InternalTransfers" -Action IGNORE -ServerInterfaceIP "eq,10.0.0.33" -PassThru -ZoneName "contoso.com"
```

## <a name="dns-policy-scenarios"></a>Escenarios de directivas DNS

Para obtener información sobre cómo usar la Directiva de DNS para escenarios específicos, vea los temas siguientes en esta guía.

- [Uso de directiva DNS para la administración del tráfico basada en la ubicación geográfica con servidores principales](primary-geo-location.md)
- [Uso de directiva DNS para la administración del tráfico basada en la ubicación geográfica con implementaciones primarias-secundarias](primary-secondary-geo-location.md)
- [Uso de la directiva de DNS para las respuestas DNS inteligentes basadas en la hora del día](dns-tod-intelligent.md)
- [Respuestas DNS basadas en la hora del día con un servidor de aplicaciones en la nube de Azure](dns-tod-azure-cloud-app-server.md)
- [Usar la Directiva DNS para la implementación de DNS de Split-Brain](split-brain-DNS-deployment.md)
- [Usar la Directiva de DNS para Split-Brain DNS en Active Directory](dns-sb-with-ad.md)
- [Usar la Directiva de DNS para aplicar filtros en consultas DNS](apply-filters-on-dns-queries.md)
- [Uso de la directiva de DNS para el equilibrio de carga de aplicación](app-lb.md)
- [Uso de la directiva de DNS para equilibrio de carga de aplicación con reconocimiento de ubicación geográfica](app-lb-geo.md)

## <a name="using-dns-policy-on-read-only-domain-controllers"></a>Uso de la Directiva DNS en controladores de dominio de Read-Only

La Directiva DNS es compatible con los controladores de dominio de Read-Only. Tenga en cuenta que es necesario reiniciar el servicio servidor DNS para que se carguen las nuevas directivas DNS en Read-Only controladores de dominio. Esto no es necesario en los controladores de dominio de escritura.
