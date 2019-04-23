---
ms.assetid: d254fca3-85a1-424d-ac22-d6687ec3798e
title: Proporcionar a los usuarios de Active Directory acceso a aplicaciones y servicios habilitados para notificaciones
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f6fb37c16c20915c0051e3a24cdb0c147ae92d9c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835876"
---
# <a name="provide-your-active-directory-users-access-to-your-claims-aware-applications-and-services"></a>Proporcionar a los usuarios de Active Directory acceso a aplicaciones y servicios habilitados para notificaciones

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cuando es un administrador en la organización del asociado de cuenta en un Active Directory Federation Services \(AD FS\) implementación y tiene un objetivo de implementación para proporcionar solo\-sesión\-en \( Inicio de sesión único\) acceso para los empleados de la red corporativa a tus recursos hospedados:  
  
-   Los empleados que iniciaron sesión en un bosque de Active Directory de la red corporativa pueden usar SSO para tener acceso a varias aplicaciones o servicios de la red perimetral de tu propia organización. Estas aplicaciones y servicios están protegidos por AD FS.  
  
    Por ejemplo, es posible que Fabrikam desee que los empleados de la red corporativa tengan acceso federado a Web\-en función de las aplicaciones que se hospedan en la red perimetral para Fabrikam.  
  
-   Los empleados remotos que iniciaron sesión a un dominio de Active Directory pueden obtener tokens de AD FS del servidor de federación de su organización para obtener acceso federado a AD FS\-protegido Web\-en función de las aplicaciones o servicios que también residen en su organización.  
  
-   Se puede rellenar la información del almacén de atributos de Active Directory en los tokens de AD FS de los empleados.  
  
Los componentes siguientes son necesarios para este objetivo de implementación:  
  
-   **Servicios de dominio de Active Directory \(AD DS\):** AD DS contiene las cuentas de usuario de los empleados que se usan para generar tokens de AD FS. La información, como la pertenencia a grupos y los atributos, se rellena en los tokens de AD FS como notificaciones de grupo y notificaciones personalizadas.  
  
    > [!NOTE]  
    > También puede usar Lightweight Directory Access Protocol \(LDAP\) o lenguaje de consulta estructurado \(SQL\) para contener las identidades de AD FS generación del token.  
  
-   **DNS corporativo:** Esta implementación de sistema de nombres de dominio \(DNS\) contiene un host simple \(A\) del registro de recursos para que los clientes de intranet puedan localizar el servidor de federación de cuenta. Esta implementación de DNS también puede hospedar otros registros DNS que se requieren en la red corporativa. Para obtener más información, consulte [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md).  
  
-   **Servidor de federación del asociado de cuenta:** Este servidor de federación está unido a un dominio en el bosque del asociado de cuenta. Autentica las cuentas de usuario de empleados y genera tokens de AD FS. El equipo cliente del empleado realiza la autenticación integrada de Windows en este servidor de federación para generar un token de AD FS. Para obtener más información, consulte [Review the Role of the Federation Server in the Account Partner](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md).  
  
    El servidor de federación del asociado de cuenta puede autenticar a los usuarios siguientes:  
  
    -   Empleados con cuentas de usuario en este dominio  
  
    -   Empleados con cuentas de usuario en cualquier sitio de este bosque  
  
    -   Empleados con cuentas de usuario desde cualquier lugar de los bosques de confianza para este bosque \(a través de un dos\-manera de confianza de Windows\)  
  
-   **Empleado:** Un empleado tiene acceso a un sitio Web\-servicio basado en \(a través de una aplicación\) o una Web\-aplicación basada en \(a través de un explorador Web compatible\) mientras que se ha iniciado sesión en el red corporativa. Equipo de cliente del empleado en la red corporativa se comunica directamente con el servidor de federación para la autenticación.  
  
Después de revisar la información de los temas vinculados, puede empezar a implementar este objetivo siguiendo los pasos descritos en [lista de comprobación: Implementar un diseño de SSO Web federado](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md).  
  
La siguiente ilustración muestra cada uno de los componentes necesarios para este objetivo de implementación de AD FS.  
  
![el acceso a sus notificaciones](media/31394ea8-fecb-4372-ac3f-cc3cf566ffc9.gif)  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
