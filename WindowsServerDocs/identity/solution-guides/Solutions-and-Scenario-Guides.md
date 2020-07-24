---
ms.assetid: bdb9ad4b-139c-4031-8f26-827432779829
title: Guías de soluciones y situaciones
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 02f0bb99b4948810c153bf1109ba4f04151b3030
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86954197"
---
# <a name="solutions-and-scenario-guides"></a>Guías de soluciones y situaciones

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
 
  
Con las soluciones de protección de información y acceso de Microsoft, puede implementar y configurar el acceso a los recursos corporativos en el entorno local y en las aplicaciones en la nube. Y puede hacerlo al mismo tiempo que protege la información de la empresa.  
  
Protección de acceso e información  
  
|Guía|¿Cómo puede ayudarle esta guía?                                                                                                                                                                                                                                                                                                                                                                                                    
|-----|-----  
| [Acceso seguro a los recursos de la compañía desde cualquier ubicación y dispositivo](/previous-versions/windows/it-pro/solutions-guidance/dn550982(v=ws.11))|En esta guía se muestra cómo permitir que los empleados usen dispositivos personales y de empresa para acceder de forma segura a las aplicaciones y los datos corporativos.                                                                                                                                                                                    
| [Unirse a un área de trabajo desde cualquier dispositivo para SSO y autenticación de segundo factor sin problemas en todas las aplicaciones de la compañía](../ad-fs/operations/join-to-workplace-from-any-device-for-sso-and-seamless-second-factor-authentication-across-company-applications.md) | Los empleados pueden acceder a aplicaciones y datos en cualquier lugar, en cualquier dispositivo. Los empleados pueden usar el inicio de sesión único en aplicaciones de explorador o en aplicaciones empresariales. Los administradores pueden controlar quién tiene acceso a los recursos de la compañía según la aplicación, el usuario, el dispositivo y la ubicación.                                        
| [Administración de riesgos con la autenticación multifactor adicional para aplicaciones confidenciales](../ad-fs/operations/manage-risk-with-additional-multi-factor-authentication-for-sensitive-applications.md)| En este escenario, habilitará MFA basándose en los datos de pertenencia a grupos del usuario para una aplicación específica. Es decir, configurará una directiva de autenticación en el servidor de federación que requiera MFA cuando los usuarios que pertenezcan a un grupo concreto soliciten acceso a una aplicación específica hospedada en un servidor web.  
| [Administración de riesgos con control de acceso condicional](../ad-fs/operations/manage-risk-with-conditional-access-control.md) | El control de acceso en AD FS se implementa con reglas de notificación de autorización de emisión que se usan para emitir un permiso o denegar las notificaciones que determinarán si se permitirá a un usuario o a un grupo de usuarios acceder a los recursos protegidos AD FS. Las reglas de autorización solo se pueden establecer en relaciones de confianza para usuario autenticado.
|[Configuración del Servicio web de inscripción de certificados para la renovación basada en claves de certificados en un puerto personalizado](certificate-enrollment-certificate-key-based-renewal.md)|En este artículo se proporcionan instrucciones paso a paso para implementar el Servicio web de inscripción de certificados (o la Directiva de inscripción de certificados (CEP)/servicio de inscripción de certificados (CES)) en un puerto personalizado distinto de 443 para la renovación basada en claves de certificado para aprovechar la característica de renovación automática de procesamiento de eventos complejos y CES. |
