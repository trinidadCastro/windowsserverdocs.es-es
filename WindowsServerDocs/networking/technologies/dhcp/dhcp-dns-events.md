---
title: Eventos de registro de DHCP para registros de registros DNS
description: En este tema se proporciona información sobre los eventos de registro del servidor DHCP en Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: beb8c188-6fcf-4520-8825-d17f8ee9fb04
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ccd8024af30f1103afa8eac52926a6b42d32940a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355428"
---
# <a name="dhcp-logging-events-for-dns-registrations"></a>Eventos de registro de DHCP para registros DNS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Los registros de eventos del servidor DHCP ahora proporcionan información detallada sobre los errores de registro de DNS.

>[!NOTE]
>En muchos casos, la razón de los errores de registro de registros de DNS por parte de los servidores DHCP es que una zona DNS inversa @ no__t-0Lookup se ha configurado incorrectamente o no está configurada.

Los siguientes eventos DHCP nuevos le ayudan a identificar fácilmente Cuándo se producen errores en los registros DNS debido a una configuración incorrecta o a la zona inversa @ no__t-0Lookup de DNS.

|Id.|Evento|Valor|
|-----|--------------------|--------------------------------------------------------|
|20317|DHCPv4. ForwardRecordDNSFailure|Error% 3 en el registro de registro de reenvío para la dirección IPv4% 1 y el FQDN% 2. Es probable que esto se deba a que la zona de búsqueda directa para este registro no existe en el servidor DNS.|
|20318|DHCPv4. ForwardRecordDNSTimeout|Error% 3 en el registro de registro de reenvío para la dirección IPv4% 1 y el FQDN% 2.|
|20319|DHCPv4. PTRRecordDNSFailure|Error% 3 en el registro de registros PTR para la dirección IPv4% 1 y el FQDN% 2. Probablemente, esto se debe a que la zona de búsqueda inversa para este registro no existe en el servidor DNS.|
|20320|DHCPv4. PTRRecordDNSTimeout|Error% 3 en el registro de registros PTR para la dirección IPv4% 1 y el FQDN% 2.|
|20321|DHCPv6. ForwardRecordDNSFailure|Error% 3 en el registro de registro de reenvío para la dirección IPv6% 1 y el FQDN% 2. Es probable que esto se deba a que la zona de búsqueda directa para este registro no existe en el servidor DNS.|
|20322|DHCPv6. ForwardRecordDNSTimeout|Error% 3 en el registro de registro de reenvío para la dirección IPv6% 1 y el FQDN% 2.|
|20323|DHCPv6. PTRRecordDNSFailure|Error% 3 en el registro PTR de la dirección IPv6% 1 y el FQDN% 2. Probablemente, esto se debe a que la zona de búsqueda inversa para este registro no existe en el servidor DNS.|
|20324|DHCPv6. PTRRecordDNSTimeout|Error% 3 en el registro PTR de la dirección IPv6% 1 y el FQDN% 2.|
|20325|DHCPv4. ForwardRecordDNSError|No se pudo registrar el registro PTR para la dirección IPv4% 1 y el FQDN% 2. error:% 3 \(% 4 @ no__t-1.|
|20326|DHCPv6. ForwardRecordDNSError|Error en el registro de registro de reenvío para la dirección IPv6% 1 y el FQDN% 2 con el error% 3 \(% 4 @ no__t-1|
|20327|DHCPv6. PTRRecordDNSError|No se pudo registrar el registro PTR para la dirección IPv6% 1 y el FQDN% 2. error:% 3 \(% 4 @ no__t-1.|

