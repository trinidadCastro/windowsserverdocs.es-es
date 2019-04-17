---
title: Usar la directiva de DNS para la aplicación de filtros en las consultas DNS
description: En este tema es parte de la DNS directiva escenario guía para Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: b86beeac-b0bb-4373-b462-ad6fa6cbedfa
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ff10af5f88a03f806a5e5b5fa698c7c3637816bd
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-applying-filters-on-dns-queries"></a>Usar la directiva de DNS para la aplicación de filtros en las consultas DNS

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener información sobre cómo configurar la directiva de DNS en Windows Server&reg; 2016 para crear filtros de consulta que se basan en los criterios que proporciones. 

Filtros de consultas en la directiva de DNS te permiten configurar el servidor DNS para responder de manera personalizada, en función de la consulta DNS y el cliente DNS que envía la consulta DNS.

Por ejemplo, puede configurar la directiva DNS con el filtro de consulta lista de bloqueados que bloquea las consultas DNS de dominios malintencionados conocidos, lo que impide que responder a las consultas de estos dominios DNS. Dado que se envía ninguna respuesta desde el servidor DNS, la consulta DNS de miembro de dominio malintencionado se apague.

Otro ejemplo es crear un filtro de consulta lista en la que se permite solo un conjunto específico de los clientes a resolver algunos nombres.

## <a name="bkmk_criteria"></a>Criterios de filtro de consulta
Puedes crear filtros de consultas con cualquier combinación lógica (u o no) de los siguientes criterios.

|Nombre|Descripción|
|-----------------|---------------------|
|Subred del cliente|Nombre de una subred cliente predefinidos. Se utiliza para comprobar la subred desde el que se envió la consulta.|
|Protocolo de transporte|Transporte Protocolo usado en la consulta. Valores posibles son UDP y TCP.|
|Protocolo de Internet|Protocolo de red que se usa en la consulta. Valores posibles son direcciones IPv4 e IPv6.|
|Dirección IP de la interfaz del servidor|Dirección IP de la interfaz de red del servidor DNS que recibió la solicitud DNS|
|FQDN|Nombre de dominio completo de registro de la consulta, con la posibilidad de utilizar un carácter comodín.|
|Tipo de consulta|Tipo de registro que se está consultando \ (A, SRV, TXT, etcetera. \)|
|Hora del día|Hora del día que se recibe la consulta.|

Los siguientes ejemplos muestran cómo crear filtros para la directiva DNS bloquear o permitir que las consultas de resolución de nombre DNS.

>[!NOTE]
>Los comandos de ejemplo en este tema usan el comando de Windows PowerShell **agregar DnsServerQueryResolutionPolicy**. Para obtener más información, consulta [agregar DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx). 

##<a name="bkmk_block1"></a>Consultas de bloqueo de un dominio

En algunos casos es posible que desea bloquear la resolución de nombres DNS para dominios que has identificado como malintencionado, o para dominios que no cumplen con las directrices de uso de la organización. Puedes realizar consultas de bloqueo de dominios mediante la directiva DNS.

La directiva que configurar en este ejemplo no se crea en una zona determinada: en su lugar crear una directiva de nivel de servidor que se aplica a todas las zonas configuradas en el servidor DNS. Las directivas de nivel de servidor son los primeros en evaluarse y, por consiguiente, primero hallar cuando una consulta se ha recibido por el servidor DNS.

El siguiente comando de ejemplo configura una directiva de nivel de servidor para bloquear todas las consultas con el dominio **sufijo contosomalicious.com**.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicy" -Action IGNORE -FQDN "EQ,*.contosomalicious.com" -PassThru 
`

>[!NOTE]
>Cuando se configura la **acción** parámetro con el valor **Ignorar**, el servidor DNS está configurado para colocar consultas sin respuesta en absoluto. Esto hace que el cliente DNS del dominio malintencionado en tiempo de espera.

##<a name="bkmk_block2"></a>Consultas de bloque de una subred
Con este ejemplo, puedes bloquear las consultas de una subred si se encuentra a infectarán con malware e intenta ponerse en contacto con los sitios malintencionados con el servidor DNS. 

' DnsServerClientSubnet agregar-el nombre "MaliciousSubnet06"-IPv4Subnet 172.0.33.0/24 - paso

DnsServerQueryResolutionPolicy agregar-el nombre "BlockListPolicyMalicious06"-acción ignorar - ClientSubnet "EQ MaliciousSubnet06" - paso '

El siguiente ejemplo muestra cómo puedes usar los criterios de subred en combinación con los criterios FQDN para bloquear las consultas de determinados dominios malintencionados de subredes infectadas.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06" –FQDN “EQ,*.contosomalicious.com” -PassThru
`

##<a name="bkmk_block3"></a>Bloquear un tipo de consulta
Es posible que debas bloquear la resolución de nombres para determinados tipos de consultas en los servidores. Por ejemplo, puede bloquear la consulta 'ANY', que puede usarse de forma malintencionada para crear amplificación ataques.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyQType" -Action IGNORE -QType "EQ,ANY" -PassThru
`

##<a name="bkmk_allow1"></a>Permitir consultas solo desde un dominio
Solo no puedes usar la directiva DNS para bloquear las consultas, se puede utilizar para aprobar automáticamente consultas de dominios específicos o subredes. Cuando configuras permitir listas, el servidor DNS procesa solo las consultas de dominios permitidos, bloqueando todas las demás solicitudes de otros dominios.

El siguiente comando de ejemplo permite solo equipos y dispositivos en el dominio contoso.com y secundarios para consultar el servidor DNS.

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicyDomain" -Action IGNORE -FQDN "NE,*.contoso.com" -PassThru 
`

##<a name="bkmk_allow2"></a>Permitir consultas solo de una subred
También puedes crear listas Permitir para las subredes IP, para que se omiten todas las consultas no originados por estas subredes.

`
Add-DnsServerClientSubnet -Name "AllowedSubnet06" -IPv4Subnet 172.0.33.0/24 -PassThru
`
`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicySubnet” -Action IGNORE -ClientSubnet  "NE, AllowedSubnet06" -PassThru
`

##<a name="bkmk_allow3"></a>Permitir que solo determinados QTypes
Puedes aplicar permitir que se enumeran a QTYPEs. 

Por ejemplo, si tienes que los clientes externos consultar la interfaz del servidor DNS 164.8.1.1, solo determinados QTYPEs pueden consultarse, aunque hay otros QTYPEs como registros SRV o TXT que se usan los servidores internos para la resolución de nombres o para fines de supervisión.

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListQType" -Action IGNORE -QType "NE,A,AAAA,MX,NS,SOA" –ServerInterface “EQ,164.8.1.1” -PassThru
`

Puede crear miles de directivas DNS según el tráfico de requisitos de administración y todas las nuevas directivas se aplican de forma dinámica - sin reiniciar el servidor DNS, en las consultas entrantes. 
