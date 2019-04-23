---
title: Arquitectura de autenticación de Windows
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 07c9d6bb-9b03-407d-89b6-97c7551b256b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: fd6e2cbc233a91b62e3c8c9caf1154f91c3863ac
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855866"
---
# <a name="windows-authentication-architecture"></a>Arquitectura de autenticación de Windows

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema para profesionales de TI se explica el esquema de la arquitectura básico para la autenticación de Windows.

La autenticación es el proceso por el que el sistema valida la información de inicio de sesión o el inicio de sesión del usuario. Nombre de usuario y la contraseña se comparan con una lista autorizada, y si el sistema detecta a una coincidencia, se concede acceso a la extensión especificada en la lista de permisos para ese usuario.

Como parte de una arquitectura extensible, los sistemas operativos Windows Server implementan un conjunto predeterminado de proveedores de compatibilidad de seguridad de autenticación, que incluyen Negotiate, el protocolo de Kerberos, NTLM, Schannel (canal seguro) e implícita. Los protocolos utilizados por estos proveedores de habilitar la autenticación de usuarios, equipos y servicios, y el proceso de autenticación permite a los usuarios autorizados y servicios de acceso a los recursos de forma segura.

En Windows Server, las aplicaciones autentican a los usuarios mediante el uso de la SSPI para abstraer las llamadas para la autenticación. Por lo tanto, los desarrolladores no es necesario comprender las complejidades de los protocolos de autenticación concreto o generar los protocolos de autenticación en sus aplicaciones.

Sistemas operativos Windows Server incluyen un conjunto de componentes de seguridad que constituyen el modelo de seguridad de Windows. Estos componentes garantizan que las aplicaciones no pueden obtener acceso a recursos sin autenticación y autorización. Las siguientes secciones describen los elementos de la arquitectura de autenticación.

### <a name="local-security-authority"></a>Autoridad de seguridad local
La autoridad de seguridad Local (LSA) es un subsistema protegido que autentica e inicia sesión en el equipo local a los usuarios. Además, LSA mantiene información sobre todos los aspectos de seguridad local en un equipo (estos aspectos se conocen colectivamente como la directiva de seguridad local). También proporciona diversos servicios de traducción entre nombres e identificadores de seguridad (SID).

El subsistema de seguridad realiza un seguimiento de las directivas de seguridad y las cuentas que se encuentran en un sistema informático. En el caso de un dominio, controlador, estas directivas y las cuentas son aquellos que están en vigor para el dominio donde se encuentra el controlador de dominio. Estas directivas y las cuentas se almacenan en Active Directory. El subsistema LSA proporciona servicios para validar el acceso a objetos, comprobación de derechos de usuario y la generación de mensajes de auditoría.

### <a name="security-support-provider-interface"></a>Interfaz del proveedor de soporte técnico de seguridad
La interfaz del proveedor de soporte técnico de seguridad (SSPI) es la API que obtiene los servicios de seguridad integrada para la autenticación, integridad del mensaje, privacidad de mensajes y seguridad calidad de servicio para cualquier protocolo de aplicación distribuida.

SSPI es la implementación de la API de servicio de seguridad genérico (GSSAPI). SSPI proporciona un mecanismo por el que una aplicación distribuida puede llamar a uno de varios proveedores de seguridad para obtener una conexión autenticada sin conocimiento de los detalles del protocolo de seguridad.

## <a name="see-also"></a>Vea también

-   [Arquitectura de interfaz de proveedor de soporte técnico de seguridad](security-support-provider-interface-architecture.md)

-   [Procesos de las credenciales de autenticación de Windows](credentials-processes-in-windows-authentication.md)

-   [Introducción técnica a la autenticación de Windows](https://technet.microsoft.com/library/dn169029.aspx)


