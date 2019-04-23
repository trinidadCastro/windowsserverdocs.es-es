---
title: Información general TLS/SSL (Schannel SSP)
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: a6571e5e06e07fd62ad4cf39bab322b45c90a9f9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848606"
---
# <a name="tlsssl-overview-schannel-ssp"></a>Información general TLS/SSL (Schannel SSP)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10

Este tema para profesionales de TI introducen las implementaciones de TLS y SSL en Windows mediante el proveedor de servicios de seguridad de Schannel (SSP) describen aplicaciones prácticas, cambios en la implementación de Microsoft y los requisitos de software, además de recursos adicionales para Windows Server 2012 y Windows 8.

## <a name="BKMK_OVER"></a>Descripción
Schannel es un proveedor de compatibilidad para seguridad (SSP) que implementa los protocolos de autenticación estándar de Internet de la Capa de sockets seguros (SSL) y la Seguridad de la capa de transporte (TLS).

La interfaz del proveedor de compatibilidad para seguridad (SSPI) es una API que usan los sistemas Windows para realizar funciones basadas en la seguridad, incluida la autenticación. SSPI funciona como una interfaz común para varios SSP, incluido el SSP. Schannel

Las versiones 1.0, 1.1 y 1.2, las versiones SSL 2.0 y 3.0, TLS, así como la seguridad de capa de transporte de datagrama \(DTLS\) versión 1.0 del protocolo y el transporte de comunicaciones privadas \(PCT\) protocolo se basa en criptografía de clave pública. El conjunto de protocolos de autenticación de SChannel ofrece estos protocolos. Todos los protocolos de Schannel usan un modelo cliente/servidor.

## <a name="BKMK_APP"></a>Aplicaciones
Uno de los problemas al administrar una red es asegurar los datos que se envían entre las aplicaciones a través de una red que no es de confianza. Puede usar TLS y SSL para autenticar servidores y equipos cliente y, a continuación, usar el protocolo para cifrar mensajes entre las partes autenticadas.

Por ejemplo, puede usar TLS/SSL para:

-   Transacciones de seguridad SSL con un sitio web de comercio electrónico
-   Acceso para clientes autenticados a un sitio web con seguridad SSL
-   Acceso remoto
-   Acceso SQL
-   Correo electrónico

## <a name="BKMK_SOFT"></a>Requisitos
Los protocolos TLS y SSL usan un modelo cliente/servidor y se basan en la autenticación de certificado, que requiere una infraestructura de clave pública.

## <a name="BKMK_INSTALL"></a>Información del administrador del servidor
No hay ningún paso de configuración necesarios para implementar TLS, SSL o Schannel.

## <a name="see-also"></a>Vea también ##

-   [El paquete de seguridad de Schannel](https://docs.microsoft.com/windows/desktop/com/schannel)
-   [Canal seguro](https://docs.microsoft.com/windows/desktop/SecAuthN/secure-channel)
-   [Protocolo de seguridad de capa de transporte](https://docs.microsoft.com/windows/desktop/SecAuthN/transport-layer-security-protocol)
