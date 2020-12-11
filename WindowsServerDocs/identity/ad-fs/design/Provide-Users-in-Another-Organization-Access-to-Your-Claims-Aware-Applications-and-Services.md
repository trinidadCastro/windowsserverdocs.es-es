---
description: 'Más información acerca de: proporcionar a los usuarios de otra organización acceso a sus aplicaciones y servicios para notificaciones'
ms.assetid: de7e1e4a-f96d-4b59-ac9b-f65f5d37a96f
title: Proporcionar a los usuarios de otra organización acceso a las aplicaciones y servicios habilitados para notificaciones
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: cf3dcbd1a8c6d14e4866e33e6fab685489df1d45
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049433"
---
# <a name="provide-users-in-another-organization-access-to-your-claims-aware-applications-and-services"></a>Proporcionar a los usuarios de otra organización acceso a las aplicaciones y servicios habilitados para notificaciones


Si es un administrador de la organización del asociado de recurso en Servicios de federación de Active Directory (AD FS) \( AD FS \) y tiene un objetivo de implementación para proporcionar acceso federado para los usuarios de otra organización, \( la organización del asociado de cuenta \) a una aplicación compatible con notificaciones \- o un \- servicio basado en Web que se encuentra en la organización de \( la organización del asociado de recurso \) :

-   Los usuarios federados de su organización y de las organizaciones que han configurado una confianza de Federación a las organizaciones asociadas de la cuenta de la organización \( \) pueden acceder a la AD FS aplicación o servicio protegido que está hospedado en su organización. Para obtener más información, consulte [Federated Web SSO Design](Federated-Web-SSO-Design.md).

    Por ejemplo, es posible que Fabrikam desee que sus empleados de la red corporativa tengan acceso federado a servicios web que se hospedan en Contoso.

-   Los usuarios federados que no tienen ninguna asociación directa con una organización de confianza, como \( los clientes individuales, que han \) iniciado sesión en un almacén de atributos que se hospeda en la red perimetral, pueden tener acceso a varias \- aplicaciones protegidas AD FS, que también se hospedan en la red perimetral, iniciando sesión una vez desde los equipos cliente que se encuentran en Internet. En otras palabras, cuando se hospedan cuentas de clientes para habilitar el acceso a aplicaciones o servicios de la red perimetral, los clientes que se hospedan en un almacén de atributos pueden tener acceso a una o más aplicaciones o servicios de la red perimetral simplemente iniciando sesión una vez. Para obtener más información, consulte [Web SSO Design](Web-SSO-Design.md).

    Por ejemplo, es posible que Fabrikam desee que sus clientes \- tengan \- acceso de inicio de sesión único \( \) a varias aplicaciones o servicios que se hospedan en su red perimetral.

Los componentes siguientes son necesarios para este objetivo de implementación:

-   **Active Directory Domain Services \( AD DS \) :** el servidor de Federación del asociado de recurso debe estar unido a un dominio Active Directory.

-   **DNS perimetral:** DNS del sistema de nombres de dominio \( \) debe contener un \( registro de recursos de host simple \) para que los equipos cliente puedan ubicar el servidor de Federación del asociado de recurso y el servidor Web. El servidor DNS puede hospedar otros registros DNS que también son necesarios en la red perimetral. Para obtener más información, consulta [Requisitos de resolución de nombres para los servidores de federación](Name-Resolution-Requirements-for-Federation-Servers.md).

-   **Servidor de Federación del asociado de recurso:** El servidor de Federación del asociado de recurso valida AD FS tokens que envían los asociados de cuenta. La detección de asociado de cuenta se realiza a través de este servidor de Federación. Para obtener más información, consulte [Review the Role of the Federation Server in the Resource Partner](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md).

-   **Servidor web**: el servidor web puede hospedar una aplicación web o un servicio web. El servidor web confirma que recibe los tokens de AD FS válidos de usuarios federados antes de permitir el acceso a la aplicación o al servicio web protegido.

    Mediante el uso de Windows Identity Foundation \( WIF \) , puede desarrollar su aplicación o servicio web para que acepte solicitudes de inicio de sesión de usuario federado realizadas con cualquier método de inicio de sesión estándar, como el nombre de usuario y la contraseña.

Después de revisar la información de los temas vinculados, puede empezar a implementar este objetivo siguiendo los pasos de la [lista de comprobación: implementar un diseño de SSO Web federado y una](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md) lista de [comprobación: implementar un diseño de SSO Web](../../ad-fs/deployment/Checklist--Implementing-a-Web-SSO-Design.md).

En la siguiente ilustración se muestra cada uno de los componentes necesarios para este objetivo de implementación de AD FS.

![acceso a las notificaciones](media/75358b16-2a6f-4e16-9cc4-b0e614480305.gif)

## <a name="see-also"></a>Consulte también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
