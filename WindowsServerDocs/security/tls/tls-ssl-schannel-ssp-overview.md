---
title: Información general de TLS/SSL (Schannel SSP)
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b7b0432-1bef-4912-8c9a-8989d47a4da9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/16/2018
ms.openlocfilehash: 51e3886339b7864951b322d210e1a05bef4dc1e1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403362"
---
# <a name="tlsssl-overview-schannel-ssp"></a>Información general de TLS/SSL (Schannel SSP)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10

En este tema para profesionales de TI se presentan las implementaciones de TLS y SSL en Windows con el proveedor de servicios de seguridad de Schannel (SSP) al describir aplicaciones prácticas, cambios en la implementación de Microsoft y requisitos de software, además de recursos adicionales para Windows Server 2012 y Windows 8.

## <a name="BKMK_OVER"></a>Descripción
Schannel es un proveedor de compatibilidad para seguridad (SSP) que implementa los protocolos de autenticación estándar de Internet de la Capa de sockets seguros (SSL) y la Seguridad de la capa de transporte (TLS).

La interfaz del proveedor de compatibilidad para seguridad (SSPI) es una API que usan los sistemas Windows para realizar funciones basadas en la seguridad, incluida la autenticación. SSPI funciona como una interfaz común a varios SSP, incluido el SSP de Schannel.

Las versiones de TLS 1,0, 1,1 y 1,2, las versiones de SSL 2,0 y 3,0, así como la seguridad de la capa de transporte de datagramas \(DTLS @ no__t-1 versión 1,0 y el protocolo de transporte de comunicaciones privadas \(PCT @ no__t-3 se basan en la criptografía de clave pública. El conjunto de protocolos de autenticación de SChannel ofrece estos protocolos. Todos los protocolos de Schannel usan un modelo cliente/servidor.

## <a name="BKMK_APP"></a>Programas
Uno de los problemas al administrar una red es asegurar los datos que se envían entre las aplicaciones a través de una red que no es de confianza. Puede usar TLS y SSL para autenticar servidores y equipos cliente y, a continuación, usar el protocolo para cifrar los mensajes entre las partes autenticadas.

Por ejemplo, puede usar TLS/SSL para:

-   Transacciones de seguridad SSL con un sitio web de comercio electrónico
-   Acceso para clientes autenticados a un sitio web con seguridad SSL
-   Acceso remoto
-   Acceso SQL
-   Correo electrónico

## <a name="BKMK_SOFT"></a>Satisfacer
Los protocolos TLS y SSL usan un modelo cliente/servidor y se basan en la autenticación de certificados, que requiere una infraestructura de clave pública.

## <a name="BKMK_INSTALL"></a>Información de Administrador del servidor
No hay pasos de configuración necesarios para implementar TLS, SSL o Schannel.

## <a name="see-also"></a>Vea también ##

-   [Paquete de seguridad Schannel](https://docs.microsoft.com/windows/desktop/com/schannel)
-   [Canal seguro](https://docs.microsoft.com/windows/desktop/SecAuthN/secure-channel)
-   [Protocolo de seguridad de la capa de transporte](https://docs.microsoft.com/windows/desktop/SecAuthN/transport-layer-security-protocol)
