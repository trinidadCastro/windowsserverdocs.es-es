---
title: Protección del acceso con privilegios
description: Enfoque por fases para proteger el acceso con privilegios
ms.topic: conceptual
ms.assetid: f5dec0c2-06fe-4c91-9bdc-67cc6a3ede60
ms.date: 02/25/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: mas
ms.openlocfilehash: 23b7322b76eb60c0ae19d3aa0e9e826998a92c77
ms.sourcegitcommit: 8c0a419ae5483159548eb0bc159f4b774d4c3d85
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2020
ms.locfileid: "93235882"
---
# <a name="securing-privileged-access"></a>Protección del acceso con privilegios

>Se aplica a: Windows Server

La protección del acceso con privilegios es un primer paso crítico para establecer controles de seguridad para los recursos empresariales de una organización moderna. La seguridad de la mayoría, si no todos, los recursos empresariales de una organización de TI depende de la integridad de las cuentas con privilegios que realizan tareas de administración, gestión y desarrollo. Normalmente, a esas cuentas y otros elementos de acceso con privilegios se dirigen los ciberatacantes para obtener acceso a datos y sistemas mediante ataques de robo de credenciales como [pass-the-hash y pass-the-ticket](https://www.microsoft.com/pth).

La protección del acceso con privilegios contra determinados adversarios requiere la adopción de un enfoque completo y meditado para aislar estos sistemas frente a los riesgos.

## <a name="what-are-privileged-accounts"></a>¿Qué son las cuentas con privilegios?

Antes de hablar acerca de cómo protegerlas, debemos definir las cuentas con privilegios.

Las cuentas con privilegios, como los administradores de Active Directory Domain Services, tienen acceso directo o indirecto a la mayoría, si no a todos, los recursos de una organización de TI, por lo que estas cuentas representan un riesgo empresarial significativo.

## <a name="why-securing-privileged-access-is-important"></a>¿Por qué es importante proteger el acceso con privilegios?

Los ciberatacantes se centran en el acceso con privilegios a sistemas como Active Directory (AD) a fin de conseguir acceso rápido a todos los datos específicos de las organizaciones. Los enfoques tradicionales sobre seguridad se han centrado en la red y los firewalls como el perímetro de seguridad principal, pero la efectividad de la seguridad de red se ha reducido considerablemente a dos tendencias:

* Las organizaciones que hospedan datos y recursos fuera de los límites de red tradicionales en equipos de empresa móviles, dispositivos como teléfonos móviles y tabletas, servicios en la nube y dispositivos del tipo Bring Your Own Devices (BYOD).
* Los adversarios han demostrado una habilidad constante y sistemática para obtener acceso a estaciones de trabajo dentro de los límites de red por medio de ataques de suplantación de identidad (phishing) y otros ataques web y de correo electrónico.

Estos factores requieren la creación de un perímetro de seguridad moderno distintos de los controles de identidad de autenticación y autorización, además de la estrategia de perímetro de red tradicional. Aquí se define un perímetro de seguridad como un conjunto coherente de controles entre los recursos y las amenazas que implican. Las cuentas con privilegios están al mando de este nuevo perímetro de seguridad de forma efectiva, así que es esencial proteger el acceso con privilegios.

![Diagrama que muestra la capa de identidad de la organización](../media/securing-privileged-access/PAW_LP_Fig2.JPG)

Un atacante que toma el control de una cuenta administrativa puede usar esos privilegios para aumentar su impacto en la organización objetivo, como se representa a continuación:

![Diagrama que muestra cómo un adversario que toma el control de una cuenta administrativa puede usar esos privilegios para perseguir su beneficio a costa de la organización objetivo](../media/securing-privileged-access/PAW_LP_Fig3.JPG)

En la ilustración siguiente se muestran dos rutas de acceso:

* Una ruta de acceso "azul" donde se usa una cuenta de usuario estándar para el acceso sin privilegios a recursos como el correo electrónico, la exploración web y el trabajo cotidiano.

   > [!NOTE]
   > Los elementos de la ruta de acceso azul descritos más adelante indican amplias protecciones del entorno que se extienden más allá de las cuentas administrativas.

* Una ruta de acceso "roja" en la que se produce el acceso con privilegios en un dispositivo protegido para reducir el riesgo de suplantación de identidad (phishing) y otros ataques web y de correo electrónico.

![Diagrama que muestra la "ruta de acceso" separada para administración que establece el mapa de ruta con el fin de aislar las tareas de acceso con privilegios de las tareas de usuario estándar de alto riesgo, como la exploración web y el acceso al correo electrónico](../media/securing-privileged-access/PAW_LP_Fig4.JPG)

## <a name="securing-privileged-access-roadmap"></a>Mapa de ruta para proteger el acceso con privilegios

El mapa de ruta está diseñado para maximizar el uso de tecnologías de Microsoft que ya has implementado, aprovechar tecnologías en la nube que mejoran la seguridad e integrar herramientas de seguridad de terceros que es posible que ya estén implementadas.

El mapa de ruta de recomendaciones de Microsoft se divide en 3 fases:

* [Fase 1: Primeros 30 días](#phase-1-quick-wins-with-minimal-operational-complexity)
   * Resultados rápidos con un impacto positivo significativo.
* [Fase 2: 90 días](#phase-2-significant-incremental-improvements)
   * Mejoras incrementales significativas.
* [Fase 3: En curso](#phase-3-security-improvement-and-sustainment)
   * Mejora de la seguridad y sostenimiento.

El mapa de ruta está clasificado en orden de prioridad para programar las implementaciones más rápidas y efectivas primero en función de nuestras experiencias con estos ataques y la implementación de soluciones.

Microsoft recomienda seguir este plan de ruta para proteger el acceso con privilegios frente a determinados adversarios. Este plan se puede ajustar para adaptarlo a las funcionalidades existentes y los requisitos específicos de su organización.

> [!NOTE]
> Para proteger el acceso con privilegios se requiere una amplia variedad de elementos, como componentes técnicos (defensas de host, protecciones de cuentas, administración de identidades, etc.), así como cambios en los procesos, las prácticas y el conocimiento en materia de administración. Las escalas de tiempo del mapa de ruta son aproximadas y se basan en nuestra experiencia con las implementaciones de clientes. La duración variará en cada organización según la complejidad del entorno y los procesos de administración de cambios.

## <a name="phase-1-quick-wins-with-minimal-operational-complexity"></a>Fase 1: Resultados rápidos con una complejidad operativa mínima

La fase 1 del mapa de ruta se centra en la mitigación rápida de las técnicas de ataque usadas con mayor frecuencia para el robo y el uso indebido de credenciales. Está diseñada para implementarse en 30 días aproximadamente y se representa en este diagrama:

![Diagrama de la fase 1: 1. Cuentas de administrador y usuario separadas, 2. Contraseñas de administrador local Just-in-Time, 3. Fase 1 de estación de trabajo de administración, 4. Detección de ataques de identidad](../media/securing-privileged-access/PAW_LP_Fig6.JPG)

### <a name="1-separate-accounts"></a>1. Cuentas separadas

Para ayudar a separar los riesgos de Internet (ataques de phishing, navegación web) de las cuentas de acceso con privilegios, crea una cuenta dedicada para todo el personal que tiene acceso con privilegios. Los administradores no deben navegar en la web, comprobar su correo electrónico ni realizar tareas de productividad diarias a través de cuentas con privilegios elevados. Puedes encontrar más información al respecto en la sección [Cuentas administrativas separadas](securing-privileged-access-reference-material.md#separate-administrative-accounts) del documento de referencia.

Sigue las instrucciones del artículo [Administración de cuentas de acceso de emergencia en Azure AD](/azure/active-directory/users-groups-roles/directory-emergency-access) para crear un mínimo de dos cuentas de acceso de emergencia (con derechos de administrador asignados de forma permanente) en los entornos locales de AD y Azure AD. Estas cuentas solo se usan cuando las cuentas de administrador tradicionales no pueden realizar una tarea necesaria, como en el caso de un desastre.

### <a name="2-just-in-time-local-admin-passwords"></a>2. Contraseñas de administrador local Just-in-Time

Para mitigar el riesgo de que un adversario robe un hash de contraseña de la cuenta de administrador local desde la base de datos SAM local y lo use para atacar a otros equipos, las organizaciones deben asegurarse de que cada equipo tenga una contraseña de administrador local única. La herramienta de la solución de contraseñas de administrador local (LAPS) puede configurar contraseñas aleatorias únicas en cada estación de trabajo y almacenarlas en un servidor de Active Directory (AD) protegido mediante una ACL. Solo los usuarios autorizados válidos pueden leer o solicitar el restablecimiento de estas contraseñas de cuenta de administrador local. Puedes obtener la solución de contraseñas de administrador local para su uso en estaciones de trabajo y servidores desde el [Centro de descarga de Microsoft](https://aka.ms/LAPS).

Puedes consultar instrucciones adicionales sobre el funcionamiento de un entorno con LAPS y PAW en la sección [Estándares operativos basados en el principio de origen limpio](securing-privileged-access-reference-material.md#operational-standards-based-on-clean-source-principle).

### <a name="3-administrative-workstations"></a>3. Estaciones de trabajo administrativas

Como medida de seguridad inicial para los usuarios con Azure Active Directory y privilegios administrativos tradicionales de Active Directory local, asegúrate de que usen dispositivos Windows 10 configurados según los [Estándares para un dispositivo Windows 10 altamente seguro](/windows-hardware/design/device-experiences/oem-highly-secure). Las cuentas de administrador con privilegios no deben ser miembros del grupo de administradores locales de las estaciones de trabajo administrativas.  La elevación de privilegios a través del control de acceso de usuarios (UAC) se puede usar cuando se requieren cambios de configuración en las estaciones de trabajo.  Además, la base de referencia de seguridad de Windows 10 debe aplicarse a las estaciones de trabajo para proteger aún más el dispositivo.

### <a name="4-identity-attack-detection"></a>4. Detección de ataques de identidad

[Azure Advanced Threat Protection (ATP)](/azure-advanced-threat-protection/what-is-atp) es una solución de seguridad basada en la nube que identifica, detecta y ayuda a investigar amenazas avanzadas, identidades en peligro y acciones de infiltrado malintencionadas dirigidas al entorno local de Active Directory.

## <a name="phase-2-significant-incremental-improvements"></a>Fase 2: Mejoras incrementales significativas

La fase 2 se basa en el trabajo realizado en la fase 1 y se diseñó para completarse en aproximadamente 90 días. Los pasos de esta fase se representan en este diagrama:

![Diagrama de la fase 2: 1. Windows Hello para empresas/MFA, 2. Implementación de PAW, 3. Privilegios Just-in-Time, 4. Credential Guard, 5. Credenciales filtradas, 6. Detección de vulnerabilidades de desplazamiento lateral](../media/securing-privileged-access/PAW_LP_Fig7.JPG)

### <a name="1-require-windows-hello-for-business-and-mfa"></a>1. Solicitud de Windows Hello para empresas y MFA

Los administradores pueden beneficiarse de lo fácil que es usar Windows Hello para empresas. Pueden reemplazar las contraseñas complejas con una autenticación en dos fases sólida en sus equipos. Un atacante debe obtener tanto el dispositivo como los datos biométricos o PIN correspondientes, por lo que es mucho más difícil conseguir acceso sin que lo sepa el empleado. Puedes encontrar más información acerca de Windows Hello para empresas y la ruta de acceso para la implementación en el artículo [Información general de Windows Hello para empresas](/windows/security/identity-protection/hello-for-business/hello-overview).

Habilita la autenticación multifactor (MFA) para las cuentas de administrador en Azure AD con Azure MFA. Como mínimo, habilita la [directiva de acceso condicional para protección de base de referencia](/azure/active-directory/conditional-access/baseline-protection#require-mfa-for-admins). Puedes encontrar más información sobre Azure Multi-Factor Authentication en el artículo [Implementación de una instancia de Azure Multi-Factor Authentication basada en la nube](/azure/active-directory/authentication/howto-mfa-getstarted).

### <a name="2-deploy-paw-to-all-privileged-identity-access-account-holders"></a>2. Implementación de PAW para todos los titulares de cuentas con privilegios de acceso a identidad

Para continuar con el proceso de separar las cuentas con privilegios de las amenazas que se encuentran en el correo electrónico, la navegación web y otras tareas no administrativas, debes implementar estaciones de trabajo de acceso con privilegios (PAW) dedicadas para todo el personal que tiene acceso con privilegios a los sistemas de información de la organización. Puedes encontrar más información sobre la implementación de PAW en el artículo [Estaciones de trabajo de acceso con privilegios](privileged-access-workstations.md#paw-phased-implementation).

### <a name="3-just-in-time-privileges"></a>3. Privilegios Just-In-Time

Para reducir el tiempo de exposición de los privilegios y aumentar la visibilidad de su uso, proporciona privilegios Just-In-Time (JIT) con una solución adecuada, como las que se indican a continuación u otras soluciones de terceros:

* Para Servicios de dominio de Active Directory (AD DS), use la funcionalidad [Privileged Access Manager (PAM)](/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services) de Microsoft Identity Manager (MIM).
* Para Azure Active Directory, use la funcionalidad [Azure AD Privileged Identity Management (PIM)](/azure/active-directory/privileged-identity-management/pim-deployment-plan).

### <a name="4-enable-windows-defender-credential-guard"></a>4. Habilitación de Credential Guard de Windows Defender

La activación de Credential Guard ayuda a proteger los hash de contraseña NTLM, los vales de concesión de vales de Kerberos y las credenciales que almacenan las aplicaciones como credenciales de dominio. Esta funcionalidad ayuda a evitar los ataques de robo de credenciales, como Pass-The-Hash o Pass-The-Ticket, al aumentar la dificultad de dinamización en el entorno mediante credenciales robadas. Puedes encontrar información sobre cómo funciona Credential Guard y cómo realizar la implementación en el artículo [Protección de las credenciales de dominio derivadas con Credential Guard de Windows Defender](/windows/security/identity-protection/credential-guard/credential-guard).

### <a name="5-leaked-credentials-reporting"></a>5. Informes de credenciales filtradas

"Todos los días, Microsoft analiza más de 6,5 billones de señales con el fin de identificar las amenazas emergentes y proteger a los clientes": [Microsoft en cifras](https://news.microsoft.com/bythenumbers/cyber-attacks).

Habilita Microsoft Azure AD Identity Protection para recibir informes sobre los usuarios con credenciales filtradas para que puedas corregirlo. Puedes aprovechar [Azure AD Identity Protection](/azure/active-directory/identity-protection/index) para ayudar a que la organización proteja de amenazas a los entornos híbridos y en la nube.

### <a name="6-azure-atp-lateral-movement-paths"></a>6. Rutas de desplazamiento lateral de Azure ATP

Asegúrate de que los titulares de las cuentas de acceso con privilegios usan su PAW para administración únicamente para que las cuentas sin privilegios en riesgo no puedan obtener acceso a una cuenta con privilegios a través de ataques de robo de credenciales, como Pass-The-Hash o Pass-The-Ticket. Las [rutas de desplazamiento lateral (LMP) de Azure ATP](/azure-advanced-threat-protection/use-case-lateral-movement-path) proporcionan informes fáciles de entender para identificar dónde pueden estar expuestas a riesgos las cuentas con privilegios.

## <a name="phase-3-security-improvement-and-sustainment"></a>Fase 3: Mejora de la seguridad y sostenimiento

La fase 3 del mapa de ruta se basa en los pasos realizados en las fases 1 y 2 para reforzar la postura de seguridad. Este diagrama representa visualmente la fase 3:

![Fase 3: 1. Revisión de RBAC, 2. Reducción de superficies de ataque, 3. Integración de registros con SEIM, 4. Automatización de credenciales filtradas](../media/securing-privileged-access/PAW_LP_Fig8.JPG)

Estas funcionalidades se basarán en las fases anteriores y trasladarán las defensas a una postura más proactiva. Esta fase no tiene una línea de tiempo específica y puede tardar más en implementarse en función de tu organización específica.

### <a name="1-review-role-based-access-control"></a>1. Revisión del control de acceso basado en roles

Con el modelo de tres niveles descrito en el artículo [Modelo de nivel administrativo de Active Directory](securing-privileged-access-reference-material.md), revisa y asegúrate de que los administradores de nivel inferior no tienen acceso administrativo a recursos de nivel superior (pertenencias a grupos, ACL en las cuentas de usuario, etc.).

### <a name="2-reduce-attack-surfaces"></a>2. Reducción de superficies de ataque

Protege las cargas de trabajo de identidad, incluidos los dominios, los controladores de dominio, ADFS y Azure AD Connect, ya que poner en peligro uno de estos sistemas podría afectar otros sistemas de la organización. Los artículos [Reducción de la superficie de ataque de Active Directory](../ad-ds/plan/security-best-practices/reducing-the-active-directory-attack-surface.md) y [Cinco pasos para asegurar la infraestructura de identidad](/azure/security/azure-ad-secure-steps) proporcionan instrucciones para proteger los entornos de identidad locales e híbridos.

### <a name="3-integrate-logs-with-siem"></a>3. Integración de registros con SIEM

La integración del inicio de sesión en una herramienta SIEM centralizada puede ayudar a la organización a analizar, detectar y responder a eventos de seguridad. Los artículos [Supervisión de Active Directory en busca de indicios de riesgo](../ad-ds/plan/security-best-practices/monitoring-active-directory-for-signs-of-compromise.md) y [Apéndice L: Eventos para supervisar](../ad-ds/plan/appendix-l--events-to-monitor.md) proporcionan instrucciones sobre los eventos que se deben supervisar en el entorno.

Esto forma parte del plan futuro, ya que la agregación, creación y optimización de alertas en una administración de eventos e información de seguridad (SIEM) requiere de analistas especializados (a diferencia de Azure ATP en el plan de 30 días, que incluye alertas listas para usarse).

### <a name="4-leaked-credentials---force-password-reset"></a>4. Credenciales filtradas: forzar el restablecimiento de contraseña

Mejora aún más tu postura de seguridad al habilitar Azure AD Identity Protection para forzar automáticamente el restablecimiento de contraseñas cuando se sospeche que las contraseñas están en peligro. Las instrucciones que se encuentran en el artículo [Uso de eventos de riesgo para desencadenar Multi-Factor Authentication y cambios de contraseñas](/azure/active-directory/authentication/tutorial-risk-based-sspr-mfa) explican cómo habilitar la opción mediante una directiva de acceso condicional.

## <a name="am-i-done"></a>¿Ya he terminado?

La respuesta breve es no.

Los delincuentes nunca descansan, así que tú tampoco deberías. Este mapa de ruta puede ayudar a que tu organización se proteja de amenazas conocidas actualmente, ya que los atacantes evolucionan y cambian constantemente. Se recomienda que consideres la seguridad como un proceso continuado centrado en aumentar el costo y reducir el índice de éxito de los ataques de los adversarios hacia tu entorno.

Aunque no es la única parte del programa de seguridad de la organización, la protección del acceso con privilegios es un componente esencial de la estrategia de seguridad.
