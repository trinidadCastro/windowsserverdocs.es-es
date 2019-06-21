---
title: ¿Por qué las estaciones de trabajo con privilegios de acceso puede ayudar a proteger su organización
description: Cómo PAW puede aumentar la postura de seguridad de su organización
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 93589778-3907-4410-8ed5-e7b6db406513
ms.date: 03/13/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: mas
ms.openlocfilehash: 9ac591d65fb84f3c0a8bbd33ca71c93daf892ced
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280728"
---
# <a name="privileged-access-workstations"></a>Estaciones de trabajo de acceso con privilegios

>Se aplica a: Windows Server

Las estaciones de trabajo de acceso con privilegios (Privileged Access Workstations, PAW) proporcionan un sistema operativo dedicado para tareas confidenciales que están protegidas de los ataques en Internet y los vectores de amenazas. Separar estas tareas y cuentas delicadas del diario use estaciones de trabajo y los dispositivos proporcionan una protección muy eficaz de los ataques de suplantación de identidad, aplicación y las vulnerabilidades del sistema operativo, diversos ataques de suplantación y ataques de robo de credenciales, como pulsaciones de teclas iniciar sesión, [Pass-the-Hash](https://aka.ms/pth)y Pass-The-Ticket.

## <a name="what-is-a-privileged-access-workstation"></a>¿Qué es una estación de trabajo de acceso con privilegios?

En términos sencillos, una PAW es una estación de trabajo protegida y bloqueada diseñada para proporcionar controles de alta seguridad para cuentas y tareas confidenciales.  Las PAW se recomiendan para la administración de sistemas de identidad, servicios en la nube y tejido de nube privada, así como para funciones empresariales de carácter delicado.

> [!NOTE]
> La arquitectura PAW no requiere una asignación 1:1 de cuentas a estaciones de trabajo, aunque esta es una configuración habitual. PAW crea un entorno de estaciones de trabajo de confianza que se puede usar en una o varias cuentas.

Con el fin de proporcionar la mayor seguridad, las Paw deben ejecutar siempre el sistema de operativo más actualizado y seguro: Microsoft recomienda Windows 10 Enterprise, que incluye varias características de seguridad adicionales no disponibles en otras ediciones (en concreto, [Credential Guard](https://technet.microsoft.com/library/mt483740%28v=vs.85%29.aspx) y [Device Guard](https://technet.microsoft.com/library/dn986865(v=vs.85).aspx)).

> [!NOTE]
> Las organizaciones sin acceso a Windows 10 Enterprise pueden usar Windows 10 Pro, que incluye muchas de las tecnologías fundamentales de las PAW, como Trusted Boot, BitLocker y Escritorio remoto.  Los clientes de educación pueden usar Windows 10 Education.  Windows 10 Home no debe usarse para PAW.
>
> Puede encontrar una matriz de comparación de las distintas ediciones de Windows 10 en [este artículo](https://www.microsoft.com/en-us/WindowsForBusiness/Compare).

Los controles de seguridad PAW se centran en mitigar el impacto alto y los riesgos de probabilidad alta de riesgo. Estos incluyen mitigar los ataques en el entorno y los riesgos que pueden reducir la eficacia de los controles PAW con el tiempo:

* **Ataques por Internet**: la mayoría de los ataques se originan directa o indirectamente en Internet y usan Internet para exfiltración y comando y control (C2). Un elemento clave para garantizar la seguridad de la PAW consiste en aislarla de la red Internet abierta.
* **Riesgo de facilidad de uso**: si una PAW es demasiado difícil de usar para las tareas diarias, los administradores se sentirán empujados a crear soluciones para que sus trabajos sean más fáciles. Con frecuencia, estas soluciones abren las estaciones de trabajo y cuentas administrativas a riesgos de seguridad considerables. Por este motivo, es muy importante buscar la colaboración de los usuarios de PAW y autorizarles para mitigar estos problemas de facilidad de uso de forma segura. Esto puede realizarse por escuchar sus comentarios, instalar las herramientas y los scripts necesarios para realizar sus trabajos y garantizar todo el personal administrativo son conscientes de por qué se necesita usar una PAW, qué una PAW es y cómo usarla correctamente y correctamente.
* **Riesgos del entorno**: como muchos otros equipos y cuentas del entorno están expuestos a los riesgos de Internet bien directa o indirectamente, una PAW debe estar protegida frente a los ataques de recursos en peligro en el entorno de producción. Para ello, minimiza el uso de las herramientas de administración y las cuentas que tienen acceso a las Paw para proteger y supervisar estas estaciones de trabajo especializadas.
* **Manipulación de la cadena de suministro**: aunque es imposible quitar todos los posibles riesgos de manipulación en la cadena de suministro para el hardware y el software, unas pocas acciones pueden mitigar los vectores de ataques críticos que están fácilmente disponibles para los atacantes. Esto incluye validar la integridad de todos los medios de instalación ([principio de origen limpio](https://aka.ms/cleansource)) y usar un proveedor de confianza para el hardware y el software.
* **Ataques físicos**: como las PAW se pueden mover físicamente y se usan fuera de instalaciones físicamente seguras, se deben proteger frente a ataques que aprovechan el acceso físico no autorizado al equipo.

> [!NOTE]
> Una PAW no protegerá un entorno de un adversario que ya haya conseguido acceso administrativo a través de un bosque de Active Directory.
> Como muchas implementaciones existentes de Active Directory Domain Services llevan funcionando años con el riesgo de robo de credenciales, las organizaciones deben asumir las brechas y considerar la posibilidad de que puedan tener un peligro sin detectar en las credenciales de administrador de dominio o empresa. Una organización que sospecha de peligro en un dominio debe considerar el uso de servicios profesionales de respuesta a incidentes.
>
> Para más información sobre la guía de respuesta y recuperación, consulte las secciones "Respond to suspicious activity" (Respuesta a actividad sospechosa) y "Recover from a breach" (Recuperación de una infracción) de [Mitigating Pass-the-Hash and Other Credential Theft](https://aka.ms/pth), version 2 (Mitigación de ataques pass-the-hash y otros robos de credenciales, versión 2).
>
> Visite la página sobre los [servicios de recuperación y respuesta a incidentes de Microsoft](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx) para más información.

### <a name="paw-hardware-profiles"></a>Perfiles de hardware PAW

El personal administrativo también es usuarios estándar, que necesitan una PAW, así como una estación de trabajo de usuario estándar para comprobar el correo electrónico, explorar la web y tener acceso a la línea corporativa de aplicaciones empresariales.  Para el éxito de cualquier implementación de PAW, es necesario asegurarse de que los administradores pueden permanecer productivos y seguros.  Una solución segura que limite la productividad de forma considerable se abandonará en pro de una que la mejore (aunque sea de manera insegura).

Para equilibrar la necesidad de seguridad con la necesidad de productividad, Microsoft recomienda usar uno de estos perfiles de hardware de PAW:

* **Hardware dedicado** -separar dispositivos dedicados para tareas de usuario frente a las tareas administrativas.
* **Uso simultáneo**: dispositivo único que puede ejecutar tareas de usuario y tareas administrativas a la vez aprovechando las ventajas de la virtualización del sistema operativo o de la presentación.

Las organizaciones pueden usar un solo perfil o ambos. No existen problemas de interoperabilidad entre los perfiles de hardware, y las organizaciones cuentan con la flexibilidad para relacionar el perfil de hardware con la necesidad y la situación específicas de un administrador dado.

> [!NOTE]
> Es fundamental que, en todos estos escenarios, el personal administrativo se emite una cuenta de usuario estándar que es independiente de las cuentas administrativas designadas. Las cuentas administrativas solo deben usarse en el sistema operativo administrativo de PAW.

En esta tabla se resumen las ventajas y desventajas relativas de cada perfil de hardware desde la perspectiva de la facilidad de uso operativa y la productividad y seguridad.  Ambos enfoques de hardware proporcionan una fuerte seguridad para las cuentas administrativas contra el robo y la reutilización de credenciales.

|**Escenario**|**Ventajas**|**Desventajas**|
|--------|---------|-----------|
|Hardware dedicado|-   Señal fuerte para confidencialidad de tareas<br />-   Separación de seguridad más fuerte|-   Espacio de escritorio adicional<br />-   Peso adicional (para trabajo remoto)<br />-   Costo de hardware|
|Uso simultáneo|-   Menos costo de hardware<br />-   Experiencia de un solo dispositivo|-   El uso compartido de un único teclado o mouse crea el riesgo de errores o peligros inadvertidos.|

Esta guía contiene las instrucciones detalladas para la configuración de PAW para el enfoque de hardware dedicado. Si tiene necesidad de usar perfiles de hardware simultáneos, puede adaptar las instrucciones a su caso o contratar una empresa de servicios profesionales, como Microsoft, para que le ayude.

#### <a name="dedicated-hardware"></a>Hardware dedicado

En este escenario, se usa para la administración una PAW que está completamente separada del equipo que se usa para las actividades diarias, como correo electrónico, edición de documentos y trabajo de desarrollo. Todas las herramientas y aplicaciones administrativas se instalan en la PAW y todas las aplicaciones de productividad se instalan en la estación de trabajo de usuario estándar. Las instrucciones paso a paso de esta guía se basan en este perfil de hardware.

#### <a name="simultaneous-use---adding-a-local-user-vm"></a>Uso simultáneo: adición de una máquina virtual de usuario local

En este escenario de uso simultáneo, se usa un solo equipo para las tareas de administración y las tareas diarias, como correo electrónico, edición de documentos y trabajo de desarrollo. En esta configuración, el sistema operativo del usuario está disponible mientras está desconectado (para editar documentos y trabajar en el correo electrónico almacenado localmente en la caché), pero requiere hardware y procesos de soporte técnico que puedan acomodar este estado desconectado.

![Diagrama que representa un solo equipo en un escenario de uso simultáneo utilizado para las tareas de administración y las tareas diarias, como correo electrónico, edición de documentos y trabajo de desarrollo](../media/privileged-access-workstations/PAWFig10.JPG)

El hardware físico ejecuta localmente dos sistemas operativos:

* **Sistema operativo de administración**: el host físico ejecuta Windows 10 en el host de PAW para las tareas administrativas.
* **Sistema operativo de usuario**: una máquina virtual invitada con Hyper-V de un cliente de Windows 10 ejecuta una imagen corporativa.

Con Windows 10 Hyper-V, una máquina virtual de invitado (que también ejecutan Windows 10) puede tener una experiencia de usuario enriquecida incluyendo sonido, vídeo y aplicaciones de comunicaciones de Internet, como Skype para la empresa.

En esta configuración, el trabajo diario que no requiere privilegios administrativos se realiza en la máquina virtual del SO de usuario que tiene una imagen corporativa normal de Windows 10 y no está sujeta a las restricciones aplicadas al host de PAW. Todo el trabajo administrativo se realiza en el SO de administración.

Para configurar esto, siga las instrucciones de esta guía para el host de PAW, agregue características de Hyper-V de cliente, cree una máquina virtual de usuario y luego instale una imagen corporativa de Windows 10 en la máquina virtual de usuario.

Lea el artículo [Client Hyper-V](https://docs.microsoft.com/virtualization/hyper-v-on-windows/index) (Cliente Hyper-V) para más información sobre esta funcionalidad. Tenga en cuenta que el sistema operativo de las máquinas virtuales invitadas debe tener la [licencia de producto de Microsoft](https://www.microsoft.com/en-us/Licensing/product-licensing/products.aspx), que también se describe [aquí](https://download.microsoft.com/download/9/8/D/98D6A56C-4D79-40F4-8462-DA3ECBA2DC2C/Licensing_Windows_Desktop_OS_for_Virtual_Machines.pdf).

#### <a name="simultaneous-use---adding-remoteapp-rdp-or-a-vdi"></a>Uso simultáneo: adición de RemoteApp, RDP o una VDI

En este escenario de uso simultáneo, se usa un solo equipo para las tareas de administración y las tareas diarias, como correo electrónico, edición de documentos y trabajo de desarrollo. En esta configuración, los sistemas operativos de usuario se implementan y administran de forma centralizada (en la nube o en el centro de datos), pero no están disponibles sin conexión.

![Ilustración que representa un solo equipo en un escenario de uso simultáneo utilizado para las tareas de administración y las tareas diarias, como correo electrónico, edición de documentos y trabajo de desarrollo](../media/privileged-access-workstations/PAWFig11.JPG)

El hardware físico ejecuta un solo sistema operativo PAW localmente para las tareas administrativas y establece comunicación con un servicio de Escritorio remoto de Microsoft o de un tercero para las aplicaciones de usuario como correo electrónico, edición de documentos y línea de aplicaciones empresariales.

En esta configuración, el trabajo diario que no requiere privilegios administrativos se realiza en el SO remoto y las aplicaciones que no están sujetas a restricciones se aplican al host de PAW. Todo el trabajo administrativo se realiza en el SO de administración.

Para configurar esto, siga las instrucciones de esta guía para el host de PAW, permita la conectividad de red con los servicios de Escritorio remoto y luego agregue accesos directos al escritorio del usuario de PAW para acceder a las aplicaciones. Los servicios de Escritorio remoto podrían hospedarse de muchas maneras, por ejemplo:

* Un servicio de Escritorio remoto o VDI existentes (local o en la nube).
* Un nuevo servicio que se instala de forma local o en la nube.
* Azure RemoteApp, mediante plantillas de Office 365 preconfiguradas o con sus propias imágenes de instalación.

Para más información sobre Azure RemoteApp, visite [esta página](https://www.remoteapp.windowsazure.com).

## <a name="how-microsoft-is-using-administrative-workstations"></a>Cómo usa Microsoft las estaciones de trabajo administrativas

Microsoft emplea el enfoque de arquitectura PAW tanto a nivel interno en nuestros sistemas como con nuestros clientes. Microsoft utiliza las estaciones de trabajo administrativas internamente en varias funcionalidades, como administración de infraestructura de Microsoft IT, desarrollo de infraestructura de tejido de nube de Microsoft y las operaciones y otros recursos de gran valor.

Esta guía se basa directamente en la arquitectura de referencia de estación de trabajo de acceso con privilegios (PAW) implementada por nuestros servicios profesionales de ciberseguridad para proteger a los clientes frente a los ataques a la seguridad en Internet. Las estaciones de trabajo administrativas son también un elemento clave de la protección más fuerte para las tareas de administración de dominios, la arquitectura de referencia de bosque administrativo Enhanced Security Administrative Environment (Entorno administrativo de seguridad mejorada, ESAE).

Para obtener más información sobre el bosque administrativo ESAE, consulte el *enfoque de diseño de bosque administrativo ESAE* sección [proteger el Material de referencia de acceso con privilegios](../securing-privileged-access/securing-privileged-access-reference-material.md#esae-administrative-forest-design-approach).

## <a name="architecture-overview"></a>Introducción a la arquitectura

El siguiente diagrama representa un "canal" aparte para la administración (una tarea enormemente confidencial) que se crea al mantener cuentas y estaciones de trabajo administrativas dedicadas.

![Diagrama que representa un "canal" aparte para la administración (una tarea enormemente confidencial) que se crea al mantener cuentas y estaciones de trabajo administrativas dedicadas.](../media/privileged-access-workstations/PAWFig1.JPG)

Este enfoque de arquitectura se basa en las protecciones encontradas en las características [Credential Guard](https://technet.microsoft.com/library/mt483740%28v=vs.85%29.aspx) y [Device Guard](https://technet.microsoft.com/library/dn986865(v=vs.85).aspx) de Windows 10 y va más allá de esas protecciones para abarcar las cuentas y las tareas confidenciales.

Esta metodología es apropiada para las cuentas con acceso a recursos de alto valor:

* **Privilegios administrativos** -Paw proporcionan mayor seguridad de gran impacto roles administrativos de TI y las tareas. Esta arquitectura se puede aplicar a la administración de muchos tipos de sistemas, como dominios y bosques de Active Directory, inquilinos de Microsoft Azure Active Directory, inquilinos de Office 365, redes de control de proceso (PCN), Sistemas de control de supervisión y adquisición de datos (SCADA), cajeros automáticos (ATM) y dispositivos de punto de venta (PDV).
* **Alta sensibilidad trabajadores** -el enfoque usado en una PAW también puede proporcionar protección para las tareas de trabajo de información confidencial y personal, como las relativas a la actividad de fusiones y adquisiciones de anuncio previo, versión preliminar informes financieros, presencia de medios sociales organizativos, comunicaciones ejecutivas, secretos comerciales sin patentar, investigaciones confidenciales u otros datos propietarios o confidenciales. En esta guía no se analiza en profundidad la configuración de estos escenarios de trabajadores de la información ni se incluye este escenario de instrucciones técnicas.

    > [!NOTE]
    > Microsoft IT emplea PAW (lo que se conoce internamente como "estaciones de trabajo de administración seguras", o SAW) para administrar el acceso seguro a sistemas de gran valor dentro de Microsoft. Esta guía contiene detalles adicionales sobre el uso de PAW en Microsoft en la sección "Uso de Microsoft de las estaciones de trabajo de administración" que se verá más adelante. Para más información sobre este enfoque del entorno de recursos de alto valor, consulte el artículo [Protecting high-value assets with secure admin workstations](https://msdn.microsoft.com/library/mt186538.aspx) (Protección de recursos de alto valor con estaciones de administración seguras).

En este documento se describe por qué se recomienda este procedimiento para proteger las cuentas con privilegios de gran impacto, qué tal son estas soluciones PAW para proteger los privilegios administrativos y cómo implementar rápidamente una solución PAW para la administración de dominios y servicios en la nube.

Se proporcionan instrucciones detalladas para la implementación de varias configuraciones PAW y se incluyen instrucciones de implementación paso a paso para comenzar a proteger las cuentas comunes de gran impacto:

* [**Fase 1: implementación inmediata para administradores de Active Directory** ](#phase-1-immediate-deployment-for-active-directory-administrators) se proporciona una PAW rápidamente que puede proteger en el entorno local de los roles de administración de dominios y bosques
* [**Fase 2: extensión de la PAW a todos los administradores** ](#phase-2-extend-paw-to-all-administrators) Esto habilita la protección para los administradores de servicios en la nube como Office 365 y Azure, servidores empresariales, aplicaciones empresariales y estaciones de trabajo
* [**Fase 3: seguridad avanzada de PAW** ](#phase-3-extend-and-enhance-protection) aquí discuten protecciones y consideraciones de seguridad de PAW adicionales

### <a name="why-dedicated-workstations"></a>¿Por qué dedicado a las estaciones de trabajo?

El entorno actual de amenazas para las organizaciones está repleto de ataques de suplantación de identidad (phishing) sofisticados y otros ataques por Internet que suponen un riesgo continuo para la seguridad de las cuentas y las estaciones de trabajo expuestas en la red.

En este entorno requiere que las organizaciones adopten una postura de seguridad de "presunción de brecha" al diseñar protecciones para recursos de gran valor, como cuentas administrativas y los recursos empresariales confidenciales. Estos recursos de gran valor deben estar protegidos contra amenazas directas de Internet, así como de ataques montados desde otras estaciones de trabajo, servidores y dispositivos del entorno.

![Ilustración que representa el riesgo para los recursos administrados si un atacante lograra obtener el control de la estación de trabajo de un usuario en la que se usan credenciales confidenciales.](../media/privileged-access-workstations/PAWFig2.JPG)

En esta ilustración se representa el riesgo para los recursos administrados si un atacante lograra obtener el control de la estación de trabajo de un usuario en la que se usan credenciales confidenciales.

Un atacante que tenga el control de un sistema operativo dispone de numerosas formas de obtener acceso ilícito a toda la actividad de la estación de trabajo y suplantar la cuenta legítima. Para obtener este nivel de acceso, puede usar una variedad de técnicas de ataque conocidas y menos conocidas. El aumento en el volumen y la sofisticación de los ataques ha hecho que sea necesario extender el concepto de separación a sistemas operativos cliente completamente separados para cuentas confidenciales. Para más información sobre estos tipos de ataques, visite el [sitio web de Pass The Hash](https://www.microsoft.com/pth), donde encontrará documentos técnicos, vídeos y mucho más.

El enfoque PAW es una extensión del procedimiento recomendado ya consolidado de usar cuentas de administración y de usuario distintas para el personal administrativo. En este procedimiento se usa una cuenta administrativa asignada individualmente que está completamente separada de la cuenta de usuario estándar. PAW se crea sobre ese procedimiento de separación de cuentas al proporcionar una estación de trabajo de plena confianza para esas cuentas confidenciales.

Esta guía de PAW está diseñada para ayudarle a implementar esta funcionalidad de protección de cuentas de gran valor, como cuentas de administradores de TI con privilegios elevados y cuentas empresariales de gran confidencialidad. La guía le ayuda a:

* Limitar la exposición de credenciales a solo hosts de confianza.
* Proporcionar una estación de trabajo de alta seguridad a los administradores para que puedan realizar fácilmente las tareas administrativas.

Restringir las cuentas confidenciales al uso únicamente de estaciones de trabajo PAW con seguridad reforzada ofrece un método de protección sencillo que resulta muy fácil de usar para los administradores y muy difícil de vencer para un adversario.

### <a name="alternate-approaches"></a>Enfoques alternativos

Esta sección contiene información sobre cómo se compara con PAW la seguridad de enfoques alternativos y cómo integrar correctamente estos enfoques en una arquitectura PAW. todos estos enfoques conllevan riesgos importantes cuando se implementa de forma aislada, pero pueden agregar valor a una implementación de PAW en algunos escenarios.

#### <a name="credential-guard-and-windows-hello-for-business"></a>Credential Guard y Windows Hello para empresas

Presentada en Windows 10, la funcionalidad [Credential Guard](https://technet.microsoft.com/library/mt483740%28v=vs.85%29.aspx) emplea seguridad basada en hardware y virtualización para mitigar ataques comunes de robo de credenciales, como pass-the-hash, y proteger las credenciales obtenidas. La clave privada para las credenciales usadas por [Windows Hello para empresas](https://aka.ms/passport) puede ser también se pueden proteger mediante hardware de módulo de plataforma segura (TPM).

Estas mitigaciones son eficaces, pero las estaciones de trabajo aún pueden ser vulnerables a determinados ataques incluso si las credenciales estén protegidas con Credential Guard o Windows Hello para empresas. Entre estos ataques privilegios y el uso de credenciales directamente desde un dispositivo en peligro, la reutilización de credenciales anteriormente robadas antes de habilitar Credential Guard y el uso indebido de herramientas de administración y las configuraciones de aplicación débiles en la estación de trabajo.

Las instrucciones de PAW en esta sección incluyen el uso de muchas de estas tecnologías con cuentas y tareas de un alto nivel de confidencialidad.

#### <a name="administrative-vm"></a>Máquina virtual administrativa

Una máquina virtual administrativa (Administrador de VM) es un sistema operativo dedicado para tareas administrativas hospedadas en un escritorio de usuario estándar. Aunque este enfoque es similar a PAW en cuanto que proporciona un sistema operativo dedicado para tareas administrativas, tiene un grave defecto y es que la máquina virtual administrativa depende del escritorio de usuario estándar para su seguridad.

En el siguiente diagrama se representa la habilidad de los atacantes para seguir la cadena de control hasta el objeto de destino de interés con una máquina virtual administrativa en una estación de trabajo de usuario y la dificultad de crear una ruta en la configuración inversa.

La arquitectura PAW no permite el hospedaje de una VM de administración en una estación de trabajo de usuario, pero se puede hospedar una máquina virtual de usuario con una imagen corporativa estándar en una PAW de administración para proporcionar al personal un único equipo para todas las responsabilidades.

![Diagrama de la arquitectura de PAW](../media/privileged-access-workstations/PAWFig9.JPG)

#### <a name="shielded-vm-based-paws"></a>Paw basadas en máquinas virtuales blindadas

Una variante del modelo de máquina virtual administrativa segura es usar [máquinas virtuales blindadas](../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md) para hospedar una o varias máquinas virtuales de administración junto con una máquina virtual de usuario.
Las máquinas virtuales blindadas están diseñadas para ejecutar cargas de trabajo seguras en un entorno donde los usuarios potencialmente no confiables o código puede estar ejecutando en el escritorio de usuario estándar del equipo físico.
Una máquina virtual blindada tiene un TPM virtual que le permite cifrar sus propios datos en reposo y varios controles administrativos como acceso de consola básica, PowerShell Direct y la capacidad de depurar la máquina virtual están deshabilitados para aislar aún más la máquina virtual desde el escritorio del usuario estándar y otras máquinas virtuales.
Las claves para una máquina virtual blindada se almacenan en un servidor de administración de claves de confianza, lo que requiere el dispositivo físico para atestiguar su identidad y el estado antes de soltar una tecla para iniciar la máquina virtual.
Esto garantiza que sólo pueden iniciar las máquinas virtuales blindadas en los dispositivos previstos y que ejecutan en estos dispositivos configuraciones de software conocida y de confianza.

Dado que las máquinas virtuales blindadas están aisladas entre sí y el escritorio del usuario estándar, es aceptable para ejecutar varias máquinas virtuales blindadas de PAW en un único host, aunque esas máquinas virtuales de administración administración distintos niveles.

Consulte la [implementar Paw mediante un tejido protegido](#deploy-paws-using-a-guarded-fabric) sección para obtener más información.

#### <a name="jump-server"></a>Servidor de salto

Las arquitecturas de "servidor de salto" de administración configuran un pequeño número de servidores de consola administrativos cuyo uso se limita a las tareas administrativas por parte del personal. Normalmente se basan en servicios de escritorio remoto, soluciones de virtualización de presentación de terceros o en una tecnología de Infraestructura de escritorio virtual (VDI).

Este enfoque se propone con frecuencia para mitigar el riesgo para la administración y proporciona algunos controles de seguridad, pero por sí solo resulta vulnerable a determinados ataques porque infringe el [principio de "origen limpio"](../securing-privileged-access/securing-privileged-access-reference-material.md#clean-source-principle). El principio de origen limpio requiere que todas las dependencias de seguridad sean tan confiables como el objeto que se protege.

![Esta ilustración representa una relación de control sencilla.](../media/privileged-access-workstations/PAWFig3.JPG)

Esta ilustración representa una relación de control sencilla. Cualquier sujeto con el control de un objeto es una dependencia de seguridad de dicho objeto. Si un adversario puede controlar una dependencia de seguridad de un objeto de destino (sujeto), puede controlar ese objeto.

La sesión administrativa en el servidor de salto depende de la integridad del equipo local que accede a él. Si este equipo es una estación de trabajo de usuario sujeta a ataques de suplantación y a otros vectores de ataques basados en Internet, la sesión administrativa está sujeta también a esos riesgos.

![Ilustración que representa cómo los atacantes pueden seguir una cadena de control establecida hacia el objeto de destino de su interés](../media/privileged-access-workstations/PAWFig4.JPG)

La figura anterior representa cómo los atacantes pueden seguir una cadena de control establecida hacia el objeto de destino de su interés.

Aunque algunos controles de seguridad avanzados, como la autenticación multifactor, pueden hacer más difícil que un atacante se haga con la sesión administrativa desde la estación de trabajo de usuario, ninguna característica de seguridad puede proteger totalmente contra los ataques técnicos cuando un atacante tiene acceso administrativo al equipo de origen (por ejemplo, mediante la introducción de comandos ilícitos en una sesión legítima, el secuestro de los procesos legítimos, etc.).

La configuración predeterminada en esta guía de PAW instala herramientas administrativas en la PAW, pero una arquitectura de servidor de salto también se puede agregar en caso necesario.

![Ilustración que muestra cómo el hecho de revertir la relación de control y el acceso a las aplicaciones de usuario desde una estación de trabajo de administración no proporciona al atacante ninguna ruta al objeto de destino](../media/privileged-access-workstations/PAWFig5.JPG)

Esta ilustración muestra cómo el hecho de revertir la relación de control y el acceso a las aplicaciones de usuario desde una estación de trabajo de administración no proporciona al atacante ninguna ruta al objeto de destino. El servidor de salto del usuario sigue expuesto al riesgo, así que todavía se aplican los controles de protección, los controles de detección y los procesos de respuesta adecuados para ese equipo accesible desde Internet.

En esta configuración los administradores deben seguir atentamente las prácticas operativas para garantizar que no insertan por accidente las credenciales de administrador en la sesión de usuario de su escritorio.

![Ilustración que indica cómo el hecho de acceder a un servidor de salto administrativo desde una PAW no proporciona al atacante ninguna ruta a los recursos administrativos](../media/privileged-access-workstations/PAWFig6.JPG)

Esta ilustración indica cómo el hecho de acceder a un servidor de salto administrativo desde una PAW no proporciona al atacante ninguna ruta a los recursos administrativos. En este caso, un servidor de salto con una PAW permite consolidar el número de ubicaciones para supervisar la actividad administrativa y distribuir aplicaciones y herramientas administrativas. Si bien la complejidad del diseño aumenta, se pueden simplificar la supervisión de seguridad y las actualizaciones de software si un gran número de cuentas y estaciones de trabajo se usan en la implementación de la PAW. El servidor de salto se tendría que crear y configurar según los estándares de seguridad de la PAW.

#### <a name="privilege-management-solutions"></a>Soluciones de administración con privilegios

Las soluciones de administración con privilegios son aplicaciones que proporcionan acceso temporal a privilegios discretos o a cuentas con privilegios a petición. Se trata de un componente extremadamente valioso de una estrategia completa para proteger el acceso con privilegios y proporcionar visibilidad de vital importancia y responsabilidad de la actividad administrativa.

Estas soluciones emplean normalmente un flujo de trabajo flexible para proporcionar acceso y muchas cuentan con características y funcionalidades de seguridad adicionales, como administración de contraseñas de cuentas de servicio e integración con servidores de salto administrativos. Existen muchas soluciones en el mercado que proporcionan funcionalidades de administración de privilegios, una de las cuales es Privileged Access Management (PAM) de Microsoft Identity Manager (MIM).

Microsoft recomienda el uso de una PAW para acceder a soluciones de administración con privilegios. El acceso a estas soluciones solo se debe conceder a las PAW. Microsoft no recomienda el uso de estas soluciones como sustituto de una PAW porque el acceso a privilegios mediante estas soluciones desde un escritorio de usuario posiblemente en peligro infringe el principio de [origen limpio](https://aka.ms/cleansource) que se representa en el siguiente diagrama:

![Diagrama que muestra por qué Microsoft no recomienda el uso de estas soluciones como sustituto de una PAW porque el acceso a privilegios mediante estas soluciones desde un escritorio de usuario posiblemente en peligro infringe el principio de origen limpio](../media/privileged-access-workstations/PAWFig7.JPG)

Proporcionar una PAW para acceder a estas soluciones le permite obtener las ventajas de seguridad tanto de la PAW como de la solución de administración de privilegios, como se representa en este diagrama:

![Diagrama que representa cómo proporcionar una PAW para acceder a estas soluciones le permite obtener las ventajas de seguridad tanto de la PAW como de la solución de administración de privilegios](../media/privileged-access-workstations/PAWFig8.JPG)

> [!NOTE]
> Estos sistemas se deben clasificar en el nivel más alto del privilegio que administran y se deben proteger con ese nivel de seguridad o uno superior. Normalmente se configuran para administrar soluciones de Nivel 0 y recursos de Nivel 0 y se deben clasificar con el Nivel 0.
> Para obtener más información sobre el modelo de niveles, consulte [ https://aka.ms/tiermodel ](https://aka.ms/tiermodel) para obtener más información sobre los grupos de nivel 0, consulte la equivalencia de nivel 0 en [proteger el Material de referencia de acceso con privilegios](../securing-privileged-access/securing-privileged-access-reference-material.md).

Para obtener más información sobre la implementación de administración de acceso de Microsoft Identity Manager (MIM) con privilegios (PAM), consulte [https://aka.ms/mimpamdeploy](https://aka.ms/mimpamdeploy)

## <a name="paw-scenarios"></a>Escenarios de PAW

Esta sección contiene instrucciones sobre los escenarios a los que se debe aplicar esta guía de PAW. En todos ellos, se debe entrenar a los administradores para usar solo PAW para realizar el soporte técnico de sistemas remotos. Para fomentar el uso seguro y correcto, todos los usuarios de PAW también deben proporcionar comentarios encaminados a mejorar la experiencia de PAW, y estos comentarios se deben revisar cuidadosamente para integrarlos en su programa de PAW.

En todos los escenarios, se puede usar protección adicional en fases posteriores, y se pueden usar diferentes perfiles de esta guía para satisfacer los requisitos de facilidad de uso o seguridad de los roles.

> [!NOTE]
> Esta guía explícitamente marca la diferencia entre la necesidad de tener acceso a servicios específicos en internet (por ejemplo, los portales administrativos de Azure y Office 365) y la "Internet abierta" de todos los hosts y los servicios.

Consulte la [página del modelo de niveles](https://aka.ms/tiermodel) para más información sobre las designaciones de nivel.

|**Escenarios**|**¿Usar PAW?**|**Ámbito y consideraciones de seguridad**|
|---------|--------|---------------------|
|Administradores de Active Directory: Nivel 0|Sí|Una PAW creada con la guía de la Fase 1 es suficiente para este rol.<br /><br />-   Se puede agregar un bosque administrativo para proporcionar la protección más fuerte en este escenario. Para más información sobre el bosque administrativo de ESAE, consulte [Enfoque de diseño de bosque administrativo ESAE](../securing-privileged-access/securing-privileged-access-reference-material.md#esae-administrative-forest-design-approach).<br />-   Se puede usar una PAW para varios dominios o varios bosques administrados.<br />-Si los controladores de dominio se hospedan en una infraestructura como servicio (IaaS) o solución de virtualización local, debe dar prioridad a la implementación de Paw para los administradores de esas soluciones.|
|Administración de servicios de IaaS y PaaS de Azure: Nivel 0 o Nivel 1 (consulte las consideraciones sobre ámbito y diseño)|Sí|Una PAW creada mediante la guía proporcionada en la Fase 2 es suficiente para este rol.<br /><br />-   También se deben usar PAW para al menos el administrador global y el administrador de facturación de suscripciones. Asimismo, se deben usar PAW para administradores delegados de servidores críticos o de carácter confidencial.<br />-Paw deben usarse para administrar el sistema operativo y las aplicaciones que proporcionan sincronización de directorios y federación de identidades para servicios en la nube, como [Azure AD Connect](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect/) y servicios de federación de Active Directory (ADFS).<br />-   Las restricciones de red de salida deben permitir la conectividad solo a servicios en la nube autorizados mediante la guía de la Fase 2. No se debe permitir acceso abierto a Internet desde las PAW.<br />-Windows Defender Exploit Guard debe estar configurada en la estación de trabajo **Nota:**     Una suscripción se considera nivel 0 para un bosque si controladores de dominio u otros hosts de nivel 0 se encuentran en la suscripción. Una suscripción es el Nivel 1 si no hay servidores de Nivel 0 que se hospedan en Azure.|
|Inquilino de administración de Office 365 <br />- Nivel 1|Sí|Una PAW creada mediante la guía proporcionada en la Fase 2 es suficiente para este rol.<br /><br />-   Se deben usar PAW para al menos los roles de administrador de facturación de suscripciones, administrador global, administrador de Exchange, administrador de SharePoint y administrador de administración de usuarios. Debe considerar también seriamente el uso de PAW para administradores delegados de datos enormemente críticos o confidenciales.<br />-Windows Defender Exploit Guard debe configurarse en la estación de trabajo.<br />-Las restricciones de red saliente deben permitir conectividad sólo a los servicios de Microsoft con la orientación en la Fase 2. No se debe permitir acceso abierto a Internet desde las PAW.|
|Otros administradores de servicios en la nube IaaS o PaaS<br />-Nivel 0 o Nivel 1 (consulte las consideraciones sobre ámbito y diseño)|Sí|Una PAW creada mediante la guía proporcionada en la Fase 2 es suficiente para este rol.<br /><br />-   Se deben usar PAW para cualquier rol con derechos administrativos sobre máquinas virtuales hospedadas en la nube, lo que incluye la posibilidad de instalar agentes, exportar archivos de disco duro o acceder al almacenamiento donde residen unidades de disco duro con sistemas operativos, datos confidenciales o datos críticos de la empresa.<br />-Las restricciones de red saliente deben permitir conectividad sólo a los servicios de Microsoft con la orientación en la Fase 2. No se debe permitir acceso abierto a Internet desde las PAW.<br />-Windows Defender Exploit Guard debe configurarse en la estación de trabajo. **Nota:** Una suscripción es nivel 0 para un bosque si controladores de dominio o de otros hosts de nivel 0 se encuentran en la suscripción. Una suscripción es el Nivel 1 si no hay servidores de Nivel 0 que se hospedan en Azure.|
|Administradores de virtualización<br />-Nivel 0 o Nivel 1 (consulte las consideraciones sobre ámbito y diseño)|Sí|Una PAW creada mediante la guía proporcionada en la Fase 2 es suficiente para este rol.<br /><br />-   Se deben usar PAW para cualquier rol con derechos administrativos sobre máquinas virtuales, lo que incluye la posibilidad de instalar agentes, exportar archivos de disco duro virtual o acceder al almacenamiento donde residen unidades de disco duro con información de sistemas operativos invitados, datos confidenciales o datos críticos de la empresa. **Nota:** Un sistema de virtualización (y sus administradores) se consideran nivel 0 para un bosque si controladores de dominio u otros hosts de nivel 0 se encuentran en la suscripción. Una suscripción es Nivel 1 si no hay servidores de Nivel 0 hospedados en el sistema de virtualización.|
|Administración de mantenimiento del servidor<br />- Nivel 1|Sí|Una PAW creada mediante la guía proporcionada en la Fase 2 es suficiente para este rol.<br /><br />-   Se debe usar una PAW para administradores que actualicen, apliquen revisiones y solucionen problemas para servidores y aplicaciones de empresa que ejecutan Windows Server, Linux y otros sistemas operativos.<br />-   Puede que sea necesario agregar a las PAW herramientas de administración dedicadas para gestionar la escala más grande de estos administradores.|
|Administradores de estaciones de trabajo de usuario <br />- Nivel 2|Sí|Una PAW creada mediante la guía proporcionada en la Fase 2 es suficiente para los roles que tienen derechos administrativos sobre los dispositivos de usuario final (como roles del departamento de soporte técnico).<br /><br />-   Puede ser necesario instalar aplicaciones adicionales en las PAW para permitir la administración de vales y otras funciones de soporte técnico.<br />-Windows Defender Exploit Guard debe configurarse en la estación de trabajo.<br />    Puede que sea necesario agregar a las PAW herramientas de administración dedicadas para gestionar la escala más grande de estos administradores.|
|Administradores de SQL, SharePoint o de línea de negocios (LOB)<br />- Nivel 1|Sí|Una PAW creada con la guía de la Fase 2 es suficiente para este rol.<br /><br />-   Puede que sea necesario instalar en las PAW herramientas adicionales para administrar las aplicaciones sin necesidad de conectarse a los servidores mediante Escritorio remoto.|
|Usuarios que administran la presencia de medios sociales|Parcialmente|Como punto de partida, se puede usar una PAW creada usando la guía de la Fase 2 para proporcionar seguridad para estos roles.<br /><br />-   Proteja y administre cuentas de medios sociales mediante Azure Active Directory (AAD) para compartir, proteger y realizar el seguimiento del acceso a cuentas de medios sociales.<br />    Para más información sobre esta funcionalidad, lea [esta entrada de blog](http://blogs.technet.com/b/ad/archive/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview.aspx).<br />-   Las restricciones de red saliente deben permitir la conectividad a estos servicios. Para lograr esto, se pueden permitir conexiones abiertas a Internet (un riesgo mucho mayor para la seguridad que niegan muchos controles de PAW) o permitir únicamente las direcciones DNS necesarias para el servicio (que pueden ser difíciles de obtener).|
|Usuarios estándar|No|Aunque se pueden usar muchos pasos de protección para usuarios estándar, las PAW están diseñadas para aislar las cuentas de acceso abierto a Internet que la mayoría de los usuarios necesitan en sus deberes diarios.|
|VDI/quiosco de invitado|No|Aunque se pueden usar muchos pasos de protección en un sistema de quiosco para los invitados, la arquitectura PAW está diseñada para proporcionar una mayor seguridad en cuentas de alta confidencialidad, no una mayor seguridad en cuentas de baja confidencialidad.|
|Usuario VIP (ejecutivo, investigador, etc.)|Parcialmente|Una PAW creada mediante las instrucciones proporcionadas en la fase 2 se puede usar como punto de partida para proporcionar seguridad para estos roles.<br /><br />-   Este escenario es similar a un escritorio de usuario estándar, pero normalmente tiene un perfil de aplicación más pequeño, sencillo y bien conocido. Este escenario requiere normalmente la detección y la protección de datos, servicios y aplicaciones confidenciales (que pueden o no estar instalados en los escritorios).<br />-   Estos roles requieren normalmente un alto grado de seguridad y un grado muy alto de facilidad de uso, lo que implica cambios de diseño para satisfacer las preferencias de los usuarios.|
|Sistemas de control industrial (por ejemplo, SCADA, PCN y DCS)|Parcialmente|Como punto de partida, se puede usar una PAW creada mediante la guía de la Fase 2 para proporcionar seguridad para estos roles dado que la mayoría de las consolas de ICS (lo que incluye estándares tan comunes como SCADA y PCN) no requieren explorar la Internet abierta ni consultar el correo electrónico.<br /><br />-Aplicaciones usadas para controlar la maquinaria física tendría que integrado y probado la compatibilidad y estén protegidas correctamente.|
|Sistema operativo incrustado|No|Aunque muchos de los pasos de protección de una PAW se pueden usar en sistemas operativos incrustados, habría que desarrollar una solución personalizada para proteger este escenario.|

> [!NOTE]
> **Escenarios de combinación:** algunos miembros del personal pueden tener responsabilidades administrativas que abarcan varios escenarios.
> En estos casos, las reglas principales a tener en cuenta son que siempre se deben seguir las reglas del modelo de nivel. Consulte la página del modelo de niveles para más información.

> [!NOTE]
> **Escalado del programa de PAW:** a medida que el programa de PAW se escala para abarcar más administradores y roles, debe seguir asegurándose de mantener la adhesión a los estándares de seguridad y la facilidad de uso. Para ello, puede ser necesario actualizar las estructuras de soporte técnico de TI o crear otras nuevas para resolver las dificultades específicas de las PAW, como los procesos de incorporación de PAW, la administración de incidentes, la administración de configuración y la recopilación de comentarios para abordar los retos de la facilidad de uso.  Un ejemplo podría ser que su organización decidiera habilitar escenarios de trabajo desde casa para los administradores, lo que precisaría de un cambio de las PAW de escritorio a las PAW de equipos portátiles, cambio que podría necesitar tener en cuenta otros aspectos de la seguridad.  Otro ejemplo común es crear o actualizar entrenamiento para nuevos administradores, entrenamiento que debe incluir ahora contenido sobre el uso adecuado de una PAW (lo que incluye por qué es importante y qué es y no es una PAW).  Para conocer otras consideraciones que se deben analizar al escalar el programa de PAW, consulte la fase 2 de las instrucciones.

Esta guía contiene instrucciones detalladas para la configuración de PAW en los escenarios indicados anteriormente. Si tiene requisitos para los otros escenarios, puede adaptar las instrucciones a su caso o contratar una empresa de servicios profesionales, como Microsoft, para que le ayude.

Para más información sobre cómo contratar servicios de Microsoft para diseñar una PAW a la medida de su entorno, póngase en contacto con su representante de Microsoft o visite [esta página](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).

## <a name="paw-phased-implementation"></a>Implementación de PAW Phased

Como la PAW debe proporcionar un origen seguro y de confianza para los administradores, es esencial que el proceso de compilación sea seguro y de confianza.  En esta sección se proporcionan instrucciones detalladas que le permitirán crear su propia PAW por medio de principios y conceptos generales muy parecidos a los que usa Microsoft IT y las organizaciones de ingeniería y administración de servicios en la nube.

Las instrucciones se dividen en tres fases que se centran en implantar las mitigaciones más críticas rápidamente y en aumentar y expandir progresivamente el uso de PAW en la empresa.

* [Fase 1: implementación inmediata para administradores de Active Directory](#phase-1-immediate-deployment-for-active-directory-administrators)
* [Fase 2: extensión de la PAW a todos los administradores](#phase-2-extend-paw-to-all-administrators)
* [Fase 3: seguridad avanzada de PAW](#phase-3-extend-and-enhance-protection)

Es importante tener en cuenta que las fases se deben realizar siempre en orden, incluso si se planean e implementan como parte del mismo proyecto general.

### <a name="phase-1-immediate-deployment-for-active-directory-administrators"></a>Fase 1: Su implementación inmediata para administradores de Active Directory

Propósito: Proporciona una PAW rápidamente que puede proteger los roles de administración de dominios y bosques locales.

Ámbito: Administradores de nivel 0 como administradores de organización, Admins. del dominio (para todos los dominios) y los administradores de otros sistemas de identidad autorizados.

La Fase 1 se centra en los administradores que administran su dominio de Active Directory local, roles que son de vital importancia y a los que con frecuencia se dirigen los ataques. Estos sistemas de identidad funcionarán de forma efectiva para proteger a estos administradores tanto si los controladores de dominio (DC) de Active Directory están hospedados en centros de datos locales, en una Infraestructura como servicio (IaaS) de Azure o en otro proveedor de IaaS.

Durante esta fase, se creará la estructura de unidad organizativa (OU) de Active Directory administrativa segura para hospedar su estación de trabajo de acceso con privilegios (PAW) y para implementar las PAW propiamente dichas.  Esta estructura también incluye las directivas de grupo y los grupos necesarios para admitir la PAW.  La mayor parte de la estructura se creará mediante scripts de PowerShell que están disponibles en la [ TechNet Gallery](https://aka.ms/pawmedia).

Los scripts crearán las siguientes unidades organizativas y grupos de seguridad:

* Unidades organizativas (OU)
   * Seis unidades organizativas nuevas nivel superior:  Admin; Grupos; Servidores de nivel 1; Estaciones de trabajo; Cuentas de usuario; y cuarentena de equipos.  Cada unidad organizativa superior contendrá varios OU secundarias.
* Grupos
   * Seis nuevos grupos con seguridad globales:  Mantenimiento de replicación de nivel 0; Mantenimiento del servidor de nivel 1; Operadores del departamento de soporte técnico de servicio; Mantenimiento de la estación de trabajo; Usuarios de PAW; Mantenimiento PAW.

También creará varios objetos de directiva de grupo: Configuración de PAW: equipo; Configuración de PAW: usuario; Se requiere RestrictedAdmin: equipo; Restricciones de salida de PAW; Restringir el inicio de sesión de la estación de trabajo; Restringir el inicio de sesión de servidor.

La Fase 1 incluye los siguientes pasos:

#### <a name="complete-the-prerequisites"></a>Realización de los requisitos previos

1. **Asegúrese de que todos los administradores usan cuentas individuales independientes para la administración y las actividades de usuario final** (como el correo electrónico, la exploración de Internet, aplicaciones de línea de negocio y otras actividades no administrativas).  En el modelo PAW es esencial asignar una cuenta administrativa a cada personal autorizado separada de su cuenta de usuario estándar, ya que solo ciertas cuentas tendrán permiso para iniciar sesión en la propia PAW.

   > [!NOTE]
   > Cada administrador debe usar su cuenta para la administración.  No comparta una cuenta administrativa.

2. **Minimice el número de administradores con privilegios de Nivel 0**.  Dado que cada administrador debe usar una PAW, reducir el número de administradores reduce el número de PAW necesarias para respaldarlos y los costos asociados. El número menor de administradores también tiene como resultado una menor exposición de estos privilegios y los riesgos asociados. Aunque es posible que los administradores de una ubicación compartan una PAW, los administradores de ubicaciones físicas diferentes necesitarán PAW diferentes.
3. **Adquiera hardware de un proveedor de confianza que cumpla todos los requisitos técnicos**. Microsoft recomienda la adquisición de hardware que cumpla los requisitos técnicos en el artículo [Proteger las credenciales de dominio derivadas con Credential Guard](https://technet.microsoft.com/library/mt483740%28v=vs.85%29.aspx).

   > [!NOTE]
   > Una PAW instalada en hardware sin estas funcionalidades puede proporcionar importantes protecciones, pero características de seguridad avanzadas, como Credential Guard y Device Guard, estarán disponibles.  Credential Guard y Device Guard no son necesarias para la Fase 1, pero se recomiendan encarecidamente como parte de la Fase 3 (protección avanzada).
   >
   > Asegúrese de que el hardware usado para la PAW proceda de un fabricante y un proveedor cuyas prácticas de seguridad sean de confianza para la organización. Esta es una aplicación del principio de origen limpio a la seguridad de la cadena de suministro.
   >
   > Para más información sobre la importancia de la seguridad de la cadena de suministro, visite [este sitio](https://www.microsoft.com/security/cybersecurity/).

4. **Adquiera y valide las requiere Windows 10 Enterprise Edition y el software de aplicación**. Obtenga el software necesario para PAW y valídelo con la guía de [origen limpio para medios de instalación](https://aka.ms/cleansource).

   * Windows 10 Enterprise Edition
   * [Herramientas de administración remota del servidor](https://www.microsoft.com/en-us/download/details.aspx?id=45520) para Windows 10
   * [Líneas de base de seguridad de Windows 10](https://aka.ms/win10baselines)

      > [!NOTE]
      > Microsoft publica hashes MD5 para todos los sistemas operativos y aplicaciones en MSDN, pero no todos los proveedores de software proporcionan documentación similar.  En esos casos, se requerirán otras estrategias.  Para más información sobre la validación de software, consulte la sección sobre el principio de [rigen limpio](https://aka.ms/cleansource) para medios de instalación.

5. **Asegúrese de tener un servidor WSUS disponible en la intranet**. Necesitará un servidor WSUS en la intranet para descargar e instalar actualizaciones para PAW. Este servidor WSUS se debe configurar para aprobar automáticamente todas las actualizaciones de seguridad para Windows 10, o un personal administrativo debe ser responsable de aprobar rápidamente actualizaciones de software.

   > [!NOTE]
   > Para más información, consulte la sección "Automatically Approve Updates for Installation" (Aprobación automática de actualizaciones para la instalación) en la [guía de aprobación de actualizaciones](https://technet.microsoft.com/library/cc708458(v=ws.10).aspx).

#### <a name="deploy-the-admin-ou-framework-to-host-the-paws"></a>Implementación del marco de unidad organizativa de administración para hospedar las PAW

1. Descargue la biblioteca de scripts de PAW de la [Galería de TechNet](https://aka.ms/PAWmedia).

   > [!NOTE]
   > Descargar todos los archivos y guardarlos en el mismo directorio y ejecutarlos en el siguiente orden.  Create-PAWGroups depende de la estructura de unidad organizativa creada por Create-PAWOU, y Set-PAWOUDelegation depende de los grupos creados por Create-PAWGroups.
   > No modifique ninguno de los scripts o el archivo de valores separados por comas (CSV).

2. **Ejecute el script Create-PAWOUs.ps1**.  Este script creará la nueva estructura de unidad organizativa (OU) en Active Directory y bloqueará la herencia de GPO en las nuevas unidades organizativas, según sea adecuado.
3. **Ejecute el script Create-PAWGroups.ps1**.  Este script creará los nuevos grupos de seguridad globales en las unidades organizativas adecuadas.

   > [!NOTE]
   > Aunque este script creará los nuevos grupos de seguridad, no se llenarán automáticamente.

4. **Ejecute el script Set-PAWOUDelegation.ps1**.  Este script asignará permisos a las nuevas unidades organizativas a los grupos adecuados.

#### <a name="move-tier-0-accounts-to-the-admintier-0accounts-ou"></a>Mueva las cuentas de nivel 0 unidad organizativa administrador\nivel 0\cuentas

Mueva cada cuenta que sea miembro de los grupos equivalentes de Administradores de dominio, Administradores de empresa o Nivel 0 (incluida la pertenencia anidada) a esta unidad organizativa. Si su organización tiene sus propios grupos que se agregan a estos grupos, debe moverlos a la unidad organizativa de administración/Nivel 0/grupos.

   > [!NOTE]
   > Para más información sobre qué grupos son de Nivel 0, consulte "Tier 0 Equivalency" (Equivalencia de Nivel 0) [Protección del material de referencia de acceso con privilegios](../securing-privileged-access/securing-privileged-access-reference-material.md).

#### <a name="add-the-appropriate-members-to-the-relevant-groups"></a>Adición de los miembros adecuados a los grupos pertinentes

1. **Usuarios de PAW**: agregue los administradores de Nivel 0 con los grupos Administradores de dominio o Administradores de empresa identificados en el paso 1 de la Fase 1.
2. **Mantenimiento de PAW**: agregue al menos una cuenta que se use para las tareas de mantenimiento y solución de problemas de PAW. Las cuentas de mantenimiento de PAW se usarán solo en raras ocasiones.

   > [!NOTE]
   > No agregue la misma cuenta de usuario o grupo a Usuarios de PAW y Mantenimiento de PAW.  El modelo de seguridad de PAW se basa parcialmente en la suposición de que la cuenta de usuario de PAW tiene derechos con privilegios sobre los sistemas administrados o sobre la propia PAW, pero no sobre ambos.
   >
   > * Esto es importante para la creación de buenos procedimientos y hábitos administrativos en la Fase 1.
   > * También es de importancia crítica para la Fase 2 y más adelante para impedir el escalado de privilegios mediante PAW ya que las PAW abarcan niveles.
   >
   > Idealmente, no se asigna personal a //deberes en varios niveles para aplicar el principio de segregación de deberes, pero Microsoft reconoce que muchas organizaciones tienen personal limitado (u otros requisitos organizativos) que no permiten esta segregación completa. En estos casos, se puede asignar el mismo personal a ambos roles, pero no deben usar la misma cuenta para estas funciones.

#### <a name="create-paw-configuration---computer-group-policy-object-gpo"></a>Crear objeto de directiva de grupo (GPO) de "PAW configuración: equipo"

En esta sección, creará un nuevo "PAW configuración: equipo" GPO que proporciona protecciones específicas para estas Paw y vincularlo a la unidad organizativa de dispositivos de nivel 0 ("dispositivos" en nivel 0\administrador).

   > [!NOTE]
   > **No agregue estos valores a la directiva de dominio predeterminada**.  Si lo hace, podría afectar a las operaciones del entorno entero de Active Directory.  Configure solo estos valores en los GPO recién creados que se describen aquí y aplíquelos solamente a la unidad organizativa de PAW.

1. **PAW Maintenance Access** (Acceso de mantenimiento de PAW): esta opción establecerá la pertenencia de grupos con privilegios específicos de las PAW en un conjunto específico de usuarios. Vaya a *Configuración de equipo\Preferencias\Configuración del Panel de control\Usuarios y grupos locales* y siga los pasos siguientes:
   1. Haga clic en **Nuevo** y haga clic en **Grupo local**.
   2. Seleccione la acción **Actualizar** y seleccione "Administradores (integrados)" (no use el botón Examinar para seleccionar los administradores del grupo de dominios).
   3. Active las casillas **Eliminar todos los usuarios miembro** y **Eliminar todos los grupos de miembros**.
   4. Agregue Mantenimiento de PAW (pawmaint) y Administrador (de nuevo, no use el botón Examinar para seleccionar el administrador).

      > [!NOTE]
      > No agregue el grupo Usuarios de PAW a la lista de pertenencia del grupo de administradores locales.  Para asegurarse de que los usuarios de PAW no puedan modificar accidental o deliberadamente la configuración de seguridad de la propia PAW, no deben ser miembros de los grupos de administradores locales.
      >
      > Para más información sobre el uso de las preferencias de directiva de grupo para modificar la pertenencia a grupos, consulte el artículo de TechNet [Configurar un elemento de grupo local](https://technet.microsoft.com/library/cc732525.aspx).

2. **Restrict Local Group Membership** (Restringir la pertenencia al grupo local): esta opción garantiza que la pertenencia de los grupos de administradores locales en la estación de trabajo siempre está vacía.
   1. Vaya a Configuración de equipo\Preferencias\Configuración del Panel de control\Usuarios y grupos locales y siga estos pasos:
      1. Haga clic en **Nuevo** y haga clic en **Grupo local**.
      2. Seleccione la acción **Actualizar** y seleccione "Operadores de copia de seguridad (integrados)" (no use el botón Examinar para seleccionar los operadores de copia de seguridad del grupo de dominio).
      3. Active las casillas **Eliminar todos los usuarios miembro** y **Eliminar todos los grupos de miembros**.
      4. No agregue ningún miembro al grupo.  Simplemente haga clic en **Aceptar**.  Al asignar una lista vacía, la directiva de grupo quita automáticamente todos los miembros y garantiza una lista de miembros vacía cada vez que se actualiza la directiva de grupo.
   2. Realice los pasos anteriores para los siguientes grupos adicionales:
      * Operadores criptográficos
      * Administradores de Hyper-V
      * Operadores de configuración de red
      * Usuarios avanzados
      * Usuarios de escritorio remoto
      * Replicadores
   3. **Restricciones de inicio de sesión PAW** -esta configuración limita las cuentas que pueden iniciar sesión en la PAW. Para configurar este valor, siga estos pasos:
      1. Vaya a Configuración de equipo\Directivas\Configuración de Windows\Configuración de seguridad\Directivas locales\Asignación de derechos de usuario\Permitir el inicio de sesión local.
      2. Seleccione Definir esta configuración de directiva y agregue "Usuarios de PAW" y los administradores (de nuevo, no use el botón Examinar para seleccionar los administradores).
   4. **Bloquear el tráfico de red entrante**: esta opción garantiza que no se permite tráfico de red no solicitado a la PAW. Para configurar este valor, siga estos pasos:
      1. Vaya a Configuración del equipo\Directivas\Configuración de Windows\Configuración de seguridad\Firewall de Windows con seguridad avanzada\Firewall de Windows con seguridad avanzada y siga estos pasos:
         1. Haga clic con el botón derecho en Firewall de Windows con seguridad avanzada y seleccione **Importar directiva**.
         2. Haga clic en **Sí** para aceptar que se sobrescribirá cualquier directiva de firewall existente.
         3. Vaya a PAWFirewall.wfw y seleccione **Abrir**.
         4. Haga clic en **Aceptar**.

            > [!NOTE]
            > En este punto, puede agregar direcciones o subredes que deben establecer comunicación con la PAW (por ejemplo, exploración de seguridad o software de administración).
            > La configuración del archivo WFW habilitará el firewall en modo "Bloquear (predeterminado)" para todos los perfiles de firewall, desactivará la combinación de reglas y habilitará el registro de paquetes descartados y paquetes correctos. Esta configuración se bloquea el tráfico no solicitado mientras sigue permitiendo la comunicación bidireccional en las conexiones iniciadas desde la PAW, impedir que los usuarios con acceso administrativo local creen reglas de firewall local que podría invalidar la configuración del GPO y Asegúrese de que se registra el tráfico dentro y fuera de la PAW.
            > **Al abrir este firewall se ampliará la superficie de ataque para la PAW y aumentará el riesgo para la seguridad. Antes de agregar cualquier dirección, consulte la sección Administración y funcionamiento de PAW en esta guía**.

   5. **Configurar Windows Update para WSUS**: siga estos pasos para configurar Windows Update para las PAW:
      1. Vaya al equipo Configuración del equipo\Directivas\Plantillas administrativas\Componentes de Windows\Windows Updates y siga los pasos siguientes:
         1. Habilite la **Configurar Actualizaciones automáticas**.
         2. Seleccione la opción **4 - Descargar automáticamente y programar la instalación**.
         3. Cambie la opción **Día de instalación programado** en **0 - Todos los días** y la opción **Hora de instalación programada** en su preferencia organizativa.
         4. Habilitar la opción **especificar la ubicación del servicio Microsoft update de la intranet** directiva, y especifique en ambas opciones de la dirección URL del servidor WSUS de ESAE.
   6. Vincule el "PAW configuración: equipo" GPO como sigue:

         |Directiva|Ubicación del vínculo|
         |-----|---------|
         |Configuración de PAW: equipo |Administrador\Nivel 0\Dispositivos|

#### <a name="create-paw-configuration---user-group-policy-object-gpo"></a>Crear objeto de directiva de grupo (GPO) de ": configuración de PAW usuario"

En esta sección, creará un nuevo "PAW Configuración – usuario" los GPO que proporciona protecciones específicas para estas Paw y un vínculo a la unidad organizativa de cuentas de nivel 0 ("Accounts" en nivel 0\administrador).

   > [!NOTE]
   > No agregue estos valores a la directiva de dominio predeterminada.

1. **Block internet browsing** (Bloquear exploración de Internet): para impedir la exploración en Internet por accidente, esta opción establecerá una dirección proxy de una dirección de bucle invertido (127.0.0.1).
   1. Vaya a configuración de usuario\preferencias\configuración. Haga clic en registro, seleccione **New** > **elemento del registro** y configure las siguientes opciones:
      1. Acción:  Reemplazar
      2. Hive: HKEY_CURRENT_USER
      3. Ruta de acceso de clave:  Software\Microsoft\Windows\CurrentVersion\Internet Settings
      4. Nombre del valor: ProxyEnable

         > [!NOTE]
         > No seleccione el cuadro Valor predeterminado a la izquierda del nombre de valor.

      5. Tipo de valor: REG_DWORD
      6. Datos del valor: 1
         1. Haga clic en la pestaña Común y seleccione **Quitar este elemento cuando ya no se aplique**.
         2. En la pestaña Común, seleccione **Destinatarios de nivel de elemento** y haga clic en **Destino**.
         3. Haga clic en **Nuevo elemento** y seleccione **Grupo de seguridad**.
         4. Seleccione el botón "..." y busque el grupo Usuarios de PAW.
         5. Haga clic en **Nuevo elemento** y seleccione **Grupo de seguridad**.
         6. Seleccione el botón "..." y busque el **Administradores de servicios en la nube**.
         7. Haga clic en el elemento **Administradores de servicios en la nube** y haga clic en **Opciones de elemento**.
         8. Seleccione **No es**.
         9. Haga clic en **Aceptar** en la ventana de destino.
      7. Haga clic en **Aceptar** para completar la configuración de directiva de grupo ProxyServer.
   2. Vaya a configuración de usuario\preferencias\configuración. Haga clic en registro, seleccione **New** > **elemento del registro** y configure las siguientes opciones:

      * Acción: Reemplazar
      * Hive: HKEY_CURRENT_USER
      * Ruta de acceso de clave: Software\Microsoft\Windows\CurrentVersion\Internet Settings
         * Nombre del valor: ProxyServer

            > [!NOTE]
            > No seleccione el cuadro Valor predeterminado a la izquierda del nombre de valor.

         * Tipo de valor: REG_SZ
         * Datos del valor: 127.0.0.1:80
            1. Haga clic en la pestaña **Común** y seleccione **Quitar este elemento cuando ya no se aplique**.
            2. En la pestaña **Común**, seleccione **Destinatarios de nivel de elemento** y haga clic en **Destino**.
            3. Haga clic en **Nuevo elemento** y seleccione el grupo de seguridad.
            4. Seleccione el botón "..." y agregue el grupo Usuarios de PAW.
            5. Haga clic en **Nuevo elemento** y seleccione el grupo de seguridad.
            6. Seleccione el botón "..." y busque el **Administradores de servicios en la nube**.
            7. Haga clic en el elemento **Administradores de servicios en la nube** y haga clic en **Opciones de elemento**.
            8. Seleccione **No es**.
            9. Haga clic en **Aceptar** en la ventana de destino.

   3. Haga clic en **Aceptar** para completar la configuración de directiva de grupo ProxyServer.
2. Vaya a usuario Configuración del equipo\Directivas\Plantillas administrativas\Componentes de Windows\Internet Explorer y habilite las siguientes opciones. Esta configuración impedirá que los administradores invaliden manualmente la configuración de proxy.
   1. Habilite la opción **Deshabilitar el cambio de valores de Configuración automática**.
   2. Habilite la opción **Impedir el cambio de configuración de proxy**.

#### <a name="restrict-administrators-from-logging-onto-lower-tier-hosts"></a>Impedir que los administradores iniciar sesión en hosts de nivel inferior

En esta sección, configuraremos directivas de grupo para impedir que las cuentas administrativas con privilegios inicien sesión en los hosts de nivel inferior.

1. Cree el nuevo GPO **Restringir inicio de sesión de estación de trabajo**: esta configuración restringirá a las cuentas de administrador de Nivel 0 y Nivel 1 el inicio de sesión en estaciones de trabajo estándar.  Este GPO debe estar vinculada a la OU de nivel superior "Estaciones de trabajo" y tener las siguientes opciones:
   * En el equipo equipo\Directivas\Configuración Windows\Configuración configuración de seguridad\Directivas locales\Asignación de derechos de usuario\denegar inicio de sesión como trabajo por lotes, seleccione **definir esta configuración de directiva** y agregue los grupos de nivel 1 y nivel 0:     Enterprise Domain Admins esquema administradores administradores dominio\administradores cuenta operadores operadores de copia de impresión operadores Server operadores dominio controladores grupo de controladores de dominio de solo lectura directiva Creadores propietarios criptográfico Oper mouse

         > [!NOTE]
         > Built-in Tier 0 Groups, see Tier 0 equivalency for more details.

         Other Delegated Groups

         > [!NOTE]
         > Any custom created groups with effective Tier 0 access, see Tier 0 equivalency for more details.

         Tier 1 Admins

         > [!NOTE]
         > This Group was created earlier in Phase 1.

   * En el equipo equipo\Directivas\Configuración Windows\Configuración configuración de seguridad\Directivas locales\Asignación de derechos de usuario\denegar inicio de sesión como servicio, seleccione **definir esta configuración de directiva** y agregue los grupos de nivel 1 y nivel 0:     Enterprise Domain Admins esquema administradores administradores dominio\administradores cuenta operadores operadores de copia de impresión operadores Server operadores dominio controladores grupo de controladores de dominio de solo lectura directiva Creadores propietarios criptográfico Oper mouse

         > [!NOTE]
         > Note: Built-in Tier 0 Groups, see Tier 0 equivalency for more details.

         Other Delegated Groups

         > [!NOTE]
         > Note: Any custom created groups with effective Tier 0 access, see Tier 0 equivalency for more details.

         Tier 1 Admins

         > [!NOTE]
         > Note: This Group was created earlier in Phase 1

2. Crear el nuevo **restringir el inicio de sesión de servidor** GPO: esta configuración restringirá a las cuentas de administrador de nivel 0 de sesión en servidores de nivel 1.  Este GPO debe estar vinculada a la OU de nivel superior "Servidores de capa 1" y tener las siguientes opciones:
   * En el equipo equipo\Directivas\Configuración Windows\Configuración configuración de seguridad\Directivas locales\Asignación de derechos de usuario\denegar inicio de sesión como trabajo por lotes, seleccione **definir esta configuración de directiva** y agregue los grupos nivel 0:     Enterprise Domain Admins esquema administradores administradores dominio\administradores cuenta operadores operadores de copia de impresión operadores Server operadores dominio controladores grupo de controladores de dominio de solo lectura directiva Creadores propietarios criptográfico Oper mouse

         > [!NOTE]
         > Built-in Tier 0 Groups, see Tier 0 equivalency for more details.

         Other Delegated Groups

         > [!NOTE]
         > Any custom created groups with effective Tier 0 access, see Tier 0 equivalency for more details.

   * En el equipo equipo\Directivas\Configuración Windows\Configuración configuración de seguridad\Directivas locales\Asignación de derechos de usuario\denegar inicio de sesión como servicio, seleccione **definir esta configuración de directiva** y agregue los grupos nivel 0:     Enterprise Domain Admins esquema administradores administradores dominio\administradores cuenta operadores operadores de copia de impresión operadores Server operadores dominio controladores grupo de controladores de dominio de solo lectura directiva Creadores propietarios criptográfico Oper mouse

         > [!NOTE]
         > Built-in Tier 0 Groups, see Tier 0 equivalency for more details.

         Other Delegated Groups

         > [!NOTE]
         > Any custom created groups with effective Tier 0 access, see Tier 0 equivalency for more details.

   * En el equipo equipo\Directivas\Configuración Windows\Configuración configuración de seguridad\Directivas locales\Asignación de derechos de usuario\denegar inicio de sesión localmente, seleccione **definir esta configuración de directiva** y agregue los grupos nivel 0:     Operadores de copia de Enterprise Domain Admins administradores esquema cuenta Administradores operadores operadores Server operadores dominio controladores grupo de controladores de dominio de solo lectura directiva Creadores propietarios criptográfico operadores de impresión

         > [!NOTE]
         > Note: Built-in Tier 0 Groups, see Tier 0 equivalency for more details.

         Other Delegated Groups

         > [!NOTE]
         > Note: Any custom created groups with effective Tier 0 access, see Tier 0 equivalency for more details.

#### <a name="deploy-your-paws"></a>Implemente sus PAW.

   > [!NOTE]
   > Asegúrese de que la PAW está desconectada de la red durante el proceso de compilación del sistema operativo.

1. Instale Windows 10 con el medio de instalación de origen limpio que obtuvo anteriormente.

   > [!NOTE]
   > Puede usar Microsoft Deployment Toolkit (MDT) u otro sistema de implementación de imágenes automatizado para automatizar la implementación de PAW, pero debe asegurarse de que el proceso de compilación sea tan confiable como la PAW. Los adversarios en concreto buscan imágenes corporativas y sistemas de implementación (como ISO, paquetes de implementación, etc.) como mecanismo de continuidad, así que no se deben usar imágenes o sistemas de implementación ya existentes.
   >
   > Si automatiza la implementación de la PAW, debe seguir estos pasos:
   >
   > * Compile el sistema usando medios de instalación validados siguiendo la guía de [origen limpio para medios de instalación](https://aka.ms/cleansource).
   > * Asegúrese de que el sistema de implementación automatizado esté desconectado de la res durante el proceso de compilación del sistema operativo.

2. Establezca una contraseña compleja única para la cuenta de administrador local.  No use una contraseña que se haya utilizado para cualquier otra cuenta en el entorno.

   > [!NOTE]
   > Microsoft recomienda usar una [solución de contraseña de administrador local (LAPS)](https://www.microsoft.com/en-us/download/details.aspx?id=46899) para administrar la contraseña de administrador local en todas las estaciones de trabajo, incluidas las PAW.  Si usa LAPS, asegúrese de que solo conceda al grupo Mantenimiento de PAW el derecho a leer contraseñas administradas con LAPS para las PAW.

3. Instale Herramientas de administración remota del servidor para Windows 10 con los medios de instalación de origen limpio.
4. Configurar Windows Defender Exploit Guard

   > [!NOTE]
   > Guía de configuración para seguir

5. Conecte la PAW a la red.  Asegúrese de que la PAW pueda conectarse a al menos un controlador de dominio (DC).
6. Mediante una cuenta miembro del grupo Mantenimiento de PAW, ejecute el siguiente comando de PowerShell desde la PAW recién creada para unirla al dominio en la unidad organizativa adecuada:

   `Add-Computer -DomainName Fabrikam -OUPath "OU=Devices,OU=Tier 0,OU=Admin,DC=fabrikam,DC=com"`

   Reemplace las referencias a *Fabrikam* por su nombre de dominio, según corresponda.  Si el nombre de dominio se extiende a varios niveles (por ejemplo, child.fabrikam.com), agregue los nombres adicionales con el identificador "DC =" en el orden en que aparecen en el nombre de dominio completo del dominio.

   > [!NOTE]
   > Si ha implementado un [bosque administrativo de ESAE](https://aka.ms/esae) (para administradores de Nivel 0 en la Fase 1) o [Privileged Access Management (PAM) de Microsoft Identity Manager (MIM)](https://aka.ms/mimpamdeploy) (para administradores de Nivel 1 y 2 en la Fase 2), aquí uniría la PAW al dominio en ese entorno en lugar de al dominio de producción.

7. Antes de instalar cualquier otro software (incluidas herramientas administrativas, agentes, etc.), aplique todas las actualizaciones de Windows críticas e importantes.
8. Fuerce la aplicación de directiva de grupo.
   1. Abra un símbolo del sistema con privilegios elevados y escriba el siguiente comando: `Gpupdate /force /sync`
   2. Reinicia el equipo.

9. (Opcional) Instale las herramientas necesarias adicionales para que los administradores de Active Directory. Instale cualquier otra herramienta o script necesarios para desempeñar los deberes del puesto de trabajo. Asegúrese de evaluar el riesgo de la exposición de credenciales en los equipos de destino con alguna herramienta antes de agregarlas a una PAW. Acceda a [esta página](https://aka.ms/logontypes) para más información sobre la evaluación de las herramientas administrativas y los métodos de conexión para el riesgo de exposición de credenciales. Asegúrese de obtener todos los medios de instalación mediante la guía sobre el [origen limpio para medios de instalación](https://aka.ms/cleansource).

   > [!NOTE]
   > Mediante el uso de un servidor de salto en una ubicación central con estas herramientas, se puede reducir la complejidad aunque no sirva como límite de seguridad.

10. (Opcional) Descargue e instale software de acceso remoto necesario. Si los administradores van a usar la PAW de forma remota para la administración, instale el software de acceso remoto mediante las instrucciones de seguridad del proveedor de la solución de acceso remoto. Asegúrese de obtener todos los medios de instalación siguiendo la guía de origen limpio para medios de instalación.

    > [!NOTE]
    > Considere detenidamente todos los riesgos que implica permitir el acceso remoto a través de una PAW.  Aunque una PAW móvil permite muchos escenarios importantes, como trabajar desde casa, el software de acceso remoto puede ser vulnerable en potencia a los ataques y usarse para poner en peligro una PAW.

11. Valide la integridad del sistema PAW mediante la revisión y la confirmación de que todos los valores de configuración adecuados están implementados mediante los pasos siguientes:
    1. Confirme que solo las directivas de grupo específicas de PAW se apliquen a la PAW.
       1. Abra un símbolo del sistema con privilegios elevados y escriba el siguiente comando: `Gpresult /scope computer /r`
       2. Revise la lista resultante y asegúrese de que solo las directivas de grupo que aparecen sean las que se crearon anteriormente.
    2. Confirme que ninguna cuenta de usuario adicional sea miembro de grupos con privilegios en la PAW mediante los pasos siguientes:
       1. Abra **Editar usuarios y grupos locales** (lusrmgr.msc), seleccione **Grupos** y confirme que los únicos miembros del grupo local Administradores sean la cuenta Administrador local y el grupo de seguridad global Mantenimiento de PAW.

          > [!NOTE]
          > El grupo Usuarios de PAW no debe ser miembro del grupo Administradores local.  Los únicos miembros deben ser la cuenta Administrador local y el grupo de seguridad global Mantenimiento de PAW (y el grupo Usuarios de PAW no debe ser tampoco miembro de ese grupo global).

       2. Además, mediante **Editar usuarios y grupos locales**, asegúrese de que los siguientes grupos no tengan miembros: Los administradores de Hyper-V de operadores criptográficos de operadores de copia de seguridad de red replicadores de usuarios de escritorio remoto a los usuarios de configuración operadores Power

12. (Opcional) Si su organización usa una solución de administración (SIEM) de eventos e información de seguridad, asegúrese de que la PAW es [configurado para reenviar eventos al sistema mediante reenvío de eventos de Windows (WEF)](http://blogs.technet.com/b/jepayne/archive/2015/11/24/monitoring-what-matters-windows-event-forwarding-for-everyone-even-if-you-already-have-a-siem.aspx) o si no está registrado con el solución de forma que SIEM reciba activamente eventos e información de la PAW.  Los detalles de esta operación variarán en función de su solución SIEM.

    > [!NOTE]
    > Si su SIEM requiere un agente que funciona como una cuenta administrativa local o del sistema en las PAW, asegúrese de que las SIEM se administren con el mismo nivel de confianza que los controladores de dominio y los sistemas de identidad.

13. (Opcional) Si decide implementar LAPS para administrar la contraseña de la cuenta de administrador local en la PAW, compruebe que la contraseña se ha registrado correctamente.

    * Mediante una cuenta con permisos para leer contraseñas administradas por LAPS, abra **Equipos y usuarios de Active Directory** (dsa.msc).  Asegúrese de que estén habilitadas las características avanzadas y luego haga clic con el botón derecho en el objeto de equipo adecuado.  Seleccione la ficha Editor de atributos y confirme que el valor de msSVSadmPwd se haya rellenado con una contraseña válida.

### <a name="phase-2-extend-paw-to-all-administrators"></a>Fase 2: Extensión de la PAW a todos los administradores

Ámbito: Todos los usuarios con derechos administrativos sobre las aplicaciones críticas y las dependencias.  Aquí deben incluirse al menos los administradores de servidores de aplicaciones, las soluciones de supervisión de estado y seguridad, las soluciones de virtualización, los sistemas de almacenamiento y los dispositivos de red.

> [!NOTE]
> En las instrucciones de esta fase se supone que se ha completado totalmente la Fase 1.  No comience la fase 2 hasta que haya completado todos los pasos en la fase 1.

Cuando conforme que todos los pasos se han realizado, lleve a cabo los pasos siguientes para completar la Fase 2:

#### <a name="recommended-enable-restrictedadmin-mode"></a>(Recomendado) Habilitar **RestrictedAdmin** modo

Habilite esta característica en los servidores existentes y las estaciones de trabajo y exigir el uso de esta característica. Esta característica requiere que los servidores de destino que se esté ejecutando Windows Server 2008 R2 o posterior y tener como destino las estaciones de trabajo que se esté ejecutando Windows 7 o posterior.

1. Habilite el modo **RestrictedAdmin** en los servidores y las estaciones de trabajo siguiendo las instrucciones disponibles en esta [página](https://aka.ms/rdpra).

   > [!NOTE]
   > Antes de habilitar esta característica en los servidores accesibles desde Internet, debe tener en cuenta el riesgo de que los adversarios sean capaces de autenticarse en esos servidores con un hash de contraseña previamente robado.

2. Cree el objeto de directiva de grupo (GPO) "Se requiere RestrictedAdmin: equipo". En esta sección se crea un GPO que exige el uso del modificador /RestrictedAdmin para las conexiones salientes de Escritorio remoto, de forma que se protegen las cuentas del robo de credenciales en los sistemas de destino.
   * Vaya a Configuración de equipo\Directivas\Plantillas administrativas\Sistema\Delegación de credenciales\Restringir delegación de credenciales a servidores remotos y establezca esta opción en **Ha**.
3. Vincule el GPO **Se requiere RestrictedAdmin**: equipo a los dispositivos de Nivel 1 o Nivel 2 adecuados mediante las siguientes opciones de directiva:
   * Configuración de PAW: equipo
      * -> Ubicación del vínculo: Administrador\Nivel 0\Dispositivos (existente)
   * Configuración de PAW: usuario
      * -> Ubicación del vínculo: Administrador\nivel 0\cuentas
   * Se requiere RestrictedAdmin: equipo
      * -> Admin\Tier1\Devices o -> Admin\Tier2\Devices (ambos son opcionales)

   > [!NOTE]
   > Esto no es necesario para los sistemas de Nivel 0, ya que estos sistemas ya tienen el control completo de todos los recursos del entorno.

#### <a name="move-tier-1-objects-to-the-appropriate-ous"></a>Mover objetos de capa de 1 a las unidades organizativas adecuadas

1. Mueva los grupos de Nivel 1 a la unidad organizativa Administrador\Nivel 1\Grupos. Busque todos los grupos que conceden los siguientes derechos administrativos y muévalos a esta unidad organizativa.
   * Administrador local en más de un servidor
      * Acceso administrativo a servicios en la nube
      * Acceso administrativo a aplicaciones empresariales
2. Mueva las cuentas de Nivel 1 la unidad organizativa Administrador/Nivel 1/Cuentas. Mueva cada cuenta que sea miembro de esos grupos de Nivel 1 (incluida la pertenencia anidada) a esta unidad organizativa.
3. Adición de los miembros adecuados a los grupos pertinentes
   * **Administradores Nivel 1**: este grupo contiene los administradores de Nivel 1 con inicio de sesión limitado en los hosts de Nivel 2. Agregue todos los grupos administrativos de nivel 1 que tengan privilegios administrativos en los servidores o servicios de internet.

      > [!NOTE]
      > Si entre los deberes del personal administrativo están los de administrar recursos en varios niveles, deberá crear una cuenta de administrador separada por nivel.

4. Habilite Credential Guard para reducir el riesgo de robo y reutilización de credenciales.  Credential Guard es una nueva característica de Windows 10 que restringe el acceso de las aplicaciones a las credenciales, de forma que se impiden los ataques de robo de credenciales (como pass-the-hash).  Credential Guard es completamente transparente para el usuario y se instala de forma fácil en cuestión de minutos.  Para más información sobre Credential Guard, como los pasos de implementación y los requisitos de hardware, consulte el artículo [Proteger las credenciales de dominio derivadas con Credential Guard](https://technet.microsoft.com/library/mt483740%28v=vs.85%29.aspx).

   > [!NOTE]
   > Device Guard debe estar habilitado para poder configurar y usar Credential Guard.  Sin embargo, no es necesario configurar ninguna otra protección de Device Guard para usar Credential Guard.

5. (Opcional) Habilitar la conectividad a servicios en la nube. Este paso permite la administración de servicios en la nube como Azure y Office 365 con controles de seguridad adecuados. Este paso también es necesario en Microsoft Intune para administrar las PAW.

   > [!NOTE]
   > Si no necesita conectividad a la nube para la administración de servicios en la nube o la administración por Intune omita este paso.

   * Estos pasos restringirán la comunicación a través de Internet a solo los servicios en la nube autorizados (pero no la Internet abierta) y agregarán protecciones a los exploradores y otras aplicaciones que procesarán contenido de Internet. Nunca se deben usar estas PAW de administración para tareas de usuario estándar como productividad y comunicaciones a través de Internet.
   * Para habilitar la conectividad a servicios de PAW, siga estos pasos:

   1. Configure PAW para permitir solo destino de Internet autorizados.  Conforme extiende la implementación de PAW para habilitar la administración en la nube, debe permitir el acceso a servicios autorizados y al mismo tiempo filtrar el acceso desde la Internet abierta donde es más fácil que se monten ataques contra sus administradores.

      1. Crear **administradores de servicios en la nube** agrupar y agregar todas las cuentas que requieren acceso a servicios en la nube en internet.
      2. Descargue el archivo *proxy.pac* de PAW de la [Galería de TechNet](https://aka.ms/pawmedia) y publíquelo en un sitio web interno.

         > [!NOTE]
         > Después de descargar el archivo *proxy.pac* debe actualizarlo para asegurarse de que está actualizado y completo.  
         > Microsoft publica todas las direcciones URL de Office 365 y Azure en el [Centro de soporte técnico](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US) de Office. En estas instrucciones se supone que se usa Internet Explorer (o Microsoft Edge) para la administración de Office 365, Azure y otros servicios en la nube. Microsoft recomienda configurar restricciones similares para cualquier explorador de terceros que sea necesario para la administración. Los exploradores web de las PAW solo se deben usar para la administración de servicios en la nube y nunca para la exploración web general.
         >
         > Puede que tenga que agregar otros destinos de Internet válidos a esta lista para otros proveedores de IaaS, pero que no sean sitios de productividad, entretenimiento, noticias o búsqueda.
         >
         > Puede que también tenga que ajustar el archivo PAC para permitir una dirección de proxy válida para su uso con estas direcciones.
         >
         > También puede restringir el acceso desde la PAW mediante un proxy web para una defensa más profunda. Sin embargo, no se recomienda sin el archivo PAC dado que solo restringirá el acceso de las PAW mientras esté conectado a la red corporativa.

      3. Cuando haya configurado el archivo *proxy.pac*, actualice el GPO Configuración de PAW: usuario.
         1. Vaya a configuración de usuario\preferencias\configuración. Haga clic en registro, seleccione **New** > **elemento del registro** y configure las siguientes opciones:
            1. Acción: Reemplazar
            2. Hive: HKEY_ CURRENT_USER
            3. Ruta de acceso de clave: Software\Microsoft\Windows\CurrentVersion\Internet Settings
            4. Nombre del valor: AutoConfigUrl

               > [!NOTE]
               > No seleccione el cuadro **Valor predeterminado** situado al lado izquierdo del nombre del valor.

            5. Tipo de valor: REG_SZ
            6. Datos del valor: escriba la dirección URL completa para el *proxy.pac* archivo, incluidos http:// y el nombre de archivo: por ejemplo http://proxy.fabrikam.com/proxy.pac.  La dirección URL también puede ser una dirección URL de una sola etiqueta - por ejemplo, http://proxy/proxy.pac

               > [!NOTE]
               > El archivo PAC también se puede hospedar en un recurso compartido de archivo, con la sintaxis de file://server.fabrikan.com/share/proxy.pac, pero para ello es necesario permitir el protocolo file://. Consulte la "Nota: Sección file://-based Proxy Scripts Deprecated"de este [Understanding Web Proxy Configuration](http://blogs.msdn.com/b/ieinternals/archive/2013/10/11/web-proxy-configuration-and-ie11-changes.aspx) blog para obtener detalles adicionales sobre cómo configurar el valor del registro necesarias.

            7. Haga clic en la pestaña **Común** y seleccione **Quitar este elemento cuando ya no se aplique**.
            8. En la pestaña **Común**, seleccione **Destinatarios de nivel de elemento** y haga clic en **Destino**.
            9. Haga clic en **Nuevo elemento** y seleccione **Grupo de seguridad**.
            10. Seleccione el botón "..." y busque el **Administradores de servicios en la nube**.
            11. Haga clic en **Nuevo elemento** y seleccione **Grupo de seguridad**.
            12. Seleccione el botón "..." y busque el grupo **Usuarios de PAW**.
            13. Haga clic en el elemento **Usuarios de PAW** y haga clic en **Opciones de elemento**.
            14. Seleccione **No es**.
            15. Haga clic en **Aceptar** en la ventana de destino.
            16. Haga clic en **Aceptar** para completar la configuración de directiva de grupo **AutoConfigUrl**.
   2. Aplique las líneas de base de seguridad de Windows 10 y las líneas de base de seguridad del vínculo de acceso de servicios en la nube para Windows y para el acceso a servicios en la nube (si es necesario) a las unidades organizativas correctas mediante estos pasos:
      1. Extraiga el contenido del archivo zip de líneas de base de seguridad de Windows 10.
      2. Cree estos GPO, [importe la configuración](https://technet.microsoft.com/library/cc753786.aspx) de directiva y [cree vínculos](https://technet.microsoft.com/library/cc732979.aspx) según esta tabla. Vincule cada directiva con cada ubicación para asegurarse de que se sigue el orden de la tabla (las entradas inferiores de la tabla se deben aplicar más tarde y deben tener una prioridad más alta):

         **Directivas:**

         |||
         |-|-|
         |CM Windows 10: Seguridad de dominio|N/D: No vincular ahora|
         |SCM Windows 10 TH2: Equipo|Administrador\Nivel 0\Dispositivos|
         ||Administrador\Nivel 1\Dispositivos|
         ||Administrador\Nivel 2\Dispositivos|
         |SCM Windows 10 TH2: BitLocker|Administrador\Nivel 0\Dispositivos|
         ||Administrador\Nivel 1\Dispositivos|
         ||Administrador\Nivel 2\Dispositivos|
         |SCM Windows 10: Credential Guard|Administrador\Nivel 0\Dispositivos|
         ||Administrador\Nivel 1\Dispositivos|
         ||Administrador\Nivel 2\Dispositivos|
         |SCM Internet Explorer: Equipo|Administrador\Nivel 0\Dispositivos|
         ||Administrador\Nivel 1\Dispositivos|
         ||Administrador\Nivel 2\Dispositivos|
         |Configuración de PAW: equipo|Administrador\Nivel 0\Dispositivos (existente)|
         ||Administrador\Nivel 1\Dispositivos (nuevo vínculo)|
         ||Administrador\Nivel 2\Dispositivos (nuevo vínculo)|
         |Se requiere RestrictedAdmin: equipo|Administrador\Nivel 0\Dispositivos|
         ||Administrador\Nivel 1\Dispositivos|
         ||Administrador\Nivel 2\Dispositivos|
         |SCM Windows 10: Usuario|Administrador\Nivel 0\Dispositivos|
         ||Administrador\Nivel 1\Dispositivos|
         ||Administrador\Nivel 2\Dispositivos|
         |SCM Internet Explorer: Usuario|Administrador\Nivel 0\Dispositivos|
         ||Administrador\Nivel 1\Dispositivos|
         ||Administrador\Nivel 2\Dispositivos|
         |Configuración de PAW: usuario|Administrador\Nivel 0\Dispositivos (existente)|
         ||Administrador\Nivel 1\Dispositivos (nuevo vínculo)|
         ||Administrador\Nivel 2\Dispositivos (nuevo vínculo)|

         > [!NOTE]
         > El GPO "SCM Windows 10: Seguridad de dominio" se puede vincular al dominio con independencia de PAW, pero resultará afectado el dominio entero.

6. (Opcional) Instalar las herramientas necesarias adicionales para los administradores de nivel 1. Instale cualquier otra herramienta o script necesarios para desempeñar los deberes del puesto de trabajo. Asegúrese de evaluar el riesgo de la exposición de credenciales en los equipos de destino con alguna herramienta antes de agregarlas a una PAW. Para más información sobre cómo evaluar herramientas administrativas y métodos de conexión para el riesgo de exposición de credenciales, visite [esta página](https://aka.ms/logontypes). Asegúrese de obtener todos los medios de instalación siguiendo la guía de origen limpio para medios de instalación.
7. Identifique y obtenga de forma segura el software y las aplicaciones necesarias para la administración.  Es parecido al trabajo realizado en la Fase 1, pero con un ámbito más amplio debido al mayor número de aplicaciones, servicios y sistemas que se van a proteger.

   > [!NOTE]
   > Asegúrese de proteger estas nuevas aplicaciones (incluidos los exploradores web); para ello, en las protecciones proporcionadas por Windows Defender Exploit Guard.

   Ejemplos de aplicaciones y software adicionales son:

      * Microsoft Azure PowerShell
      * Office 365 PowerShell (también conocido como módulo de Microsoft Online Services)
      * Software de administración de aplicaciones o servicios basado en Microsoft Management Console
      * Aplicación (no basada en MMC) o software de administración de servicios en propiedad

         > [!NOTE]
         > Muchas aplicaciones se administran ahora exclusivamente mediante exploradores web, incluidos muchos servicios en la nube.  Aunque esto reduce el número de aplicaciones que se deben instalar en una PAW, también introduce el riesgo de problemas de interoperabilidad del explorador.  Es posible que tenga que implementar un explorador web que no sea de Microsoft en instancias específicas de PAW para permitir la administración de servicios específicos.  Si implementa un explorador web adicional, asegúrese de seguir todos los principios de origen limpio y de protegerlo de acuerdo con las instrucciones de seguridad del proveedor.

8. (Opcional) Descargue e instale los agentes de administración necesarios.

   > [!NOTE]
   > Si decide instalar a agentes de administración adicionales (supervisión, seguridad, administración de configuración, etc.), es fundamental asegurarse de que los sistemas de administración tengan el mismo nivel de confianza que los controladores de dominio y los sistemas de identidad. Para obtener instrucciones adicionales, consulte Administración y actualización de PAW.

9. Evalúe la infraestructura para identificar los sistemas que requieren las protecciones de seguridad adicionales que se proporcionan en una PAW.  Asegúrese de que sabe exactamente qué sistemas se deben proteger.  Formule preguntas críticas sobre los propios recursos, por ejemplo:

   * ¿Dónde están los sistemas de destino que se deben administrar?  ¿Se recopilan en una sola ubicación física o están conectados a una única subred bien definida?
   * ¿Cuántos sistemas existen?
   * ¿Dependen estos sistemas de otros sistemas (virtualización, almacenamiento, etc.)? Si es así, ¿cómo se administran esos sistemas?  ¿Cómo son los sistemas críticos expuestos a estas dependencias y cuáles son los riesgos adicionales asociados a ellas?
   * ¿Cómo de críticos son los servicios que se administran y cuál es la pérdida esperada si esos servicios están en peligro?

      > [!NOTE]
      > Incluya sus servicios en esta evaluación; los atacantes se dirigen cada vez más hacia implementaciones en la nube poco seguras, así que es de vital importancia administrar esos servicios con la misma seguridad que emplea para las aplicaciones críticas locales.

        Use esta evaluación para identificar los sistemas específicos que requieren protección adicional y luego extienda su programa PAW a los administradores de esos sistemas.  Algunos ejemplos comunes de sistemas que se benefician enormemente de la administración basada en PAW incluyen SQL Server (local y SQL Azure), aplicaciones de recursos humanos y software financiero.

        > [!NOTE]
        > Si un recurso se administra desde un sistema Windows, pueden administrarse con una PAW, incluso si la propia aplicación se ejecuta en un sistema operativo distinto de Windows o en una plataforma de nube que no es de Microsoft.  Por ejemplo, el propietario de una suscripción a Amazon Web Services solo debe usar una PAW para administrar esa cuenta.

10. Desarrolle un método de solicitud y distribución para implementar PAW a escala en su organización.  Dependiendo del número de PAW que elija implementar en la Fase 2, puede que tenga que automatizar el proceso.

    * Considere la posibilidad de desarrollar un proceso formal de solicitud y aprobación para que lo usen los administradores para obtener una PAW.  Este proceso ayudaría a: estandarizar el proceso de implementación, garantizar la responsabilidad para dispositivos PAW e identificar deficiencias en la implementación de PAW.
    * Como se indicó anteriormente, esta solución de implementación debe ser independiente de los métodos de automatización existente (que ya se han puesto en peligro) y debe seguir los principios descritos en la Fase 1.

        > [!NOTE]
        > Cualquier sistema que administra recursos debe administrarse con un nivel de confianza igual o superior.

11. Revise y, si es necesario, implemente perfiles de hardware de PAW adicionales.  El perfil de hardware que seleccionó para la implementación de la Fase 1 no puede ser adecuado para todos los administradores.  Revise los perfiles de hardware y, si es necesario, seleccione perfiles de hardware de PAW adicionales para satisfacer las necesidades de los administradores.  Por ejemplo, el perfil Hardware dedicado (independiente de PAW y de uso diario en las estaciones de trabajo) podría no ser adecuado para un administrador que viajan con frecuencia: en este caso, puede elegir implementar el perfil Uso simultáneo (PAW con VM de usuario) para ese administrador.
12. Tenga en cuenta las necesidades culturales, operativas, de comunicación y aprendizaje que acompañan a una implementación extendida de PAW.   Un cambio tan significativo en un modelo administrativo es natural que requiera la administración de cambios hasta cierto punto, así que es esencial crear esa opción en el propio proyecto de implementación.  Considere como mínimo lo siguiente:

    * ¿Cómo se comunicarán los cambios al personal directivo para garantizar su respaldo?  Cualquier proyecto sin el respaldo del personal directivo es propenso a errores o, como mínimo, a la lucha por obtener fondos y recibir la aprobación general.
    * ¿Cómo se documenta el nuevo proceso para los administradores?  Estos cambios se deben documentar y comunicarlo a los administradores existentes (que deben cambiar sus hábitos y administrar los recursos de manera diferente), sino también a los nuevos (que han ascendido internamente o se contratan de fuera de la organización).  Es fundamental que la documentación sea clara y que exprese completamente la importancia de las amenazas, el papel que desempeñan las PAW en la protección de los administradores y cómo usar PAW correctamente.

      > [!NOTE]
      > Esto es especialmente importante para roles con una rotación elevada, como los del personal del departamento de soporte técnico.

    * ¿Cómo se asegura la compatibilidad con el nuevo proceso?  Aunque el modelo PAW incluye varios controles técnicos para evitar la exposición de credenciales con privilegios, es imposible impedir completamente cualquier posible exposición solo con ellos.  Por ejemplo, aunque es posible impedir que un administrador logre iniciar sesión en el equipo de escritorio de un usuario con credenciales con privilegios, el simple hecho de intentarlo puede exponer las credenciales a malware instalado en ese equipo.  Por lo tanto, es esencial expresar no solo las ventajas del modelo PAW, sino también los riesgos de la falta de cumplimiento.  También se debería complementar con auditoría y alertas de forma que la exposición de credenciales se pueda detectar y solucionar fácilmente.

### <a name="phase-3-extend-and-enhance-protection"></a>Fase 3: Ampliar y mejorar la protección

Ámbito: Estas protecciones mejoran los sistemas creados en la fase 1 al reforzar la protección básica con características avanzadas como autenticación multifactor y reglas de acceso de red.

> [!NOTE]
> Esta fase se puede realizar en cualquier momento una vez completada la Fase 1.  No depende de la realización de la Fase 2 y, por tanto, se puede realizar antes, al mismo tiempo o después de la Fase 2.

Siga estos pasos para configurar esta fase:

1. **Habilitar la autenticación multifactor para cuentas con privilegios**.  La autenticación multifactor refuerza la seguridad de la cuenta al solicitar al usuario que proporcione un token físico además de las credenciales.  La autenticación multifactor es el complemento perfecto para las directivas de autenticación, pero no depende de las directivas de autenticación para la implementación (igualmente, las directivas de autenticación no requieren autenticación multifactor).  Microsoft recomienda usar una de estas formas de autenticación multifactor:

   * **Tarjeta inteligente**: Una tarjeta inteligente es un dispositivo físico portátil y difícil de manipular que proporciona una segunda comprobación durante el proceso de inicio de sesión de Windows.  Al exigir a una persona la posesión de una tarjeta para el inicio de sesión, puede reducir el riesgo de credenciales robadas que se reutilizan de forma remota.  Para más información sobre el inicio de sesión con tarjeta inteligente en Windows, consulte el artículo [Smart Card Overview](https://technet.microsoft.com/library/hh831433.aspx) (Información general sobre tarjetas inteligentes).
   * **Tarjeta inteligente virtual**:  Una tarjeta inteligente virtual proporciona las mismas ventajas de seguridad que las tarjetas inteligentes físicas, con la ventaja añadida de que se va a vincular a hardware específico.  Para obtener más información sobre la implementación y los requisitos de hardware, consulte los artículos, [información general de la tarjeta inteligente Virtual](https://technet.microsoft.com/library/dn593708.aspx) y [empezar a trabajar con las tarjetas inteligentes virtuales: Guía paso a paso](https://technet.microsoft.com/library/dn579260.aspx).
   * **Windows Hello para empresas**: Windows Hello para empresas permite a los usuarios autenticarse en una cuenta de Microsoft, una cuenta de Active Directory, una cuenta de Microsoft Azure Active Directory (Azure AD) o servicio que no sean de Microsoft que admite la autenticación Fast ID Online (FIDO). Después de una comprobación inicial de dos pasos durante Windows Hello para la inscripción empresarial, un Windows Hello para empresas se haya configurado en el dispositivo del usuario y el usuario establece un gesto, que puede ser Windows Hello o un PIN. Windows Hello para las credenciales de negocio son un par de claves asimétricas, que se puede generar en entornos aislados de módulos de plataforma segura (TPM).
      Para obtener más información sobre Windows Hello para la lectura de negocio [Windows Hello para empresas](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification) artículo.
   * **Microsoft Azure Multi-factor authentication**:  Microsoft Azure Multi-factor authentication (MFA) proporciona la seguridad de un segundo factor de comprobación, así como una protección mejorada a través de análisis basados en aprendizaje de máquina y supervisión.  Azure MFA puede proteger no solo a los administradores de Azure, sino también muchas otras soluciones, como aplicaciones web, Azure Active Directory y soluciones locales como acceso remoto y Escritorio remoto.  Para más información sobre la autenticación multifactor Azure, consulte [Multi-Factor Authentication](https://azure.microsoft.com/services/multi-factor-authentication).

2. **Confianza permitidas que las aplicaciones que usan Windows Defender Application Control o AppLocker**.  Al limitar la posibilidad de que se ejecute en una PAW código que no es de confianza o sin firmar, reduce la probabilidad aún más de actividad malintencionada y la exposición a peligros.  Windows incluye dos opciones principales para el control de aplicaciones:

   * **AppLocker**:  AppLocker ayuda a los administradores controlar qué aplicaciones se pueden ejecutar en un sistema determinado.  AppLocker se puede controlar centralmente mediante directivas de grupo y aplicarse a usuarios o grupos determinados (para aplicaciones que tienen como destino a usuarios de PAW).  Para más información sobre AppLocker, consulte el artículo de TechNet [Información general de AppLocker](https://technet.microsoft.com/library/hh831440.aspx).
   * **Windows Defender Application Control**: la nueva característica de Windows Defender Application Control proporciona control de la aplicación basada en hardware mejorado que, a diferencia de AppLocker, no se puede invalidar en el dispositivo afectado.  Como AppLocker, Windows Defender Application Control se puede controlar mediante directivas de grupo y destinado a usuarios específicos.  Para obtener más información sobre la restricción del uso de aplicaciones con Windows Defender Application Control, consulte [Guía de implementación de Windows Defender Application Control](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control-deployment-guide).

3. **Use usuarios protegidos, directivas de autenticación y silos de autenticación para proteger aún más las cuentas con privilegios**.  Los miembros de usuarios protegidos están sujetos a directivas de seguridad adicionales que protegen las credenciales almacenadas en el agente de seguridad local (LSA) y reducen enormemente el riesgo de robo y reutilización de credenciales.  Las directivas y los silos de autenticación controlan el modo en que los usuarios con privilegios pueden acceder a recursos del dominio.  En conjunto, estas protecciones refuerzan considerablemente la seguridad de la cuenta de estos usuarios con privilegios.  Para más información sobre estas características, consulte el artículo web [How to Configure Protected Accounts](https://technet.microsoft.com/library/dn518179.aspx) (Cómo configurar cuentas protegidas).

   > [!NOTE]
   > Estas protecciones están diseñadas para complementar, no reemplazar, las medidas de seguridad existentes en la Fase 1.  Los administradores deben seguir usando cuentas independientes para la administración y el uso general.

## <a name="managing-and-updating"></a>Administrar y actualizar

Las PAW deben tener funcionalidades antimalware y las actualizaciones de software se deben aplicar rápidamente para mantener la integridad de estas estaciones de trabajo.

También se pueden usar con las PAW funcionalidades como administración de configuración adicional, supervisión operativa y administración de la seguridad, pero la integración de estas se debe considerar con cuidado porque cada funcionalidad de administración pone en riesgo la PAW por el uso de esa herramienta. Si tiene sentido introducir funcionalidades de administración avanzadas depende de varios factores, como:

* El estado de seguridad y los procedimientos de la funcionalidad de administración (como los procedimientos de actualización de software de la herramienta, los roles administrativos y las cuentas de esos roles, los sistemas operativos en los que se hospeda la herramienta o desde los que se administra, y cualquier otra dependencia de software o hardware de esa herramienta).
* La frecuencia y la cantidad de implementaciones y actualizaciones de software en las PAW.
* Los requisitos para obtener información detallada de inventario y configuración.
* Los requisitos de supervisión de la seguridad.
* Los estándares de la organización y otros factores específicos de la organización.

Según el principio de origen limpio, todas las herramientas usadas para administrar o supervisar las PAW deben tener un nivel de confianza igual o superior al nivel de las PAW. Normalmente para ello es necesario que esas herramientas se administren desde una PAW para garantizar que no existen dependencias de seguridad de estaciones de trabajo con privilegios inferiores.

En esta tabla describen los diferentes enfoques que pueden usarse para administrar y supervisar las PAW:

|Enfoque|Consideraciones|
|------|---------|
|Predeterminado en PAW<br /><br />-   Windows Server Update Services<br />-   Windows Defender|-   Sin costo adicional<br />-   Realiza las funciones básicas de seguridad necesarias<br />-   Instrucciones incluidas en esta guía|
|Administración con [Intune](https://technet.microsoft.com/library/jj676587.aspx)|<ul><li>Proporciona control y visibilidad basados en la nube<br /><br /><ul><li>Implementación de software</li><li>Administración de actualizaciones de software</li><li>Administración de directivas de Firewall de Windows</li><li>Protección antimalware</li><li>Asistencia remota</li><li>Administración de licencias de software.</li></ul></li><li>No se necesita ninguna infraestructura de servidor</li><li>Es necesario seguir los pasos de "Habilite la conectividad a Cloud Services" de la Fase 2</li><li>Si el equipo de PAW no está unido a un dominio, será necesario aplicar las líneas de base de SCM a las imágenes locales mediante las herramientas proporcionadas en la descarga de la línea de base de seguridad.</li></ul>|
|Nuevas instancias de System Center para administrar PAW|-   Proporciona visibilidad y control de la configuración, la implementación de software y las actualizaciones de seguridad<br />-   Requiere infraestructura de servidor independiente y protegerla al nivel de las PAW, así como aptitudes del personal con altos privilegios|
|Administración de PAW con herramientas de administración existentes|-Crea un riesgo considerable en peligro las Paw a menos que se inicia la infraestructura existente de administración a nivel de seguridad de Paw **Nota:**     Microsoft lo Desaconsejo generalmente este enfoque a menos que su organización tenga una razón concreta para usarlo. En nuestra experiencia, normalmente hay un costo muy alto de poner todas estas herramientas (y sus dependencias de seguridad) hasta el nivel de seguridad de las Paw.<br />-   La mayoría de estas herramientas proporcionan visibilidad y control de la configuración, la implementación de software y las actualizaciones de seguridad|
|Herramientas de exploración o supervisión de la seguridad que requieren acceso de administrador|Incluye cualquier herramienta que instala un agente o que requiere una cuenta con acceso administrativo local.<br /><br />-   Requiere llevar la herramienta de control de seguridad al nivel de las PAW.<br />-   Puede que sea necesario reducir la postura de seguridad de las PAW para admitir la funcionalidad de la herramienta (abrir puertos, instalar Java u otro middleware, etc.) o tomar una decisión de compensación de la seguridad.|
|Administración de eventos e información de seguridad (SIEM)|<ul><li>Si SIEM no tiene agentes<br /><br /><ul><li>Puede acceder a eventos en las PAW sin acceso administrativo mediante una cuenta del grupo **Lectores del registro de eventos**</li><li>Será necesario abrir puertos de red para permitir el tráfico entrante desde los servidores SIEM</li></ul></li><li>Si SIEM requiere un agente, consulte la otra fila **Herramientas de exploración o supervisión de la seguridad que requieren acceso de administrador**.</li></ul>|
|Reenvío de eventos de Windows|-   Proporciona un método sin agente de reenviar eventos de seguridad desde las PAW hasta un colector externo o SIEM.<br />-   Puede acceder a eventos de PAW sin acceso administrativo.<br />-   No requiere la apertura de puertos de red para permitir el tráfico entrante desde los servidores SIEM.|

## <a name="operating-paws"></a>Funcionamiento de PAW

La solución PAW se debe usar siguiendo los [estándares operativos](https://aka.ms/securitystandards) que se describen en el principio de origen limpio.

## <a name="deploy-paws-using-a-guarded-fabric"></a>Implementar las Paw mediante un tejido protegido

Un [protegido fabric](https://aka.ms/shieldedvms) puede usarse para ejecutar cargas de trabajo PAW en una máquina virtual blindada en un equipo portátil o servidor de salto.
Adoptar este enfoque requiere una infraestructura adicional y pasos operativos, pero puede facilitar volver a implementar imágenes PAW a intervalos regulares y le permite consolidar varios diferentes en niveles (o clasificaciones) las Paw en máquinas virtuales que ejecutan Side-by-side en un único dispositivo.
Para obtener una explicación completa de las promesas de topología y la seguridad de tejido protegido, consulte el [documentación de tejido protegido](https://aka.ms/shieldedvms).

### <a name="changes-to-the-paw-gpos"></a>Cambios realizados en los GPO PAW

Cuando uso blindadas Paw basadas en máquinas virtuales, el [recomendada del GPO](#create-paw-configuration---computer-group-policy-object-gpo) definido anteriormente debe modificarse para admitir el uso de máquinas virtuales.

1. Crear una nueva unidad organizativa para los hosts físicos de PAW. Paw físicas y virtuales tienen distintos requisitos de seguridad y deben separarse en Active Directory según corresponda.
2. El GPO de equipo de PAW debe estar vinculado a ambos el físicos y virtuales PAW unidades organizativas.
3. Crear un nuevo GPO para la PAW agregar los usuarios PAW para el grupo de administradores de Hyper-V físico. Esto es necesario para permitir que los administradores para conectarse a la administración de máquinas virtuales y activarlas o desactivar según sea necesario. Es importante que el registro de usuario en la PAW físico no tiene derechos de administrador, acceso a internet, o la capacidad para copiar datos de la máquina virtual malintencionada de recursos compartidos de red o dispositivos de almacenamiento externo en la PAW físico.
4. Crear un nuevo GPO para la administración de máquinas virtuales para agregar los usuarios de PAW para el grupo de usuarios de escritorio remoto. Esto permitirá que los usuarios usar Hyper-V mejorado las sesiones de consola, que ofrecen una mejor experiencia de usuario y permite el acceso directo de tarjeta inteligente a la máquina virtual.

### <a name="set-up-the-host-guardian-service"></a>Configurar el servicio de protección de Host

El servicio de protección de Host es responsable de avalar la identidad y el estado de un dispositivo físico de PAW.
Solo las máquinas que se sabe que HGS y ejecución de una confianza [directiva de integridad de código](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control) pueden iniciar las máquinas virtuales blindadas.
Esto ayuda a proteger las máquinas virtuales blindadas, que se ejecutan cargas de trabajo para administrar los recursos en capas, frente a amenazas del entorno de escritorio de usuario de confianza.

Puesto que es responsable de determinar qué dispositivos pueden ejecutar máquinas virtuales de PAW HGS, se considera un recurso de nivel 0.
Debe implementar junto con otros recursos de nivel 0 y protegerse contra el acceso lógico y físico no autorizado.
HGS es un rol en clúster, lo que facilita la escalabilidad horizontal para la implementación de cualquier tamaño.
La regla general consiste en planear 1 servidor HGS por cada 1.000 dispositivos que tiene, con un mínimo de 3 nodos.

1. Para instalar el primer servidor HGS, comience con la [instalar HGS - bosque bastión](../../security/guarded-fabric-shielded-vm/guarded-fabric-install-hgs-in-a-bastion-forest.md) artículo y unir HGS a su dominio de nivel 0.

2. A continuación, [crear certificados de HGS](../../security/guarded-fabric-shielded-vm/guarded-fabric-obtain-certs.md) mediante su entidad de certificación empresarial.
Cualquier usuario en posesión de certificados de firma y el cifrado de HGS puede descifrar una máquina virtual blindada, por lo que si tiene acceso a un módulo de seguridad de Hardware para proteger las claves privadas, es conveniente generar estos certificados mediante un HSM.
Para obtener una mayor seguridad, seleccione un tamaño de clave mayor o igual a 4096 bits.

3. Por último, siga los pasos para [inicializar el servidor HGS](../../security/guarded-fabric-shielded-vm/guarded-fabric-initialize-hgs-tpm-mode-bastion.md) en **TPM modo**.
Inicialización establece la atestación y servicios web de protección de claves utilizados por sus huellas.
Debe ser HGS [configurado con un certificado TLS](../../security/guarded-fabric-shielded-vm/guarded-fabric-configure-hgs-https.md) proteger estas comunicaciones y solo el puerto 443 debería abrirse desde redes no confiables a HGS.

4. Siga los pasos para [agregar nodos adicionales](../../security/guarded-fabric-shielded-vm/guarded-fabric-configure-additional-hgs-nodes.md) para la segunda, terceros y más nodos HGS.

5. Si el servidor HGS se está ejecutando Windows Server 2019 o versiones posteriores, puede habilitar una característica opcional para almacenar en caché las claves para las máquinas virtuales blindadas en las Paw para que puedan usar sin conexión. Las claves están selladas a la configuración de seguridad actual del sistema para evitar el uso de claves en caché en otro equipo o en el mismo equipo en un estado poco seguro. Esto puede ser una solución útil si los usuarios PAW viajan sin acceso a Internet, pero aun así deben ser capaz de iniciar sesión en sus máquinas virtuales de PAW. Para usar esta característica, ejecute el siguiente comando en cualquier servidor HGS:

      ```powershell
      Set-HgsKeyProtectionConfiguration -AllowKeyMaterialCaching:$true
      ```

### <a name="set-up-the-physical-paw-device"></a>Configurar el dispositivo físico de PAW

El dispositivo físico de PAW se considera que no es de confianza de forma predeterminada en la solución de tejido protegido.
Puede demostrar que es de confianza durante el proceso de atestación, tras el cual pueden obtener las claves necesarias para iniciar una VM blindada de administración.
El dispositivo debe ser capaz de ejecutar Hyper-V y tiene el arranque seguro y un TPM 2.0 habilitado para satisfacer la [protegidos de los requisitos previos de host](../../security/guarded-fabric-shielded-vm/guarded-fabric-guarded-host-prerequisites.md).
La versión mínima del sistema operativo para admitir toda la funcionalidad de PAW es **Windows 10, versión 1803**.

La PAW física debe configurarse como cualquier otro, con la excepción de que los usuarios PAW deberá ser administradores de Hyper-V para poder activar el Administrador de la máquina virtual y conectarse a él.
En su entorno de espacio limpio, deberá crear una configuración maestra para cada combinación única de hardware y software que va a implementar como hosts protegidos para la administración de máquinas virtuales.
En cada configuración de golden, complete las tareas siguientes:

1. Instalar las actualizaciones más recientes de Windows, los controladores y firmware en el equipo, así como cualquier administración de terceros o agentes de supervisión.
2. [Capturar la información necesaria de la línea de base](../../security/guarded-fabric-shielded-vm/guarded-fabric-tpm-trusted-attestation-capturing-hardware.md), incluido el TPM identificador (clave de aprobación), arranque medidas (registro TCG) y la directiva de integridad de la máquina de código.
3. Copie estos artefactos a un servidor HGS y ejecute los comandos de atestación de HGS en el artículo anterior para registrar el host. Si todos los hosts de usar la misma directiva de integridad de código o utilizan la misma configuración de hardware, solo deberá registrar el registro TCG/directiva de integridad de código una vez.

### <a name="create-the-signed-template-disk"></a>Crear el disco plantilla firmada

Las máquinas virtuales blindadas se crean mediante discos de plantilla firmada.
La firma se comprueba en tiempo de implementación para comprobar la integridad del disco y la autenticidad antes de liberar los secretos, como la contraseña de administrador en la máquina virtual.

Para crear un disco de plantilla firmada, siga los pasos de implementación de la fase 1 en regular, máquinas virtuales de generación 2.
Esta máquina se convertirá en la imagen dorada para una máquina virtual de administración.
Puede crear más de un disco de plantilla para tener especializadas de las herramientas disponibles en diferentes contextos.

Cuando se configura la máquina virtual como lo desea, ejecute `C:\Windows\System32\sysprep\sysprep.exe` y decide **generalizar** el disco. **Apagar** el sistema operativo cuando se completa la generalización.

Por último, ejecute el [Asistente de disco de plantilla](../../security/guarded-fabric-shielded-vm/guarded-fabric-create-a-shielded-vm-template.md) en el archivo VHDX desde la máquina virtual para instalar los componentes de BitLocker y generar la firma del disco.

#### <a name="create-the-shielding-data-file"></a>Crear el archivo de datos de blindaje

El disco de plantilla generalizada se empareja con un archivo de datos de blindaje, que contiene los secretos necesarios para aprovisionar una máquina virtual blindada.
El archivo de datos de blindaje incluye:
   - Una lista de protecciones, que definen a los tejidos en donde se puede ejecutar la máquina virtual. Cada clúster HGS es una protección para los dispositivos PAW que protege.
   - Una lista de firmas de disco de confianza para la implementación. Archivos de datos de blindaje sólo liberará sus secretos para las máquinas virtuales creadas mediante los medios de origen autorizado.
   - Una directiva de seguridad que determina si se deben colocar las protecciones adicionales para proteger la máquina virtual desde el host y si la máquina virtual se puede mover a otra máquina. Las máquinas virtuales del Administrador de PAW debe siempre totalmente protegido.
   - El archivo unattend.xml especialización, lo que permite que Windows completar la instalación automáticamente incluye secretos, como la contraseña del administrador local.
   - Archivos adicionales, como certificados RDP o VPN.

Consulte la [artículo del archivo de datos de blindaje](../../security/guarded-fabric-shielded-vm/guarded-fabric-tenant-creates-shielding-data.md) para obtener información sobre cómo crear un archivo de datos de blindaje.

Las claves de propietario de las máquinas virtuales blindadas son extremadamente sensibles y deben mantenerse en un HSM o sin conexión almacenadas en una ubicación segura.
Puede utilizarse en un escenario de vidrio de interrupción de emergencia para arrancar una máquina virtual blindada sin la presencia de HGS.

Se recomienda encarecidamente que los datos para la administración PAW de blindaje máquinas virtuales incluyen la configuración para bloquear una máquina virtual para el primer host físico donde se arranca.
Esto evitará que alguien de la migración de una máquina virtual de administración desde una PAW a otro PAW en el mismo entorno.
Para usar esta característica, cree el archivo de datos de blindaje con PowerShell e incluyen el **BindToHostTpm** parámetro:

```powershell
New-ShieldingDataFile -Policy Shielded -BindToHostTpm [...]
```

#### <a name="deploy-an-admin-vm"></a>Implementar una máquina virtual de administración

Una vez que el disco de plantilla y el archivo de datos de blindaje están listos, puede implementar un administrador de máquina virtual en cualquier PAW que se registró con HGS.

1. Copie el disco de plantilla (.vhdx) y el archivo de datos de blindaje (. PDK) en un dispositivo de confianza PAW.
2. Siga las instrucciones para [implementar una nueva máquina virtual blindada mediante PowerShell](../../security/guarded-fabric-shielded-vm/guarded-fabric-create-a-shielded-vm-using-powershell.md)
3. Complete los pasos restantes en la fase 1 del proceso de implementación para proteger el sistema operativo de máquina virtual y configurarlo para su rol deseado, según corresponda.


## <a name="related-topics"></a>Temas relacionados

[Servicios de ciberseguridad de Microsoft](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx)

[Taste of Premier: Cómo mitigar Pass-the-Hash y otras formas de robo de credenciales](https://channel9.msdn.com/Blogs/Taste-of-Premier/Taste-of-Premier-How-to-Mitigate-Pass-the-Hash-and-Other-Forms-of-Credential-Theft)

[Microsoft Advanced Threat Analytics](https://aka.ms/ata)

[Proteger las credenciales de dominio derivadas con Credential Guard](https://technet.microsoft.com/library/mt483740%28v=vs.85%29.aspx)

[Información general sobre la protección de dispositivos](https://technet.microsoft.com/library/dn986865(v=vs.85).aspx)

[Protección de activos de gran valor con estaciones de administración seguras](https://msdn.microsoft.com/library/mt186538.aspx)

[Modo de usuario aislado en Windows 10 con Dave Probert (Channel 9)](https://channel9.msdn.com/Blogs/Seth-Juarez/Isolated-User-Mode-in-Windows-10-with-Dave-Probert)

[Aislamiento de los procesos en modo usuario y las características de Windows 10 con Logan Gabriel (Channel 9)](http://channel9.msdn.com/Blogs/Seth-Juarez/Isolated-User-Mode-Processes-and-Features-in-Windows-10-with-Logan-Gabriel)

[Más sobre los procesos y características en el modo de usuario aislado de Windows 10 con Dave Probert (Channel 9)](https://channel9.msdn.com/Blogs/Seth-Juarez/More-on-Processes-and-Features-in-Windows-10-Isolated-User-Mode-with-Dave-Probert)

[Mitigación de robo de credenciales mediante el modo de usuario aislado (Channel 9) de Windows 10](https://channel9.msdn.com/Blogs/Seth-Juarez/Mitigating-Credential-Theft-using-the-Windows-10-Isolated-User-Mode)

[Habilitar la validación estricta de KDC en Kerberos de Windows](https://www.microsoft.com/en-us/download/details.aspx?id=6382)

[Novedades de la autenticación Kerberos para Windows Server 2012](https://technet.microsoft.com/library/hh831747.aspx)

[Comprobación del mecanismo de autenticación para AD DS en la Guía paso a paso de Windows Server 2008 R2](https://technet.microsoft.com/library/dd378897(v=ws.10).aspx)

[Módulo de plataforma segura](C:/sd/docs/p_ent_keep_secure/p_ent_keep_secure/trusted_platform_module_technology_overview.xml)
