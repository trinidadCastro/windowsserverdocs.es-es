---
title: Uso de la directiva de DNS para la implementación de DNS de cerebro dividido
description: Este tema forma parte de la guía del escenario de la Directiva DNS para Windows Server 2016
manager: brianlic
ms.topic: article
ms.assetid: a255a4a5-c1a0-4edc-b41a-211bae397e3c
ms.author: lizross
author: eross-msft
ms.openlocfilehash: e8b19df2313bd0f3f6599aae8a23a18233f469e7
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766928"
---
# <a name="use-dns-policy-for-split-brain-dns-deployment"></a>Uso de la Directiva de DNS para la \- implementación de DNS de cerebro dividido

>Se aplica a: Windows Server 2016

Puede usar este tema para obtener información sobre cómo configurar la Directiva de DNS en Windows Server &reg; 2016 para implementaciones de DNS de cerebro dividido, en las que hay dos versiones de una sola zona: una para los usuarios internos de la intranet de la organización y otra para los usuarios externos, que suelen ser usuarios en Internet.

>[!NOTE]
>Para obtener información sobre cómo usar la Directiva de DNS para la \- implementación de DNS de cerebro dividido con Active Directory zonas DNS integrada, consulte [uso de la Directiva de DNS para DNS de cerebro dividido en Active Directory](dns-sb-with-ad.md).

Anteriormente, este escenario requería que los administradores de DNS mantengan dos servidores DNS diferentes, cada uno de los cuales proporciona servicios a cada conjunto de usuarios, tanto internos como externos. Si solo algunos registros dentro de la zona se han dividido \- de forma cerebro o ambas instancias de la zona (interna y externa) se han delegado en el mismo dominio primario, se convirtió en un dilema de administración.

Otro escenario de configuración para la implementación de división de cerebro es el control de recursividad selectiva para la resolución de nombres DNS. En algunas circunstancias, se espera que los servidores DNS empresariales realicen una resolución recursiva a través de Internet para los usuarios internos, mientras que también deben actuar como servidores de nombres autoritativos para los usuarios externos y bloquear la recursividad para ellos.

En este tema se incluyen las siguientes secciones.

- [Ejemplo de implementación de división de cerebro de DNS](#bkmk_sbexample)
- [Ejemplo de control de recursividad selectiva de DNS](#bkmk_recursion)

## <a name="example-of-dns-split-brain-deployment"></a><a name="bkmk_sbexample"></a>Ejemplo de implementación de división de cerebro de DNS
El siguiente es un ejemplo de cómo se puede usar la Directiva de DNS para llevar a cabo el escenario descrito anteriormente de DNS de cerebro dividido.

Esta sección contiene los temas siguientes.

- [Cómo funciona la implementación de división de cerebro de DNS](#bkmk_sbhow)
- [Configuración de la implementación de división de cerebro de DNS](#bkmk_sbconfigure)

En este ejemplo se usa una empresa ficticia, Contoso, que mantiene un sitio web de carrera en www.career.contoso.com.

El sitio tiene dos versiones, una para los usuarios internos donde están disponibles los envíos de trabajos internos. Este sitio interno está disponible en la dirección IP local 10.0.0.39.

La segunda versión es la versión pública del mismo sitio, que está disponible en la dirección IP pública 65.55.39.10.

En ausencia de la Directiva de DNS, el administrador debe hospedar estas dos zonas en servidores DNS de Windows Server independientes y administrarlas por separado.

Uso de directivas de DNS estas zonas ahora se pueden hospedar en el mismo servidor DNS.

En la ilustración siguiente se muestra este escenario.

![Implementación de DNS de cerebro dividido](../../media/DNS-Split-Brain/Dns-Split-Brain-01.jpg)

## <a name="how-dns-split-brain-deployment-works"></a><a name="bkmk_sbhow"></a>Cómo funciona la implementación de división de cerebro de DNS

Cuando el servidor DNS está configurado con las directivas DNS necesarias, cada solicitud de resolución de nombres se evalúa con respecto a las directivas del servidor DNS.

La interfaz de servidor se usa en este ejemplo como criterio para diferenciar entre los clientes internos y externos.

Si la interfaz de servidor en la que se recibe la consulta coincide con cualquiera de las directivas, el ámbito de la zona asociada se utiliza para responder a la consulta.

Por lo tanto, en nuestro ejemplo, las consultas de DNS para www.career.contoso.com que se reciben en la IP privada (10.0.0.56) reciben una respuesta DNS que contiene una dirección IP interna. y las consultas DNS que se reciben en la interfaz de red pública reciben una respuesta DNS que contiene la dirección IP pública en el ámbito de zona predeterminado (esto es lo mismo que la resolución de consultas normal).

## <a name="how-to-configure-dns-split-brain-deployment"></a><a name="bkmk_sbconfigure"></a>Configuración de la implementación de división de cerebro de DNS
Para configurar la implementación de un cerebro dividido en DNS mediante la Directiva DNS, debe seguir estos pasos.

- [Crear los ámbitos de zona](#bkmk_zscopes)
- [Agregar registros a los ámbitos de zona](#bkmk_records)
- [Crear las directivas de DNS](#bkmk_policies)

En las secciones siguientes se proporcionan instrucciones de configuración detalladas.

>[!IMPORTANT]
>En las secciones siguientes se incluyen comandos de Windows PowerShell de ejemplo que contienen valores de ejemplo para muchos parámetros. Asegúrese de reemplazar los valores de ejemplo de estos comandos por los valores adecuados para su implementación antes de ejecutar estos comandos.

### <a name="create-the-zone-scopes"></a><a name="bkmk_zscopes"></a>Crear los ámbitos de zona

Un ámbito de zona es una instancia única de la zona. Una zona DNS puede tener varios ámbitos de zona, con cada ámbito de zona que contenga su propio conjunto de registros DNS. El mismo registro puede estar presente en varios ámbitos, con direcciones IP diferentes o con las mismas direcciones IP.

> [!NOTE]
> De forma predeterminada, existe un ámbito de zona en las zonas DNS. Este ámbito de zona tiene el mismo nombre que la zona y las operaciones DNS heredadas funcionan en este ámbito. Este ámbito de zona predeterminado hospedará la versión externa de www.career.contoso.com.

Puede usar el siguiente comando de ejemplo para particionar el ámbito de zona contoso.com para crear un ámbito de zona interna. El ámbito de la zona interna se usará para mantener la versión interna de www.career.contoso.com.

`Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "internal"`

Para obtener más información, consulte [Add-DnsServerZoneScope](/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

### <a name="add-records-to-the-zone-scopes"></a><a name="bkmk_records"></a>Agregar registros a los ámbitos de zona

El siguiente paso consiste en agregar los registros que representan el host del servidor Web en los dos ámbitos de zona: interno y predeterminado (para clientes externos).

En el ámbito de la zona interna, se agrega el registro <strong>www.Career.contoso.com</strong> con la dirección IP 10.0.0.39, que es una IP privada. y en el ámbito de zona predeterminado se agrega el mismo registro, <strong>www.Career.contoso.com</strong>, con la dirección IP 65.55.39.10.

No se proporciona el parámetro **– ZoneScope** en los siguientes comandos de ejemplo cuando el registro se agrega al ámbito de zona predeterminado. Esto es similar a agregar registros a una zona vainilla.

`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10"
`
`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39” -ZoneScope "internal"
`

Para obtener más información, consulte [Add-DnsServerResourceRecord](/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).

### <a name="create-the-dns-policies"></a><a name="bkmk_policies"></a>Crear las directivas de DNS

Una vez que haya identificado las interfaces de servidor para la red externa y la red interna y haya creado los ámbitos de zona, debe crear directivas DNS que conecten los ámbitos de zona externa y interna.

>[!NOTE]
>En este ejemplo se usa la interfaz de servidor como criterio para diferenciar entre los clientes internos y externos. Otro método para diferenciar entre clientes externos e internos es mediante el uso de subredes de cliente como criterio. Si puede identificar las subredes a las que pertenecen los clientes internos, puede configurar la Directiva de DNS para diferenciar en función de la subred de cliente. Para obtener información sobre cómo configurar la administración del tráfico mediante los criterios de subred de cliente, consulte [uso de la Directiva de DNS para la administración del tráfico basado en la ubicación geográfica con los servidores principales](./primary-geo-location.md).

Cuando el servidor DNS recibe una consulta en la interfaz privada, el ámbito de la zona interna devuelve la respuesta de la consulta DNS.

>[!NOTE]
>No se requieren directivas para asignar el ámbito de zona predeterminado.

En el siguiente comando de ejemplo, 10.0.0.56 es la dirección IP de la interfaz de red privada, tal como se muestra en la ilustración anterior.

`Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,10.0.0.56" -ZoneScope "internal,1" -ZoneName contoso.com`

Para obtener más información, consulte [Add-DnsServerQueryResolutionPolicy](/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).

## <a name="example-of-dns-selective-recursion-control"></a><a name="bkmk_recursion"></a>Ejemplo de control de recursividad selectiva de DNS

A continuación se expone un ejemplo de cómo se puede usar la Directiva de DNS para llevar a cabo el escenario descrito previamente del control de recursividad selectiva de DNS.

Esta sección contiene los temas siguientes.

- [Cómo funciona el control de recursividad selectiva de DNS](#bkmk_recursionhow)
- [Cómo configurar el control de recursividad selectiva de DNS](#bkmk_recursionconfigure)

En este ejemplo se utiliza la misma compañía ficticia que en el ejemplo anterior, Contoso, que mantiene un sitio web de carrera en www.career.contoso.com.

En el ejemplo de implementación de un cerebro dividido en DNS, el mismo servidor DNS responde a los clientes externos e internos y les proporciona respuestas diferentes.

Es posible que algunas implementaciones de DNS requieran que el mismo servidor DNS realice la resolución recursiva de nombres para los clientes internos, además de actuar como el servidor de nombres autoritativo para los clientes externos. Esta circunstancia se denomina control de recursividad selectiva de DNS.

En versiones anteriores de Windows Server, la habilitación de la recursividad significaba que se habilitaba en todo el servidor DNS para todas las zonas. Dado que el servidor DNS está escuchando también consultas externas, la recursividad está habilitada para los clientes internos y externos, lo que convierte al servidor DNS en un solucionador abierto.

Un servidor DNS que está configurado como un solucionador abierto puede ser vulnerable al agotamiento de recursos y puede ser atacado por clientes malintencionados para crear ataques de reflexión.

Por este motivo, los administradores de DNS de Contoso no quieren que el servidor DNS de contoso.com realice la resolución de nombres recursivo para los clientes externos. Solo es necesario el control de recursividad para los clientes internos, mientras que el control de recursividad puede bloquearse para clientes externos.

En la ilustración siguiente se muestra este escenario.

![Control de recursividad selectivo](../../media/DNS-Split-Brain/Dns-Split-Brain-02.jpg)


### <a name="how-dns-selective-recursion-control-works"></a><a name="bkmk_recursionhow"></a>Cómo funciona el control de recursividad selectiva de DNS

Si se recibe una consulta para la que el servidor DNS de Contoso no es autoritativo, como para https://www.microsoft.com , la solicitud de resolución de nombres se evalúa con respecto a las directivas del servidor DNS.

Dado que estas consultas no se encuentran en ninguna zona, las directivas de nivel de zona \( tal y como se definen en el ejemplo de división-cerebro \) no se evalúan.

El servidor DNS evalúa las directivas de recursividad y las consultas que se reciben en la interfaz privada coinciden con el **SplitBrainRecursionPolicy**. Esta directiva apunta a un ámbito de recursividad en el que se habilita la recursividad.

A continuación, el servidor DNS realiza la recursividad para obtener la respuesta de https://www.microsoft.com Internet y almacena en caché la respuesta de forma local.

Si la consulta se recibe en la interfaz externa, no coinciden las directivas DNS y se aplica la configuración de recursividad predeterminada, que en este caso está **deshabilitada** .

Esto evita que el servidor actúe como un solucionador abierto para clientes externos, mientras actúa como un solucionador de almacenamiento en caché para los clientes internos.

### <a name="how-to-configure-dns-selective-recursion-control"></a><a name="bkmk_recursionconfigure"></a>Cómo configurar el control de recursividad selectiva de DNS

Para configurar el control de recursividad selectiva de DNS mediante la Directiva DNS, debe seguir estos pasos.

- [Crear ámbitos de recursividad de DNS](#bkmk_recscopes)
- [Crear directivas de recursividad de DNS](#bkmk_recpolicy)

#### <a name="create-dns-recursion-scopes"></a><a name="bkmk_recscopes"></a>Crear ámbitos de recursividad de DNS

Los ámbitos de recursividad son instancias únicas de un grupo de valores de configuración que controlan la recursividad en un servidor DNS. Un ámbito de recursividad contiene una lista de reenviadores y especifica si está habilitada la recursividad. Un servidor DNS puede tener muchos ámbitos de recursividad.

La configuración de recursividad heredada y la lista de reenviadores se conocen como el ámbito de recursividad predeterminado. No se puede agregar ni quitar el ámbito de recursividad predeterminado, identificado por el nombre punto \( "." \) .

En este ejemplo, la configuración de recursividad predeterminada está deshabilitada, mientras que se crea un nuevo ámbito de recursividad para clientes internos donde se habilita la recursividad.

```powershell
Set-DnsServerRecursionScope -Name . -EnableRecursion $False
Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True
```

Para obtener más información, consulte [Add-DnsServerRecursionScope](/powershell/module/dnsserver/add-dnsserverrecursionscope?view=win10-ps)

#### <a name="create-dns-recursion-policies"></a><a name="bkmk_recpolicy"></a>Crear directivas de recursividad de DNS

Puede crear directivas de recursividad de servidor DNS para elegir un ámbito de recursividad para un conjunto de consultas que coincidan con criterios específicos.

Si el servidor DNS no es autoritativo para algunas consultas, las directivas de recursividad del servidor DNS le permiten controlar cómo resolver las consultas.

En este ejemplo, el ámbito de recursividad interno con la recursividad habilitada está asociado a la interfaz de red privada.

Puede usar el siguiente comando de ejemplo para configurar directivas de recursividad de DNS.

```powershell
Add-DnsServerQueryResolutionPolicy -Name "SplitBrainRecursionPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP "EQ,10.0.0.39"
```

Para obtener más información, consulte [Add-DnsServerQueryResolutionPolicy](/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).

Ahora, el servidor DNS está configurado con las directivas DNS necesarias para un servidor de nombres de cerebro dividido o un servidor DNS con el control de recursividad selectivo habilitado para los clientes internos.

Puede crear miles de directivas DNS según los requisitos de administración del tráfico y todas las directivas nuevas se aplican de forma dinámica, sin necesidad de reiniciar el servidor DNS, en las consultas entrantes.

Para obtener más información, consulte la guía del escenario de la [Directiva DNS](DNS-Policy-Scenario-Guide.md).