---
title: Protocolo de seguridad de capa de transporte de datagramas
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
ms.date: 10/12/2016
ms.openlocfilehash: 31c1cf1f3218c0511a4407b560be30d0c6f86233
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# Protocolo de seguridad de capa de transporte de datagramas

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Este tema de referencia para profesionales de TI describe el protocolo de seguridad de capa de transporte de datagramas (DTLS), que forma parte del proveedor de soporte técnico de seguridad (SSP) de Schannel.

## <a name="BKMK_DTLS"></a>
Introducidas en Schannel SSP en Windows Server 2012 y Windows 8, el protocolo DTLS proporciona la privacidad de comunicación para los protocolos de datagramas. Para obtener información sobre qué DTLS versión se admite en versiones de Windows, consulta [protocolos TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/en-us/library/windows/desktop/mt808159(v=vs.85).aspx). El protocolo permite que las aplicaciones de cliente y servidor para comunicarse de forma que se ha diseñado para evitar interceptaciones, manipulación o falsificación de mensajes. El protocolo DTLS se basa en el protocolo de seguridad de la capa de transporte (TLS) y proporciona seguridad equivalente garantías, lo que reduce la necesidad de usar IPsec o diseñar un protocolo de seguridad de nivel de aplicación personalizada.

Datagramas son comunes en streaming de medios, como juego o segura videoconferencia. Los desarrolladores pueden desarrollar aplicaciones que usan el protocolo DTLS dentro del contexto del modelo de interfaz de proveedor de soporte técnico de seguridad (SSPI) de autenticación de Windows para proteger la comunicación entre clientes y servidores. El protocolo DTLS está integrado en la parte superior el protocolo de datagramas de usuario (UDP). DTLS está diseñado para ser lo más parecido TLS como sea posible para minimizar el nuevo invento de seguridad y para maximizar la cantidad de la reutilización de código y de infraestructura.

Después de los que se puede configurar para TLS reproduzcan los conjuntos de cifrados que están disponibles para la configuración. No se permite el cifrado RC4. Schannel continúa usando la criptografía de próxima generación (CNG). Esto aprovecha las ventajas de la certificación de FIPS 140, que se introdujo en Windows Vista.

## Consulta también

[IETF RFC 4347 seguridad de la capa de transporte de datagramas](http://tools.ietf.org/html/rfc4347)


