---
title: Arquitectura de autenticación de Windows
description: Seguridad de Windows Server
ms.prod: windows-server
ms.technology: security-windows-auth
ms.topic: article
ms.assetid: 07c9d6bb-9b03-407d-89b6-97c7551b256b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: da4f173a5d91f73c73d3f537f58228890f90b136
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85471680"
---
# <a name="windows-authentication-architecture"></a>Arquitectura de autenticación de Windows

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema de introducción para profesionales de TI se explica el esquema de arquitectura básico para la autenticación de Windows.

La autenticación es el proceso por el cual el sistema valida la información de inicio de sesión o de inicio de sesión de un usuario. El nombre y la contraseña de un usuario se comparan con una lista autorizada y, si el sistema detecta una coincidencia, se concede el acceso a la extensión especificada en la lista de permisos para ese usuario.

Como parte de una arquitectura extensible, los sistemas operativos Windows Server implementan un conjunto predeterminado de proveedores de compatibilidad con seguridad de autenticación, que incluyen Negotiate, el protocolo Kerberos, NTLM, Schannel (canal seguro) y Digest. Los protocolos utilizados por estos proveedores permiten la autenticación de usuarios, equipos y servicios, y el proceso de autenticación permite a los usuarios y servicios autorizados tener acceso a los recursos de forma segura.

En Windows Server, las aplicaciones autentican a los usuarios mediante SSPI para realizar llamadas abstractas para la autenticación. Por lo tanto, no es necesario que los desarrolladores conozcan las complejidades de los protocolos de autenticación específicos o los protocolos de autenticación de compilación en sus aplicaciones.

Los sistemas operativos Windows Server incluyen un conjunto de componentes de seguridad que constituyen el modelo de seguridad de Windows. Estos componentes garantizan que las aplicaciones no puedan tener acceso a los recursos sin autenticación y autorización. En las secciones siguientes se describen los elementos de la arquitectura de autenticación.

### <a name="local-security-authority"></a>Autoridad de seguridad local
La autoridad de seguridad local (LSA) es un subsistema protegido que autentica a los usuarios y los inicia sesión en el equipo local. Además, LSA mantiene información sobre todos los aspectos de la seguridad local en un equipo (estos aspectos se conocen colectivamente como la Directiva de seguridad local). También proporciona diversos servicios de traducción entre los nombres y los identificadores de seguridad (SID).

El subsistema de seguridad realiza un seguimiento de las directivas de seguridad y las cuentas que se encuentran en un equipo. En el caso de un controlador de dominio, estas directivas y cuentas son las que están en vigor para el dominio en el que se encuentra el controlador de dominio. Estas directivas y cuentas se almacenan en Active Directory. El subsistema LSA proporciona servicios para validar el acceso a los objetos, comprobar los derechos de usuario y generar mensajes de auditoría.

### <a name="security-support-provider-interface"></a>Interfaz del proveedor de compatibilidad con seguridad
La interfaz del proveedor de compatibilidad para seguridad (SSPI) es la API que obtiene servicios de seguridad integrados para la autenticación, la integridad de mensajes, la privacidad de mensajes y la calidad de servicio de seguridad para cualquier protocolo de aplicación distribuida.

SSPI es la implementación de la API de servicio de seguridad genérico (GSSAPI). SSPI proporciona un mecanismo por el que una aplicación distribuida puede llamar a uno de varios proveedores de seguridad para obtener una conexión autenticada sin conocer los detalles del Protocolo de seguridad.

## <a name="additional-references"></a>Referencias adicionales

-   [Security Support Provider Interface Architecture](security-support-provider-interface-architecture.md)

-   [Procesos de las credenciales en la autenticación de Windows](credentials-processes-in-windows-authentication.md)

-   [Información técnica sobre autenticación de Windows](https://technet.microsoft.com/library/dn169029.aspx)


