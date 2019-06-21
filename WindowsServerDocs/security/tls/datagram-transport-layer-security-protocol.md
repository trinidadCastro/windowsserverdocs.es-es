---
title: Protocolo de seguridad de capa de transporte de datagrama
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57b8873a-ad9c-4f2c-93e0-a2af352c6965
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/16/2018
ms.openlocfilehash: 6f8d7d10ccaace0f75bb470647e3f4571940d9c0
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284323"
---
# <a name="datagram-transport-layer-security-protocol"></a>Protocolo de seguridad de capa de transporte de datagrama

Windows Server (canal semianual), Windows Server 2016, Windows 10

Este tema de referencia para profesionales de TI describe el protocolo de seguridad de capa de transporte de datagrama (DTLS), que forma parte del proveedor de soporte técnico de seguridad Schannel (SSP).

## <a name="BKMK_DTLS"></a>
Se introdujo en el SSP de Schannel en Windows Server 2012 y Windows 8, el protocolo DTLS proporciona privacidad de comunicación de protocolos de datagramas. Para obtener información acerca de qué DTLS versión es compatible con las versiones de Windows, consulte [protocolos en TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159(v=vs.85).aspx). El protocolo permite que las aplicaciones de cliente y servidor se comuniquen de acuerdo con el modo en que fueron diseñadas para evitar interceptaciones, alteraciones o falsificación de mensajes. El protocolo DTLS se basa en el protocolo de Seguridad de la capa de transporte (TLS) y proporciona garantías de seguridad equivalentes, lo que reduce la necesidad de usar IPsec o diseñar un protocolo personalizado de seguridad de la capa de la aplicación.

Los datagramas son comunes en streaming multimedia, como juegos o protegida videoconferencia. Los desarrolladores pueden desarrollar aplicaciones para usar el protocolo DTLS dentro del contexto del modelo de interfaz de proveedor de compatibilidad para seguridad (SSPI) de autenticación de Windows para proteger la comunicación entre clientes y servidores. El protocolo DTLS se basa en la parte superior del protocolo de datagramas de usuario (UDP). DTLS está diseñado para ser tan similar a TLS como sea posible para minimizar la nueva invención de seguridad y para maximizar la cantidad de reutilización de código y la infraestructura.

Los conjuntos de cifrado que están disponibles para la configuración se modelan después de los que puede configurar para que TLS. No se permite RC4. Schannel continúa usando Cryptography Next Generation (CNG). Esto aprovecha las ventajas de la certificación FIPS 140, que se introdujo en Windows Vista.

## <a name="see-also"></a>Vea también

[Seguridad de capa de transporte del datagrama RFC 4347 de IETF](http://tools.ietf.org/html/rfc4347)


