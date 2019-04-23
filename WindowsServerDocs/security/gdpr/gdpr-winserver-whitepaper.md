---
title: Inicio de tu viaje por el Reglamento general de protección de datos (GDPR) para Windows Server 2016
description: Usa este artículo para comprender qué es GDPR y cuáles son los productos que Microsoft proporciona para ayudarte a empezar en el camino del cumplimiento.
keywords: privacidad, GDPR
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.technology:
- techgroup-security
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 09/25/2017
ms.author: nirb
ms.openlocfilehash: be9509de0291924bb95733f995b447230bb75214
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870136"
---
# <a name="beginning-your-general-data-protection-regulation-gdpr-journey-for-windows-server"></a>A partir de su viaje Reglamento General de protección de datos (RGPD) para Windows Server 

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este artículo se proporciona información sobre la GDPR, entre la que se incluye en qué consiste y los productos que Microsoft proporciona para ayudarte a cumplirla.

## <a name="introduction"></a>Introducción
El 25 de mayo de 2018 entrará en vigor una ley de privacidad europea que establece un nuevo listón global de derechos de privacidad, seguridad y cumplimiento.

La GDPR (normativa general de protección de datos) se ocupa fundamentalmente de la protección y la habilitación de los derechos de privacidad de las personas. La GDPR establece estrictos requisitos de privacidad global que rigen la manera en que se administran y protegen los datos personales a la vez que se respecta la elección individual, independientemente de dónde se envían, procesan o almacenan los datos.

Microsoft y nuestros clientes se encuentran ahora en el camino de lograr los objetivos de privacidad de la GDPR. En Microsoft, creemos que la privacidad es un derecho fundamental y que la GDPR supone un importante paso hacia adelante para aclarar y permitir los derechos de privacidad individuales. Pero también sabemos que la GDPR requerirá cambios importantes por parte de las organizaciones de todo el mundo.

Hemos explicado nuestro compromiso con la GDPR y la manera en que damos soporte a nuestros clientes en la entrada de blog [Get GDPR compliant with the Microsoft Cloud](https://blogs.microsoft.com/on-the-issues/2017/02/15/get-gdpr-compliant-with-the-microsoft-cloud/#hv52B68OZTwhUj2c.99) de nuestro Director de privacidad [Brendon Lynch](https://blogs.microsoft.com/on-the-issues/author/brendonlynch/) y la entrada de blog [Earning your trust with contractual commitments to the General Data Protection Regulation](https://blogs.microsoft.com/on-the-issues/2017/04/17/earning-trust-contractual-commitments-general-data-protection-regulation/#6QbqoGWXCLavGM63.99)"de [Rich Sauer](https://blogs.microsoft.com/on-the-issues/author/rsauer/), vicepresidente corporativo y consejero general adjunto de Microsoft.

Aunque su viaje hasta el cumplimiento de GDPR puede parecer un desafío, estamos aquí para ayudarte. Para obtener información específica sobre la GDPR, nuestros compromisos y cómo comenzar el viaje, visita la [sección GDPR del Microsoft Trust Center](https://www.microsoft.com/en-us/trustcenter/privacy/gdpr).

## <a name="gdpr-and-its-implications"></a>GDPR y sus implicaciones
La GDPR es una normativa compleja que puede requerir cambios importantes respecto a cómo se recopila, usan y administran datos personales. Microsoft cuenta con una larga trayectoria ayudando a nuestros clientes a cumplir complejas disposiciones reglamentarias y, en lo referente a tu preparación para la GDPR, somos tu partner en este viaje.

La GDPR impone reglas en las organizaciones que ofrecen bienes y servicios a las personas de la Unión Europea (UE), o que recopilan y analizan datos vinculados a los residentes de la UE, independientemente de dónde se encuentren esas empresas. Entre los elementos clave de la GDPR se encuentran los siguientes:

- **Derechos de privacidad personal mejoradas.** Se ha reforzado la protección de datos para residentes de la UE garantizando que tienen el derecho de tener acceso a sus datos personales, a corregir imprecisiones de dichos datos, a borrar dichos datos, a oponerse al procesamiento de sus datos personales y a moverlos.

- **Mayor derecho para proteger los datos personales.** Responsabilidad reforzada de las organizaciones que procesan datos personales, lo que proporciona una mayor claridad de la responsabilidad en la garantía del cumplimiento.

- **Los informes de infracción obligatorios datos personales.** Las organizaciones que controlan datos personales son necesarias para notificar las infracciones de datos personales que suponen un riesgo para los derechos y las libertades de las personas a sus autoridades de control sin retraso debido y, cuando sea posible, en un plazo no posterior a 72 horas una vez que conozcan la infracción.

Como se podría esperar, la GDPR puede tener un impacto importante en tu empresa, lo que puede requerir que actualices las directivas de privacidad, implementes y refuerces los controles de protección de datos y los procedimientos de notificación de incumplimiento, implementes directivas muy transparentes e inviertas más en TI y formación. Microsoft Windows 10 puede ayudarte a abordar de forma eficaz y eficiente algunos de estos requisitos.

## <a name="personal-and-sensitive-data"></a>Datos personales y confidenciales
Como parte de tu esfuerzo por cumplir con la GDPR, debes comprender de qué manera la normativa define los datos personales y confidenciales, y cómo se relacionan esas definiciones con los datos mantenidos por tu organización. En función de que comprensión, podrá detectar donde se crean que los datos, procesar, administra y almacena.

La GDPR considera que los datos personales son cualquier información relacionada con una persona física identificada o identificable. Eso puede incluir tanto identificación directa (por ejemplo, la razón social) como la identificación indirecta (por ejemplo, información específica que hace que resulte evidente que es a ti a quien se refieren los datos). La GDPR también deja claro que el concepto de datos personales incluye identificadores en línea (por ejemplo, direcciones IP, id. de dispositivos móviles) y datos de ubicación.

La GDPR presenta definiciones específicas de datos genéticos (por ejemplo, la secuencia genética de una persona) y datos biométricos. Los datos genéticos y biométricos junto con otras subcategorías de datos personales (datos personales que revelan el origen étnico o racial, las opiniones políticas, las convicciones religiosas o filosóficas o la afiliación sindical: datos relativos a la salud; o datos relacionados con la vida sexual de la persona o la orientación sexual) se tratan como datos personales confidenciales según la GDPR. Los datos personales confidenciales requieren protecciones mejoradas y suelen requerir el consentimiento explícito de la persona de la que se van a procesar estos datos.

### <a name="examples-of-info-relating-to-an-identified-or-identifiable-natural-person-data-subject"></a>Ejemplos de información relacionada con una persona natural identificada o identificable (interesado)
En esta lista se proporcionan ejemplos de varios tipos de información que se regularán a través de GDPR. No se trata de una lista exhaustiva.

-   Name

-   Número de identificación (por ejemplo, SSN)

-   Datos de ubicación (por ejemplo, dirección particular)

-   Identificador en línea (por ejemplo, dirección de correo electrónico, nombres de pantalla, dirección IP, id. de dispositivo)

-   Datos seudónimos (por ejemplo, usando una clave para identificar individuos)

-   Datos genéticos (por ejemplo, muestras biológicas de una persona)

-   Datos biométricos (por ejemplo, huellas digitales, reconocimiento facial)

## <a name="getting-started-on-the-journey-towards-gdpr-compliance"></a>Tareas iniciales en el camino hacia el cumplimiento de GDPR
Dado todo lo que implica el cumplimiento de la GDPR, te recomendamos encarecidamente que no esperes a prepararte hasta que comience su aplicación. Debes revisar ahora tus prácticas de administración de datos y privacidad. Te recomendamos que comiences tu viaje hacia el cumplimiento de la GDPR centrándote en cuatro pasos clave:

-   **Detectar.** Identifica qué datos personales tienes y dónde se encuentran. 

-   **Administrar.** Controla cómo se usan los datos personales y se obtiene acceso a ellos.

-   **Proteger.** Establece controles de seguridad para evitar y detectar vulnerabilidades e infracciones de datos, y responder ante ellas.  

-   **Informe.** Actúa sobre solicitudes de datos, infracciones de datos de informe y mantén la documentación necesaria.

    ![Diagrama sobre cómo funcionan conjuntamente los 4 pasos clave de GDPR](../media/GDPR-Windows-Server-Overview/gdpr-steps-diagram.png)

Para cada uno de los pasos, hemos descrito herramientas de ejemplo, recursos y características en distintas soluciones de Microsoft, que se pueden usar para ayudarte a cumplir los requisitos de ese paso. Aunque este artículo no es una completa guía "procedimiento", hemos incluido vínculos para averiguar más detalles y más información está disponible en el [sección GDPR de Microsoft Trust Center](https://www.microsoft.com/en-us/trustcenter/privacy/gdpr).

## <a name="windows-server-security-and-privacy"></a>Privacidad y seguridad de Windows Server
El RGPD, deberá implementar medidas de seguridad técnicas y organizativas apropiadas para proteger datos personales y sistemas de procesamiento. En el contexto del RGPD, los entornos de servidores físicos y virtuales procesan potencialmente datos personales y confidenciales. El procesamiento puede significar cualquier operación o un conjunto de operaciones, como la recopilación de datos, almacenamiento y recuperación.

Su capacidad para satisfacer este requisito y a implementar medidas de seguridad técnica adecuadas debe reflejar las amenazas que se encuentra en el entorno de TI de hoy cada vez más hostil. El panorama actual de las amenazas de seguridad es el de unas amenazas agresivas y tenaces. En años anteriores, los atacantes malintencionados se centraban principalmente en obtener el reconocimiento de la comunidad a través de sus ataques la emoción que resulta de desconectar un sistema temporalmente. Desde entonces, la intención del atacante ha pasado a ser ganar dinero, por ejemplo, la retención de dispositivos y datos hasta que el propietario pague el rescate solicitado.

Los ataques modernos se centran cada vez más en el robo de propiedad intelectual a gran escala, la degradación de los sistemas dirigidos, que se pueden traducir en pérdidas financieras y, ahora, incluso el ciberterrorismo, que amenaza la seguridad de personas, empresas e intereses nacionales en todo el mundo. Estos atacantes suelen ser individuos con una alta formación y expertos en seguridad, algunos de los cuales están contratados por Estados nación con grandes presupuestos y recursos humanos aparentemente ilimitados. Estos tipos de amenaza requieren un enfoque que pueda hacer frente a este desafío.

No solo estas amenazas suponen un riesgo para tu capacidad de mantener el control de los datos personales o confidenciales que puedes tener, sino que también constituyen un riesgo material para toda tu empresa en general. Tenga en cuenta los datos recientes de McKinsey, Ponemon Institute, Verizon y Microsoft:

- El coste medio del tipo de la infracción de datos que la GDPR espera que notifique es de 3.500.000 $.

- El 63 % de estas infracciones implican contraseñas débiles o robadas que la GDPR espera que corrijas.

- Todos los días se crean y se propagan más de 300.000 nuevas muestras de malware, lo que hace que tu tarea de abordar la protección de datos resulte aún más difícil.

Tal como se muestra con los recientes ataques de Ransomware, una vez llamados el negro plaga de Internet, los atacantes va después de los destinos más grandes que pueden permitirse que pagar más, con consecuencias potencialmente dañinas. El RGPD incluye sanciones que hacen que sus sistemas, incluidos equipos de escritorio y portátiles, que contienen datos personales y confidenciales enriquecido los destinos de hecho.

Dos principios claves guiaron y continuarán guiar el desarrollo de Windows:

- **Seguridad.** Los datos de que almacén de nuestro software y servicios en nombre de nuestros clientes deben protegidos frente a daños y usa o modificar solo de forma adecuada. Modelos de seguridad deberían ser sencillo para que los desarrolladores más clara y en sus aplicaciones.

- **Privacidad.** Los usuarios deben estar en el control del usan de sus datos. Las directivas de uso de la información deben quedar claros al usuario. Los usuarios deben estar en el control de cuando y si reciben información para aprovechar al máximo su tiempo. Debería ser fácil para los usuarios especificar el uso adecuado de su información, incluido el control del uso de correo electrónico que envíen.

Microsoft ha permanecido sólida contra estos principios observó recientemente como al CEO de Microsoft, Satya Nadella, 

> "_Como sigue cambiando el mundo y requisitos comerciales evolucionan, algunas cosas son coherentes: a petición del cliente para la seguridad y privacidad._ "

Mientras trabaja para cumplir con el RGPD, descripción de la función de los servidores físicos y virtuales en crear, obtener acceso a, procesar, almacenar y administrar datos que pueden clasificarse como personal y datos potencialmente confidenciales en el RGPD están importantes. Windows Server proporciona capacidades que le ayudarán a cumplan con los requisitos del RGPD para implementar medidas de seguridad técnicas y organizativas apropiadas para proteger los datos personales.

La postura de seguridad de Windows Server 2016 no es un bolt en; es un principio de arquitectura. Y se puede entender mejor en las cuatro entidades de seguridad:

- **Proteger.** Enfoque en curso y la innovación en medidas preventivas; bloquear los ataques conocidos y malware conocido.

- **Detectar.** Integral de herramientas para ayudar a detectar anomalías y responder a ataques más rápidos de supervisión.

- **Responder.** Líder de respuesta y la recuperación de las tecnologías más profundo asesoramiento experto.

- **Aislar.** Aislar los secretos de componentes y los datos del sistema operativo, limitar los privilegios de administrador y medir rigurosamente el estado del host.

Con Windows Server, en gran medida se ha mejorado la capacidad de proteger, detectar y defenderse contra los tipos de ataques que pueden dar lugar a infracciones de datos. Dados los estrictos requisitos en torno a la notificación de infracciones en la GDPR, garantizar que los sistemas de escritorio y portátiles están bien protegidos disminuirán los riesgos a los que te enfrentas y que podrían dar lugar a costosas notificaciones y análisis de incumplimientos.

En la sección siguiente, verá cómo Windows Server proporciona capacidades que se ajusten a lleno en la fase de "Protección" de su recorrido del cumplimiento de RGPD. Estas capacidades se dividen en tres escenarios de protección:

- **Proteger las credenciales y limitar los privilegios de administrador.** Windows Server 2016 ayuda a implementar estos cambios, para ayudar a impedir que el sistema que se va a usar como punto de partida para aún más las intrusiones.

- **Proteger el sistema operativo para ejecutar sus aplicaciones e infraestructura.** Windows Server 2016 proporciona niveles de protección, lo que ayuda a bloquear los atacantes externos que ejecuta software malintencionado o aprovechando las vulnerabilidades.

- **Virtualización segura.** Windows Server 2016 permite una virtualización segura, el uso de máquinas de virtuales blindadas y tejido protegido. Esto ayuda a cifrar y ejecutar las máquinas virtuales en hosts de confianza en el tejido, mejor protección frente a ataques malintencionados.

Estas capacidades, que se describe con más detalle más adelante con referencias a los requisitos específicos de RGPD, se crean sobre la protección avanzada de los dispositivos que le ayuda a mantener la integridad y seguridad del sistema operativo y los datos.

Aprovisionar clave en el RGPD es la protección de datos por diseño y de forma predeterminada, y ayuda con su capacidad para satisfacer esta disposición son características dentro de Windows 10 como dispositivos de BitLocker. BitLocker usa la tecnología del módulo de plataforma segura (TPM), que proporciona funciones relacionadas con la seguridad basada en hardware. Este chip crypto-procesador incluye varios mecanismos de seguridad física para que sea resistente a alteraciones y software malintencionado no puede alterar las funciones de seguridad de TPM.

El chip incluye varios mecanismos de seguridad física que hacen que sea resistente a las alteraciones y que las funciones de seguridad no permitan que el software malintencionado realice alteraciones. Algunas de las principales ventajas de usar la tecnología TPM son que permiten:

-   Generar, almacenar y limitar el uso de claves criptográficas.

-   Usar la tecnología del TPM para la autenticación de dispositivos de plataforma mediante el uso de la clave RSA exclusiva del TPM, que se grabará en sí misma.

-   Garantizar la integridad de la plataforma llevando y almacenando medidas de seguridad.

La protección de dispositivo avanzada adicional relevante para el funcionamiento sin las infracciones de datos incluyen el Arranque seguro de Windows para ayudar a mantener la integridad del sistema al garantizar que el malware no se puede iniciar antes de las defensas del sistema.

## <a name="windows-server-supporting-your-gdpr-compliance-journey"></a>Windows Server: Compatibilidad con sus labores de cumplimiento del RGPD
Las características clave dentro de Windows Server pueden ayudar a los mecanismos de seguridad y privacidad que requiere el RGPD para el cumplimiento de implementación de manera eficiente y eficaz. Mientras que el uso de estas características no garantizará su cumplimiento, admitirán sus esfuerzos para hacerlo.

El sistema operativo de servidor se encuentra en un nivel estratégico en la infraestructura de la organización, ofrece nuevas oportunidades para crear capas de protección frente a ataques que podrían robar datos y se interrumpe su negocio. Los aspectos clave del RGPD como privacidad por diseño, protección de datos y Control de acceso deben solucionarse dentro de su infraestructura de TI en el nivel de servidor.

Trabaja para ayudar a proteger la identidad, el sistema operativo y los niveles de virtualización, Windows Server 2016 ayuda a bloquear los vectores de ataque común usados para obtener acceso ilícito a los sistemas: robo de credenciales, malware y un tejido de virtualización en peligro. Además de reducir el riesgo empresarial, los componentes de seguridad integradas en Ayuda de Windows Server 2016 los requisitos de cumplimiento de dirección para la clave y del sector las normas de seguridad. 

Estas protecciones de virtualización, identidad y sistema operativo le permiten proteger mejor su centro de datos que ejecutan Windows Server como una máquina virtual en cualquier nube y limitar la capacidad de los atacantes para poner en peligro las credenciales, inicio el malware y permanecer en la sombra en su red. Del mismo modo, cuando se implementa como un host de Hyper-V, Windows Server 2016 ofrece garantía de seguridad para los entornos de virtualización a través de máquinas de virtuales blindadas y capacidades de firewall distribuido. Con Windows Server 2016, el sistema operativo de servidor se convierte en un participante activo en la seguridad de su centro de datos.

### <a name="protect-your-credentials-and-limit-administrator-privileges"></a>Proteger las credenciales y limitar los privilegios de administrador 
Control sobre el acceso a los datos personales y los sistemas que procesan datos, es un área con el RGPD que tenga requisitos específicos, incluido el acceso a los administradores. Las identidades con privilegios son las cuentas que tienen privilegios, como las cuentas de usuario que son miembros de los administradores de dominio, administradores de empresa, administradores locales o incluso los grupos de usuarios avanzados elevados. Estas identidades pueden incluir también las cuentas que se han concedido privilegios directamente, como realizar copias de seguridad, apagar el sistema u otros derechos que se enumeran en el nodo de la asignación de derechos de usuario en la consola Directiva de seguridad Local.

Como una entidad de seguridad de control de acceso general y en línea con el RGPD, necesita proteger estas identidades con privilegios de los riesgos causados por posibles atacantes. En primer lugar, es importante comprender cómo se ven comprometidas las identidades; a continuación, puede planear impedir que los atacantes obtengan acceso a estas identidades con privilegios.

#### <a name="how-do-privileged-identities-get-compromised"></a>¿Cómo las identidades con privilegios puedan estar en peligro?
Pueden obtener en peligro las identidades con privilegios cuando las organizaciones no tienen las directrices para protegerlos. A continuación, se exponen algunos ejemplos:

- **Más privilegios que son necesarios.** Uno de los problemas más comunes es que los usuarios tengan más privilegios que son necesarios para realizar su función de trabajo. Por ejemplo, un usuario que administre DNS podría ser un administrador de AD. A menudo, esto se hace para evitar la necesidad de configurar distintos niveles de administración. Sin embargo, si este tipo de cuenta se ve comprometida, el atacante automáticamente tiene privilegios elevados.

- **Constantemente se inició sesión con privilegios elevados.** Otro problema común es que los usuarios con privilegios elevados pueden usarlo durante un tiempo ilimitado. Esto es muy común con profesionales de TI que inicie sesión en un equipo de escritorio con una cuenta con privilegios, mantener la sesión iniciada y usar la cuenta con privilegios para explorar la web y usar correo electrónico (trabajo de TI típica funciones de trabajo). Duración ilimitada de cuentas con privilegios hace que la cuenta más susceptibles a ataques y aumenta las probabilidades de que la cuenta se vea comprometida.

- **Investigación de ingeniería social.** La mayoría de las amenazas de credencial para empezar, investigar la organización y, a continuación, lleva a cabo a través de ingeniería social. Por ejemplo, un atacante puede realizar un ataque de suplantación de identidad de correo electrónico cuentas legítimo de poner en peligro (pero no necesariamente elevadas cuentas) que tienen acceso a la red de la organización. El atacante usa, a continuación, estas cuentas válidas para realizar investigaciones adicionales en la red e identificar las cuentas con privilegios que pueden realizar tareas administrativas. 

- **Aprovechar las cuentas con privilegios elevados.** Incluso con una cuenta de usuario normal sin privilegios elevados en la red, los atacantes pueden obtener acceso a las cuentas con permisos elevados. Por lo tanto, uno de los métodos más comunes de hacerlo es mediante Pass-the-Hash o ataques Pass-the-Token. Para obtener más información sobre el Pass-the-Hash y otras técnicas de robo de credenciales, vea los recursos en el [páginaPass-the-Hash (PtH)](https://technet.microsoft.com/dn785092.aspx).

Por supuesto, hay otros métodos que los atacantes pueden usar para identificar y poner en peligro las identidades con privilegios (con nuevos métodos que se crean todos los días). Por lo tanto, es importante que establezca procedimientos recomendados para que los usuarios inician sesión con cuentas con privilegios mínimos para reducir la capacidad de los atacantes obtener acceso a las identidades con privilegios. Las secciones siguientes describen la funcionalidad en Windows Server puede mitigar estos riesgos.

#### <a name="just-in-time-admin-jit-and-just-enough-admin-jea"></a>Administración Just-in-Time (JIT) y Just Enough Admin (JEA)
Mientras protege frente a Pass-the-Hash o ataques Pass-the-Ticket es importante, todavía se pueden robar las credenciales de administrador por otros medios, como la ingeniería social, empleados descontentos y fuerza bruta. Por lo tanto, además de aislamiento de credenciales tanto como sea posibles, también desea alguna forma de limitar el alcance de los privilegios de administrador en caso de que se ven en peligro.

En la actualidad, demasiadas cuentas de administrador son con demasiados privilegios, incluso si tienen solo un área de responsabilidad. Por ejemplo, un administrador de DNS, que requiere un conjunto muy reducido de privilegios para administrar servidores DNS, a menudo se concede privilegios de administrador de dominio. Además, dado que estas credenciales se conceden para perpetuo, no hay ningún límite en cuánto tiempo se pueden usar.

Todas las cuentas con privilegios de nivel de administrador de dominio innecesarios aumenta la exposición a atacantes que pretenden poner en peligro las credenciales. Para minimizar la superficie de ataque, desea proporcionar únicamente el conjunto específico de derechos que un administrador debe hacer el trabajo, y solo para la ventana de tiempo necesario para completar el proceso.

Usar Just Enough Administration y Just-in-Time administración, los administradores pueden solicitar los privilegios específicos que necesitan para la ventana de tiempo necesario exacta. Para un administrador DNS, por ejemplo, usar PowerShell para habilitar Just Enough Administration le permite crear un conjunto limitado de comandos que están disponibles para la administración de DNS.

Si el Administrador de DNS debe realizar una actualización en uno de sus servidores, podría solicitar acceso para administrar DNS con Microsoft Identity Manager 2016. El flujo de trabajo de la solicitud puede incluir un proceso de aprobación, como la autenticación en dos fases, lo que podría llamar al teléfono del administrador para confirmar su identidad antes de conceder los privilegios solicitados. Una vez concedido, esos privilegios DNS proporcionan acceso a la función de PowerShell para DNS de un intervalo de tiempo específico.

Imagínese esta situación si se lo roban las credenciales del administrador DNS. En primer lugar, puesto que las credenciales no tienen conectadas a ellas privilegios de administrador, el atacante no sería capaz de obtener acceso al servidor DNS: o a otros sistemas: realizar ningún cambio. Si el atacante ha intentado solicitar privilegios para el servidor DNS, autenticación de segundo factor podría pedirles que confirmar su identidad. Puesto que no es probable que el atacante tiene teléfono del administrador DNS, autenticación produciría un error. Esto podría bloquear el atacante fuera del sistema y la organización de TI de alertas que podrían estar en peligro las credenciales.

Además, muchas organizaciones usan la versión gratuita [solución de contraseña de administrador Local (LAP)](http://aka.ms/laps) como un mecanismo sencillo y eficaz de administración de JIT para sus sistemas de servidor y cliente. La capacidad de LAPS proporciona administración de contraseñas de cuentas locales de equipos unidos al dominio. Las contraseñas se almacenan en Active Directory (AD) y protegidas por y lista de Control de acceso (ACL) por lo que los usuarios aptos sólo pueden leerlo o solicitar su restablecimiento.

Como se indicó en el [Windows Credential Theft Mitigation Guide](https://www.microsoft.com/en-us/download/confirmation.aspx?id=54095), 

> "_los delincuentes herramientas y técnicas que se usa para llevar a cabo el robo de credenciales y mejoran la reutilización de ataques, los atacantes malintencionados que les resulta más fácil de lograr sus objetivos. Robo de credenciales a menudo se basa en las prácticas operativas o exposición de credenciales de usuario, por lo que las mitigaciones eficaz requieren un enfoque holístico que se dirige a personas, procesos y tecnología. Además, estos ataques se basan en el atacante robo de credenciales después de poner en peligro un sistema para expandir o conservar el acceso, por lo que las organizaciones deben contener infracciones rápidamente mediante la implementación de estrategias que impiden que los atacantes mover libremente y no se detectan en un red en peligro._ "

Una consideración de diseño importantes para Windows Server mitigar el robo de credenciales, deriva en concreto, las credenciales. Credential Guard proporciona seguridad considerablemente mejorada contra el robo de credenciales derivada y volver a utilizar al implementar un cambio de arquitectura importante en Windows diseñada para ayudar a erradicar los ataques de aislamiento basado en hardware, en lugar de simplemente intentar defenderse contra ellas.

Mientras que con Credential Guard de Windows Defender, NTLM y Kerberos credenciales obtenidas están protegidas mediante seguridad basada en virtualización, técnicas de ataques de robo de credenciales y las herramientas usan en muchos ataques dirigidos se bloquean. Con la ejecución de malware en el sistema operativo con privilegios administrativos no se pueden extraer secretos que están protegidos con la seguridad basada en la virtualización. Aunque Credential Guard de Windows Defender es una solución eficaz, los ataques de amenaza persistente probablemente MAYÚS nuevas técnicas de ataque, también deberían incluir a Device Guard, tal como se describe a continuación, junto con otras estrategias de seguridad y las arquitecturas de voluntad.

#### <a name="windows-defender-credential-guard"></a>Credential Guard de Windows Defender
Credential Guard de Windows Defender usa la seguridad basada en virtualización para aislar la información de credenciales, impide que se intercepten los hashes de contraseña o vales de Kerberos. Usa un completamente nuevo proceso aislado autoridad de seguridad Local (LSA), que no es accesible para el resto del sistema operativo. Todos los archivos binarios utilizados por la LSA aislada se firman con certificados que se validan antes de iniciar a ellos en el entorno protegido, realizar ataques de tipo Pass-the-Hash ineficaz.

Credential Guard de Windows Defender usa:

- Seguridad basada en virtualización (obligatorio). También se requiere:

    - CPU de 64 bits

    - Extensiones de virtualización de CPU, además de las tablas de página extendida

    - Hipervisor de Windows

- Arranque seguro (obligatorio)

- TPM 2.0 discreto o de firmware (preferido: proporciona el enlace al hardware)

Puede usar a Credential Guard de Windows Defender para ayudar a proteger las identidades con privilegios mediante la protección de las credenciales y derivados de las credenciales en Windows Server 2016. Para obtener más información sobre los requisitos de Credential Guard de Windows Defender, consulte [derivados de proteger las credenciales de dominio con Credential Guard de Windows Defender](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard).

#### <a name="windows-defender-remote-credential-guard"></a>Credential Guard remoto de Windows Defender
Windows Defender de Credential Guard remoto en Windows Server 2016 y Windows 10 Anniversary Update también ayuda a proteger las credenciales de usuario con conexiones a Escritorio remoto. Anteriormente, cualquier persona que use servicios de escritorio remoto tendría que iniciar sesión en su equipo local y, a continuación, se requiere que inicie sesión en el nuevo cuando realiza una conexión remota a su equipo de destino. Este segundo inicio de sesión podría pasar credenciales a la máquina de destino, exponerlos a Pass-the-Hash o ataques Pass-the-Ticket.

Con Credential Guard remoto a Windows Defender, Windows Server 2016 implementa un inicio de sesión único para las sesiones de escritorio remoto, lo que elimina la necesidad de volver a escribir su nombre de usuario y contraseña. En su lugar, aprovecha las credenciales que ya ha usado para iniciar sesión en el equipo local. Para usar a de Credential Guard remoto de Windows Defender, el cliente de escritorio remoto y el servidor deben cumplir los siguientes requisitos:

- Debe estar unido a un dominio de Active Directory y estar en el mismo dominio o un dominio con una relación de confianza.

- Debe usar la autenticación Kerberos.

- Debe estar ejecutando al menos Windows 10 versión 1607 o Windows Server 2016.  

- Se requiere la aplicación de Windows clásica de escritorio remoto. La aplicación de la plataforma Windows Universal de escritorio remoto no es compatible con Windows Defender remota Credential Guard.

Puede habilitar a de Credential Guard remoto de Windows Defender mediante el uso de una configuración del registro en el servidor de escritorio remoto y directiva de grupo o un parámetro de conexión a Escritorio remoto en el cliente de escritorio remoto. Para obtener más información sobre la habilitación de Credential Guard remoto de Windows Defender, consulte [credenciales proteger escritorio remoto con Windows Defender remota Credential Guard](https://docs.microsoft.com/windows/access-protection/remote-credential-guard). Como con Windows Defender Credential Guard, puede usar a de Credential Guard remoto de Windows Defender para ayudar a proteger las identidades con privilegios en Windows Server 2016.

### <a name="secure-the-operating-system-to-run-your-apps-and-infrastructure"></a>Proteger el sistema operativo para ejecutar sus aplicaciones e infraestructura
Evitar las amenazas de Internet también requiere buscar y bloquear malware y los ataques que intentan obtener el control subverting las prácticas estándar de funcionamiento de su infraestructura. Si los atacantes pueden obtener un sistema operativo o la aplicación se ejecute de forma no predeterminados, que no es viable, es probable que usan ese sistema para realizar acciones malintencionadas. Windows Server 2016 proporciona niveles de protección que bloquean los atacantes externos que ejecuta software malintencionado o aprovechando las vulnerabilidades. El sistema operativo tiene un rol activo en la protección de aplicaciones e infraestructura por los administradores a la actividad que indica que se ha infringido ya un sistema de alertas.

#### <a name="windows-defender-device-guard"></a>Device Guard de Windows Defender
Windows Server 2016 incluye la protección de dispositivos de Windows Defender para asegurarse de que el software de confianza solo se puede ejecutar en el servidor. Utiliza la seguridad basada en virtualización, puede limitar qué archivos binarios se pueden ejecutar en el sistema basado en directiva de la organización. Si nada más, excepto los archivos binarios especificados intenta ejecutar, Windows Server 2016 lo bloquea y registra el intento fallido de para que los administradores pueden ver que ha habido posibles brechas. Notificación de incumplimiento es una parte fundamental de los requisitos de cumplimiento del RGPD.

Protección de dispositivos de Windows Defender también se integra con PowerShell, por lo que puede autorizar a qué secuencias de comandos pueden ejecutarse en el sistema. En versiones anteriores de Windows Server, los administradores podrían omitir el cumplimiento de la integridad de código mediante la simple eliminación de la directiva desde el archivo de código. Con Windows Server 2016, puede configurar una directiva que está firmada por la organización para que solo una persona con acceso al certificado que firmó la directiva puede cambiar la directiva.

#### <a name="control-flow-guard"></a>Protección de flujo de control 
Windows Server 2016 también incluye protección integrada frente a algunas clases de ataques de corrupción de memoria. Los servidores de aplicación de revisiones es importante, pero siempre hay posibilidades de que el malware podría estar desarrollado en una vulnerabilidad que no se hayan identificado. Algunos de los métodos más comunes para aprovechar estas vulnerabilidades son proporcionar datos nuevos o inusuales extremos a un programa en ejecución. Por ejemplo, un atacante puede aprovechar una vulnerabilidad de desbordamiento de búfer proporcionando más entradas para un programa que se esperaba y saturación en el área reservado por el programa para almacenar una respuesta. Esto puede dañar la memoria adyacente que podría contener un puntero de función.

Cuando el programa llama a través de esta función, a continuación, puede saltar a una ubicación no deseada, especificada por el atacante. Estos ataques son también conocido como ataques orientados al salto de programación (JOP). Protección de flujo de control evita los ataques JOP colocando restricciones estrictas en qué código de la aplicación puede ser ejecutada: especialmente indirecta llamar a las instrucciones. Agrega las comprobaciones de seguridad ligero para identificar el conjunto de funciones de la aplicación que constituyen destinos válidos para las llamadas indirectas. Cuando se ejecuta una aplicación, comprueba que estos destinos de llamada indirecta son válidos.

Si se produce un error en la comprobación de protección de flujo de Control en tiempo de ejecución, Windows Server 2016 finaliza inmediatamente el programa, interrumpir cualquier vulnerabilidad de seguridad que intenta llamar indirectamente una dirección no válida. Protección de flujo de control proporciona una capa de protección adicional importante a Device Guard. Si se ha puesto en peligro una aplicación de la lista de permitidos, sería capaz de ejecutar desactivada por Device Guard, dado que la protección del dispositivo filtrado vería que la aplicación se ha firmado y se considera de confianza.

Pero, puesto que puede identificar la protección de flujo de Control si la aplicación se ejecuta en un orden no predeterminados, que no es viable, el ataque produciría un error, para impedir que la aplicación se ejecute en peligro. Juntas, estas protecciones dificultan a los atacantes inyectar código malintencionado en software que se ejecuta en Windows Server 2016.

Los desarrolladores que crean aplicaciones donde se controlarán los datos personales se recomienda habilitar la protección de flujo de Control (CFG) en sus aplicaciones. Esta característica está disponible en Microsoft Visual Studio 2015 y se ejecuta en "Compatible con CFG" de Windows, libera el x86 y x64 para escritorio y servidor de Windows 10 y Windows 8.1 Update (KB3000850). No tienes que habilitar CFG para todas las partes del código, como una mezcla de CFG habilitado y código no CFG habilitado se ejecutará correctamente. Pero no se puede habilitar CFG para todo el código puede abrir brechas en la protección. Además, CFG habilitado código funciona bien en las versiones "CFG consciente" de Windows y, por tanto, es totalmente compatible con ellos.

#### <a name="windows-defender-antivirus"></a>Antivirus de Windows Defender
Windows Server 2016 incluye la industria iniciales, active capacidades de detección de Windows Defender para bloquear malware conocido. Windows Defender Antivirus (AV) funciona junto con la protección de dispositivos de Windows Defender y protección de flujo de Control para evitar que código malintencionado de cualquier tipo que se instalen en sus servidores. Está activado de forma predeterminada, el administrador no necesita realizar ninguna acción para que pueda empezar a trabajar. Windows Defender AV también está optimizado para admitir los distintos roles de servidor en Windows Server 2016. En el pasado, los atacantes usan shells como PowerShell para iniciar programas malintencionados binario. En Windows Server 2016, PowerShell se integra ahora con Windows Defender AV para examinar en busca de malware antes de iniciar el código.

Antivirus de Windows Defender es una solución antimalware integrada que proporciona administración de seguridad y antimalware para escritorios, equipos portátiles y servidores. Windows Defender AV se ha mejorado significativamente desde que se presentó en Windows 8. Antivirus de Windows Defender en Windows Server utiliza varias vertientes para mejorar antimalware:

- La **protección de entrega en la nube** ayuda a detectar y bloquear nuevo malware en segundos, incluso si el malware no se ha visto nunca antes.

- El **Contexto local enriquecido** mejora la manera en que se identifica el malware. Windows Server informa a Windows Defender AV no sólo sobre contenido, como archivos y los procesos, sino también el contenido procedencia, donde se ha almacenado y mucho más. 

- **Sensores globales amplio** ayudar a mantener Windows Defender AV actual y tener en cuenta incluso el malware más reciente. Esto se logra de dos maneras: mediante la recopilación de los datos de contexto local enriquecido de extremos y analizando los datos de forma centralizada.

- **Corrección de alteraciones** ayuda a protegerse de Windows Defender AV propio contra los ataques de malware. Por ejemplo, Windows Defender AV utiliza procesos protegidos, lo que impide que los procesos de confianza intenta alterar los componentes de Windows Defender AV, sus claves del registro y así sucesivamente.

- **Características de nivel empresarial** dar a los profesionales de TI las herramientas y opciones de configuración necesarias para que Windows Defender AV una solución antimalware de clase empresarial.

#### <a name="enhanced-security-auditing"></a>Auditoría de seguridad mejorada 
Windows Server 2016 activamente alerta a los administradores de posibles intentos de incumplimiento con la auditoría de seguridad mejorada que proporciona información más detallada, que se puede usar para la detección de ataques más rápida y el análisis forense. Registra los eventos de protección de flujo de Control, Windows Defender Device Guard y otras características de seguridad en una ubicación, lo que facilita que los administradores determinen qué sistemas pueden estar en peligro.

Se incluyen nuevas categorías de eventos:

- **Pertenencia a grupos de auditoría.** Permite auditar la información de pertenencia a grupo en el token de inicio de sesión del usuario. Se generan eventos cuando las pertenencias a grupos se enumerar o consultar en el equipo donde se creó la sesión de inicio de sesión. 
 
- **Auditar la actividad de Plug and Play.** Le permite auditar cuando plug and play detecta un dispositivo externo, lo que podría contener código malintencionado. Eventos Plug and Play pueden utilizarse para realizar un seguimiento de cambios en el hardware del sistema. Una lista de identificadores de proveedor de hardware se incluye en el evento.

Windows Server 2016 se integra fácilmente con sistemas de administración (SIEM) de eventos de incidente de seguridad, como Microsoft Operations Management Suite (OMS), que puede incorporar la información en los informes de inteligencia en posibles infracciones. La profundidad de la información proporcionada por la auditoría mejorada permite a los equipos de seguridad identificar y reaccionar ante posibles infracciones de forma más rápida y eficazmente.

### <a name="secure-virtualization"></a>Virtualización segura
Las empresas hoy en día virtualizar todo lo que pueden, desde SQL Server a SharePoint para los controladores de dominio de Active Directory. Máquinas virtuales (VM) simplemente hacer más fácil de implementar, administrar, servicio y automatizar su infraestructura. Pero en cuanto a seguridad, tejidos de virtualización en peligro se han convertido en un nuevo vector de ataque que es difícil defenderse contra: hasta ahora. Desde una perspectiva de RGPD, debe pensar acerca de cómo proteger las máquinas virtuales que desea proteger servidores físicos, incluido el uso de tecnología de VM TPM.

Windows Server 2016 cambia fundamentalmente cómo las empresas pueden proteger virtualización, mediante la inclusión de varias tecnologías que le permiten crear máquinas virtuales que se ejecute solo en su propio fabric; ayudar a proteger desde el almacenamiento, red y dispositivos de host que se ejecutan.

#### <a name="shielded-virtual-machines"></a>Máquinas virtuales blindadas
Las mismas cosas que hacer tan fáciles de migrar las máquinas virtuales, copia de seguridad y replicación, también que sean más fáciles modificar y copiar. Una máquina virtual es simplemente un archivo, por lo que no está protegido en la red, en el almacenamiento, en las copias de seguridad, o en otro lugar. Otro problema es que los administradores del tejido: si es un administrador de almacenamiento o un administrador de red, tienen acceso a todas las máquinas virtuales.

Un administrador en el tejido en peligro puede hacer fácilmente en peligro datos entre máquinas virtuales. Todo el atacante debe hacer es usar las credenciales en peligro para copiar los archivos de máquina virtual que les gusta en una unidad USB y recorrer fuera de la organización, donde pueden tener acceso a esos archivos de máquina virtual desde cualquier otro sistema. Si cualquiera de esas máquinas virtuales robadas fuera un controlador de dominio de Active Directory, por ejemplo, el atacante podría fácilmente ver el contenido y usar técnicas de fuerza bruta estén disponibles para averiguar las contraseñas en la base de datos de Active Directory, en última instancia, concédales acceso a todo lo demás dentro de su infraestructura.

Windows Server 2016 presenta blindadas máquinas virtuales (VM blindadas) para ayudar a proteger frente a escenarios como la que acabamos de describir. Las máquinas virtuales blindadas incluyen un dispositivo TPM virtual, que permite a las organizaciones a aplicar el cifrado de BitLocker a las máquinas virtuales y asegurarse de que se ejecutan solo en hosts de confianza para ayudar a protegerse frente a los administradores de host, red y almacenamiento en peligro. Las máquinas virtuales blindadas se crean mediante la generación 2 máquinas virtuales que admiten el firmware Unified Extensible Firmware Interface (UEFI) y tienen TPM virtual.

#### <a name="host-guardian-service"></a>Servicio guardián de host
Junto con máquinas virtuales blindadas, el servicio de protección de Host es un componente esencial para la creación de un tejido de virtualización segura. Su trabajo consiste en dar fe de la salud de un host de Hyper-V antes de que una VM blindada para arrancar o para migrar a ese host. Contiene las claves de las máquinas virtuales blindadas y no liberará hasta que se garantiza el estado de seguridad. Hay dos maneras en que puede requerir que los hosts de Hyper-V dar fe de que el servicio de protección de Host.

La primera y más segura son atestación de confianza de hardware. Esta solución requiere que se ejecutan las máquinas virtuales blindadas en hosts que tienen los chips de TPM 2.0 y UEFI 2.3.1. Este hardware debe proporcionar el arranque medido y la información de integridad del kernel del sistema operativo requerido por el servicio de protección de Host para garantizar el host de Hyper-V no se ha alterado.

Las organizaciones de TI tienen la alternativa de con la atestación de administrador de confianza, que puede ser conveniente si el hardware de TPM 2.0 no está en uso en su organización. Este modelo de atestación es fácil de implementar porque hosts simplemente se colocan en un grupo de seguridad y el servicio de protección de Host está configurado para permitir que las VM blindadas se ejecuten en hosts que son miembros del grupo de seguridad. Con este método, no hay ninguna medida compleja para asegurarse de que el equipo host no se ha manipulado. Sin embargo, se elimina la posibilidad de sin cifrar unidades de máquinas virtuales saliendo por la puerta de USB o que la máquina virtual se ejecutará en un host no autorizado. Esto es porque los archivos de máquina virtual no se ejecutarán en cualquier equipo que no estén en el grupo designado. Si aún no dispone de hardware de TPM 2.0, puede iniciar con la atestación de administrador de confianza y cambie a la atestación de confianza de hardware cuando se actualiza el hardware.

#### <a name="virtual-machine-trusted-platform-module"></a>Módulo de plataforma segura de máquina virtual
Windows Server 2016 es compatible con TPM para las máquinas virtuales, lo que le permite admitir tecnologías de seguridad avanzadas, como el cifrado de unidad BitLocker® en las máquinas virtuales. Puede habilitar la compatibilidad TPM en cualquier máquina virtual de Hyper-V de generación 2 con el Administrador de Hyper-V o el cmdlet Enable-VMTPM Windows PowerShell.

Puede proteger TPM virtual (vTPM) mediante el uso de las claves de cifrado locales almacenados en el host o almacenada en el servicio de protección de Host. Por lo tanto, mientras que el servicio de protección de Host requiere más infraestructura, también ofrece más protección.

#### <a name="distributed-network-firewall-using-software-defined-networking"></a>Firewall de red distribuida utilizando redes definidas por software
Es una manera de mejorar la protección en entornos virtualizados segmentar la red de forma que permite a las VM comunicarse solo a los sistemas específicos necesarios para funcionar. Por ejemplo, si la aplicación no necesita conectarse a Internet, puede dividirlo desactiva, lo que elimina esos sistemas como destinos de los intrusos externos. Las redes definidas por software (SDN) en Windows Server 2016 incluyen un firewall de red distribuida que permite crear dinámicamente las directivas de seguridad que pueden proteger sus aplicaciones frente a ataques procedentes de dentro o fuera de una red. Este servidor de seguridad de red distribuida añade capas a la seguridad por lo que le permite aislar las aplicaciones en la red. Las directivas pueden aplicarse en cualquier lugar en toda la infraestructura de red virtual, aislar el tráfico de máquina virtual a otra, el tráfico de host de máquina virtual o el tráfico de Internet de VM en caso necesario, para los sistemas individuales que se han comprometido o mediante programación a través de varias subredes. Capacidades de red definida por software de Windows Server 2016 también permiten enrutar o para reflejar el tráfico entrante a aplicaciones virtuales que no sean de Microsoft. Por ejemplo, puede elegir enviar todo el tráfico de correo electrónico a través de un dispositivo virtual Barracuda para la protección de filtrado de correo no deseado adicional. Esto le permite fácilmente la capa de seguridad adicional tanto en el entorno local o en la nube.

### <a name="other-gdpr-considerations-for-servers"></a>Otras consideraciones de RGPD para servidores
El RGPD incluye requisitos explícitos de notificación de incumplimiento donde significa una vulneración de datos personales, "_una infracción de seguridad que dan lugar a la destrucción accidental o ilícita, pérdida, la modificación, la divulgación no autorizadas, o acceso a, personal datos transmitidos, almacenada o procesada de otro modo._ "  Obviamente, no puede empezar a avanzar para satisfacer los requisitos estrictos de notificación de RGPD en 72 horas si no puede detectar la brecha en primer lugar.

Como se indica en las notas de Windows Security Center, [brecha de Post: Tratar las amenazas avanzadas](http://wincom.blob.core.windows.net/documents/Post_Breach_Dealing_with_Advanced_Threats_Whitepaper.pdf)

> "_a diferencia de infracción anterior, posterior a la infracción se supone una infracción ya se ha producido, que actúa como una caja negra y Crime escena investigador (CSI). Posterior a la infracción proporciona a los equipos de seguridad la información y herramientas necesitan para identificar, investigar y responder a los ataques que en caso contrario, se mantendrá sin detectar y a continuación el radar._ "

En esta sección veremos cómo Windows Server puede ayudarle a cumplir sus obligaciones de notificación de infracción de RGPD. Esto comienza con la comprensión de los datos subyacentes de amenazas disponibles en Microsoft que se recopilan y analizan para ayudarle y cómo, a través de Windows Defender amenaza protección avanzada (ATP), que los datos pueden ser importantes para usted.

#### <a name="insightful-security-diagnostic-data"></a>Datos de diagnóstico de seguridad detallados
Durante casi dos décadas, Microsoft ha activando las amenazas en inteligencia útil que puede ayudar a reforzar su plataforma y proteger a los clientes. En la actualidad, con las grandes ventajas informáticas que ofrece la nube, estamos encontrando nuevas maneras de usar nuestros motores de análisis enriquecidos controlados por medio de inteligencia contra amenazas para proteger a nuestros clientes.

Al aplicar una combinación de procesos manuales y automatizados, aprendizaje automático y expertos humanos, podemos crear un gráfico de seguridad inteligente que aprende de sí mismo y evoluciona en tiempo real, reduciendo nuestro tiempo colectivo para detectar y responder a nuevos incidentes a través de nuestros productos.

![Gráfico de seguridad de la inteligencia de Microsoft](../media/GDPR-Windows-Server-Overview/gdpr-intelligent-security-graph.png)

El ámbito de amenazas de Microsoft abarca, literalmente, miles de millones de puntos de datos: 35 millones de mensajes analizan mensualmente, mil millones de 1 a los clientes a través de los segmentos de enterprise y consumidor obtener acceso a más de 200 cloud services y las autenticaciones de 14 millones realizadas diaria. Todos estos datos se reúne en su nombre por Microsoft para crear el gráfico de seguridad inteligente que puede ayudar a proteger su casa de forma dinámica para mantener la seguridad, ser productivo y cumplir los requisitos del RGPD.

#### <a name="detecting-attacks-and-forensic-investigation"></a>Detección de ataques e investigación forense
Incluso las mejores defensas de extremo se pueden infringir a la larga, a medida que los ciberataques se vuelven más sofisticados y dirigidos. Dos funcionalidades pueden usarse para ayudar con la detección de brechas potenciales - protección de amenazas avanzada de Windows Defender (ATP) y Microsoft Advanced Threat Analytics (ATA).

La Protección contra amenazas avanzada de Windows Defender (ATP) te ayuda a detectar, investigar y responder a ataques avanzados e infracciones de datos en las redes. Los tipos de infracciones de datos espera el RGPD protegerse frente a través de las medidas de seguridad técnica para garantizar la confidencialidad en curso, integridad y disponibilidad de los datos personales y sistemas de procesamiento.

Entre las ventajas principales de Windows Defender ATP son los siguientes:

- **Detectar el modo no detectables.** Sensores integrados profunda en el núcleo del sistema operativo, los expertos de seguridad de Windows y óptica único entre más de mil millones de 1 máquinas y las señales en todos los servicios de Microsoft.

- **Integrada, no de agregada.** Sin agente, con un impacto mínimo, con tecnología de nube; y de alto rendimiento administración sencilla con ninguna implementación. 

- **Panel único para la seguridad de Windows.** Explore los 6 meses de enriquecido, máquina: escala de tiempo, unificar los eventos de seguridad de ATP de Windows Defender, Antivirus de Windows Defender y Windows Defender Device Guard.

- **Potencia de Microsoft graph.** Aprovecha la seguridad de inteligencia de Microsoft Graph para integrar la detección y la exploración con suscripción a Office 365 ATP, para seguir la pista y responder a los ataques.

Lee más información en [What’s new in the Windows Defender ATP Creators Update preview](https://blogs.microsoft.com/microsoftsecure/2017/03/13/whats-new-in-the-windows-defender-atp-creators-update-preview/).

ATA es un producto local que ayuda a detectar el riesgo de la identidad de una organización. ATA puede capturar y analizar el tráfico de red para la autenticación, autorización y protocolos (como Kerberos, DNS, RPC, NTLM y otros protocolos) de recopilación de información. ATA usa estos datos para crear un perfil de comportamiento acerca de los usuarios y otras entidades en una red para que pueden detectar anomalías y patrones de ataque conocidos. En la tabla siguiente se enumera los tipos de ataque detectados por ATA.


|Tipo de ataque |Descripción |
|---------|---------|
|Ataques malintencionados |Estos ataques se detectan mediante la búsqueda de los ataques desde una lista conocida de los tipos de ataque, incluidos:<ul><li>PASS-the-Ticket (PtT)</li><li>PASS-the-Hash (PtH)</li><li>Overpass-the-Hash</li><li>PAC falsificado (MS14-068)</li><li>Golden Ticket</li><li>Replicaciones malintencionadas</li><li>Reconocimiento</li><li>Fuerza bruta</li><li>Ejecución remota</li></ul>Para obtener una lista completa de los ataques malintencionados que pueden detectarse y su descripción, consulte [qué actividades sospechosas puede detectar ATA?](https://docs.microsoft.com/advanced-threat-analytics/understand-explore/ata-threats).|
|Comportamiento anómalo |Estos ataques se detectan mediante el uso de análisis del comportamiento y utilicen aprendizaje automático para identificar actividades cuestionables, incluidos:<ul><li>Inicios de sesión anómalos</li><li>Amenazas desconocidas</li><li>Uso compartido de contraseña</li><li>Desplazamiento lateral</li></ul>|
|Los riesgos y problemas de seguridad |Estos ataques se detectan echando un vistazo a la red actual y la configuración del sistema, incluidos:<ul><li>Confianza rota</li><li>Protocolos débiles</li><li>Vulnerabilidades conocidas de protocolos</li></ul>|

Puede usar ATA para ayudar a detectar los atacantes intentan comprometer las identidades con privilegios. Para obtener más información sobre la implementación de ATA, vea los temas de implementación, diseño y planeación de la [documentación de Advanced Threat Analytics](https://docs.microsoft.com/advanced-threat-analytics/).

## <a name="related-content-for-associated-windows-server-2016-solutions"></a>Contenido relacionado de soluciones de Windows Server 2016 asociados

- **Antivirus de Windows Defender:** https://www.youtube.com/watch?v=P1aNEy09NaI y https://docs.microsoft.com/windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10

- **Protección contra amenazas avanzada de Windows Defender:** https://www.youtube.com/watch?v=qxeGa3pxIwg y https://docs.microsoft.com/windows/threat-protection/windows-defender-atp/configure-server-endpoints-windows-defender-advanced-threat-protection

- **Protección de dispositivos de Windows Defender:** https://www.youtube.com/watch?v=F-pTkesjkhI y https://docs.microsoft.com/windows/device-security/device-guard/device-guard-deployment-guide

- **Windows Defender Credential Guard:** https://www.youtube.com/watch?v=F-pTkesjkhI y https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard

- **Protección de flujo de control:** https://msdn.microsoft.com/en-us/library/windows/desktop/mt637065(v=vs.85).aspx

- **Seguridad y control:** https://docs.microsoft.com/windows-server/security/security-and-assurance

## <a name="disclaimer"></a>Aviso de declinación de responsabilidades
Este artículo es un comentario sobre la GDPR, como la interpreta Microsoft, desde la fecha de publicación. Hemos dedicado mucho tiempo a la GDPR y creemos que hemos tenido muy en cuenta su propósito y significado. Pero la aplicación de GDPR se refiere a hechos muy concretos y no todos los aspectos y las interpretaciones de la GDPR están bien establecidos.

Como resultado, este artículo se proporciona solo con fines informativos y no se debería usar como asesoramiento legal o para determinar la manera en que la GDPR puede aplicarse para ti y tu organización. Te animamos a trabajar con un profesional con cualificación legal para hablar de la GDPR, de cómo se aplica de manera específica a tu organización y de la mejor manera para garantizar el cumplimiento.

MICROSOFT NO OTORGA NINGUNA GARANTÍA, YA SEA EXPRESA, IMPLÍCITA O REGLAMENTARIA, EN CUANTO A LA INFORMACIÓN DE ESTE ARTÍCULO. Este artículo se proporciona “tal cual”. La información y las opiniones expresadas en este artículo, incluidas las direcciones URL y otras referencias a sitios web de Internet, pueden sufrir cambios sin previo aviso.

Este artículo no proporciona derechos legales sobre ninguna propiedad intelectual en ningún producto de Microsoft.  Puedes copiar y usar este artículo solo para tu referencia interna.  

Publicado en septiembre de 2017<br>
Versión 1.0<br>
© 2017 Microsoft. Todos los derechos reservados.


