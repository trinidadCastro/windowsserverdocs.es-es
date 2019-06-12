---
title: Uso de la directiva DNS para DNS de cerebro dividido en Active Directory
description: Puede usar este tema para aprovechar el tráfico de capacidades de administración de directivas DNS para las implementaciones de cerebro dividido con Active Directory integran zonas DNS en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: f9533204-ad7e-4e49-81c1-559324a16aeb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 66931d2196b741e469cb726929f7b58985b8d0cd
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812151"
---
# <a name="use-dns-policy-for-split-brain-dns-in-active-directory"></a>Uso de la directiva DNS para DNS de cerebro dividido en Active Directory

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para aprovechar las capacidades de administración de tráfico de las directivas DNS para la división\-implementaciones cerebro con Active Directory integrado zonas DNS en Windows Server 2016.

En Windows Server 2016, compatibilidad con las directivas DNS se extiende a Active Directory zonas DNS integradas. Integración de Active Directory proporciona varios\-dominar las capacidades de alta disponibilidad para el servidor DNS. 

Anteriormente, este escenario requiere que los administradores de DNS mantengan dos servidores DNS diferentes, cada que proporcionan servicios a cada conjunto de usuarios internos y externos. Si solo unos pocos registros dentro de la zona se dividieron\-brained o ambas instancias de la zona (interna y externa) se delega al mismo dominio primario, esto se convirtió en un dilema de administración.

> [!NOTE]
> - Se dividen en las implementaciones DNS\-cerebral cuando hay dos versiones de una sola zona, una versión para los usuarios internos de la intranet de la organización y una versión para los usuarios externos, que son, por lo general, los usuarios en Internet.
> - El tema [usar Directiva de DNS para la implementación de DNS Split-Brain](split-brain-DNS-deployment.md) explica cómo puede usar las directivas DNS y los ámbitos de zona para implementar una división\-cerebral sistema DNS en un único servidor DNS de Windows Server 2016.



##  <a name="example-split-brain-dns-in-active-directory"></a>División de ejemplo\-cerebral DNS en Active Directory

Este ejemplo usa una compañía ficticia, Contoso, que mantiene un sitio Web de carrera en www.career.contoso.com.

El sitio tiene dos versiones, uno para los usuarios internos que están disponibles los registros de trabajo interno. Este sitio interno está disponible en la dirección IP local 10.0.0.39. 

La segunda versión es la versión pública del mismo sitio, que está disponible en la dirección IP pública 65.55.39.10.

En ausencia de la directiva de DNS, es necesario el administrador para hospedar estos dos zonas en los servidores DNS de Windows Server independientes y administrarlos por separado. 

Mediante las directivas DNS estas zonas ahora se pueden hospedar en el mismo servidor DNS.

Si el servidor DNS de contoso.com está integrada en Active Directory y está escuchando en dos interfaces de red, el Administrador de DNS de Contoso puede seguir los pasos descritos en este tema para lograr una división\-cerebral implementación.

El Administrador de DNS se configura las interfaces de servidor DNS con las siguientes direcciones IP.

- El adaptador de red accesible desde Internet se configura con una dirección IP pública de 208.84.0.53 para consultas externas.
- El adaptador de red con orientación de Intranet se configura con una dirección IP privada de 10.0.0.56 para las consultas internas.

La siguiente ilustración muestra este escenario.

![Implementación de DNS integrado en AD de cerebro dividido](../../media/DNS-SB-AD/DNS-SB-AD.jpg)

## <a name="how-dns-policy-for-split-brain-dns-in-active-directory-works"></a>Cómo divide la directiva DNS para\-cerebral DNS en Active Directory funciona

Cuando el servidor DNS está configurado con las directivas DNS necesarias, cada solicitud de resolución de nombres se evalúa con las directivas en el servidor DNS.

El servidor de la interfaz se usa en este ejemplo como criterio para diferenciar entre los clientes internos y externos.

Si la interfaz de servidor en el que se recibe la consulta coincide con cualquiera de las directivas, el ámbito de la zona asociada se usa para responder a la consulta. 

Por lo tanto, en nuestro ejemplo, las consultas DNS para www.career.contoso.com que se reciben en la dirección IP privada (10.0.0.56) recepción una respuesta DNS que contiene una dirección IP interna; y las consultas DNS que se reciben en la interfaz de red pública recibirán una respuesta DNS que contiene la dirección IP pública en el ámbito de la zona predeterminada (es igual que la resolución de consultas normal).  

Compatibilidad con DNS dinámico \(DDNS\) actualizaciones y eliminación de registros obsoletos sólo se admite en el ámbito de la zona predeterminada. Dado que los clientes internos son atendidos por el ámbito de la zona de forma predeterminada, los administradores de DNS de Contoso puede seguir utilizando los mecanismos existentes (dinámico DNS o estático) para actualizar los registros en contoso.com. Para que no sean\-predeterminado ámbitos zona \(como el ámbito externo en este ejemplo\), DDNS o eliminación de registros obsoletos de soporte técnico no está disponible.

### <a name="high-availability-of-policies"></a>Alta disponibilidad de directivas

Las directivas DNS no están integradas en Active Directory. Por este motivo, las directivas DNS no se replican en otros servidores DNS que hospedan la misma zona integrada de Active Directory. 

Las directivas DNS se almacenan en el servidor DNS local. Fácilmente puede exportar las directivas DNS desde un servidor a otro mediante el uso de los siguientes comandos de Windows PowerShell de ejemplo.

    $policies = Get-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server01
    
    $policies |  Add-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server02

Para obtener más información, consulte los siguientes temas de referencia de Windows PowerShell.

- [Get-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/get-dnsserverqueryresolutionpolicy?view=win10-ps)
- [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)


## <a name="how-to-configure-dns-policy-for-split-brain-dns-in-active-directory"></a>Configuración de directiva DNS para la división\-cerebral DNS en Active Directory

Para configurar DNS Split-Brain implementación mediante la directiva de DNS, debe usar las siguientes secciones, que proporcionan las instrucciones de configuración detallados.

### <a name="add-the-active-directory-integrated-zone"></a>Agregue la zona integrada de Active Directory

Puede usar el siguiente comando de ejemplo para agregar la zona contoso.com integrada de Active Directory para el servidor DNS.

    Add-DnsServerPrimaryZone -Name "contoso.com" -ReplicationScope "Domain" -PassThru

Para obtener más información, consulte [Add-DnsServerPrimaryZone](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverprimaryzone?view=win10-ps).

### <a name="create-the-scopes-of-the-zone"></a>Creación de los ámbitos de la zona

Puede utilizar esta sección para la partición de la zona contoso.com para crear un ámbito externo de zona.

Un ámbito de la zona es una instancia única de la zona. Una zona DNS puede tener varios ámbitos de zona, con cada ámbito de la zona que contiene su propio conjunto de registros DNS. El mismo registro puede estar presente en varios ámbitos, con direcciones IP diferentes o las mismas direcciones IP. 

Dado que va a agregar este nuevo ámbito de la zona en una zona integrada de Active Directory, el ámbito de la zona y los registros dentro de él se replicarán a través de Active Directory a otros servidores de réplica en el dominio.

De forma predeterminada, un ámbito de la zona existe en cada zona DNS. Este ámbito de la zona tiene el mismo nombre que la zona y las operaciones de DNS heredadas funcionan en este ámbito. Este ámbito de la zona predeterminada va a hospedar la versión interna de www.career.contoso.com.

Puede usar el siguiente comando de ejemplo para crear el ámbito de la zona en el servidor DNS.

    Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "external"

Para obtener más información, consulte [agregar DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).

### <a name="add-records-to-the-zone-scopes"></a>Agregar registros a los ámbitos de zona

El siguiente paso es agregar los registros que representa el host del servidor web en los dos ámbitos externos de la zona y default \(para clientes internos\). 

En el ámbito de la zona interna predeterminada, se agrega el registro www.career.contoso.com con la dirección IP 10.0.0.39, que es una dirección IP privada. y en el ámbito de la zona externa, el mismo registro \(www.career.contoso.com\) se agrega con la dirección IP pública 65.55.39.10. 

Los registros \(tanto en el valor predeterminado interno ámbito y el ámbito de la zona externa de la zona\) se replicarán automáticamente en todo el dominio con sus ámbitos de la zona correspondiente.

Puede usar el siguiente comando de ejemplo para agregar registros a los ámbitos de zona en el servidor DNS.

    Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10" -ZoneScope "external"
    
    Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39”

> [!NOTE]
> El **– zonaámbito** parámetro no se incluye cuando se agrega el registro para el ámbito de la zona predeterminada. Esta acción es igual que agregar registros a una zona normal.

Para obtener más información, consulte [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).

### <a name="create-the-dns-policies"></a>Cree las directivas DNS

Una vez que haya identificado las interfaces de servidor para la red externa y la red interna y ha creado los ámbitos de zona, debe crear las directivas DNS que se conectan los ámbitos de la zona interna y externa.

> [!NOTE]
> Este ejemplo utiliza la interfaz de servidor \(el parámetro - ServerInterface en el siguiente comando de ejemplo\) como criterio para diferenciar entre los clientes internos y externos. Otro método para diferenciar entre clientes internos y externos es mediante el uso de subredes de cliente como criterio. Si puede identificar las subredes a los que pertenecen los clientes internos, puede configurar la directiva de DNS para diferenciar en función de la subred de cliente. Para obtener información sobre cómo configurar la administración del tráfico con criterios de la subred de cliente, consulte [uso de directiva DNS para la administración de tráfico en función de la ubicación geográfica con servidores principales](primary-geo-location.md).

Después de configurar las directivas, cuando se recibe una consulta DNS en la interfaz pública, se devuelve la respuesta desde el ámbito externo de la zona. 

> [!NOTE]
> No hay directivas son necesarios para asignar el ámbito de la zona interna predeterminada. 

    Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,208.84.0.53" -ZoneScope "external,1" -ZoneName contoso.com

> [!NOTE]
> 208.84.0.53 es la dirección IP de la interfaz de red pública.

Para obtener más información, consulte [agregar DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).

Ahora el servidor DNS está configurado con las directivas DNS necesarias para DNS integrado en un servidor de nombres de cerebro dividido con Active Directory zona.

Puede crear miles de las directivas DNS según el tráfico de los requisitos de administración y todas las nuevas directivas se aplican dinámicamente - sin necesidad de reiniciar el servidor DNS, en las consultas entrantes. 
