---
title: Protocolo de seguridad de capa de transporte de datagrama
description: Seguridad de Windows Server
ms.prod: windows-server
ms.technology: security-tls-ssl
ms.topic: article
ms.assetid: 57b8873a-ad9c-4f2c-93e0-a2af352c6965
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/16/2018
ms.openlocfilehash: d0c066b063cbfc8def54c2e0d02cbb0eaf7f1d40
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852928"
---
# <a name="datagram-transport-layer-security-protocol"></a>Protocolo de seguridad de capa de transporte de datagrama

Windows Server (canal semianual), Windows Server 2016 y Windows 10

En este tema de referencia para profesionales de TI se describe el protocolo de seguridad de la capa de transporte de datagrama (DTLS), que forma parte del proveedor de compatibilidad para seguridad de Schannel (SSP).

## <a name="BKMK_DTLS"></a>
Introducido en el SSP de Schannel en Windows Server 2012 y Windows 8, el protocolo DTLS proporciona privacidad de comunicación para los protocolos de datagramas. Para obtener información sobre la versión de DTLS que se admite en las versiones de Windows, consulte [protocolos en TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159(v=vs.85).aspx). El protocolo permite que las aplicaciones de cliente y servidor se comuniquen de acuerdo con el modo en que fueron diseñadas para evitar interceptaciones, alteraciones o falsificación de mensajes. El protocolo DTLS se basa en el protocolo de Seguridad de la capa de transporte (TLS) y proporciona garantías de seguridad equivalentes, lo que reduce la necesidad de usar IPsec o diseñar un protocolo personalizado de seguridad de la capa de la aplicación.

Los datagramas son comunes en el streaming multimedia, como juegos o videoconferencias seguras. Los desarrolladores pueden desarrollar aplicaciones para usar el protocolo DTLS en el contexto del modelo de la interfaz del proveedor de compatibilidad con seguridad (SSPI) de autenticación de Windows para proteger la comunicación entre clientes y servidores. El protocolo DTLS se basa en el protocolo de datagramas de usuario (UDP). DTLS está diseñado para ser tan similar a TLS como sea posible para minimizar la nueva invención de seguridad y para maximizar la cantidad de código y la reutilización de la infraestructura.

Los conjuntos de cifrado que están disponibles para la configuración se incluyen en el patrón después de los que se pueden configurar para TLS. No se permite RC4. Schannel sigue usando Cryptography Next Generation (CNG). Esto aprovecha la certificación FIPS 140, que se presentó en Windows Vista.

## <a name="see-also"></a>Vea también

[Seguridad de la capa de transporte de datagrama RFC 4347 de IETF](http://tools.ietf.org/html/rfc4347)


                                        