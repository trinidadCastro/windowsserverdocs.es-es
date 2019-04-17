---
title: DHCP registrar eventos de registros DNS
description: Este tema proporciona información sobre el servidor DHCP registrar eventos en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: beb8c188-6fcf-4520-8825-d17f8ee9fb04
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 61a5099cd5e1ef1d4687baa8c20411c96ea8f519
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="dhcp-logging-events-for-dns-registrations"></a>Eventos de registro de DHCP para los registros DNS

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Registros de eventos de servidor DHCP ahora proporcionan información detallada acerca de los errores de registro DNS.

>[!NOTE]
>En muchos casos, el motivo de errores de registro de los registros de DNS servidores DHCP es que una zona de Reverse\ búsqueda DNS está configurada correctamente o no está configurada en absoluto.

Los siguientes eventos DHCP nuevos ayudan a identificar fácilmente cuando los registros DNS se error debido a un mal configurado o falta la zona de Reverse\ búsqueda DNS.

|ID.|Evento|Valor|
|-----|--------------------|--------------------------------------------------------|
|20317|DHCPv4.ForwardRecordDNSFailure|El registro del registro hacia delante para dirección IPv4%1 y FQDN %2 error %3. Esto es probable que sea porque la zona de búsqueda hacia delante para este registro no existe en el servidor DNS.|
|20318|DHCPv4.ForwardRecordDNSTimeout|El registro del registro hacia delante para dirección IPv4%1 y FQDN %2 error %3.|
|20319|DHCPv4.PTRRecordDNSFailure|No se pudo realizar la inscripción de registros PTR para dirección IPv4%1 y FQDN %2 con error %3. Esto es probable que sea porque la zona de búsqueda inversa para este registro no existe en el servidor DNS.|
|20320|DHCPv4.PTRRecordDNSTimeout|No se pudo realizar la inscripción de registros PTR para dirección IPv4%1 y FQDN %2 con error %3.|
|20321|DHCPv6.ForwardRecordDNSFailure|El registro del registro hacia delante para dirección IPv6%1 y FQDN %2 error %3. Esto es probable que sea porque la zona de búsqueda hacia delante para este registro no existe en el servidor DNS.|
|20322|DHCPv6.ForwardRecordDNSTimeout|El registro del registro hacia delante para dirección IPv6%1 y FQDN %2 error %3.|
|20323|DHCPv6.PTRRecordDNSFailure|No se pudo realizar la inscripción de registros PTR para dirección IPv6%1 y FQDN %2 con error %3. Esto es probable que sea porque la zona de búsqueda inversa para este registro no existe en el servidor DNS.|
|20324|DHCPv6.PTRRecordDNSTimeout|No se pudo realizar la inscripción de registros PTR para dirección IPv6%1 y FQDN %2 con error %3.|
|20325|DHCPv4.ForwardRecordDNSError|No se pudo realizar la inscripción de registros PTR para dirección IPv4%1 y FQDN %2 con error %3 \(%4\).|
|20326|DHCPv6.ForwardRecordDNSError|No se pudo realizar la inscripción de registros hacia delante para dirección IPv6%1 y FQDN %2 con error %3 \(%4\)|
|20327|DHCPv6.PTRRecordDNSError|No se pudo realizar la inscripción de registros PTR para dirección IPv6%1 y FQDN %2 con error %3 \(%4\).|

