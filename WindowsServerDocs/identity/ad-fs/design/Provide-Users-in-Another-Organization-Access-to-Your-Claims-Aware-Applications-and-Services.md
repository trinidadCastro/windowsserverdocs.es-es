---
ms.assetid: de7e1e4a-f96d-4b59-ac9b-f65f5d37a96f
title: Proporcionar a los usuarios de otra organización acceso a aplicaciones y servicios habilitados para notificaciones
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a0b2429599036f2893f23df7921a11c8232d9f67
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359069"
---
# <a name="provide-users-in-another-organization-access-to-your-claims-aware-applications-and-services"></a>Proporcionar a los usuarios de otra organización acceso a aplicaciones y servicios habilitados para notificaciones


Cuando es administrador en la organización del asociado de recurso en Servicios de federación de Active Directory (AD FS) \(AD FS @ no__t-1 y tiene un objetivo de implementación para proporcionar acceso federado para los usuarios de otra organización @no__t asociado de cuenta de 2The Organization @ no__t-3 para una aplicación Claims @ no__t-4aware o un servicio Web @ no__t-5based que se encuentra en su organización \(The Resource Partner Organization @ no__t-7:  
  
-   Los usuarios federados de su organización y de las organizaciones que han configurado una confianza de Federación a su organización @no__t las organizaciones de asociados de 0account-no__t-1 pueden acceder a la AD FS aplicación o servicio protegido que está hospedado en su organización. Para obtener más información, consulte [Federated Web SSO Design](Federated-Web-SSO-Design.md).  
  
    Por ejemplo, es posible que Fabrikam desee que sus empleados de la red corporativa tengan acceso federado a servicios web que se hospedan en Contoso.  
  
-   Los usuarios federados que no tienen ninguna asociación directa con una organización de confianza @no__t 0such como clientes individuales @ no__t-1, que han iniciado sesión en un almacén de atributos que se hospeda en la red perimetral, pueden tener acceso a varias aplicaciones AD FS @ no__t-2secured. que también se hospedan en la red perimetral, iniciando sesión una vez desde los equipos cliente que se encuentran en Internet. En otras palabras, cuando se hospedan cuentas de clientes para habilitar el acceso a aplicaciones o servicios de la red perimetral, los clientes que se hospedan en un almacén de atributos pueden tener acceso a una o más aplicaciones o servicios de la red perimetral simplemente iniciando sesión una vez. Para obtener más información, consulte [Web SSO Design](Web-SSO-Design.md).  
  
    Por ejemplo, es posible que Fabrikam desee que sus clientes tengan acceso único a @ no__t-0sign @ no__t-1ON \(SSO @ no__t-3 a varias aplicaciones o servicios que se hospedan en su red perimetral.  
  
Los componentes siguientes son necesarios para este objetivo de implementación:  
  
-   **Active Directory Domain Services \(AD DS @ no__t-2:** El servidor de Federación del asociado de recurso debe estar unido a un dominio de Active Directory.  
  
-   **DNS perimetral:** El sistema de nombres de dominio \(DNS @ no__t-1 debe contener un registro de recursos de host simple \(A @ no__t-3 para que los equipos cliente puedan encontrar el servidor de Federación del asociado de recurso y el servidor Web. El servidor DNS puede hospedar otros registros DNS que también son necesarios en la red perimetral. Para obtener más información, consulte [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md).  
  
-   **Servidor de Federación del asociado de recurso:** El servidor de Federación del asociado de recurso valida AD FS tokens que envían los asociados de cuenta. La detección de asociado de cuenta se realiza a través de este servidor de Federación. Para obtener más información, consulte [revisar el rol del servidor de federación del asociado de recurso](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md).  
  
-   **Servidor web:** El servidor web puede hospedar una aplicación web o un servicio web. El servidor web confirma que recibe los tokens de AD FS válidos de usuarios federados antes de permitir el acceso a la aplicación o al servicio web protegido.  
  
    Mediante el uso de Windows Identity Foundation \(WIF @ no__t-1, puede desarrollar su aplicación o servicio web para que acepte solicitudes de inicio de sesión de usuario federado realizadas con cualquier método de inicio de sesión estándar, como el nombre de usuario y la contraseña.  
  
Después de revisar la información de los temas vinculados, puede empezar a implementar este objetivo siguiendo los pasos descritos en @no__t 0Checklist: Implementación de un diseño de SSO Web federado @ no__t-0 y [Checklist: Implementar un diseño de SSO Web @ no__t-0.  
  
En la siguiente ilustración se muestra cada uno de los componentes necesarios para este objetivo de implementación de AD FS.  
  
![acceso a las notificaciones](media/75358b16-2a6f-4e16-9cc4-b0e614480305.gif)  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
