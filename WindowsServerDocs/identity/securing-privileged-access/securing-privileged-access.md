---
title: Proteger el acceso con privilegios
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f5dec0c2-06fe-4c91-9bdc-67cc6a3ede60
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: eb83903204b00ef6c1eb116554ec54bc2211a399
ms.sourcegitcommit: 7b01b54032ec56432116626e08fbd92508c3a7d5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2018
---
# <a name="securing-privileged-access"></a>Proteger el acceso con privilegios

>Se aplica a: Windows Server 2016

Protección con privilegios acceso es un primer paso para establecer las garantías de seguridad para los activos de negocio en una organización moderna fundamental. La seguridad de la mayoría o todos los activos de negocio en una organización depende de la integridad de las cuentas con privilegios que administrar y administrar los sistemas de TI. Los atacantes cibernéticos están dirigido a estas cuentas y otros elementos de acceso con privilegios para acceder rápidamente a los datos de destino y sistemas mediante ataques de robo de credenciales como [Pass-the-Hash y Pass-the-Ticket](https://www.microsoft.com/pth).

Proteger el acceso administrativo contra determinado adversarios requieren que tome un enfoque esmerado y completo para aislar estos sistemas de riesgos. Esta figura muestra las tres fases de recomendaciones para separar y proteger la administración en esta guía básica:

![Diagrama de las tres fases de recomendaciones para separar y proteger la administración en esta guía básica](../media/securing-privileged-access/PAW_LP_Fig1.JPG)

Guía básica objetivos:

-   **Planear la semana 2 y 4**: rápidamente mitigar las técnicas de ataque usados con más frecuencia

-   **plan de 1 a 3 meses**: compilar visibilidad y control de actividad de administrador

-   **plan mensual de 6 +**: seguir compilando defensas a un nivel de seguridad más proactivo

Microsoft recomienda que sigue esta guía básica para proteger el acceso con privilegios contra determinados adversarios. Se puede ajustar esta guía básica para dar cabida a las capacidades de existentes y los requisitos específicos en sus organizaciones.

> [!NOTE]
> Proteger con privilegios acceso requiere una amplia variedad de elementos, incluidos componentes técnicos (las defensas de host, protecciones de cuenta, administración de identidades, etc.), así como cambios en procesos y las prácticas administrativas y conocimientos.

## <a name="why-is-securing-privileged-access-important"></a>¿Por qué es importante proteger el acceso a privilegios?
En la mayoría de las organizaciones, la seguridad de la mayoría o todos los activos de negocio depende de la integridad de las cuentas con privilegios que administrar y administrar los sistemas de TI. Los atacantes cibernéticos se centran en el acceso a sistemas con privilegios como datos de destino de Active Directory para tener rápidamente acceso a todas las organizaciones.

Enfoques de seguridad tradicionales han centrado sobre el uso de los puntos de entrada y salida de una red de las organizaciones como el perímetro de la seguridad principal, pero la eficacia de seguridad de red se ha reducido significativamente las tendencias dos:

-   Las organizaciones tienen datos y recursos fuera de los límites de red tradicional en móviles de empresa de equipos, dispositivos como tabletas y teléfonos móviles, así como los servicios de nube y dispositivos BYOD

-   Adversarios han demostrado que una capacidad en curso y coherente para obtener acceso en estaciones de trabajo dentro de los límites de red a través de suplantación de identidad y otros ataques web y correo electrónico.

El reemplazo natural para el perímetro de la seguridad de red en una empresa moderno compleja es los controles de autenticación y autorización de capa de identidad de la organización. Las cuentas con privilegios administrativas son efectivamente en un control de esta nueva "perímetro de seguridad" por lo que es esencial proteger con privilegios de acceso:

![Diagrama que muestra la capa de identidad de la organización](../media/securing-privileged-access/PAW_LP_Fig2.JPG)

Un adversario que toma el control de una cuenta de administrador puede usar esos privilegios para ejercer su ganancia a costa de la organización de destino, como se muestra a continuación:

![Diagrama que muestra cómo un adversario que toma el control de una cuenta de administrador puede usar esos privilegios para ejercer su ganancia a costa de la organización de destino](../media/securing-privileged-access/PAW_LP_Fig3.JPG)

Para obtener más información sobre los tipos de ataques que normalmente llevan a los atacantes en control de cuentas administrativas, visita el [sitio web de pasar el Hash](https://www.microsoft.com/pth) informativos notas del producto, vídeos y mucho más.

Esta figura muestra "canal" independiente para la administración que establece la guía básica para aislar tareas con privilegios de acceso de tareas de usuario estándar de riesgo alto como web exploración y el acceso a correo electrónico.

![Diagrama que muestra "canal" independiente para la administración que establece la guía básica para aislar tareas con privilegios de acceso de tareas de usuario estándar de riesgo alto como web exploración y el acceso a correo electrónico](../media/securing-privileged-access/PAW_LP_Fig4.JPG)

Porque el adversario puede tener un control de acceso con privilegios mediante una variedad de métodos, mitigar este riesgo requiere un enfoque holístico y detallado técnico, como se describe en esta guía básica. La guía básica se aislar y proteger los elementos en el entorno que permiten con privilegios de acceso mediante la creación de mitigaciones en cada área de la columna de defensa en esta ilustración:

![Tabla donde se muestran las columnas de ataque y defensa](../media/securing-privileged-access/PAW_LP_Fig5.JPG)

## <a name="security-privileged-access-roadmap"></a>Guía básica de acceso con privilegios de seguridad
La guía básica está diseñada para maximizar el uso de tecnologías que puede que ya esté implementado, sacar provecho de las tecnologías de clave de seguridad actuales y futuras y se integran las herramientas de seguridad de terceros 3 que ya han implementado.

La guía básica de las recomendaciones de Microsoft se divide en 3 fases:

-   Planear la semana 2-4 - rápidamente mitigar las técnicas de ataque usados con más frecuencia

-   plan de 1 a 3 meses: construir la visibilidad y control de actividad de administrador

-   6 + mes planear - seguir compilando defensas a un nivel de seguridad más proactivo

Cada fase de la guía básica está diseñado para generar el costo y la dificultad para adversarios a los ataques de acceso con privilegios para su local y activos en la nube. Se da prioridad a la guía básica para programar la más eficaz y las implementaciones más rápidas en primer lugar en función de la experiencia con estos ataques y la implementación de la solución.

> [!NOTE]
> Las escalas de tiempo para la guía básica son aproximados y se basan en nuestra experiencia con las implementaciones del cliente. La duración variará en la organización según la complejidad de su entorno y los procesos de administración de cambio.

### <a name="security-privileged-access-roadmap-stage-1"></a>Seguridad privilegiadas guía básica de acceso: Etapa 1
Etapa 1 de la guía básica se centra en rápidamente mitigar las técnicas de ataque usados con más frecuencia de robo de credenciales y el abuso. Etapa 1 está diseñada para implementarse en aproximadamente 2 a 4 semanas y se muestra en este diagrama:

![Ilustración que muestra esa fase 1 está diseñada para implementarse en aproximadamente 2 a 4 semanas](../media/securing-privileged-access/PAW_LP_Fig6.JPG)

Etapa 1 de la guía básica de acceso con privilegios de seguridad incluye los siguientes componentes:

**1. separar la cuenta de administrador de tareas de administración**

Para ayudar a separar los riesgos de internet (ataques de suplantación de identidad, exploración web) privilegios administrativos, crear una cuenta dedicada para todas las personas con privilegios administrativos. Directrices adicionales sobre este se incluyen en las instrucciones de PATA publicadas [aquí](http://Aka.ms/CyberPAW).

**2. privilegiadas acceso estaciones de trabajo (garras) en la fase 1: Los administradores de Active Directory**

Para ayudar a separar los riesgos de internet (ataques de suplantación de identidad, exploración web) de privilegios de administrador de dominio, crear estaciones de trabajo de acceso con privilegios dedicado (garras) para el personal con privilegios administrativos de AD. Este es el primer paso de un programa PATA y es la fase 1 de la guía publicada [aquí](http://Aka.ms/CyberPAW).

**3. las contraseñas de administrador Local único para estaciones de trabajo**

**4. contraseñas de administrador Local único para servidores**

Para mitigar el riesgo de que un adversario roban un hash de contraseña de cuenta de administrador local desde la base de datos de SAM local y se abusa para atacar otros equipos, debes usar la herramienta PARCIALES configurar contraseñas aleatorias únicas en cada estación de trabajo y el servidor y registrar las contraseñas en Active Directory. Puedes obtener la solución de contraseña de administrador Local para su uso en los servidores y estaciones de trabajo [aquí](http://Aka.ms/LAPS).

Pueden encontrarse directrices adicionales para trabajar en un entorno con tiempos PARCIALES y tiempos garras [aquí](http://aka.ms/securitystandards).

### <a name="security-privileged-access-roadmap-stage-2"></a>Seguridad privilegiadas guía básica de acceso: Fase 2
Fase 2 se basa en las mitigaciones de fase 1 y está diseñada para implementarse en meses, aproximadamente 1-3. En este diagrama se representan los pasos de esta fase:

![Diagrama que muestra los pasos de la fase 2](../media/securing-privileged-access/PAW_LP_Fig7.JPG)

**1. PATA fases 2 y 3: todos los administradores y seguridad adicionales**

Para separar los riesgos de internet de las cuentas administrativas con privilegios, continuar con la pata iniciar en la fase 1 e implementar las estaciones de trabajo dedicados para todas las personas con acceso con privilegios. Este se asigna a la fase 2 y 3 de la guía publicado [aquí](http://Aka.ms/CyberPAW).

**2. controlado por tiempo privilegios (no administradores permanente)**

Para reducir el tiempo de exposición de privilegios y aumentar la visibilidad de su uso, proporcionan privilegios solo en time (JIT) con una solución apropiada como los siguientes:

-   Para los servicios de dominio de Active Directory (AD DS), usa Microsoft Identity Manager (MIM) del [Manager con privilegios de acceso (PAM)](https://technet.microsoft.com/en-us/library/mt150258.aspx) funcionalidad.

-   Para Azure Active Directory, usa [administración con privilegios de identidad (PIM) de Azure AD](http://aka.ms/AzurePIM) funcionalidad.

**3. multifactor de elevación controlado por tiempo**

Para aumentar el nivel de seguridad de autenticación de administrador, deberías requerir la autenticación multifactor antes de conceder privilegios.
Esto se puede lograr con MIM PAM y Azure AD PIM con Azure la autenticación multifactor (AMF).

**4. suficientes administrador (JEA) para el mantenimiento de DC**

Para reducir la cantidad de cuentas con privilegios de administración del dominio y la exposición de riesgo asociado, usa la característica solo lo suficientemente administración (JEA) en PowerShell para realizar operaciones comunes de mantenimiento en controladores de dominio. La tecnología JEA permite a usuarios específicos realizar determinadas tareas administrativas en los servidores (como controladores de dominio) sin tener que darle derechos de administrador. Descarga esta guía de [TechNet](http://aka.ms/JEA).

**5. menor superficie expuesta a ataques de dominio y controladores de dominio**

Para reducir las oportunidades de adversarios a tomar el control de un bosque, debe reducir las rutas de que un atacante puede seguir para obtener el control de los controladores de dominio o los objetos de control del dominio. Sigue las instrucciones para reducir este riesgo publicado [aquí](http://aka.ms/HardenAD).

**6. detección de ataques de**

Para obtener la visibilidad en el robo de credenciales activo e identidad de ataques para que pueda responder rápidamente a los eventos y contener daños, implementar y configurar [análisis de amenazas avanzada de Microsoft (ATA)](https://www.microsoft.com/ata).

Antes de instalar ATA, debes asegurarte de que tienes un proceso en el lugar para controlar un incidente de seguridad más importantes que puede detectar ATA.

-   Para obtener más información sobre cómo configurar un proceso de respuesta a incidentes, consulta [responder a incidentes de seguridad de TI](https://aka.ms/irr) y la "responder a la actividad sospechosa" y "Recuperar de una infracción" secciones de [Mitigating Pass-the-Hash y otros robo de credenciales](https://www.microsoft.com/pth), versión 2.

-   Para obtener más información sobre un atractivo ayudan a preparar el proceso de infrarrojos ATA genera eventos y ATA de implementación de los servicios de Microsoft, póngase en contacto con tu representante de Microsoft mediante el acceso a [esta página](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).

-   Acceso [esta página](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx) para obtener más información sobre sus servicios de Microsoft para ayudar con la investigación y la recuperación de un incidente

-   Para implementar ATA, sigue la Guía de implementación disponible [aquí](http://aka.ms/ata).

### <a name="security-privileged-access-roadmap-stage-3"></a>Seguridad privilegiadas guía básica de acceso: Etapa 3
Etapa 3 de la guía básica se basa en las mitigaciones de las fases 1 y 2 para reforzar y agregar mitigaciones en todo el espectro. Este diagrama muestra la fase 3 visualmente:

![Diagrama que muestra la fase 3](../media/securing-privileged-access/PAW_LP_Fig8.JPG)

Estas funcionalidades se compila en las mitigaciones de fases anteriores y mover las defensas a una más proactiva postura.

**1. moderniza Roles y modelo de delegación**

Para reducir el riesgo de seguridad, debes rediseñar todos los aspectos de sus funciones y el modelo de delegación para que sea compatible con las reglas del modelo de nivel, dar cabida a roles administrativos de servicio de nube e incorporar la facilidad de uso del administrador como uno de los principios clave. Este modelo debe sacar provecho de las funcionalidades JIT y JEA implementadas en las fases anteriores, así como la tecnología de automatización de la tarea para alcanzar estos objetivos.

**2. autenticación de Passport para todos los administradores o tarjeta inteligente**

Para aumentar el nivel de seguridad y facilidad de uso de autenticación de administrador, deberías requerir la autenticación firme para todas las cuentas administrativas hospedadas en Azure Active Directory y en Windows Server Active Directory (incluidas las cuentas de federados a un servicio de nube).

**3. administrador bosque para los administradores de Active Directory**

Para ofrecer la máxima protección para los administradores de Active Directory, configurar un entorno que no tiene ninguna dependencia de seguridad de tu Active Directory de producción y está aislado, pero los ataques de todos los sistemas de más confianza en el entorno de producción. Para obtener más información sobre la arquitectura ESAE visita [esta página](http://aka.ms/esae).

**4. directiva de integridad de código para controladores de dominio (servidor de 2016)**

Para limitar el riesgo de programas no autorizados en los controladores de dominio de las operaciones de ataque de adversario e involuntarios errores administrativos, configurar la integridad de código de Windows Server 2016 kernel (controladores) y el modo de usuario (aplicaciones) para permitir solamente los archivos ejecutables autorizados para ejecutarse en el equipo.

**5. blindadas máquinas virtuales para controladores de dominio virtuales (estructura de servidor 2016 Hyper-V)**

Para proteger los controladores de dominio virtualizada de vectores de ataque que aprovechan la pérdida inherente de la máquina virtual de seguridad física, usa esta nueva funcionalidad de 2016 Hyper-V Server para ayudar a evitar el robo de los secretos de Active Directory de DC Virtual. Con esta solución, puede cifrar las máquinas virtuales de generación 2 para proteger los datos de la máquina virtual con inspección, el robo y la alteración por los administradores de red y almacenamiento, así como para reforzar el acceso a la máquina virtual contra los ataques de los administradores de host de Hyper-V.

## <a name="am-i-done"></a>¿He terminado?
Completar esta guía básica, se obtendrán protecciones de acceso con privilegios segura para los ataques que están actualmente adversarios conocidos y estará disponibles para hoy. Por desgracia, las amenazas de seguridad constantemente evolucionan y MAYÚS, por lo que recomendamos que ver seguridad como un proceso continuo centrado en generar el costo y reducir la tasa de éxito de adversarios destinados a tu entorno.

Protección con privilegios acceso es un primer paso para establecer las garantías de seguridad para los activos de negocio en una organización moderna fundamental, pero no es la única parte de un programa de seguridad completa que incluye elementos como directiva, las operaciones, seguridad de la información, servidores, aplicaciones, equipos, dispositivos, fabric de nube y otros componentes proporcionan las garantías de seguridad que necesites.

Para obtener más información sobre la creación de un plan de seguridad completa, consulta la sección "las responsabilidades del cliente y guía básica" de la seguridad de la nube de Microsoft de documento arquitectos disponible [aquí](http://aka.ms/securecustomer).

Para obtener más información sobre sus servicios de Microsoft para ayudar con cualquiera de estos temas, ponte en contacto con tu representante de Microsoft o visite [esta página](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).

### <a name="related-topics"></a>Temas relacionados
[Probar Premier: cómo mitigar los Pass-the-Hash y otras formas de robo de credenciales](https://channel9.msdn.com/Blogs/Taste-of-Premier/Taste-of-Premier-How-to-Mitigate-Pass-the-Hash-and-Other-Forms-of-Credential-Theft)

[Microsoft avanzado de análisis de amenazas](http://aka.ms/ata)

[Proteger las credenciales de dominio derivadas con Credential Guard](https://technet.microsoft.com/en-us/library/mt483740%28v=vs.85%29.aspx)

[Device Guard Overview](https://technet.microsoft.com/en-us/library/dn986865(v=vs.85).aspx)

[Proteger activos de gran valor con estaciones de trabajo de administración segura](https://msdn.microsoft.com/en-us/library/mt186538.aspx)

[Modo de usuario aislado de Windows 10 con David Probert (Channel 9)](https://channel9.msdn.com/Blogs/Seth-Juarez/Isolated-User-Mode-in-Windows-10-with-Dave-Probert)

[Aísla los procesos de modo usuario y las características de Windows 10 con Logan Gabriel (Channel 9)](http://channel9.msdn.com/Blogs/Seth-Juarez/Isolated-User-Mode-Processes-and-Features-in-Windows-10-with-Logan-Gabriel)

[Más en procesos y características en modo de usuario aislado de Windows 10 con David Probert (Channel 9)](https://channel9.msdn.com/Blogs/Seth-Juarez/More-on-Processes-and-Features-in-Windows-10-Isolated-User-Mode-with-Dave-Probert)

[Mitigación de robo de credenciales mediante el modo de usuario aislado (Channel 9) de Windows 10](https://channel9.msdn.com/Blogs/Seth-Juarez/Mitigating-Credential-Theft-using-the-Windows-10-Isolated-User-Mode)

[Habilitar validación KDC estricta en Windows Kerberos](https://www.microsoft.com/en-us/download/details.aspx?id=6382)

[Novedades en la autenticación Kerberos para Windows Server 2012](https://technet.microsoft.com/library/hh831747.aspx)

[Mecanismo de autenticación de AD DS en la Guía paso a paso de Windows Server 2008 R2](https://technet.microsoft.com/library/dd378897(v=ws.10).aspx)

[Módulo de plataforma segura](https://docs.microsoft.com/en-us/windows/device-security/tpm/trusted-platform-module-overview)
