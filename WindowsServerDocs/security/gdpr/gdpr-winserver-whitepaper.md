---
title: "A partir de su viaje normativa de protección de datos General (GDPR) para Windows Server 2016"
description: "Usa este artículo para saber lo que te GDPR y acerca de los productos Microsoft proporciona para ayudarte a empezar a hacia el cumplimiento."
keywords: Privacidad, GDPR
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.technology:
- techgroup-security
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 09/25/2017
ms.author: nirb
ms.openlocfilehash: e68b8d926f7749f7fcecf5f3752822c54db5ce76
ms.sourcegitcommit: c5aa1eedd1383b8b8f6b8fcb0af995c28277002a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2018
---
# <a name="beginning-your-general-data-protection-regulation-gdpr-journey-for-windows-server"></a>A partir de su viaje normativa de protección de datos General (GDPR) para Windows Server 

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Este artículo proporciona información sobre la GDPR, como qué es, y los productos Microsoft que se proporciona para ayudarte a ser compatibles.

## <a name="introduction"></a>Introducción
En el 25 de mayo de 2018, una ley de privacidad europeas es debido a efectivas que establece una nueva barra global de derechos de privacidad, la seguridad y la conformidad.

La normativa General de protección de datos o GDPR, es esencialmente sobre protección y habilitar los derechos de privacidad de las personas. La GDPR establece los requisitos de privacidad estrictas de global que rigen cómo administrar y proteger los datos personales respetando selección individual, sin importar dónde se envían, procesa o almacena datos.

Microsoft y nuestros clientes se encuentran ahora en un viaje a lograr los objetivos de privacidad de la GDPR. En Microsoft, creemos privacidad es un derecho fundamental, y creemos que la GDPR es un importante paso adelante para aclarar y habilitar los derechos de privacidad individuales. Pero también sabemos que el GDPR requerirá cambios significativos por las organizaciones de todo el mundo.

Describimos nuestro compromiso con la GDPR y cómo nos estamos dar soporte a nuestros clientes dentro de la [GDPR obtener compatible con la Microsoft Cloud](https://blogs.microsoft.com/on-the-issues/2017/02/15/get-gdpr-compliant-with-the-microsoft-cloud/#hv52B68OZTwhUj2c.99) por nuestro director de privacidad de entrada de blog [Brendon Lynch](https://blogs.microsoft.com/on-the-issues/author/brendonlynch/) y [Earning tu confianza con los compromisos contractuales a la normativa General de protección de datos](https://blogs.microsoft.com/on-the-issues/2017/04/17/earning-trust-contractual-commitments-general-data-protection-regulation/#6QbqoGWXCLavGM63.99)"entrada de blog por [Rich Sauer](https://blogs.microsoft.com/on-the-issues/author/rsauer/) -vicepresidente corporativo de Microsoft y asesor adjunto.

Aunque su viaje a GDPR cumplimiento puede parecer un desafío, estamos aquí para ayudarte. Para obtener información específica sobre la GDPR, nuestro compromiso y cómo comenzar a su viaje, visite la [sección GDPR del Microsoft Trust Center](https://www.microsoft.com/en-us/trustcenter/privacy/gdpr).

## <a name="gdpr-and-its-implications"></a>GDPR y sus implicaciones
El GDPR es una normativa compleja que probablemente requieran cambios significativos en cómo recopilar, usar y administrar los datos personales. Microsoft tiene una larga historia de ayudar a nuestros clientes cumplir las reglamentaciones complejas, y en cuanto a la preparación para la GDPR, estamos su compañero este camino.

La GDPR impone las reglas en las organizaciones que ofrecen bienes y servicios a las personas de la Unión Europea (UE), o que recopilan y analizar los datos vinculados a los residentes de la Unión Europea, independientemente de dónde se encuentren esas empresas. Entre los elementos clave de la GDPR son los siguientes:

- **Derechos de privacidad personal mejoradas.** Refuerza la protección de datos para los residentes de la UE asegurándose de que tienen derecho a acceder a sus datos personales a imprecisión correcta en esos datos, para borrar los datos, al objeto de procesamiento de sus datos personales y para moverlo.

- **Derecho mayor para proteger los datos personales.** Reforzada responsabilidad de las organizaciones que procesan los datos personales, proporcionar una mayor claridad de responsabilidad de cumplimiento.

- **Informe de infracción obligatorio datos personales.** Las organizaciones que controlan los datos personales son necesarias para las infracciones de los datos personales de informe que suponen un riesgo para los derechos y libertades de las personas a sus autoridades de control mayor brevedad y, cuando sea posible, ya no posterior 72 horas una vez estén cuenta de la infracción.

Como es posible que prever, la GDPR puede tener un impacto significativo en tu empresa, potencialmente necesidad de actualizar las directivas de privacidad, implementar y reforzar los controles de protección de datos e incumple los procedimientos de notificación, implementar las directivas de muy transparentes, y aún más invertir en TI y formación. Microsoft Windows 10 puede ayudarte de forma eficaz y eficaz solucionar algunos de estos requisitos.

## <a name="personal-and-sensitive-data"></a>Datos personales y con información confidencial
Como parte de tus esfuerzos para cumplir con la GDPR, debes comprender cómo la normativa define los datos personales y con información confidencial y cómo se relacionan esas definiciones de los datos mantenidos por su organización. Se basa en que comprensión, podrás descubrir donde dichos datos se crean, procesa, administra y almacena.

La GDPR considera los datos personales que cualquier información relacionada con una persona física identificada o identificable. Que puede incluir tanto identificación directa (por ejemplo, la razón social) y la identificación indirecta (como por ejemplo, información específica que hace que desactivarla es las referencias de datos). El GDPR también aclara que el concepto de datos personales incluye identificadores en línea (por ejemplo, direcciones IP, identificadores de dispositivos móviles) y datos de ubicación.

La GDPR presenta definiciones específicas para los datos biométricos y datos genéticos (por ejemplo, la secuencia de gene de una persona). Datos genéticas y los datos biométricos junto con otros sub categorías de datos personales (datos personales que revelen raza o étnico, las opiniones, convicciones religiosas o filosóficas o pertenencia de unión de comercio: datos relativos a la salud; o datos relacionados con un vida sexo de la persona o la orientación sexual) se tratan como datos personales con información confidencial en la GDPR. Datos personales con información confidencial se contemplados protecciones mejoradas y por lo general, requiere consentimiento explícito de una persona dónde están estos datos para ser procesados.

### <a name="examples-of-info-relating-to-an-identified-or-identifiable-natural-person-data-subject"></a>Ejemplos de información sobre una persona natural identificada o identificable (asunto de datos)
Esta lista proporciona algunos ejemplos de los distintos tipos de información que se regular a través de GDPR. Esto no es una lista exhaustiva.

-   Nombre

-   Número de identificación (por ejemplo, NSS)

-   Datos de ubicación (por ejemplo, dirección particular)

-   Identificador en línea (por ejemplo, dirección de correo electrónico, nombres de pantalla, la dirección IP, identificadores de dispositivos)

-   Datos pseudónimo se registró (por ejemplo, usando una clave para identificar a individuos)

-   Datos genéticos (por ejemplo, biológicos muestras de una persona)

-   Datos biométricos (por ejemplo, huellas digitales, reconocimiento facial)

## <a name="getting-started-on-the-journey-towards-gdpr-compliance"></a>Tareas iniciales en el camino hacia el cumplimiento de GDPR
Dada la cantidad es complicado sea compatible con GDPR, te recomendamos encarecidamente que no esperes a preparar hasta que comience la aplicación. Debes revisar ahora tu privacidad y procedimientos de administración de datos. Te recomendamos que comenzar su viaje a cumplimiento de GDPR centrándose en cuatro pasos principales:

-   **Descubre.** Identificar qué datos personales que tienes y donde reside. 

-   **Administrar.** Se rige por los datos personales en cómo se utiliza y se accede.

-   **Proteger.** Establecer los controles de seguridad para evitar, detectar y responder a vulnerabilidades y las infracciones de datos.  

-   **Informe.** Respondan a solicitudes de datos, violaciones de datos de informe y mantener la documentación necesaria.

    ![Diagrama sobre cómo funcionan conjuntamente los 4 pasos GDPR clave](../media/GDPR-Windows-Server-Overview/gdpr-steps-diagram.png)

Cada uno de los pasos, hemos trazado herramientas de ejemplo, recursos y características en distintas soluciones de Microsoft, que pueden usarse para ayudar a cumplir los requisitos de este paso. Mientras este artículo no es una guía completa "instrucciones", que hemos incluido vínculos para que puedas conocer más detalles y obtener más información está disponible en la [sección GDPR del Microsoft Trust Center](https://www.microsoft.com/en-us/trustcenter/privacy/gdpr).

## <a name="windows-server-security-and-privacy"></a>Privacidad y seguridad de Windows Server
El GDPR requiere que implementar medidas de seguridad técnicos y organizativos apropiadas para proteger los datos personales y sistemas de procesamiento. En el contexto de la GDPR, los entornos de servidor físicas y virtuales potencialmente están procesando los datos personales y con información confidencial. Procesamiento puede significar cualquier operación o un conjunto de operaciones, como la recopilación de datos, almacenamiento y recuperación.

Tu capacidad para cumplir este requisito e implementar medidas de seguridad técnicas adecuadas debe reflejar de las amenazas que enfrentan en cada vez más hostil entorno de TI actual. El panorama de amenazas de seguridad de hoy es una de las amenazas agresivas y tenaces. En años anteriores, los atacantes malintencionados se centraban principalmente en obtener el reconocimiento de la Comunidad a través de sus ataques o la emoción de desconectar sistemas temporalmente sin conexión. Desde entonces, la intención del atacante ha pasado a ganar dinero, incluidos mantiene los dispositivos y la retención de datos hasta que el propietario pague el rescate solicitado.

Los ataques modernos se centran cada vez más en el robo de propiedad intelectual a gran escala; degradación del sistema de destino que puede ocasionar pérdidas financieras y ahora, incluso el ciberterrorismo, que amenaza la seguridad de personas, empresas e intereses nacionales en todo el mundo. Estos atacantes suelen ser individuos con una alta formación y expertos en seguridad, algunos de los cuales están contratados por Estados nación con grandes presupuestos y recursos humanos aparentemente ilimitados. Amenazas como estos requieren un enfoque que pueda cumplir este desafío.

No solo son un riesgo para la capacidad de mantener el control de los datos personales o confidenciales, es posible que tengas de estas amenazas, pero también son un riesgo material para tu negocio global. Ten en cuenta los datos más recientes de McKinsey, Ponemon Institute, Verizon y Microsoft:

- El costo promedio del tipo de la GDPR esperarán que informe de fuga de datos es $3. 5M.

- 63% de estos infracciones implican débiles o robadas contraseñas que la GDPR espera a dirección.

- Más de 300.000 nuevas muestras de malware se crean y separa cada día en hacer que la tarea a la protección de datos de dirección aun más exigentes.

Tal como se muestra con los ataques Ransomware recientes, llamado una vez el negro plagas de Internet, los atacantes va después de unos objetivos más grandes que se pueden permitir que pagar más con consecuencias potencialmente dañinas. La GDPR incluye sanciones que hacen que los sistemas, incluidos equipos de escritorio y portátiles, que contienen efectivamente destinos enriquecida de los datos personales y con información confidencial.

Dos principios clave han interactiva y continuarán guiar el desarrollo de Windows:

- **Seguridad.** Los datos de que nuestro software y servicios de la tienda en nombre de nuestros clientes deben protegerse frente a daños y usados o modificados solo de forma adecuada. Modelos de seguridad deben ser fáciles para los desarrolladores comprender y crear en sus aplicaciones.

- **Privacidad.** Los usuarios deben tener control de cómo se usan sus datos. Las directivas de uso de la información deben distinguirse claramente al usuario. Los usuarios deben tener control de cuando y si reciben información para hacer mejor uso de su tiempo. Debe ser fácil a los usuarios especificar el uso adecuado de la información que se envían, incluyendo controlar el uso del correo electrónico.

Microsoft ha permanecido steadfast contra estos principios recientemente como cuenta, director general de Microsoft de Satya Nadella, 

> "_Continúa cambiar el mundo y evolucionan los requisitos empresariales, algunas de las cosas son coherentes: petición de un cliente de seguridad y privacidad. _"

Mientras trabaja para cumplir con la GDPR, descripción de la función de los servidores físicas y virtuales de creación, acceso, procesamiento, almacenar y administrar los datos que puedan considerarse personales y datos potencialmente sensibles bajo la GDPR están importantes. Windows Server proporciona funcionalidades que te ayudará a cumplan con los requisitos de GDPR medidas de seguridad técnicos y organizativos apropiadas para proteger los datos personales.

La posición de seguridad de Windows Server 2016 no es un rayo en; es un principio de arquitectura. Y se puede entender mejor en cuatro principales:

- **Proteger.** El enfoque en curso e innovación en medidas preventivas; bloquear ataques conocidos y malware conocido.

- **Detectar.** Completos de herramientas para ayudarte a anomalías manchas y responder a los ataques más rápidos de supervisión.

- **Responder.** Líder de respuesta y la recuperación tecnologías más profundos conocimientos técnicos de consultoría.

- **Aislar.** Aislar secretos de datos y componentes del sistema operativo, limitar los privilegios de administrador y rigurosamente medir el estado de host.

Con Windows Server, se ha mejorado la capacidad para proteger, detectar y defenderse de los tipos de ataques que pueden dar lugar a las infracciones de datos. Proporciona los requisitos estrictos alrededor de notificación de una infracción en la GDPR, garantizar que los sistemas de escritorio y portátiles se protegían bien se reducirán los riesgos que se enfrenta que podrían ocasionar notificación y análisis de incumplimiento costosa.

En la sección siguiente, podrás ver cómo Windows Server proporciona funciones que se ajusten exactamente en la fase de "Proteger" de tu viaje de cumplimiento GDPR. Estas funcionalidades se dividen en tres escenarios de protección:

- **Proteger las credenciales y limita los privilegios de administrador.** Windows Server 2016 ayuda a implementar estos cambios, para ayudar a impedir que el sistema se usa como un punto de partida para intrusiones aún más.

- **Proteger el sistema operativo para ejecutar las aplicaciones y la infraestructura.** Windows Server 2016 proporciona niveles de protección, lo que ayuda a bloquear los atacantes externos ejecuten software malintencionado o aprovechar las vulnerabilidades.

- **Virtualización segura.** Windows Server 2016 permite la virtualización segura, con máquinas virtuales de blindado y Fabric protegido. Esto ayuda a cifrar y ejecutar las máquinas virtuales en hosts de confianza en tu fabric, mejor protección frente a ataques malintencionados.

Estas funcionalidades, que se explica con más detalle a continuación con referencias a los requisitos específicos de GDPR, están creadas sobre protección avanzadas del dispositivo que ayuda a mantener la integridad y la seguridad del sistema operativo y los datos.

Una clave aprovisionar dentro de la GDPR es protección de datos por diseño y de manera predeterminada y ayudar con su capacidad para satisfacer esta disposición son características de Windows 10 como dispositivo de BitLocker. BitLocker usa la tecnología de módulo de plataforma segura (TPM), que proporciona funciones relacionadas con la seguridad basada en hardware. Este chip de procesador de criptografía incluye varios mecanismos de seguridad física para que sea resistente a las alteraciones y software malintencionado no puede altere las funciones de seguridad del TPM.

El chip incluye varios mecanismos de seguridad física para que sea resistente a las alteraciones y software malintencionado no puede altere las funciones de seguridad del TPM. Algunas de las principales ventajas de usar la tecnología TPM son que puede:

-   Generar, almacenar y limitar el uso de claves criptográficas.

-   Usar la tecnología TPM para la autenticación de dispositivo de plataforma mediante el uso de la clave RSA exclusiva del TPM, que se grabará en Sí.

-   Ayudar a garantizar la integridad de la plataforma llevando y almacenando medidas de seguridad.

Protección adicional avanzadas del dispositivo relevante para el funcionamiento sin las infracciones de datos incluyen el arranque seguro de Windows para ayudar a mantener la integridad del sistema asegurándose de malware es no se puede iniciar antes las defensas del sistema.

## <a name="windows-server-supporting-your-gdpr-compliance-journey"></a>Windows Server: Compatibilidad con su viaje de cumplimiento GDPR
Características clave de Windows Server pueden ayudarte de forma eficaz y eficiente implementar los mecanismos de seguridad y privacidad que requiere la GDPR cumplimiento. Mientras el uso de estas funciones no garantiza la conformidad, se admiten los esfuerzos de hacerlo.

El sistema operativo de servidor se encuentra en una capa estratégica en la infraestructura de la organización, que nuevas oportunidades para crear niveles de protección frente a ataques que podrían robar los datos e interrumpir tu negocio. Aspectos clave de la GDPR como privacidad por diseño, protección de datos y el Control de acceso tienen que abordarse en la infraestructura de TI en el nivel de servidor.

Trabaja para ayudar a proteger la identidad, el sistema operativo y los niveles de virtualización, Windows Server 2016 ayuda a bloquear los vectores de ataque comunes usados para obtener acceso ilícito a los sistemas: robo de credenciales, malware y una estructura de virtualización en peligro. Además de reducir el riesgo de empresas, los componentes de seguridad habían integrado en Ayuda de Windows Server 2016 requisitos de cumplimiento de dirección para la administración de clave pública y del sector las normas de seguridad. 

Estos identidad, el sistema operativo y la protección de virtualización le permiten proteger mejor su centro de datos que ejecutan Windows Server como una máquina virtual en cualquier nube y limitar la capacidad de los atacantes para poner en peligro las credenciales, iniciar el software malintencionado y permanecer en la sombra en tu red. De igual modo, cuando se implementa como un host de Hyper-V, Windows Server 2016 ofrece garantía de seguridad de los entornos de virtualización a través de máquinas virtuales de blindado y capacidades de firewall distribuido. Con Windows Server 2016, el sistema operativo de servidor se convierte en un participante activo en el centro de datos de seguridad.

### <a name="protect-your-credentials-and-limit-administrator-privileges"></a>Proteger las credenciales y limita los privilegios de administrador 
Control sobre el acceso a datos personales y los sistemas que procesan estos datos, que es un área con la GDPR que tiene requisitos específicos, incluido el acceso a los administradores. Identidades con privilegios son todas las cuentas que privilegios, como las cuentas de usuario que son miembros de los administradores de dominio, los administradores de empresa, los administradores locales o incluso los grupos de usuarios avanzados elevados. Tales identidades también pueden incluir cuentas que se han concedido privilegios directamente, por ejemplo, realizar copias de seguridad, apagar el sistema, o a otros derechos que se enumeran en el nodo de asignación de derechos de usuario en la consola de directiva de seguridad Local.

Como principio de control de acceso general y en línea con el GDPR, debes proteger estas identidades con privilegios de compromiso por los posibles atacantes. En primer lugar, es importante comprender cómo se pongan en riesgo de identidades; a continuación, planear evitar que los atacantes obtengan acceso a estas identidades con privilegios.

#### <a name="how-do-privileged-identities-get-compromised"></a>¿Cómo compromete identidades con privilegios?
Cuando las organizaciones no tienen directrices para protegerlos pueden compromete identidades con privilegios. Estos son algunos ejemplos:

- **Privilegios más de los necesarios.** Uno de los problemas más comunes es que los usuarios tengan privilegios más de los necesarios para llevar a cabo su función de trabajo. Por ejemplo, un usuario que administra DNS podría ser un administrador de AD. A menudo, esto se hace para evitar la necesidad de configurar los distintos niveles de administración. Sin embargo, si dicha cuenta se ve comprometida, el atacante automáticamente tiene privilegios elevados.

- **Constantemente iniciado sesión con privilegios elevados.** Otro problema común es que los usuarios con privilegios elevados pueden usarla durante un tiempo ilimitado. Esto es muy común con los profesionales de TI que iniciar sesión en un equipo de escritorio con una cuenta con privilegios, mantenían la sesión iniciada y usan la cuenta con privilegios para navegar por Internet y usar el correo electrónico (trabajo típico de TI funciones de trabajo). Duración de un número ilimitada de cuentas con privilegios hace que la cuenta más susceptibles de sufrir ataques y aumenta las probabilidades de que se vea comprometida la cuenta.

- **Investigación de ingeniería social.** La mayoría de las amenazas de credenciales empezar por investigar la organización y, a continuación, se realizaron a través de ingeniería social. Por ejemplo, un atacante puede realizar un ataque de suplantación de identidad de correo electrónico compromiso legítimos cuentas (pero no necesariamente elevadas cuentas) que tienen acceso a la red de una organización. El atacante, a continuación, usa estas cuentas válidas para realizar investigación adicional en la red y para identificar las cuentas con privilegios que pueden realizar tareas administrativas. 

- **Saca provecho de las cuentas con privilegios elevados.** Incluso con una cuenta de usuario normal, sin privilegios elevados en la red, los atacantes pueden acceder a las cuentas con permisos elevados. Por lo tanto, uno de los métodos más comunes de hacerlo es mediante el Pass-the-Hash o los ataques Pass-the-Token. Para obtener más información sobre el Pass-the-Hash y otras técnicas de robo de credenciales, consulta los recursos en el [Pass-the-Hash (PtH)](https://technet.microsoft.com/en-us/dn785092.aspx).

Por supuesto, hay otros métodos que los atacantes pueden usar para identificar y poner en peligro identidades con privilegios (con nuevos métodos que se crean todos los días). Por lo tanto, es importante que debes establecer los procedimientos recomendados para los usuarios inicien sesión con cuentas de privilegios mínimos para reducir la capacidad de los atacantes para obtener acceso a las identidades con privilegios. Las siguientes secciones describen funcionalidad donde Windows Server mitigar estos riesgos.

#### <a name="just-in-time-admin-jit-and-just-enough-admin-jea"></a>Administrador de Just-in-Time (JIT) y suficientes administrador (JEA)
Mientras la protección contra Pass-the-Hash o los ataques Pass-the-Ticket es importante, las credenciales de administrador aún pueden obtenerse por otros medios, como ingeniería social, los empleados antiguos y ataques de fuerza bruta. Por lo tanto, además de aislar credenciales tanto como sea posibles, también te convenga una forma de limitar el alcance de privilegios de administrador en caso de que se pongan en riesgo.

En la actualidad, muchas cuentas de administrador se con demasiados privilegios, incluso si tienen solo un área de responsabilidad. Por ejemplo, un administrador DNS, que requiere un conjunto muy estrecho de privilegios para administrar los servidores DNS, a menudo se concede privilegios de administrador de dominio. Además, dado que estas credenciales se conceden para siempre, no hay ningún límite de cuánto tiempo pueden usarse.

Cada cuenta con privilegios de administrador de dominio innecesarios aumenta la exposición a los atacantes que deseen poner en peligro las credenciales. Para minimizar la superficie de ataque, quieres proporcionar solo el conjunto específico de los derechos que un administrador debe realizar el trabajo, y solo para la ventana de tiempo necesario para completarlo.

Con solo lo suficientemente administración y Just-in-Time administración, los administradores pueden solicitar los privilegios específicas que necesitan para la ventana exacta de tiempo requerido. Para que un administrador DNS, por ejemplo, mediante PowerShell para permitir que solo lo suficientemente administración te permite crear un conjunto limitado de comandos que están disponibles para la administración de DNS.

Si el administrador DNS necesita realizar una actualización en uno de sus servidores, podría solicitar acceso a administrar DNS mediante Microsoft Identity Manager 2016. El flujo de trabajo de la solicitud puede incluir un proceso de aprobación, como la autenticación de dos factores, que podría llamar a móvil del administrador para confirmar su identidad antes de conceder los privilegios solicitados. Una vez concedido, esos privilegios DNS proporcionan acceso a la función de PowerShell de DNS para un intervalo de tiempo específico.

Supongamos que este escenario si se roban las credenciales del administrador DNS. En primer lugar, dado que las credenciales no tienen privilegios de administrador que se adjunta a ellos, el atacante sería capaz de obtener acceso al servidor DNS – u otros sistemas: hacer ningún cambio. Si el atacante ha intentado solicitar privilegios para el servidor DNS, autenticación de factor de segundo podría pedirle para confirmar su identidad. Dado que no es probable que el atacante tenga móvil del administrador DNS, se producirá un error en la autenticación. Esto sería bloquear el atacante fuera del sistema y la organización de TI de alerta que podrían estar en riesgo las credenciales.

Además, muchas organizaciones usan la versión gratuita [solución de contraseña de administrador Local (PARCIALES)](http://aka.ms/laps) como un mecanismo de administración de JIT sencillo pero eficaces para sus sistemas de servidor y cliente. La funcionalidad de VUELTAS proporciona administración de contraseñas de cuentas locales de los equipos unidos a un dominio. Las contraseñas se almacenan en Active Directory (AD) y protegidas por y lista de Control de acceso (ACL) por lo tanto, los usuarios solo pueden leer o solicitar el restablecimiento de su.

Como se explicó en la [Guía de mitigación de Windows credenciales robo](https://www.microsoft.com/en-us/download/confirmation.aspx?id=54095), 

> "_criminales de herramientas y técnicas que se usan para llevar a cabo el robo de credenciales y mejoran la reutilización de ataques, los atacantes malintencionados les resulta más fácil alcanzar sus objetivos. Robo de credenciales a menudo se basa en procedimientos operativos o exposición de credenciales de usuario, por lo que mitigaciones eficaces requieren un enfoque holístico que abarque personas, procesos y tecnología. Además, estos ataques se basan en el atacante robar credenciales después de poner en peligro un sistema para expandir o conservar el acceso, por lo que las organizaciones deben contener infracciones rápidamente implementando estrategias que impide que los atacantes mover libremente y no ser detectado en una red en peligro. _"

Un aspecto importante del diseño para Windows Server mitigación de robo de credenciales, en particular, las credenciales derivadas. Credential Guard ofrece mejoras significativas en seguridad contra el robo de credenciales derivadas y reutilización por implementar un importante cambio de arquitectura de Windows diseñada para ayudar a eliminar los ataques de aislamiento basado en hardware en lugar de intentar simplemente Defender contra ellos.

Mientras que con Windows Defender Credential Guard, NTLM y Kerberos credenciales derivadas están protegidas con la seguridad basada en virtualización, técnicas de ataques de robo de credenciales y herramientas que se usan en muchos ataques dirigidos están bloqueados. Ejecución de malware en el sistema operativo con privilegios administrativos no puede extraer secretos que están protegidos por la seguridad basada en virtualización. Aunque Credential Guard de Windows Defender es una mitigación eficaz, ataques de amenazas persistentes probablemente cambiarán por nuevas técnicas de ataque y también deberías incorporar a Device Guard, tal como se describe a continuación, junto con otras arquitecturas y estrategias de seguridad.

#### <a name="windows-defender-credential-guard"></a>Windows Defender Credential Guard
Windows Defender Credential Guard usa seguridad basada en virtualización para aislar la información de credenciales, impide que se intercepte vales Kerberos de hash de contraseña. Usa un totalmente nuevo proceso aislado autoridad de seguridad Local (LSA), que no es accesible para el resto del sistema operativo. Todos los archivos binarios que usan la LSA aislado se firman con certificados que se validan antes de iniciar a ellos en el entorno protegido, realizar ataques de tipo Pass-the-Hash ineficaz.

Windows Defender Credential Guard usa:

- Seguridad de virtualización (necesaria). También es necesario:

    - CPU de 64 bits

    - Extensiones de virtualización de CPU, además de tablas de página extendida

    - Hipervisor de Windows

- Arranque seguro (necesario)

- TPM 2.0 bien discreto o de firmware (preferido: proporciona el enlace de hardware)

Puedes usar a Credential Guard de Windows Defender para ayudar a proteger las identidades con privilegios protege las credenciales y derivados de credenciales en Windows Server 2016. Para obtener más información sobre los requisitos de Windows Defender Credential Guard, consulta [proteger las credenciales de dominio con Credential Guard de Windows Defender](https://docs.microsoft.com/en-us/windows/access-protection/credential-guard/credential-guard).

#### <a name="windows-defender-remote-credential-guard"></a>Windows Defender remoto Credential Guard
Windows Defender remoto Credential Guard en Windows Server 2016 y actualización de aniversario de Windows 10 también ayuda a proteger las credenciales de los usuarios con conexiones a Escritorio remoto. Anteriormente, cualquier persona que use los servicios de escritorio remoto tendría que iniciar sesión en su equipo local y, a continuación, se requiere para iniciar sesión nuevamente cuando realizan una conexión remota a su equipo de destino. Este inicio de sesión segunda superara credenciales para el equipo de destino, que se expone a Pass-the-Hash o los ataques Pass-the-Ticket.

Con Windows Defender remoto Credential Guard, Windows Server 2016 implementa un inicio de sesión único para las sesiones de escritorio remoto, eliminan la necesidad de volver a escribir tu nombre de usuario y contraseña. En su lugar, aprovecha las credenciales que ya has usado para iniciar sesión en tu equipo local. Para usar a Windows Defender remoto Credential Guard, el cliente de escritorio remoto y el servidor deben cumplir los siguientes requisitos:

- Debe estar unido a un dominio de Active Directory y estar en el mismo dominio o en un dominio con una relación de confianza.

- Debes usar la autenticación Kerberos.

- Debes estar ejecutando al menos Windows 10, versión 1607 o Windows Server 2016.  

- Se requiere la aplicación clásica de Windows de escritorio remoto. La aplicación remota escritorio plataforma Universal de Windows no admite a Windows Defender remoto Credential Guard.

Puedes habilitar a Windows Defender remoto Credential Guard mediante el uso de una configuración del registro en el servidor de escritorio remoto y directiva de grupo o un parámetro de conexión a Escritorio remoto en el cliente de escritorio remoto. Para obtener más información acerca de cómo habilitar Windows Defender remoto Credential Guard, consulta [credenciales proteger escritorio remoto con Windows Defender remoto Credential Guard](https://docs.microsoft.com/en-us/windows/access-protection/remote-credential-guard). Como con Windows Defender Credential Guard, puedes usar a Windows Defender remoto Credential Guard para ayudar a proteger las identidades con privilegios en Windows Server 2016.

### <a name="secure-the-operating-system-to-run-your-apps-and-infrastructure"></a>Proteger el sistema operativo para ejecutar las aplicaciones y la infraestructura
Impedir que las amenazas cibernéticas también requiere buscar y bloquear el malware y ataques que intentan obtener el control subverting los procedimientos operativos estándar de su infraestructura. Si los atacantes pueden obtener un sistema operativo o la aplicación se ejecute de manera no predeterminados, inviables, que es probable que usen ese sistema para realizar acciones malintencionadas. Windows Server 2016 proporciona niveles de protección que bloquean los atacantes externos ejecutar software malintencionado o aprovechar las vulnerabilidades. El sistema operativo tiene un rol activo para proteger la infraestructura y las aplicaciones por los administradores de actividad que indica que un sistema se ha producido la infracción de alertas.

#### <a name="windows-defender-device-guard"></a>Device Guard de Windows Defender
Windows Server 2016 incluye Windows Defender Device Guard para garantizar que el software de confianza solo se puede ejecutar en el servidor. Con la seguridad de virtualización, puede limitar qué archivos binarios pueden ejecutarse en el sistema en función de la directiva de la organización. Si todo lo que no sean los archivos binarios especificados intenta ejecutar, Windows Server 2016 lo bloquea y registra el intento fallido para que los administradores pueden ver que se ha producido una infracción posible. Notificación de incumplimiento es una parte fundamental de los requisitos de cumplimiento de GDPR.

Device Guard de Windows Defender también está integrado con PowerShell, de modo que puede autorizar pueden ejecutar los scripts en el sistema. En versiones anteriores de Windows Server, los administradores pueden eludir la aplicación de integridad de código simplemente elimina la directiva desde el archivo de código. Con Windows Server 2016, puedes configurar una directiva que esté firmada por la organización para que solo una persona con acceso al certificado que la directiva firmada puede cambiar la directiva.

#### <a name="control-flow-guard"></a>Protección de flujo de control 
Windows Server 2016 también incluye protección integrada contra algunas clases de ataques de daños de memoria. Los servidores de la aplicación de revisión es importante, pero siempre hay posibilidades de que el malware podría desarrollarse para una vulnerabilidad que aún no se ha identificado. Algunos de los métodos más comunes para aprovechar las vulnerabilidades son proporcionar datos inusuales o extremos a un programa en ejecución. Por ejemplo, un atacante puede aprovechar una vulnerabilidad de desbordamiento de búfer al proporcionar más información a un programa de lo esperado y saturación el área reservado por el programa para mantener una respuesta. Esto puede alterar la memoria adyacentes que podría contener un puntero de función.

Cuando el sistema llama a través de esta función, pueden saltar a continuación, en una ubicación no intencionada especificada por el atacante. Estos ataques son también conocido como accesos directos y orientada a ataques de programación (JOP). Protección de flujo de control impide los ataques JOP colocando estrecha restricciones en el código de la aplicación puede ser especialmente indirecta: ejecutado llamar a las instrucciones. Agrega comprobaciones de seguridad ligero para identificar el conjunto de funciones de la aplicación que son destinos válidos para llamadas indirectas. Cuando se ejecuta una aplicación, comprueba que estos objetivos de llamada indirecta son válidos.

Si se produce un error en la comprobación de protección de flujo de Control en tiempo de ejecución, Windows Server 2016 finaliza inmediatamente el programa, cualquier vulnerabilidad de seguridad que intenta llamar indirectamente a una dirección no válida de última hora. Protección de flujo de control proporciona una capa de protección adicional importante a Device Guard. Si ha habido algún riesgo de una aplicación de la lista blanca, sería capaz de ejecutar desmarcada de forma Device Guard, porque el Device Guard filtrado vería que la aplicación se ha firmado y se considera de confianza.

Pero puesto que protección de flujo de Control puede identificar si la aplicación se esté ejecutando en un orden inviables no predeterminados, el ataque produciría un error, impidiendo que se ejecute la aplicación afectada. Juntas, estas protecciones hacer muy difícil para los atacantes insertar malware en el software que se ejecuta en Windows Server 2016.

Desarrolladores que crean aplicaciones que se administrarán de datos personales se recomienda para habilitar protección de flujo de Control (CFG) en sus aplicaciones. Esta característica está disponible en Microsoft Visual Studio 2015 y se ejecuta en "CFG cuenta" versiones de Windows, libera la x86 y x64 para escritorio y servidor de Windows 10 y Windows 8.1 Update (KB3000850). No tienes que habilitar CFG para cada parte del código, como una mezcla de CFG habilitado y código no CFG habilitado se ejecutará correctamente. Pero si no se permiten CFG para todo el código puede abrir espacios en la protección. Además, CFG habilitado código funcione bien en "No reconocen CFG" versiones de Windows y, por tanto, es totalmente compatible con ellos.

#### <a name="windows-defender-antivirus"></a>Windows Defender Antivirus
Windows Server 2016 incluye el sector inicial, active capacidades de detección de Windows Defender para bloquear el malware conocido. Windows Defender Antivirus (AV) funciona junto con Device Guard de Windows Defender y protección de flujo de Control para evitar que el código malintencionado de cualquier tipo que se instalan en los servidores. Está activado de manera predeterminada, el administrador no tiene que realizar ninguna acción para que pueda empezar a trabajar. Windows Defender AV también está optimizado para admitir los diversos roles de servidor en Windows Server 2016. En el pasado, los atacantes usan shells, como PowerShell para iniciar el código binario malintencionado. En Windows Server 2016, PowerShell está ahora integrado con AV de Windows Defender para detectar malware antes de iniciar el código.

Windows Defender AV es una solución antimalware integrada que proporciona administración de seguridad y antimalware para equipos de escritorio, equipos portátiles y servidores. Windows Defender AV se ha mejorado significativamente desde que se presentó en Windows 8. Antivirus de Windows Defender en Windows Server usa un enfoque diversa para mejorar el antimalware:

- **Protección de entrega de nube** ayuda a detectar y bloquear nuevo malware en segundos, incluso si el malware no se ha visto nunca antes.

- **Contexto local de enriquecido** mejora cómo se identifica el malware. Windows Server informa a Windows Defender AV no solo sobre el contenido, como archivos y procesos, sino también dónde procede el contenido, donde se ha almacenado y mucho más. 

- **Sensores globales amplios** ayudar a mantener Windows Defender AV actual y al tanto del malware más reciente. Esto se logra de dos maneras: mediante la recopilación de los datos de contexto local enriquecido de extremos y analizando esos datos de forma centralizada.

- **Alterar la corrección** ayuda a protege Windows Defender AV propio contra ataques de malware. Por ejemplo, Windows Defender AV usa procesos protegidos, lo que impide que los procesos de confianza intenten manipular los componentes de Windows Defender AV, sus claves del registro y así sucesivamente.

- **Características de nivel empresarial** dar a los profesionales de TI las herramientas y opciones de configuración necesarias para que Windows Defender AV una solución antimalware de clase empresarial.

#### <a name="enhanced-security-auditing"></a>Las auditorías de seguridad mejorada 
Windows Server 2016 activamente alerte a los administradores a los intentos de incumplimiento posibles con la auditoría de seguridad mejorada que proporciona la información más detallada, que puede usarse para la detección de ataque más rápida y análisis detallado. Registra los eventos de protección de flujo de Control, Device Guard de Windows Defender y otras características de seguridad en una ubicación, lo que facilita a los administradores a determinar qué sistemas pueden estar en peligro.

Se incluyen nuevas categorías de evento:

- **Pertenencia a grupos de auditoría.** Te permite auditar la información de pertenencia al grupo en el token de inicio de sesión de un usuario. Los eventos se generan cuando las pertenencias a grupos se enumeran o consultan en el equipo donde se creó la sesión de inicio de sesión. 
 
- **Auditar actividad PnP.** Te permite auditar cuando plug and play detecta un dispositivo externo, que podría contener malware. Eventos de PnP pueden usarse para realizar un seguimiento de cambios en el hardware del sistema. Una lista de identificadores de proveedor de hardware se incluye en el evento.

Windows Server 2016 se integra fácilmente con sistemas de administración (SIEM) evento incidente de seguridad, como Microsoft Operations Management Suite (OMS), que puede incorporar la información de los informes de inteligencia sobre posibles infracciones. La profundidad de la información proporcionada por la auditoría mejorada permite a los equipos de seguridad identificar y responder a posibles infracciones de manera más rápida y eficaz.

### <a name="secure-virtualization"></a>Virtualización segura
Hoy en día, las empresas virtualizar todo lo que pueden de SQL Server a SharePoint para controladores de dominio de Active Directory. Máquinas virtuales (VM) simplemente que sea más fácil implementar, administrar, servicio y automatizar la infraestructura. Pero cuando se trata de seguridad, telas de virtualización en peligro se han convertido en un nuevo vector de ataque es difícil defenderse – hasta ahora. Desde una perspectiva GDPR, debes pensar acerca de cómo proteger máquinas virtuales que se proteja servidores físicos, incluido el uso de la tecnología del TPM de VM.

Windows Server 2016 cambia fundamentalmente cómo las empresas pueden proteger virtualización, incluyendo varias tecnologías que te permiten crear máquinas virtuales que solo se ejecutará en tu propio fabric; ayudar a proteger contra el almacenamiento, la red y se ejecutan en los dispositivos host.

#### <a name="shielded-virtual-machines"></a>Blindado máquinas virtuales
Las mismas cosas que facilitan la máquinas virtuales por lo tanto, migrar, copia de seguridad y replicar, también facilitan su modificar y copiar. Una máquina virtual es simplemente un archivo, por lo que no está protegido en la red, en el almacenamiento, las copias de seguridad o en otro sitio. Otro problema es que los administradores de fabric: independientemente de si son un administrador de almacenamiento o el Administrador de red: tengan acceso a todas las máquinas virtuales.

Un administrador en peligro en la estructura puede tener como resultado de información confidencial en máquinas virtuales. Todo el atacante debe hacer es usar las credenciales en peligro para copiar los archivos VM les gusta en una unidad USB y recorrer fuera de la organización, donde pueden tener acceso a los archivos de la máquina virtual desde cualquier otro sistema. Si cualquiera de las máquinas virtuales robadas fuera un controlador de dominio de Active Directory, por ejemplo, el atacante podría fácilmente ver el contenido y usar técnicas de ataques de fuerza bruta estén disponibles de inmediato para averiguar las contraseñas en la base de datos de Active Directory, en última instancia ofrecer acceso para todo lo demás dentro de la infraestructura.

Windows Server 2016 presenta blindado máquinas virtuales (VM blindado) para ayudar a proteger frente a escenarios como el indicado anteriormente. Máquinas virtuales blindadas incluyen un dispositivo virtual de TPM, que permite a las organizaciones para el cifrado de BitLocker se aplican a las máquinas virtuales y asegurarse de que se ejecutan solo en los hosts de confianza para ayudar a proteger contra los administradores de host, la red y almacenamiento en peligro. Máquinas virtuales blindadas se crean mediante la generación 2 las máquinas virtuales, que admiten el firmware de Unified Extensible Firmware Interface (UEFI) y tienen TPM virtual.

#### <a name="host-guardian-service"></a>Hospedar el servicio de Tutor
Junto a las VM blindado, el servicio de Tutor de Host es un componente esencial para la creación de una estructura de virtualización segura. Su trabajo es atestar el estado de un host de Hyper-V antes de que permita una VM blindado para arrancar o migrar a ese host. Mantiene las claves para máquinas virtuales blindado y no liberará hasta que el estado de seguridad está garantizado. Existen dos formas que puede requerir hosts de Hyper-V atestar el servicio de Host de Tutor.

La primera y más segura, son la certificación de hardware de confianza. Esta solución requiere que la VM blindado se ejecutan en los hosts que disponen de chips de TPM 2.0 y UEFI 2.3.1. Este hardware es necesaria para proporcionar el arranque medido y la información de integridad del kernel de sistema operativo requerida por el servicio de Tutor Host para garantizar el host de Hyper-V no se ha alterado.

Las organizaciones de TI tienen la alternativa del uso de atestación de administración de confianza, que puede ser conveniente si el hardware de TPM 2.0 no está en uso en la organización. Este modelo de atestación es fácil de implementar porque hosts simplemente se colocan en un grupo de seguridad y el servicio de Tutor Host está configurado para permitir blindado máquinas virtuales ejecutar en los hosts que son miembros del grupo de seguridad. Con este método, no hay ninguna medida compleja para garantizar que el equipo host no ha sido alterado. Sin embargo, se elimina la posibilidad de sin cifrar unidades de máquinas virtuales a pie un vistazo a la puerta de USB o que la máquina virtual se ejecutará en un host no autorizado. Esto es porque los archivos de la máquina virtual no se ejecutarán en cualquier equipo que no están en el grupo designado. Si aún no tienes hardware de TPM 2.0, puedes empezar con atestación de administración de confianza y cambiar a la certificación de hardware de confianza cuando se actualiza el hardware.

#### <a name="virtual-machine-trusted-platform-module"></a>Máquinas virtuales de módulo de plataforma segura
Windows Server 2016 admite TPM para máquinas virtuales, lo que permite la compatibilidad con tecnologías de seguridad avanzada, como el cifrado de unidad BitLocker® en máquinas virtuales. Puedes habilitar la compatibilidad con el TPM en cualquier máquina virtual de generación 2 Hyper-V mediante el Administrador de Hyper-V o el cmdlet de Windows PowerShell Enable-VMTPM.

Puedes proteger los TPM virtual (vTPM) mediante el uso de las claves de cifrado locales almacenados en el equipo host o almacenado en el servicio de Tutor de Host. Por lo tanto, mientras que el servicio de Tutor Host requiere infraestructura más, también proporciona más protección.

#### <a name="distributed-network-firewall-using-software-defined-networking"></a>Firewall de red distribuida usando definidos por el software de red
Es una forma de mejorar la protección en entornos virtualizados segmento de la red de manera que permite que las máquinas virtuales hablar solo con los sistemas específicos necesarios para funcionar. Por ejemplo, si la aplicación no necesita conectarse a Internet, se puede dividir desactivado, eliminar esos sistemas como destinos de los atacantes externos. Las definidos por el software de red (SDN) en Windows Server 2016 incluyen un firewall de red distribuida que te permite crear dinámicamente las directivas de seguridad que se pueden proteger las aplicaciones de ataques procedentes de dentro o fuera de una red. Este servidor de seguridad de red distribuida agrega capas a la seguridad, ya que permite aislar las aplicaciones de la red. Se pueden aplicar directivas en cualquier lugar a través de la infraestructura de red virtual, aislando VM-to-VM tráfico, VM-to-host tráfico o VM-to-Internet tráfico cuando es necesario, para los sistemas individuales que pueden haberse puesto en riesgo o mediante programación en varias subredes. Funcionalidades de red definidos por el software de Windows Server 2016 también permiten la ruta o para reflejar el tráfico entrante a los dispositivos virtuales no son de Microsoft. Por ejemplo, puede elegir enviar todo el tráfico de correo electrónico a través de un dispositivo de Barracuda virtual para la protección de filtrado de correo no deseado adicional. Esto le permite fácilmente la capa de seguridad adicional tanto local o en la nube.

### <a name="other-gdpr-considerations-for-servers"></a>Otras consideraciones de GDPR para servidores
La GDPR incluye requisitos explícitos de notificación de incumplimiento donde significa una infracción de los datos personales, "_una infracción de seguridad para su destrucción accidental o ilícita, pérdida, alteración, la divulgación no autorizada de, o acceso a, datos personales transmite, almacenada u o bien procesa. _"  Obviamente, no se puede comenzar a avanzar para cumplir los requisitos de notificación de GDPR estrictos en 72 horas si no puede detectar el incumplimiento en primer lugar.

Como se explicó en las notas del centro de seguridad de Windows, [incumplimiento de publicación: tratar con amenazas avanzada](http://wincom.blob.core.windows.net/documents/Post_Breach_Dealing_with_Advanced_Threats_Whitepaper.pdf,)

> "_a diferencia de incumplimiento preliminar, incumplimiento posterior a supone que ya se ha producido una infracción: actúa como una caja negra y Crime escena investigadores (CSI). POST-Post-breach proporciona equipos de seguridad la información y conjunto de herramientas necesarios para identificar, investigar y responder a los ataques que de lo contrario se desapercibido y debajo de la radar. _"

En esta sección veremos cómo Windows Server puede ayudarte a cumplir con tus obligaciones de notificación GDPR incumplimiento. Esto comienza con la comprensión de los datos subyacentes de amenazas disponibles a Microsoft que se recopila y analiza para su beneficio y cómo hacerlo, a través de Windows Defender avanzada amenaza protección (Promesa), esos datos pueden ser vital.

#### <a name="insightful-security-diagnostic-data"></a>Datos de diagnóstico de seguridad perspicaces
Durante casi dos años, Microsoft ha desactivando las amenazas en inteligencia útil que puede ayudar a fortalecer su plataforma y proteger a los clientes. Hoy en día, con las grandes ventajas informáticas contempladas en la nube, estamos encontrar nuevas maneras de usar nuestros motores de análisis enriquecida controla por medio de inteligencia contra amenazas para proteger a nuestros clientes.

Al aplicar una combinación de procesos automatizados y manuales, aprendizaje automático y expertos humanos, te podemos crear un gráfico de seguridad inteligente que aprende de sí misma y evoluciona en tiempo real, reducir el tiempo de nuestra colectivo para detectar y responder a nuevos incidentes a través de nuestros productos.

![Gráfico de seguridad de inteligencia de Microsoft](../media/GDPR-Windows-Server-Overview/gdpr-intelligent-security-graph.png)

El ámbito de inteligencia de amenaza de Microsoft expande, literalmente, miles de millones de puntos de datos: mensajes de mil millones de 35 analizan mensual, 1 mil millones de clientes a través de la empresa y obtener acceso a más de 200 de segmentos de consumidor en la nube autenticaciones mil millones de 14 realiza y servicios cada día. Todos estos datos se extraen juntos en su nombre por Microsoft para crear el gráfico de seguridad inteligente que pueden ayudarte a proteger la puerta de entrada de forma dinámica para mantener la seguridad, seguir siendo productivo y cumpla los requisitos de la GDPR.

#### <a name="detecting-attacks-and-forensic-investigation"></a>Detección de ataques e investigación detallada
Incluso las defensas de extremo mejor pueda pueden infringir más adelante, cuando cyberattacks más quedan sofisticadas y de destino. Dos funcionalidades pueden usarse para ayudar con la detección de incumplimiento posibles: protección de amenazas avanzada de Windows Defender (Promesa) y análisis de amenazas avanzada de Microsoft (ATA).

Protección de amenazas avanzadas (Promesa) de Windows Defender le ayuda a detectar, investigar y responder a ataques avanzados y las infracciones de los datos en las redes. Los tipos de fuga de datos la GDPR espera protegerse frente a través de las medidas de seguridad técnica para garantizar la confidencialidad en curso, la integridad y la disponibilidad de los datos personales y sistemas de procesamiento.

Entre las ventajas claves de Windows Defender Promesa son los siguientes:

- **Detectar la no detectable.** Sensores integrados profundizar en el núcleo del sistema operativo, expertos en seguridad de Windows y único óptica más de mil millones de 1 máquinas y señales en todos los servicios de Microsoft.

- **Incorporada en lugar de agregada.** Sin agente con alto rendimiento y un impacto mínimo, con tecnología de nube; fácil administración con ninguna implementación. 

- **Lugar único para la seguridad de Windows.** Explora los 6meses de enriquecidas y de tiempo de máquina, unificador eventos de seguridad de la Promesa de Windows Defender, Antivirus de Windows Defender y Device Guard de Windows Defender.

- **Energía del gráfico de Microsoft.** Aprovecha el gráfico de seguridad de inteligencia de Microsoft para integrar la detección y la exploración con la suscripción a Office 365 Promesa, para seguir la pista y responder a los ataques.

Leer más información en [lo que es nuevo en la vista previa de la actualización de Windows Defender Promesa creadores](https://blogs.microsoft.com/microsoftsecure/2017/03/13/whats-new-in-the-windows-defender-atp-creators-update-preview/).

ATA es un producto local que ayuda a detectar el compromiso de identidad de una organización. ATA capturar y analizar el tráfico de red para la autenticación, autorización y protocolos (como Kerberos, DNS, RPC, NTLM y otros protocolos) de recopilación de información. ATA utiliza estos datos para generar un perfil comportamiento acerca de los usuarios y otras entidades en una red de modo que puedan detectar anomalías y patrones de ataques conocidos. La siguiente tabla enumera los tipos de ataques detectados ATA.


|Tipo de ataque |Descripción |
|---------|---------|
|Ataques malintencionados |Estos ataques se detectan mediante la búsqueda de ataques de una lista conocida de tipos de ataques, incluidos:<ul><li>PASS-the-Ticket (PtT)</li><li>PASS-the-Hash (PtH)</li><li>Paso elevado-the-Hash</li><li>Falsificado PAC (MS14-068)</li><li>Vale dorado</li><li>Duplicaciones malintencionados</li><li>Reconocimiento</li><li>Ataques de fuerza bruta</li><li>Ejecución remota</li></ul>¿Para obtener una lista completa de los ataques malintencionados que se pueden detectar y su descripción, consulta [detectar qué sospechoso actividades puede ATA?](https://docs.microsoft.com/en-us/advanced-threat-analytics/understand-explore/ata-threats).|
|Comportamiento anómalo |Estos ataques se detectan mediante el análisis de comportamiento y usan máquina aprender a identificar las actividades cuestionables, incluidos:<ul><li>Anómalas inicios de sesión</li><li>Amenazas desconocidas</li><li>Uso compartido por contraseña</li><li>Desplazamiento lateral</li></ul>|
|Riesgos y problemas de seguridad |Estos ataques se detectan echando un vistazo a la red actual y la configuración del sistema, incluidos:<ul><li>Confianza roto</li><li>Protocolos débiles</li><li>Vulnerabilidades conocidas de protocolo</li></ul>|

Puedes usar ATA para ayudar a detectar los atacantes intentan poner en peligro identidades con privilegios. Para obtener más información sobre cómo implementar ATA, consulta los temas de Plan, diseño e implementación de la [documentación avanzada de análisis de la amenaza](https://docs.microsoft.com/en-us/advanced-threat-analytics/).

## <a name="related-content-for-associated-windows-server-2016-solutions"></a>Contenido relacionado asociado soluciones de Windows Server 2016

- **Windows Defender Antivirus:** https://www.youtube.com/watch?v=P1aNEy09NaI y https://docs.microsoft.com/en-us/windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10

- **Windows Defender avanzada protección contra amenazas:** https://www.youtube.com/watch?v=qxeGa3pxIwg y https://docs.microsoft.com/en-us/windows/threat-protection/windows-defender-atp/configure-server-endpoints-windows-defender-advanced-threat-protection

- **Windows Defender Device Guard:** https://www.youtube.com/watch?v=F-pTkesjkhI y https://docs.microsoft.com/en-us/windows/device-security/device-guard/device-guard-deployment-guide

- **Windows Defender Credential Guard:** https://www.youtube.com/watch?v=F-pTkesjkhI y https://docs.microsoft.com/en-us/windows/access-protection/credential-guard/credential-guard

- **Protección de flujo de control:** https://msdn.microsoft.com/en-us/library/windows/desktop/mt637065(v=vs.85).aspx

- **Seguridad y garantía:** https://docs.microsoft.com/en-us/windows-server/security/security-and-assurance

## <a name="disclaimer"></a>Declinación de responsabilidades de
Este artículo es un comentario sobre la GDPR, como Microsoft interpreta, hasta la fecha de publicación. Se ha dedicado mucho tiempo con GDPR y quieres que lo hemos hecho detallista sobre su propósito y su significado. Pero la aplicación de GDPR es muy específicos de hecho, y no todos los aspectos e interpretaciones de GDPR liquidadas bien.

Como resultado, este artículo se proporciona con fines informativos y no debería confiar en como asesoramiento legal o para determinar cómo GDPR pueden aplicarse a usted y su organización. Te animamos a trabajar con un profesional legalmente completo para explicar GDPR, cómo se aplica específicamente a la organización y cómo recomendados garantizar la conformidad.

Microsoft NO OFRECE NINGUNA GARANTÍA, EXPRESA, IMPLÍCITA O LEGAL RESPECTO DE LA INFORMACIÓN DE ESTE ARTÍCULO. En este artículo se proporciona "como-es." Información y las opiniones expresadas en este artículo, incluidas las URL y otras referencias del sitio Web de Internet, pueden cambiar sin previo aviso.

En este artículo no proporciona ningún derecho legal de ninguna propiedad intelectual de ningún producto de Microsoft.  Puede copiar y usar este artículo internos, solo con fines de referencia.  

Publicado en septiembre de 2017<br>
Versión 1.0<br>
© 2017 Microsoft. Todos los derechos reservados.


