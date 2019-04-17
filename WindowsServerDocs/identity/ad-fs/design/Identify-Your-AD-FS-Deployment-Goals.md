---
ms.assetid: c81b8291-fba5-4b30-a43d-7feb2f4b66be
title: "Guía de diseño de AD FS en Windows Server 2012 R2"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f618add4c4803142b3bd7278908834a412f30999
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="identify-your-ad-fs-deployment-goals"></a>Identificar los objetivos de implementación de AD FS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Es fundamental para el éxito de tu proyecto de diseño de AD FS correctamente identificar los objetivos de implementación de servicios de federación de Active Directory \(AD FS\). Establecer prioridades y, posiblemente, combina los objetivos de implementación para que puedes diseñar e implementar AD FS mediante el uso de un enfoque iterativo. Puedes aprovechar existente, documentado y predefinidos objetivos de la implementación de AD FS que sean relevantes para los diseños de AD FS y desarrollar una solución que funcione para su situación.  
  
Las versiones anteriores de AD FS normalmente se implementaron para lograr lo siguiente:  
  
-   Proporcionar a tus empleados o clientes basados en web\, experiencia SSO al obtener acceso a aplicaciones basadas en claims\ dentro de tu empresa.  
  
-   Proporcionar a tus empleados o clientes una experiencia de inicio de sesión único basada en web\, acceder a los recursos de cualquier organización de partner de federación.  
  
-   Proporcionar a tus empleados o clientes basados en Web\, experiencia SSO internamente el acceso remoto hospedados los sitios Web o servicios.  
  
-   Proporcionar a tus empleados o clientes una experiencia SSO basado en web\ al acceder a recursos o servicios en la nube.  
  
Además, AD FS en Windows Server® 2012 R2 agrega funciones que pueden ayudarte a lograr lo siguiente:  
  
-   Unirse a un área de trabajo para SSO y transparente segundo dispositivo autenticación multifactor. Esto permite a las organizaciones permitir el acceso desde los dispositivos personales del usuario y administrar el riesgo al proporcionar este acceso.  
  
-   Administración de riesgos con el control de acceso multi\ factor. AD FS proporciona un nivel de autorización enriquecido que controla quién tiene acceso a las aplicaciones. Esto puede basarse en los atributos de usuario \ (UPN, correo electrónico, pertenencia al grupo de seguridad, intensidad de la autenticación, etc. \), los atributos del dispositivo \ (si el dispositivo es el área de trabajo joined\) o solicitar atributos \ (ubicación, dirección IP o usuario agente\ de red).  
  
-   Administración de riesgo con la autenticación de factor de multi\ adicional para aplicaciones con información confidencial. AD FS le permite controlar las directivas para potencialmente requerir la autenticación de factor de multi\ globalmente o en según la aplicación. Además, AD FS proporciona puntos de extensibilidad para cualquier proveedor multi\ factor integre estrechamente para ofrecer una experiencia multi\ fases segura y transparente para los usuarios finales.  
  
-   Proporcionar funcionalidades de autenticación y autorización para acceder a recursos web desde la extranet que están protegidos por el Proxy de aplicación Web.  
  
En resumen, se puede implementar AD FS en Windows Server 2012 R2 para lograr los siguientes objetivos de la organización:  
  
### <a name="enable-your-users-to-access-resources-on-their-personal-devices-from-anywhere"></a>Permitir a los usuarios acceder a recursos en sus dispositivos personales desde cualquier lugar  
  
-   Unirse a un área de trabajo que los usuarios pueden unirse a sus dispositivos personales en Active Directory corporativa y así obtener acceso y experiencias sin problemas al obtener acceso a recursos corporativos de estos dispositivos.  
  
-   Autenticación de Pre\ de recursos dentro de la red corporativa que estén protegidos mediante el proxy de aplicación Web y acceder a él desde internet.  
  
-   Cambio de contraseña para permitir que los usuarios pueden cambiar su contraseña desde cualquier dispositivo de estén unidos a un área de trabajo cuando ha caducado su contraseña para que pueda continuar acceder a recursos.  
  
### <a name="enhance-your-access-control-risk-management-tools"></a>Mejorar las herramientas de administración de riesgo de control de acceso  
Administración de riesgo es un aspecto importante de la compatibilidad de cada organización de TI y control. Hay el control de acceso numerosas mejoras de administración de riesgo de AD FS en Windows Server® 2012 R2, incluido lo siguiente:  
  
-   Según la ubicación de red para controlar cómo se autentica un usuario para acceder a una aplicación protegida por AD FS\ de controles flexibles.  
  
-   Directiva flexible para determinar si un usuario necesita realizar la autenticación de factor de multi\ basándose en los datos del usuario, datos de dispositivo y ubicación de red.  
  
-   Control de Per\ aplicación omita SSO y exigir al usuario que proporcione las credenciales cada vez que acceden a una aplicación con información confidencial.  
  
-   Directiva de acceso flexible de per\ aplicación basada en datos de usuario, datos de dispositivo o ubicación de red.  
  
-   AD FS Extranet bloqueo, que permite a los administradores proteger cuentas de Active Directory contra ataques de fuerza bruta de internet.  
  
-   Revocación de acceso para todos los dispositivos estén unidos a un área de trabajo que está deshabilitado o eliminado en Active Directory.  
  
### <a name="use-ad-fs-to-enhance-the-sign-in-experience"></a>Usar AD FS para mejorar la experiencia de sign\  
Estos son nuevas capacidades de AD FS de Windows Server® 2012 R2 que permiten personalizar y mejorar la experiencia de sign\ administrador:  
  
-   Personalización de los servicios de AD FS, donde los cambios se realizan una vez y, a continuación, se propagan automáticamente al resto de los servidores de federación de AD FS en un determinado conjunto de unificado.  
  
-   Sign\ en páginas actualizadas aspecto moderno y atender a todos los diferentes factores de forma automáticamente.  
  
-   Compatibilidad de reserva automática autenticación basada en forms\ para los dispositivos que no están unidos al dominio corporativo pero aún se usan en generar solicitudes de acceso desde dentro de la \(intranet\) red corporativa.  
  
-   Controles simples para personalizar el logotipo de la compañía, imagen de ilustración, vínculos estándar de soporte técnico, página principal, privacidad, etcetera.  
  
-   Personalización de mensajes de la descripción de las páginas de sign\.  
  
-   Personalización de temas de la web.  
  
-   Casa \(HRD\) territorio detección en función de sufijo organizativa del usuario para privacidad mejorada de socios de la empresa.  
  
-   HRD filtrado de forma per\ desde la aplicación para seleccionar automáticamente un territorio en función de la aplicación.  
  
-   Informe de forma más fácil de errores de One\ y haga clic en TI solución de problemas.  
  
-   Mensajes de error personalizables.  
  
-   Elección de autenticación de usuario cuando hay más de un proveedor de autenticación.  
  
## <a name="see-also"></a>Consulta también  
[Guía de diseño de AD FS en Windows Server 2012 R2](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

