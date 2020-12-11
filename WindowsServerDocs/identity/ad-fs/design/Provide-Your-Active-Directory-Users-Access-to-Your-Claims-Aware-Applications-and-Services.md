---
description: 'Más información acerca de: proporcionar a los usuarios Active Directory acceso a sus aplicaciones y servicios para notificaciones'
ms.assetid: d254fca3-85a1-424d-ac22-d6687ec3798e
title: Proporcionar a los usuarios de Active Directory acceso a las aplicaciones y servicios habilitados para notificaciones
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: a37147b46dc0ed8a6aac7700ad3549052c90c4bb
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049423"
---
# <a name="provide-your-active-directory-users-access-to-your-claims-aware-applications-and-services"></a>Proporcionar a los usuarios de Active Directory acceso a las aplicaciones y servicios habilitados para notificaciones

Si es administrador en la organización del asociado de cuenta en una implementación de Servicios de federación de Active Directory (AD FS) \( AD FS \) y tiene un objetivo de implementación para proporcionar \- acceso de SSO de inicio de sesión único a los \- \( \) empleados de la red corporativa a los recursos hospedados:

-   Los empleados que iniciaron sesión en un bosque de Active Directory de la red corporativa pueden usar SSO para tener acceso a varias aplicaciones o servicios de la red perimetral de tu propia organización. Estas aplicaciones y servicios están protegidos por AD FS.

    Por ejemplo, es posible que Fabrikam quiera que los empleados de la red corporativa tengan acceso federado a \- las aplicaciones basadas en Web que se hospedan en la red perimetral de fabrikam.

-   Los empleados remotos que han iniciado sesión en un dominio de Active Directory pueden obtener AD FS tokens del servidor de Federación de su organización para obtener acceso federado a AD FS \- \- aplicaciones o servicios basados en Web protegidos que también residen en su organización.

-   Se puede rellenar la información del almacén de atributos de Active Directory en los tokens de AD FS de los empleados.

Los componentes siguientes son necesarios para este objetivo de implementación:

-   **Active Directory Domain Services \( AD DS \) :** AD DS contiene las cuentas de usuario de los empleados que se usan para generar AD FS tokens. La información, como la pertenencia a grupos y los atributos, se rellena en los tokens de AD FS como notificaciones de grupo y notificaciones personalizadas.

    > [!NOTE]
    > También puede usar LDAP de Protocolo ligero de acceso a directorios \( \) o lenguaje de consulta estructurado \( SQL \) para contener las identidades para la generación de tokens de AD FS.

-   **DNS corporativo:** Esta implementación de DNS del sistema de nombres de dominio \( \) contiene un \( registro de recursos de host simple \) para que los clientes de intranet puedan encontrar el servidor de Federación de la cuenta. Esta implementación de DNS también puede hospedar otros registros DNS que se requieren en la red corporativa. Para obtener más información, consulta [Requisitos de resolución de nombres para los servidores de federación](Name-Resolution-Requirements-for-Federation-Servers.md).

-   **Servidor de Federación del asociado de cuenta:** Este servidor de Federación está unido a un dominio del bosque del asociado de cuenta. Autentica las cuentas de usuario de empleados y genera tokens de AD FS. El equipo cliente del empleado realiza la autenticación integrada de Windows en este servidor de Federación para generar un token de AD FS. Para obtener más información, consulte [Review the Role of the Federation Server in the Account Partner](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md).

    El servidor de Federación del asociado de cuenta puede autenticar a los usuarios siguientes:

    -   Empleados con cuentas de usuario en este dominio

    -   Empleados con cuentas de usuario en cualquier sitio de este bosque

    -   Empleados con cuentas de usuario en cualquier parte de los bosques en los que confía este bosque \( a través de una confianza de Windows de dos \- sentidos\)

-   **Empleado:** Un empleado accede a un \- servicio basado en Web \( a través de una aplicación \) o una \- aplicación basada en Web \( a través de un explorador Web compatible \) mientras está conectado a la red corporativa. El equipo cliente del empleado de la red corporativa se comunica directamente con el servidor de Federación para la autenticación.

Después de revisar la información de los temas vinculados, puedes empezar a implementar este objetivo siguiendo los pasos descritos en [Checklist: Implementing a Federated Web SSO Design](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md).

En la siguiente ilustración se muestra cada uno de los componentes necesarios para este objetivo de implementación de AD FS.

![acceso a las notificaciones](media/31394ea8-fecb-4372-ac3f-cc3cf566ffc9.gif)

## <a name="see-also"></a>Consulte también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
