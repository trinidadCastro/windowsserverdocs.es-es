---
description: 'Más información sobre: identificación de los objetivos de implementación de AD FS'
ms.assetid: c81b8291-fba5-4b30-a43d-7feb2f4b66be
title: dentify sus objetivos de implementación de AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 2265e3150dfcfe65d4f958fecf22ad9c7e599e15
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049983"
---
# <a name="identify-your-ad-fs-deployment-goals"></a>Identificar los objetivos de implementación de AD FS

La identificación correcta \( de los \) objetivos de implementación de servicios de Federación de Active Directory (AD FS) AD FS es esencial para el éxito del proyecto de diseño de AD FS. Priorizar y, posiblemente, combinar los objetivos de implementación para que pueda diseñar e implementar AD FS mediante un enfoque iterativo. Puede aprovechar las ventajas de los objetivos de implementación de AD FS predefinidos, documentados y predefinidos que son relevantes para los diseños de AD FS y desarrollar una solución de trabajo para su situación.

Las versiones anteriores de AD FS se implementaban con más frecuencia para lograr lo siguiente:

-   Proporcionar a los empleados o clientes una \- experiencia de SSO basada en Web al acceder a \- aplicaciones basadas en notificaciones dentro de la empresa.

-   Proporcionar a los empleados o clientes una \- experiencia de SSO basada en web para tener acceso a los recursos de cualquier organización asociada de Federación.

-   Proporcionar a los empleados o clientes una \- experiencia de SSO basada en Web al obtener acceso remoto a sitios o servicios web hospedados internamente.

-   Proporcionar a los empleados o clientes una \- experiencia de SSO basada en Web al acceder a recursos o servicios en la nube.

Además, AD FS en Windows Server &reg; 2012 R2 agrega funcionalidad que puede ayudarle a lograr lo siguiente:

-   Unión al área de trabajo de un dispositivo para la autenticación SSO y de segundo factor sin problemas Esto permite a las organizaciones permitir el acceso desde dispositivos personales del usuario y administrar el riesgo al proporcionar este acceso.

-   Administración de riesgos con el \- control de acceso multifactor. AD FS proporciona un nivel mayor de autorización que controla quién tiene acceso a determinadas aplicaciones. Esto puede basarse en los atributos de usuario \( UPN, correo electrónico, pertenencia a grupos de seguridad, nivel de autenticación, etc. \) , atributos de dispositivo \( si el dispositivo está unido al área de trabajo \) o atributos de solicitud \( Ubicación de red, dirección IP o agente de usuario \) .

-   Administración de riesgos con \- autenticación multifactor adicional para aplicaciones confidenciales. AD FS permite controlar las directivas para requerir potencialmente multi- \- factor Authentication globalmente o por aplicación. Además, AD FS proporciona puntos de extensibilidad para que cualquier \- proveedor multifactor se integre profundamente para ofrecer una experiencia de multifactor segura y sin problemas a \- los usuarios finales.

-   Proporcionar capacidades de autenticación y autorización para tener acceso a los recursos web desde la extranet que están protegidos por el proxy de aplicación Web.

En Resumen, se pueden implementar AD FS en Windows Server 2012 R2 para lograr los siguientes objetivos de la organización:

### <a name="enable-your-users-to-access-resources-on-their-personal-devices-from-anywhere"></a>Permitir a los usuarios tener acceso a recursos en sus dispositivos personales desde cualquier lugar

-   Unión al área de trabajo que permite a los usuarios unir sus dispositivos personales con Active Directory corporativo y así tener acceso y experiencias sin problemas al obtener acceso a recursos corporativos desde estos dispositivos.

-   \-Autenticación previa de los recursos dentro de la red corporativa que están protegidos por el proxy de aplicación web y a los que se tiene acceso desde Internet.

-   Cambio de contraseña que habilita a los usuarios para cambiar su contraseña desde cualquier dispositivo unido al área de trabajo cuando ha expirado su contraseña, de manera que puedan seguir teniendo acceso a recursos.

### <a name="enhance-your-access-control-risk-management-tools"></a>Mejora de las herramientas de administración de riesgos de control de acceso
La administración de riesgos es un aspecto importante del control y el cumplimiento en cada organización de TI. Hay numerosas mejoras en la administración de riesgos de control de acceso en AD FS en Windows Server &reg; 2012 R2, entre las que se incluyen las siguientes:

-   Controles flexibles basados en la ubicación de red para controlar cómo se autentica un usuario para tener acceso a una AD FS \- aplicación protegida.

-   Directiva flexible para determinar si un usuario tiene que realizar \- la autenticación multifactor en función de los datos del usuario, los datos del dispositivo y la ubicación de red.

-   Por \- control de aplicación para omitir el inicio de sesión único y obligar al usuario a proporcionar credenciales cada vez que accede a una aplicación confidencial.

-   Flexible por \- Directiva de acceso a las aplicaciones según los datos del usuario, los datos del dispositivo o la ubicación de red.

-   Bloqueo de la extranet de AD FS, que permite que los administradores puedan proteger las cuentas de Active Directory contra los ataques por fuerza bruta desde Internet.

-   Revocación de acceso para cualquier dispositivo unido al área de trabajo que esté deshabilitado o eliminado en Active Directory.

### <a name="use-ad-fs-to-enhance-the-sign-in-experience"></a>Usar AD FS para mejorar la \- experiencia de inicio de sesión
A continuación se muestran nuevas capacidades de AD FS en Windows Server &reg; 2012 R2 que permiten al administrador personalizar y mejorar la \- experiencia de inicio de sesión:

-   Personalización unificada del servicio AD FS, donde los cambios se realizan una vez y después se propagan automáticamente al resto de los servidores de federación de AD FS en una granja de servidores determinada.

-   Páginas de inicio de sesión actualizadas \- que tienen un aspecto moderno y que se ajustan a distintos factores de forma automáticamente.

-   Compatibilidad con la reserva automática para \- la autenticación basada en formularios para dispositivos que no están Unidos al dominio corporativo pero que todavía se usan para generar solicitudes de acceso desde dentro de la intranet de la red corporativa \( \) .

-   Controles simples para personalizar el logotipo de la compañía, la imagen de ilustración, los vínculos estándar para el soporte técnico de TI, la página principal, la privacidad, etc.

-   Personalización de los mensajes de descripción en las \- páginas de inicio de sesión.

-   Personalización de temas web.

-   La detección del dominio de inicio \( HRD \) en función del sufijo de la organización del usuario para mejorar la privacidad de los asociados de una empresa.

-   HRD filtrado por \- aplicación para seleccionar automáticamente un dominio Kerberos basado en la aplicación.

-   \-Informes de errores de un clic para facilitar la solución de problemas de ti.

-   Mensajes de error personalizables.

-   Elección de autenticación del usuario cuando hay más de un proveedor de autenticación disponible.

## <a name="see-also"></a>Consulte también
[Guía de diseño de AD FS en Windows Server 2012 R2](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)


