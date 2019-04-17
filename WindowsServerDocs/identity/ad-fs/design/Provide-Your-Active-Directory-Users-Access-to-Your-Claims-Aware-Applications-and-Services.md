---
ms.assetid: d254fca3-85a1-424d-ac22-d6687ec3798e
title: Proporcionar el acceso a los usuarios de Active Directory a los servicios y las aplicaciones de notificaciones
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2eb68ecd9df363481ab9cc7f0281a7d161963732
ms.sourcegitcommit: 877a50cd8d6e727048cdfac9b614a98ac3220876
ms.translationtype: HT
ms.contentlocale: es-ES
---
# <a name="provide-your-active-directory-users-access-to-your-claims-aware-applications-and-services"></a>Proporcionar el acceso a los usuarios de Active Directory a los servicios y las aplicaciones de notificaciones

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cuando es un administrador de la organización de partner de la cuenta en una implementación de servicios de federación de Active Directory \(AD FS\) y tiene un objetivo de implementación para proporcionar single\ sign\ en \(SSO\) acceso para los empleados en la red corporativa a los recursos hospedadas:  
  
-   Los empleados que han iniciado sesión un bosque de Active Directory de la red corporativa pueden usar SSO para acceder a varias aplicaciones o servicios en la red perimetral en su propia organización. Estas aplicaciones y servicios se protegen mediante AD FS.  
  
    Por ejemplo, Fabrikam puede los empleados de la red corporativa tengan acceso a aplicaciones basadas en Web\ que están hospedados en la red perimetral de Fabrikam federado.  
  
-   Los empleados remotos que han iniciado sesión a un dominio de Active Directory pueden obtener tokens de AD FS desde el servidor de federación de la organización para obtener acceso federado a AD FS\ protegidas Web\ aplicaciones o servicios que también se encuentran en la organización.  
  
-   Los tokens de AD FS de los empleados de se puede rellenar información en el almacén de atributo de Active Directory.  
  
Los siguientes componentes son necesarios para este objetivo de implementación:  
  
-   **Active \(AD DS\) directorio los servicios de dominio:** AD DS contiene las cuentas de usuario de los empleados que se usan para generar los tokens de AD FS. Obtener información, como las pertenencias a grupos y los atributos, se llena en tokens de AD FS como notificaciones de grupo y notificaciones personalizadas.  
  
    > [!NOTE]  
    > También puedes usar el protocolo ligero de acceso a directorios \(LDAP\) o lenguaje de consulta estructurado \(SQL\) para contener las identidades para la generación de token de AD FS.  
  
-   **DNS corporativa:** esta implementación de sistema de nombres de dominio \(DNS\) contiene un registro de host simple \(A\) recursos para que los clientes de intranet pueden encontrar el servidor de federación de cuenta. Esta implementación de DNS también puede hospedar otros registros DNS que son necesarios en la red corporativa. Para obtener más información, consulta [requisitos de resolución de nombres para los servidores de federación](Name-Resolution-Requirements-for-Federation-Servers.md).  
  
-   **Servidor de federación de asociado de cuenta:** este servidor de federación está unido a un dominio en el bosque de asociado de la cuenta. Autentica cuentas de usuario de los empleados y genera tokens de AD FS. El equipo cliente para el empleado realiza la autenticación integrada de Windows con este servidor de federación para generar un token de AD FS. Para obtener más información, consulta [revisar el rol de servidor de federación de la cuenta de Partner](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md).  
  
    El servidor de federación de asociado de cuenta puede autenticar los usuarios siguientes:  
  
    -   Empleados con cuentas de usuario en este dominio  
  
    -   Empleados con cuentas de usuario en cualquier lugar en el bosque  
  
    -   Los empleados con cuentas de usuario en cualquier lugar bosques que son de confianza para este bosque \ (a través de un trust\ de Windows de forma de two\)  
  
-   **Los empleados:** un empleado tiene acceso a un servicio basado en Web\ \(through an application\) o una aplicación basada en Web\ \ (a través de un browser\ Web compatible) mientras que está conectado a la red corporativa. El equipo del empleado cliente en la red corporativa se comunica directamente con el servidor de federación para la autenticación.  
  
Después de revisar la información de los temas vinculados, puedes empezar a implementar este objetivo siguiendo los pasos de [lista de comprobación: implementar un diseño de inicio de sesión ÚNICO Web federados](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md).  
  
La siguiente ilustración muestra cada uno de los componentes necesarios para este objetivo de implementación de AD FS.  
  
![Acceso a las reclamaciones](media/31394ea8-fecb-4372-ac3f-cc3cf566ffc9.gif)  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
