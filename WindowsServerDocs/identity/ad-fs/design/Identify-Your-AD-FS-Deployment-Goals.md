---
ms.assetid: c81b8291-fba5-4b30-a43d-7feb2f4b66be
title: Guía de diseño de AD FS en Windows Server 2012 R2
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2b881553431be873ed9883da67a7989527d7d288
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191269"
---
# <a name="identify-your-ad-fs-deployment-goals"></a>Identificar los objetivos de implementación de AD FS

Identificar correctamente los servicios de federación de Active Directory \(AD FS\) objetivos de implementación es esencial para el éxito de su proyecto de diseño de AD FS. Establecer prioridades y, posiblemente, combine los objetivos de implementación para que puede diseñar e implementar AD FS mediante el uso de un enfoque iterativo. Puede aprovechar las ventajas de las existentes, documenta y predefinidos objetivos de implementación de AD FS que son relevantes para los diseños AD FS y desarrollar una solución que funcione para su situación.  
  
Las versiones anteriores de AD FS se implementaron con más frecuencia para lograr lo siguiente:  
  
-   Proporcionar a los empleados o clientes una web\-basado, la experiencia de SSO al tener acceso a las notificaciones\-en función de las aplicaciones dentro de su empresa.  
  
-   Proporcionar a los empleados o clientes una web\-basado, experiencia de inicio de sesión único para acceder a recursos en cualquier organización del asociado de federación.  
  
-   Proporcionar a los empleados o clientes una Web\-basado, la experiencia de SSO cuando acceso remoto internamente hospeda sitios Web o servicios.  
  
-   Proporcionar a los empleados o clientes una web\-basado, la experiencia de SSO al acceder a recursos o servicios en la nube.  
  
Además, AD FS en Windows Server® 2012 R2 agrega una funcionalidad que puede ayudarle a lograr lo siguiente:  
  
-   Unión al área de trabajo de un dispositivo para la autenticación SSO y de segundo factor sin problemas Esto permite a las organizaciones permitir el acceso desde dispositivos personales del usuario y administrar el riesgo al proporcionar este acceso.  
  
-   Administración de riesgos con múltiples\-factorizar el control de acceso. AD FS proporciona un nivel mayor de autorización que controla quién tiene acceso a determinadas aplicaciones. Esto puede basarse en atributos de usuario \(UPN, correo electrónico, pertenencia al grupo de seguridad, seguridad de la autenticación, etc.\), atributos del dispositivo \(si el dispositivo está unido\) o solicitar atributos \(ubicación de red, la dirección IP o el agente de usuario\).  
  
-   Administración de riesgos con múltiples adicionales\-autenticación de factor para aplicaciones confidenciales. AD FS le permite controlar las directivas para requerir potencialmente multi\-factor de autenticación global o según una función de la aplicación. Además, AD FS ofrece puntos de extensibilidad para cualquier multi\-proveedor factor integre profundamente para un seguro y sin problemas de varios\-factor experiencia para los usuarios finales.  
  
-   Que proporciona capacidades de autenticación y autorización para acceder a recursos web desde la extranet que están protegidos por el Proxy de aplicación Web.  
  
En resumen, se puede implementar AD FS en Windows Server 2012 R2 para lograr los objetivos siguientes en su organización:  
  
### <a name="enable-your-users-to-access-resources-on-their-personal-devices-from-anywhere"></a>Permitir que los usuarios tener acceso a recursos en sus dispositivos personales desde cualquier lugar  
  
-   Unión al área de trabajo que permite a los usuarios unir sus dispositivos personales con Active Directory corporativo y así tener acceso y experiencias sin problemas al obtener acceso a recursos corporativos desde estos dispositivos.  
  
-   Pre\-autenticación de los recursos dentro de la red corporativa que están protegidos por el proxy de aplicación Web y accesibles desde internet.  
  
-   Cambio de contraseña que habilita a los usuarios para cambiar su contraseña desde cualquier dispositivo unido al área de trabajo cuando ha expirado su contraseña, de manera que puedan seguir teniendo acceso a recursos.  
  
### <a name="enhance-your-access-control-risk-management-tools"></a>Mejorar las herramientas de administración de riesgos de control de acceso  
La administración de riesgos es un aspecto importante del control y el cumplimiento en cada organización de TI. Hay control de acceso a numerosas mejoras de administración de riesgos de AD FS en Windows Server® 2012 R2, incluido lo siguiente:  
  
-   Controles flexibles en función de la ubicación de red para controlar cómo se autentica un usuario para tener acceso a AD FS\-aplicación protegida.  
  
-   Directivas flexibles para determinar si un usuario tiene que realizar varias\-autenticación multifactor en función de los datos del usuario, datos del dispositivo y ubicación de red.  
  
-   Por\-control de la aplicación para omitir el SSO y forzar al usuario que proporcione credenciales cada vez que accede a una aplicación confidencial.  
  
-   Flexible por\-basada en directivas de acceso de aplicación en los datos de usuario, datos del dispositivo o ubicación de red.  
  
-   Bloqueo de la extranet de AD FS, que permite que los administradores puedan proteger las cuentas de Active Directory contra los ataques por fuerza bruta desde Internet.  
  
-   Revocación de acceso para cualquier dispositivo unido al área de trabajo que esté deshabilitado o eliminado en Active Directory.  
  
### <a name="use-ad-fs-to-enhance-the-sign-in-experience"></a>Usar AD FS para mejorar el inicio de sesión\-experiencia  
Los siguientes son nuevas capacidades de AD FS en Windows Server® 2012 R2 que permiten al administrador personalizar y mejorar el inicio de sesión\-experiencia:  
  
-   Personalización unificada del servicio AD FS, donde los cambios se realizan una vez y después se propagan automáticamente al resto de los servidores de federación de AD FS en una granja de servidores determinada.  
  
-   Actualiza el inicio de sesión\-en las páginas que tienen un aspecto moderno y abastecen los distintos factores de forma automáticamente.  
  
-   Compatibilidad con la reserva automática para formularios\-basados en la autenticación para dispositivos que no están unidos al dominio corporativo pero que todavía se utilizan generan solicitudes de acceso desde dentro de la red corporativa \(intranet\).  
  
-   Controles simples para personalizar el logotipo de la compañía, la imagen de ilustración, los vínculos estándar para el soporte técnico de TI, la página principal, la privacidad, etc.  
  
-   Personalización de descripción de los mensajes en el inicio de sesión\-en páginas.  
  
-   Personalización de temas web.  
  
-   Detección del dominio de inicio \(HRD\) según sufijo organizativa del usuario para mejorar la privacidad de los socios de la empresa.  
  
-   Filtro de HRD un por\-según la aplicación para seleccionar automáticamente un dominio Kerberos basado en la aplicación.  
  
-   Una\-haga clic en informes de errores para sea más fácil TI solución de problemas.  
  
-   Mensajes de error personalizables.  
  
-   Elección de autenticación del usuario cuando hay más de un proveedor de autenticación disponible.  
  
## <a name="see-also"></a>Vea también  
[Guía de diseño de AD FS en Windows Server 2012 R2](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

