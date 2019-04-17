---
title: "Arquitectura de autenticación de Windows"
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
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="windows-authentication-architecture"></a>Arquitectura de autenticación de Windows

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Este tema de introducción para profesionales de TI explica el esquema de la arquitectura básico para la autenticación de Windows.

Autenticación es el proceso por el que el sistema valida la información de inicio de sesión o el inicio de sesión del usuario. Nombre de usuario y la contraseña se comparan con una lista autorizada y, si el sistema detecta a una coincidencia, se concede acceso a las condiciones especificadas en la lista de permisos para ese usuario.

Como parte de una arquitectura extensible, los sistemas operativos Windows Server implementar un conjunto predeterminado de proveedores de soporte técnico de seguridad de autenticación, que incluyen Negotiate, el protocolo de Kerberos, NTLM, Schannel (canal seguro) y resumen. Los protocolos usados por estos proveedores de habilitar la autenticación de usuarios, equipos y servicios, y el proceso de autenticación permite a los usuarios autorizados y servicios acceder a recursos de manera segura.

En Windows Server, aplicaciones autentican los usuarios mediante la SSPI abstraer llamadas para la autenticación. Por lo tanto, los desarrolladores no es necesario comprender las complejidades de los protocolos de autenticación específicos o crear los protocolos de autenticación en sus aplicaciones.

Sistemas operativos de servidor de Windows incluyen un conjunto de componentes de seguridad que conforman el modelo de seguridad de Windows. Estos componentes garantizan que las aplicaciones no pueden tener acceso a recursos sin autenticación y autorización. Las siguientes secciones describen los elementos de la arquitectura de autenticación.

### <a name="local-security-authority"></a>Autoridad de seguridad local
La autoridad de seguridad Local (LSA) es un subsistema protegido que autentica e inicia sesión en el equipo local a los usuarios. Además, la LSA mantiene información acerca de todos los aspectos de seguridad local en un equipo (estos aspectos se conocen como la directiva de seguridad local). También proporciona distintos servicios de traducción entre los nombres y los identificadores de seguridad (SID).

El subsistema de seguridad realiza un seguimiento de las directivas de seguridad y las cuentas que se encuentran en un sistema del equipo. En el caso de un dominio, controlador, estas directivas y las cuentas son los que están en vigor para el dominio donde se encuentra el controlador de dominio. Estas directivas y las cuentas se almacenan en Active Directory. El subsistema LSA proporciona servicios para validar el acceso a objetos, comprueba los derechos de usuario y la generación de mensajes de auditoría.

### <a name="security-support-provider-interface"></a>Interfaz de proveedor de soporte técnico de seguridad
La interfaz de proveedor de soporte técnico de seguridad (SSPI) es la API que obtiene los servicios de seguridad integrada para la autenticación, integridad de los mensajes, privacidad de los mensajes y seguridad calidad de servicio para cualquier aplicación distribuida de protocolo.

SSPI es la implementación de la API de servicio de seguridad genérico (GSSAPI). SSPI proporciona un mecanismo mediante el cual una aplicación distribuida puede llamar a uno de varios proveedores de seguridad para obtener una conexión autenticada sin conocimiento de los detalles del protocolo de seguridad.

## <a name="see-also"></a>Consulta también

-   [Arquitectura de interfaz de proveedor de soporte técnico de seguridad](security-support-provider-interface-architecture.md)

-   [Procesos de credenciales de autenticación de Windows](credentials-processes-in-windows-authentication.md)

-   [Introducción técnica de autenticación de Windows](https://technet.microsoft.com/library/dn169029.aspx)


