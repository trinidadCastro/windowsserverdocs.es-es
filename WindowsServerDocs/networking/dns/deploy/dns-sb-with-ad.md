---
title: Usar la directiva de DNS para produzca DNS en Active Directory
description: Puedes usar este tema para aprovechar el tráfico de zonas DNS en Windows Server 2016 integradas en capacidades de administración de directivas DNS para implementaciones produzca con Active Directory.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: f9533204-ad7e-4e49-81c1-559324a16aeb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d294fcad3b48c8698dffd93e94f6ef7ffc681ea2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-split-brain-dns-in-active-directory"></a>Usar la directiva de DNS para produzca DNS en Active Directory

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para aprovechar las funcionalidades de administración de tráfico de directivas DNS para implementaciones split\ cerebro con Active Directory integrado DNS zonas en Windows Server 2016.

En Windows Server 2016, se ha ampliado el soporte técnico de las directivas DNS para Active Directory zonas DNS integradas. Integración de Active Directory proporciona capacidades de multi\ maestro alta disponibilidad para el servidor DNS. 

Anteriormente, en este escenario se requería que los administradores de DNS mantengan dos servidores DNS diferentes, cada proporcionar servicios a cada conjunto de usuarios, internos y externos. Si solo unos registros dentro de la zona estaban brained split\ o ambas instancias de la zona (interna y externa) se delegan a del mismo dominio principal, esto se convirtió en un dilema de administración.

>[!NOTE]
> - Las implementaciones de DNS son split\ cerebro cuando hay dos versiones de una zona, una versión para los usuarios internos de la intranet de la organización y una versión de los usuarios externos, que son, por lo general, los usuarios en Internet.
> - El tema [usar la directiva de DNS para Split-Brain DNS implementación](split-brain-DNS-deployment.md) explica cómo usar las directivas DNS y ámbitos zona para implementar un sistema DNS split\ cerebro en un único servidor DNS de Windows Server 2016.



##  <a name="example-split-brain-dns-in-active-directory"></a>Ejemplo Split\ cerebro DNS en Active Directory

Este ejemplo usa una empresa ficticia, Contoso, que mantiene un sitio Web de profesiones en www.career.contoso.com.

El sitio tiene dos versiones, uno para los usuarios internos que están disponibles de publicaciones de trabajo interno. Este sitio interno está disponible en la dirección IP local 10.0.0.39. 

La segunda versión es la versión pública del mismo sitio, que está disponible en la dirección IP pública 65.55.39.10.

Ante la ausencia de directiva DNS, el administrador debe hospedar estos dos zonas en servidores DNS de Windows Server independientes y se administran por separado. 

Uso de directivas de DNS estas zonas ahora se hospeden en el mismo servidor DNS.

Si el servidor DNS para contoso.com está integrada en Active Directory y está escuchando en dos interfaces de red, el Administrador de DNS de Contoso puedes seguir los pasos de este tema para lograr una implementación split\ cerebro.

El Administrador de DNS configura las interfaces de servidor DNS con las siguientes direcciones IP.

- El adaptador de red frontal de Internet se configura con una dirección IP pública de 208.84.0.53 para consultas externas.
- El adaptador de red frontal de Intranet se configura con una dirección IP privada de 10.0.0.56 para realizar consultas internas.

La ilustración siguiente muestra este escenario.

![Split-Brain Implementación de DNS integrado en AD](../../media/DNS-SB-AD/DNS-SB-AD.jpg)

## <a name="how-dns-policy-for-split-brain-dns-in-active-directory-works"></a>Cómo funciona la directiva DNS para Split\ cerebro DNS en Active Directory

Cuando el servidor DNS está configurado con las directivas necesarias de DNS, se evalúa cada solicitud de resolución de nombres con las directivas en el servidor DNS.

El servidor interfaz se usa en este ejemplo como los criterios para diferenciar entre los clientes internos y externos.

Si la interfaz del servidor en el que se recibe la consulta coincide con ninguna de las directivas, se usa el ámbito de la zona asociada para responder a la consulta. 

Por lo tanto, en nuestro ejemplo, las consultas DNS para www.career.contoso.com que se reciben en el IP privada (10.0.0.56) reciban una respuesta DNS que contiene una dirección IP interna. y las consultas DNS que se reciben en la interfaz de red pública recibirán una respuesta DNS que contiene la dirección IP pública en el ámbito de la zona de predeterminado (Esto es lo mismo que la resolución de la consulta normal).  

Soporte técnico para las actualizaciones de DNS dinámico \(DDNS\) y el borrado se admite únicamente en el ámbito de la zona de forma predeterminada. Dado que los clientes internos son mantenidos por el ámbito de la zona de manera predeterminada, los administradores de DNS de Contoso puede continuar mediante los mecanismos existentes (dinámico DNS o estáticos) para actualizar los registros en contoso.com. Para los ámbitos de zona non\ predeterminado \ (por ejemplo, el ámbito externo en este example\), DDNS o compactación de soporte técnico no está disponible.

### <a name="high-availability-of-policies"></a>Alta disponibilidad de directivas

Las directivas DNS no están integradas en Active Directory. Por este motivo, las directivas DNS no se replican a otros servidores DNS que alojan la misma zona integrada de Active Directory. 

Directivas DNS se almacenan en el servidor DNS local. Fácilmente puede exportar las directivas DNS de un servidor a otro mediante el uso de los siguientes comandos de Windows PowerShell de ejemplo.

    $policies = Get-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server01
    
    $policies |  Add-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server02

Para obtener más información, consulta los siguientes temas de referencia de Windows PowerShell.

- [Get-DnsServerQueryResolutionPolicy](https://technet.microsoft.com/itpro/powershell/windows/dns-server/get-dnsserverqueryresolutionpolicy)
- [Add-DnsServerQueryResolutionPolicy](https://technet.microsoft.com/itpro/powershell/windows/dns-server/add-dnsserverqueryresolutionpolicy)


## <a name="how-to-configure-dns-policy-for-split-brain-dns-in-active-directory"></a>Cómo configurar la directiva DNS para Split\ cerebro DNS en Active Directory

Configurar DNS Split-Brain implementación mediante la directiva de DNS, debes usar las siguientes secciones, que se proporcionan instrucciones de configuración detallada.

### <a name="add-the-active-directory-integrated-zone"></a>Agregar la zona integrada de Active Directory

Puedes usar el siguiente comando de ejemplo para agregar la zona de Active Directory contoso.com integrado en el servidor DNS.

    Add-DnsServerPrimaryZone -Name "contoso.com" -ReplicationScope "Domain" -PassThru

Para obtener más información, consulta [agregar DnsServerPrimaryZone](https://technet.microsoft.com/library/jj649876.aspx).

### <a name="create-the-scopes-of-the-zone"></a>Crear los ámbitos de la zona

En esta sección puedes usar la partición de la zona contoso.com para crear un ámbito de la zona externa.

Un ámbito de la zona es una instancia única de la zona. Una zona DNS puede tener varios ámbitos zona, con cada ámbito de la zona que contiene su propio conjunto de registros de DNS. El mismo registro puede encontrarse en varios ámbitos, con diferentes direcciones IP o las mismas direcciones IP. 

Dado que vas a agregar este nuevo ámbito de la zona en una zona integrada de Active Directory, el ámbito de la zona y los registros en su interior replicará a través de Active Directory en otros servidores réplica en el dominio.

De manera predeterminada, un ámbito de la zona existe en todas las zonas DNS. El ámbito de esta zona tiene el mismo nombre que la zona y las operaciones de DNS heredadas funcionan en este ámbito. Este ámbito de la zona predeterminado hospedará versión interna de www.career.contoso.com.

Puedes usar el siguiente comando de ejemplo para crear el ámbito de la zona en el servidor DNS.

    Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "external"

Para obtener más información, consulta [agregar DnsServerZoneScope](https://technet.microsoft.com/library/mt126267.aspx).

### <a name="add-records-to-the-zone-scopes"></a>Agregar registros a los ámbitos zona

El siguiente paso es agregar los registros que representan el host del servidor web en los dos ámbitos externo la zona y predeterminados \(for internal clients\). 

En el ámbito de la zona interna de forma predeterminada, se agrega el registro www.career.contoso.com con la dirección IP 10.0.0.39, que es una dirección IP privada; y en el ámbito de la zona externa, se agrega el mismo \(www.career.contoso.com\) registro con la dirección IP pública 65.55.39.10. 

Los registros \ (tanto en el ámbito de la zona interna predeterminado y la scope\ zona externa) se replicarán automáticamente en el dominio con sus ámbitos respectivos zona.

Puedes usar el siguiente comando de ejemplo para agregar registros a los ámbitos de la zona en el servidor DNS.

    Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10" -ZoneScope "external"
    
    Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39”

>[!NOTE]
>El **: zonaámbito** parámetro no se incluye cuando se agrega el registro para el ámbito de la zona de forma predeterminada. Esta acción es la misma agregar registros a una zona normal.

Para obtener más información, consulta [agregar DnsServerResourceRecord](https://technet.microsoft.com/library/jj649925.aspx).

### <a name="create-the-dns-policies"></a>Crear las directivas DNS

Después de haber identificado las interfaces de servidor de la red externa y la red interna y has creado los ámbitos de la zona, debes crear directivas DNS que conectan los ámbitos zona internas y externas.

>[!NOTE]
>Este ejemplo usa la interfaz del servidor \ (el parámetro - ServerInterface en la below\ de comando de ejemplo) como los criterios para diferenciar entre los clientes internos y externos. Otro método para diferenciar entre clientes internos y externos es mediante subredes del cliente como un criterio. Si puedes identificar las subredes a la que pertenecen los clientes internos, puedes configurar la directiva DNS para diferenciar en función de la subred del cliente. Para obtener información sobre cómo configurar la administración de tráfico con criterios de subred de cliente, consulta [usar la directiva de DNS para la administración de tráfico en función de ubicación geográfica con los servidores principal](primary-geo-location.md).

Después de configurar las directivas, cuando se recibe una consulta DNS en la interfaz pública, se devuelve la respuesta desde el ámbito externo de la zona. 

>[!NOTE]
>No hay directivas son necesarias para asignar el ámbito de la zona interna predeterminado. 

    Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,208.84.0.53" -ZoneScope "external,1" -ZoneName contoso.com

>[!NOTE]
>208.84.0.53 Es la dirección IP en la interfaz de red pública.

Para obtener más información, consulta [agregar DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx).

Ahora el servidor DNS está configurado con las directivas necesarias de DNS para un servidor de nombres produzca con Active Directory integrado DNS zona.

Puede crear miles de directivas DNS según el tráfico de requisitos de administración y todas las nuevas directivas se aplican de forma dinámica - sin reiniciar el servidor DNS, en las consultas entrantes. 
