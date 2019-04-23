---
title: Uso de la directiva de DNS para aplicar filtros en las consultas DNS
description: En este tema forma parte de las DNS directiva escenario guía para Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: b86beeac-b0bb-4373-b462-ad6fa6cbedfa
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4b00c773462569f005a73f535b1a872ae7b389db
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859906"
---
# <a name="use-dns-policy-for-applying-filters-on-dns-queries"></a>Uso de la directiva de DNS para aplicar filtros en las consultas DNS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para aprender a configurar la directiva DNS en Windows Server&reg; 2016 para crear filtros de consulta que se basan en los criterios especificados. 

Filtros de consulta en la directiva DNS permiten configurar el servidor DNS para responder de manera personalizada basada en la consulta DNS y el cliente DNS que envía la consulta DNS.

Por ejemplo, puede configurar la directiva de DNS con el filtro de consulta lista de bloques que bloquea las consultas DNS de dominios malintencionados conocidos, lo que impide que responde a las consultas de estos dominios DNS. Dado que no hay respuesta se envía desde el servidor DNS, DNS consulta del miembro dominio malintencionado agote el tiempo.

Otro ejemplo consiste en crear un lista de permitidos que permite solo un conjunto específico de los clientes a resolver algunos nombres de filtro de consulta.

## <a name="bkmk_criteria"></a> Criterios de filtro de consulta
Puede crear filtros de consulta con cualquier combinación lógica (Y/O/no) de los siguientes criterios.

|Nombre|Descripción|
|-----------------|---------------------|
|Subred de cliente|Nombre de una subred de cliente predefinido. Se usa para comprobar la subred desde la que se envió la consulta.|
|Protocolo de transporte|Transporte de protocolo usado en la consulta. Los valores posibles son TCP y UDP.|
|Protocolo de Internet|Protocolo de red utilizado en la consulta. Los valores posibles son IPv4 e IPv6.|
|Dirección IP de interfaz del servidor|Dirección IP de la interfaz de red del servidor DNS que recibió la solicitud DNS.|
|FQDN|Nombre de dominio completo del registro en la consulta, con la posibilidad de usar un carácter comodín.|
|Tipo de consulta|Tipo de registro que se está consultando \(A, SRV, TXT, etc.\).|
|Hora del día|Hora del día en que se recibe la consulta.|

Los ejemplos siguientes muestran cómo crear filtros para la directiva DNS bloquear o permitir las consultas de resolución de nombres DNS.

>[!NOTE]
>Los comandos de ejemplo en este tema usan el comando de Windows PowerShell **agregar DnsServerQueryResolutionPolicy**. Para obtener más información, consulte [agregar DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps). 

##<a name="bkmk_block1"></a>Consultas de bloqueo de un dominio

En algunas circunstancias puede bloquear la resolución de nombres DNS para dominios que se han identificado como malintencionada o dominios que no son compatibles con las instrucciones de uso de su organización. Puede realizar consultas de bloqueo para dominios mediante la directiva DNS.

La directiva que se configura en este ejemplo no se crea en cualquier zona determinada, en su lugar cree una directiva de nivel de servidor que se aplica a todas las zonas configuradas en el servidor DNS. Directivas de nivel de servidor son los primeros en evaluar y, por tanto, primero debe coincidir cuando una consulta es recibido por el servidor DNS.

El siguiente comando de ejemplo configura una directiva de nivel de servidor para bloquear todas las consultas con el dominio **sufijo contosomalicious.com**.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicy" -Action IGNORE -FQDN "EQ,*.contosomalicious.com" -PassThru 
`

>[!NOTE]
>Al configurar el **acción** parámetro con el valor **omitir**, el servidor DNS está configurado para quitar las consultas sin respuesta en absoluto. Esto hace que al cliente DNS del dominio malintencionado en tiempo de espera.

##<a name="bkmk_block2"></a>Consultas de bloqueo de una subred
Con este ejemplo, puede bloquear las consultas de una subred si se encuentra infectados por malware y está intentando ponerse en contacto con los sitios malintencionados mediante su servidor DNS. 

` Add-DnsServerClientSubnet -Name "MaliciousSubnet06" -IPv4Subnet 172.0.33.0/24 -PassThru

DnsServerQueryResolutionPolicy Agregar-nombre "BlockListPolicyMalicious06"-acción Omitir - ClientSubnet "EQ, MaliciousSubnet06" - PassThru '

El ejemplo siguiente muestra cómo puede usar los criterios de la subred en combinación con los criterios FQDN para bloquea las consultas para ciertos dominios malintencionados desde subredes infectadas.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06" –FQDN “EQ,*.contosomalicious.com” -PassThru
`

##<a name="bkmk_block3"></a>Bloquear un tipo de consulta
Es posible que deba bloquear la resolución de nombres para ciertos tipos de consultas en los servidores. Por ejemplo, puede bloquear la consulta 'ANY', que se puede usar de forma malintencionada para crear ataques de amplificación.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyQType" -Action IGNORE -QType "EQ,ANY" -PassThru
`

##<a name="bkmk_allow1"></a>Permitir consultas solo desde un dominio
No puede usar solo de directiva para bloquear las consultas DNS, puede usar para aprobar automáticamente las consultas entre subredes o dominios específicos. Al configurar listas de permitir, el servidor DNS solo procesa las consultas de dominios permitidos, mientras se bloquea todas las consultas de otros dominios.

El siguiente comando de ejemplo permite que solo los equipos y dispositivos en los dominios contoso.com y secundario para consultar el servidor DNS.

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicyDomain" -Action IGNORE -FQDN "NE,*.contoso.com" -PassThru 
`

##<a name="bkmk_allow2"></a>Permitir consultas solo desde una subred
También puede crear listas de permitir para las subredes IP, para que se omiten todas las consultas no que se origina en estas subredes.

`
Add-DnsServerClientSubnet -Name "AllowedSubnet06" -IPv4Subnet 172.0.33.0/24 -PassThru
`
`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicySubnet” -Action IGNORE -ClientSubnet  "NE, AllowedSubnet06" -PassThru
`

##<a name="bkmk_allow3"></a>Permitir solo determinadas QTypes
Permitir que se muestran se puede aplicar a QTYPEs. 

Por ejemplo, si tiene clientes externos consultar la interfaz del servidor DNS 164.8.1.1, sólo ciertos QTYPEs se permiten consultar, aunque hay otros QTYPEs similares a los registros SRV o TXT que se usan los servidores internos para la resolución o con fines de supervisión.

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListQType" -Action IGNORE -QType "NE,A,AAAA,MX,NS,SOA" –ServerInterface “EQ,164.8.1.1” -PassThru
`

Puede crear miles de las directivas DNS según el tráfico de los requisitos de administración y todas las nuevas directivas se aplican dinámicamente - sin necesidad de reiniciar el servidor DNS, en las consultas entrantes. 
