---
ms.assetid: 7a7ab95c-9cb3-4a7b-985a-3fc08334cf4f
title: Implementación de modelos administrativos de menor privilegio
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: d6a740f0fdc76698114cace8ded8533cebffc5cb
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86964037"
---
# <a name="implementing-least-privilege-administrative-models"></a>Implementación de modelos administrativos de menor privilegio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El siguiente fragmento procede de [la guía de planeamiento de la seguridad de las cuentas de administrador](/previous-versions/tn-archive/cc162797(v=technet.10)), publicada por primera vez el 1 de abril de 1999:

> "La mayoría de los cursos de aprendizaje y la documentación relacionados con la seguridad describen la implementación de un principio de privilegios mínimos, pero las organizaciones raramente lo siguen. El principio es sencillo y el impacto de aplicarlo correctamente aumenta la seguridad y reduce el riesgo. El principio indica que todos los usuarios deben iniciar sesión con una cuenta de usuario que tenga los permisos mínimos necesarios para completar la tarea actual y nada más. Al hacerlo, se proporciona protección contra código malintencionado, entre otros ataques. Este principio se aplica a los equipos y a los usuarios de esos equipos.   
> "Una de las razones por las que este principio funciona tan bien es que obliga a realizar alguna investigación interna. Por ejemplo, debe determinar los privilegios de acceso que realmente necesita un equipo o usuario y, a continuación, implementarlos. Para muchas organizaciones, es posible que esta tarea parezca inicialmente una gran cantidad de trabajo. sin embargo, es un paso esencial para proteger correctamente el entorno de red.
> "Debe conceder a todos los usuarios de administrador de dominio sus privilegios de dominio en el concepto de privilegios mínimos. Por ejemplo, si un administrador inicia sesión con una cuenta con privilegios y ejecuta accidentalmente un programa antivirus, el virus tiene acceso administrativo al equipo local y a todo el dominio. Si el administrador hubiera iniciado sesión con una cuenta sin privilegios (no administrativa), el ámbito de daños del virus solo sería el equipo local, ya que se ejecuta como un usuario del equipo local.
> "En otro ejemplo, las cuentas a las que concede derechos de administrador en el nivel de dominio no deben tener derechos elevados en otro bosque, incluso si hay una relación de confianza entre los bosques. Esta táctica ayuda a evitar daños generalizados si un atacante logra poner en peligro un bosque administrado. Las organizaciones deben auditar regularmente su red para protegerse de la escalación de privilegios no autorizada ".  

El siguiente fragmento procede del [Kit de recursos de seguridad de Microsoft Windows](https://www.microsoftpressstore.com/store/microsoft-windows-security-resource-kit-9780735621749), que se publicó por primera vez en 2005:  

> "Piense siempre en la seguridad en cuanto a la concesión de la mínima cantidad de privilegios necesarios para realizar la tarea. Si una aplicación que tiene demasiados privilegios debe estar en peligro, el atacante podría expandir el ataque más allá de lo que haría si la aplicación hubiera superado la menor cantidad de privilegios posible. Por ejemplo, examine las consecuencias de que un administrador de red abra involuntariamente un archivo adjunto de correo electrónico que inicie un virus. Si el administrador ha iniciado sesión con la cuenta de administrador de dominio, el virus tendrá privilegios de administrador en todos los equipos del dominio y, por tanto, tendrá acceso sin restricciones a casi todos los datos de la red. Si el administrador ha iniciado sesión con una cuenta de administrador local, el virus tendrá privilegios de administrador en el equipo local y, por tanto, podrá tener acceso a los datos del equipo e instalar software malintencionado como el software de registro de trazos de clave en el equipo. Si el administrador ha iniciado sesión con una cuenta de usuario normal, el virus solo tendrá acceso a los datos del administrador y no podrá instalar software malintencionado. Mediante el uso de los mínimos privilegios necesarios para leer el correo electrónico, en este ejemplo, se reduce en gran medida el posible ámbito del riesgo. "  

## <a name="the-privilege-problem"></a>El problema del privilegio

Los principios descritos en los extractos anteriores no han cambiado, pero en la evaluación de las instalaciones Active Directory, encontramos invariablemente un número excesivo de cuentas a las que se les han concedido derechos y permisos mucho más allá de los necesarios para realizar el trabajo cotidiano. El tamaño del entorno afecta a los números sin procesar de las cuentas con privilegios elevados, pero no a los directorios proportionmidsized pueden tener docenas de cuentas en los grupos con más privilegios, mientras que las instalaciones grandes pueden tener cientos o incluso miles. Con pocas excepciones, independientemente de la sofisticación de los conocimientos y el arsenal de un atacante, los atacantes suelen seguir la ruta de acceso de menor resistencia. Aumentan la complejidad de sus herramientas y su enfoque solo si y cuándo se producen errores en mecanismos más sencillos o están frustrados por los defensores.  

Desafortunadamente, la ruta de acceso de menos resistencia en muchos entornos ha demostrado ser el uso excesivo de cuentas con privilegios amplios y profundos. Los amplios privilegios son derechos y permisos que permiten que una cuenta realice actividades específicas en una gran sección transversal del entorno; por ejemplo, se puede conceder permisos al personal del Departamento de soporte técnico que les permita restablecer las contraseñas en muchas cuentas de usuario.  

Los privilegios profundos son privilegios eficaces que se aplican a un segmento estrecho de la población, como ofrecer derechos de administrador de ingenieros en un servidor para que puedan realizar reparaciones. Ninguno de los privilegios amplios ni los privilegios profundos son necesariamente peligrosos, pero cuando muchas cuentas del dominio tienen concedidos permisos amplios y profundos de forma permanente, si solo una de las cuentas se ve comprometida, se puede usar rápidamente para volver a configurar el entorno a los propósitos del atacante o incluso destruir segmentos grandes de la infraestructura.  

Los ataques Pass-The-hash, que son un tipo de ataque de robo de credenciales, están omnipresentes porque las herramientas para realizarlos están disponibles de forma gratuita y fáciles de usar, y porque muchos entornos son vulnerables a los ataques. Sin embargo, los ataques Pass-The-hash no son el problema real. El quid del problema es doble:  

1. Normalmente, es fácil que un atacante obtenga privilegios profundos en un único equipo y, a continuación, propague ese privilegio en general a otros equipos.  
2. Normalmente hay demasiadas cuentas permanentes con altos niveles de privilegios en el panorama informático.

Aunque se eliminen los ataques Pass-The-hash, los atacantes simplemente usarían tácticas diferentes, no una estrategia diferente. En lugar de plantar el malware que contiene las herramientas de robo de credenciales, pueden plantar el malware que registra pulsaciones de teclas o aprovechar cualquier otro enfoque para capturar credenciales que son eficaces en todo el entorno. Independientemente de las tácticas, los destinos siguen siendo los mismos: cuentas con privilegios amplios y profundos.  

La concesión de permisos excesivos no se encuentra solo en Active Directory en entornos en peligro. Cuando una organización ha desarrollado el hábito de conceder más privilegios de los necesarios, normalmente se encuentra en toda la infraestructura, tal como se describe en las secciones siguientes.  

## <a name="in-active-directory"></a>En Active Directory

En Active Directory, es habitual encontrar que los grupos EA, DA y BA contengan un número excesivo de cuentas. Normalmente, el grupo EA de una organización contiene el menor número de miembros, los grupos DA normalmente contienen un multiplicador del número de usuarios del grupo EA y los grupos administradores normalmente contienen más miembros que los de los demás grupos combinados. Esto suele deberse a la creencia de que los administradores tienen "menos privilegios" que DAs o EAs. Aunque los derechos y permisos concedidos a cada uno de estos grupos difieren, deben considerarse grupos igualmente eficaces, ya que un miembro de uno de ellos puede convertirse en miembro de los otros dos.  

## <a name="on-member-servers"></a>En servidores miembro

Cuando recuperamos la pertenencia de los grupos de administradores locales en servidores miembro en muchos entornos, encontramos la pertenencia a un puñado de cuentas locales y de dominio, a docenas de grupos anidados que, cuando se expanden, revelan cientos, incluso miles, de cuentas con privilegios de administrador local en los servidores. En muchos casos, los grupos de dominio con pertenencias grandes se anidan en los grupos de administradores locales de los servidores miembro, sin tener en cuenta el hecho de que cualquier usuario que pueda modificar la pertenencia a grupos en el dominio puede obtener el control administrativo de todos los sistemas en los que el grupo se ha anidado en un grupo de administradores local.  

## <a name="on-workstations"></a>En las estaciones de trabajo

Aunque las estaciones de trabajo suelen tener muchos menos miembros en sus grupos de administradores locales que los servidores miembro, en muchos entornos, se concede a los usuarios la pertenencia al grupo de administradores locales en sus equipos personales. Cuando esto ocurre, incluso si UAC está habilitado, los usuarios presentan un riesgo elevado para la integridad de sus estaciones de trabajo.  

> [!IMPORTANT]  
> Considere detenidamente si los usuarios necesitan derechos administrativos en sus estaciones de trabajo y, si lo hacen, un enfoque mejor podría ser crear una cuenta local independiente en el equipo que sea miembro del grupo administradores. Cuando los usuarios necesitan elevación, pueden presentar las credenciales de esa cuenta local para la elevación, pero como la cuenta es local, no se puede usar para poner en peligro otros equipos o tener acceso a los recursos del dominio. No obstante, al igual que con las cuentas locales, las credenciales de la cuenta local con privilegios deben ser únicas. Si crea una cuenta local con las mismas credenciales en varias estaciones de trabajo, expondrá los equipos a ataques Pass-The-hash.  

## <a name="in-applications"></a>En aplicaciones

En los ataques en los que el destino es la propiedad intelectual de una organización, las cuentas a las que se les han concedido privilegios eficaces dentro de las aplicaciones se pueden destinar para permitir la exfiltración de datos. Aunque es posible que las cuentas que tienen acceso a datos confidenciales no hayan concedido privilegios elevados en el dominio o en el sistema operativo, las cuentas que pueden manipular la configuración de una aplicación o el acceso a la información que la aplicación proporciona están en riesgo.  

## <a name="in-data-repositories"></a>En repositorios de datos

Como ocurre con otros destinos, los atacantes que buscan el acceso a la propiedad intelectual en forma de documentos y otros archivos pueden tener como destino las cuentas que controlan el acceso a los almacenes de archivos, las cuentas que tienen acceso directo a los archivos, o incluso los grupos o los roles que tienen acceso a los archivos. Por ejemplo, si se usa un servidor de archivos para almacenar documentos de contrato y se concede el acceso a los documentos mediante el uso de un grupo de Active Directory, un atacante que pueda modificar la pertenencia al grupo puede agregar cuentas en peligro al grupo y obtener acceso a los documentos del contrato. En los casos en que las aplicaciones como SharePoint proporcionan el acceso a los documentos, los atacantes pueden dirigirse a las aplicaciones como se describió anteriormente.  

## <a name="reducing-privilege"></a>Reducción de privilegios

El entorno más grande y más complejo es más difícil de administrar y proteger. En organizaciones pequeñas, revisar y reducir los privilegios puede ser una propuesta relativamente simple, pero cada servidor, estación de trabajo, cuenta de usuario y aplicación adicionales en uso en una organización agrega otro objeto que se debe proteger. Dado que puede ser difícil o incluso imposible proteger correctamente todos los aspectos de la infraestructura de TI de una organización, debe centrar los esfuerzos primero en las cuentas cuyo privilegio crea el mayor riesgo, que normalmente son las cuentas y los grupos con privilegios integrados en Active Directory y las cuentas locales con privilegios en estaciones de trabajo y servidores miembro.  

### <a name="securing-local-administrator-accounts-on-workstations-and-member-servers"></a>Protección de cuentas de administrador local en estaciones de trabajo y servidores miembro

Aunque este documento se centra en la protección de Active Directory, como se ha explicado anteriormente, la mayoría de los ataques contra el directorio comienzan como ataques contra hosts individuales. No se pueden proporcionar instrucciones completas para proteger los grupos locales en los sistemas miembro, pero se pueden usar las siguientes recomendaciones para proteger las cuentas de administrador local en estaciones de trabajo y servidores miembro.  

#### <a name="securing-local-administrator-accounts"></a>Protección de cuentas de administrador local

En todas las versiones de Windows que se encuentran actualmente en soporte estándar, la cuenta de administrador local está deshabilitada de forma predeterminada, lo que hace que la cuenta sea inutilizable para Pass-The-hash y otros ataques de robo de credenciales. Sin embargo, en los dominios que contienen sistemas operativos heredados o en los que se han habilitado cuentas de administrador local, estas cuentas se pueden usar como se ha descrito anteriormente para propagar el compromiso entre los servidores miembro y las estaciones de trabajo. Por esta razón, se recomiendan los siguientes controles para todas las cuentas de administrador local en sistemas Unidos a un dominio.  

Las instrucciones detalladas para implementar estos controles se proporcionan en el [Apéndice H: protección de grupos y cuentas de administrador local](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md). Sin embargo, antes de implementar esta configuración, asegúrese de que las cuentas de administrador local no se usan actualmente en el entorno para ejecutar servicios en equipos o para realizar otras actividades para las que no se deben usar estas cuentas. Pruebe estas opciones minuciosamente antes de implementarlas en un entorno de producción.  

#### <a name="controls-for-local-administrator-accounts"></a>Controles para las cuentas de administrador local

Las cuentas de administrador integradas nunca deben usarse como cuentas de servicio en servidores miembro, ni deben usarse para iniciar sesión en equipos locales (excepto en el modo seguro, que se permite incluso si la cuenta está deshabilitada). El objetivo de implementar la configuración que se describe aquí es impedir que se pueda usar la cuenta de administrador local de cada equipo a menos que se inviertan primero los controles de protección. Mediante la implementación de estos controles y la supervisión de los cambios de las cuentas de administrador, puede reducir significativamente la probabilidad de éxito de un ataque que tenga como destino las cuentas de administrador local.  

##### <a name="configuring-gpos-to-restrict-administrator-accounts-on-domain-joined-systems"></a>Configuración de GPO para restringir las cuentas de administrador en sistemas Unidos a un dominio

En uno o más GPO que cree y vincule a las unidades organizativas de estaciones de trabajo y servidores miembro de cada dominio, agregue la cuenta de administrador a los siguientes derechos de usuario en el **equipo \ configuración de Seguridad\directivas \ configuración de directivas**de usuario \ asignación de derechos:  

- Denegar el acceso desde la red a este equipo
- Denegar el inicio de sesión como trabajo por lotes
- Denegar el inicio de sesión como servicio
- Denegar inicio de sesión a través de Servicios de Escritorio remoto

Al agregar cuentas de administrador a estos derechos de usuario, especifique si va a agregar la cuenta de administrador local o la cuenta de administrador del dominio por la forma de etiquetar la cuenta. Por ejemplo, para agregar la cuenta de administrador del dominio NWTRADERS a estos derechos de denegación, debe escribir la cuenta como **Nwtraders\Administrador**o buscar la cuenta de administrador del dominio nwtraders. Para asegurarse de que restringe la cuenta de administrador local, escriba **Administrador** en esta configuración de derechos de usuario en el editor de objetos de directiva de grupo.  

> [!NOTE]  
> Incluso si se cambia el nombre de las cuentas de administrador local, se seguirán aplicando las directivas.  

Esta configuración garantizará que la cuenta de administrador de un equipo no se puede usar para conectarse a los otros equipos, incluso si está habilitada de forma accidental o malintencionada. Los inicios de sesión locales con la cuenta de administrador local no se pueden deshabilitar por completo, ni tampoco se debe intentar hacerlo, porque la cuenta de administrador local de un equipo está diseñada para utilizarse en escenarios de recuperación ante desastres.  

Si un servidor miembro o una estación de trabajo se separan del dominio sin ninguna otra cuenta local con privilegios administrativos, el equipo se puede arrancar en modo seguro, se puede habilitar la cuenta de administrador y la cuenta se puede usar para realizar reparaciones en el equipo. Una vez completadas las reparaciones, se debe deshabilitar la cuenta de administrador.  

### <a name="securing-local-privileged-accounts-and-groups-in-active-directory"></a>Protección de grupos y cuentas con privilegios locales en Active Directory

*Número de ley seis: un equipo es tan seguro como el administrador es de confianza.* - [Diez leyes inmutables de seguridad (versión 2,0)](https://www.microsoft.com/en-us/msrc?rtc=1)  

La información que se proporciona aquí está pensada para proporcionar directrices generales para proteger las cuentas y los grupos integrados de privilegios más elevados en Active Directory. También se proporcionan instrucciones paso a paso detalladas en el [Apéndice D: protección de cuentas de administrador integradas en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md), [Apéndice E: protección de grupos de administradores de empresa en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md), [Apéndice F: protección de grupos de administradores de dominio en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)y en el [Apéndice G: protección de grupos de administradores en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md).  

Antes de implementar cualquiera de estas opciones, también debe probar exhaustivamente todas las configuraciones para determinar si son adecuadas para su entorno. No todas las organizaciones podrán implementar esta configuración.  

#### <a name="securing-built-in-administrator-accounts-in-active-directory"></a>Protección de cuentas de administrador integradas en Active Directory

En cada dominio de Active Directory, se crea una cuenta de administrador como parte de la creación del dominio. De forma predeterminada, esta cuenta es miembro de los grupos Admins. del dominio y administrador del dominio, y si el dominio es el dominio raíz del bosque, la cuenta también es miembro del grupo administradores de empresas. El uso de la cuenta de administrador local de un dominio solo se debe reservar para actividades de compilación iniciales y, posiblemente, escenarios de recuperación ante desastres. Para asegurarse de que se puede usar una cuenta de administrador integrada para realizar reparaciones en caso de que no se puedan usar otras cuentas, no debe cambiar la pertenencia predeterminada de la cuenta de administrador en ningún dominio del bosque. En su lugar, debe seguir las directrices para ayudar a proteger la cuenta de administrador en cada dominio del bosque. En el [Apéndice D: proteger las cuentas de administrador integradas en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)se proporcionan instrucciones detalladas para implementar estos controles.  

#### <a name="controls-for-built-in-administrator-accounts"></a>Controles para las cuentas predefinidas de administrador

El objetivo de implementar la configuración que se describe aquí es evitar que cada cuenta de administrador de dominio (no un grupo) se pueda usar a menos que se inviertan varios controles. Mediante la implementación de estos controles y la supervisión de los cambios en las cuentas de administrador, puede reducir significativamente la probabilidad de que se produzca un ataque correctamente aprovechando la cuenta de administrador de un dominio. En el caso de la cuenta de administrador de cada dominio del bosque, debe configurar las siguientes opciones.  

##### <a name="enable-the-account-is-sensitive-and-cannot-be-delegated-flag-on-the-account"></a>Habilitar la marca "la cuenta es importante y no se puede delegar" en la cuenta

De forma predeterminada, todas las cuentas de Active Directory se pueden delegar. La delegación permite a un equipo o servicio presentar las credenciales de una cuenta que se ha autenticado en el equipo o servicio en otros equipos para obtener los servicios en nombre de la cuenta. Cuando se habilita la **cuenta es sensible y no se puede delegar** el atributo en una cuenta basada en dominio, las credenciales de la cuenta no se pueden presentar a otros equipos o servicios de la red, lo que limita los ataques que aprovechan la delegación para usar las credenciales de la cuenta en otros sistemas.  

##### <a name="enable-the-smart-card-is-required-for-interactive-logon-flag-on-the-account"></a>Habilitar la marca "se requiere tarjeta inteligente para el inicio de sesión interactivo" en la cuenta

Cuando se habilita la **tarjeta inteligente es necesaria para** el atributo de inicio de sesión interactivo en una cuenta, Windows restablece la contraseña de la cuenta a un valor aleatorio de 120 caracteres. Al establecer esta marca en las cuentas predefinidas de administrador, se asegura de que la contraseña de la cuenta no sea tan larga y compleja, pero que no la conozca ningún usuario. Técnicamente, no es necesario crear tarjetas inteligentes para las cuentas antes de habilitar este atributo, pero, si es posible, se deben crear tarjetas inteligentes para cada cuenta de administrador antes de configurar las restricciones de cuenta y las tarjetas inteligentes deben almacenarse en ubicaciones seguras.  

Aunque el establecimiento de la **tarjeta inteligente es necesario para que la marca de inicio de sesión interactivo** restablezca la contraseña de la cuenta, no impide que un usuario con derechos restablezca la contraseña de la cuenta para establecer la cuenta en un valor conocido y usar el nombre de la cuenta y la nueva contraseña para acceder a los recursos de la red. Por ello, debe implementar los siguientes controles adicionales en la cuenta.  

##### <a name="configuring-gpos-to-restrict-domains-administrator-accounts-on-domain-joined-systems"></a>Configuración de GPO para restringir las cuentas de administrador de dominios en sistemas Unidos a un dominio

Aunque deshabilitar la cuenta de administrador en un dominio hace que la cuenta sea inutilizable, debe implementar restricciones adicionales en la cuenta en caso de que la cuenta esté habilitada de forma inadvertida o malintencionada. Aunque la cuenta de administrador puede revertir estos controles en última instancia, el objetivo es crear controles que ralenticen el progreso de un atacante y limitar el daño que puede infligir la cuenta.  

En uno o más GPO creados y vinculados a las unidades organizativas de estaciones de trabajo y servidores miembro de cada dominio, agregue la cuenta de administrador de cada dominio a los siguientes derechos de usuario en el **equipo \ configuración**de seguridad\Directivas locales\Opciones de trabajo \ asignaciones de derechos:  

- Denegar el acceso desde la red a este equipo  
- Denegar el inicio de sesión como trabajo por lotes  
- Denegar el inicio de sesión como servicio  
- Denegar inicio de sesión a través de Servicios de Escritorio remoto  

> [!NOTE]  
> Al agregar cuentas de administrador local a esta configuración, debe especificar si va a configurar cuentas de administrador local o cuentas de administrador de dominio. Por ejemplo, para agregar la cuenta de administrador local del dominio NWTRADERS a estos derechos de denegación, debe escribir la cuenta como **Nwtraders\Administrador**o buscar la cuenta de administrador local para el dominio nwtraders. Si escribe **Administrador** en esta configuración de derechos de usuario en el editor de objetos de directiva de grupo, se restringe la cuenta de administrador local en cada equipo al que se aplica el GPO.  
>
> Se recomienda restringir las cuentas de administrador local en los servidores miembro y las estaciones de trabajo de la misma manera que las cuentas de administrador basadas en dominio. Por lo tanto, normalmente debe agregar la cuenta de administrador para cada dominio del bosque y la cuenta de administrador de los equipos locales a esta configuración de derechos de usuario. 

##### <a name="configuring-gpos-to-restrict-administrator-accounts-on-domain-controllers"></a>Configuración de GPO para restringir cuentas de administrador en controladores de dominio

En cada dominio del bosque, se debe modificar la directiva predeterminada de controladores de dominio o una directiva vinculada a la unidad organizativa controladores de dominio para agregar la cuenta de administrador de cada dominio a los siguientes derechos de usuario en el equipo: \ configuración de seguridad\Directivas locales **\ asignaciones de derechos**:  

- Denegar el acceso desde la red a este equipo  
- Denegar el inicio de sesión como trabajo por lotes  
- Denegar el inicio de sesión como servicio  
- Denegar inicio de sesión a través de Servicios de Escritorio remoto  

> [!NOTE]  
> Esta configuración garantizará que la cuenta de administrador local no se puede usar para conectarse a un controlador de dominio, aunque la cuenta, si está habilitada, puede iniciar sesión localmente en los controladores de dominio. Dado que esta cuenta solo debe habilitarse y usarse en escenarios de recuperación ante desastres, se prevé que el acceso físico a un controlador de dominio como mínimo estará disponible, o que se puedan usar otras cuentas con permisos para tener acceso a los controladores de dominio de forma remota.  

##### <a name="configure-auditing-of-built-in-administrator-accounts"></a>Configurar la auditoría de cuentas de administrador integradas

Una vez que haya protegido la cuenta de administrador de cada dominio y lo haya deshabilitado, debe configurar la auditoría para supervisar los cambios de la cuenta. Si la cuenta está habilitada, se restablece su contraseña o se realizan otras modificaciones en la cuenta, se deben enviar alertas a los usuarios o equipos responsables de la administración de AD DS, además de los equipos de respuesta a los incidentes de la organización.  

### <a name="securing-administrators-domain-admins-and-enterprise-admins-groups"></a>Protección de administradores, administradores de dominio y grupos de administradores de organización

#### <a name="securing-enterprise-admin-groups"></a>Protección de grupos de administradores de empresa

El grupo administradores de empresas, que está hospedado en el dominio raíz del bosque, no debe contener ningún usuario en la base diaria, con la excepción posible de la cuenta de administrador local del dominio, siempre que esté protegido como se describió anteriormente y en el [Apéndice D: proteger cuentas de administrador integradas en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

Cuando se requiere acceso de EA, los usuarios cuyas cuentas requieren derechos y permisos de EA deben colocarse temporalmente en el grupo administradores de empresas. Aunque los usuarios usan las cuentas con privilegios elevados, sus actividades se deben auditar y, preferiblemente, realizarse con un usuario que realice los cambios y otro usuario observando los cambios para reducir la probabilidad de un mal uso o una configuración incorrecta. Una vez completadas las actividades, las cuentas deben quitarse del grupo EA. Esto se puede lograr a través de procedimientos manuales y procesos documentados, software de administración de identidades y accesos con privilegios de terceros (PIM/PAM) o una combinación de ambos. Las instrucciones para crear cuentas que se pueden usar para controlar la pertenencia de grupos con privilegios en Active Directory se proporcionan en [cuentas atractivas para el robo de credenciales](../../../ad-ds/plan/security-best-practices/Attractive-Accounts-for-Credential-Theft.md) e instrucciones detalladas en el [Apéndice I: crear cuentas de administración para cuentas y grupos protegidos en Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  

Los administradores de empresas son, de forma predeterminada, miembros del grupo de administradores integrado en cada dominio del bosque. La eliminación del grupo administradores de organización de los grupos administradores de cada dominio es una modificación inadecuada, ya que, en caso de que se produzca un escenario de recuperación ante desastres de bosque, es probable que se necesiten derechos de EA. Si se ha quitado el grupo administradores de empresas de los grupos administradores de un bosque, debe agregarse al grupo administradores de cada dominio y deben implementarse los siguientes controles adicionales:  

- Tal y como se ha descrito anteriormente, el grupo administradores de empresas no debe contener ningún usuario en la base diaria, con la excepción posible de la cuenta de administrador del dominio raíz del bosque, que debe protegerse tal como se describe en el [Apéndice D: proteger cuentas de administrador integradas en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
- En los GPO vinculados a las unidades organizativas que contienen servidores miembro y estaciones de trabajo en cada dominio, el grupo EA debe agregarse a los siguientes derechos de usuario:  
   - Denegar el acceso desde la red a este equipo  
   - Denegar el inicio de sesión como trabajo por lotes  
   - Denegar el inicio de sesión como servicio  
   - Denegar el inicio de sesión localmente  
   - Denegar el inicio de sesión a través de Servicios de Escritorio remoto.  

Esto impedirá que los miembros del grupo EA inicien sesión en los servidores y estaciones de trabajo miembro. Si se usan servidores de salto para administrar controladores de dominio y Active Directory, asegúrese de que los servidores de salto se encuentran en una unidad organizativa a la que no están vinculados los GPO restrictivos.  

- La auditoría debe estar configurada para enviar alertas si se realizan modificaciones en las propiedades o la pertenencia al grupo EA. Estas alertas se deben enviar, como mínimo, a los usuarios o equipos responsables de Active Directory la administración y la respuesta a los incidentes. También debe definir procesos y procedimientos para rellenar temporalmente el grupo EA, incluidos los procedimientos de notificación cuando se realiza un rellenado legítimo del grupo.  
  
#### <a name="securing-domain-admins-groups"></a>Protección de grupos Admins. del dominio

Como es el caso del grupo administradores de empresas, la pertenencia a grupos Admins. del dominio solo debe ser necesaria en escenarios de compilación o recuperación ante desastres. No debe haber cuentas de usuario cotidianas en el grupo de DA con la excepción de la cuenta de administrador local para el dominio, si se ha protegido como se describe en el [Apéndice D: protección de cuentas de administrador integradas en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
Cuando se requiere el acceso a DA, las cuentas que necesitan este nivel de acceso deben colocarse temporalmente en el grupo de DA para el dominio en cuestión. Aunque los usuarios usan las cuentas con privilegios elevados, las actividades se deben auditar y, preferiblemente, realizarse con un usuario que realice los cambios y otro usuario observando los cambios para reducir la probabilidad de un mal uso o una configuración incorrecta. Una vez completadas las actividades, las cuentas deben quitarse del grupo Admins. del dominio. Esto se puede lograr a través de procedimientos manuales y procesos documentados, a través de software de administración de identidades y accesos con privilegios de terceros (PIM/PAM) o una combinación de ambos. En el Apéndice I se proporcionan directrices para la creación de cuentas que se pueden usar para controlar la pertenencia de grupos con privilegios en Active Directory [: creación de cuentas de administración para cuentas y grupos protegidos en Active Directory](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
De forma predeterminada, los administradores de dominio son miembros de los grupos de administradores locales en todos los servidores miembro y estaciones de trabajo de sus respectivos dominios. Este anidamiento predeterminado no debe modificarse porque afecta a las opciones de compatibilidad y recuperación ante desastres. Si los grupos Admins. del dominio se han quitado de los grupos de administradores locales en los servidores miembro, deben agregarse al grupo administradores en cada servidor miembro y estación de trabajo del dominio a través de la configuración de grupo restringido en los GPO vinculados. Los siguientes controles generales, que se describen en profundidad en el [Apéndice F: proteger los grupos de Admins. del dominio en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md) también deben implementarse.  

Para el grupo Admins. del dominio en cada dominio del bosque:  

1. Quite todos los miembros del grupo de DA, con la posible excepción de la cuenta de administrador integrada para el dominio, siempre que se haya protegido como se describe en el [Apéndice D: proteger las cuentas de administrador integradas en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
2. En los GPO vinculados a las unidades organizativas que contienen servidores miembro y estaciones de trabajo en cada dominio, el grupo de DA se debe agregar a los siguientes derechos de usuario:  
   - Denegar el acceso desde la red a este equipo  
   - Denegar el inicio de sesión como trabajo por lotes  
   - Denegar el inicio de sesión como servicio  
   - Denegar el inicio de sesión localmente  
   - Denegar inicio de sesión a través de Servicios de Escritorio remoto  
  
   Esto impedirá que los miembros del grupo de DA inicien sesión en los servidores y estaciones de trabajo miembros. Si se usan servidores de salto para administrar controladores de dominio y Active Directory, asegúrese de que los servidores de salto se encuentran en una unidad organizativa a la que no están vinculados los GPO restrictivos.  

3. La auditoría debe estar configurada para enviar alertas si se realizan modificaciones en las propiedades o la pertenencia del grupo de DA. Estas alertas se deben enviar, como mínimo, a los usuarios o equipos responsables de AD DS la administración y la respuesta a los incidentes. También debe definir procesos y procedimientos para rellenar temporalmente el grupo de DA, incluidos los procedimientos de notificación cuando se realiza un rellenado legítimo del grupo.  

#### <a name="securing-administrators-groups-in-active-directory"></a>Protección de grupos de administradores en Active Directory

Como es el caso de los grupos EA y DA, la pertenencia al grupo administradores (BA) solo debe ser necesaria en escenarios de compilación o recuperación ante desastres. No debe haber cuentas de usuario cotidianas en el grupo de administradores con la excepción de la cuenta de administrador local para el dominio, si se ha protegido como se describe en el [Apéndice D: protección de cuentas de administrador integradas en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

Cuando se requiere acceso a los administradores, las cuentas que necesitan este nivel de acceso deben colocarse temporalmente en el grupo de administradores para el dominio en cuestión. Aunque los usuarios usan las cuentas con privilegios elevados, las actividades se deben auditar y, preferiblemente, realizarse con un usuario que realice los cambios y otro usuario observando los cambios para reducir la probabilidad de un mal uso o una configuración incorrecta. Una vez completadas las actividades, las cuentas deben quitarse inmediatamente del grupo administradores. Esto se puede lograr a través de procedimientos manuales y procesos documentados, a través de software de administración de identidades y accesos con privilegios de terceros (PIM/PAM) o una combinación de ambos.  

De forma predeterminada, los administradores son los propietarios de la mayoría de los objetos de AD DS en sus respectivos dominios. La pertenencia a este grupo puede ser necesaria en escenarios de compilación y recuperación ante desastres en los que se requiere la propiedad o la capacidad de asumir la propiedad de los objetos. Además, DAs y EAs heredan una serie de sus derechos y permisos en virtud de su pertenencia predeterminada en el grupo de administradores. No se debe modificar el anidamiento de grupos predeterminados para los grupos con privilegios en Active Directory y el grupo de administradores de cada dominio debe protegerse tal y como se describe en el [Apéndice G: proteger los grupos de administradores en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)y en las instrucciones generales siguientes.  

1. Quite todos los miembros del grupo administradores, con la excepción posible de la cuenta de administrador local para el dominio, siempre que se haya protegido como se describe en el [Apéndice D: protección de cuentas de administrador integradas en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
2. Los miembros del grupo administradores del dominio nunca deben tener que iniciar sesión en los servidores miembro o las estaciones de trabajo. En uno o más GPO vinculados a unidades organizativas de estaciones de trabajo y servidores miembro en cada dominio, el grupo administradores debe agregarse a los siguientes derechos de usuario:  
   - Denegar el acceso desde la red a este equipo  
   - Denegar el inicio de sesión como un trabajo por lotes,  
   - Denegar el inicio de sesión como servicio  
   - Esto impedirá que los miembros del grupo administradores se usen para iniciar sesión o conectarse a estaciones de trabajo o servidores miembro (a menos que se infrinjan primero varios controles), donde sus credenciales se pueden almacenar en caché y, por tanto, estar en peligro. Nunca se debe usar una cuenta con privilegios para iniciar sesión en un sistema con menos privilegios y aplicar estos controles ofrece protección contra una serie de ataques.  

3. En la unidad organizativa controladores de dominio de cada dominio del bosque, al grupo administradores se le deben conceder los siguientes derechos de usuario (si aún no tienen estos derechos), lo que permitirá a los miembros del grupo administradores realizar las funciones necesarias para un escenario de recuperación ante desastres en todo el bosque:  
   - Obtener acceso a este equipo desde la red  
   - Permitir el inicio de sesión local  
   - Permitir inicio de sesión a través de Servicios de Escritorio remoto  

4. La auditoría debe estar configurada para enviar alertas si se realizan modificaciones en las propiedades o en la pertenencia del grupo administradores. Estas alertas se deben enviar, como mínimo, a los miembros del equipo responsable de AD DS administración. Las alertas también se deben enviar a los miembros del equipo de seguridad y los procedimientos se deben definir para modificar la pertenencia del grupo administradores. En concreto, estos procesos deberían incluir un procedimiento por el que se notifica al equipo de seguridad cuando se va a modificar el grupo de administradores, de modo que cuando se envíen las alertas, se esperen y no se produzca una alarma. Además, los procesos para notificar al equipo de seguridad cuando se ha completado el uso del grupo de administradores y se han quitado las cuentas utilizadas del grupo deben implementarse.  

> [!NOTE]  
> Cuando se implementan restricciones en el grupo de administradores de GPO, Windows aplica la configuración a los miembros del grupo de administradores locales de un equipo además del grupo de administradores del dominio. Por lo tanto, debe tener cuidado al implementar restricciones en el grupo de administradores. Aunque se recomienda prohibir los inicios de sesión de red, Batch y Service para los miembros del grupo de administradores, siempre que sea factible implementarlos, no restrinja los inicios de sesión locales ni los inicios de sesión a través de Servicios de Escritorio remoto. El bloqueo de estos tipos de inicio de sesión puede bloquear la administración legítima de un equipo por parte de los miembros del grupo de administradores locales. En la captura de pantalla siguiente se muestran los valores de configuración que bloquean el uso indebido de cuentas de administrador de dominio y locales integradas, además de un uso incorrecto de los grupos de administradores locales o de dominio integrados. Tenga en cuenta que el derecho de usuario **denegar el inicio de sesión a través de servicios de escritorio remoto** no incluye el grupo administradores, ya que si se incluye en esta configuración, también se bloquearán estos inicios de sesión para las cuentas que son miembros del grupo administradores del equipo local. Si los servicios de en los equipos están configurados para ejecutarse en el contexto de cualquiera de los grupos con privilegios descritos en esta sección, la implementación de esta configuración puede provocar errores en los servicios y las aplicaciones. Por lo tanto, al igual que con todas las recomendaciones de esta sección, debe probar exhaustivamente la configuración de aplicabilidad en su entorno.  
>
> ![modelos de administración con privilegios mínimos](media/Implementing-Least-Privilege-Administrative-Models/SAD_3.gif)  

### <a name="role-based-access-controls-rbac-for-active-directory"></a>Controles de acceso basado en rol (RBAC) para Active Directory

En general, los controles de acceso basado en roles (RBAC) son un mecanismo para agrupar usuarios y proporcionar acceso a los recursos en función de las reglas de negocios. En el caso de Active Directory, la implementación de RBAC para AD DS es el proceso de creación de roles a los que se delegan derechos y permisos para permitir que los miembros del rol realicen tareas administrativas cotidianas sin concederles un privilegio excesivo. RBAC para Active Directory se puede diseñar e implementar a través de herramientas e interfaces nativas, aprovechando el software que ya posee, comprando productos de terceros o cualquier combinación de estos enfoques. En esta sección no se proporcionan instrucciones paso a paso para implementar RBAC para Active Directory, sino que se describen los factores que se deben tener en cuenta a la hora de elegir un enfoque para implementar RBAC en las instalaciones de AD DS.  

#### <a name="native-approaches-to-rbac-for-active-directory"></a>Enfoques nativos de RBAC para Active Directory

En la implementación de RBAC más sencilla, puede implementar roles como AD DS grupos y delegar derechos y permisos a los grupos que les permiten realizar la administración diaria dentro del ámbito designado del rol.  

En algunos casos, los grupos de seguridad existentes en Active Directory se pueden usar para conceder derechos y permisos adecuados para una función de trabajo. Por ejemplo, si los empleados específicos de su organización de TI son responsables de la administración y el mantenimiento de los registros y las zonas DNS, la delegación de esas responsabilidades puede ser tan simple como crear una cuenta para cada administrador de DNS y agregarla al grupo de administradores de DNS en Active Directory. El grupo de administradores de DNS, a diferencia de los grupos con más privilegios, tiene unos pocos derechos en Active Directory, aunque los miembros de este grupo tienen permisos delegados que les permiten administrar DNS y siguen sujeto a riesgos y abuso podrían provocar la elevación de privilegios.

En otros casos, puede que necesite crear grupos de seguridad y delegar derechos y permisos para Active Directory objetos, objetos del sistema de archivos y objetos del registro para permitir que los miembros de los grupos realicen tareas administrativas designadas. Por ejemplo, si los operadores del Departamento de soporte técnico son responsables de restablecer las contraseñas olvidadas, de ayudar a los usuarios con problemas de conectividad y de solucionar problemas de configuración de la aplicación, es posible que tenga que combinar la configuración de delegación en objetos de usuario en Active Directory con privilegios que permitan a los usuarios del Departamento de soporte técnico conectarse de forma remota a los equipos de los usuarios para Para cada rol que defina, debe identificar:  

1. Qué tareas realizan los miembros del rol en una base diaria y qué tareas se realizan con menos frecuencia.  
2. En qué sistemas y en qué aplicaciones deben concederse derechos y permisos a los miembros de un rol.  
3. A qué usuarios se les debe conceder la pertenencia a un rol.  
4. Cómo se realizará la administración de pertenencias a roles.  

En muchos entornos, la creación manual de controles de acceso basados en roles para la administración de un entorno de Active Directory puede ser difícil de implementar y mantener. Si tiene roles y responsabilidades claramente definidos para la administración de la infraestructura de ti, es posible que desee aprovechar las herramientas adicionales que le ayudarán a crear una implementación de RBAC nativa administrable. Por ejemplo, si Forefront Identity Manager (FIM) está en uso en su entorno, puede usar FIM para automatizar la creación y el rellenado de roles administrativos, lo que puede facilitar la administración continuada. Si usa Microsoft Endpoint Configuration Manager y System Center Operations Manager (SCOM), puede usar roles específicos de la aplicación para delegar funciones de administración y supervisión, y también aplicar una configuración y auditoría coherentes en todos los sistemas del dominio. Si ha implementado una infraestructura de clave pública (PKI), puede emitir y requerir tarjetas inteligentes para el personal de TI responsable de la administración del entorno. Con la administración de credenciales de FIM (FIM CM), puede incluso combinar la administración de roles y credenciales para el personal administrativo.  

En otros casos, puede ser preferible que una organización considere la posibilidad de implementar software RBAC de terceros que proporcione la funcionalidad "de la caja". Una serie de proveedores ofrecen soluciones comerciales, de un solo uso (COTS) para RBAC para Active Directory, Windows y directorios y sistemas operativos que no son de Windows. Al elegir entre soluciones nativas y productos de terceros, debe tener en cuenta los siguientes factores:  

1. Presupuesto: al invertir en el desarrollo de RBAC mediante el software y las herramientas que ya posea, puede reducir los costos de software que conlleva la implementación de una solución. Sin embargo, a menos que tenga personal que tenga experiencia en la creación e implementación de soluciones RBAC nativas, es posible que necesite participar en los recursos de consultoría para desarrollar la solución. Debe sopesar detenidamente los costos anticipados de una solución desarrollada de forma personalizada con los costos de implementar una solución "integrada", especialmente si el presupuesto es limitado.  
2. Composición del entorno de TI: Si el entorno se compone principalmente de sistemas Windows, o si ya está aprovechando Active Directory para la administración de sistemas y cuentas que no son de Windows, las soluciones nativas personalizadas pueden proporcionar la solución óptima para sus necesidades. Si su infraestructura contiene muchos sistemas que no ejecutan Windows y que no están administrados por Active Directory, es posible que tenga que tener en cuenta las opciones para la administración de sistemas que no son de Windows independientemente del entorno de Active Directory.  
3. Modelo de privilegios en la solución: Si un producto se basa en la selección de ubicación de sus cuentas de servicio en grupos con privilegios elevados en Active Directory y no ofrece opciones que no requieren que se concedan permisos excesivos al software RBAC, no ha reducido realmente su Active Directory superficie expuesta a ataques. solo ha cambiado la composición de los grupos con más privilegios en el directorio. A menos que un proveedor de la aplicación pueda proporcionar controles para las cuentas de servicio que minimizan la probabilidad de que las cuentas se vean comprometidas y utilicen de forma malintencionada, puede considerar otras opciones.  

### <a name="privileged-identity-management"></a>Privileged Identity Management

Privileged Identity Management (PIM), a veces denominado administración de cuentas con privilegios (PAM) o privileged Credential Management (PCM), es el diseño, la construcción y la implementación de métodos para administrar cuentas con privilegios en la infraestructura. En general, PIM proporciona mecanismos mediante los que se concede a las cuentas derechos y permisos temporales necesarios para realizar funciones de corrección de compilación o interrupción, en lugar de dejar los privilegios conectados permanentemente a las cuentas. Si la funcionalidad de PIM se crea manualmente o se implementa a través de la implementación de software de terceros, es posible que estén disponibles una o varias de las siguientes características:  
  
- Credenciales "almacenes", donde las contraseñas de las cuentas con privilegios se "desprotegen" y se les asigna una contraseña inicial y, a continuación, se "protegen" cuando se han completado las actividades, momento en el que se vuelven a restablecer las contraseñas en las cuentas.  
- Restricciones de límite de tiempo en el uso de credenciales con privilegios  
- Credenciales de uso único  
- Concesión de privilegios generados por el flujo de trabajo con supervisión e informes de actividades realizadas y eliminación automática de privilegios cuando se completan las actividades o hasta que el tiempo asignado ha expirado  
- Reemplazo de credenciales codificadas de forma rígida, como nombres de usuario y contraseñas, en scripts con interfaces de programación de aplicaciones (API) que permiten recuperar credenciales de almacenes según sea necesario.  
- Administración automática de las credenciales de la cuenta de servicio  

### <a name="creating-unprivileged-accounts-to-manage-privileged-accounts"></a>Creación de cuentas sin privilegios para administrar cuentas con privilegios

Uno de los desafíos de la administración de cuentas con privilegios es que, de forma predeterminada, las cuentas que pueden administrar cuentas y grupos con privilegios y protegidos son cuentas protegidas y con privilegios. Si implementa soluciones de RBAC y PIM adecuadas para la instalación de Active Directory, las soluciones pueden incluir enfoques que le permitan despoblar de forma eficaz la pertenencia de los grupos con más privilegios en el directorio, rellenando los grupos solo temporalmente y cuando sea necesario.  

Sin embargo, si implementa RBAC nativo y PIM, debe considerar la creación de cuentas sin privilegios y con la única función de rellenar y despoblar grupos con privilegios en Active Directory cuando sea necesario. [Apéndice I: crear cuentas de administración para cuentas y grupos protegidos en Active Directory](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md) proporciona instrucciones paso a paso que se pueden usar para crear cuentas con este fin.  

### <a name="implementing-robust-authentication-controls"></a>Implementación de controles de autenticación sólidos

*Número de la ley Six: en realidad, alguien está intentando adivinar sus contraseñas.* - [10 leyes inmutables de administración de seguridad](/previous-versions//cc722488(v=technet.10))  

Pass-The-hash y otros ataques de robo de credenciales no son específicos de los sistemas operativos Windows ni son nuevos. El primer ataque Pass-The-hash se creó en 1997. Históricamente, sin embargo, estos ataques requerían herramientas personalizadas, eran aciertos o faltaban en el éxito y requerían que los atacantes tuvieran un grado de habilidad relativamente alto. La introducción de herramientas disponibles y fáciles de usar que extraen de forma nativa las credenciales ha dado como resultado un aumento exponencial del número y el éxito de los ataques de robo de credenciales en los últimos años. Sin embargo, los ataques de robo de credenciales no son, por lo tanto, los únicos mecanismos por los que las credenciales están destinadas y en peligro.  

Aunque debe implementar controles que le ayuden a protegerse frente a ataques de robo de credenciales, también debe identificar las cuentas de su entorno que es más probable que tengan como destino los atacantes e implementar controles de autenticación sólidos para esas cuentas. Si las cuentas con más privilegios usan la autenticación de un solo factor, como nombres de usuario y contraseñas (ambos son "algo que conoce", que es un factor de autenticación), esas cuentas están protegidas de forma débil. Todo lo que necesita un atacante es conocer el nombre de usuario y el conocimiento de la contraseña asociada a la cuenta, y los ataques Pass-The-hash no son necesarios. el atacante puede autenticarse como el usuario en cualquier sistema que acepte credenciales de un solo factor.  

Aunque la implementación de multi-factor Authentication no le protege frente a ataques Pass-The-hash, la implementación de multi-factor Authentication en combinación con sistemas protegidos puede. En las secciones siguientes se proporciona más información acerca de la implementación de sistemas protegidos en la [implementación de hosts administrativos seguros](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md)y las opciones de autenticación.  

#### <a name="general-authentication-controls"></a>Controles de autenticación generales

Si aún no ha implementado la autenticación multifactor como tarjetas inteligentes, considere la posibilidad de hacerlo. Las tarjetas inteligentes implementan la protección forzada de hardware de claves privadas en un par de claves pública y privada, evitando que se tenga acceso a la clave privada de un usuario o se use a menos que el usuario presente el PIN, el código de acceso o el identificador biométrico correctos a la tarjeta inteligente. Aunque el PIN o el código de acceso de un usuario sea interceptado por un registrador de pulsaciones de teclas en un equipo en peligro, para que un atacante vuelva a utilizar el PIN o el código de acceso, la tarjeta también debe estar presente físicamente.

En los casos en que se ha demostrado que las contraseñas complejas son difíciles de implementar debido a la resistencia del usuario, las tarjetas inteligentes proporcionan un mecanismo mediante el cual los usuarios pueden implementar PIN o códigos de acceso relativamente simples sin que las credenciales sean susceptibles a ataques por fuerza bruta o de tabla de arco iris. Los pin de tarjeta inteligente no se almacenan en Active Directory o en bases de datos SAM locales, aunque los hashes de credenciales todavía se pueden almacenar en la memoria protegida de LSASS en los equipos en los que se han usado tarjetas inteligentes para la autenticación.  

#### <a name="additional-controls-for-vip-accounts"></a>Controles adicionales para las cuentas de VIP

Otra ventaja de implementar tarjetas inteligentes u otros mecanismos de autenticación basada en certificados es la posibilidad de aprovechar la garantía del mecanismo de autenticación para proteger datos confidenciales a los que pueden acceder los usuarios VIP. La garantía del mecanismo de autenticación está disponible en los dominios en los que el nivel funcional está establecido en Windows Server 2012 o Windows Server 2008 R2. Cuando está habilitada, la garantía de mecanismo de autenticación agrega una pertenencia a un grupo global designado por el administrador al token de Kerberos de un usuario cuando las credenciales del usuario se autentican durante el inicio de sesión mediante un método de inicio de sesión basado en certificados.  

Esto permite a los administradores de recursos controlar el acceso a los recursos, como archivos, carpetas e impresoras, en función de si el usuario inicia sesión mediante un método de inicio de sesión basado en certificados, además del tipo de certificado utilizado. Por ejemplo, cuando un usuario inicia sesión mediante una tarjeta inteligente, el acceso del usuario a los recursos de la red puede especificarse como diferente del acceso cuando el usuario no usa una tarjeta inteligente (es decir, cuando el usuario inicia sesión escribiendo un nombre de usuario y una contraseña). Para obtener más información sobre la seguridad del mecanismo de autenticación, consulte la [Guía paso a paso de la comprobación del mecanismo de autenticación para AD DS en Windows Server 2008 R2](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd378897(v=ws.10)).  

#### <a name="configuring-privileged-account-authentication"></a>Configuración de la autenticación de cuentas con privilegios

En Active Directory para todas las cuentas administrativas, habilite el atributo **requerir tarjeta inteligente para el inicio de sesión interactivo** y audite los cambios en (como mínimo), cualquiera de los atributos de la pestaña **cuenta** para los objetos de usuario administrativo de cuenta (por ejemplo, CN, Name, samAccountName, userPrincipalName y UserAccountControl).  

Aunque el establecimiento de la contraseña de **requerir tarjeta inteligente para el inicio de sesión interactivo** en cuentas restablece la contraseña de la cuenta en un valor aleatorio de 120 caracteres y requiere tarjetas inteligentes para los inicios de sesión interactivos, los usuarios pueden sobrescribir el atributo con permisos que les permitan cambiar las contraseñas de las cuentas.  

En otros casos, en función de la configuración de las cuentas de Active Directory y de la configuración de certificados en Active Directory servicios de Certificate Server (AD CS) o una PKI de terceros, los atributos de nombre principal de usuario (UPN) para cuentas administrativas o de VIP pueden destinarse a un tipo de ataque específico, como se describe aquí.  

##### <a name="upn-hijacking-for-certificate-spoofing"></a>Secuestro de UPN para la suplantación de certificados

Aunque un análisis exhaustivo de los ataques contra las infraestructuras de clave pública (PKI) está fuera del ámbito de este documento, los ataques contra PKI públicas y privadas han aumentado exponencialmente desde 2008. Las infracciones de las PKI públicas se han generalizado en general, pero los ataques contra la PKI interna de una organización son quizás incluso más prolíficas. Uno de estos ataques aprovecha Active Directory y certificados para permitir que un atacante suplantee las credenciales de otras cuentas de una manera que pueda ser difícil de detectar.  

Cuando se presenta un certificado para la autenticación en un sistema unido a un dominio, el contenido del asunto o el atributo del nombre alternativo del sujeto (SAN) del certificado se utilizan para asignar el certificado a un objeto de usuario en Active Directory. Dependiendo del tipo de certificado y de cómo se construya, el atributo de asunto de un certificado normalmente contiene el nombre común (CN) del usuario, como se muestra en la siguiente captura de pantalla.  

![modelos de administración con privilegios mínimos](media/Implementing-Least-Privilege-Administrative-Models/SAD_4.gif)  

De forma predeterminada, Active Directory construye el CN de un usuario concatenando el nombre de la cuenta + "" + Apellido. Sin embargo, los componentes de CN de los objetos de usuario de Active Directory no son necesarios o se garantiza que son únicos, y mover una cuenta de usuario a una ubicación diferente en el directorio cambia el nombre distintivo (DN) de la cuenta, que es la ruta de acceso completa al objeto en el directorio, tal como se muestra en el panel inferior de la captura de pantalla anterior.  

Dado que no se garantiza que los nombres de sujeto del certificado sean estáticos o únicos, el contenido del nombre alternativo del sujeto suele usarse para buscar el objeto de usuario en Active Directory. El atributo SAN de los certificados emitidos para los usuarios de las entidades de certificación de empresa (Active Directory CA integradas) normalmente contiene el UPN o la dirección de correo electrónico del usuario. Dado que se garantiza que los UPN son únicos en un bosque de AD DS, la ubicación de un objeto de usuario por UPN se realiza normalmente como parte de la autenticación, con o sin certificados implicados en el proceso de autenticación.  

El uso de UPN en los atributos de SAN en los certificados de autenticación puede ser aprovechado por los atacantes para obtener certificados fraudulentos. Si un atacante ha puesto en peligro una cuenta que tiene la capacidad de leer y escribir UPN en objetos de usuario, el ataque se implementa de la siguiente manera:  

El atributo UPN de un objeto de usuario (por ejemplo, un usuario VIP) se cambia temporalmente a un valor diferente. El atributo de nombre de cuenta SAM y el CN también se pueden cambiar en este momento, aunque normalmente esto no es necesario por las razones descritas anteriormente.  

Cuando se ha cambiado el atributo UPN de la cuenta de destino, una cuenta de usuario habilitada o desusada o un atributo UPN de la cuenta de usuario recién creada se cambia al valor que se asignó originalmente a la cuenta de destino. Las cuentas de usuario obsoletas y habilitadas son cuentas que no han iniciado sesión durante largos períodos de tiempo, pero que no se han deshabilitado. Están destinadas a atacantes que quieren "ocultarse sin problemas" por los siguientes motivos:  

1. Dado que la cuenta está habilitada pero no se ha usado recientemente, es poco probable que el uso de la cuenta desencadene alertas de la manera en que se puede habilitar una cuenta de usuario deshabilitada.  
2. El uso de una cuenta existente no requiere la creación de una nueva cuenta de usuario que pueda ser detectada por el personal administrativo.  
3. Las cuentas de usuario obsoletas que todavía están habilitadas suelen ser miembros de varios grupos de seguridad y se les concede acceso a los recursos de la red, lo que simplifica el acceso y la "fusión" en un rellenado de usuarios existente.  

La cuenta de usuario en la que se ha configurado el UPN de destino se usa para solicitar uno o más certificados de Active Directory servicios de Certificate Server.  

Cuando se han obtenido certificados para la cuenta del atacante, los UPN de la cuenta "nueva" y la cuenta de destino se devuelven a sus valores originales.  

Ahora, el atacante tiene uno o más certificados que se pueden presentar para la autenticación en recursos y aplicaciones como si el usuario es el usuario VIP cuya cuenta se modificó temporalmente. Aunque un análisis completo de todas las formas en que los certificados y la PKI pueden ser objetivo de los atacantes está fuera del ámbito de este documento, este mecanismo de ataque se proporciona para ilustrar por qué debe supervisar las cuentas con privilegios y VIP en AD DS para los cambios, especialmente para los cambios en cualquiera de los atributos de la pestaña **cuenta** de la cuenta (por ejemplo, CN, Name, samAccountName, UserPrincipalName y UserAccountControl). Además de supervisar las cuentas, debe restringir quién puede modificar las cuentas como un pequeño conjunto de usuarios administrativos como sea posible. Del mismo modo, las cuentas de los usuarios administrativos deben protegerse y supervisarse para realizar cambios no autorizados.  
