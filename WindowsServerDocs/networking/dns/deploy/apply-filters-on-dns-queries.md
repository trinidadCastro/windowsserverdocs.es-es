---
title: Uso de la directiva de DNS para aplicar filtros en las consultas DNS
description: Este tema forma parte de la guía del escenario de la Directiva DNS para Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: b86beeac-b0bb-4373-b462-ad6fa6cbedfa
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 95b68995326dc3d3bf48ca36caa9b2ab4923a7c3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406205"
---
# <a name="use-dns-policy-for-applying-filters-on-dns-queries"></a>Uso de la directiva de DNS para aplicar filtros en las consultas DNS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información sobre cómo configurar la Directiva de DNS en&reg; Windows Server 2016 para crear filtros de consulta basados en los criterios que proporcione. 

Los filtros de consulta de la Directiva de DNS permiten configurar el servidor DNS para responder de forma personalizada en función de la consulta DNS y el cliente DNS que envía la consulta DNS.

Por ejemplo, puede configurar la Directiva DNS con la lista de bloques de filtros de consulta que bloquea las consultas DNS de dominios malintencionados conocidos, lo que impide que DNS responda a las consultas de estos dominios. Dado que no se envía ninguna respuesta desde el servidor DNS, se agota el tiempo de espera de la consulta DNS del miembro de dominio malintencionado.

Otro ejemplo es crear una lista de permitidos de filtro de consulta que solo permita a un conjunto específico de clientes resolver determinados nombres.

## <a name="bkmk_criteria"></a>Criterios de filtro de consulta
Puede crear filtros de consulta con cualquier combinación lógica (y/o/no) de los criterios siguientes.

|Nombre|Descripción|
|-----------------|---------------------|
|Subred de cliente|Nombre de una subred de cliente predefinida. Se utiliza para comprobar la subred desde la que se envió la consulta.|
|Protocolo de transporte|Protocolo de transporte utilizado en la consulta. Los valores posibles son UDP y TCP.|
|Protocolo de Internet|Protocolo de red utilizado en la consulta. Los valores posibles son IPv4 e IPv6.|
|Dirección IP de la interfaz de servidor|Dirección IP de la interfaz de red del servidor DNS que recibió la solicitud DNS.|
|FQDN|Nombre de dominio completo del registro en la consulta, con la posibilidad de usar un carácter comodín.|
|Tipo de consulta|Tipo de registro al que \(se consulta un, SRV, txt,\)etc.|
|Hora del día|Hora del día a la que se recibe la consulta.|

En los siguientes ejemplos se muestra cómo crear filtros para la Directiva DNS que bloquean o permiten consultas de resolución de nombres DNS.

>[!NOTE]
>Los comandos de ejemplo de este tema usan el comando de Windows PowerShell **Add-DnsServerQueryResolutionPolicy**. Para obtener más información, consulte [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps). 

## <a name="bkmk_block1"></a>Bloquear consultas desde un dominio

En algunas circunstancias, es posible que desee bloquear la resolución de nombres DNS para los dominios identificados como malintencionados o para los dominios que no cumplan las directrices de uso de su organización. Puede realizar consultas de bloqueo de dominios mediante la Directiva DNS.

La Directiva que configure en este ejemplo no se crea en ninguna zona determinada; en su lugar, cree una directiva de nivel de servidor que se aplique a todas las zonas configuradas en el servidor DNS. Las directivas de nivel de servidor son las primeras que se evalúan y, por tanto, coinciden primero cuando el servidor DNS recibe una consulta.

El siguiente comando de ejemplo configura una directiva de nivel de servidor para bloquear cualquier consulta con el **sufijo**de dominio contosomalicious.com.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicy" -Action IGNORE -FQDN "EQ,*.contosomalicious.com" -PassThru 
`

>[!NOTE]
>Cuando se configura el parámetro de **acción** con el valor **omitir**, el servidor DNS se configura para quitar las consultas sin respuesta. Esto hace que el cliente DNS del dominio malintencionado agote el tiempo de espera.

## <a name="bkmk_block2"></a>Bloquear consultas desde una subred
Con este ejemplo, puede bloquear las consultas desde una subred si detecta algún malware y está intentando ponerse en contacto con sitios malintencionados mediante el servidor DNS. 

' Add-DnsServerClientSubnet-name "MaliciousSubnet06"-IPv4Subnet 172.0.33.0/24-PassThru

Add-DnsServerQueryResolutionPolicy-name "BlockListPolicyMalicious06"-Action IGNORE-ClientSubnet "EQ, MaliciousSubnet06"-PassThru "

En el ejemplo siguiente se muestra cómo puede usar los criterios de subred en combinación con los criterios de FQDN para bloquear consultas de determinados dominios malintencionados de subredes infectadas.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06" –FQDN “EQ,*.contosomalicious.com” -PassThru
`

## <a name="bkmk_block3"></a>Bloquear un tipo de consulta
Es posible que tenga que bloquear la resolución de nombres para determinados tipos de consultas en los servidores. Por ejemplo, puede bloquear la consulta "ANY", que se puede usar de forma malintencionada para crear ataques de amplificación.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyQType" -Action IGNORE -QType "EQ,ANY" -PassThru
`

## <a name="bkmk_allow1"></a>Permitir solo consultas desde un dominio
No solo puede usar la Directiva de DNS para bloquear consultas, sino que puede usarlas para aprobar automáticamente las consultas de dominios o subredes específicos. Cuando se configuran las listas de permitidos, el servidor DNS solo procesa las consultas de dominios permitidos mientras se bloquea el resto de consultas desde otros dominios.

El siguiente comando de ejemplo permite que solo los equipos y dispositivos de los dominios contoso.com y secundarios consulten el servidor DNS.

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicyDomain" -Action IGNORE -FQDN "NE,*.contoso.com" -PassThru 
`

## <a name="bkmk_allow2"></a>Permitir solo consultas desde una subred
También puede crear listas de permitidos para subredes IP, de modo que todas las consultas que no se originen en estas subredes se omitan.

`
Add-DnsServerClientSubnet -Name "AllowedSubnet06" -IPv4Subnet 172.0.33.0/24 -PassThru
`
`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicySubnet” -Action IGNORE -ClientSubnet  "NE, AllowedSubnet06" -PassThru
`

## <a name="bkmk_allow3"></a>Permitir solo determinados QTypes
Puede aplicar listas de permitidos a QTYPEs. 

Por ejemplo, si tiene clientes externos que consultan la interfaz del servidor DNS 164.8.1.1, solo se permite consultar ciertos QTYPEs, mientras que hay otros QTYPEs como registros SRV o TXT que usan los servidores internos para la resolución de nombres o con fines de supervisión.

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListQType" -Action IGNORE -QType "NE,A,AAAA,MX,NS,SOA" –ServerInterface “EQ,164.8.1.1” -PassThru
`

Puede crear miles de directivas DNS según los requisitos de administración del tráfico y todas las directivas nuevas se aplican de forma dinámica, sin necesidad de reiniciar el servidor DNS, en las consultas entrantes. 
