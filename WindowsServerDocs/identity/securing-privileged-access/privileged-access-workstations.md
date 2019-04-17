---
title: Estaciones de trabajo con privilegios de acceso
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 93589778-3907-4410-8ed5-e7b6db406513
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 06/09/2016
ms.openlocfilehash: ce64974d771a11ef11257bceef1c3fde1797a7da
ms.sourcegitcommit: 3bf47cf4e25896725137d983d63b8041a53cb9a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2018
---
# <a name="privileged-access-workstations"></a>Estaciones de trabajo con privilegios de acceso

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Estaciones de trabajo con privilegios de acceso (garras) proporciona un sistema operativo dedicado para las tareas con información confidencial que están protegidos contra ataques de Internet y vectores de amenazas. Separar estos tareas con información confidencial y cuentas de la diarias usar estaciones de trabajo y dispositivos proporciona muy alta protección frente a ataques de suplantación de identidad, aplicación y vulnerabilidades de sistema operativo, varios ataques de suplantación y ataques de robo de credenciales como el registro de pulsación de tecla, [Pass-the-Hash](https://www.microsoft.com/en-us/download/details.aspx?id=36036), y [Pass-The-Ticket](https://download.microsoft.com/download/7/7/A/77ABC5BD-8320-41AF-863C-6ECFB10CB4B9/Mitigating%20Pass-the-Hash%20(PtH)%20Attacks%20and%20Other%20Credential%20Theft%20Techniques_English.pdf).

## <a name="architecture-overview"></a>Introducción a la arquitectura
El siguiente diagrama muestra un "canal" independiente para la administración (una tarea muy confidencial) que se crea al mantener separadas las cuentas administrativas dedicadas y estaciones de trabajo.

![Diagrama que muestra un canal"independiente" para la administración (una tarea muy confidencial) que se crea al mantener separadas las cuentas administrativas dedicadas y estaciones de trabajo](../media/privileged-access-workstations/PAWFig1.JPG)

Este enfoque de diseño se basa en las protecciones que se encuentran en Windows 10 [Credential Guard](https://technet.microsoft.com/en-us/library/mt483740%28v=vs.85%29.aspx) y [Device Guard](https://technet.microsoft.com/en-us/library/dn986865(v=vs.85).aspx) características y va más allá de las protecciones para cuentas con información confidencial y tareas.

Esta metodología es apropiada para cuentas con acceso a activos de gran valor:

-   **Privilegios administrativos** la garras proporcionan una mayor seguridad gran impacto roles administrativos de TI y tareas. Esta arquitectura puede aplicarse a la administración de varios tipos de sistemas, incluidos los dominios de Active Directory y bosques, inquilinos de Azure Active Directory de Microsoft, inquilinos de Office 365, redes de Control de proceso (NCP), sistemas de Control de supervisión y datos de compra (SCADA), cajeros automáticos (cajeros automáticos) y dispositivos de punto de venta (PoS).

-   **Trabajadores de información de sensibilidad altos** el enfoque usado en un PATA también puede proporcionar protección de tareas del trabajador información muy confidencial y personal, como aquellas que involucran la actividad de fusión y adquisición de anuncio anterior, informes financieros de versión preliminar, presencia organizativa medios sociales, comunicaciones ejecutivas, no patentados secretos comerciales, investigación con información confidencial u otros propietarias o datos confidenciales. Esta guía no describa la configuración de estos escenarios de trabajo de la información en profundidad o incluir este escenario en las instrucciones técnicas.

    > [!NOTE]
    > Microsoft IT utiliza garras (internamente denominados "estaciones de trabajo de administración segura", o las saw) para administrar el acceso seguro a los sistemas de gran valor internos de Microsoft. Esta guía tiene detalles adicionales sobre el uso de PATA de Microsoft en la sección "Cómo Microsoft usa estaciones de trabajo de administrador". Para obtener más información acerca de este enfoque de entorno de activos de gran valor, consulte el artículo, [proteger activos de gran valor con estaciones de trabajo de administración segura](https://msdn.microsoft.com/en-us/library/mt186538.aspx).

Este documento describe por qué se recomienda para proteger cuentas de gran impacto con privilegios, el aspecto de estas soluciones PATA para proteger los privilegios administrativos y cómo implementar rápidamente una solución PATA para la administración de servicios de dominio y la nube.

Este documento proporciona instrucciones detalladas para implementar varias configuraciones de PATA e incluye instrucciones de implementación detallados para empezar a trabajar en proteger cuentas de gran impacto comunes:

-   **La fase 1: implementación inmediata para los administradores de Active Directory** Esto proporciona un PATA rápidamente que puede proteger local, funciones de administración de dominios y bosques

-   **Fase 2 - PATA se extiende a todos los administradores** Esto habilita la protección de los administradores de servicios de nube como Office 365 y Azure, servidores de empresa, las aplicaciones de empresa y estaciones de trabajo

-   **Fase 3: seguridad avanzada PATA** esta sección explica protecciones adicionales y sobre consideraciones de seguridad PATA

### <a name="why-a-dedicated-workstation"></a>¿Por eso, una estación de trabajo dedicado?
El entorno actual para las organizaciones es abundan sofisticados de suplantación de identidad y otros ataques de internet que crean continuo riesgo de peligro de seguridad para las cuentas de expuestas a internet y estaciones de trabajo.

Este entorno de amenaza requiere que las organizaciones adoptar una posición de seguridad "suponga una infracción" al diseñar las protecciones de activos de gran valor, como las cuentas administrativas y activos empresariales confidenciales. Estos activos de gran valor deben protegerse contra tanto directa amenazas de internet como ataques montada desde otros dispositivos en el entorno, servidores y estaciones de trabajo.

![Ilustración que muestra el riesgo que administrados activos si un atacante obtiene el control de una estación de trabajo del usuario en el que se usan las credenciales confidenciales](../media/privileged-access-workstations/PAWFig2.JPG)

Esta figura muestra riesgo que administrados activos si un atacante obtiene el control de una estación de trabajo del usuario en el que se usan las credenciales confidenciales.

Un atacante en control de un sistema operativo tiene numerosas formas en que se va a obtener acceso a toda la actividad de la estación de trabajo de forma ilícita y suplantar la cuenta legítima. Una variedad de técnicas de ataques conocidos y desconocidos puede usarse para obtener este nivel de acceso. El aumento volumen y sofisticación de cyberattacks han hecho necesario ampliar dicho concepto separación para separar completamente los sistemas operativos de cliente para las cuentas con información confidencial. Para obtener más información sobre estos tipos de ataques, visita el [sitio Web de pasar el Hash](https://www.microsoft.com/pth) informativos notas del producto, vídeos y mucho más.

El enfoque PATA es una extensión de la práctica recomendada bien establecida para usar separada de administración y las cuentas de usuario para personal administrativo. Este procedimiento usa una cuenta de administrador individualmente asignada que es completamente independiente de la cuenta de usuario estándar del usuario. PATA se basa en la práctica de separación de esa cuenta proporcionando una estación de trabajo de confianza de esas cuentas con información confidencial.

> [!NOTE]
> Microsoft IT utiliza garras (internamente denominados "estaciones de trabajo de administración segura", o las saw) para administrar el acceso seguro a los sistemas de gran valor internos de Microsoft.  Esta guía tiene detalles adicionales sobre el uso de PATA de Microsoft en la sección "Cómo Microsoft usa estaciones de trabajo de administrador"
>
> Para obtener más información acerca de este enfoque de entorno de activos de gran valor, consulte el artículo [proteger activos de gran valor con estaciones de trabajo de administración segura](https://msdn.microsoft.com/en-us/library/mt186538.aspx).

Esta guía PATA pretende que te ayudarán a implementar esta funcionalidad para proteger cuentas de gran valor, como los administradores de TI con privilegios elevados y alta sensibilidad empresarial. La guía te ayudará a:

-   Restringir la exposición de credenciales a solo los hosts de confianza

-   Proporcionar una estación de trabajo de alta seguridad para los administradores para que puedan realizar fácilmente tareas administrativas.

Restringir las cuentas con información confidencial a usando solo reforzados garras es una protección sencilla de estas cuentas que es muy difícil para un adversario derrotar y muy útil para los administradores.

#### <a name="alternate-approaches---limitations-considerations-and-integration"></a>Enfoques alternativos - integración, consideraciones y limitaciones
Esta sección contiene información sobre cómo se compara la seguridad de enfoques alternativos para PATA y cómo integrar correctamente estos enfoques dentro de una arquitectura PATA. Todos estos métodos llevan riesgos importantes cuando se implementa de manera aislada, pero pueden agregar valor a una implementación PATA en algunos escenarios.

**Credential Guard y Microsoft Passport**

Introducida en Windows 10, [Credential Guard](https://technet.microsoft.com/en-us/library/mt483740%28v=vs.85%29.aspx) usa hardware y seguridad de virtualización para mitigar los ataques de robo de credenciales comunes, como Pass-the-Hash, proteger las credenciales derivadas. La clave privada las credenciales utilizadas por [Microsoft Passport](http://aka.ms/passport) puede ser también esté protegida por hardware de módulo de plataforma segura (TPM).

Estos son mitigaciones eficaces, pero las estaciones de trabajo pueden seguir siendo vulnerables a determinados ataques incluso si las credenciales están protegidas con Credential Guard o el pasaporte. Ataques pueden incluir el mal uso de privilegios y el uso de credenciales directamente desde un dispositivo en peligro, reutilizar credenciales robadas anteriormente antes de habilitar Credential Guard y el abuso de configuraciones de aplicación de administración herramientas y débil en la estación de trabajo.

La Guía de PATA en esta sección incluye el uso de muchas de estas tecnologías para las cuentas de alta sensibilidad y tareas.

**VM administrativa**

Una máquina virtual (VM) de administrativa es un sistema operativo dedicado para las tareas administrativas hospedado en un equipo de escritorio de usuario estándar. Aunque este enfoque es similar a PATA proporcionando un sistema operativo dedicado para las tareas administrativas, tiene un problema grave que depende de la máquina virtual administrativa en el escritorio del usuario estándar para su seguridad.

El siguiente diagrama muestra la capacidad de los atacantes sigue la cadena de control para el objeto de destino de interés con una VM de administrador en una estación de trabajo del usuario y que es difícil de crear una ruta de acceso de la configuración inversa.

No se admite la arquitectura de PATA para hospedar un máquina virtual en una estación de trabajo de usuario de administrador, pero un usuario de la máquina virtual con una imagen corporativa estándar se pueden hospedar en un host PATA para proporcionar a personal con un solo equipo para todas las responsabilidades.

![Diagrama de la arquitectura de PATA](../media/privileged-access-workstations/PAWFig9.JPG)

**Servidor de accesos directos**

Administrativas arquitecturas "Accesos directos servidor" configuración un pequeño número de servidores consola de administración y restringir a personal a usarlos para tareas administrativas. Por lo general, esto se basa en los servicios de escritorio remoto, una solución de virtualización de la presentación de terceros 3 o una tecnología de infraestructura de Escritorio Virtual (VDI).

Este enfoque se propone con frecuencia para mitigar el riesgo para la administración y proporcionar algunos garantías de seguridad, pero el enfoque de servidor de accesos directos por sí sola vulnerable a determinados ataques porque infringe el [principio de "origen limpio"](http://aka.ms/cleansource). El principio de origen limpio requiere que todas las dependencias de seguridad de confianza como como el objeto que se protege.

![Ilustración que muestra la relación de un control simple](../media/privileged-access-workstations/PAWFig3.JPG)

Esta figura muestra la relación de un control simple. Cualquier asunto de control de un objeto es una dependencia de seguridad de ese objeto. Si un adversario puede controlar una dependencia de seguridad de un objeto de destino (sujeto), pueden controlar ese objeto.

La sesión administrativa en el servidor de accesos directos se basa en la integridad del equipo local, tener acceso a él. Si este equipo es una estación de trabajo del usuario sujetos a ataques de suplantación de identidad y otros ataques basados en internet, a continuación, la sesión administrativa también está sujeta a los riesgos.

![Ilustración que muestra cómo los atacantes pueden sigue una cadena de control establecidos para el objeto de destino de interés](../media/privileged-access-workstations/PAWFig4.JPG)

La figura anterior muestra cómo los atacantes pueden sigue una cadena de control establecidos para el objeto de destino de interés.

Aunque algunos controles de seguridad avanzada, como la autenticación multifactor puede aumentar la dificultad de un atacante apoderen de esta sesión administrativa de la estación de trabajo del usuario, ninguna característica de seguridad completamente pueden protegerse contra ataques técnicos cuando un atacante tiene acceso administrativo del equipo de origen (por ejemplo, inserte ilícitos comandos en una sesión legítima, los procesos de secuestro legítimos y así sucesivamente.)

La configuración predeterminada en esta guía PATA instala las herramientas administrativas en el pata, pero una arquitectura de servidor de accesos directos también puede agregarse si es necesario.

![Ilustración que muestra cómo revertir la relación de control y acceso a las aplicaciones de usuario de una estación de trabajo de administrador te ofrece el atacante ninguna ruta de acceso para el objeto de destino](../media/privileged-access-workstations/PAWFig5.JPG)

Esta figura muestra cómo revertir la relación de control y acceso a las aplicaciones de usuario de una estación de trabajo de administrador te ofrece el atacante ninguna ruta de acceso para el objeto de destino. El servidor de accesos directos de usuario aún se expone a riesgo, por lo que aún se deben aplicar controles protectoras adecuados, controles investigue y procesos de respuesta para ese equipo a través de internet.

Esta configuración requiere que los administradores se siguen los procedimientos operativos estrechamente para garantizar que no accidentalmente entran credenciales de administrador en la sesión del usuario en el escritorio.

![Ilustración que muestra cómo obtener acceso a un servidor de accesos directos administrativas desde un PATA no agrega ninguna ruta de acceso para el atacante en los activos administrativos](../media/privileged-access-workstations/PAWFig6.JPG)

Esta figura muestra cómo acceder a un servidor de accesos directos administrativas desde un PATA no agrega ninguna ruta de acceso para el atacante en los activos administrativos. Un servidor de accesos directos con un PATA permite en este caso consolidar el número de ubicaciones para supervisar la actividad administrativa y distribuir las herramientas y las aplicaciones administrativas. Esto agrega cierta complejidad del diseño, pero puede simplificar las actualizaciones de supervisión y el software de seguridad si un gran número de cuentas y estaciones de trabajo se usan en tu implementación PATA. Compilar y configurada a los estándares de seguridad similar como la pata que el servidor de accesos directos.

**Soluciones de administración de privilegios**

Soluciones de administración con privilegios son aplicaciones que ofrecen acceso temporal que discretos privilegios o cuentas con privilegios a petición. Soluciones de administración de privilegios son un componente de una estrategia completa para proteger el acceso con privilegios y proporcionar crucial visibilidad y responsabilidad de actividad administrativa muy valioso.

Estas soluciones normalmente usan un flujo de trabajo flexible para conceder acceso y muchos tienen características de seguridad adicionales y funciones como la administración de contraseñas de cuenta de servicio y la integración con los servidores de accesos directos administrativas. Hay muchas soluciones en el mercado que proporcionan funcionalidades de administración, uno de los cuales es la administración de acceso de administrador de identidad de Microsoft (MIM) con privilegios (PAM) de privilegios.

Microsoft recomienda usar un PATA a soluciones de administración de privilegios de acceso. Debe concederse acceso a estas soluciones solo para garras. Microsoft recomienda el uso de estas soluciones como sustituto de un PATA porque el acceso a privilegios de uso de estas soluciones de un escritorio del usuario potencialmente comprometidos infringe el [origen limpio](http://aka.ms/cleansource) principio como se muestra en el siguiente diagrama:

![Diagrama que muestra cómo Microsoft recomienda el uso de estas soluciones como sustituto de un PATA porque tiene acceso a privilegios de uso de estas soluciones de un escritorio del usuario potencialmente comprometidos infringe el principio de origen limpio](../media/privileged-access-workstations/PAWFig7.JPG)

Proporcionar un PATA para acceder a estas soluciones te permite aprovechar las ventajas de seguridad de PATA y la solución de administración de privilegios, como se muestra en este diagrama:

![Diagrama que muestra cómo proporcionar un PATA para acceder a estas soluciones le permite obtener los beneficios de seguridad de PATA y la solución de administración de privilegios](../media/privileged-access-workstations/PAWFig8.JPG)

> [!NOTE]
> Estos sistemas deben clasificarse en el nivel más alto del privilegio que administran y estar protegidos igual o superior a ese nivel de seguridad. Estos normalmente están configurados para administrar activos de nivel 0 y soluciones de nivel 0 y deben estar clasificados en nivel 0.
> Para obtener más información sobre el modelo de niveles, consulta [http://aka.ms/tiermodel](http://aka.ms/tiermodel) para obtener más información sobre los grupos de nivel 0, consulta equivalencia de nivel 0 en [proteger el Material de referencia de acceso con privilegios](../securing-privileged-access/securing-privileged-access-reference-material.md).

Para obtener más información sobre la implementación de administración de Microsoft Identity Manager (MIM) con privilegios de acceso (PAM), consulta [http://aka.ms/mimpamdeploy](http://aka.ms/mimpamdeploy)

#### <a name="how-microsoft-is-using-admin-workstations"></a>¿Cómo utiliza Microsoft estaciones de trabajo de administrador
Microsoft usa el enfoque de arquitectura PATA tanto internamente en nuestros sistemas, así como con nuestros clientes. Microsoft usa estaciones de trabajo administrativas internamente en un número de capacidades de administración de infraestructura de IT de Microsoft, desarrollo de infraestructura de fabric de nube de Microsoft y las operaciones y otros activos de gran valor.

Esta guía se basa directamente en la arquitectura de referencia de la estación de trabajo de acceso con privilegios (PATA) implementada por nuestros equipos de los servicios profesionales la ciberseguridad proteger a los clientes la ciberseguridad ataques. Las estaciones de trabajo administrativas también son un elemento clave de la máxima protección para las tareas de administración de dominio, la arquitectura de referencia de bosque administrativas mejorado seguridad administrativas entorno (ESAE).

Para obtener más detalles en el bosque de ESAE administrativo, consulta [enfoque de diseño de bosque administrativas ESAE](http://aka.ms/ESAE) sección [proteger el Material de referencia de acceso con privilegios](../securing-privileged-access/securing-privileged-access-reference-material.md).

Para obtener más información sobre atraiga a los servicios de Microsoft para implementar un PATA o ESAE para el entorno, ponte en contacto con tu representante de Microsoft o visite [esta página](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).

### <a name="what-is-a-privileged-access-workstation-paw"></a>¿Qué es una estación de trabajo con privilegios de acceso (PATA)?
En términos sencillos, un PATA es una estación de trabajo reforzado y bloqueada diseñado para proporcionar garantías de alta seguridad para las tareas y cuentas con información confidencial.  Garras se recomiendan para la administración de sistemas de identidad, servicios en la nube y fabric nube privada, así como las funciones confidenciales del negocio.

> [!NOTE]
> La arquitectura de PATA no requiere una asignación 1:1 de cuentas en estaciones de trabajo, aunque se trata de una configuración común. PATA crea un entorno de confianza de la estación de trabajo que puede usarse en una o varias cuentas.

Para proporcionar la mayor seguridad, garras siempre deben ejecutar más actualizado y protegido sistema operativo disponible: Microsoft recomienda Windows 10 Enterprise, que incluye una serie de características de seguridad adicional no está disponibles en otras ediciones (en particular, [Credential Guard](https://technet.microsoft.com/en-us/library/mt483740%28v=vs.85%29.aspx) y [Device Guard](https://technet.microsoft.com/en-us/library/dn986865(v=vs.85).aspx)).

> [!NOTE]
> Las organizaciones sin acceso a Windows 10 Enterprise pueden usar Windows 10 Pro, que incluye muchas de las críticas tecnologías fundamentales para garras, incluidas escritorio remoto, BitLocker y arranque seguro.  Los clientes de Education pueden usar Windows 10 Education.  Windows 10 Home no debe usarse para un PATA.
>
> Para una matriz de comparación de las diferentes ediciones de Windows 10 lea [este artículo](https://www.microsoft.com/en-us/WindowsForBusiness/Compare).

Los controles de seguridad en PATA están centrados en mitigar el impacto más notable y probablemente riesgos de comprometer. Entre ellas se incluyen la mitigación de ataques en el entorno y mitigar los riesgos que pueden afectar a los controles de PATA con el tiempo:

-   **Ataques de Internet** -mayoría de los ataques proceden directa o indirectamente de fuentes de internet y usar internet para exfiltration y comando y control (C2). Aislar el PATA de abrir internet es un elemento clave para garantizar la pata no se ve afectado.

-   **El riesgo de facilidad de uso** -si un PATA es demasiado difícil de usar para las tareas diarias, los administradores será motivados para crear soluciones para facilitar su trabajo. A menudo, estas soluciones alternativas abren la estación de trabajo y las cuentas de riesgos de seguridad importantes, por lo que es fundamental para implicar y permiten a los usuarios PATA para mitigar estos problemas de facilidad de uso de forma segura. Con frecuencia, esto se logra mediante la escucha de sus comentarios, instalar herramientas y scripts necesarios para realizar sus trabajos y asegurarte de que el personal administrativo todos los son conscientes de por qué debe usar un pata, qué un PATA es y cómo usarlo correctamente y correctamente.

-   **Riesgos del entorno** -porque muchos otros equipos y cuentas en el entorno se exponen al directorio de riesgo de internet o indirectamente, un PATA debe estar protegido contra los ataques de activos en peligro en el entorno de producción. Esto requiere limitar las herramientas de administración y las cuentas que tengan acceso a la garras en el mínimo necesario para proteger y supervisar estas estaciones de trabajo especializados.

-   **Proporcionar la manipulación de cadena** : si bien es imposible quitar todos los posibles riesgos de manipulación en la cadena de suministro de hardware y software, realizar algunas acciones claves pueden mitigar los vectores de ataque críticos que estén disponibles para los atacantes. Esto incluye validar la integridad de todos los medios de instalación ([principio de origen limpio](http://aka.ms/cleansource)) y el uso de un proveedor de confianza y de confianza para hardware y software.

-   **Ataques físicos** -garras porque puede ser móvil físicamente y se usa fuera de instalaciones físicamente seguras, deben protegerse contra ataques que aprovechan no autorizado acceso físico al equipo.

> [!NOTE]
> Un PATA protege un entorno de un adversario que ya ha obtenido acceso administrativo a través de un bosque de Active Directory.
> Dado que muchas de las implementaciones existentes de los servicios de dominio de Active Directory han estado funcionando durante años corriendo el riesgo de robo de credenciales, las organizaciones deben suponga una infracción y considerar la posibilidad de que pueden tener un el compromiso de credenciales de administrador de dominio o enterprise. Una organización que sospecha compromiso de dominio debe considerar el uso de servicios de respuesta a incidentes profesional.
>
> Para obtener más información acerca de la Guía de respuesta y la recuperación, consulta la "responder a la actividad sospechosa" y "Recuperar de una infracción" secciones de [Mitigating Pass-the-Hash y otros robo de credenciales](https://www.microsoft.com/pth), versión 2.
>
> Visita [respuesta a incidentes de Microsoft y servicios de recuperación](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx) para obtener más información.

### <a name="paw-hardware-profiles"></a>PATA perfiles de Hardware
Personal administrativo también es usuarios estándar demasiado: necesitan no solo un pata, sino también una estación de trabajo de usuario estándar para comprobar el correo electrónico, navegar por la web y obtener acceso a corporativa aplicaciones de línea de negocio.  Garantizar que los administradores pueden permanecer productivas y seguras es fundamental para el éxito de cualquier implementación PATA.  Una solución segura que limita considerablemente la productividad se abandonada por los usuarios en favor de uno que mejora la productividad (incluso si se realiza en un modo no seguro).

Para equilibrar la necesidad de seguridad con la necesidad de productividad, Microsoft recomienda usar uno de estos perfiles de hardware PATA:

-   **Hardware dedicado** -dispositivos dedicados para las tareas del usuario frente a las tareas administrativas independientes

-   **El uso simultáneo** -dispositivo único que se puede ejecutar tareas de usuario y tareas administrativas al mismo tiempo, al aprovechar la virtualización de sistema operativo o una presentación.

Las organizaciones pueden usar solo perfil o ambos. No hay ningún preocupaciones de interoperabilidad entre los perfiles de hardware y las organizaciones tienen la flexibilidad para que coincida con el perfil de hardware para la situación de un administrador determinado y una necesidad específica.

> [!NOTE]
> Es fundamental que, en todos estos escenarios, personal administrativo se emite una cuenta de usuario estándar es independiente de designado cuentas administrativas. Las cuentas administrativas solo deben usarse en el sistema operativo de PATA administrativo.

Esta tabla resume las ventajas y desventajas de cada perfil de hardware, desde la perspectiva de facilidad de uso operativa y la productividad y la seguridad.  Ambos enfoques de hardware proporcionan contundentes de seguridad para las cuentas administrativas contra el robo de credenciales y reutilización.

|**Escenario**|**Ventajas**|**Desventajas de**|
|--------|---------|-----------|
|Hardware dedicado|-Cobertura alta sensibilidad de tareas<br />-Separación de seguridad más seguro|-Espacio en el escritorio adicionales<br />-Peso adicional (para el trabajo remoto)<br />: Costo de hardware|
|Uso simultáneo|-Un costo de hardware<br />-Experiencia de dispositivo único|-Uso compartido sencillo con el teclado o mouse crea un riesgo de errores y riesgos involuntarios|

Esta guía contiene instrucciones detalladas para la configuración de PATA para el enfoque de hardware dedicado. Si tienes requisitos para los perfiles de hardware de uso simultáneo, puedes adaptar las instrucciones que se basa en esta guía o contratar una organización de los servicios profesionales como Microsoft para ayudar con él.

**Hardware dedicado**

En este escenario, un PATA se usa para la administración que es completamente independiente del equipo que se utiliza para actividades diarias como correo electrónico, la edición de documentos o el trabajo de desarrollo. Todas las aplicaciones y herramientas administrativas están instaladas en la pata y todas las aplicaciones de productividad están instaladas en la estación de trabajo de usuario estándar. Las instrucciones paso a paso en esta guía se basan en el perfil de hardware.

**Uso simultáneo - agregar un máquina virtual de usuario local**

En este escenario de uso simultáneo, un solo equipo se usa para las tareas de administración y las actividades diarias como correo electrónico, la edición de documentos o el trabajo de desarrollo. En esta configuración, el sistema operativo del usuario está disponible mientras está desconectado (para editar documentos y trabajar en el correo electrónico almacenada en caché localmente), pero requiere hardware y soporte técnico de los procesos que pueden acomodar este estado desconectado.

![Diagrama que muestra el mismo equipo en un escenario de uso simultáneo usado para las tareas de administración y las actividades diarias, como correo electrónico, la edición de documentos y el trabajo de desarrollo](../media/privileged-access-workstations/PAWFig10.JPG)

El hardware físico ejecuta localmente dos sistemas operativos:

-   **Sistema operativo del administrador** -el host físico ejecuta Windows 10 en el host PATA para tareas administrativas

-   **Sistema operativo del usuario** -invitado de máquina virtual de cliente Hyper-V A Windows 10 ejecuta una imagen corporativa

Con Windows 10Hyper-V, una máquina virtual de invitado (también ejecuta Windows 10) puede tener una experiencia de usuario enriquecida incluidas sonido, vídeo y aplicaciones de comunicaciones de Internet como Skype para empresas.

En esta configuración, se realiza el trabajo diario que no se requieren privilegios administrativos en la máquina virtual de sistema operativo de usuario que tiene una imagen de Windows 10 corporativa normal y no está sujeta a restricciones aplicadas al host PATA. Todas las tareas administrativas se realiza en el sistema operativo de administrador.

Para configurarlo, sigue las instrucciones de esta guía para el host pata, agregar características de cliente Hyper-V, crear una máquina virtual de usuario y, a continuación, instala una imagen corporativa de Windows 10 en la máquina virtual de usuario.

Lectura [Client Hyper-V](https://technet.microsoft.com/en-us/library/hh857623.aspx) artículo para obtener más información acerca de esta funcionalidad. Ten en cuenta que el sistema operativo en máquinas virtuales de invitado, deberás tener una licencia por [licencias de productos de Microsoft](https://www.microsoft.com/en-us/Licensing/product-licensing/products.aspx), como se describe también [aquí](https://www.microsoft.com/en-us/Licensing/learn-more/brief-windows-virtual-machine.aspx).

**Uso simultáneo - Agregar conexión de RemoteApp, RDP o un VDI**

En este escenario de uso simultáneo, se usa un solo equipo tanto las tareas de administración y funcionan actividades diarias como correo electrónico, la edición de documentos y el desarrollo. En esta configuración, los sistemas operativos de usuario se implementan y administran de forma centralizada (en la nube o en el centro de datos), pero no están disponibles mientras está desconectado.

![Ilustración que muestra un solo equipo en un escenario de uso simultáneo usa tanto las tareas de administración y las actividades diarias como correo electrónico, la edición de documentos y el desarrollo funcionan](../media/privileged-access-workstations/PAWFig11.JPG)

El hardware físico ejecuta un sistema operativo PATA localmente para tareas administrativas y los contactos de un Microsoft o 3 terceros servicio Escritorio remoto para aplicaciones de usuario, como correo electrónico, edición de documentos y aplicaciones empresariales.

En esta configuración, se realiza trabajo diario que no se requieren privilegios administrativos en los OS(es) remoto y las aplicaciones que no están sujetas a restricciones aplicadas al host PATA. Todas las tareas administrativas se realiza en el sistema operativo de administrador.

Para configurarlo, sigue las instrucciones de esta guía para el host pata, permitir la conectividad de red a los servicios de escritorio remoto y, a continuación, agregar accesos directos al escritorio del usuario pata obtener acceso a las aplicaciones. Los servicios de escritorio remotos podrían hospedarse en muchas formas, incluidos:

-   Un servicio de escritorio remoto o VDI existente (local o en la nube)

-   Un nuevo servicio instalas local o en la nube

-   Azure RemoteApp con plantillas de Office 365 preconfiguradas o tus propias imágenes de instalación

Para obtener más información sobre Azure RemoteApp, visite [esta página](https://www.remoteapp.windowsazure.com).

### <a name="paw-scenarios"></a>PATA escenarios
Esta sección contiene instrucciones sobre qué escenarios esta guía PATA debe aplicarse a. En todos los escenarios, los administradores deben aprender a usar solo garras para llevar a cabo soporte técnico de sistemas remotos. Para favorecer el uso correcto y seguro, todos los usuarios de PATA también se debería proporcionar comentarios para mejorar la experiencia de PATA y esta información deben ser revisados cuidadosamente para la integración con el programa PATA.

En todos los escenarios, pueden usarse para cumplir los requisitos de seguridad o la facilidad de uso de las funciones de seguridad adicionales en fases posteriores y perfiles de hardware diferentes en esta guía.

> [!NOTE]
> Ten en cuenta que esta guía explícitamente diferencia entre que requieren acceso a servicios específicos en internet (por ejemplo, los portales de administración de Azure y Office 365) y "Abrir Internet" de todos los hosts y servicios.

Consulta la [página de la capa modelo](http://aka.ms/tiermodel) para obtener más información sobre las designaciones de nivel.

|**Escenarios**|**¿Usar PATA?**|**Ámbito y las consideraciones de seguridad**|
|---------|--------|---------------------|
|Administradores de Active Directory - nivel 0|Sí|Un PATA creada con la orientación de la fase 1 es suficiente para esta función.<br /><br />-Un bosque administrativo puede agregarse a ofrecer la máxima protección para este escenario. Para obtener más información en el bosque de ESAE administrativo, consulta [ESAE administrativas enfoque de diseño de bosque](http://aka.ms/esae)<br />-Un PATA puede usarse para administrar varios dominios o varios bosques.<br />-Si los controladores de dominio se hospedan en una infraestructura como servicio (IaaS) o solución de virtualización local, debería dar prioridad a implementar huellas de los administradores de estas soluciones|
|Servicios de administración de Azure IaaS y PaaS - nivel 1 o nivel 0 (consulta ámbito y las consideraciones de diseño)|Sí|Un PATA creada con la orientación proporcionada en la fase 2 es suficiente para esta función.<br /><br />-Garras deben usarse para al menos el administrador Global y el Administrador de facturación de la suscripción. También puedes usar garras para los administradores delegados de servidores con información confidencial o críticos.<br />-Garras deben usarse para administrar el sistema operativo y las aplicaciones que proporcionan la sincronización de directorios y federación de identidad para servicios en la nube como [Azure AD Connect](https://azure.microsoft.com/en-us/documentation/articles/active-directory-aadconnect/) y servicios de federación de Active Directory (ADFS).<br />-Las restricciones de red saliente deben permitir conectividad solo a los servicios en la nube autorizados mediante las instrucciones en la fase 2. Acceso internet abiertas no debería estar permitida de huellas.<br />-EMET debe estar configurado para todos los exploradores que se usan en la estación de trabajo **Nota:** una suscripción se considera nivel 0 para un bosque si hay controladores de dominio u otros hosts de nivel 0 en la suscripción. Una suscripción es el nivel 1 si no hay servidores de nivel 0 se hospedan en Azure|
|Inquilino de administración de Office 365 <br />-Nivel 1|Sí|Un PATA creada con la orientación proporcionada en la fase 2 es suficiente para esta función.<br /><br />-Garras deben usarse para al menos el Administrador de facturación de suscripción, administrador Global, Administrador de Exchange, Administrador de SharePoint y administración de usuarios roles de administrador. Dudes también en el uso de huellas de los administradores delegados de muy críticas o datos confidenciales.<br />-EMET debe estar configurado para todos los exploradores que se usan en la estación de trabajo<br />-Las restricciones de red saliente deben permitir conectividad solo a los servicios de Microsoft mediante las instrucciones en la fase 2. Acceso internet abiertas no debería estar permitida de huellas.|
|Administración del servicio de nube de otros IaaS o PaaS<br />-Franja de 0 o nivel 1 (consulta ámbito y las consideraciones de diseño)||Un PATA creada con la orientación proporcionada en la fase 2 es suficiente para esta función.<br /><br />-Garras deben usarse para cualquier otro rol que tenga derechos administrativos sobre máquinas virtuales de hospedado en la nube como la posibilidad de instalar a agentes, exportar archivos de disco duro o acceder al almacenamiento donde se almacenan las unidades de disco duro con sistemas operativos, los datos confidenciales o datos críticos del negocio.<br />-Las restricciones de red saliente deben permitir conectividad solo a los servicios de Microsoft mediante las instrucciones en la fase 2. Acceso internet abiertas no debería estar permitida de huellas.<br />-EMET debe estar configurado para todos los exploradores que se usan en la estación de trabajo. **Nota:** una suscripción es el nivel 0 para un bosque si hay controladores de dominio u otros hosts de nivel 0 en la suscripción. Una suscripción es el nivel 1 si no hay servidores de nivel 0 se hospedan en Azure.|
|Administradores de virtualización<br />-Franja de 0 o nivel 1 (consulta ámbito y las consideraciones de diseño)|Sí|Un PATA creada con la orientación proporcionada en la fase 2 es suficiente para esta función.<br /><br />-Garras deben usarse para cualquier otro rol que tenga derechos administrativos sobre máquinas virtuales como la posibilidad de instalar a agentes, exportar archivos de disco duro virtual o acceder al almacenamiento donde se almacenan las unidades de disco duro con la información del sistema operativo invitado, datos confidenciales o datos críticos del negocio. **Nota:** un sistema de virtualización (y sus administradores) se consideran nivel 0 para un bosque si hay controladores de dominio u otros hosts de nivel 0 en la suscripción. Una suscripción es el nivel 1 si no hay servidores de nivel 0 se hospedan en el sistema de virtualización.|
|Administradores de mantenimiento del servidor<br />-Nivel 1|Sí|Un PATA creada con la orientación proporcionada en la fase 2 es suficiente para esta función.<br /><br />-Un PATA debe usarse para los administradores que actualizarán, revisión y solucionar problemas de servidores empresariales y aplicaciones que se ejecutan Windows server, Linux y otros sistemas operativos.<br />-Herramientas de administración dedicado que necesite agregarse de garras controlen la escala más grande de los administradores de TI.|
|Administradores de la estación de trabajo de usuario <br />-Nivel de 2|Sí|Un PATA creada con la orientación proporcionada en la fase 2 es suficiente para funciones que tengan derechos administrativos en los dispositivos del usuario final (como la asistencia y el hardware admiten funciones).<br /><br />-Otras aplicaciones pueden necesitar instalarse en garras para habilitar la administración de vale y otras funciones de compatibilidad.<br />-EMET debe estar configurado para todos los exploradores que se usan en la estación de trabajo.<br />    Herramientas de administración dedicado que necesite agregarse de garras controlen la escala más grande de los administradores de TI.|
|SQL, SharePoint, la línea de negocio (LOB) o de administrador<br />-Nivel 1||Un PATA creada con la orientación de la fase 2 es suficiente para esta función.<br /><br />-Herramientas de administración adicional que necesite instalarse en garras para permitir que los administradores administrar las aplicaciones sin necesidad de conectarse a los servidores mediante Escritorio remoto.|
|Usuarios administrar la presencia de redes sociales|Parcialmente|Un PATA creada con la orientación proporcionada en la fase 2 sirve como punto de partida para proporcionar la seguridad de estas funciones.<br /><br />-Proteger y administrar cuentas de redes sociales con Azure Active Directory (AAD) para uso compartido, la protección y acceso de seguimiento a las cuentas de redes sociales.<br />    Para obtener más información sobre esta funcionalidad lee [esta entrada de blog](http://blogs.technet.com/b/ad/archive/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview.aspx).<br />-Las restricciones de red saliente deben permitir la conectividad a estos servicios. Esto puede hacerse al permitir conexiones a internet abiertas (mucho mayor riesgo de seguridad que niega muchos garantías PATA) o al permitir que solo direcciones DNS necesarias para el servicio (puede ser todo un reto obtener).|
|Usuarios estándar|No|Mientras que muchos de los pasos de refuerzo pueden usarse para los usuarios estándar, PATA está diseñado para aislar cuentas desde el acceso a internet abiertas que requieren la mayoría de usuarios de los deberes.|
|VDI invitado o pantalla completa|No|Mientras que muchos de los pasos de refuerzo pueden usarse para un sistema de pantalla completa para invitados, la arquitectura de PATA está diseñada para proporcionar una mayor seguridad para las cuentas de alta sensibilidad, no una mayor seguridad para las cuentas de sensibilidad inferior.|
|VIP usuario (ejecutivo, investigador, etcetera).|Parcialmente|Un PATA creada con la orientación proporcionada en la fase 2 sirve como punto de partida para proporcionar seguridad de estas funciones<br /><br />-Este escenario es similar a un equipo de escritorio de usuario estándar, pero generalmente tiene un perfil de la aplicación más pequeños, sencillo y conocidas. Normalmente, este escenario requiere descubrir y proteger datos confidenciales, servicios y aplicaciones (que pueden o no pueden instalarse en los equipos de escritorio).<br />-Estas funciones por lo general requieren un alto grado de seguridad y muy alto grado de facilidad de uso, que requieren cambios de diseño para cumplir con las preferencias del usuario.|
|Sistemas de control industriales (por ejemplo, SCADA NCP y controladores de dominio)|Parcialmente|Puede usarse un PATA creada con la orientación proporcionada en la fase 2 como punto de partida para proporcionar seguridad estas funciones como la mayoría ICS consolas (incluidos estas normas comunes como SCADA y NCP) no requieren navegar por Internet abierta y comprobar el correo electrónico.<br /><br />: Aplicaciones que se usan para controlar la máquina física tendría que integrado y compatibilidad probado y protegido correctamente|
|Sistema operativo|No|Mientras muchos pasos de refuerzo de PATA pueden usarse para sistemas operativos incrustados, una solución personalizada que se ha desarrollado para reforzar en este escenario.|

> [!NOTE]
> **Escenarios de combinación** algunos personal puede tener las responsabilidades administrativas que ocupan varios escenarios.
> En estos casos, las reglas claves a tener en cuenta son que deben cumplirse las reglas de modelo de nivel en todo momento. Consulta la página de modelo de nivel para obtener más información.

> [!NOTE]
> **Ajuste de escala el programa PAW** como el programa PATA escalas para abarcar más administradores y los roles, debes seguir garantizar que se mantiene el cumplimiento de las normas de seguridad y facilidad de uso. Esto puede requerir que actualice su TI admite estructuras o crea nuevas reglas para resolver PATA desafíos específicos, como el proceso de incorporación pata, administración de incidentes, administración de la configuración y los desafíos de recopilación de información a la facilidad de uso de direcciones.  Un ejemplo puede ser que tu organización decide habilitar escenarios de trabajo desde casa para los administradores, que podrían requerir un cambio de escritorio garras a portátil garras - un cambio que puede requerir consideraciones de seguridad adicionales.  Otro ejemplo común es crear o actualizar formación para los nuevos administradores - formación que ahora se debe incluir el contenido sobre el uso adecuado de un PATA (incluidos por qué es importante y qué un PATA es y no).  Para otras consideraciones que deben tratarse como escalar tu programa pata, consulta la fase 2 de las instrucciones.

Esta guía contiene instrucciones detalladas para la configuración de PATA para los escenarios, como se mencionó anteriormente. Si tienes requisitos para los otros escenarios, puedes adaptar las instrucciones que se basa en esta guía o contratar una organización de los servicios profesionales como Microsoft para ayudar con él.

Para obtener más información sobre un atractivo un PATA adaptada para el entorno de diseño de los servicios de Microsoft, póngase en contacto con tu representante de Microsoft o visite [esta página](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).

### <a name="paw-installation-instructions"></a>PATA instrucciones de instalación
Dado que la pata debe proporcionar un origen de confianza y seguro para la administración, es esencial que el proceso de compilación es seguro y de confianza.  En esta sección se proporciona instrucciones detalladas que te permitirá crear tu propios PATA con principios generales y conceptos muy similares a los usados por Microsoft IT y Microsoft en la nube de ingeniería y las organizaciones de administración de servicios.

Las instrucciones se dividen en tres fases que se centran en implementar las mitigaciones más importantes rápidamente y, a continuación, progresivamente aumentar y ampliando el uso de PATA para la empresa.

-   Fase 1: implementación inmediata para los administradores de Active Directory

-   Fase 2 - PATA se extiende a todos los administradores

-   Fase 3: seguridad avanzada PATA

Es importante tener en cuenta que las fases siempre se deben realizar en orden incluso si se planean y se implementa como parte de todo el mismo proyecto.

#### <a name="phase-1---immediate-deployment-for-active-directory-administrators"></a>Fase 1: implementación inmediata para los administradores de Active Directory
Propósito: Proporciona una PATA rápidamente que puede proteger funciones de administración de dominios y bosques local.

Ámbito: Los administradores de nivel 0 como administradores de empresa, los administradores de dominio (para todos los dominios) y los administradores de otros sistemas de identidad autorizado.

La fase 1 se centra en los administradores de tu dominio de Active Directory local, que son muy importantes roles con frecuencia objetivo de los atacantes. Estos sistemas de identidad funcionan en conjunto para proteger estos administradores si los controladores de dominio de Active Directory (DC) se hospedan en los centros de datos local, en Azure infraestructura como servicio (IaaS) o de otro proveedor de IaaS.

Durante esta fase, se crea la estructura de unidad organizativa (OU) de Active Directory administrativa segura para hospedar la estación de trabajo con privilegios de acceso (PATA), así como para implementar la garras a sí mismos.  Esta estructura también incluye las directivas de grupo y los grupos que se requiere para admitir la pata.  Crearás la mayor parte de la estructura con los scripts de PowerShell que están disponibles en [Galería de TechNet](http://aka.ms/pawmedia).

Los scripts crean los siguientes de unidades organizativas y grupos de seguridad:

-   Unidades organizativas (OU)

    -   Seis unidades organizativas de nivel superior nuevo: Admin; Grupos; Servidores de nivel 1. Estaciones de trabajo; Cuentas de usuario; y cuarentena del equipo.  Cada unidad organizativa de nivel superior contendrá un número de unidades organizativas secundarias.

-   Grupos

    -   Seis nuevos con seguridad habilitada globales grupos: mantenimiento de replicación de nivel 0; Mantenimiento de servidor de nivel 1; Operadores de servicios de asistencia al cliente; Mantenimiento de la estación de trabajo; Usuarios PATA; PATA mantenimiento.

También creará un número de objetos de directiva de grupo: configuración PAW - equipo; PAW Configuración - usuario; Se necesita RestrictedAdmin - equipo; PATA restricciones de salida; Restringir el inicio de sesión de la estación de trabajo; Restringir el inicio de sesión del servidor.

La fase 1 incluye los siguientes pasos:

1.  Completar los requisitos previos

    1.  **Asegúrate de que todos los administradores usan cuentas separadas, individuales para actividades de administración y el usuario final** (incluido el correo electrónico, la exploración de Internet, aplicaciones de línea de negocio y otras actividades no administrativas).  Asignar una cuenta de administrador a cada separar personal autorizado de su cuenta de usuario estándar es fundamental para el modelo pata, como solo determinadas cuentas se podrán iniciar sesión en la pata sí.

        > [!NOTE]
        > Cada administrador debe usar su cuenta para la administración.  No comparten una cuenta de administrador.

    2.  **Minimizar el número de administradores de nivel 0 privilegiada**.  Dado que cada administrador debe usar un pata, lo que reduce el número de administradores reduce el número de garras necesarios para ellos y los costos asociados. El recuento más bajo de los administradores también se genera en inferior de la exposición de estos privilegios y los riesgos asociados. Si bien es posible que los administradores en una ubicación compartir un pata, los administradores en ubicaciones físicas separadas requerirá garras independientes.

    3.  **Comprar hardware de un proveedor de confianza que cumpla con todos los requisitos técnicos**. Microsoft recomienda adquirir el hardware que cumpla los requisitos técnicos en el artículo [proteger las credenciales de dominio con Credential Guard](https://technet.microsoft.com/en-us/library/mt483740%28v=vs.85%29.aspx).

        > [!NOTE]
        > PATA instalado en el hardware sin estas funcionalidades puede proporcionar protecciones importantes, pero las características de seguridad avanzada, como Credential Guard y Device Guard no estarán disponibles.  Credential Guard y Device Guard no son necesarios para la implementación de la fase 1, pero se recomiendan como parte de la fase 3 (refuerzo avanzada).

        Asegúrate de que el hardware utilizado para el PATA proceden un fabricante y el proveedor cuyas prácticas de seguridad son de confianza para la organización. Se trata de una aplicación del principio de origen limpio para proporcionar seguridad de la cadena.

        > [!NOTE]
        > Para obtener más información sobre la importancia de seguridad de la cadena de suministro, visite [este sitio](https://www.microsoft.com/security/cybersecurity/).

    4.  Adquirir y validar el software de aplicación y requiere Windows 10 Enterprise Edition. Obtener el software necesario para PATA y validar mediante las instrucciones de [origen limpio medios de instalación](http://aka.ms/cleansource).

        -   Windows 10 Enterprise Edition

        -   [Herramientas de administración remota del servidor](https://www.microsoft.com/en-us/download/details.aspx?id=45520.) para Windows 10

        -   Windows 10 [líneas de base de seguridad](http://aka.ms/win10baselines)

        -   [Kit de herramientas de mitigación mejorada experiencia (EMET)](https://www.microsoft.com/en-us/download/details.aspx?id=49166)

            > [!NOTE]
            > Microsoft publica hash MD5 para todos los sistemas operativos y aplicaciones en MSDN, pero no todos los proveedores de software proporcionan documentación similar.  En esos casos, otras estrategias será necesarios.  Para obtener más información sobre la validación de software, consulte [origen limpio](http://aka.ms/cleansource) medios de instalación.

    5.  **Asegúrate de que tienes un servidor WSUS disponible en la intranet**. Necesitarás un servidor WSUS en la intranet para descargar e instalar actualizaciones para PATA. Este servidor WSUS debe estar configurado para aprobar automáticamente todas las actualizaciones de seguridad para Windows 10 o debe tener un personal administrativo responsabilidad y responsabilidad rápidamente aprobar las actualizaciones de software.

        > [!NOTE]
        > Para obtener más información, consulta la sección "Automáticamente aprobar actualizaciones para la instalación" en la [instrucciones de aprobación de actualizaciones](https://technet.microsoft.com/en-us/library/cc708458(v=ws.10).aspx).

2.  Implementar el marco de la unidad organizativa de administrador para hospedar las huellas

    1.  Descargar de la biblioteca de script PATA [Galería de TechNet](http://aka.ms/PAWmedia)

        > [!NOTE]
        > Descargar todos los archivos y guardarlos en el mismo directorio y ejecutarlas en el siguiente orden.  Create-PAWGroups depende de la estructura de unidad organizativa creada por Create-PAWOUs y Set-PAWOUDelegation depende de los grupos creados por Create-PAWGroups.
        > No modifiques cualquiera de los scripts o el archivo de valores separados por comas (CSV).

    2.  **Ejecutar el script. ps1 Create-PAWOUs**.  Este script creará la nueva estructura de unidad organizativa (OU) en Active Directory y bloquear la herencia de GPO en las nuevas unidades organizativas según corresponda.

    3.  **Ejecutar el script. ps1 Create-PAWGroups**.  Este script crea los nuevos grupos de seguridad global en las unidades organizativas adecuadas.

        > [!NOTE]
        > Mientras este script crea los nuevos grupos de seguridad, le no rellenan automáticamente.

    4.  **Ejecutar el script. ps1 Set-PAWOUDelegation**.  Este script asigne permisos a las nuevas unidades organizativas para los grupos adecuados.

3.  **Mover cuentas de nivel 0a la unidad organizativa 0\Accounts de Admin\Tier**. Mover todas las cuentas que sea miembro del Administrador de dominio, administradores de organización o grupos equivalentes 0 (incluida la pertenencia anidado) a esta unidad organizativa de nivel. Si tu organización tiene sus propios grupos que se agregan a estos grupos, debe moverse a la unidad organizativa 0\Groups de Admin\Tier.

    > [!NOTE]
    > Para obtener más información en el que grupos de nivel 0, consulta "Equivalencia de nivel 0" en [proteger el Material de referencia de acceso con privilegios](../securing-privileged-access/securing-privileged-access-reference-material.md).

4.  Agregar a los miembros correspondientes a los grupos relevantes

    1.  **Los usuarios PATA** : agregar los administradores de nivel 0 con el dominio o grupos de administrador de la empresa que has identificado en el paso 1 de la fase 1.

    2.  **PATA mantenimiento** : agregar al menos una cuenta que se usará para el mantenimiento de PATA y solucionar problemas de tareas. Las cuentas de mantenimiento PAW usará sólo en raras ocasiones.

        > [!NOTE]
        > No agregues la misma cuenta de usuario o grupo a los usuarios PAW y PAW mantenimiento.  El modelo de seguridad PATA se basa en la parte de la suposición de que la cuenta de usuario PATA ha privilegiadas derechos en sistemas administrados o a través de la pata Sí, pero no ambas.
        >
        > -   Esto es importante para la creación de buenas prácticas administrativas y hábitos en la fase 1.
        > -   Esto es crucial para la fase 2 y mucho más, para evitar la escalación de privilegios a través de PATA como garras que abarque niveles.
        >
        > Lo ideal es personal no se asigna a los derechos en varios niveles para aplicar el principio de separación de funciones, pero Microsoft reconoce que muchas organizaciones pueden limitada personal (u otros requisitos de la organización) que no permiten esta separación completa. En estos casos, el mismo personal puede asignarse a ambos roles, pero no debes usar la misma cuenta para estas funciones.

5.  **Crear el objeto de directiva de grupo (GPO) de "PAW configuración – equipo" y vínculo a la unidad organizativa de dispositivos de nivel 0** ("dispositivos" en el nivel 0\Admin).  En esta sección, vas a crear un nuevo "PAW configuración – equipo" GPO que proporcionan protecciones específicas para estos garras.

    > [!NOTE]
    > **No agregues estas opciones de configuración a la directiva de dominio predeterminada**.  Si lo haces, posiblemente afectará a las operaciones en todo el entorno de Active Directory.  Solo configurar estas opciones de configuración en los GPO recién creado se describen aquí y solo se aplican a la unidad organizativa PAW.

    1.  **PATA mantenimiento acceso** : esta opción establece la pertenencia a grupos específicos de privilegios en la garras a un conjunto específico de usuarios. Ve a *los usuarios del equipo Configuration\Preferences\Control Panel seguridad\Directivas* y grupos y sigue los pasos siguientes:

        1.  Haz clic en **nueva** y haz clic en **grupo Local**

        2.  Selecciona el **actualización** acción y seleccionados "administradores (integrado)" (no usar el botón Examinar para seleccionar el grupo Administradores de dominio).

        3.  Selecciona el **eliminar todos los usuarios miembro** y **eliminar todos los grupos de miembro** casillas de verificación

        4.  Agregar PAW mantenimiento (pawmaint) y el administrador (de nuevo, no uses el botón Examinar para seleccionar Administrador).

            > [!NOTE]
            > No agregues el grupo usuarios PAW a la lista de miembros para el grupo de administradores local.  Para garantizar que los usuarios PAW no se deliberadamente o por accidente modificar la configuración de seguridad de la pata Sí, no deben ser miembros del grupo de administradores local.

            Para obtener más información sobre cómo usar las preferencias de directiva de grupo para modificar la pertenencia al grupo, consulte el artículo de TechNet [configurar un elemento de grupo Local](https://technet.microsoft.com/en-us/library/cc732525.aspx).

    2.  **Restringir la pertenencia al grupo Local** -esta configuración se asegurará de que la pertenencia a grupos de administrador local en la estación de trabajo siempre está vacía

        1.  Ve al equipo Configuration\Preferences\Control Panel seguridad\Directivas usuarios y grupos y sigue los pasos siguientes:

            1.  Haz clic en **nueva** y haz clic en **grupo Local**

            2.  Selecciona el **actualización** acción y selecciona "operadores de copia (integrado)" (no usar el botón Examinar para seleccionar el grupo Operadores de copia de seguridad de dominio).

            3.  Selecciona el **eliminar todos los usuarios miembro** y **eliminar todos los grupos de miembro** casillas de verificación.

            4.  No agregues a todos los miembros del grupo.  Simplemente haz clic en **Aceptar**.  Al asignar una lista vacía, directiva de grupo automáticamente quitar a todos los miembros y garantizar una lista de miembros en blanco que se actualiza cada directiva de grupo de tiempo.

        2.  Completar los pasos anteriores para los siguientes grupos:

            -   Operadores criptográficos

            -   Administradores de Hyper-V

            -   Operadores de configuración de red

            -   Usuarios avanzados

            -   Usuarios de escritorio remoto

            -   Replicadores

        3.  Restricciones de inicio de sesión de PATA: esta configuración limitará las cuentas que pueden iniciar sesión en la pata. Sigue estos pasos para configurar esta configuración:

            1.  Ve al equipo equipo\Directivas\Configuración Windows\Configuración seguridad\Directivas locales\Asignación derechos Assignment\Allow inicio de sesión localmente.

            2.  Selecciona Definir esta configuración de directiva y agrega "Usuarios PAW" y administradores (de nuevo, no uses el botón Examinar para seleccionar los administradores).

        4.  **Bloquear el tráfico de red entrantes** -esta opción garantizará que no se permite la pata ningún tráfico de red entrantes no solicitadas. Sigue estos pasos para configurar esta configuración:

            1.  Ve al equipo equipo\Directivas\Configuración Windows\Configuración de Seguridad\firewall con seguridad con seguridad avanzada y sigue los pasos siguientes:

                1.  Haga clic con el botón secundario en el Firewall de Windows con seguridad avanzada y selecciona **Importar directiva**.

                2.  Haz clic en **Sí** para aceptar Esto sobrescribirá cualquier directivas de firewall existentes.

                3.  Busca PAWFirewall.wfw y selecciona **abrir**.

                4.  Haz clic en **Aceptar**.

                    > [!NOTE]
                    > Puede agregar direcciones o subredes que deben alcanzar el PATA con tráfico no solicitado en este momento (por ejemplo, software de seguridad análisis o de administración.
                    > La configuración en el archivo WFW habilitar el firewall en el modo "Bloque –" predeterminado"para todos los perfiles de firewall, desactivar la combinación de regla y habilitar el registro de paquetes colocados y correctos. Estas opciones de configuración bloquean el tráfico unsolicitied mientras se permite la comunicación bidireccional en conexiones iniciadas desde la pata, impedir que los usuarios con acceso administrativo local crear reglas de firewall local que invalidar la configuración del GPO y garantizar que se registra el tráfico dentro y fuera de la pata.
                    > **Abrir firewall de este se expande la superficie de ataque para el PATA y aumenta el riesgo de seguridad. Antes de agregar las direcciones, consulte la sección de administración y operativo PAW en esta guía**.

        5.  **Configurar Windows Update para WSUS** -sigue estos pasos para cambiar la configuración para configurar Windows Update para las huellas:

            1.  Ve al equipo equipo\Directivas\Plantillas administrativas\Componentes ofrecidas actualizaciones y sigue los pasos siguientes:

                1.  Habilitar la **directiva Configurar actualizaciones automáticas**.

                2.  Selecciona la opción **4 - Descargar automáticamente y programar la instalación**.

                3.  Cambia la opción **día de instalación programado** a **0 - todos los días** y la opción **hora de instalación programado** a sus preferencias de la organización.

                4.  Habilitar la opción **especificar la ubicación del servicio de actualización de Microsoft de intranet** directiva, y especifica en ambas opciones de la dirección URL del servidor WSUS ESAE.

        6.  Vincular la "PAW configuración – equipo" GPO como sigue:

            |Directiva|Ubicación del vínculo|
            |-----|---------|
            |PATA configuración - equipo |Admin\Tier 0\Devices|

6.  **Crear el objeto de directiva de grupo (GPO) de "PAW configuración – User" y vínculo a la unidad organizativa cuentas de nivel 0 ("cuentas" en el nivel 0\Admin)**.  En esta sección, vas a crear un nuevo "PAW Configuración – usuario" GPO que proporcionan protecciones específicas para estos garras.

    > [!NOTE]
    > No agregues estas opciones de configuración a la directiva de dominio predeterminada

    1.  **Bloquear la exploración de internet** - disuadir involuntaria la exploración de internet, se establecerá una dirección de proxy de una dirección de bucle invertido (127.0.0.1).

        1.  Ve a seguridad\Registro equipo\preferencias\configuración de usuario. Haz clic en el registro, seleccione **nueva** \> **elemento del registro** y configura las opciones siguientes:

            1.  Acción: reemplazar

            2.  Sección: HKEY_CURRENT_USER

            3.  Ruta de la clave: Configuración de Software\Microsoft\Windows\CurrentVersion\Internet

            4.  Nombre del valor: ProxyEnable

                > [!NOTE]
                > No se active la casilla de forma predeterminada a la izquierda del nombre del valor.

            5.  Tipo de valor: REG_DWORD

            6.  Información del valor: 1

                1.  Un archivo.  Haz clic en la ficha común y selecciona **quitar este elemento cuando ya no se aplica**.

                2.  En la pestaña comunes selecciona **destinados a nivel de elemento** y haz clic en **administración del objetivo**.

                3.  Haz clic en **nuevo elemento** y selecciona **grupo de seguridad**.

                4.  Selecciona el botón "..." y busca el grupo usuarios PAW.

                5.  Haz clic en **nuevo elemento** y selecciona **grupo de seguridad**.

                6.  Selecciona el botón "..." y busca el **administradores de servicios de nube** grupo.

                7.  Haz clic en el **administradores de servicios de nube** de elemento y haz clic en **opciones artículos**.

                8.  Selecciona **no es**.

                9. Haz clic en **Aceptar** en la ventana de selección de destino.

            7.  Haz clic en **Aceptar** para completar la configuración de directiva de grupo ProxyServer

        2.  Ve a seguridad\Registro equipo\preferencias\configuración de usuario. Haz clic en el registro, seleccione **nueva** \> **elemento del registro** y configura las opciones siguientes:

            -   Acción: reemplazar

            -   Sección: HKEY_CURRENT_USER

            -   Ruta de la clave: Configuración de Software\Microsoft\Windows\CurrentVersion\Internet

            -   Nombre del valor: ProxyServer

                > [!NOTE]
                > No se active la casilla de forma predeterminada a la izquierda del nombre del valor.

            -   Tipo de valor: REG_SZ

            -   Información del valor: 127.0.0.1:80

                1.  Haz clic en el **comunes** pestaña y selecciona **quitar este elemento cuando ya no se aplica**.

                2.  En la **comunes** ficha seleccione **destinados a nivel de elemento** y haz clic en **administración del objetivo**.

                3.  Haz clic en **nuevo elemento** y grupo de seguridad selecciona.

                4.  Selecciona el botón "..." y agrega el grupo usuarios PAW.

                5.  Haz clic en **nuevo elemento** y grupo de seguridad selecciona.

                6.  Selecciona el botón "..." y busca el **administradores de servicios de nube** grupo.

                7.  Haz clic en el **administradores de servicios de nube** de elemento y haz clic en **opciones artículos**.

                8.  Selecciona **no es**.

                9. Haz clic en **Aceptar** en la ventana de selección de destino.

        3.  Haz clic en **Aceptar** para completar la configuración de directiva de grupo ProxyServer

    2.  Ve al usuario equipo\Directivas\Plantillas administrativas\Componentes de Windows\Internet Explorer y habilitar las opciones siguientes. Estas opciones de configuración impedirá que los administradores reemplazando manualmente la configuración de proxy.

        1.  Habilitar la **deshabilitar el cambio de configuración automática** configuración.

        2.  Habilitar la **impedir el cambio de configuración de proxy**.

7.  **Restringir los administradores de hosts de nivel inferior de inicio de sesión**.  En esta sección, configuramos las directivas de grupo para evitar que las cuentas con privilegios administrativas hosts de nivel inferior de inicio de sesión.

    1.  Crear la nueva **restringir el inicio de sesión de estación de trabajo** GPO - esta configuración restringirá nivel 0 y cuentas de administrador de nivel 1 del inicio de sesión en estaciones de trabajo estándar.  Este GPO debe estar vinculado a la unidad organizativa de nivel superior de "Estaciones de trabajo" y tener las siguientes opciones:

        - (i) En el equipo equipo\Directivas\Configuración Windows\Configuración seguridad\Directivas locales\Asignación derechos Assignment\Deny inicio de sesión como trabajo por lotes, selecciona **definir esta configuración de directiva** y agrega los grupos de nivel 1 y de nivel 0:

            **Grupos para agregar a la configuración de directiva:**

            Administradores de empresa

            Administradores de dominio

            Administradores de esquema

            DOMAIN\Administrators

            Operadores de cuentas

            Operadores de copia de seguridad

            Operadores de impresión

            Operadores de servidor

            Controladores de dominio

            Read-Only Controladores de dominio

            Propietarios de creadores de directivas de grupo

            Operadores criptográficos

            > [!NOTE]
            > Nota: Los grupos de nivel 0 integrados, consulte equivalencia de nivel 0 para obtener más detalles.

            Otros delegado grupos

            > [!NOTE]
            > Nota: Cualquier crea grupos con el acceso de nivel 0 eficaz, de forma personalizada consulta equivalencia de nivel 0 para obtener más detalles.

            Administradores de nivel 1

            > [!NOTE]
            > Nota: Este grupo creada anteriormente en la fase 1

        - (ii) En el equipo equipo\Directivas\Configuración Windows\Configuración seguridad\Directivas locales\Asignación derechos Assignment\Deny el inicio de sesión como servicio, selecciona **definir esta configuración de directiva** y agrega los grupos de nivel 1 y de nivel 0:

            **Grupos para agregar a la configuración de directiva:**

            Administradores de empresa

            Administradores de dominio

            Administradores de esquema

            DOMAIN\Administrators

            Operadores de cuentas

            Operadores de copia de seguridad

            Operadores de impresión

            Operadores de servidor

            Controladores de dominio

            Read-Only Controladores de dominio

            Propietarios de creadores de directivas de grupo

            Operadores criptográficos

            > [!NOTE]
            > Nota: Los grupos de nivel 0 integrados, consulte equivalencia de nivel 0 para obtener más detalles.

            Otros delegado grupos

            > [!NOTE]
            > Nota: Cualquier crea grupos con el acceso de nivel 0 eficaz, de forma personalizada consulta equivalencia de nivel 0 para obtener más detalles.

            Administradores de segmento 1

            > [!NOTE]
            > Nota: Este grupo creada anteriormente en la fase 1

    2.  Crear la nueva **restringir el inicio de sesión de servidor** GPO - esta configuración restringirá cuentas de administrador de nivel 0 del inicio de sesión en los servidores de nivel 1.  Este GPO debe estar vinculado a la unidad organizativa de nivel superior de "Servidores de nivel 1" y tener las siguientes opciones:

        -   (i) En el equipo equipo\Directivas\Configuración Windows\Configuración seguridad\Directivas locales\Asignación derechos Assignment\Deny inicio de sesión como trabajo por lotes, selecciona **definir esta configuración de directiva** y agregar los grupos de nivel 0:

            **Grupos para agregar a la configuración de directiva:**

            Administradores de empresa

            Administradores de dominio

            Administradores de esquema

            DOMAIN\Administrators

            Operadores de cuentas

            Operadores de copia de seguridad

            Operadores de impresión

            Operadores de servidor

            Controladores de dominio

            Read-Only Controladores de dominio

            Propietarios de creadores de directivas de grupo

            Operadores criptográficos

            > [!NOTE]
            > Nota: Los grupos de nivel 0 integrados, consulte equivalencia de nivel 0 para obtener más detalles.

            Otros delegado grupos

            > [!NOTE]
            > Nota: Cualquier crea grupos con el acceso de nivel 0 eficaz, de forma personalizada consulta equivalencia de nivel 0 para obtener más detalles.

        -   (ii) En el equipo equipo\Directivas\Configuración Windows\Configuración seguridad\Directivas locales\Asignación derechos Assignment\Deny el inicio de sesión como servicio, selecciona **definir esta configuración de directiva** y agregar los grupos de nivel 0:

            **Grupos para agregar a la configuración de directiva:**

            Administradores de empresa

            Administradores de dominio

            Administradores de esquema

            DOMAIN\Administrators

            Operadores de cuentas

            Operadores de copia de seguridad

            Operadores de impresión

            Operadores de servidor

            Controladores de dominio

            Read-Only Controladores de dominio

            Propietarios de creadores de directivas de grupo

            Operadores criptográficos

            > [!NOTE]
            > Nota: Los grupos de nivel 0 integrados, consulte equivalencia de nivel 0 para obtener más detalles.

            Otros delegado grupos

            > [!NOTE]
            > Nota: Cualquier crea grupos con el acceso de nivel 0 eficaz, de forma personalizada consulta equivalencia de nivel 0 para obtener más detalles.

        -   (iii) En el equipo equipo\Directivas\Configuración Windows\Configuración seguridad\Directivas locales\Asignación derechos Assignment\Deny inicio de sesión localmente, selecciona **definir esta configuración de directiva** y agregar los grupos de nivel 0:

            **Grupos para agregar a la configuración de directiva:**

            Administradores de empresa

            Administradores de dominio

            Administradores de esquema

            Operadores de cuentas

            Operadores de copia de seguridad

            Operadores de impresión

            Operadores de servidor

            Controladores de dominio

            Read-Only Controladores de dominio

            Propietarios de creadores de directivas de grupo

            Operadores criptográficos

            > [!NOTE]
            > Nota: Los grupos de nivel 0 integrados, consulte equivalencia de nivel 0 para obtener más detalles.

            Otros delegado grupos

            > [!NOTE]
            > Nota: Cualquier crea grupos con el acceso de nivel 0 eficaz, de forma personalizada consulta equivalencia de nivel 0 para obtener más detalles.

8.  Implementar las garras

    > [!NOTE]
    > Asegúrate de que la pata se desconecta de la red durante el proceso de compilación del sistema operativo.

    1.  Instalar Windows 10 con los medios de instalación limpia de origen que obtuviste anteriormente.

        > [!NOTE]
        > Puede usar Microsoft Deployment Toolkit (MDT) o en otro sistema de implementación automatizada de imágenes para automatizar la implementación de pata, pero Asegúrate de que el proceso de compilación es de tanta confianza como la pata. Adversarios específicamente buscan imágenes corporativas y sistemas de implementación (incluidas las imágenes ISO, paquetes de implementación, etc.) como un mecanismo de persistencia para que preexistente sistemas de implementación o imágenes no deben usarse.
        >
        > Si vas a automatizar la implementación de la pata, debes:
        >
        > -   Crear el sistema con medios de instalación validados con las instrucciones en [origen limpio medios de instalación](http://aka.ms/cleansource).
        > -   Asegúrate de que el sistema de implementación automatizada se desconecta de la red durante el proceso de compilación del sistema operativo.

    2.  Establecer una contraseña compleja única para la cuenta de administrador local.  No uses una contraseña que se ha utilizado para cualquier otra cuenta en el entorno.

        > [!NOTE]
        > Microsoft recomienda usar [solución de contraseña de administrador Local (PARCIALES)](https://www.microsoft.com/en-us/download/details.aspx?id=46899) para administrar la contraseña de administrador local para todas las estaciones de trabajo, incluidos garras.  Si usas PARCIALES, asegúrate de que solo concede al grupo de mantenimiento PAW el derecho a leer administrado VUELTAS contraseñas para las huellas.

    3.  Instalar remoto Server Administration Tools para Windows 10 con los medios de instalación limpia de origen.

    4.  Instalar mejorado mitigación experiencia Kit de herramientas (EMET) con los medios de instalación limpia de origen.

        > [!NOTE]
        > Ten en cuenta que, en el momento de la última actualización de esta guía, EMET 5.5 todavía se estaba pruebas beta.  Dado que es la primera versión compatible con Windows 10, se incluye aquí a pesar de su estado de la versión beta.  Si la instalación de software beta en la pata supera la tolerancia de riesgos, puede omitir este paso por ahora.

    5.  Conecta el PATA a la red.  Asegúrate de que la pata pueda conectarse al menos un controlador de dominio (DC).

    6.  Con una cuenta que es un miembro del grupo PAW mantenimiento, ejecutarlo el siguiente comando de PowerShell desde la pata recién creado para unirte a ella en el dominio en la unidad organizativa apropiada:

        `Add-Computer -DomainName Fabrikam -OUPath "OU=Devices,OU=Tier 0,OU=Admin,DC=fabrikam,DC=com"`

        Reemplaza las referencias a *Fabrikam* con tu nombre de dominio, según corresponda.  Si el nombre de dominio se extiende a varios niveles (por ejemplo, child.fabrikam.com), agrega los nombres adicionales con los "DC =" identificador en el orden en que aparecen en el nombre de dominio completo del dominio.

        > [!NOTE]
        > Si ha implementado una [ESAE administrativo bosque](http://aka.ms/esae) (para los administradores de nivel 0 en la fase 1) o un [Microsoft Identity Manager (MIM) con privilegios de administración de acceso (PAM)](http://aka.ms/mimpamdeploy) (en nivel 1 y 2 Administradores en la fase 2), entrarían la pata al dominio en ese entorno aquí en lugar del dominio de producción.

    7.  Aplicar todas las actualizaciones críticas e importantes a Windows antes de instalar otro software (incluidas las herramientas administrativas, agentes, etcetera.).

    8.  Hacer que la aplicación de la directiva de grupo.

        1.  Abre un símbolo del sistema con privilegios elevados y escribe el siguiente comando:

            `Gpupdate /force /sync`

        2.  Reinicia el equipo

    9. **(Opcional) **Instalar otras herramientas necesarias para administradores de Active Directory. Instalar otras herramientas ni scripts necesarios para realizar los deberes. Asegúrate de para evaluar el riesgo de exposición de credenciales en los equipos de destino con cualquier herramienta antes de agregarlo a un PATA. Acceso [esta página](http://aka.ms/logontypes) para obtener más información acerca de cómo evaluar herramientas administrativas y métodos de conexión para el riesgo de exposición de credenciales. Asegúrate de para obtener todos los medios de instalación mediante las instrucciones en [origen limpio medios de instalación](http://aka.ms/cleansource).

        > [!NOTE]
        > Usar un servidor de accesos directos para una ubicación central para estas herramientas puede reducir la complejidad, incluso si no sirva de un límite de seguridad.

    10. **(Opcional) **Descargar e instalar el software necesario acceso remoto. Si los administradores a usar la pata remotamente para la administración, instala el software de acceso remoto con la orientación de seguridad de su proveedor de soluciones de acceso remoto. Asegúrate de para obtener todos los medios de instalación con las instrucciones de origen limpio de medios de instalación.

        > [!NOTE]
        > Analiza detenidamente todos los riesgos que implica permitir el acceso remoto a través de un PATA.  Mientras un PATA móvil permite muchos escenarios importantes, incluidos el trabajo desde casa, software de acceso remoto puede ser vulnerable a ataques y usarse para poner en peligro un PATA.

    11. Validar la integridad del sistema PATA revisa y confirmar que se toda la configuración apropiada en su lugar mediante los pasos siguientes:

        1.  Confirma que solo las directivas de grupo específica PATA se aplican a la pata

            1.  Abre un símbolo del sistema con privilegios elevados y escribe el siguiente comando:

                `Gpresult /scope computer /r`

            2.  Revisa el listado resultante y comprueba que el grupo solo las directivas que aparecen son las que ha creado anteriormente.

        2.  Confirmar que no hay cuentas de usuario adicionales son miembros del grupo con privilegios en la pata siguiendo los pasos siguientes:

            1.  Abre **editar usuarios y grupos locales** (lusrmgr.msc), selecciona **grupos**y confirme que los miembros del grupo de administradores local solo son la cuenta de administrador local y el grupo de seguridad global PAW mantenimiento.

                > [!NOTE]
                > El grupo usuarios PAW no debe ser miembro del grupo de administradores local.  Los miembros solo deben ser la cuenta de administrador local y el grupo de seguridad global de mantenimiento PAW (y los usuarios PAW no debe ser un miembro de ese grupo global o bien).

            2.  También usan **editar usuarios y grupos locales**, asegúrate de que los siguientes grupos no tienen miembros:

                -   Operadores de copia de seguridad

                -   Operadores criptográficos

                -   Administradores de Hyper-V

                -   Operadores de configuración de red

                -   Usuarios avanzados

                -   Usuarios de escritorio remoto

                -   Replicadores

    12. **(Opcional) **Si tu organización usa una solución de administración (SIEM) de eventos y la información de seguridad, asegúrate de que es la pata [configurado para reenviar eventos en el sistema mediante el reenvío de eventos de Windows (WEF)](http://blogs.technet.com/b/jepayne/archive/2015/11/24/monitoring-what-matters-windows-event-forwarding-for-everyone-even-if-you-already-have-a-siem.aspx) o de lo contrario, se registra con la solución para que la SIEM activamente está recibiendo eventos e información de la pata.  Los detalles de esta operación variarán en función de la solución SIEM.

        > [!NOTE]
        > Si tu SIEM requiere a un agente que se ejecuta como sistema o una cuenta de administrador local en el garras, asegúrate de que la SIEMs se administran con el mismo nivel de confianza que los sistemas de identidad y controladores de dominio.

    13. **(Opcional) **Si ha decidido implementar VUELTAS para administrar la contraseña de la cuenta de administrador local en su pata, comprueba que la contraseña está registrada correctamente.

        -   Con una cuenta con permisos para leer las contraseñas administradas PARCIALES, abre **equipos y usuarios de Active Directory** (dsa.msc).  Asegúrate de que se habilita características avanzadas y, a continuación, haz clic en el objeto de equipo correspondiente.  Selecciona la pestaña del Editor de atributos y confirma que el valor de msSVSadmPwd se rellena con una contraseña válida.

#### <a name="phase-2---extend-paw-to-all-administrators"></a>Fase 2 - PATA se extiende a todos los administradores
Ámbito: Todos a los usuarios con derechos administrativos sobre aplicaciones críticas y de dependencias.  Esto debería incluir al menos los administradores de servidores de la aplicación, estado operativa y supervisión de dispositivos de red, las soluciones de virtualización, sistemas de almacenamiento y soluciones de seguridad.

> [!NOTE]
> Las instrucciones de esta fase, se supone que se ha completado la fase 1 en su totalidad.  No comience la fase 2 hasta que haya completado todos los pasos en la fase 1.

Después de confirmar que se realizaron todos los pasos, realiza los siguientes pasos para completar la fase 2:

1.  **(Recomendado) **Habilitar **RestrictedAdmin** modo - habilitar esta característica en tus servidores existentes y estaciones de trabajo, a continuación, aplicar el uso de esta característica. Esta característica requiere los servidores de destino que se esté ejecutando Windows Server 2008 R2 o posterior y seleccionar como destino las estaciones de trabajo que se esté ejecutando Windows 7 o posterior.

    1.  Habilitar **RestrictedAdmin** modo en los servidores y estaciones de trabajo siguiendo las instrucciones disponibles en este [página](http://aka.ms/rdpra).

        > [!NOTE]
        > Antes de habilitar esta característica frontal para servidores de internet, debe tener en cuenta el riesgo de adversarios poder autenticar a esos servidores con un hash de contraseña robado anteriormente.

    2.  Crear el objeto de directiva de grupo (GPO) de "RestrictedAdmin necesario – equipo". En esta sección crea un GPO que se aplica el uso de la /RestrictedAdmin cambiar para las conexiones de escritorio remoto salientes, protege cuentas contra el robo de credenciales en los sistemas de destino

        -   Ve al equipo equipo\Directivas\Plantillas Templates\System\Credentials Delegation\Restrict delegación de credenciales hacia servidores remotos y establece como **habilitado**.

    3.  Vínculo la **RestrictedAdmin** requerido - equipo hasta el nivel 1 o de nivel 2 dispositivos mediante el uso de las siguientes opciones de directiva apropiada:

        PATA configuración - equipo

        -> Ubicación vínculo: Admin\Tier 0\Devices (existente)

        PAW usuario de configuración:

        -> Ubicación vínculo: Admin\Tier 0\Accounts

        RestrictedAdmin requerido - equipo

        -> Admin\Tier1\Devices o -> Admin\Tier2\Devices (ambos son opcionales)

        > [!NOTE]
        > Esto no es necesario para sistemas de nivel 0 como estos sistemas ya están en un control total de todos los activos en el entorno.

2.  Mover los objetos de nivel 1a las unidades organizativas adecuadas.

    1.  Mover grupos de nivel 1 para la Admin\Tier 1\Groups UO. Busca todos los grupos que concesión los siguientes derechos administrativos y moverlos a esta unidad organizativa.

        -   Administrador local en más de un servidor

        -   Acceso administrativo a los servicios en la nube

        -   Acceso administrativo a las aplicaciones de empresa

    2.  Mover cuentas de nivel 1a la unidad organizativa 1\Accounts de Admin\Tier. Mueve cada cuenta que es un miembro de esos grupos de nivel 1 (incluida la pertenencia anidado) a esta unidad organizativa.

3.  Agregar a los miembros correspondientes a los grupos relevantes

    -   **Nivel 1 administradores** -este grupo contendrá los administradores de nivel 1 que no se puede iniciar sesión en los hosts de nivel 2. Agregar todos los grupos administrativos de nivel 1 que tengan privilegios administrativos en los servidores o servicios de internet.

        > [!NOTE]
        > Si personal administrativo tiene derechos para administrar activos en varios niveles, deberás crear una cuenta de administrador independiente por nivel.

4.  Habilitar Credential Guard reducir el riesgo de robo de credenciales y volver a usar.  Credential Guard es una nueva característica de Windows 10 que restringe el acceso de la aplicación a las credenciales, y evitar los ataques de robo de credenciales (incluido el Pass-the-Hash).  Credential Guard es totalmente transparente para el usuario final y requiere una configuración mínima tiempo y esfuerzo.  Para obtener más información sobre Credential Guard, incluidos los pasos de implementación y requisitos de hardware, consulte el artículo [proteger las credenciales de dominio con Credential Guard](https://technet.microsoft.com/en-us/library/mt483740%28v=vs.85%29.aspx).

    > [!NOTE]
    > Device Guard debe estar habilitado para poder configurar y usar a Credential Guard.  Sin embargo, no es necesario configurar las otras protecciones de Device Guard para usar a Credential Guard.

5.  **(Opcional) **Permitir la conexión a servicios en la nube. Este paso permite la administración de servicios de nube como Azure y Office 365 con garantías de seguridad adecuado. Este paso también se requiere para Microsoft Intune administrar las huellas.

    > [!NOTE]
    > Omitir este paso si no hay conectividad en la nube se requiere para la administración de servicios en la nube o administración de Intune.

    Estos pasos se restringir la comunicación a través de internet para únicamente los servicios de nube autorizados (pero no abre internet) y agrega protección a los exploradores y otras aplicaciones que se procesan el contenido de internet. Estos garras para la administración nunca deben usarse para realizar tareas de usuario estándar como comunicaciones de internet y productividad.

    Para habilitar la conectividad a PATA servicios sigan los pasos siguientes:

    1.  Configurar PATA para permitir que solo los destinos de Internet autorizados.  Como ampliar la implementación de PATA para habilitar la administración de la nube, deberás permitir el acceso a los servicios autorizados filtrando los acceso desde internet abiertas donde más fácilmente posible montar ataques contra los administradores.

        1.  Crear **administradores de servicios de nube** de grupo y agregar todas las cuentas que necesitan acceder a servicios en la nube en internet.

        2.  Descargar el PATA *proxy.pac* del archivo de [Galería de TechNet](http://aka.ms/pawmedia) y lo publica en un sitio Web interno.

            > [!NOTE]
            > Tendrás que actualizar la *proxy.pac* archivo después de descargar para asegurarse de que esté actualizado y completo.  
            > Microsoft publica todos los actuales de Office 365 y Azure las direcciones URL en la oficina [centro de soporte técnico](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US).
            >
            > Debes agregar otros destinos de Internet válidas para agregar a esta lista para otro proveedor de IaaS, pero no agregar productividad, entretenimiento, noticias o sitios a la lista de búsqueda.
            >
            > También necesitarás ajustar el archivo PAC para dar cabida a una dirección de proxy válida que se usará para estas direcciones.

            > [!NOTE]
            > También puede restringir el acceso desde el PATA con un servidor proxy web también defensa en profundidad. No recomendamos usar esto por sí sola sin el archivo PAC como solo restringirá acceso para garras mientras están conectados a la red corporativa.

            Estas instrucciones se supone que vas a usar Internet Explorer (o en Microsoft Edge) para la administración de Office 365, Azure y otros servicios de nube. Microsoft recomienda restricciones similar para los exploradores de terceros 3 que necesite para la administración de configuración. Los exploradores Web en garras solo deben usarse para la administración de servicios en la nube y nunca para la exploración web general.

        3.  Una vez que hayas configurado el *proxy.pac* de archivos, actualizar la configuración PAW: GPO de usuario.

            1.  Ve a seguridad\Registro equipo\preferencias\configuración de usuario. Haz clic en el registro, seleccione **nueva** \> **elemento del registro** y configura las opciones siguientes:

                1.  Acción: reemplazar

                2.  Subárbol: HKEY_ CURRENT_USER

                3.  Ruta de la clave: Configuración de Software\Microsoft\Windows\CurrentVersion\Internet

                4.  Nombre del valor: AutoConfigUrl

                    > [!NOTE]
                    > No actives el **predeterminado** cuadro a la izquierda del nombre del valor.

                5.  Tipo de valor: REG_SZ

                6.  Información del valor: escribe la dirección URL completa la *proxy.pac* archivo, incluido http:// y el nombre del archivo: por ejemplo http://proxy.fabrikam.com/proxy.pac.  La dirección URL también puede ser una dirección URL única etiqueta - por ejemplo, http://proxy/proxy.pac

                    > [!NOTE]
                    > El archivo PAC también pueden hospedarse en un recurso compartido de archivos, con la sintaxis de file://server.fabrikan.com/share/proxy.pac pero para ello deberás permitir el protocolo file://. Consulta la sección "Nota: File://-based Proxy Scripts en desuso" de este [configuración de Proxy Web de descripción](http://blogs.msdn.com/b/ieinternals/archive/2013/10/11/web-proxy-configuration-and-ie11-changes.aspx) blog para obtener más detalles sobre cómo configurar el valor del registro necesarias.

                7.  Haz clic en el **comunes** pestaña y selecciona **quitar este elemento cuando ya no se aplica**.

                8.  En la **comunes** ficha seleccione **destinados a nivel de elemento** y haz clic en **administración del objetivo**.

                9. Haz clic en **nuevo elemento** y selecciona **grupo de seguridad**.

                10. Selecciona el botón "..." y busca el **administradores de servicios de nube** grupo.

                11. Haz clic en **nuevo elemento** y selecciona **grupo de seguridad**.

                12. Selecciona el botón "..." y busca el **PAW usuarios** grupo.

                13. Haz clic en el **PAW usuarios** de elemento y haz clic en **opciones artículos**.

                14. Selecciona **no es**.

                15. Haz clic en **Aceptar** en la ventana de selección de destino.

                16. Haz clic en **Aceptar** para completar la **AutoConfigUrl** configuración de directiva de grupo.

    2.  Líneas de base de seguridad de Windows 10 y el vínculo de acceso del servicio de nube acceder las líneas de base de seguridad para Windows y servicio de nube (si procede) se aplican a las unidades organizativas correctas con los siguientes pasos:

        1.  Extrae el contenido del archivo ZIP de líneas de base de seguridad de Windows 10.

        2.  Crear estos GPO, [importar la directiva](https://technet.microsoft.com/en-us/library/cc753786.aspx) configuración, y [vínculo](https://technet.microsoft.com/en-us/library/cc732979.aspx) por esta tabla. Vincular cada directiva a cada ubicación y garantizar el orden sigue la tabla (inferior entradas en la tabla deberían ser prioridad aplicada más adelante y posterior):

            **Directivas:**

            |||
            |-|-|
            |Windows 10: seguridad de dominio de CM|N/D: no se vincula ahora|
            |Windows 10 SCM TH2 - equipo|Admin\Tier 0\Devices|
            ||Admin\Tier 1\Devices|
            ||Admin\Tier 2\Devices|
            |Windows 10 de SCM TH2-BitLocker|Admin\Tier 0\Devices|
            ||Admin\Tier 1\Devices|
            ||Admin\Tier 2\Devices|
            |SCM Windows 10 - Credential Guard|Admin\Tier 0\Devices|
            ||Admin\Tier 1\Devices|
            ||Admin\Tier 2\Devices|
            |Internet Explorer SCM - equipo|Admin\Tier 0\Devices|
            ||Admin\Tier 1\Devices|
            ||Admin\Tier 2\Devices|
            |PATA configuración - equipo|Admin\Tier 0\Devices (existente)|
            ||Admin\Tier 1\Devices (nuevo vínculo)|
            ||Admin\Tier 2\Devices (nuevo vínculo)|
            |RestrictedAdmin requerido - equipo|Admin\Tier 0\Devices|
            ||Admin\Tier 1\Devices|
            ||Admin\Tier 2\Devices|
            |SCM Windows 10: el usuario|Admin\Tier 0\Devices|
            ||Admin\Tier 1\Devices|
            ||Admin\Tier 2\Devices|
            |SCM Internet Explorer: usuario|Admin\Tier 0\Devices|
            ||Admin\Tier 1\Devices|
            ||Admin\Tier 2\Devices|
            |PAW usuario de configuración:|Admin\Tier 0\Devices (existente)|
            ||Admin\Tier 1\Devices (nuevo vínculo)|
            ||Admin\Tier 2\Devices (nuevo vínculo)|

            > [!NOTE]
            > La "SCM Windows 10: dominio seguridad" GPO pueden vincularse al dominio independientemente pata, pero afectará a todo el dominio.

6.  **(Opcional) **Instalar otras herramientas necesarias para administradores de nivel 1. Instalar otras herramientas ni scripts necesarios para realizar los deberes. Asegúrate de para evaluar el riesgo de exposición de credenciales en los equipos de destino con cualquier herramienta antes de agregarlo a un PATA. Para obtener más información acerca de cómo evaluar herramientas administrativas y conexión de métodos de riesgo de exposición de credenciales visite [esta página](http://aka.ms/logontypes). Asegúrate de para obtener todos los medios de instalación con las instrucciones de origen limpio de medios de instalación

7.  Identificar y obtener de forma segura software y las aplicaciones necesarias para la administración.  Esto es similar para el trabajo realizado en la fase 1, pero con un ámbito más amplio debido al mayor número de aplicaciones, servicios y sistemas se protege.

    > [!NOTE]
    > Garantizar la protección de estas nuevas aplicaciones (incluidos los exploradores web) Si optas por ellas en las protecciones de EMET.

    Ejemplos de aplicaciones y software adicional:

    -   Microsoft Azure PowerShell

    -   Office 365 PowerShell (también conocido como Microsoft Online Services módulo)

    -   Aplicación o el software de administración de servicio en función de la consola de administración de Microsoft

    -   Aplicación propietaria (no basado en MMC) o software de administración de servicio

        > [!NOTE]
        > Muchas aplicaciones ahora se administran exclusivamente a través de los exploradores web, como muchos de los servicios de nube.  Si bien esto reduce el número de aplicaciones que deben instalarse en un pata, también presenta el riesgo de problemas de interoperabilidad del explorador.  Debes implementar un explorador web no son de Microsoft en instancias específicas de PATA para habilitar la administración de servicios concretos.  Si implementas un explorador web adicional, asegúrate de que cumpla con todos los principios de origen limpio y proteger el explorador según la orientación de seguridad del proveedor.

8.  **(Opcional) **Descargar e instalar los agentes de administración necesarios.

    > [!NOTE]
    > Si decides instalar a agentes de administración adicional (supervisión, seguridad, la administración de la configuración, etc.), es fundamental que te asegures de que los sistemas de administración son de confianza del mismo nivel como controladores de dominio y sistemas de identidad. Consulta la administración y garras de actualización para obtener instrucciones adicionales.

9. Evaluar la infraestructura para identificar sistemas que requieren las protecciones de seguridad adicional proporcionadas por un PATA.  Asegúrate de que sabe exactamente qué sistemas deben protegerse.  Preguntar críticas sobre los recursos de sí mismos, como:

    -   ¿Dónde están los sistemas de destino que se deben administrar?  ¿Recopilan en una sola ubicación física o conectados a una única subred bien definida?

    -   ¿Cuántos sistemas existen?

    -   ¿Estos sistemas dependen de otros sistemas (virtualización, almacenamiento, etc.) y, si es así, ¿cómo se administran los sistemas?  ¿Cómo los sistemas críticos expuestos a estas dependencias y cuáles son los riesgos adicionales asociados con las dependencias?

    -   La importancia son los servicios que administrados y ¿cuál es la pérdida esperada si se pongan en riesgo esos servicios?

        > [!NOTE]
        > Los servicios de nube se incluyen en esta evaluación: atacantes suelen destinar cada vez más las implementaciones de nube no segura y es fundamental que administrar esos servicios con la seguridad como lo haría con las aplicaciones esenciales de locales.

        Usar esta evaluación para identificar los sistemas específicos que requieren una protección adicional y, a continuación, ampliar tu programa PATA a los administradores de esos sistemas.  Comunes sistemas que benefician de administración basada en PATA ejemplos de SQL Server (tanto de forma local y SQL Azure), aplicaciones de recursos humanos y software financiero.

        > [!NOTE]
        > Si un recurso se administra desde un sistema de Windows, pueden administrarse con un pata, incluso si la propia aplicación se ejecuta en un sistema operativo que no sea Windows o en una plataforma de nube no son de Microsoft.  Por ejemplo, el propietario de una suscripción a servicios Web de Amazon solo debes usar un PATA para administrar esa cuenta.

10. Desarrollar un método de solicitud y distribución para la implementación de garras a escala de la organización.  Según el número de huellas que elijas para implementar en la fase 2, necesitarás automatizar el proceso.

    -   Considera la posibilidad de desarrollar un proceso de aprobación para que los administradores usar para obtener un PATA y solicitud formal.  Este proceso podría ayudar a estandarizar el proceso de implementación, garantizar la responsabilidad para dispositivos PATA y ayudar a identificar diferencias en la implementación de PATA.

    -   Como se indicó anteriormente, esta solución de implementación debe ser independiente de los métodos de automatización existentes (que ya haya sufrido) y debe seguir los principios descritos en la fase 1.

        > [!NOTE]
        > Cualquier sistema que administra los recursos deberían administradas en el nivel de confianza igual o superior.

11. Revisa y si es necesario implementar perfiles de hardware PATA adicionales.  El perfil de hardware que has elegido para la implementación de la fase 1 puede no ser adecuado para todos los administradores.  Revisa los perfiles de hardware y si es necesario seleccionar perfiles de hardware de PATA adicionales para satisfacer las necesidades de los administradores.  Por ejemplo, el perfil de Hardware dedicado (PATA y diarias separados usan estaciones de trabajo) no es adecuado para un administrador que viajan con frecuencia: en este caso, puedes elegir implementar el perfil de utilizar simultáneos (PATA con usuario máquina virtual) para el administrador.

12. Ten en cuenta las necesidades de formación que acompañan a una implementación PATA extendida y culturales, operativos, comunicaciones.   Estos cambios significativos a un modelo administrativo naturalmente requerirá administración de cambios hasta cierto punto, y es esencial compilar en el propio proyecto de implementación.  Ten en cuenta como mínimo lo siguiente:

    -   ¿Cómo comunicará senior liderazgo para garantizar su compatibilidad con los cambios?  Cualquier proyecto sin liderazgo senior de respaldo, es probable que produce un error, o en la lucha menos por financiación y de amplia aceptación.

    -   ¿Cómo se documenta el proceso de nuevo para los administradores?  Estos cambios se deben documentados y comunica no solo a los administradores existentes (que deben cambiar sus hábitos y administrar recursos de una forma diferente), sino también para los administradores nuevos (aquellos promueve desde o contratados desde fuera de la organización).  Es esencial que la documentación está desactivada y totalmente quiere transmitir la importancia de las amenazas, rol de PATA para proteger los administradores y cómo usar PATA correctamente.

        > [!NOTE]
        > Esto es especialmente importante para las funciones con rotación constante, incluidos, pero sin carácter limitativo personal de ayuda.

    -   ¿Cómo asegurará cumplimiento con el nuevo proceso?  Si bien el modelo PATA incluye una serie de controles técnicas para evitar la exposición de credenciales con privilegios, resulta imposible impedir por completo toda la exposición posible exclusivamente con controles técnicos.  Por ejemplo, si bien es posible evitar que un administrador inicia sesión correctamente en un escritorio del usuario con credenciales con privilegios, el simple hecho de intentar el inicio de sesión puede exponer las credenciales al malware instalado en ese escritorio del usuario.  Por lo tanto, es fundamental que articular no solo las ventajas del modelo de pata, pero los riesgos de incumplimiento.  Esto debería complementarse la auditoría y alertas para que pueda detecta rápidamente exposición de credenciales y dirigido.

#### <a name="phase-3-extend-and-enhance-protection"></a>La fase 3: Ampliar y mejorar la protección
Ámbito: Estas protecciones mejoran los sistemas basados en la fase 1, respalda notablemente la protección básica con características avanzadas como la autenticación multifactor y reglas de acceso de red.

> [!NOTE]
> Esta fase puede realizarse en cualquier momento una vez completada la fase 1.  No es dependiente de la finalización de la fase 2 y, por consiguiente, puede ser simultáneas, o después de la fase 2, realización antes.

Sigue estos pasos para configurar esta fase:

1.  Habilitar la autenticación multifactor para cuentas con privilegios.  La autenticación multifactor refuerza la seguridad de la cuenta, solicitando al usuario proporcionar un token físico además de credenciales.  La autenticación multifactor complementa las directivas de autenticación muy bien, pero no dependen de las directivas de autenticación para la implementación (y, de forma similar, directivas de autenticación no necesitan la autenticación multifactor).  Microsoft recomienda usar una de estas formas de autenticación multifactor:

    -   **Tarjeta inteligente**: una tarjeta inteligente es un dispositivo físico de manipulaciones y portátil que proporciona una comprobación segundo durante el proceso de inicio de sesión de Windows.  Al exigir una persona para que cuentan con una tarjeta de inicio de sesión, puede reducir el riesgo de credenciales robadas está reutilizando de forma remota.  Para obtener detalles sobre el inicio de sesión de tarjeta inteligente en Windows, consulte el artículo [Introducción a las tarjetas inteligentes](https://technet.microsoft.com/en-us/library/hh831433.aspx).

    -   **Tarjeta inteligente virtual**: una tarjeta inteligente virtual ofrece las mismas ventajas de seguridad físicas como tarjetas inteligentes, con la ventaja de que se vincula específicas del hardware.  Para obtener detalles sobre la implementación y los requisitos de hardware, consulta los artículos, [introducción de tarjeta inteligente Virtual](https://technet.microsoft.com/en-us/library/dn593708.aspx) y [empezar a trabajar con tarjetas inteligentes virtuales: Guía paso a paso](https://technet.microsoft.com/en-us/library/dn579260.aspx).

    -   **Microsoft Passport**: Microsoft Passport permite a los usuarios autenticarse en una cuenta de Microsoft, una cuenta de Active Directory, una cuenta de Microsoft Azure Active Directory (Azure AD) o el servicio de Microsoft que admite la autenticación mediante Fast ID Online (FIDO). Después de una comprobación inicial de dos pasos durante la inscripción de Microsoft Passport, Microsoft Passport está configurado en el dispositivo del usuario y el usuario establece un gesto, que puede ser Windows Hello o un PIN. Credenciales de Microsoft Passport son un par de claves asimétrico, lo que se puede generar en entornos aislados de módulos de plataforma segura (TPM).
        Para obtener más información sobre Microsoft Passport lee [Introducción a Microsoft Passport](https://technet.microsoft.com/en-us/library/dn985839%28v=vs.85%29.aspx) artículo.

    -   **Azure la autenticación multifactor**: Azure la autenticación multifactor (AMF) proporciona la seguridad de un segundo factor de verificación, así como protección mejorada mediante el análisis de máquina basados en el aprendizaje y supervisión.  Puede proteger MFA Azure no solo los administradores de Azure pero muchas otras soluciones, incluidas aplicaciones web, Azure Active Directory y soluciones de local como escritorio remoto y acceso remoto.  Para obtener más información sobre Azure la autenticación multifactor, consulte el artículo [la autenticación multifactor](https://azure.microsoft.com/en-us/services/multi-factor-authentication).

2.  **Aplicaciones con Device Guard y AppLocker de confianza de la lista blanca**.  Al limitar la capacidad de código sin firmar o que no se confía para ejecutarse en un pata, reducir aún más la probabilidad de compromiso y actividades malintencionadas.  Windows incluye dos opciones principales para el control de la aplicación:

    -   **AppLocker**: AppLocker ayuda a los administradores control que se pueden ejecutar aplicaciones en un sistema determinado.  AppLocker forma centralizada se puede controlar mediante Directiva de grupo y aplicar a determinados usuarios o grupos (por aplicación dirigida a los usuarios de garras).  Para obtener más información sobre AppLocker, consulta el artículo de TechNet [Introducción a AppLocker](https://technet.microsoft.com/library/hh831440.aspx).

    -   **Device Guard**: la nueva característica de Device Guard proporciona el control de aplicaciones basada en hardware mejorados que, a diferencia de AppLocker, no se pueden reemplazar en el dispositivo afectado.  Como AppLocker, Device Guard se puede controlar mediante Directiva de grupo y destinada a usuarios específicos.  Para obtener más información sobre la restricción de uso de aplicaciones con Device Guard, consulte el artículo de TechNet [Guía de implementación de dispositivo Guard](https://technet.microsoft.com/en-us/library/mt463091(v=vs.85).aspx).

3.  **Usar los usuarios protegido, las directivas de autenticación y Silos de autenticación para proteger aún más cuentas con privilegios**.  Los miembros de los usuarios protegido están sujetas a las directivas de seguridad adicionales que protección las credenciales almacenados en el agente de seguridad local (LSA) y minimizar en gran medida el riesgo de robo de credenciales y la reutilización.  Directivas de autenticación y silos controlan cómo con privilegios que los usuarios pueden acceder a recursos en el dominio.  De manera colectiva, estas protecciones considerablemente refuerzan la seguridad de la cuenta de estos usuarios con privilegios.  Para obtener más detalles sobre estas características, consulte el artículo web [cómo configurar cuentas protegido](https://technet.microsoft.com/en-us/library/dn518179.aspx).

    > [!NOTE]
    > Estas protecciones están diseñadas para complementar, no se reemplaza, existente medidas de seguridad en la fase 1.  Los administradores aún deben usar cuentas separadas para la administración y de uso general.

### <a name="managing-and-updating-paws"></a>Administrar y actualizar garras
Garras deben tener funcionalidades de antimalware y actualizaciones de software deben aplicarse rápidamente para mantener la integridad de estas estaciones de trabajo.

Administración de configuración adicional, la supervisión operativa y la administración de seguridad también pueden usarse con huellas, pero la integración de estas debe considerarse cuidadosamente porque cada funcionalidad de administración también presenta el riesgo de peligro PATA a través de la herramienta. Si tiene sentido introducir funciones avanzadas de administración depende de una serie de factores como:

-   El estado de seguridad y procedimientos de la funcionalidad de administración (incluidos los procedimientos recomendados de actualización de software para la herramienta, funciones administrativas y las cuentas de esos roles, los sistemas operativos está hospedada en o administrar desde la herramienta y cualquier otra dependencia de hardware o software de herramienta)

-   La frecuencia y la cantidad de implementaciones de software y actualizaciones en tus garras

-   Requisitos para obtener información detallada de inventario y configuración

-   Requisitos de la supervisión de seguridad

-   Estándares de la organización y otros factores específicos de la organización

Según el principio de origen limpio, todas las herramientas que se usan para administrar o supervisar la garras deben ser de confianza o mayor que el nivel de las huellas. Por lo general, esto requiere estas herramientas para administrar desde un PATA para no garantizar ninguna dependencia de seguridad de las estaciones de trabajo de menores privilegio.

Esta tabla describen diferentes enfoques que se pueden usar para administrar y supervisar las huellas:

|Enfoque|Consideraciones|
|------|---------|
|PATA de forma predeterminada<br /><br />-Windows Server Update Services<br />-Windows Defender|-Sin ningún costo adicional<br />-Realiza funciones de seguridad necesario básica<br />-Instrucciones incluidas en esta guía|
|Administrar con [Intune](https://technet.microsoft.com/en-us/library/jj676587.aspx)|<ul><li>Proporciona el control y visibilidad en la nube<br /><br /><ul><li>Implementación de software</li><li>Actualizaciones de software o administrar</li><li>Administración de directivas de Firewall de Windows</li><li>Protección contra el malware</li><li>Asistencia remota</li><li>Administración de licencias de software.</li></ul></li><li>Ninguna infraestructura de servidor necesaria</li><li>Es necesario seguir los pasos de "Habilitar conectividad a servicios en la nube" en la fase 2</li><li>Si el equipo PATA no está unido a un dominio, esto requiere que la aplicación de las líneas de base SCM a las imágenes locales mediante las herramientas proporcionadas en la descarga de línea base de seguridad.</li></ul>|
|Nuevas instancias de System Center para administrar garras|: Proporciona la visibilidad y el control de configuración, la implementación de software y actualizaciones de seguridad<br />-Requiere infraestructura de servidor diferentes, garantizar la seguridad a nivel de huellas y el personal de conocimientos para esos personal con privilegios elevados|
|Administrar garras con herramientas de administración existentes|-Crea riesgo significativo en peligro de huellas, a menos que se muestra la infraestructura existente de administración a nivel de seguridad de garras **Nota:** Microsoft por lo general, desaconseja este enfoque, a menos que tu organización tiene un motivo específico para usarlo. En nuestra experiencia, normalmente hay un coste muy elevado de Traer todas estas herramientas (y sus dependencias de seguridad) hasta el nivel de seguridad de las huellas.<br />-La mayoría de estas herramientas proporcionan visibilidad y el control de configuración, la implementación de software y actualizaciones de seguridad|
|Análisis de seguridad o solicitar el acceso de administrador de herramientas de supervisión|Incluye cualquier herramienta que instala a un agente o requiere una cuenta con acceso administrativo local.<br /><br />-Requiere traer la garantía de seguridad de la herramienta hasta el nivel de huellas.<br />-Puede requerir disminuye la posición de seguridad de garras para admitir la funcionalidad de la herramienta (abrir puertos, instalar Java o de otro software intermedio, etc.), crear una decisión de equilibrio de seguridad|
|Administración de eventos e información de seguridad (SIEM)|<ul><li>Si no tiene agente SIEM<br /><br /><ul><li>Acceder a los eventos en garras sin acceso administrativo usando una cuenta en el **los lectores de registro de eventos** grupo</li><li>Será necesario abrir puertos de red para permitir el tráfico entrante desde los servidores SIEM</li></ul></li><li>Si SIEM requiere un agente, consulta otro fila **análisis de seguridad o solicitar el acceso de administrador de herramientas de supervisión**.</li></ul>|
|Reenvío de eventos de Windows|: Proporciona un método de agentes de reenvío de eventos de seguridad de las huellas a un selector externo o SIEM<br />: Puede tener acceso a los eventos en garras sin acceso administrativo<br />-No requiere la apertura de puertos de red para permitir el tráfico entrante desde los servidores SIEM|

#### <a name="operating-paws"></a>Operativo garras
La solución PATA debe funcionar con los estándares de [estándares operativa](http://aka.ms/securitystandards) basado en principio de origen limpio.

## <a name="related-topics"></a>Temas relacionados
[Servicios de la ciberseguridad atractiva de Microsoft](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx)

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

[Módulo de plataforma segura](C:\sd\docs\p_ent_keep_secure\p_ent_keep_secure\trusted_platform_module_technology_overview.xml)
