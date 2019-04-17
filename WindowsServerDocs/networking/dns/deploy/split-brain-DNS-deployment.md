---
title: Usar la directiva de DNS para la implementación de DNS produzca
description: En este tema es parte de la DNS directiva escenario guía para Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: a255a4a5-c1a0-4edc-b41a-211bae397e3c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0b25ac752ea347f4d184628eb26bc7e297443306
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-split-brain-dns-deployment"></a>Usar la directiva de DNS para la implementación de DNS Split\ cerebro

>Se aplica a: Windows Server 2016

Puedes usar este tema para obtener información sobre cómo configurar la directiva de DNS en Windows Server&reg; 2016 para implementaciones de DNS produzca, donde hay dos versiones de una sola zona - uno de los usuarios internos de la intranet de la organización y otra para los usuarios externos, que suelen ser usuarios de Internet.

>[!NOTE]
>Para obtener información sobre cómo usar la directiva de DNS para zonas DNS integradas en la implementación de DNS split\ cerebro con Active Directory, consulte [usar la directiva de DNS para Split-Brain DNS en Active Directory](dns-sb-with-ad.md).

Anteriormente, en este escenario se requería que los administradores de DNS mantengan dos servidores DNS diferentes, cada proporcionar servicios a cada conjunto de usuarios, internos y externos. Si solo unos registros dentro de la zona estaban brained split\ o ambas instancias de la zona (interna y externa) se delegan a del mismo dominio principal, esto se convirtió en un dilema de administración. 

Otro escenario de configuración para la implementación produzca es selectiva de recursión de Control para la resolución de nombre DNS. En algunas circunstancias, se espera a que los servidores DNS de empresa para realizar resolución recursiva por Internet para los usuarios internos, mientras también debes actuar como servidores de nombres autorizados para los usuarios externos y bloquear recursividad para ellos. 

Este tema contiene las siguientes secciones.

- [Ejemplo de implementación de división del núcleo de DNS](#bkmk_sbexample)
- [Ejemplo de Control de recursión selectiva DNS](#bkmk_recursion)

##<a name="bkmk_sbexample"></a>Ejemplo de implementación de división del núcleo de DNS
Siguiente es un ejemplo de cómo puede usar la directiva DNS para realizar el escenario de DNS produzca se ha descrito anteriormente.

Esta sección contiene los siguientes temas.

- [Cómo funciona la implementación de división del núcleo de DNS](#bkmk_sbhow)
- [Cómo configurar la implementación de división del núcleo de DNS](#bkmk_sbconfigure)


Este ejemplo usa una empresa ficticia, Contoso, que mantiene un sitio Web de profesiones en www.career.contoso.com.

El sitio tiene dos versiones, uno para los usuarios internos que están disponibles de publicaciones de trabajo interno. Este sitio interno está disponible en la dirección IP local 10.0.0.39. 

La segunda versión es la versión pública del mismo sitio, que está disponible en la dirección IP pública 65.55.39.10.

Ante la ausencia de directiva DNS, el administrador debe hospedar estos dos zonas en servidores DNS de Windows Server independientes y se administran por separado. 

Uso de directivas de DNS estas zonas ahora se hospeden en el mismo servidor DNS.  

La ilustración siguiente muestra este escenario.

![Implementación de DNS produzca](../../media/DNS-Split-Brain/Dns-Split-Brain-01.jpg)  


##<a name="bkmk_sbhow"></a>Cómo funciona la implementación de división del núcleo de DNS

Cuando el servidor DNS está configurado con las directivas necesarias de DNS, se evalúa cada solicitud de resolución de nombres con las directivas en el servidor DNS.

El servidor interfaz se usa en este ejemplo como los criterios para diferenciar entre los clientes internos y externos.

Si la interfaz del servidor en el que se recibe la consulta coincide con ninguna de las directivas, se usa el ámbito de la zona asociada para responder a la consulta. 

Por lo tanto, en nuestro ejemplo, las consultas DNS para www.career.contoso.com que se reciben en el IP privada (10.0.0.56) reciban una respuesta DNS que contiene una dirección IP interna. y las consultas DNS que se reciben en la interfaz de red pública recibirán una respuesta DNS que contiene la dirección IP pública en el ámbito de la zona de predeterminado (Esto es lo mismo que la resolución de la consulta normal).  

##<a name="bkmk_sbconfigure"></a>Cómo configurar la implementación de división del núcleo de DNS
Para configurar la implementación de Split-Brain DNS mediante la directiva de DNS, debes usar los siguientes pasos.

- [Crear los ámbitos zona](#bkmk_zscopes)  
- [Agregar registros a los ámbitos zona](#bkmk_records)  
- [Crear las directivas DNS](#bkmk_policies)

Las siguientes secciones proporcionan instrucciones de configuración detallada.

>[!IMPORTANT]
>Las secciones siguientes incluyen ejemplos de comandos de Windows PowerShell que contienen los valores de ejemplo para el número de parámetros. Asegúrate de que los valores de ejemplo en estos comandos, se reemplaza con valores que son adecuados para su implementación antes de ejecutar estos comandos. 

###<a name="bkmk_zscopes"></a>Crear los ámbitos zona

Un ámbito de la zona es una instancia única de la zona. Una zona DNS puede tener varios ámbitos zona, con cada ámbito de la zona que contiene su propio conjunto de registros de DNS. El mismo registro puede encontrarse en varios ámbitos, con diferentes direcciones IP o las mismas direcciones IP. 

>[!NOTE]
>De manera predeterminada, existe un ámbito de la zona en las zonas DNS. El ámbito de esta zona tiene el mismo nombre que la zona y las operaciones de DNS heredadas funcionan en este ámbito. La versión externa de www.career.contoso.com hospede este ámbito de la zona de forma predeterminada.

Puedes usar el siguiente comando de ejemplo para particionar el contoso.com ámbito zona para crear un ámbito de la zona interna. El ámbito de la zona interna se usará para mantener la versión interna de www.career.contoso.com.

`Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "internal"`

Para obtener más información, consulta [agregar DnsServerZoneScope](https://technet.microsoft.com/library/mt126267.aspx)

###<a name="bkmk_records"></a>Agregar registros a los ámbitos zona

El siguiente paso es agregar los registros que representan el host del servidor Web en los ámbitos dos zona - internos y predeterminado (para los clientes externos). 

En el ámbito de la zona interna, el registro **www.career.contoso.com** se agrega con la dirección IP 10.0.0.39, que es una dirección IP privada; y en el ámbito de la zona de forma predeterminada el mismo registro, **www.career.contoso.com**, se agrega con la dirección IP 65.55.39.10.

No **: zonaámbito** parámetro se proporciona en los siguientes comandos de ejemplo, cuando se agrega el registro para el ámbito de la zona de forma predeterminada. Esto es similar a la adición de registros a una zona de vainilla.

`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10"
`
`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39” -ZoneScope "internal"
`

Para obtener más información, consulta [agregar DnsServerResourceRecord](https://technet.microsoft.com/library/jj649925.aspx).

###<a name="bkmk_policies"></a>Crear las directivas DNS

Después de haber identificado las interfaces de servidor de la red externa y la red interna y has creado los ámbitos de la zona, debes crear directivas DNS que conectan los ámbitos zona internas y externas.

>[!NOTE]
>Este ejemplo usa la interfaz del servidor como los criterios para diferenciar entre los clientes internos y externos. Otro método para diferenciar entre clientes internos y externos es mediante subredes del cliente como un criterio. Si puedes identificar las subredes a la que pertenecen los clientes internos, puedes configurar la directiva DNS para diferenciar en función de la subred del cliente. Para obtener información sobre cómo configurar la administración de tráfico con criterios de subred de cliente, consulta [usar la directiva de DNS para la administración de tráfico en función de ubicación geográfica con los servidores principal](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/scenario--use-dns-policy-for-geo-location-based-traffic-management-with-primary-servers).

Cuando el servidor DNS recibe una consulta en la interfaz privada, se devuelve la respuesta de la consulta DNS desde el ámbito de la zona interna.

>[!NOTE]
>No hay directivas son necesarias para asignar el ámbito de la zona de forma predeterminada. 

En el siguiente comando de ejemplo, 10.0.0.56 es la dirección IP en la interfaz de red privada, como se muestra en la ilustración anterior.

`Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,10.0.0.56" -ZoneScope "internal,1" -ZoneName contoso.com`

Para obtener más información, consulta [agregar DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx).  


## <a name="bkmk_recursion"></a>Ejemplo de Control de recursión selectiva DNS

Siguiente es un ejemplo de cómo puede usar la directiva DNS para realizar el escenario de DNS recursividad selectiva control se ha descrito anteriormente.

Esta sección contiene los siguientes temas.

- [¿Cómo funciona el Control de DNS recursividad selectiva](#bkmk_recursionhow)
- [Cómo configurar Control recursividad selectiva de DNS](#bkmk_recursionconfigure)

Este ejemplo usa la misma empresa ficticia del ejemplo anterior, Contoso, que mantiene un sitio Web de profesiones en www.career.contoso.com.

En el ejemplo de implementación produzca DNS, el mismo servidor DNS responde a los clientes internos y externos y les proporcionará respuestas diferentes. 

Algunas implementaciones de DNS pueden requerir el mismo servidor DNS para realizar la resolución de nombres recursiva para clientes internos además actúa como servidor de autorización de nombres para los clientes externos. Este caso se denomina control de recursión selectiva de DNS.

En versiones anteriores de Windows Server, habilitar recursividad había destinada a que estaba activado en todo el servidor DNS para todas las zonas. Porque el servidor DNS también está escuchando consultas externas, recursión está habilitada para los clientes internos y externos, hacer que al servidor DNS de una resolución abierta. 

Un servidor DNS que está configurado como una resolución abrir podría quedar expuesta a agotamiento de recursos y puede ser usada por los clientes malintencionados crear ataques de reflexión. 

Por este motivo, los administradores de DNS de Contoso no desean que el servidor DNS para contoso.com para llevar a cabo la resolución de nombres recursiva para los clientes externos. Hay solo una necesidad de control de recursión para clientes internos, mientras se puede bloquear el control de recursión para los clientes externos. 

La ilustración siguiente muestra este escenario.

![Control de recursividad selectiva](../../media/DNS-Split-Brain/Dns-Split-Brain-02.jpg) 


### <a name="bkmk_recursionhow"></a>¿Cómo funciona el Control de DNS recursividad selectiva

Si se recibe una consulta para que el servidor DNS de Contoso es no autorizados, como para www.microsoft.com, a continuación, la solicitud de resolución de nombres se evalúa las directivas en el servidor DNS. 

Porque estas consultas no entran en cualquier zona, la zona nivel directivas \ (según se define en la example\ produzca) no se evalúan. 

El servidor DNS evalúa las directivas de recursión y las consultas que se reciben en la coincidencia de interfaz privada la **SplitBrainRecursionPolicy**. Esta directiva apunta a un ámbito de recursión donde recursividad está habilitada.

El servidor DNS, a continuación, realiza recursividad para obtener la respuesta www.microsoft.com desde Internet y almacena en caché localmente la respuesta. 

Si se recibe la consulta en la interfaz externa, sin coincidencia de directivas DNS y la configuración de recursión predeterminada - que en este caso es **deshabilitado** -se aplica.

Esto impide que el servidor actúa como una resolución abierta para los clientes externos, mientras que actúa como una resolución de almacenamiento en caché para los clientes internos. 

### <a name="bkmk_recursionconfigure"></a>Cómo configurar Control recursividad selectiva de DNS

Para configurar el control de recursión selectiva de DNS mediante la directiva de DNS, debes usar los siguientes pasos.

- [Crear ámbitos de recursión DNS](#bkmk_recscopes)
- [Crear directivas de recursión DNS](#bkmk_recpolicy)

#### <a name="bkmk_recscopes"></a>Crear ámbitos de recursión DNS

Ámbitos de recursión son instancias únicas de un grupo de configuración que controla recursividad en un servidor DNS. Un ámbito de recursión contiene una lista de servidores de reenvío y especifica si está habilitada recursividad. Un servidor DNS puede tener muchos de los ámbitos de recursión. 

La lista de servidores de reenvío y configuración recursividad heredadas se conocen como el ámbito de recursión predeterminado. No se puede agregar o quitar el ámbito de recursión predeterminado, que se identifica mediante el punto de nombre \("." \).

En este ejemplo, se deshabilita la configuración de recursión predeterminada, mientras se crea un nuevo ámbito de recursión para los clientes internos donde recursividad está habilitada.

    
    Set-DnsServerRecursionScope -Name . -EnableRecursion $False
    Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True 
    

Para obtener más información, consulta [agregar DnsServerRecursionScope](https://technet.microsoft.com/library/mt126268.aspx)

#### <a name="bkmk_recpolicy"></a>Crear directivas de recursión DNS

Puedes crear servidor DNS recursividad directivas para elegir un ámbito de recursión para un conjunto de consultas que coincidan con criterios específicos. 

Si el servidor DNS no está autorizado para algunas consultas, las directivas de recursividad de servidor DNS te permiten controlar cómo resolver las consultas. 

En este ejemplo, el ámbito de recursión interna con recursividad habilitada es asociado con la interfaz de red privada

Puedes usar el siguiente comando de ejemplo para configurar las directivas de recursión DNS.

    
    Add-DnsServerQueryResolutionPolicy -Name "SplitBrainRecursionPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP  "EQ,10.0.0.39"
    

Para obtener más información, consulta [agregar DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx).

Ahora el servidor DNS está configurado con las directivas necesarias de DNS para un servidor de nombres produzca o un servidor DNS con el control de recursión selectiva habilitado para clientes internos.

Puede crear miles de directivas DNS según el tráfico de requisitos de administración y todas las nuevas directivas se aplican de forma dinámica - sin reiniciar el servidor DNS, en las consultas entrantes. 

Para obtener más información, consulta [DNS directiva escenario guía](DNS-Policy-Scenario-Guide.md).
