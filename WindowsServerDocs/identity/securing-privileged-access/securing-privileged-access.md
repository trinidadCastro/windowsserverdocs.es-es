---
title: Protección del acceso con privilegios
description: Enfoque por fases para proteger el acceso con privilegios
ms.prod: windows-server-threshold
ms.topic: conceptual
ms.assetid: f5dec0c2-06fe-4c91-9bdc-67cc6a3ede60
ms.date: 02/25/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: mas
ms.openlocfilehash: 0d54a94d51a4d1e0a1d28f78ec39bf16bc3d9100
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822016"
---
# <a name="securing-privileged-access"></a>Protección del acceso con privilegios

>Se aplica a: Windows Server

La protección del acceso con privilegios es un primer paso crítico para establecer controles de seguridad para los recursos empresariales de una organización moderna. La seguridad de la mayoría o todos los recursos empresariales en una organización de TI depende de la integridad de las cuentas con privilegios que se utiliza para administrar, administrar y desarrollar. Los Ciberatacantes de destino a menudo estas cuentas y otros elementos de acceso con privilegios para tener acceso a datos y sistemas mediante ataques de robo de credenciales como [Pass-the-Hash y Pass-the-Ticket](https://www.microsoft.com/pth).

La protección del acceso con privilegios frente a determinados adversarios requiere adoptar un enfoque completo y meditado para aislar estos sistemas frente a riesgos.

## <a name="what-are-privileged-accounts"></a>¿Cuáles son las cuentas con privilegios?

Antes de hablar acerca de cómo protegerlos permite definir las cuentas con privilegios.

Las cuentas con privilegios, como los administradores de servicios de dominio de Active Directory tienen acceso directo o indirecto a la mayoría o todos los recursos en una organización de TI, lo un riesgo de estas cuentas un riesgo importante actividad empresarial.

## <a name="why-securing-privileged-access-is-important"></a>¿Por qué es importante proteger el acceso con privilegios?

Foco de los Ciberatacantes acceso con privilegios a sistemas como Active Directory (AD) para obtener acceso a todos los de las organizaciones rápidamente datos de destino. Enfoques de seguridad tradicionales se han centrado en la red y los servidores de seguridad como el perímetro de seguridad principal, pero la eficacia de la seguridad de red se ha reducido considerablemente a dos tendencias:

* Las organizaciones que hospedan datos y los recursos fuera del límite de red tradicionales en enterprise mobile PC, dispositivos como teléfonos móviles y tabletas, servicios en la nube y traen sus propios dispositivos (BYOD)
* Los adversarios han demostrado una habilidad constante y sistemática para obtener acceso a estaciones de trabajo dentro de los límites de red por medio de ataques de suplantación de identidad (phishing) y otros ataques web y de correo electrónico.

Estos factores exigen la generación de un perímetro de seguridad modernas fuera de la autenticación y autorización de controles de identidad además de la estrategia de perímetro de red tradicional. Aquí un perímetro de seguridad se define como un conjunto coherente de los controles entre los activos y las amenazas a ellos. Las cuentas con privilegios son eficazmente en el control de este nuevo perímetro de seguridad, por lo que es fundamental para proteger el acceso con privilegios.

![Diagrama que muestra la capa de identidad de la organización](../media/securing-privileged-access/PAW_LP_Fig2.JPG)

Un atacante que toma el control de una cuenta administrativa puede usar esos privilegios para aumentar su impacto en la organización de destino, como se muestra a continuación:

![Diagrama que muestra cómo un adversario que toma el control de una cuenta administrativa puede usar esos privilegios para perseguir su beneficio a costa de la organización objetivo](../media/securing-privileged-access/PAW_LP_Fig3.JPG)

La siguiente ilustración muestra dos rutas de acceso:

* Se completa una ruta de acceso "azul", donde se usa una cuenta de usuario estándar para el acceso sin privilegios a los recursos como correo electrónico y exploración web y el trabajo diario.

   > [!NOTE]
   > Los elementos de ruta azul se describe más adelante indican amplio protecciones del entorno que van más allá de las cuentas administrativas.

* Una ruta de acceso "rojo" donde se produce el acceso con privilegios en un dispositivo protegido para reducir el riesgo de "phishing" y otros ataques web y correo electrónico.

![Diagrama que muestra la ruta"independiente" para la administración que establece el mapa de ruta para aislar las tareas de acceso con privilegios de las tareas de usuario estándar de alto riesgo, como exploración web y acceso al correo electrónico](../media/securing-privileged-access/PAW_LP_Fig4.JPG)

## <a name="securing-privileged-access-roadmap"></a>Protección de mapa de ruta de acceso con privilegios

El mapa de ruta está diseñado para maximizar el uso de tecnologías de Microsoft que ya han implementado, aprovechar las ventajas de las tecnologías de nube para mejorar la seguridad e integrar cualquier 3rd herramientas de seguridad de terceros que ya han implementado.

El mapa de ruta de recomendaciones de Microsoft se divide en 3 fases:

* [Fase 1: Primeros 30 días]()
   * Wins rápido con un impacto positivo significativo.
* [Fase 2: 90 días]()
   * Mejoras incrementales importantes.
* [Fase 3: En curso]()
   * Mejora de seguridad y sustainment.

El mapa de ruta está clasificado en orden de prioridad para programar las implementaciones más rápidas y efectivas primero en función de nuestras experiencias con estos ataques y la implementación de soluciones. 

Microsoft recomienda seguir este plan de ruta para proteger el acceso con privilegios frente a determinados adversarios. Este plan se puede ajustar para adaptarlo a las funcionalidades existentes y los requisitos específicos de su organización.

> [!NOTE]
> Para proteger el acceso con privilegios se requiere una amplia variedad de elementos, como componentes técnicos (defensas de host, protecciones de cuentas, administración de identidades, etc.), así como cambios en los procesos, las prácticas y el conocimiento en materia de administración. Las escalas de tiempo del mapa de ruta son aproximadas y se basan en nuestra experiencia con las implementaciones de clientes. La duración variará en cada organización según la complejidad del entorno y los procesos de administración de cambios.

## <a name="phase-1-quick-wins-with-minimal-operational-complexity"></a>Fase 1: Wins rápidos con complejidad operativa mínima

Fase 1 del mapa de ruta se centra en mitigar rápidamente las técnicas de ataque usadas con más frecuencia de robo de credenciales y el uso indebido. Fase 1 está diseñada para implementarse en aproximadamente 30 días y se describe en este diagrama:

![Diagrama de la fase 1: 1. Administración y el usuario cuenta independiente, 2. Just-in-contraseñas de administrador local del tiempo, 3. Fase de estación de trabajo administración 1, 4. Detección de ataques de identidad](../media/securing-privileged-access/PAW_LP_Fig6.JPG)

### <a name="1-separate-accounts"></a>1. Cuentas independientes

Para ayudar a separar los riesgos de internet (ataques de phishing, exploración web) de privilegios acceder a las cuentas, cree una cuenta dedicada para todo el personal con acceso con privilegios. Los administradores deben no se explora la web, comprobando su correo electrónico y realizar tareas de productividad diarias con cuentas con privilegios elevados. Encontrará más información sobre esto en la sección [cuentas administrativas separadas](securing-privileged-access-reference-material.md#separate-administrative-accounts) del documento de referencia.

Siga las instrucciones del artículo [administrar cuentas de acceso de emergencia en Azure AD](/azure/active-directory/users-groups-roles/directory-emergency-access) para crear el acceso de emergencia al menos dos cuentas, con derechos de administrador asignados de forma permanente, en sus locales AD y Azure AD entornos . Estas cuentas son solo para su uso cuando las cuentas de administrador tradicionales no pueden realizar las tareas necesarias como en un caso de un desastre.

### <a name="2-just-in-time-local-admin-passwords"></a>2. Just-in-contraseñas de administrador local de tiempo

Para mitigar el riesgo de que un adversario robar un hash de contraseña de cuenta de administrador local de la base de datos SAM local y lo use para atacar a otros equipos, las organizaciones deben asegurarse de que cada máquina tiene una contraseña de administrador local única. La herramienta de solución de contraseña de administrador Local (LAP) puede configurar contraseñas aleatorias únicas en cada estación de trabajo y servidor almacenarlas en Active Directory (AD) protegidos por una ACL. Solo los usuarios autorizados aptos pueden leer o solicitar el restablecimiento de estas contraseñas de cuenta de administrador local. Puede obtener el LAPS para su uso en las estaciones de trabajo y servidores de [Microsoft Download Center](http://Aka.ms/LAPS).

Instrucciones adicionales para trabajar en un entorno con LAPS y Paw pueden encontrarse en la sección [estándares operativos según el principio de origen limpio](securing-privileged-access-reference-material.md#operational-standards-based-on-clean-source-principle).

### <a name="3-administrative-workstations"></a>3. Estaciones de trabajo administrativas

Como medida de seguridad inicial para los usuarios con Azure Active Directory y privilegios de administrador de Active Directory locales tradicionales, asegúrese de que usan los dispositivos de Windows 10 configurados con la [estándares para una alta seguridad Windows dispositivo 10](/windows-hardware/design/device-experiences/oem-highly-secure). 

### <a name="4-identity-attack-detection"></a>4. Detección de ataques de identidad

[Azure protección de amenazas avanzada (ATP)](/azure-advanced-threat-protection/what-is-atp) es una solución de seguridad basada en la nube que identifica, detecta y le ayuda a investigar amenazas avanzadas, las identidades en peligro y acciones infiltrado malintencionado dirigidas a su activo en el entorno local Entorno de directorio.

## <a name="phase-2-significant-incremental-improvements"></a>Fase 2: Importantes mejoras incrementales

Fase 2 se basa en el trabajo realizado en la fase 1 y está diseñada para completarse en 90 días aproximadamente. Los pasos de esta fase se representan en este diagrama:

![Diagrama de la fase 2: 1. Windows Hello para empresas / MFA, 2. Implementación PAW, 3. Justo a tiempo privilegios, 4. Credential Guard, 5. Credenciales filtradas, 6. Detección de vulnerabilidades de desplazamiento lateral](../media/securing-privileged-access/PAW_LP_Fig7.JPG)

### <a name="1-require-windows-hello-for-business-and-mfa"></a>1. Requiere Windows Hello para empresas y MFA

Los administradores pueden beneficiarse de la facilidad de uso asociado con Windows Hello para empresas. Los administradores pueden reemplazar sus contraseñas complejas con autenticación de dos factores segura en sus equipos. Un atacante debe tener el dispositivo y la información biométrica o PIN, es mucho más difícil obtener acceso sin conocimiento del empleado. Se puede encontrar más información acerca de Windows Hello para empresas y la ruta de acceso para la implementación en el artículo [Windows Hello para información general del negocio](/windows/security/identity-protection/hello-for-business/hello-overview)

Habilitar autenticación multifactor (MFA) para las cuentas de administrador en Azure AD con Azure MFA. Al habilitar mínimo el [directiva de acceso condicional de protección de línea base](/azure/active-directory/conditional-access/baseline-protection#require-mfa-for-admins) se puede encontrar más información sobre Azure Multi-factor Authentication en el artículo [implementar en la nube Azure Multi-factor Authentication](/azure/active-directory/authentication/howto-mfa-getstarted)

### <a name="2-deploy-paw-to-all-privileged-identity-access-account-holders"></a>2. Implementar PAW a todos los titulares de cuentas de acceso de identidades con privilegios

Continuando el proceso de separar las cuentas con privilegios frente a amenazas de correo electrónico, exploración web y otras tareas no administrativas, debe implementar estaciones de trabajo dedicado de acceso con privilegios (PAW) para todo el personal con acceso con privilegios a su sistemas de información de la organización. Encontrará instrucciones adicionales para la implementación de PAW en el artículo [estaciones de trabajo de acceso con privilegios](privileged-access-workstations.md#paw-phased-implementation).

### <a name="3-just-in-time-privileges"></a>3. Just-in-privilegios de tiempo

Para reducir el tiempo de exposición de los privilegios y aumentar la visibilidad sobre su uso, proporcione privilegios just-in-time (JIT) con otras soluciones de terceros o de una solución adecuada, como los siguientes:

* Para Servicios de dominio de Active Directory (AD DS), use la funcionalidad [Privileged Access Manager (PAM)](/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services) de Microsoft Identity Manager (MIM).
* Para Azure Active Directory, use la funcionalidad [Azure AD Privileged Identity Management (PIM)](/azure/active-directory/privileged-identity-management/pim-deployment-plan).

### <a name="4-enable-windows-defender-credential-guard"></a>4. Habilitar Credential Guard de Windows Defender

Habilitar Credential Guard ayuda a proteger los hash de contraseña NTLM, concesión de vales de Kerberos y credenciales almacenadas por aplicaciones como las credenciales de dominio. Esta funcionalidad ayuda a evitar ataques de robo de credenciales, como Pass-the-Hash o Pass-The-Ticket aumentando la dificultad de dinamizar en el entorno con credenciales robadas. Se puede encontrar información sobre cómo funciona Credential Guard y cómo se implementa en el artículo [derivados de proteger las credenciales de dominio con Credential Guard de Windows Defender](/windows/security/identity-protection/credential-guard/credential-guard).

### <a name="5-leaked-credentials-reporting"></a>5. Informes de credenciales perdidas

"Cada día, Microsoft analiza las señales en billones de 6.5 con el fin de identificar las amenazas emergentes y proteger a los clientes" - [Microsoft por números](https://news.microsoft.com/bythenumbers/cyber-attacks)

Habilitar la protección de identidad de Microsoft Azure AD informar sobre los usuarios con credenciales perdidas, por lo que puede corregirlas. [Azure AD Identity Protection](/azure/active-directory/identity-protection/index) puede aprovecharse para ayudar a su organización proteger entornos de nube y híbridos frente a amenazas.

### <a name="6-azure-atp-lateral-movement-paths"></a>6. Rutas de desplazamiento Lateral de ATP de Azure

Asegúrese de privilegios acceder a la cuenta marcadores están usando su PAW para la administración solo para que una cuenta sin privilegios en peligro no puede obtener acceso a una cuenta con privilegios mediante ataques de robo de credenciales, como Pass-the-Hash o Pass-The-Ticket. [Rutas de desplazamiento Lateral con ATP de Azure (LMPs)](/azure-advanced-threat-protection/use-case-lateral-movement-path) proporciona fácil comprensión de informes para identificar las cuentas con privilegios donde esté abiertas para poner en peligro.

## <a name="phase-3-security-improvement-and-sustainment"></a>Fase 3: Sustainment y mejora la seguridad

Fase 3 de la hoja de ruta se basa en los pasos realizados en las fases 1 y 2 para Refuerce su seguridad. Este diagrama representa visualmente la fase 3:

![Fase 3: 1. Revisión de RBAC, 2. Reducir las superficies de ataque, 3. Integración de registros con SEIM, 4. Automatización de credenciales perdidas](../media/securing-privileged-access/PAW_LP_Fig8.JPG)

Estas capacidades se compile en los pasos de fases anteriores y trasladarán sus defensas a una postura más proactiva. Esta fase no tiene ninguna escala de tiempo específico y puede tardar más tiempo en implementar en función de su organización individual.

### <a name="1-review-role-based-access-control"></a>1. Revise el control de acceso basado en roles

Mediante el modelo en niveles tres descritas en el artículo [modelo de nivel administrativo de Active Directory](securing-privileged-access-reference-material.md), revise y asegúrese de que los administradores de nivel inferior no tienen acceso administrativo a los recursos de nivel superior (pertenencia al grupo, las ACL en cuentas de usuario, etcetera...).

### <a name="2-reduce-attack-surfaces"></a>2. Reducir las superficies de ataque

Proteger las cargas de trabajo de identidad incluidos los dominios, los controladores de dominio, AD FS y Azure AD Connect en poner en peligro alguno de estos sistemas podrían poner en peligro otros sistemas de su organización. Los artículos [lo que reduce la superficie de ataque de Active Directory](../ad-ds/plan/security-best-practices/reducing-the-active-directory-attack-surface.md) y [cinco pasos para proteger la infraestructura de identidad](/azure/security/azure-ad-secure-steps) proporcionan instrucciones para proteger híbrida local y entornos de identidad.

### <a name="3-integrate-logs-with-siem"></a>3. Integración de registros con SIEM

Integración de registro en una herramienta SIEM centralizada puede ayudar a su organización para analizar, detectar y responder a eventos de seguridad. Los artículos [supervisión de Active Directory para signos de peligro](../ad-ds/plan/security-best-practices/monitoring-active-directory-for-signs-of-compromise.md) y [Apéndice L: Eventos para supervisar](../ad-ds/plan/appendix-l--events-to-monitor.md) ofrecen orientación sobre los eventos que deben supervisarse en su entorno.

Esto forma parte de la más allá de lo previsto porque la agregación, crear y ajustar alertas en una seguridad información y administración de eventos (SIEM) requiere cualificados analistas (a diferencia de ATP de Azure en el plan de 30 días que incluye fuera del cuadro de alerta)

### <a name="4-leaked-credentials---force-password-reset"></a>4. Credenciales filtradas: restablecimiento de contraseña Force

Seguir mejorando la postura de seguridad al habilitar Azure AD Identity Protection automáticamente forzar los restablecimientos de contraseña cuando las contraseñas se sospecha de peligro. Las instrucciones que encontrará en el artículo [usar eventos de riesgo para la autenticación multifactor de desencadenador y los cambios de contraseña](/azure/active-directory/authentication/tutorial-risk-based-sspr-mfa) se explica cómo habilitar esta opción mediante una directiva de acceso condicional.

## <a name="am-i-done"></a>¿Ya he terminado?

La respuesta corta es no.

Los tipos malos nunca detenga, por lo que no puede. Este plan puede ayudar a su organización a protegerse frente a conocido amenazas como constantemente los atacantes evolucionará y MAYÚS. Le recomendamos que visualice seguridad como un proceso continuado centrado en aumentar el costo y reducir la tasa de éxito de los adversarios hacia su entorno.

Aunque no es la única parte del programa de seguridad de su organización la protección del acceso con privilegios es un componente esencial de su estrategia de seguridad.
