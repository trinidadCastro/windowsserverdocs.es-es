---
title: Información general de TLS/SSL (Schannel SSP)
description: Seguridad de Windows Server
ms.topic: article
ms.assetid: 1b7b0432-1bef-4912-8c9a-8989d47a4da9
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 05/16/2018
ms.openlocfilehash: 21ad7977039eda311dd6f093fc53c09c08cf0317
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89637845"
---
# <a name="tlsssl-overview-schannel-ssp"></a>Información general de TLS/SSL (Schannel SSP)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016 y Windows 10

En este tema para profesionales de TI se presentan las implementaciones de TLS y SSL en Windows con el proveedor de servicios de seguridad de Schannel (SSP) al describir aplicaciones prácticas, cambios en la implementación de Microsoft y requisitos de software, además de recursos adicionales para Windows Server 2012 y Windows 8.

## <a name="description"></a><a name="BKMK_OVER"></a>Descripción
Schannel es un proveedor de compatibilidad para seguridad (SSP) que implementa los protocolos de autenticación estándar de Internet de la Capa de sockets seguros (SSL) y la Seguridad de la capa de transporte (TLS).

La interfaz del proveedor de compatibilidad para seguridad (SSPI) es una API que usan los sistemas Windows para realizar funciones basadas en la seguridad, incluida la autenticación. SSPI funciona como una interfaz común a varios SSP, incluido el SSP de Schannel.

Las versiones de TLS 1,0, 1,1 y 1,2, las versiones de SSL 2,0 y 3,0, así como el protocolo DTLS de seguridad de la capa de transporte de datagramas \( \) versión 1,0, y el protocolo PCT de transporte de comunicaciones privadas \( \) se basan en la criptografía de clave pública. El conjunto de protocolos de autenticación de SChannel ofrece estos protocolos. Todos los protocolos de Schannel usan un modelo cliente/servidor.

## <a name="applications"></a><a name="BKMK_APP"></a>APLICACIONES
Uno de los problemas existentes a la hora de administrar una red es el de proteger los datos que se envían entre las aplicaciones a través de una red que no es de confianza. Puede usar TLS y SSL para autenticar servidores y equipos cliente y, a continuación, usar el protocolo para cifrar los mensajes entre las partes autenticadas.

Por ejemplo, puede usar TLS/SSL para:

-   Transacciones de seguridad SSL con un sitio web de comercio electrónico
-   Acceso para clientes autenticados a un sitio web con seguridad SSL
-   Acceso remoto
-   Acceso SQL
-   Correo electrónico

## <a name="requirements"></a><a name="BKMK_SOFT"></a>Requisitos
Los protocolos TLS y SSL usan un modelo cliente/servidor y se basan en la autenticación de certificados, que requiere una infraestructura de clave pública.

## <a name="server-manager-information"></a><a name="BKMK_INSTALL"></a>Información sobre el Administrador del servidor
No hay pasos de configuración necesarios para implementar TLS, SSL o Schannel.

## <a name="additional-references"></a>Referencias adicionales ##

-   [Paquete de seguridad Schannel](/windows/desktop/com/schannel)
-   [Canal seguro](/windows/desktop/SecAuthN/secure-channel)
-   [Protocolo de seguridad de la capa de transporte](/windows/desktop/SecAuthN/transport-layer-security-protocol)