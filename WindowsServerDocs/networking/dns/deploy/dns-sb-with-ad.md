---
title: Usar la Directiva de DNS para DNS de cerebro dividido en Active Directory
description: Puede usar este tema para aprovechar las capacidades de administración del tráfico de las directivas DNS para implementaciones de cerebro dividido con Active Directory zonas DNS integradas en Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: f9533204-ad7e-4e49-81c1-559324a16aeb
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 1f6da8584f7a2b2221fb1a283b8ea4de842ddc58
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87964141"
---
# <a name="use-dns-policy-for-split-brain-dns-in-active-directory"></a>Usar la Directiva de DNS para DNS de cerebro dividido en Active Directory

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para aprovechar las capacidades de administración del tráfico de las directivas DNS para las \- implementaciones de cerebro dividido con Active Directory zonas DNS integradas en Windows Server 2016.

En Windows Server 2016, la compatibilidad con las directivas DNS se extiende a Active Directory zonas DNS integradas. La integración de Active Directory proporciona \- funciones de alta disponibilidad para el servidor DNS.

Anteriormente, este escenario requería que los administradores de DNS mantengan dos servidores DNS diferentes, cada uno de los cuales proporciona servicios a cada conjunto de usuarios, tanto internos como externos. Si solo algunos registros dentro de la zona se han dividido \- de forma cerebro o ambas instancias de la zona (interna y externa) se han delegado en el mismo dominio primario, se convirtió en un dilema de administración.

> [!NOTE]
> - Las implementaciones de DNS se dividen \- en cerebro cuando hay dos versiones de una sola zona, una versión para los usuarios internos de la intranet de la organización y una versión para los usuarios externos, que suelen ser usuarios de Internet.
> - En el tema [uso de la Directiva de DNS para la implementación de DNS de cerebro dividido](split-brain-DNS-deployment.md) se explica cómo puede usar las directivas de DNS y los ámbitos de zona para implementar un \- sistema DNS de cerebro dividido en un solo servidor DNS de Windows Server 2016.

## <a name="example-split-brain-dns-in-active-directory"></a>Ejemplo \- de DNS de cerebro dividido en Active Directory

En este ejemplo se usa una empresa ficticia, Contoso, que mantiene un sitio web de carrera en www.career.contoso.com.

El sitio tiene dos versiones, una para los usuarios internos donde están disponibles los envíos de trabajos internos. Este sitio interno está disponible en la dirección IP local 10.0.0.39.

La segunda versión es la versión pública del mismo sitio, que está disponible en la dirección IP pública 65.55.39.10.

En ausencia de la Directiva de DNS, el administrador debe hospedar estas dos zonas en servidores DNS de Windows Server independientes y administrarlas por separado.

Uso de directivas de DNS estas zonas ahora se pueden hospedar en el mismo servidor DNS.

Si el servidor DNS para contoso.com está Active Directory integrado y está escuchando en dos interfaces de red, el administrador de DNS de Contoso puede seguir los pasos de este tema para lograr una \- implementación de cerebro dividido.

El administrador de DNS configura las interfaces de servidor DNS con las siguientes direcciones IP.

- El adaptador de red accesible desde Internet se configura con una dirección IP pública de 208.84.0.53 para las consultas externas.
- El adaptador de red orientado a la intranet se configura con una dirección IP privada de 10.0.0.56 para las consultas internas.

En la ilustración siguiente se muestra este escenario.

![Implementación de DNS integrada en AD de cerebro dividido](../../media/DNS-SB-AD/DNS-SB-AD.jpg)

## <a name="how-dns-policy-for-split-brain-dns-in-active-directory-works"></a>Cómo funciona la Directiva de DNS para la división \- de DNS de cerebro dividido en Active Directory

Cuando el servidor DNS está configurado con las directivas DNS necesarias, cada solicitud de resolución de nombres se evalúa con respecto a las directivas del servidor DNS.

La interfaz de servidor se usa en este ejemplo como criterio para diferenciar entre los clientes internos y externos.

Si la interfaz de servidor en la que se recibe la consulta coincide con cualquiera de las directivas, el ámbito de la zona asociada se utiliza para responder a la consulta.

Por lo tanto, en nuestro ejemplo, las consultas de DNS para www.career.contoso.com que se reciben en la IP privada (10.0.0.56) reciben una respuesta DNS que contiene una dirección IP interna. y las consultas DNS que se reciben en la interfaz de red pública reciben una respuesta DNS que contiene la dirección IP pública en el ámbito de zona predeterminado (esto es lo mismo que la resolución de consultas normal).

La compatibilidad con las actualizaciones dinámicas de DNS \( \) y la eliminación de registros obsoletos solo se admite en el ámbito de zona predeterminado. Dado que el ámbito de zona predeterminado presta servicio a los clientes internos, los administradores de DNS de Contoso pueden seguir usando los mecanismos existentes (DNS dinámico o estático) para actualizar los registros de contoso.com. \-En el caso de los ámbitos de zona no predeterminados \( , como el ámbito externo en este ejemplo \) , la compatibilidad con DDNS o la eliminación de registros obsoletos no está disponible.

### <a name="high-availability-of-policies"></a>Alta disponibilidad de las directivas

Las directivas DNS no están Active Directory integradas. Por este motivo, las directivas DNS no se replican en los otros servidores DNS que hospedan la misma Active Directory zona integrada.

Las directivas de DNS se almacenan en el servidor DNS local. Puede exportar fácilmente las directivas DNS de un servidor a otro usando los siguientes comandos de Windows PowerShell de ejemplo.

```powershell
$policies = Get-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server01
$policies |  Add-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server02
```

Para obtener más información, vea los siguientes temas de referencia de Windows PowerShell.

- [Get-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/get-dnsserverqueryresolutionpolicy?view=win10-ps)
- [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)

## <a name="how-to-configure-dns-policy-for-split-brain-dns-in-active-directory"></a>Configuración de la Directiva de DNS para el \- DNS de cerebro dividido en Active Directory

Para configurar la implementación de un cerebro dividido en DNS mediante la Directiva DNS, debe usar las siguientes secciones, que proporcionan instrucciones de configuración detalladas.

### <a name="add-the-active-directory-integrated-zone"></a>Agregar la Active Directory zona integrada

Puede usar el siguiente comando de ejemplo para agregar la Active Directory zona integrada contoso.com al servidor DNS.

```powershell
Add-DnsServerPrimaryZone -Name "contoso.com" -ReplicationScope "Domain" -PassThru
```

Para obtener más información, consulte [Add-DnsServerPrimaryZone](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverprimaryzone?view=win10-ps).

### <a name="create-the-scopes-of-the-zone"></a>Crear los ámbitos de la zona

Puede usar esta sección para particionar la zona contoso.com para crear un ámbito de zona externa.

Un ámbito de zona es una instancia única de la zona. Una zona DNS puede tener varios ámbitos de zona, con cada ámbito de zona que contenga su propio conjunto de registros DNS. El mismo registro puede estar presente en varios ámbitos, con direcciones IP diferentes o con las mismas direcciones IP.

Dado que va a agregar este nuevo ámbito de zona en una Active Directory zona integrada, el ámbito de zona y los registros que contiene se replicarán a través de Active Directory a otros servidores de réplica del dominio.

De forma predeterminada, existe un ámbito de zona en cada zona DNS. Este ámbito de zona tiene el mismo nombre que la zona y las operaciones DNS heredadas funcionan en este ámbito. Este ámbito de zona predeterminado hospedará la versión interna de www.career.contoso.com.

Puede usar el siguiente comando de ejemplo para crear el ámbito de zona en el servidor DNS.

```powershell
Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "external"
```

Para obtener más información, consulte [Add-DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).

### <a name="add-records-to-the-zone-scopes"></a>Agregar registros a los ámbitos de zona

El siguiente paso consiste en agregar los registros que representan el host del servidor Web en los dos ámbitos de zona: externo y predeterminado \( para los clientes internos \) .

En el ámbito de zona interna predeterminada, se agrega el registro www.career.contoso.com con la dirección IP 10.0.0.39, que es una dirección IP privada; y en el ámbito de la zona externa, se agrega el mismo registro \( www.Career.contoso.com \) con la dirección IP pública 65.55.39.10.

Los registros \( en el ámbito de zona interna predeterminada y el ámbito de zona externa \) se replicarán automáticamente en el dominio con sus respectivos ámbitos de zona.

Puede usar el siguiente comando de ejemplo para agregar registros a los ámbitos de zona en el servidor DNS.

```powershell
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10" -ZoneScope "external"
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39”
```

> [!NOTE]
> El parámetro **– ZoneScope** no se incluye cuando el registro se agrega al ámbito de la zona predeterminada. Esta acción es igual que agregar registros a una zona normal.

Para obtener más información, consulte [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).

### <a name="create-the-dns-policies"></a>Crear las directivas de DNS

Una vez que haya identificado las interfaces de servidor para la red externa y la red interna y haya creado los ámbitos de zona, debe crear directivas DNS que conecten los ámbitos de zona externa y interna.

> [!NOTE]
> En este ejemplo se usa el \( parámetro-ServerInterface de la interfaz de servidor en el comando de ejemplo siguiente \) como criterio para diferenciar entre los clientes internos y externos. Otro método para diferenciar entre clientes externos e internos es mediante el uso de subredes de cliente como criterio. Si puede identificar las subredes a las que pertenecen los clientes internos, puede configurar la Directiva de DNS para diferenciar en función de la subred de cliente. Para obtener información sobre cómo configurar la administración del tráfico mediante los criterios de subred de cliente, consulte [uso de la Directiva de DNS para la administración del tráfico basado en la ubicación geográfica con los servidores principales](primary-geo-location.md).

Después de configurar las directivas, cuando se recibe una consulta DNS en la interfaz pública, se devuelve la respuesta del ámbito externo de la zona.

> [!NOTE]
> No se requieren directivas para asignar el ámbito de zona interna predeterminado.

```powershell
Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,208.84.0.53" -ZoneScope "external,1" -ZoneName contoso.com
```

> [!NOTE]
> 208.84.0.53 es la dirección IP de la interfaz de red pública.

Para obtener más información, consulte [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).

Ahora el servidor DNS está configurado con las directivas DNS necesarias para un servidor de nombres de cerebro dividido con una zona DNS integrada Active Directory.

Puede crear miles de directivas DNS según los requisitos de administración del tráfico y todas las directivas nuevas se aplican de forma dinámica, sin necesidad de reiniciar el servidor DNS, en las consultas entrantes.
