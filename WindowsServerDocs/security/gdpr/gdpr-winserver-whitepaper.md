---
title: Inicio de tu viaje por el Reglamento general de protección de datos (GDPR) para Windows Server 2016
description: Usa este artículo para comprender qué es GDPR y cuáles son los productos que Microsoft proporciona para ayudarte a empezar en el camino del cumplimiento.
ms.technology: techgroup-security
ms.topic: article
ms.date: 09/25/2017
ms.author: nirb
author: nirb-ms
ms.openlocfilehash: f196be152e339b229c4c476f73a2d4b0e7a644d5
ms.sourcegitcommit: 2082335e1260826fcbc3dccc208870d2d9be9306
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2019
ms.locfileid: "69980313"
---
# <a name="beginning-your-general-data-protection-regulation-gdpr-journey-for-windows-server"></a>Inicio del viaje de Reglamento general de protección de datos (RGPD) para Windows Server 

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

- **Derechos de privacidad personal mejorados.** Se ha reforzado la protección de datos para residentes de la UE garantizando que tienen el derecho de tener acceso a sus datos personales, a corregir imprecisiones de dichos datos, a borrar dichos datos, a oponerse al procesamiento de sus datos personales y a moverlos.

- **Aumento del deber de protección de datos personales.** Responsabilidad reforzada de las organizaciones que procesan datos personales, lo que proporciona una mayor claridad de la responsabilidad en la garantía del cumplimiento.

- **Informes de infracciones de datos personales obligatorios.** Las organizaciones que controlan datos personales son necesarias para notificar las infracciones de datos personales que suponen un riesgo para los derechos y las libertades de las personas a sus autoridades de control sin retraso debido y, cuando sea posible, en un plazo no posterior a 72 horas una vez que conozcan la infracción.

Como se podría esperar, la GDPR puede tener un impacto importante en tu empresa, lo que puede requerir que actualices las directivas de privacidad, implementes y refuerces los controles de protección de datos y los procedimientos de notificación de incumplimiento, implementes directivas muy transparentes e inviertas más en TI y formación. Microsoft Windows 10 puede ayudarte a abordar de forma eficaz y eficiente algunos de estos requisitos.

## <a name="personal-and-sensitive-data"></a>Datos personales y confidenciales
Como parte de tu esfuerzo por cumplir con la GDPR, debes comprender de qué manera la normativa define los datos personales y confidenciales, y cómo se relacionan esas definiciones con los datos mantenidos por tu organización. En función de esta comprensión, podrá detectar dónde se crean, procesan, administran y almacenan los datos.

La GDPR considera que los datos personales son cualquier información relacionada con una persona física identificada o identificable. Eso puede incluir tanto identificación directa (por ejemplo, la razón social) como la identificación indirecta (por ejemplo, información específica que hace que resulte evidente que es a ti a quien se refieren los datos). La GDPR también deja claro que el concepto de datos personales incluye identificadores en línea (por ejemplo, direcciones IP, id. de dispositivos móviles) y datos de ubicación.

La GDPR presenta definiciones específicas de datos genéticos (por ejemplo, la secuencia genética de una persona) y datos biométricos. Los datos genéticos y biométricos junto con otras subcategorías de datos personales (datos personales que revelan el origen étnico o racial, las opiniones políticas, las convicciones religiosas o filosóficas o la afiliación sindical: datos relativos a la salud; o datos relacionados con la vida sexual de la persona o la orientación sexual) se tratan como datos personales confidenciales según la GDPR. Los datos personales confidenciales requieren protecciones mejoradas y suelen requerir el consentimiento explícito de la persona de la que se van a procesar estos datos.

### <a name="examples-of-info-relating-to-an-identified-or-identifiable-natural-person-data-subject"></a>Ejemplos de información relacionada con una persona natural identificada o identificable (interesado)
En esta lista se proporcionan ejemplos de varios tipos de información que se regularán a través de GDPR. No se trata de una lista exhaustiva.

-   NOMBRE

-   Número de identificación (por ejemplo, SSN)

-   Datos de ubicación (por ejemplo, dirección particular)

-   Identificador en línea (por ejemplo, dirección de correo electrónico, nombres de pantalla, dirección IP, id. de dispositivo)

-   Datos seudónimos (por ejemplo, usando una clave para identificar individuos)

-   Datos genéticos (por ejemplo, muestras biológicas de una persona)

-   Datos biométricos (por ejemplo, huellas digitales, reconocimiento facial)

## <a name="getting-started-on-the-journey-towards-gdpr-compliance"></a>Tareas iniciales en el camino hacia el cumplimiento de GDPR
Dado todo lo que implica el cumplimiento de la GDPR, te recomendamos encarecidamente que no esperes a prepararte hasta que comience su aplicación. Debes revisar ahora tus prácticas de administración de datos y privacidad. Te recomendamos que comiences tu viaje hacia el cumplimiento de la GDPR centrándote en cuatro pasos clave:

-   **Cubierto.** Identifica qué datos personales tienes y dónde se encuentran. 

-   **No.** Controla cómo se usan los datos personales y se obtiene acceso a ellos.

-   **Proteger.** Establece controles de seguridad para evitar y detectar vulnerabilidades e infracciones de datos, y responder ante ellas.  

-   **Enviar.** Actúa sobre solicitudes de datos, infracciones de datos de informe y mantén la documentación necesaria.

    ![Diagrama sobre cómo funcionan conjuntamente los 4 pasos clave de GDPR](../media/GDPR-Windows-Server-Overview/gdpr-steps-diagram.png)

Para cada uno de los pasos, hemos descrito herramientas de ejemplo, recursos y características en distintas soluciones de Microsoft, que se pueden usar para ayudarte a cumplir los requisitos de ese paso. Aunque este artículo no es una guía completa de "procedimientos", hemos incluido vínculos para obtener más detalles y hay más información disponible en la [sección RGPD del centro de confianza de Microsoft](https://www.microsoft.com/en-us/trustcenter/privacy/gdpr).

## <a name="windows-server-security-and-privacy"></a>Seguridad y privacidad de Windows Server
RGPD requiere la implementación de las medidas de seguridad técnicas y organizativas adecuadas para proteger los datos personales y los sistemas de procesamiento. En el contexto del RGPD, los entornos de servidores físicos y virtuales pueden procesar potencialmente datos personales y confidenciales. El procesamiento puede significar cualquier operación o conjunto de operaciones, como la recopilación, el almacenamiento y la recuperación de datos.

La capacidad de cumplir este requisito y de implementar las medidas de seguridad técnicas adecuadas debe reflejar las amenazas a las que se enfrentan en el entorno de ti cada vez más hostil. El panorama actual de las amenazas de seguridad es el de unas amenazas agresivas y tenaces. En años anteriores, los atacantes malintencionados se centraban principalmente en obtener el reconocimiento de la comunidad a través de sus ataques la emoción que resulta de desconectar un sistema temporalmente. Desde entonces, la intención del atacante ha pasado a ser ganar dinero, por ejemplo, la retención de dispositivos y datos hasta que el propietario pague el rescate solicitado.

Los ataques modernos se centran cada vez más en el robo de propiedad intelectual a gran escala, la degradación de los sistemas dirigidos, que se pueden traducir en pérdidas financieras y, ahora, incluso el ciberterrorismo, que amenaza la seguridad de personas, empresas e intereses nacionales en todo el mundo. Estos atacantes suelen ser individuos con una alta formación y expertos en seguridad, algunos de los cuales están contratados por Estados nación con grandes presupuestos y recursos humanos aparentemente ilimitados. Estos tipos de amenaza requieren un enfoque que pueda hacer frente a este desafío.

No solo estas amenazas suponen un riesgo para tu capacidad de mantener el control de los datos personales o confidenciales que puedes tener, sino que también constituyen un riesgo material para toda tu empresa en general. Considere los datos recientes de McKinsey, Ponemon Institute, Verizon y Microsoft:

- El coste medio del tipo de la infracción de datos que la GDPR espera que notifique es de 3.500.000 $.

- El 63 % de estas infracciones implican contraseñas débiles o robadas que la GDPR espera que corrijas.

- Todos los días se crean y se propagan más de 300.000 nuevas muestras de malware, lo que hace que tu tarea de abordar la protección de datos resulte aún más difícil.

Tal como se ha mostrado con los ataques de ransomware recientes, una vez que se ha llamado a la plaga de Internet, los atacantes van después de objetivos más grandes que pueden permitirse pagar más, con consecuencias catastróficas. El RGPD incluye penalizaciones que hacen que sus sistemas, incluidos equipos de escritorio y portátiles, contengan de hecho datos personales y con objetivos enriquecidos.

Dos principios clave han guiado y continúan guiando el desarrollo de Windows:

- **Seguridad.** Los datos que el software y los servicios almacenen en nombre de nuestros clientes deben protegerse frente a daños y usarse o modificarse solo de la manera adecuada. Los modelos de seguridad deben ser fáciles de entender y compilar en sus aplicaciones.

- **Service.** Los usuarios deben tener control sobre cómo se usan sus datos. Las directivas para el uso de la información deben ser claras para el usuario. Los usuarios deben tener el control de Cuándo y si reciben información para hacer el mejor uso de su tiempo. Los usuarios deben ser sencillos para especificar el uso adecuado de su información, incluido el control del uso del correo electrónico que envían.

Microsoft ha permanecido sólidas en relación con estos principios, tal y como lo mencionó recientemente el CEO de Microsoft, Satya Nadella 

> "_A medida que el mundo sigue cambiando y los requisitos empresariales evolucionan, algunas cosas son coherentes: la demanda de un cliente de seguridad y privacidad"._

A medida que trabaja para cumplir con el RGPD, es importante conocer el rol de los servidores físicos y virtuales en la creación, el acceso, el procesamiento, el almacenamiento y la administración de datos que puedan calificarse como datos personales y potencialmente confidenciales en RGPD. Windows Server proporciona funcionalidades que le ayudarán a cumplir los requisitos de RGPD para implementar las medidas de seguridad técnicas y organizativas adecuadas para proteger los datos personales.

La postura de seguridad de Windows Server 2016 no es un Bolt. es un principio arquitectónico. Además, se puede entender mejor en cuatro entidades de seguridad:

- **Proteger.** Enfoque continuo e innovación en medidas preventivas; bloquee ataques conocidos y malware conocido.

- **Determinar.** Herramientas de supervisión integrales para ayudarle a detectar anomalías y responder a los ataques con mayor rapidez.

- **Frente.** Principales tecnologías de respuesta y recuperación además de una profunda experiencia de consultoría.

- **Aislar.** Aísle los componentes del sistema operativo y los secretos de datos, limite los privilegios de administrador y mida rigurosamente el estado del host.

Con Windows Server, su capacidad de proteger, detectar y defender contra los tipos de ataques que pueden provocar infracciones de datos se ha mejorado considerablemente. Dados los estrictos requisitos en torno a la notificación de infracciones en la GDPR, garantizar que los sistemas de escritorio y portátiles están bien protegidos disminuirán los riesgos a los que te enfrentas y que podrían dar lugar a costosas notificaciones y análisis de incumplimientos.

En la siguiente sección, verá cómo Windows Server proporciona funcionalidades que se ajustan de forma cuadrada en la fase de protección del recorrido de cumplimiento de RGPD. Estas funciones se dividen en tres escenarios de protección:

- **Proteja sus credenciales y limite los privilegios de administrador.** Windows Server 2016 ayuda a implementar estos cambios, para evitar que el sistema se use como punto de partida para las intrusiones posteriores.

- **Proteja el sistema operativo para ejecutar las aplicaciones y la infraestructura.** Windows Server 2016 proporciona niveles de protección, lo que ayuda a impedir que los atacantes externos ejecuten software malintencionado o vulnerabilidades de seguridad.

- **Virtualización segura.** Windows Server 2016 habilita la virtualización segura mediante el Virtual Machines blindado y el tejido protegido. Esto le ayuda a cifrar y ejecutar las máquinas virtuales en hosts de confianza en el tejido, con lo que se protegen mejor contra ataques malintencionados.

Estas funciones, que se describen con más detalle a continuación con referencias a requisitos específicos de RGPD, se basan en la protección avanzada de dispositivos que ayuda a mantener la integridad y la seguridad del sistema operativo y los datos.

Una disposición clave dentro de RGPD es la protección de datos por diseño y, de forma predeterminada, y ayuda a la capacidad de cumplir esta disposición son características de Windows 10 como el cifrado de dispositivos de BitLocker. BitLocker usa la tecnología de Módulo de plataforma segura (TPM), que proporciona funciones relacionadas con la seguridad basadas en hardware. Este chip de procesador criptográfico incluye varios mecanismos de seguridad física para que sea resistente a alteraciones y el software malintencionado no pueda alterar las funciones de seguridad del TPM.

El chip incluye varios mecanismos de seguridad física que hacen que sea resistente a las alteraciones y que las funciones de seguridad no permitan que el software malintencionado realice alteraciones. Algunas de las principales ventajas de usar la tecnología TPM son que permiten:

-   Generar, almacenar y limitar el uso de claves criptográficas.

-   Usar la tecnología del TPM para la autenticación de dispositivos de plataforma mediante el uso de la clave RSA exclusiva del TPM, que se grabará en sí misma.

-   Garantizar la integridad de la plataforma llevando y almacenando medidas de seguridad.

La protección de dispositivo avanzada adicional relevante para el funcionamiento sin las infracciones de datos incluyen el Arranque seguro de Windows para ayudar a mantener la integridad del sistema al garantizar que el malware no se puede iniciar antes de las defensas del sistema.

## <a name="windows-server-supporting-your-gdpr-compliance-journey"></a>Windows Server: Compatibilidad con el recorrido de cumplimiento de RGPD
Las principales características de Windows Server pueden ayudarle a implementar de forma eficaz y eficaz los mecanismos de seguridad y privacidad que RGPD requiere para el cumplimiento. Aunque el uso de estas características no garantizará su cumplimiento, se apoyará en sus esfuerzos por hacerlo.

El sistema operativo del servidor se encuentra en una capa estratégica de la infraestructura de la organización, lo que ofrece nuevas oportunidades para crear niveles de protección frente a ataques que podrían robar datos e interrumpir su negocio. Los aspectos clave del RGPD, como la privacidad por diseño, la protección de datos y Access Control deben abordarse dentro de la infraestructura de TI en el nivel de servidor.

Al trabajar para proteger la identidad, el sistema operativo y los niveles de virtualización, Windows Server 2016 ayuda a bloquear los vectores de ataque comunes que se usan para obtener acceso ilícito a los sistemas: credenciales robadas, malware y un tejido de virtualización en peligro. Además de reducir el riesgo empresarial, los componentes de seguridad integrados en Windows Server 2016 ayudan a satisfacer los requisitos de cumplimiento de normas de seguridad del sector y del gobierno clave. 

Estas protecciones de identidad, sistema operativo y virtualización permiten proteger mejor su centro de recursos que ejecuta Windows Server como una máquina virtual en cualquier nube, y limitar la capacidad de los atacantes de poner en peligro las credenciales, iniciar malware y permanecer sin detectar en el Storage. Del mismo modo, cuando se implementa como un host de Hyper-V, Windows Server 2016 ofrece garantía de seguridad para los entornos de virtualización a través de las funciones blindadas Virtual Machines y el Firewall distribuido. Con Windows Server 2016, el sistema operativo del servidor se convierte en un participante activo en su centro de seguridad.

### <a name="protect-your-credentials-and-limit-administrator-privileges"></a>Proteja sus credenciales y limite los privilegios de administrador 
El control sobre el acceso a los datos personales y los sistemas que procesan los datos, es un área con el RGPD que tiene requisitos específicos, incluido el acceso por parte de los administradores. Las identidades con privilegios son aquellas que tienen privilegios elevados, como cuentas de usuario que son miembros de los grupos administradores de dominio, administradores de empresas, administradores locales o incluso usuarios avanzados. Estas identidades también pueden incluir cuentas a las que se les han concedido privilegios directamente, como la realización de copias de seguridad, el apagado del sistema u otros derechos que aparecen en el nodo asignación de derechos de usuario de la consola de la Directiva de seguridad local.

Como principio de control de acceso general y en línea con el RGPD, debe proteger estas identidades privilegiadas frente a posibles atacantes. En primer lugar, es importante comprender cómo se ven comprometidas las identidades. a continuación, puede planear impedir que los atacantes obtengan acceso a estas identidades con privilegios.

#### <a name="how-do-privileged-identities-get-compromised"></a>¿Cómo se comprometen las identidades con privilegios?
Las identidades con privilegios pueden ponerse en peligro cuando las organizaciones no tienen directrices para protegerlas. A continuación, se exponen algunos ejemplos:

- **Más privilegios de los necesarios.** Uno de los problemas más comunes es que los usuarios tienen más privilegios de los necesarios para realizar su función de trabajo. Por ejemplo, un usuario que administre DNS podría ser un administrador de AD. A menudo, esto se hace para evitar la necesidad de configurar distintos niveles de administración. Sin embargo, si una cuenta se ve comprometida, el atacante automáticamente tendrá privilegios elevados.

- **Sesión iniciada constantemente con privilegios elevados.** Otro problema común es que los usuarios con privilegios elevados pueden usarlo durante un tiempo ilimitado. Esto es muy común con los profesionales de ti que inician sesión en un equipo de escritorio mediante una cuenta con privilegios, mantienen la sesión iniciada y usan la cuenta con privilegios para explorar la web y usar el correo electrónico (funciones de trabajo de ti típicas). La duración ilimitada de las cuentas con privilegios hace que la cuenta sea más susceptible a ataques y aumenta la probabilidad de que la cuenta se vea comprometida.

- **Investigación de ingeniería social.** La mayoría de amenazas de credenciales se inician investigando la organización y, a continuación, realizadas a través de la ingeniería social. Por ejemplo, un atacante puede realizar un ataque de suplantación de identidad por correo electrónico para poner en peligro cuentas legítimas (pero no necesariamente cuentas elevadas) que tengan acceso a la red de una organización. A continuación, el atacante utiliza estas cuentas válidas para realizar una investigación adicional en la red e identificar cuentas con privilegios que puedan realizar tareas administrativas. 

- **Aproveche las cuentas con privilegios elevados.** Incluso con una cuenta de usuario normal sin privilegios elevados en la red, los atacantes pueden obtener acceso a las cuentas con permisos elevados. Uno de los métodos más comunes para hacerlo es mediante el uso de los ataques Pass-The-hash o Pass-The-token. Para obtener más información sobre Pass-The-hash y otras técnicas de robo de credenciales, vea los recursos en la [Página Pass-The-hash (PtH)](https://technet.microsoft.com/dn785092.aspx).

Hay otros métodos que los atacantes pueden usar para identificar y poner en peligro las identidades con privilegios (con nuevos métodos que se crean cada día). Por lo tanto, es importante que establezca prácticas para que los usuarios inicien sesión con cuentas con privilegios mínimos para reducir la capacidad de los atacantes de obtener acceso a las identidades con privilegios. En las secciones siguientes se describe la funcionalidad donde Windows Server puede mitigar estos riesgos.

#### <a name="just-in-time-admin-jit-and-just-enough-admin-jea"></a>Administrador Just-in-Time (JIT) y solo un administrador suficiente (JEA)
Aunque la protección contra los ataques Pass-The-hash o Pass-The-ticket es importante, las credenciales de administrador se pueden robar por otros medios, como la ingeniería social, los empleados descontentos y la fuerza bruta. Por lo tanto, además de aislar las credenciales lo máximo posible, también desea una manera de limitar el alcance de los privilegios de nivel de administrador en caso de que se vean comprometidos.

En la actualidad, hay demasiadas cuentas de administrador con privilegios elevados, incluso si solo tienen un área de responsabilidad. Por ejemplo, a menudo se conceden privilegios de nivel de administrador de dominio a un administrador de DNS que requiera un conjunto muy reducido de privilegios para administrar servidores DNS. Además, dado que estas credenciales se conceden para perpetuidad, no hay ningún límite en cuanto al tiempo que se puede usar.

Cada cuenta con privilegios de nivel de administrador de dominio innecesarios aumenta la exposición a los atacantes que buscan poner en peligro las credenciales. Para minimizar el área expuesta a ataques, desea proporcionar solo el conjunto específico de derechos que necesita un administrador para realizar el trabajo, y solo para el período de tiempo necesario para completarlo.

Mediante el uso de la administración suficiente y la administración Just-in-Time, los administradores pueden solicitar los privilegios específicos que necesitan para el período exacto de tiempo necesario. Para un administrador de DNS, por ejemplo, el uso de PowerShell para habilitar solo la administración suficiente le permite crear un conjunto limitado de comandos que están disponibles para la administración de DNS.

Si el administrador de DNS necesita realizar una actualización a uno de sus servidores, solicitaría acceso para administrar DNS mediante Microsoft Identity Manager 2016. El flujo de trabajo de solicitud puede incluir un proceso de aprobación, como la autenticación en dos fases, que podría llamar al teléfono móvil del administrador para confirmar su identidad antes de conceder los privilegios solicitados. Una vez concedidos, esos privilegios DNS proporcionan acceso al rol de PowerShell para DNS durante un intervalo de tiempo específico.

Imagínese este escenario si se robaron las credenciales del administrador de DNS. En primer lugar, puesto que las credenciales no tienen privilegios de administrador adjuntos, el atacante no podrá obtener acceso al servidor DNS (ni a ningún otro sistema) para realizar cambios. Si el atacante intenta solicitar privilegios para el servidor DNS, la autenticación de segundo factor le pedirá que confirmen su identidad. Puesto que no es probable que el atacante tenga el teléfono móvil del administrador de DNS, la autenticación no se realizará correctamente. Esto bloquearía el atacante del sistema y avisaría a la organización de TI de que las credenciales podrían estar en peligro.

Además, muchas organizaciones usan la solución de [contraseña de administrador local gratuita (laps)](http://aka.ms/laps) como un mecanismo de administración JIT sencillo pero eficaz para sus sistemas de servidor y cliente. La capacidad de LAPS proporciona la administración de contraseñas de cuentas locales de equipos Unidos a un dominio. Las contraseñas se almacenan en Active Directory (AD) y se protegen mediante y Access Control lista (ACL), por lo que solo los usuarios válidos pueden leerla o solicitar su restablecimiento.

Como se indica en la guía de mitigación de robo de credenciales de [Windows](https://www.microsoft.com/en-us/download/confirmation.aspx?id=54095), 

> "_las herramientas y las técnicas que los delincuentes usan para llevar a cabo el robo de credenciales y los ataques de reutilización mejoran, los atacantes malintencionados están encontrando más fácilmente alcanzar sus objetivos. El robo de credenciales suele basarse en prácticas operativas o en la exposición de credenciales de usuario, de modo que las mitigaciones eficaces requieren un enfoque holístico que aborda personas, procesos y tecnología. Además, estos ataques se basan en el ataque de robo de credenciales después de poner en peligro un sistema para expandir o conservar el acceso, por lo que las organizaciones deben contener infracciones rápidamente mediante la implementación de estrategias que impidan que los atacantes se muevan libremente y no se detecten en una red en peligro._ "

Una consideración de diseño importante para Windows Server fue la mitigación del robo de credenciales, en particular, las credenciales derivadas. Credential Guard proporciona una seguridad significativamente mejorada contra el robo de credenciales derivadas y la reutilización mediante la implementación de un importante cambio arquitectónico en Windows diseñado para ayudar a eliminar ataques de aislamiento basados en hardware en lugar de simplemente intentar defenderse contra ellos.

Al usar credenciales de protección de credenciales de Windows Defender, NTLM y Kerberos derivadas protegidas mediante la seguridad basada en la virtualización, se bloquean las técnicas y las herramientas de ataque de robo de credenciales usadas en muchos ataques dirigidos. Con la ejecución de malware en el sistema operativo con privilegios administrativos no se pueden extraer secretos que están protegidos con la seguridad basada en la virtualización. Aunque la protección de credenciales de Windows Defender es una mitigación eficaz, los ataques de amenazas persistentes probablemente se desplazarán a nuevas técnicas de ataque y también debe incorporar Device Guard, tal y como se describe a continuación, junto con otras estrategias de seguridad y arquitecturas.

#### <a name="windows-defender-credential-guard"></a>Protección de credenciales de Windows Defender
Credential Guard de Windows Defender utiliza la seguridad basada en la virtualización para aislar la información de credenciales, evitando que se intercepten los hash de contraseña o los vales de Kerberos. Utiliza un proceso de autoridad de seguridad local (LSA) aislado totalmente nuevo, al que no puede acceder el resto del sistema operativo. Todos los archivos binarios usados por la LSA aislada están firmados con certificados que se validan antes de iniciarlos en el entorno protegido, lo que hace que los ataques de tipo Pass-The-hash sean completamente ineficaces.

Credential Guard de Windows Defender usa:

- Seguridad basada en la virtualización (obligatorio). También se requiere:

    - CPU de 64 bits

    - Extensiones de virtualización de CPU, además de tablas de páginas extendidas

    - Hipervisor de Windows

- Arranque seguro (obligatorio)

- TPM 2.0 discreto o de firmware (preferido: proporciona el enlace al hardware)

Puede usar Credential Guard de Windows Defender para ayudar a proteger las identidades con privilegios mediante la protección de las credenciales y los derivados de las credenciales en Windows Server 2016. Para obtener más información sobre los requisitos de Credential Guard de Windows Defender, consulte [protección de las credenciales de dominio derivadas con protección de credenciales de Windows Defender](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard).

#### <a name="windows-defender-remote-credential-guard"></a>Protección de credenciales remota de Windows Defender
La protección de credenciales remota de Windows Defender en Windows Server 2016 y la actualización de aniversario de Windows 10 también ayuda a proteger las credenciales para los usuarios con conexiones a escritorio remoto. Anteriormente, cualquier persona que usara Servicios de Escritorio remoto tendría que iniciar sesión en su equipo local y, a continuación, volver a iniciar sesión cuando realizara una conexión remota a su equipo de destino. Este segundo inicio de sesión pasaría las credenciales a la máquina de destino y las exponería a ataques Pass-The-hash o Pass-The-ticket.

Con Credential Guard remoto de Windows Defender, Windows Server 2016 implementa el inicio de sesión único para sesiones de Escritorio remoto, lo que elimina el requisito de volver a escribir su nombre de usuario y contraseña. En su lugar, aprovecha las credenciales que ya ha usado para iniciar sesión en el equipo local. Para usar la protección de credenciales remota de Windows Defender, el cliente de Escritorio remoto y el servidor deben cumplir los siguientes requisitos:

- Debe estar unido a un dominio de Active Directory y estar en el mismo dominio o en un dominio con una relación de confianza.

- Debe usar la autenticación Kerberos.

- Debe ejecutar al menos Windows 10 versión 1607 o Windows Server 2016.  

- Se requiere la aplicación de Windows clásica Escritorio remoto. La aplicación de Plataforma universal de Windows de Escritorio remoto no es compatible con Credential Guard remoto de Windows Defender.

Puede habilitar Credential Guard remoto de Windows Defender mediante una configuración del registro en el servidor de Escritorio remoto y directiva de grupo o un parámetro Conexión a Escritorio remoto en el cliente de Escritorio remoto. Para obtener más información sobre cómo habilitar la protección de credenciales remotas de Windows Defender, consulte protección de credenciales de [escritorio remoto con protección de credenciales remotas de Windows Defender](https://docs.microsoft.com/windows/access-protection/remote-credential-guard). Al igual que con la protección de credenciales de Windows Defender, puede usar Credential Guard remoto de Windows Defender para ayudar a proteger las identidades con privilegios en Windows Server 2016.

### <a name="secure-the-operating-system-to-run-your-apps-and-infrastructure"></a>Protección del sistema operativo para ejecutar las aplicaciones y la infraestructura
La prevención de amenazas de Cyber también requiere la búsqueda y el bloqueo de malware y ataques que buscan obtener control mediante la reversión de las prácticas operativas estándar de la infraestructura. Si los atacantes pueden obtener un sistema operativo o una aplicación para que se ejecuten de manera no predeterminada y no viable, es probable que utilicen ese sistema para realizar acciones malintencionadas. Windows Server 2016 proporciona capas de protección que bloquean a los atacantes externos que ejecutan software malintencionado o vulnerabilidades de seguridad. El sistema operativo toma un rol activo en la protección de la infraestructura y las aplicaciones al avisar a los administradores de la actividad que indica que se ha infringido el sistema.

#### <a name="windows-defender-device-guard"></a>Device Guard de Windows Defender
Windows Server 2016 incluye Windows Defender Device Guard para asegurarse de que solo se puede ejecutar en el servidor software de confianza. Con la seguridad basada en la virtualización, puede limitar los archivos binarios que se pueden ejecutar en el sistema en función de la Directiva de la organización. Si algo que no sea el binario especificado intenta ejecutarse, Windows Server 2016 lo bloquea y registra el intento con error para que los administradores puedan ver que se ha producido una posible brecha. La notificación de infracciones es una parte fundamental de los requisitos de cumplimiento de RGPD.

Windows Defender Device Guard también se integra con PowerShell para que pueda autorizar qué scripts se pueden ejecutar en el sistema. En versiones anteriores de Windows Server, los administradores podían omitir el cumplimiento de la integridad de código simplemente eliminando la Directiva del archivo de código. Con Windows Server 2016, puede configurar una directiva que esté firmada por su organización para que solo una persona con acceso al certificado que firmó la directiva pueda cambiar la Directiva.

#### <a name="control-flow-guard"></a>Protección de flujo de control 
Windows Server 2016 también incluye protección integrada frente a algunas clases de ataques de daños en la memoria. La revisión de los servidores es importante, pero siempre hay una posibilidad de que se pueda desarrollar malware para una vulnerabilidad que todavía no se ha identificado. Algunos de los métodos más comunes para aprovechar estas vulnerabilidades son proporcionar datos inusuales o extremos a un programa en ejecución. Por ejemplo, un atacante puede aprovechar una vulnerabilidad de desbordamiento de búfer al proporcionar más entrada a un programa de lo esperado y saturar el área reservada por el programa para contener una respuesta. Esto puede dañar la memoria adyacente que podría contener un puntero de función.

Cuando el programa llama a través de esta función, puede saltar a una ubicación no deseada especificada por el atacante. Estos ataques también se conocen como ataques de programación orientada a saltos (JOP). La protección de flujo de control evita ataques de JOP al colocar restricciones estrictas sobre qué código de aplicación se puede ejecutar, especialmente instrucciones de llamadas indirectas. Agrega comprobaciones de seguridad ligeras para identificar el conjunto de funciones de la aplicación que son destinos válidos para las llamadas indirectas. Cuando se ejecuta una aplicación, comprueba que estos destinos de llamadas indirectas son válidos.

Si se produce un error en la comprobación de la protección del flujo de control en tiempo de ejecución, Windows Server 2016 finaliza inmediatamente el programa, interrumpiendo cualquier ataque que intente llamar indirectamente a una dirección no válida. La protección de flujo de control proporciona una capa de protección adicional importante a Device Guard. Si una aplicación de la lista blanca se ha puesto en peligro, se podría ejecutar sin activar en Device Guard, ya que el filtrado de Device Guard vería que la aplicación se firmó y se considera de confianza.

Sin embargo, como la protección de flujo de control puede identificar si la aplicación se ejecuta en un orden no predeterminado y no viable, se produciría un error en el ataque, lo que impidió la ejecución de la aplicación en peligro. En conjunto, estas protecciones hacen que sea muy difícil que los atacantes inserten malware en el software que se ejecuta en Windows Server 2016.

Se recomienda a los desarrolladores que creen aplicaciones donde se controlen los datos personales que habiliten la protección de flujo de control (CFG) en sus aplicaciones. Esta característica está disponible en Microsoft Visual Studio 2015 y se ejecuta en las versiones de Windows "compatibles con CFG": las versiones x86 y x64 para Desktop y Server de Windows 10 y Windows 8.1 Update (KB3000850). No tiene que habilitar CFG para todas las partes del código, ya que se ejecutará correctamente una combinación de CFG habilitada y el código no compatible con CFG. Pero si no se habilita CFG para todo el código, pueden abrirse brechas en la protección. Además, el código habilitado para CFG funciona correctamente en las versiones de Windows que no son compatibles con CFG y, por lo tanto, es totalmente compatible con ellos.

#### <a name="windows-defender-antivirus"></a>Antivirus de Windows Defender
Windows Server 2016 incluye las capacidades de detección activa y líder del sector de Windows Defender para bloquear malware conocido. Antivirus de Windows Defender (AV) funciona junto con Windows Defender Device Guard y la protección de flujo de control para impedir que se instale código malintencionado de cualquier tipo en los servidores. Está activada de forma predeterminada: el administrador no tiene que realizar ninguna acción para que empiece a funcionar. Windows Defender AV también está optimizado para admitir los distintos roles de servidor en Windows Server 2016. En el pasado, los atacantes usaban shells como PowerShell para iniciar código binario malintencionado. En Windows Server 2016, PowerShell ahora está integrado con Windows Defender AV para buscar malware antes de iniciar el código.

AV de Windows Defender es una solución antimalware integrada que ofrece administración de seguridad y antimalware para escritorios, equipos portátiles y servidores. Windows Defender AV se ha mejorado significativamente desde que se presentó en Windows 8. El antivirus de Windows Defender en Windows Server usa un enfoque de varias puntas para mejorar el antimalware:

- La **protección de entrega en la nube** ayuda a detectar y bloquear nuevo malware en segundos, incluso si el malware no se ha visto nunca antes.

- El **Contexto local enriquecido** mejora la manera en que se identifica el malware. Windows Server informa a AV de Windows Defender no solo sobre contenido como archivos y procesos, sino también de dónde procede el contenido, dónde se ha almacenado, etc. 

- Los **sensores globales extensivos** ayudan a mantener el AV de Windows Defender actualizado y a reconocer incluso el malware más reciente. Esto se logra de dos maneras: mediante la recopilación de los datos de contexto local enriquecido de extremos y analizando los datos de forma centralizada.

- La **prueba** de alteración ayuda a proteger el antivirus de Windows defender contra ataques de malware. Por ejemplo, Windows Defender AV usa procesos protegidos, que evitan que los procesos que no son de confianza intenten alterar los componentes de AV de Windows Defender, las claves del registro, etc.

- **Las características de nivel empresarial** proporcionan a los profesionales de ti las herramientas y las opciones de configuración necesarias para que Windows Defender AV sea una solución antimalware de clase empresarial.

#### <a name="enhanced-security-auditing"></a>Auditoría de seguridad mejorada 
Windows Server 2016 alerta activamente a los administradores ante posibles intentos de infracción con una auditoría de seguridad mejorada que proporciona información más detallada, que se puede usar para una detección de ataques más rápida y análisis forense. Registra los eventos de la protección de flujo de control, la protección de dispositivos de Windows Defender y otras características de seguridad en una ubicación, lo que facilita a los administradores determinar qué sistemas pueden estar en riesgo.

Las nuevas categorías de eventos incluyen:

- **Auditar pertenencia a grupos.** Permite auditar la información de pertenencia a grupos en el token de inicio de sesión de un usuario. Los eventos se generan cuando se enumeran o se consultan las pertenencias a grupos en el equipo donde se creó la sesión de inicio de sesión. 
 
- **Auditar la actividad de PnP.** Permite auditar Cuándo plug and Play detecta un dispositivo externo, que podría contener malware. Los eventos PnP se pueden usar para realizar un seguimiento de los cambios en el hardware del sistema. En el evento se incluye una lista de identificadores de proveedor de hardware.

Windows Server 2016 se integra fácilmente con sistemas de administración de eventos de incidentes de seguridad (SIEM), como Microsoft Operations Management Suite (OMS), que pueden incorporar la información en informes de inteligencia sobre posibles infracciones. La profundidad de la información proporcionada por la auditoría mejorada permite a los equipos de seguridad identificar y responder a posibles brechas de forma más rápida y eficaz.

### <a name="secure-virtualization"></a>Virtualización segura
Hoy en día, las empresas virtualizan todo lo posible, desde SQL Server a SharePoint para Dominio de Active Directory controladores. Las máquinas virtuales (VM) simplemente facilitan la implementación, la administración, el servicio y la automatización de la infraestructura. Pero cuando se trata de la seguridad, los tejidos de virtualización en peligro se han convertido en un nuevo vector de ataque que es difícil de defender, hasta ahora. Desde la perspectiva de RGPD, debe pensar en proteger las máquinas virtuales como lo haría con la protección de servidores físicos, incluido el uso de la tecnología TPM de máquinas virtuales.

Windows Server 2016 fundamentalmente cambia el modo en que las empresas pueden proteger la virtualización mediante la inclusión de varias tecnologías que le permiten crear máquinas virtuales que se ejecutarán solo en su propio tejido; ayuda para protegerse de los dispositivos de almacenamiento, red y host en los que se ejecutan.

#### <a name="shielded-virtual-machines"></a>Máquinas virtuales blindadas
Lo mismo que hace que las máquinas virtuales sean tan fáciles de migrar, realizar copias de seguridad y replicar, también facilitan la modificación y la copia. Una máquina virtual es simplemente un archivo, por lo que no está protegida en la red, en almacenamiento, en copias de seguridad o en otro lugar. Otro problema es que los administradores del tejido, ya sean administradores de almacenamiento o administradores de red, tienen acceso a todas las máquinas virtuales.

Un administrador en peligro en el tejido puede producir fácilmente datos en peligro en las máquinas virtuales. Todo el atacante debe usar las credenciales en peligro para copiar los archivos de la máquina virtual que desee en una unidad USB y desplazarlo fuera de la organización, donde se puede tener acceso a esos archivos de la máquina virtual desde cualquier otro sistema. Si alguna de esas máquinas virtuales robadas fuera un controlador de dominio de Active Directory, por ejemplo, el atacante podría ver fácilmente el contenido y usar las técnicas de fuerza bruta de fácil disponibilidad para descifrar las contraseñas en la base de datos de Active Directory y, en última instancia, darles acceso a todo lo demás dentro de la infraestructura.

Windows Server 2016 presenta Virtual Machines blindadas (máquinas virtuales blindadas) para ayudar a protegerse frente a escenarios como el que se acaba de describir. Las máquinas virtuales blindadas incluyen un dispositivo TPM virtual, que permite a las organizaciones aplicar cifrado de BitLocker a las máquinas virtuales y asegurarse de que solo se ejecutan en hosts de confianza para ayudar a protegerse frente a administradores de hosts, redes y almacenamiento en peligro. Las máquinas virtuales blindadas se crean mediante máquinas virtuales de generación 2, que admiten el firmware de Unified Extensible Firmware Interface (UEFI) y tienen un TPM virtual.

#### <a name="host-guardian-service"></a>Servicio de protección de host
Junto con las máquinas virtuales blindadas, el servicio de protección de host es un componente esencial para crear un tejido de virtualización seguro. Su trabajo es atestiguar el estado de un host de Hyper-V antes de permitir que una máquina virtual blindada se arranque o migre a ese host. Contiene las claves para las máquinas virtuales blindadas y no las libera hasta que se garantiza el estado de seguridad. Hay dos maneras de requerir que los hosts de Hyper-V certifiquen al servicio de protección de host.

La primera y más segura es la atestación de confianza de hardware. Esta solución requiere que las máquinas virtuales blindadas se ejecuten en hosts que tengan TPM 2,0 chips y UEFI 2.3.1. Este hardware es necesario para proporcionar la información de integridad del kernel de arranque y sistema operativo medida que requiere el servicio de protección de host para asegurarse de que el host de Hyper-V no se ha manipulado.

Las organizaciones de TI tienen la alternativa de usar la atestación de confianza de administrador, que puede ser deseable si el hardware de TPM 2,0 no está en uso en su organización. Este modelo de atestación es fácil de implementar porque los hosts simplemente se colocan en un grupo de seguridad y el servicio de protección de host está configurado para permitir que las máquinas virtuales blindadas se ejecuten en hosts que son miembros del grupo de seguridad. Con este método, no hay ninguna medida compleja para asegurarse de que la máquina host no se ha manipulado. Sin embargo, se elimina la posibilidad de que las máquinas virtuales sin cifrar destaquen la puerta de las unidades USB o que la máquina virtual se ejecute en un host no autorizado. Esto se debe a que los archivos de la máquina virtual no se ejecutan en ningún equipo que no sea el del grupo designado. Si aún no dispone de hardware de TPM 2,0, puede empezar con la atestación de confianza de administrador y cambiar a la atestación de confianza de hardware cuando se actualice el hardware.

#### <a name="virtual-machine-trusted-platform-module"></a>Módulo de plataforma segura de máquina virtual
Windows Server 2016 admite TPM para máquinas virtuales, lo que le permite admitir tecnologías de seguridad avanzadas como el cifrado de unidad de BitLocker® en máquinas virtuales. Puede habilitar la compatibilidad con TPM en cualquier máquina virtual de Hyper-V de generación 2 mediante el administrador de Hyper-V o el cmdlet enable-VMTPM de Windows PowerShell.

Puede proteger el TPM virtual (vTPM) mediante las claves de cifrado locales almacenadas en el host o almacenadas en el servicio de protección de host. Por lo tanto, aunque el servicio de protección de host requiere más infraestructura, también proporciona más protección.

#### <a name="distributed-network-firewall-using-software-defined-networking"></a>Firewall de red distribuido mediante redes definidas por software
Una manera de mejorar la protección en entornos virtualizados es segmentar la red de forma que permita que las máquinas virtuales se comuniquen solo con los sistemas específicos que se necesitan para funcionar. Por ejemplo, si la aplicación no necesita conectarse a Internet, puede crear particiones y eliminar esos sistemas como destinos de atacantes externos. Las redes definidas por software (SDN) en Windows Server 2016 incluyen un firewall de red distribuida que le permite crear dinámicamente las directivas de seguridad que pueden proteger sus aplicaciones frente a ataques procedentes de dentro o fuera de una red. Este Firewall de red distribuido agrega capas a la seguridad permitiéndole aislar las aplicaciones en la red. Las directivas se pueden aplicar en cualquier parte de la infraestructura de red virtual, aislar el tráfico de máquina virtual a máquina virtual, el tráfico de máquina virtual a host o el tráfico de máquina virtual a Internet cuando sea necesario, ya sea para sistemas individuales que puedan estar en peligro o mediante programación en varias subredes. Las capacidades de red definidas por software de Windows Server 2016 también permiten enrutar o reflejar el tráfico entrante en dispositivos virtuales que no son de Microsoft. Por ejemplo, podría optar por enviar todo el tráfico de correo electrónico a través de una aplicación virtual de Barracuda para la protección adicional del filtrado de correo no deseado. Esto le permite disponer fácilmente en capas de seguridad adicionales tanto en el entorno local como en la nube.

### <a name="other-gdpr-considerations-for-servers"></a>Otras consideraciones de RGPD para servidores
RGPD incluye requisitos explícitos para la notificación de brechas en los que una infracción de datos personales significa "_una infracción de seguridad que conduce a la destrucción accidental o ilícita, la pérdida, la alteración, la divulgación no autorizada de datos personales, o el acceso a ellos. se transmiten, almacenan o procesan de otra manera._ "  Obviamente, no puede empezar a avanzar para cumplir los estrictos requisitos de notificación de RGPD en 72 horas si no puede detectar la brecha en primer lugar.

Como se indicó en las notas del producto de [Security Center de Windows, infracción de post: Tratamiento de amenazas avanzadas](http://wincom.blob.core.windows.net/documents/Post_Breach_Dealing_with_Advanced_Threats_Whitepaper.pdf)

> "_A diferencia de la prefracción previa, la infracción posterior supone que ya se ha producido una infracción: actuar como una caja negra y un investigador de la escena de delitos (CSI). Después del incumplimiento, los equipos de seguridad proporcionan la información y el conjunto de herramientas necesarios para identificar, investigar y responder a los ataques que de otro modo permanecerán sin detectar y por debajo del radar._ "

En esta sección, veremos cómo Windows Server puede ayudarle a cumplir sus obligaciones de notificación de incumplimiento de RGPD. Esto comienza con la comprensión de los datos de amenazas subyacentes disponibles para Microsoft que se recopilan y analizan para su beneficio y cómo, a través de protección contra amenazas avanzada (ATP) de Windows Defender, los datos pueden ser críticos para usted.

#### <a name="insightful-security-diagnostic-data"></a>Datos de diagnóstico de seguridad detallados
En casi dos décadas, Microsoft ha estado convirtiendo las amenazas en una inteligencia útil que puede ayudar a fortalecer su plataforma y a proteger a los clientes. En la actualidad, con las grandes ventajas informáticas que ofrece la nube, estamos encontrando nuevas maneras de usar nuestros motores de análisis enriquecidos controlados por medio de inteligencia contra amenazas para proteger a nuestros clientes.

Al aplicar una combinación de procesos manuales y automatizados, aprendizaje automático y expertos humanos, podemos crear un gráfico de seguridad inteligente que aprende de sí mismo y evoluciona en tiempo real, reduciendo nuestro tiempo colectivo para detectar y responder a nuevos incidentes a través de nuestros productos.

![Gráfico de seguridad de Microsoft Intelligence](../media/GDPR-Windows-Server-Overview/gdpr-intelligent-security-graph.png)

El ámbito de los intervalos de inteligencia de amenazas de Microsoft, literalmente, miles de millones de puntos de datos: 35 mil millones mensajes examinados mensualmente, 1 mil millones clientes a través de segmentos de consumidores y empresas que obtienen acceso a más de 200 servicios en la nube y 14 mil millones autenticaciones realizadas a diario. Microsoft reúne todos estos datos en su nombre para crear los Intelligent Security Graph que pueden ayudarle a proteger la puerta de la puerta de una forma dinámica de mantener la seguridad, ser productivo y cumplir los requisitos de RGPD.

#### <a name="detecting-attacks-and-forensic-investigation"></a>Detección de ataques e investigación forense
Incluso las mejores defensas de extremo se pueden infringir a la larga, a medida que los ciberataques se vuelven más sofisticados y dirigidos. Se pueden usar dos funciones para ayudar a detectar posibles infracciones: protección contra amenazas avanzada (ATP) de Windows Defender y Microsoft Advanced Threat Analytics (ATA).

La Protección contra amenazas avanzada de Windows Defender (ATP) te ayuda a detectar, investigar y responder a ataques avanzados e infracciones de datos en las redes. Los tipos de infracción de datos que RGPD espera que proteja mediante medidas de seguridad técnicas para garantizar la confidencialidad, integridad y disponibilidad continua de los datos personales y los sistemas de procesamiento.

Entre las principales ventajas de ATP de Windows Defender se encuentran las siguientes:

- **Detección de la no detectable.** Sensores integrados en el kernel del sistema operativo, expertos de seguridad de Windows y dispositivos ópticos únicos de más de 1 mil millones equipos y señales en todos los servicios de Microsoft.

- **Integrado, sin Bolt.** Sin agente, con alto rendimiento y un impacto mínimo, con tecnología de nube; Administración sencilla sin implementación. 

- **Un solo panel de cristal para la seguridad de Windows.** Explore 6 meses de riqueza, escala de tiempo de equipo, unificando eventos de seguridad de ATP de Windows Defender, antivirus de Windows Defender y protección de dispositivos de Windows Defender.

- **Eficacia de Microsoft Graph.** Aprovecha el gráfico de seguridad de Microsoft Intelligence para integrar la detección y la exploración con la suscripción de ATP de Office 365, para realizar un seguimiento de los ataques y responder a ellos.

Lee más información en [What’s new in the Windows Defender ATP Creators Update preview](https://blogs.microsoft.com/microsoftsecure/2017/03/13/whats-new-in-the-windows-defender-atp-creators-update-preview/).

ATA es un producto local que ayuda a detectar el riesgo de identidad en una organización. ATA puede capturar y analizar el tráfico de red para la autenticación, autorización y protocolos de recopilación de información (como Kerberos, DNS, RPC, NTLM y otros protocolos). ATA usa estos datos para crear un perfil de comportamiento sobre los usuarios y otras entidades de una red para que pueda detectar anomalías y patrones de ataque conocidos. En la tabla siguiente se enumeran los tipos de ataque detectados por ATA.


|Tipo de ataque |Descripción |
|---------|---------|
|Ataques malintencionados |Estos ataques se detectan mediante la búsqueda de ataques a partir de una lista conocida de tipos de ataque, incluidos:<ul><li>Pass-The-ticket (PtT)</li><li>Pass-The-hash (PtH)</li><li>Overpass-The-hash</li><li>PAC falsificada (MS14-068)</li><li>Golden Ticket</li><li>Replicaciones malintencionadas</li><li>Reconocimiento</li><li>Fuerza bruta</li><li>Ejecución remota</li></ul>Para obtener una lista completa de los ataques malintencionados que se pueden detectar y su descripción, vea [¿qué actividades sospechosas puede detectar ATA?](https://docs.microsoft.com/advanced-threat-analytics/understand-explore/ata-threats).|
|Comportamiento anómalo |Estos ataques se detectan mediante el análisis de comportamiento y usan el aprendizaje automático para identificar actividades cuestionables, entre las que se incluyen:<ul><li>Inicios de sesión anómalos</li><li>Amenazas desconocidas</li><li>Uso compartido de contraseñas</li><li>Movimiento lateral</li></ul>|
|Problemas y riesgos de seguridad |Estos ataques se detectan examinando la configuración actual de la red y del sistema, incluidos:<ul><li>Confianza rota</li><li>Protocolos débiles</li><li>Vulnerabilidades de protocolo conocidas</li></ul>|

Puede usar ATA para ayudar a detectar atacantes que intenten poner en peligro las identidades con privilegios. Para obtener más información sobre la implementación de ATA, consulte los temas planeación, diseño e implementación de la [documentación de Advanced Threat Analytics](https://docs.microsoft.com/advanced-threat-analytics/).

## <a name="related-content-for-associated-windows-server-2016-solutions"></a>Contenido relacionado para las soluciones de Windows Server 2016 asociadas

- **Antivirus de Windows Defender:** https://www.youtube.com/watch?v=P1aNEy09NaI y https://docs.microsoft.com/windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10

- **Protección contra amenazas avanzada de Windows Defender:** https://www.youtube.com/watch?v=qxeGa3pxIwg y https://docs.microsoft.com/windows/threat-protection/windows-defender-atp/configure-server-endpoints-windows-defender-advanced-threat-protection

- **Protección de dispositivos de Windows Defender:** https://www.youtube.com/watch?v=F-pTkesjkhI y https://docs.microsoft.com/windows/device-security/device-guard/device-guard-deployment-guide

- **Protección de credenciales de Windows Defender:** https://www.youtube.com/watch?v=F-pTkesjkhI y https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard

- **Protección de flujo de control:** https://msdn.microsoft.com/library/windows/desktop/mt637065(v=vs.85).aspx

- **Seguridad y garantía:** https://docs.microsoft.com/windows-server/security/security-and-assurance

## <a name="disclaimer"></a>Declinación de responsabilidades
Este artículo es un comentario sobre la GDPR, como la interpreta Microsoft, desde la fecha de publicación. Hemos dedicado mucho tiempo a la GDPR y creemos que hemos tenido muy en cuenta su propósito y significado. Pero la aplicación de GDPR se refiere a hechos muy concretos y no todos los aspectos y las interpretaciones de la GDPR están bien establecidos.

Como resultado, este artículo se proporciona solo con fines informativos y no se debería usar como asesoramiento legal o para determinar la manera en que la GDPR puede aplicarse para ti y tu organización. Te animamos a trabajar con un profesional con cualificación legal para hablar de la GDPR, de cómo se aplica de manera específica a tu organización y de la mejor manera para garantizar el cumplimiento.

MICROSOFT NO OTORGA NINGUNA GARANTÍA, YA SEA EXPRESA, IMPLÍCITA O REGLAMENTARIA, EN CUANTO A LA INFORMACIÓN DE ESTE ARTÍCULO. Este artículo se proporciona “tal cual”. La información y las opiniones expresadas en este artículo, incluidas las direcciones URL y otras referencias a sitios web de Internet, pueden sufrir cambios sin previo aviso.

Este artículo no proporciona derechos legales sobre ninguna propiedad intelectual en ningún producto de Microsoft.  Puedes copiar y usar este artículo solo para tu referencia interna.  

Publicado en septiembre de 2017<br>
Versión 1.0<br>
© 2017 Microsoft. Todos los derechos reservados.


