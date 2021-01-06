---
title: Eventos de registro de DHCP para registros de registros DNS
description: En este tema se proporciona información sobre los eventos de registro del servidor DHCP en Windows Server 2016.
manager: brianlic
ms.topic: how-to
ms.assetid: beb8c188-6fcf-4520-8825-d17f8ee9fb04
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 51ca6af9b060b754f75de63089464d8310263734
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97948811"
---
# <a name="dhcp-logging-events-for-dns-registrations"></a>Eventos de registro de DHCP para registros DNS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Los registros de eventos del servidor DHCP ahora proporcionan información detallada sobre los errores de registro de DNS.

>[!NOTE]
>En muchos casos, la razón de los errores de registro de registros DNS por parte de los servidores DHCP es que una zona de búsqueda inversa de DNS \- está configurada incorrectamente o no está configurada.

Los siguientes eventos DHCP nuevos le ayudan a identificar fácilmente Cuándo se producen errores en los registros DNS debido a una configuración incorrecta o a que falta una zona de búsqueda inversa de DNS \- .

|ID|Evento|Valor|
|-----|--------------------|--------------------------------------------------------|
|20317|DHCPv4. ForwardRecordDNSFailure|Error %3 en el registro de registro de reenvío para la dirección IPv4% 1 y el FQDN %2. Es probable que esto se deba a que la zona de búsqueda directa para este registro no existe en el servidor DNS.|
|20318|DHCPv4. ForwardRecordDNSTimeout|Error %3 en el registro de registro de reenvío para la dirección IPv4% 1 y el FQDN %2.|
|20319|DHCPv4. PTRRecordDNSFailure|Error %3 en el registro de registros PTR para la dirección IPv4% 1 y el FQDN %2. Probablemente, esto se debe a que la zona de búsqueda inversa para este registro no existe en el servidor DNS.|
|20320|DHCPv4. PTRRecordDNSTimeout|Error %3 en el registro de registros PTR para la dirección IPv4% 1 y el FQDN %2.|
|20321|DHCPv6. ForwardRecordDNSFailure|Error %3 en el registro de registro de reenvío para la dirección IPv6% 1 y el FQDN %2. Es probable que esto se deba a que la zona de búsqueda directa para este registro no existe en el servidor DNS.|
|20322|DHCPv6. ForwardRecordDNSTimeout|Error %3 en el registro de registro de reenvío para la dirección IPv6% 1 y el FQDN %2.|
|20323|DHCPv6. PTRRecordDNSFailure|Error %3 en el registro PTR de la dirección IPv6% 1 y el FQDN %2. Probablemente, esto se debe a que la zona de búsqueda inversa para este registro no existe en el servidor DNS.|
|20324|DHCPv6. PTRRecordDNSTimeout|Error %3 en el registro PTR de la dirección IPv6% 1 y el FQDN %2.|
|20325|DHCPv4. ForwardRecordDNSError|No se pudo registrar el registro PTR para la dirección IPv4% 1 y el FQDN %2 con el error %3 \( %4 \) .|
|20326|DHCPv6. ForwardRecordDNSError|Error en el registro de registro de reenvío para la dirección IPv6% 1 y el FQDN %2: %3 \( %4\)|
|20327|DHCPv6. PTRRecordDNSError|No se pudo registrar el registro PTR para la dirección IPv6% 1 y el FQDN %2 con el error %3 \( %4 \) .|

