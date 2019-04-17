---
title: "TLS - Introducción SSL (Schannel SSP)"
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
ms.date: 10/12/2016
ms.openlocfilehash: afd0b70264dba1e720f95e40d3d201c2c5bf1c64
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="tls---ssl-schannel-ssp-overview"></a>TLS - Introducción SSL (Schannel SSP)

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Este tema para profesionales de TI presenta la implementación de SSL/TLS en Windows con el proveedor de servicio de seguridad (SSP) de Schannel describiendo aplicaciones prácticas, cambios en la implementación de Microsoft y requisitos de software, además de recursos adicionales para Windows Server 2012 y Windows 8.

**Quisiste decir:**

-   [El paquete de seguridad Schannel](https://msdn.microsoft.com/library/ms678421.aspx)

-   [Canal seguro](https://msdn.microsoft.com/library/windows/desktop/aa380123.aspx)

-   [Protocolo de seguridad de capa de transporte](https://msdn.microsoft.com/library/windows/desktop/aa380516.aspx)

## <a name="BKMK_OVER"></a>Descripción de \(Schannel\) TLS/SSL
Schannel es un \(SSP\) proveedor de soporte técnico de seguridad que implementa la capa de Sockets seguros \(SSL\) y la seguridad de la capa de transporte \(TLS\) protocolos de autenticación de Internet.

La interfaz de proveedor de soporte técnico de seguridad \(SSPI\) es una API usada por los sistemas Windows para realizar funciones relacionadas con la security\, incluida la autenticación. Las funciones SSPI como una interfaz común a varios \(SSPs\) proveedores de compatibilidad para seguridad, incluida la SSP. Schannel

Las versiones del protocolo de seguridad de la capa de transporte \(TLS\) 1.0, 1.1 y 1.2, el protocolo de capa de Sockets seguros \(SSL\), protocolo \(PCT\) de las versiones 2.0 y 3.0, seguridad de capa de transporte de datagramas \(DTLS\) versión 1.0 y el transporte de comunicaciones privadas se basan en criptografía de clave pública. El conjunto de protocolos de autenticación de canal de seguridad \(Schannel\) proporciona estos protocolos. Todos los protocolos de Schannel utilizan un modelo de client\ cliente-servidor.

## <a name="BKMK_APP"></a>Aplicaciones prácticas
Un problema al administrar una red es proteger los datos que se envían entre aplicaciones a través de una red de confianza. Puedes usar TLS/SSL para autenticar servidores y equipos cliente y, a continuación, usar el protocolo para cifrar mensajes entre las partes autenticados.

Por ejemplo, puedes usar TLS/SSL para:

-   Protege SSL\ transacciones con un sitio Web de comercio e\

-   Cliente autenticados acceso a un sitio Web protegido SSL\

-   Acceso remoto

-   Acceso SQL

-   E\ correo

## <a name="BKMK_SOFT"></a>Requisitos de software
El protocolo TLS/SSL usa un modelo de client\server y se basan en la autenticación de certificado, que requiere una infraestructura de clave pública.

## <a name="BKMK_INSTALL"></a>Información del administrador del servidor
No hay ningún paso de configuración necesarios para implementar TLS, SSL o Schannel.

