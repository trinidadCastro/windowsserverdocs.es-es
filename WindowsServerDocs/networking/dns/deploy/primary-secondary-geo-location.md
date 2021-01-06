---
title: Uso de directiva DNS para la administración del tráfico basada en la ubicación geográfica con implementaciones primarias-secundarias
description: Obtenga información sobre cómo crear una directiva DNS para la administración del tráfico basada en la ubicación geográfica cuando la implementación de DNS incluye servidores DNS principales y secundarios.
manager: brianlic
ms.topic: article
ms.assetid: a9ee7a56-f062-474f-a61c-9387ff260929
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 2254b2bfea9949cb14e610ff6e5b08864beb20cb
ms.sourcegitcommit: 029b1e19ce11160d5f988046e04a83e8ab5a60dc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/05/2021
ms.locfileid: "97904900"
---
# <a name="use-dns-policy-for-geo-location-based-traffic-management-with-primary-secondary-deployments"></a>Uso de directiva DNS para la administración del tráfico basada en la ubicación geográfica con implementaciones primarias-secundarias

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información sobre cómo crear una directiva DNS para la administración del tráfico basada en la ubicación geográfica cuando la implementación de DNS incluye servidores DNS principal y secundario.

En el escenario anterior, se [usa la Directiva DNS para la administración del tráfico basada en Geo-Location con servidores principales](primary-geo-location.md), se proporcionan instrucciones para configurar la Directiva de DNS para la administración del tráfico basado en la ubicación geográfica en un servidor DNS principal. En la infraestructura de Internet, sin embargo, los servidores DNS están ampliamente implementados en un modelo primario-secundario, donde la copia de escritura de una zona se almacena en servidores principales seleccionados y seguros, y las copias de solo lectura de la zona se mantienen en varios servidores secundarios.

Los servidores secundarios usan la transferencia autoritaria (AXFR) y la transferencia de zona incremental (IXFR) de protocolos de transferencia de zona para solicitar y recibir actualizaciones de zona que incluyen nuevos cambios en las zonas de los servidores DNS principales.

> [!NOTE]
> Para obtener más información sobre AXFR, consulte la solicitud del equipo de ingeniería de Internet Engineering (IETF) [para obtener comentarios 5936](https://tools.ietf.org/rfc/rfc5936.txt). Para obtener más información sobre IXFR, consulte la solicitud del equipo de ingeniería de Internet Engineering (IETF) [para obtener comentarios 1995](https://tools.ietf.org/html/rfc1995).

## <a name="primary-secondary-geo-location-based-traffic-management-example"></a>Primary-Secondary ejemplo de administración del tráfico basada en Geo-Location
A continuación se ofrece un ejemplo de cómo se puede usar la Directiva DNS en una implementación principal-secundaria para lograr el redireccionamiento del tráfico en función de la ubicación física del cliente que realiza una consulta DNS.

En este ejemplo se usan dos compañías ficticias: contoso Cloud Services, que proporciona soluciones de hospedaje web y de dominio. y Woodgrove Food Services, que proporciona servicios de entrega de alimentos en varias ciudades en todo el mundo y que tiene un sitio web denominado woodgrove.com.

Para asegurarse de que los clientes de woodgrove.com obtienen una experiencia de respuesta de su sitio web, Woodgrove desea que los clientes europeos se dirijan al centro de centros de usuarios Europeo y a los clientes americanos dirigidos al centro de Estados Unidos. Los clientes ubicados en cualquier lugar del mundo pueden dirigirse a cualquiera de los centros de recursos.

Contoso Cloud Services tiene dos centros de recursos, uno en Estados Unidos y otro en Europa, en el que contoso hospeda su portal de pedidos de comida para woodgrove.com.

La implementación DNS de Contoso incluye dos servidores secundarios: **SecondaryServer1**, con la dirección IP 10.0.0.2; y **SecondaryServer2**, con la dirección IP 10.0.0.3. Estos servidores secundarios actúan como servidores de nombres en las dos regiones diferentes, con SecondaryServer1 ubicado en Europa y SecondaryServer2 ubicado en el

Hay una copia de zona principal de escritura en **PrimaryServer** (dirección IP 10.0.0.1), donde se realizan los cambios en la zona. Con las transferencias de zona normales a los servidores secundarios, los servidores secundarios están siempre actualizados con los nuevos cambios en la zona en el PrimaryServer.

En la ilustración siguiente se muestra este escenario.

![Primary-Secondary ejemplo de administración del tráfico basada en Geo-Location](../../media/Dns-Policy_PS1/dns_policy_primarysecondary1.jpg)

## <a name="how-the-dns-primary-secondary-system-works"></a>Cómo funciona el sistema de Primary-Secondary DNS

Cuando se implementa la administración de tráfico basada en la ubicación geográfica en una implementación de DNS principal-secundario, es importante comprender cómo se producen las transferencias de zona secundaria principal y secundaria antes de aprender sobre las transferencias de nivel de ámbito de zona. En las secciones siguientes se proporciona información sobre las transferencias de nivel de ámbito de zona y zona.

- [Transferencias de zona en una implementación de servidor principal-secundario de DNS](#zone-transfers-in-a-dns-primary-secondary-deployment)
- [Transferencias de nivel de ámbito de zona en una implementación de servidor principal-secundario de DNS](#zone-scope-level-transfers-in-a-dns-primary-secondary-deployment)

### <a name="zone-transfers-in-a-dns-primary-secondary-deployment"></a>Transferencias de zona en una implementación de servidor principal-secundario de DNS

Puede crear una implementación de servidor principal-secundario de DNS y sincronizar zonas con los pasos siguientes.
1. Al instalar DNS, la zona principal se crea en el servidor DNS principal.
2. En el servidor secundario, cree las zonas y especifique los servidores principales.
3. En los servidores principales, puede Agregar los servidores secundarios como secundarios de confianza en la zona principal.
4. Las zonas secundarias realizan una solicitud de transferencia de zona completa (AXFR) y reciben la copia de la zona.
5. Cuando sea necesario, los servidores principales envían notificaciones a los servidores secundarios acerca de las actualizaciones de zona.
6. Los servidores secundarios realizan una solicitud de transferencia de zona incremental (IXFR). Por este motivo, los servidores secundarios permanecen sincronizados con el servidor principal.

### <a name="zone-scope-level-transfers-in-a-dns-primary-secondary-deployment"></a>Transferencias de nivel de ámbito de zona en una implementación de servidor principal-secundario de DNS

El escenario de administración del tráfico requiere pasos adicionales para particionar las zonas en diferentes ámbitos de zona. Debido a esto, se requieren pasos adicionales para transferir los datos dentro de los ámbitos de la zona a los servidores secundarios y para transferir directivas y subredes de cliente DNS a los servidores secundarios.

Después de configurar la infraestructura DNS con los servidores principales y secundarios, el DNS realiza automáticamente las transferencias de nivel de ámbito de zona, mediante los siguientes procesos.

Para garantizar la transferencia de nivel de ámbito de zona, los servidores DNS usan los mecanismos de extensión para DNS (EDNS0) OPC RR. Todas las solicitudes de transferencia de zona (AXFR o IXFR) de las zonas con ámbitos se originan con un valor de RR OPC OPC, cuyo identificador de opción se establece en "65433" de forma predeterminada. Para obtener más información sobre EDNSO, consulte la [solicitud de comentarios de IETF 6891](https://tools.ietf.org/html/rfc6891).

El valor de OPT RR es el nombre del ámbito de la zona para el que se envía la solicitud. Cuando un servidor DNS principal recibe este paquete de un servidor secundario de confianza, interpreta la solicitud como para ese ámbito de zona.

Si el servidor principal tiene ese ámbito de zona, responde con los datos de transferencia (XFR) de ese ámbito. La respuesta contiene un RR OPT con el mismo identificador de opción "65433" y un valor establecido en el mismo ámbito de zona. Los servidores secundarios reciben esta respuesta, recuperan la información de ámbito de la respuesta y actualizan ese ámbito concreto de la zona.

Después de este proceso, el servidor principal mantiene una lista de secundarias de confianza que han enviado una solicitud de ámbito de zona para las notificaciones.

Para cualquier actualización adicional en un ámbito de zona, se envía una notificación IXFR a los servidores secundarios, con el mismo RR OPT. El ámbito de la zona que recibe esa notificación hace que la solicitud IXFR que contiene el RR OPT y el mismo proceso que se ha descrito anteriormente.

## <a name="how-to-configure-dns-policy-for-primary-secondary-geo-location-based-traffic-management"></a>Configuración de la Directiva de DNS para la administración del tráfico basada en Primary-Secondary Geo-Location

Antes de comenzar, asegúrese de que ha completado todos los pasos del tema [uso de la Directiva de DNS para la administración del tráfico basado en Geo-Location con servidores principales](./primary-geo-location.md), y el servidor DNS principal se configura con zonas, ámbitos de zona, subredes de cliente DNS y la Directiva DNS.

> [!NOTE]
> Las instrucciones de este tema para copiar las subredes de cliente DNS, los ámbitos de zona y las directivas de DNS de los servidores principales DNS a los servidores secundarios DNS son para la configuración inicial y la validación de DNS. En el futuro, es posible que desee cambiar las opciones de subredes de cliente DNS, ámbitos de zona y directivas en el servidor principal. En este caso, puede crear scripts de automatización para mantener sincronizados los servidores secundarios con el servidor principal.

Para configurar la Directiva de DNS para las respuestas de consultas basadas en la ubicación geográfica primaria-secundaria, debe realizar los pasos siguientes.

- [Creación de las zonas secundarias](#create-the-secondary-zones)
- [Configuración de la transferencia de zona en la zona principal](#configure-the-zone-transfer-settings-on-the-primary-zone)
- [Copiar las subredes de cliente DNS](#copy-the-dns-client-subnets)
- [Crear los ámbitos de zona en el servidor secundario](#create-the-zone-scopes-on-the-secondary-server)
- [Configuración de la Directiva de DNS](#configure-dns-policy)

En las secciones siguientes se proporcionan instrucciones de configuración detalladas.

> [!IMPORTANT]
> En las secciones siguientes se incluyen comandos de Windows PowerShell de ejemplo que contienen valores de ejemplo para muchos parámetros. Asegúrese de reemplazar los valores de ejemplo de estos comandos por los valores adecuados para su implementación antes de ejecutar estos comandos.
>
> La pertenencia a **DnsAdmins**, o equivalente, es necesaria para realizar los siguientes procedimientos.

### <a name="create-the-secondary-zones"></a>Creación de las zonas secundarias

Puede crear la copia secundaria de la zona que quiere replicar en SecondaryServer1 y SecondaryServer2 (suponiendo que los cmdlets se ejecuten de forma remota desde un único cliente de administración).

Por ejemplo, puede crear la copia secundaria de www.woodgrove.com en SecondaryServer1 y SecondarySesrver2.

Puede usar los siguientes comandos de Windows PowerShell para crear las zonas secundarias.

```powershell
Add-DnsServerSecondaryZone -Name "woodgrove.com" -ZoneFile "woodgrove.com.dns" -MasterServers 10.0.0.1 -ComputerName SecondaryServer1
Add-DnsServerSecondaryZone -Name "woodgrove.com" -ZoneFile "woodgrove.com.dns" -MasterServers 10.0.0.1 -ComputerName SecondaryServer2
```

Para obtener más información, consulte [Add-DnsServerSecondaryZone](/powershell/module/dnsserver/add-dnsserversecondaryzone).

### <a name="configure-the-zone-transfer-settings-on-the-primary-zone"></a>Configuración de la transferencia de zona en la zona principal

Debe configurar las opciones de la zona principal para que:

1. Se permiten las transferencias de zona desde el servidor principal a los servidores secundarios especificados.
2. El servidor principal envía notificaciones de actualización de zona a los servidores secundarios.

Puede usar los siguientes comandos de Windows PowerShell para configurar las opciones de transferencia de zona en la zona principal.

> [!NOTE]
> En el siguiente comando de ejemplo, el parámetro **-Notify** especifica que el servidor principal enviará notificaciones sobre las actualizaciones a la lista de selección de las secundarias.

```powershell
Set-DnsServerPrimaryZone -Name "woodgrove.com" -Notify Notify -SecondaryServers "10.0.0.2,10.0.0.3" -SecureSecondaries TransferToSecureServers -ComputerName PrimaryServer
```

Para obtener más información, vea [set-DnsServerPrimaryZone](/powershell/module/dnsserver/set-dnsserverprimaryzone).

### <a name="copy-the-dns-client-subnets"></a>Copiar las subredes de cliente DNS

Debe copiar las subredes de cliente DNS del servidor principal a los servidores secundarios.

Puede usar los siguientes comandos de Windows PowerShell para copiar las subredes en los servidores secundarios.

```powershell
Get-DnsServerClientSubnet -ComputerName PrimaryServer | Add-DnsServerClientSubnet -ComputerName SecondaryServer1
Get-DnsServerClientSubnet -ComputerName PrimaryServer | Add-DnsServerClientSubnet -ComputerName SecondaryServer2
```

Para obtener más información, consulte [Add-DnsServerClientSubnet](/powershell/module/dnsserver/add-dnsserverclientsubnet).

### <a name="create-the-zone-scopes-on-the-secondary-server"></a>Crear los ámbitos de zona en el servidor secundario

Debe crear los ámbitos de zona en los servidores secundarios. En DNS, los ámbitos de zona también inician la solicitud de XFRs desde el servidor principal. Con cualquier cambio en los ámbitos de zona en el servidor principal, se envía una notificación que contiene la información de ámbito de zona a los servidores secundarios. Los servidores secundarios pueden actualizar sus ámbitos de zona con cambios incrementales.

Puede usar los siguientes comandos de Windows PowerShell para crear los ámbitos de zona en los servidores secundarios.

```powershell
Get-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName PrimaryServer|Add-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName SecondaryServer1 -ErrorAction Ignore
Get-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName PrimaryServer|Add-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName SecondaryServer2 -ErrorAction Ignore
```

> [!NOTE]
> En estos comandos de ejemplo, se incluye el parámetro **-ErrorAction ignore** , porque existe un ámbito de zona predeterminado en cada zona. No se puede crear o eliminar el ámbito de zona predeterminado. La canalización producirá un intento de crear ese ámbito y se producirá un error. Como alternativa, puede crear los ámbitos de zona no predeterminados en dos zonas secundarias.

Para obtener más información, consulte [Add-DnsServerZoneScope](/powershell/module/dnsserver/add-dnsserverzonescope).

### <a name="configure-dns-policy"></a>Configuración de la Directiva de DNS

Después de crear las subredes, las particiones (ámbitos de zona) y los registros agregados, debe crear directivas que conecten las subredes y las particiones, de modo que cuando una consulta provenga de un origen en una de las subredes de cliente DNS, la respuesta de la consulta se devuelva desde el ámbito correcto de la zona. No se requieren directivas para asignar el ámbito de zona predeterminado.

Puede usar los siguientes comandos de Windows PowerShell para crear una directiva DNS que vincule las subredes de cliente DNS y los ámbitos de zona.

```powershell
$policy = Get-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName PrimaryServer
$policy | Add-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName SecondaryServer1
$policy | Add-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName SecondaryServer2
```

Para obtener más información, consulte [Add-DnsServerQueryResolutionPolicy](/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy).

Ahora los servidores DNS secundarios se configuran con las directivas DNS necesarias para redirigir el tráfico en función de la ubicación geográfica.

Cuando el servidor DNS recibe consultas de resolución de nombres, el servidor DNS evalúa los campos de la solicitud DNS con las directivas DNS configuradas. Si la dirección IP de origen de la solicitud de resolución de nombres coincide con alguna de las directivas, el ámbito de la zona asociada se usa para responder a la consulta y se dirige al usuario al recurso que está geográficamente más cercano.

Puede crear miles de directivas DNS según los requisitos de administración del tráfico y todas las directivas nuevas se aplican de forma dinámica, sin necesidad de reiniciar el servidor DNS, en las consultas entrantes.
