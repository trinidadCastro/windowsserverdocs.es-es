---
title: "Introducción a la autenticación Kerberos"
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-kerberos
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 646c6309-e865-4be2-b415-44dd125af5c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 9f8878fb959c34663b1e1f3858fa7e0b6e7b6105
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="kerberos-authentication-overview"></a>Introducción a la autenticación Kerberos

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Kerberos es un protocolo de autenticación que se usa para comprobar la identidad de un usuario o el host. Este tema contiene información sobre la autenticación de Kerberos en Windows Server 2012 y Windows 8.

## <a name="BKMK_OVER"></a>Descripción de la característica
Los sistemas operativos Windows Server implemente el protocolo de autenticación de Kerberos V5 y extensiones para la autenticación de clave pública, transportar datos de autorización y la delegación. El cliente de autenticación de Kerberos se implementa como un proveedor de soporte técnico de seguridad \(SSP\) y se pueden acceder mediante la interfaz del proveedor de soporte técnico de seguridad \(SSPI\). Autenticación de usuario inicial se integra con la arquitectura Winlogon solo en sign\.

El \(KDC\) Centro de distribución de claves Kerberos se integra con otros servicios de seguridad de Windows Server que se ejecutan en el controlador de dominio. KDC usa la base de datos de los servicios de dominio de Active Directory del dominio como su base de datos de cuentas de seguridad. Los servicios de dominio de Active Directory es necesario para las implementaciones de Kerberos predeterminado dentro del dominio o bosque.

## <a name="kerb_tr_Kerb_Benefits"></a>Aplicaciones prácticas
Las ventajas con Kerberos para autenticación basada en domain\ son:

-   **Autenticación delegada.**

    Servicios que se ejecutan en sistemas operativos Windows pueden suplantar a un equipo cliente tienen acceso a recursos en el nombre del cliente. En muchos casos, un servicio puede realizar su trabajo para el cliente mediante el acceso a recursos en el equipo local. Cuando un equipo cliente autentica el servicio, el protocolo Kerberos y NTLM proporcionar la información de autorización que un servicio necesita representar el equipo cliente localmente. Sin embargo, algunas aplicaciones distribuidas están diseñados para que un servicio front\-end debe usar la identidad del equipo cliente cuando se conecta a los servicios de back\-end en otros equipos. Autenticación de Kerberos admite un mecanismo de delegación que permite a un servicio para que actúe en nombre del cliente al conectarse a otros servicios.

-   **Inicio de sesión único.**

    Con Kerberos autenticación dentro de un dominio o en un bosque permite que el usuario o el acceso a recursos que permite a los administradores sin varias solicitudes de credenciales del servicio. Después de dominio inicial de sesión a través de Winlogon, Kerberos administra las credenciales en todo el bosque siempre que se intente el acceso a los recursos.

-   **Interoperabilidad.**

    La implementación del protocolo Kerberos V5 por Microsoft se basa en las especificaciones de standards\ pista que se recomiendan la \(IETF\) grupo de trabajo de ingeniería de Internet. Como resultado, en sistemas operativos Windows, el protocolo Kerberos establece una base para la interoperabilidad con otras redes en el que se usa el protocolo Kerberos para autenticación. Además, Microsoft publica la documentación de protocolos de Windows para implementar el protocolo Kerberos. La documentación contiene los requisitos técnicos, limitaciones, las dependencias y comportamiento de protocolo de Windows\ específico para la implementación de Microsoft del protocolo Kerberos.

-   **Autenticación más eficiente en servidores.**

    Antes de Kerberos, la autenticación NTLM podría usarse, lo que requiere un servidor de aplicaciones para conectarse a un controlador de dominio para autenticar a cada equipo cliente o servicio. Con el protocolo Kerberos, los vales de sesión renovables reemplazan pass\ a través de la autenticación. El servidor no es necesario para ir a un controlador de dominio \ (a menos que se necesita validar una \(PAC\)\) certificado de atributos de privilegios. En su lugar, el servidor puede autenticar el equipo cliente mediante el examen de credenciales presentadas por el cliente. Los equipos cliente pueden obtener credenciales para un servidor determinado una vez y, a continuación, volver a usar las credenciales durante una sesión de inicio de sesión de red.

-   **Autenticación mutua.**

    Mediante el protocolo Kerberos, una de las partes en cualquier extremo de una conexión de red puede comprobar que la parte en el otro extremo es la entidad que dice ser. NTLM no permitir que los clientes comprobar la identidad de un servidor o habilitar un servidor para comprobar la identidad de otro. La autenticación NTLM se diseñó para un entorno de red en el que los servidores se supone que original. El protocolo Kerberos no asume esto.

## <a name="see-also"></a>Consulta también
[Introducción a la autenticación de Windows](../windows-authentication/windows-authentication-overview.md)


