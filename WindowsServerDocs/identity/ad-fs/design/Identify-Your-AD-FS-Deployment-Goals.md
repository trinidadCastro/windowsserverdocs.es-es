---
ms.assetid: c81b8291-fba5-4b30-a43d-7feb2f4b66be
title: Guía de diseño de AD FS en Windows Server 2012 R2
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 1bc898ba858cf49a71edcb89b10981d0bc8c1986
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853078"
---
# <a name="identify-your-ad-fs-deployment-goals"></a>Identificar los objetivos de implementación de AD FS

La identificación correcta de los objetivos de implementación de Servicios de federación de Active Directory (AD FS) \(AD FS\) es esencial para el éxito del proyecto de diseño de AD FS. Priorizar y, posiblemente, combinar los objetivos de implementación para que pueda diseñar e implementar AD FS mediante un enfoque iterativo. Puede aprovechar las ventajas de los objetivos de implementación de AD FS predefinidos, documentados y predefinidos que son relevantes para los diseños de AD FS y desarrollar una solución de trabajo para su situación.  
  
Las versiones anteriores de AD FS se implementaban con más frecuencia para lograr lo siguiente:  
  
-   Proporcionar a los empleados o clientes una experiencia de SSO basada en\-web para acceder a las aplicaciones basadas en\-en su empresa.  
  
-   Proporcionar a los empleados o clientes una experiencia de SSO basada en\-web para tener acceso a los recursos de cualquier organización asociada de Federación.  
  
-   Proporcionar a los empleados o clientes una experiencia de SSO basada en Web\-, al tener acceso remoto a sitios o servicios web hospedados internamente.  
  
-   Proporcionar a los empleados o clientes una experiencia de SSO basada en Web\-, al tener acceso a recursos o servicios en la nube.  
  
Además, AD FS en Windows Server&reg; 2012 R2 agrega funcionalidad que puede ayudarle a lograr lo siguiente:  
  
-   Unión al área de trabajo de un dispositivo para la autenticación SSO y de segundo factor sin problemas Esto permite a las organizaciones permitir el acceso desde dispositivos personales del usuario y administrar el riesgo al proporcionar este acceso.  
  
-   Administración de riesgos con el control de acceso multi\-factor. AD FS proporciona un nivel mayor de autorización que controla quién tiene acceso a determinadas aplicaciones. Esto puede basarse en atributos de usuario \(UPN, correo electrónico, pertenencia a grupos de seguridad, nivel de autenticación, etc.\), atributos de dispositivo \(si el dispositivo está unido al área de trabajo\) o atributos de solicitud \(ubicación de red, dirección IP o agente de usuario\).  
  
-   Administración de riesgos con autenticación de varios\-factor adicional para aplicaciones confidenciales. AD FS le permite controlar las directivas para requerir potencialmente multi\-factor Authentication globalmente o en función de cada aplicación. Además, AD FS proporciona puntos de extensibilidad para que cualquier proveedor de varios\-factor se integre profundamente para ofrecer una experiencia de multi\-sin problemas y segura para los usuarios finales.  
  
-   Proporcionar capacidades de autenticación y autorización para tener acceso a los recursos web desde la extranet que están protegidos por el proxy de aplicación Web.  
  
En Resumen, se pueden implementar AD FS en Windows Server 2012 R2 para lograr los siguientes objetivos de la organización:  
  
### <a name="enable-your-users-to-access-resources-on-their-personal-devices-from-anywhere"></a>Permitir a los usuarios tener acceso a recursos en sus dispositivos personales desde cualquier lugar  
  
-   Unión al área de trabajo que permite a los usuarios unir sus dispositivos personales con Active Directory corporativo y así tener acceso y experiencias sin problemas al obtener acceso a recursos corporativos desde estos dispositivos.  
  
-   \-la autenticación previa de los recursos dentro de la red corporativa que están protegidos por el proxy de aplicación web y a los que se tiene acceso desde Internet.  
  
-   Cambio de contraseña que habilita a los usuarios para cambiar su contraseña desde cualquier dispositivo unido al área de trabajo cuando ha expirado su contraseña, de manera que puedan seguir teniendo acceso a recursos.  
  
### <a name="enhance-your-access-control-risk-management-tools"></a>Mejora de las herramientas de administración de riesgos de control de acceso  
La administración de riesgos es un aspecto importante del control y el cumplimiento en cada organización de TI. Hay numerosas mejoras en la administración de riesgos de control de acceso en AD FS en Windows Server&reg; 2012 R2, entre las que se incluyen las siguientes:  
  
-   Controles flexibles basados en la ubicación de red para controlar cómo se autentica un usuario para tener acceso a una AD FS\-aplicación protegida.  
  
-   Directiva flexible para determinar si un usuario debe realizar la autenticación de varios\-factor en función de los datos del usuario, los datos del dispositivo y la ubicación de red.  
  
-   Por\-control de aplicaciones para omitir el inicio de sesión único y obligar al usuario a proporcionar credenciales cada vez que accede a una aplicación confidencial.  
  
-   Flexible por\-Directiva de acceso a la aplicación basada en datos de usuario, datos de dispositivo o ubicación de red.  
  
-   Bloqueo de la extranet de AD FS, que permite que los administradores puedan proteger las cuentas de Active Directory contra los ataques por fuerza bruta desde Internet.  
  
-   Revocación de acceso para cualquier dispositivo unido al área de trabajo que esté deshabilitado o eliminado en Active Directory.  
  
### <a name="use-ad-fs-to-enhance-the-sign-in-experience"></a>Usar AD FS para mejorar el\-de inicio de sesión  
A continuación se muestran nuevas capacidades de AD FS en Windows Server&reg; 2012 R2 que permiten al administrador personalizar y mejorar el signo\-en la experiencia:  
  
-   Personalización unificada del servicio AD FS, donde los cambios se realizan una vez y después se propagan automáticamente al resto de los servidores de federación de AD FS en una granja de servidores determinada.  
  
-   \-de firma actualizada en páginas que tienen un aspecto moderno y que se ajustan a diferentes factores de forma automáticamente.  
  
-   Compatibilidad con la reserva automática para formularios\-la autenticación basada en dispositivos que no están Unidos al dominio corporativo pero que todavía se usan para generar solicitudes de acceso desde dentro de la red corporativa \(\)de la intranet.  
  
-   Controles simples para personalizar el logotipo de la compañía, la imagen de ilustración, los vínculos estándar para el soporte técnico de TI, la página principal, la privacidad, etc.  
  
-   Personalización de los mensajes de descripción en el\-firmar en páginas.  
  
-   Personalización de temas web.  
  
-   La detección del dominio de inicio \(HRD\) basándose en el sufijo de la organización del usuario para mejorar la privacidad de los asociados de la empresa.  
  
-   El filtrado de HRD en función de la aplicación por\-para elegir automáticamente un dominio Kerberos basado en la aplicación.  
  
-   Una\-haga clic en informes de errores para facilitar la solución de problemas.  
  
-   Mensajes de error personalizables.  
  
-   Elección de autenticación del usuario cuando hay más de un proveedor de autenticación disponible.  
  
## <a name="see-also"></a>Consulta también  
[Guía de diseño de AD FS en Windows Server 2012 R2](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

