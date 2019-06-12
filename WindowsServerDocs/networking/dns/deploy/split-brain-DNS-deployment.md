---
title: Uso de la directiva DNS para la implementación de DNS de cerebro dividido
description: En este tema forma parte de las DNS directiva escenario guía para Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: a255a4a5-c1a0-4edc-b41a-211bae397e3c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c74bb2ee2f1647716c8c38e392434a5b7f01805f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446392"
---
# <a name="use-dns-policy-for-split-brain-dns-deployment"></a>Usar la directiva de DNS para la división\-cerebral la implementación de DNS

>Se aplica a: Windows Server 2016

Puede usar este tema para aprender a configurar la directiva DNS en Windows Server&reg; 2016 para las implementaciones DNS de cerebro dividido, donde hay dos versiones de una sola zona: uno para los usuarios internos de la intranet de la organización y otro para los usuarios externos, Normalmente, que son usuarios de Internet.

>[!NOTE]
>Para obtener información sobre cómo usar la directiva de DNS para la división\-zonas DNS integradas en el cerebro implementación de DNS con Active Directory, vea [usar Directiva de DNS para DNS Split-Brain en Active Directory](dns-sb-with-ad.md).

Anteriormente, este escenario requiere que los administradores de DNS mantengan dos servidores DNS diferentes, cada que proporcionan servicios a cada conjunto de usuarios internos y externos. Si solo unos pocos registros dentro de la zona se dividieron\-brained o ambas instancias de la zona (interna y externa) se delega al mismo dominio primario, esto se convirtió en un dilema de administración. 

Otro escenario de configuración para la implementación de cerebro dividido es selectivo de recursividad de Control para la resolución DNS. En algunas circunstancias, los servidores DNS de la empresa se esperan para realizar la resolución recursiva a través de Internet para los usuarios internos, mientras que también deben actuar como servidores de nombres autoritativos para los usuarios externos y bloquear la recursividad para ellos. 

En este tema se incluyen las siguientes secciones.

- [Ejemplo de implementación de cerebro dividida de DNS](#bkmk_sbexample)
- [Ejemplo de Control de recursividad selectiva de DNS](#bkmk_recursion)

## <a name="bkmk_sbexample"></a>Ejemplo de implementación de cerebro dividida de DNS
La siguiente es un ejemplo de cómo se puede usar la directiva DNS para lograr el escenario descrito anteriormente de DNS de cerebro dividido.

Esta sección contiene los temas siguientes.

- [Cómo funciona la implementación de cerebro dividida de DNS](#bkmk_sbhow)
- [Cómo configurar la implementación de cerebro dividida de DNS](#bkmk_sbconfigure)


Este ejemplo usa una compañía ficticia, Contoso, que mantiene un sitio Web de carrera en www.career.contoso.com.

El sitio tiene dos versiones, uno para los usuarios internos que están disponibles los registros de trabajo interno. Este sitio interno está disponible en la dirección IP local 10.0.0.39. 

La segunda versión es la versión pública del mismo sitio, que está disponible en la dirección IP pública 65.55.39.10.

En ausencia de la directiva de DNS, es necesario el administrador para hospedar estos dos zonas en los servidores DNS de Windows Server independientes y administrarlos por separado. 

Mediante las directivas DNS estas zonas ahora se pueden hospedar en el mismo servidor DNS.  

La siguiente ilustración muestra este escenario.

![Implementación de DNS de cerebro dividido](../../media/DNS-Split-Brain/Dns-Split-Brain-01.jpg)  


## <a name="bkmk_sbhow"></a>Cómo funciona la implementación de cerebro dividida de DNS

Cuando el servidor DNS está configurado con las directivas DNS necesarias, cada solicitud de resolución de nombres se evalúa con las directivas en el servidor DNS.

El servidor de la interfaz se usa en este ejemplo como criterio para diferenciar entre los clientes internos y externos.

Si la interfaz de servidor en el que se recibe la consulta coincide con cualquiera de las directivas, el ámbito de la zona asociada se usa para responder a la consulta. 

Por lo tanto, en nuestro ejemplo, las consultas DNS para www.career.contoso.com que se reciben en la dirección IP privada (10.0.0.56) recepción una respuesta DNS que contiene una dirección IP interna; y las consultas DNS que se reciben en la interfaz de red pública recibirán una respuesta DNS que contiene la dirección IP pública en el ámbito de la zona predeterminada (es igual que la resolución de consultas normal).  

## <a name="bkmk_sbconfigure"></a>Cómo configurar la implementación de cerebro dividida de DNS
Para configurar DNS Split-Brain implementación mediante la directiva de DNS, debe usar los pasos siguientes.

- [Creación de los ámbitos de zona](#bkmk_zscopes)  
- [Agregar registros a los ámbitos de zona](#bkmk_records)  
- [Cree las directivas DNS](#bkmk_policies)

Las secciones siguientes proporcionan instrucciones de configuración detallada.

>[!IMPORTANT]
>Las secciones siguientes incluyen ejemplos de comandos de Windows PowerShell que contienen valores de ejemplo para muchos parámetros. Asegúrese de sustituir los valores de ejemplo de estos comandos con los valores adecuados para su implementación antes de ejecutar estos comandos. 

### <a name="bkmk_zscopes"></a>Creación de los ámbitos de zona

Un ámbito de la zona es una instancia única de la zona. Una zona DNS puede tener varios ámbitos de zona, con cada ámbito de la zona que contiene su propio conjunto de registros DNS. El mismo registro puede estar presente en varios ámbitos, con direcciones IP diferentes o las mismas direcciones IP. 

> [!NOTE]
> De forma predeterminada, un ámbito de la zona existe en las zonas DNS. Este ámbito de la zona tiene el mismo nombre que la zona y las operaciones de DNS heredadas funcionan en este ámbito. Este ámbito de la zona predeterminada va a hospedar la versión de www.career.contoso.com externa.

Puede usar el siguiente comando de ejemplo para la partición en el ámbito de zona "contoso.com" para crear un ámbito de la zona interna. El ámbito de la zona interna se usará para mantener la versión interna de www.career.contoso.com.

`Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "internal"`

Para obtener más información, consulte [agregar DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

### <a name="bkmk_records"></a>Agregar registros a los ámbitos de zona

El siguiente paso es agregar los registros que representa el host del servidor Web en los ámbitos de dos zona - internos y predeterminado (para los clientes externos). 

En el ámbito de la zona interna, el registro <strong>www.career.contoso.com</strong> se agrega con la dirección IP 10.0.0.39, que es una dirección IP privada; y en el ámbito de la zona de forma predeterminada el mismo registro, <strong>www.career.contoso.com</strong>, es se agregan con la dirección IP 65.55.39.10.

No **– zonaámbito** parámetro se proporciona en los siguientes comandos de ejemplo cuando se agrega el registro para el ámbito de la zona predeterminada. Esto es similar a agregar registros a una zona vainilla.

`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10"
`
`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39” -ZoneScope "internal"
`

Para obtener más información, consulte [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).

### <a name="bkmk_policies"></a>Cree las directivas DNS

Una vez que haya identificado las interfaces de servidor para la red externa y la red interna y ha creado los ámbitos de zona, debe crear las directivas DNS que se conectan los ámbitos de la zona interna y externa.

>[!NOTE]
>Este ejemplo utiliza la interfaz de servidor como los criterios para diferenciar entre los clientes internos y externos. Otro método para diferenciar entre clientes internos y externos es mediante el uso de subredes de cliente como criterio. Si puede identificar las subredes a los que pertenecen los clientes internos, puede configurar la directiva de DNS para diferenciar en función de la subred de cliente. Para obtener información sobre cómo configurar la administración del tráfico con criterios de la subred de cliente, consulte [uso de directiva DNS para la administración de tráfico en función de la ubicación geográfica con servidores principales](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/scenario--use-dns-policy-for-geo-location-based-traffic-management-with-primary-servers).

Cuando el servidor DNS recibe una consulta en la interfaz privada, se devuelve la respuesta a consultas DNS desde el ámbito de la zona interna.

>[!NOTE]
>No hay directivas son necesarios para la asignación de ámbito de la zona predeterminada. 

En el siguiente comando de ejemplo, 10.0.0.56 es la dirección IP en la interfaz de red privada, como se muestra en la ilustración anterior.

`Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,10.0.0.56" -ZoneScope "internal,1" -ZoneName contoso.com`

Para obtener más información, consulte [agregar DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  


## <a name="bkmk_recursion"></a>Ejemplo de Control de recursividad selectiva de DNS

La siguiente es un ejemplo de cómo puede usar Directiva de DNS para lograr el escenario descrito anteriormente del control de recursividad selectiva de DNS.

Esta sección contiene los temas siguientes.

- [Cómo funciona el Control de recursividad selectivo DNS](#bkmk_recursionhow)
- [Cómo configurar el Control de recursividad selectiva de DNS](#bkmk_recursionconfigure)

Este ejemplo utiliza la misma empresa ficticia, como se muestra en el ejemplo anterior, Contoso, que mantiene un sitio Web de carrera en www.career.contoso.com.

En el ejemplo de implementación de cerebro dividido de DNS, el mismo servidor DNS responde a los clientes externos e internos y les ofrece respuestas diferentes. 

Algunas implementaciones de DNS pueden requerir el mismo servidor DNS para realizar la resolución recursiva de nombres para los clientes internos, además de actuar como el servidor DNS autoritativo para clientes externos. Esta circunstancia se denomina control de recursividad selectiva de DNS.

En versiones anteriores de Windows Server, lo que permite la recursividad significaba que se ha habilitado en todo el servidor DNS para todas las zonas. Debido a consultas externas también está escuchando el servidor DNS, la recursión está habilitada para los clientes internos y externos, hacer que al servidor DNS de un solucionador abierto. 

Un servidor DNS que está configurado como una resolución abierta podría ser vulnerable al agotamiento de recursos y puede ser usada por los clientes malintencionados para crear los ataques de reflexión. 

Por este motivo, los administradores de DNS de Contoso no desean que el servidor DNS para "contoso.com" realizar la resolución recursiva de nombres para los clientes externos. Es necesario solo para el control de recursividad para clientes internos, mientras que se puede bloquear el control de recursividad para clientes externos. 

La siguiente ilustración muestra este escenario.

![Control de recursividad selectivo](../../media/DNS-Split-Brain/Dns-Split-Brain-02.jpg) 


### <a name="bkmk_recursionhow"></a>Cómo funciona el Control de recursividad selectivo DNS

Si se recibe una consulta para el que el servidor DNS de Contoso es no autoritativa, como de www.microsoft.com, a continuación, se evalúa la solicitud de resolución de nombres con las directivas en el servidor DNS. 

Dado que estas consultas no entran en cualquier zona, la zona de las directivas de nivel \(tal como se define en el ejemplo de división de cerebro\) no se evalúan. 

El servidor DNS evalúa las directivas de recursividad y las consultas que se reciben en la coincidencia de la interfaz privada la **SplitBrainRecursionPolicy**. Esta directiva apunta a un ámbito de recursividad donde la recursión está habilitada.

El servidor DNS almacena en caché la respuesta localmente y, a continuación, realiza recursividad para obtener la respuesta de www.microsoft.com desde Internet. 

Si se recibe la consulta en la interfaz externa, ninguna coincidencia de las directivas DNS y el valor de repetición predeterminado - que en este caso es **deshabilitado** -se aplica.

Esto impide que el servidor que actúa como una resolución abierta para clientes externos, mientras está actuando como una resolución de caché para los clientes internos. 

### <a name="bkmk_recursionconfigure"></a>Cómo configurar el Control de recursividad selectiva de DNS

Para configurar el control de recursividad selectiva de DNS mediante la directiva de DNS, debe usar los pasos siguientes.

- [Crear ámbitos de recursividad DNS](#bkmk_recscopes)
- [Crear directivas de recursividad DNS](#bkmk_recpolicy)

#### <a name="bkmk_recscopes"></a>Crear ámbitos de recursividad DNS

Los ámbitos de recursividad son instancias únicas de un grupo de configuraciones que controlan la recursividad en un servidor DNS. Un ámbito de recursividad contiene una lista de reenviadores y especifica si está habilitada la recursividad. Un servidor DNS puede tener muchos ámbitos de recursividad. 

La configuración de recursividad heredado y una lista de reenviadores se conocen como el ámbito de repetición predeterminado. No se puede agregar o quitar el ámbito de recursividad de forma predeterminada, identificado por el punto de nombre \("." \).

En este ejemplo, el valor de repetición predeterminado está deshabilitado, mientras se crea un nuevo ámbito de recursividad para clientes internos, donde la recursión está habilitada.

    
    Set-DnsServerRecursionScope -Name . -EnableRecursion $False
    Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True 
    

Para obtener más información, consulte [agregar DnsServerRecursionScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverrecursionscope?view=win10-ps)

#### <a name="bkmk_recpolicy"></a>Crear directivas de recursividad DNS

Servidor DNS puede crear directivas de recursividad para elegir un ámbito de recursividad para un conjunto de consultas que cumplen determinados criterios. 

Si el servidor DNS no está autorizado para algunas consultas, las directivas de recursividad DNS server permiten controlar cómo resolver las consultas. 

En este ejemplo, el ámbito de recursividad interna con la recursividad habilitada está asociado con la interfaz de red privada.

Puede usar el siguiente comando de ejemplo para configurar las directivas de recursividad DNS.

    
    Add-DnsServerQueryResolutionPolicy -Name "SplitBrainRecursionPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP "EQ,10.0.0.39"
    

Para obtener más información, consulte [agregar DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).

Ahora el servidor DNS está configurado con las directivas DNS necesarias para un servidor de nombres de cerebro dividido o un servidor DNS con el control de recursividad selectivo habilitado para los clientes internos.

Puede crear miles de las directivas DNS según el tráfico de los requisitos de administración y todas las nuevas directivas se aplican dinámicamente - sin necesidad de reiniciar el servidor DNS, en las consultas entrantes. 

Para obtener más información, consulte [Guía del escenario de directiva DNS](DNS-Policy-Scenario-Guide.md).
