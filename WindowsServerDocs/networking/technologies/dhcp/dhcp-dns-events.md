---
title: Eventos de registro de DHCP para registrar registros DNS
description: Este tema proporciona información acerca del servidor DHCP de los eventos de registro en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: beb8c188-6fcf-4520-8825-d17f8ee9fb04
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7f4ce217b19cfd8a63bff1ae504362d4fc24fcd0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816906"
---
# <a name="dhcp-logging-events-for-dns-registrations"></a>Eventos de registro de DHCP para los registros DNS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Ahora, los registros de eventos de servidor DHCP proporcionan información detallada sobre los errores de registro DNS.

>[!NOTE]
>En muchos casos, el motivo de errores de registro de los registros DNS por los servidores DHCP es que un DNS inverso\-zona de búsqueda se configura incorrectamente o no está configurado en absoluto.

Los siguientes nuevos eventos DHCP ayuda a identificar fácilmente cuando los registros DNS son errores debido a una mala configuración o falta el DNS inverso\-zona de búsqueda.

|Id.|Evento|Valor|
|-----|--------------------|--------------------------------------------------------|
|20317|DHCPv4.ForwardRecordDNSFailure|Error en el registro de registro hacia delante para la dirección IPv4 %1 y FQDN %2 error %3. Esto suele deberse a la zona de búsqueda directa para este registro no existe en el servidor DNS.|
|20318|DHCPv4.ForwardRecordDNSTimeout|Error en el registro de registro hacia delante para la dirección IPv4 %1 y FQDN %2 error %3.|
|20319|DHCPv4.PTRRecordDNSFailure|Registro de registros PTR para direcciones IPv4 %1 y FQDN %2 error %3. Esto suele deberse a la zona de búsqueda inversa para este registro no existe en el servidor DNS.|
|20320|DHCPv4.PTRRecordDNSTimeout|Registro de registros PTR para direcciones IPv4 %1 y FQDN %2 error %3.|
|20321|DHCPv6.ForwardRecordDNSFailure|Error en el registro de registro hacia delante para la dirección IPv6 %1 y FQDN %2 error %3. Esto suele deberse a la zona de búsqueda directa para este registro no existe en el servidor DNS.|
|20322|DHCPv6.ForwardRecordDNSTimeout|Error en el registro de registro hacia delante para la dirección IPv6 %1 y FQDN %2 error %3.|
|20323|DHCPv6.PTRRecordDNSFailure|Registro de los registro PTR para direcciones IPv6 %1 y FQDN %2 error %3. Esto suele deberse a la zona de búsqueda inversa para este registro no existe en el servidor DNS.|
|20324|DHCPv6.PTRRecordDNSTimeout|Registro de los registro PTR para direcciones IPv6 %1 y FQDN %2 error %3.|
|20325|DHCPv4.ForwardRecordDNSError|Error de registro de registros PTR para direcciones IPv4 %1 y FQDN %2 %3 \(%4\).|
|20326|DHCPv6.ForwardRecordDNSError|Error en el registro del registro hacia delante para direcciones IPv6 %1 y FQDN %2 %3 \(%4\)|
|20327|DHCPv6.PTRRecordDNSError|Error de registro de los registro PTR para direcciones IPv6 %1 y FQDN %2 %3 \(%4\).|

