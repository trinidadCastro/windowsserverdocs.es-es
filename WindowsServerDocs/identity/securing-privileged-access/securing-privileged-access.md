---
title: Protección del acceso con privilegios
description: Enfoque por fases para proteger el acceso con privilegios
ms.prod: windows-server
ms.topic: conceptual
ms.assetid: f5dec0c2-06fe-4c91-9bdc-67cc6a3ede60
ms.date: 02/25/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: mas
ms.openlocfilehash: e6ff22d0563fa11aa633004966b2cd2648ba5877
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357704"
---
# <a name="securing-privileged-access"></a>Protección del acceso con privilegios

>Se aplica a: Windows Server

La protección del acceso con privilegios es un primer paso crítico para establecer controles de seguridad para los recursos empresariales de una organización moderna. La seguridad de la mayoría o de todos los activos empresariales de una organización de ti depende de la integridad de las cuentas con privilegios que se usan para administrar, administrar y desarrollar. Los atacantes a menudo dirigen estas cuentas y otros elementos de acceso con privilegios para obtener acceso a los datos y sistemas mediante ataques de robo de credenciales como [Pass-The-hash y pass-The-ticket](https://www.microsoft.com/pth).

Para proteger el acceso con privilegios de los adversarios determinado, es necesario tomar un enfoque completo y exhaustivo para aislar estos sistemas de los riesgos.

## <a name="what-are-privileged-accounts"></a>¿Qué son las cuentas con privilegios?

Antes de hablar acerca de cómo protegerlos, permite definir cuentas con privilegios.

Las cuentas con privilegios, como los administradores de Active Directory Domain Services, tienen acceso directo o indirecto a la mayoría o a todos los recursos de una organización de ti, lo que pone en peligro estas cuentas un riesgo empresarial significativo.

## <a name="why-securing-privileged-access-is-important"></a>¿Por qué es importante proteger el acceso con privilegios?

Los atacantes cibernéticos se centran en el acceso con privilegios a sistemas como Active Directory (AD) para obtener acceso rápidamente a todos los datos de las organizaciones de destino. Los enfoques de seguridad tradicionales se han centrado en la red y los firewalls como el perímetro de seguridad principal, pero la eficacia de la seguridad de la red ha disminuido significativamente en dos tendencias:

* Las organizaciones hospedan datos y recursos fuera de los límites de red tradicionales en equipos de empresas móviles, dispositivos como teléfonos móviles y tabletas, servicios en la nube y traiga sus propios dispositivos (BYOD).
* Los adversarios han demostrado una habilidad constante y sistemática para obtener acceso a estaciones de trabajo dentro de los límites de red por medio de ataques de suplantación de identidad (phishing) y otros ataques web y de correo electrónico.

Estos factores requieren la creación de un perímetro de seguridad moderno fuera de los controles de identidad de autenticación y autorización, además de la estrategia de perímetro de red tradicional. Aquí se define un perímetro de seguridad como un conjunto coherente de controles entre los recursos y las amenazas. Las cuentas con privilegios tienen el control efectivo de este nuevo perímetro de seguridad, por lo que es fundamental proteger el acceso con privilegios.

![Diagrama que muestra la capa de identidad de la organización](../media/securing-privileged-access/PAW_LP_Fig2.JPG)

Un atacante que gana el control de una cuenta administrativa puede usar esos privilegios para aumentar su impacto en la organización de destino, como se muestra a continuación:

![Diagrama que muestra cómo un adversario que toma el control de una cuenta administrativa puede usar esos privilegios para perseguir su beneficio a costa de la organización objetivo](../media/securing-privileged-access/PAW_LP_Fig3.JPG)

En la ilustración siguiente se muestran dos rutas de acceso:

* Una ruta de acceso "azul" donde se usa una cuenta de usuario estándar para el acceso sin privilegios a recursos como el correo electrónico y la exploración Web y el trabajo cotidiano.

   > [!NOTE]
   > Los elementos de la ruta de acceso azul descritos más adelante indican amplias protecciones del entorno que se extienden más allá de las cuentas administrativas.

* Una ruta de acceso "roja" en la que se produce el acceso con privilegios en un dispositivo protegido para reducir el riesgo de suplantación de identidad (phishing) y otros ataques web y de correo electrónico.

![Diagrama que muestra la "ruta de acceso" independiente para la administración que el mapa de ruta establece para aislar las tareas de acceso con privilegios de las tareas de usuario estándar de alto riesgo, como exploración Web y acceso al correo electrónico](../media/securing-privileged-access/PAW_LP_Fig4.JPG)

## <a name="securing-privileged-access-roadmap"></a>Protección del mapa de ruta de acceso con privilegios

El mapa de ruta está diseñado para maximizar el uso de las tecnologías de Microsoft que ya ha implementado, aprovechar las ventajas de las tecnologías en la nube para mejorar la seguridad e integrar las herramientas de seguridad de terceros que ya haya implementado.

La guía básica de recomendaciones de Microsoft se divide en tres fases:

* [Fase 1: Primeros 30 días]()
   * Gana rápidamente con un impacto positivo significativo.
* [Fase 2: 90 días]()
   * Mejoras incrementales significativas.
* [Fase 3: Pendientes]()
   * Mejora de la seguridad y mantenimiento.

El mapa de ruta está clasificado en orden de prioridad para programar las implementaciones más rápidas y efectivas primero en función de nuestras experiencias con estos ataques y la implementación de soluciones. 

Microsoft recomienda seguir este plan de ruta para proteger el acceso con privilegios frente a determinados adversarios. Este plan se puede ajustar para adaptarlo a las funcionalidades existentes y los requisitos específicos de su organización.

> [!NOTE]
> Para proteger el acceso con privilegios se requiere una amplia variedad de elementos, como componentes técnicos (defensas de host, protecciones de cuentas, administración de identidades, etc.), así como cambios en los procesos, las prácticas y el conocimiento en materia de administración. Las escalas de tiempo del mapa de ruta son aproximadas y se basan en nuestra experiencia con las implementaciones de clientes. La duración variará en cada organización según la complejidad del entorno y los procesos de administración de cambios.

## <a name="phase-1-quick-wins-with-minimal-operational-complexity"></a>Fase 1: Gana rápidamente con una complejidad operativa mínima

La fase 1 del mapa de ruta se centra en mitigar rápidamente las técnicas de ataque que se usan con más frecuencia de robo de credenciales y abuso. La fase 1 está diseñada para implementarse en unos 30 días aproximadamente y se representa en este diagrama:

![Diagrama de fase 1: 1. Independiente de la cuenta de administrador y de usuario, 2. Contraseñas de administrador local Just-in-Time, 3. Fase 1, 4 de la estación de trabajo de administración. Detección de ataques de identidad](../media/securing-privileged-access/PAW_LP_Fig6.JPG)

### <a name="1-separate-accounts"></a>1. Cuentas independientes

Para ayudar a separar los riesgos de Internet (ataques de phishing, exploración Web) de las cuentas de acceso con privilegios, cree una cuenta dedicada para todo el personal con acceso con privilegios. Los administradores no deben explorar la web, comprobar su correo electrónico y realizar las tareas de productividad diarias con cuentas con privilegios elevados. Puede encontrar más información en la sección [cuentas administrativas independientes](securing-privileged-access-reference-material.md#separate-administrative-accounts) del documento de referencia.

Siga las instrucciones del artículo [Administración de cuentas de acceso de emergencia en Azure ad](/azure/active-directory/users-groups-roles/directory-emergency-access) para crear al menos dos cuentas de acceso de emergencia, con derechos de administrador asignados de forma permanente en los entornos locales de AD y Azure ad. Estas cuentas solo se usan cuando las cuentas de administrador tradicionales no pueden realizar una tarea necesaria, como en el caso de un desastre.

### <a name="2-just-in-time-local-admin-passwords"></a>2. Contraseñas de administrador local Just-in-Time

Para mitigar el riesgo de que un adversario robe un hash de contraseña de la cuenta de administrador local de la base de datos SAM local y se abusa de él para atacar a otros equipos, las organizaciones deben asegurarse de que cada equipo tenga una contraseña de administrador local única. La herramienta de solución de contraseña de administrador local (LAPS) puede configurar contraseñas aleatorias únicas en cada estación de trabajo y almacenarlas en Active Directory (AD) protegidas mediante una ACL. Solo los usuarios autorizados válidos pueden leer o solicitar el restablecimiento de estas contraseñas de cuenta de administrador local. Puede obtener los intervalos para su uso en estaciones de trabajo y servidores desde [el centro de descarga de Microsoft](http://Aka.ms/LAPS).

Encontrará orientación adicional para el funcionamiento de un entorno con LAPS y huellas en la sección [estándares operativos basados en el principio de origen limpio](securing-privileged-access-reference-material.md#operational-standards-based-on-clean-source-principle).

### <a name="3-administrative-workstations"></a>3. Estaciones de trabajo administrativas

Como medida de seguridad inicial para los usuarios con Azure Active Directory y los privilegios de administrador Active Directory locales tradicionales, asegúrese de que usan dispositivos Windows 10 configurados con los [estándares para un dispositivo Windows 10 de alta seguridad. ](/windows-hardware/design/device-experiences/oem-highly-secure). Las cuentas de administrador con privilegios no deben ser miembros del grupo de administradores locales de las estaciones de trabajo administrativas.  La elevación de privilegios a través de Access Control de usuario (UAC) se puede usar cuando se requieren cambios de configuración en las estaciones de trabajo.  Además, la línea de base de seguridad de Windows 10 debe aplicarse a las estaciones de trabajo para reforzar aún más el dispositivo.

### <a name="4-identity-attack-detection"></a>4. Detección de ataques de identidad

[Protección contra amenazas avanzada de Azure (ATP)](/azure-advanced-threat-protection/what-is-atp) es una solución de seguridad basada en la nube que identifica, detecta y ayuda a investigar amenazas avanzadas, identidades en peligro y acciones de Insider maliciosas dirigidas a su Active Directory local entorno.

## <a name="phase-2-significant-incremental-improvements"></a>Fase 2: Mejoras incrementales significativas

La fase 2 se basa en el trabajo realizado en la fase 1 y está diseñado para completarse en aproximadamente 90 días. Los pasos de esta fase se representan en este diagrama:

![Diagrama de la fase 2: 1. Windows Hello para empresas/MFA, 2. Implementación de pata, 3. Privilegios Just-in-Time, 4. Credential Guard, 5. Credenciales perdidas, 6. Detección de vulnerabilidades de movimiento lateral](../media/securing-privileged-access/PAW_LP_Fig7.JPG)

### <a name="1-require-windows-hello-for-business-and-mfa"></a>1. Requerir Windows Hello para empresas y MFA

Los administradores pueden beneficiarse de la facilidad de uso asociada con Windows Hello para empresas. Los administradores pueden reemplazar sus contraseñas complejas con una autenticación de dos factores sólida en sus equipos. Un atacante debe tener el dispositivo y la información biométrica o el PIN, es mucho más difícil obtener acceso sin el conocimiento del empleado. Puede encontrar más información acerca de Windows Hello para empresas y la ruta de acceso que se va a implementar en el artículo [Introducción a Windows Hello para empresas](/windows/security/identity-protection/hello-for-business/hello-overview) .

Habilite multi-factor Authentication (MFA) para sus cuentas de administrador en Azure AD con Azure MFA. Como mínimo, habilite la [Directiva de acceso condicional de protección de línea de base](/azure/active-directory/conditional-access/baseline-protection#require-mfa-for-admins) . puede encontrar más información sobre Azure multi-factor Authentication en el artículo implementación de [Azure multi-factor Authentication basado](/azure/active-directory/authentication/howto-mfa-getstarted) en la nube

### <a name="2-deploy-paw-to-all-privileged-identity-access-account-holders"></a>2. Implementación de pata en todos los titulares de cuentas con privilegios de acceso a identidad

Continuando el proceso de separar las cuentas con privilegios de las amenazas que se encuentran en el correo electrónico, la exploración Web y otras tareas no administrativas, debe implementar estaciones de trabajo de acceso con privilegios (pata) dedicadas para todo el personal con acceso con privilegios a su sistemas de información de la organización. Puede encontrar más información sobre la implementación de pata en el artículo [estaciones de trabajo de acceso con privilegios](privileged-access-workstations.md#paw-phased-implementation).

### <a name="3-just-in-time-privileges"></a>3. Privilegios Just-in-Time

Para reducir el tiempo de exposición de los privilegios y aumentar la visibilidad de su uso, proporcione privilegios Just-in-Time (JIT) mediante una solución adecuada, como las siguientes u otras soluciones de terceros:

* Para Servicios de dominio de Active Directory (AD DS), use la funcionalidad [Privileged Access Manager (PAM)](/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services) de Microsoft Identity Manager (MIM).
* Para Azure Active Directory, use la funcionalidad [Azure AD Privileged Identity Management (PIM)](/azure/active-directory/privileged-identity-management/pim-deployment-plan).

### <a name="4-enable-windows-defender-credential-guard"></a>4. Habilitar Credential Guard de Windows Defender

Habilitar Credential Guard ayuda a proteger los hash de contraseñas de NTLM, los vales de concesión de vales de Kerberos y las credenciales almacenadas por las aplicaciones como credenciales de dominio. Esta funcionalidad ayuda a evitar ataques de robo de credenciales, como Pass-The-hash o Pass-The-ticket al aumentar la dificultad de dinamización en el entorno mediante credenciales robadas. Puede encontrar información sobre cómo funciona Credential Guard y cómo realizar la implementación en el artículo [proteger las credenciales de dominio derivadas con protección de credenciales de Windows Defender](/windows/security/identity-protection/credential-guard/credential-guard).

### <a name="5-leaked-credentials-reporting"></a>5. Informes de credenciales perdidas

"Todos los días, Microsoft analiza más de 6,5 billones señales con el fin de identificar las amenazas emergentes y proteger a los clientes": [Microsoft por los números](https://news.microsoft.com/bythenumbers/cyber-attacks)

Habilite Microsoft Azure AD Identity Protection para informar a los usuarios con credenciales perdidas para que pueda corregirlas. [Azure ad Identity Protection](/azure/active-directory/identity-protection/index) puede aprovecharse para ayudar a su organización a proteger entornos híbridos y en la nube de amenazas.

### <a name="6-azure-atp-lateral-movement-paths"></a>6. Rutas de desplazamiento lateral de ATP de Azure

Asegúrese de que los titulares de la cuenta de acceso con privilegios usan su pata para la administración únicamente para que las cuentas sin privilegios comprometidas no puedan obtener acceso a una cuenta con privilegios a través de ataques de robo de credenciales, como Pass-The-hash o Pass-The-ticket. Las [rutas de desplazamiento lateral de ATP de Azure (LMPs)](/azure-advanced-threat-protection/use-case-lateral-movement-path) proporcionan informes fáciles de entender para identificar dónde pueden estar abiertas las cuentas con privilegios.

## <a name="phase-3-security-improvement-and-sustainment"></a>Fase 3: Mejora de la seguridad y mantenimiento

La fase 3 del mapa de ruta se basa en los pasos realizados en las fases 1 y 2 para reforzar la postura de seguridad. La fase 3 se representa visualmente en este diagrama:

![Fase 3: 1. Revise RBAC, 2. Reduzca las superficies de ataque, 3. Integre los registros con SEIM, 4. Automatización de credenciales perdidas](../media/securing-privileged-access/PAW_LP_Fig8.JPG)

Estas funcionalidades se basarán en los pasos de las fases anteriores y moverán sus defensas en una postura más proactiva. Esta fase no tiene una escala de tiempo específica y puede tardar más tiempo en implementarse en función de la organización individual.

### <a name="1-review-role-based-access-control"></a>1. Revisar el control de acceso basado en roles

Con el modelo de tres niveles descrito en el artículo [Active Directory modelo de nivel administrativo](securing-privileged-access-reference-material.md), revise y asegúrese de que los administradores de nivel inferior no tienen acceso administrativo a recursos de nivel superior (pertenencias a grupos, ACL en cuentas de usuario, etc.).

### <a name="2-reduce-attack-surfaces"></a>2. Reducir las superficies de ataque

Proteja las cargas de trabajo de identidad, incluidos los dominios, los controladores de dominio, ADFS y Azure AD Connect como poner en peligro uno de estos sistemas podría poner en peligro otros sistemas de su organización. Los artículos que [reduciendo la superficie expuesta a ataques de Active Directory](../ad-ds/plan/security-best-practices/reducing-the-active-directory-attack-surface.md) y [cinco pasos para proteger la infraestructura de identidad](/azure/security/azure-ad-secure-steps) proporcionan una guía para proteger los entornos de identidad híbridos y locales.

### <a name="3-integrate-logs-with-siem"></a>3. Integración de registros con SIEM

La integración del inicio de sesión en una herramienta SIEM centralizada puede ayudar a su organización a analizar, detectar y responder a eventos de seguridad. Los artículos que [supervisan Active Directory los síntomas de riesgo](../ad-ds/plan/security-best-practices/monitoring-active-directory-for-signs-of-compromise.md) y [el Apéndice L: Los eventos que](../ad-ds/plan/appendix-l--events-to-monitor.md) se van a supervisar proporcionan instrucciones sobre los eventos que se deben supervisar en el entorno.

Esto forma parte del plan más allá porque la agregación, creación y optimización de alertas en una administración de eventos e información de seguridad (SIEM) requiere analistas especializados (a diferencia de ATP de Azure en el plan de 30 días, que incluye alertas preparadas)

### <a name="4-leaked-credentials---force-password-reset"></a>4. Credenciales perdidas: forzar el restablecimiento de contraseña

Siga mejorando su postura de seguridad habilitando Azure AD Identity Protection para forzar automáticamente el restablecimiento de contraseña cuando se sospeche que las contraseñas están en peligro. Las instrucciones que se encuentran en el artículo [uso de eventos de riesgo para desencadenar multi-factor Authentication y cambios de contraseña](/azure/active-directory/authentication/tutorial-risk-based-sspr-mfa) explican cómo habilitarlo mediante una directiva de acceso condicional.

## <a name="am-i-done"></a>¿Ya he terminado?

La respuesta breve es no.

Los chicos no se detienen nunca, por lo que ninguno puede. Este mapa de ruta puede ayudar a su organización a protegerse frente a amenazas conocidas actualmente, ya que los atacantes evolucionan y cambian constantemente. Le recomendamos que vea la seguridad como un proceso continuo centrado en aumentar el costo y reducir la tasa de éxito de los adversarios destinado a su entorno.

Aunque no es la única parte del programa de seguridad de su organización, la protección del acceso con privilegios es un componente esencial de la estrategia de seguridad.
