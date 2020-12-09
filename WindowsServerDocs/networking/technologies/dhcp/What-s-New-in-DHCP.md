---
title: Novedades del DHCP
description: En este tema se proporciona información general sobre las nuevas características del Protocolo de configuración dinámica de host (DHCP) en Windows Server 2016.
manager: brianlic
ms.topic: get-started-article
ms.assetid: c6f36998-5b64-45d1-b1f0-0f0d6604dbe3
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 5dee105eaf14c92145e1fe70fe4627d37b2baa82
ms.sourcegitcommit: b0c73df80d7b4ff0c332d77e0cc07f7e6e061600
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/09/2020
ms.locfileid: "96925564"
---
# <a name="whats-new-in-dhcp"></a>Novedades del DHCP

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se describen las funciones del Protocolo de configuración dinámica de host (DHCP) nuevas o modificadas en Windows Server 2016.

DHCP es un estándar de Internet Engineering Task Force (IETF) diseñado para reducir la carga administrativa y la complejidad de configurar hosts en una red basada en TCP/IP \- , como una intranet privada. Al usar el servicio del servidor DHCP, el proceso de configuración de TCP/IP en clientes DHCP es automático.

En las secciones siguientes se proporciona información acerca de las nuevas características y los cambios en la funcionalidad de DHCP.

## <a name="new-dhcp-client-side-features-in-the-windows-10-may-2020-update"></a>Nuevas características del cliente DHCP en la actualización 2020 de Windows 10

El cliente DHCP de Windows 10 se ha actualizado en la actualización del 10 de mayo de 2020 (también denominada Windows 10, versión 2004). Cuando se ejecuta un cliente de Windows y se conecta a Internet a través de un teléfono Android anclado, la conexión se debe marcar como "medida". Anteriormente, las conexiones se marcaban como desmedidos. Tenga en cuenta que no todos los teléfonos con tethering de Android se detectarán como medidos y otras redes también pueden aparecer como medida.

Además, el nombre de proveedor de cliente tradicional se ha actualizado en algunos dispositivos basados en Windows. Este valor solía ser simplemente MSFT 5,0. Algunos dispositivos se mostrarán ahora como MSFT 5,0 XBOX.

## <a name="new-dhcp-client-side-features-in-the-windows-10-april-2018-update"></a>Nuevas características del cliente DHCP en la actualización 2018 de abril de Windows 10

El cliente DHCP de Windows 10 se ha actualizado en la actualización de Windows de abril de 2018 (también denominada Windows 10, versión 1803) para leer y aplicar la opción 119, la opción de búsqueda en el dominio, del servidor DHCP al que se conecta el sistema. La opción de búsqueda en el dominio proporciona sufijos DNS para búsquedas DNS de nombres cortos. La opción 119 de DHCP se especifica en [RFC 3397](https://tools.ietf.org/html/rfc3397).

## <a name="dhcp-subnet-selection-options"></a>Opciones de selección de subred DHCP

DHCP ahora admite la opción 82 \( Sub-Option 5 \) . Puede usar esta opción para permitir que los clientes proxy DHCP y los agentes de retransmisión soliciten una dirección IP para una subred específica.


Si usa un agente de retransmisión DHCP que está configurado con la opción 82 de DHCP, sub \- Option 5, el agente de retransmisión puede solicitar una concesión de dirección IP a los clientes DHCP de un intervalo de direcciones IP específico.

Para obtener más información, consulte [Opciones de selección de subred DHCP](dhcp-subnet-options.md).

## <a name="new-logging-events-for-dns-registration-failures-by-the-dhcp-server"></a>Nuevos eventos de registro para errores de registro de DNS por el servidor DHCP

DHCP incluye ahora los eventos de registro para las circunstancias en las que los registros DNS del servidor DHCP producen errores en el servidor DNS.

Para obtener más información, consulte [DHCP Logging Events for DNS record](dhcp-dns-events.md)registrations.

## <a name="dhcp-nap-is-not-supported-in-windows-server-2016"></a>El NAP de DHCP no se admite en Windows Server 2016

La protección de acceso a redes \( NAP \) está en desuso en windows Server 2012 R2 y, en windows Server 2016, el rol de servidor DHCP ya no es compatible con NAP. Para obtener más información, vea [características eliminadas o en desuso en Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303411(v=ws.11)).

La compatibilidad con NAP se presentó al rol de servidor DHCP con Windows Server 2008 y es compatible con los sistemas operativos de cliente y servidor de Windows anteriores a Windows 10 y Windows Server 2016. En la tabla siguiente se resume la compatibilidad con NAP en Windows Server.

|Sistema operativo|Compatibilidad con NAP|
|--------------------|---------------|
| Windows Server 2008 |Compatible|
| Windows Server 2008 R2 |Compatible|
| Windows Server 2012 |Compatible|
| Windows Server 2012 R2 |Compatible|
| Windows Server 2016|No compatible|

En una implementación de NAP, un servidor DHCP que ejecute un sistema operativo que admita NAP puede funcionar como un punto de cumplimiento NAP para el método de cumplimiento NAP DHCP. Para obtener más información acerca de DHCP en NAP, consulte [lista de comprobación: implementar un diseño de cumplimiento DHCP](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd314186(v=ws.10)).

En Windows Server 2016, los servidores DHCP no aplican directivas de NAP y los ámbitos DHCP no pueden estar \- habilitados para NAP. Los equipos cliente DHCP que también son clientes NAP envían un informe de \( SOH de mantenimiento \) con la solicitud DHCP. Si el servidor DHCP ejecuta Windows Server 2016, estas solicitudes se procesan como si no hubiera ningún SoH. El servidor DHCP concede una concesión DHCP normal al cliente.

Si los servidores que ejecutan Windows Server 2016 son proxies RADIUS que reenvían las solicitudes de autenticación a un NPS del servidor de directivas de redes \( \) que admite NAP, NPS evalúa estos clientes NAP como no compatibles con NAP \- y se produce un error en el procesamiento de NAP.

## <a name="additional-references"></a>Referencias adicionales

-   [Protocolo de configuración dinámica de host (DHCP)](./dhcp-top.md)
