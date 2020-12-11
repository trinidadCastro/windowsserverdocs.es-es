---
description: 'Más información acerca de: diseño de SSO Web federado'
ms.assetid: 09f335bb-896a-45dd-adc2-f215b8fba828
title: Federated Web SSO Design
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 3551ab3d14b479750d5338580aa1c43660da1623
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97047723"
---
# <a name="federated-web-sso-design"></a>Federated Web SSO Design

El inicio de sesión único Web federado en el \- \- \( \) diseño de SSO en servicios de Federación de Active Directory (AD FS) \( AD FS \) implica una comunicación segura que abarca varios firewalls, redes perimetrales y \- servidores de resolución de nombres, además de toda la infraestructura de enrutamiento de Internet.

Normalmente, este diseño se utiliza cuando dos organizaciones acuerdan crear una relación de confianza de Federación para permitir que los usuarios de una organización \( \) tengan acceso a \- aplicaciones o servicios basados en Web, que están protegidos por AD FS, en la otra organización de \( la organización del asociado de recurso \) .

En otras palabras, una relación de confianza de Federación es la encarnación de un acuerdo de nivel de negocio \- o una asociación entre dos organizaciones. Como se muestra en la siguiente ilustración, puede establecer una relación de confianza de Federación entre dos empresas, lo que resulta en un escenario de Federación de un extremo \- a \- otro.

![SSO Web federado](media/adfs2_FederatedWebSSODesign.gif)

La \- flecha unidireccional de la ilustración indica la dirección de la confianza de Federación, que, como la dirección de las confianzas de Windows, siempre señala al lado de la cuenta del bosque. Esto significa que la autenticación fluye desde la organización del asociado de cuenta a la organización del asociado de recurso.

En este diseño de SSO Web federado, dos servidores \( de Federación uno en Fabrikam y el otro en Contoso \) dirigen las solicitudes de autenticación de las cuentas de usuario de Fabrikam a \- aplicaciones o servicios basados en Web de contoso.

> [!NOTE]
> Para obtener seguridad adicional, puede usar servidores proxy de Federación para retransmitir las solicitudes a los servidores de Federación que no son accesibles directamente desde Internet.

En este ejemplo, Fabrikam es el proveedor de identidad o cuenta. La parte de Fabrikam del diseño de SSO Web federado usa el siguiente objetivo de implementación de AD FS:

-   [Proporcionar a los usuarios de Active Directory acceso a aplicaciones y servicios de otras organizaciones](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)

Contoso es el proveedor de recursos. La parte de Contoso del diseño de SSO Web federado logra los siguientes objetivos de implementación de AD FS:

-   [Proporcionar a los usuarios de otra organización acceso a aplicaciones y servicios habilitados para notificaciones](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)

-   [Proporcionar a los usuarios de Active Directory acceso a aplicaciones y servicios habilitados para notificaciones](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)

Para obtener una lista de tareas detalladas que puedes utilizar para planear e implementar el diseño de SSO web, consulta [Checklist: Implementing a Federated Web SSO Design](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md).

## <a name="see-also"></a>Consulte también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
