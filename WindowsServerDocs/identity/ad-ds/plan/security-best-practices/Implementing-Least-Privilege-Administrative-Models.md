---
ms.assetid: 7a7ab95c-9cb3-4a7b-985a-3fc08334cf4f
title: Implementación de modelos administrativos de menor privilegio
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 01113b957c6b0be4e18955b5ddc78be3f463abe4
ms.sourcegitcommit: 236a8ae1da12cea1acfff3f306246db0f022354d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2019
ms.locfileid: "67412209"
---
# <a name="implementing-least-privilege-administrative-models"></a>Implementación de modelos administrativos de menor privilegio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El siguiente fragmento es de [el administrador Accounts Security Planning Guide](https://technet.microsoft.com/library/cc162797.aspx), publicó por primera vez el 1 de abril de 1999:

> "Más relacionadas con la seguridad de cursos de aprendizaje y documentación hablan de la implementación de un principio de privilegio mínimo, aunque las organizaciones rara vez seguirlo. El principio es simple y el impacto de la aplicación de correctamente en gran medida aumenta la seguridad y reduce el riesgo. El principio indica que todos los usuarios deben iniciar sesión con una cuenta de usuario que tiene los permisos mínimos necesarios para completar la tarea actual y nada más. Si lo hace, proporciona protección contra código malintencionado, entre otros ataques. Este principio se aplica a equipos y los usuarios de esos equipos.   
> "Este principio funciona tan bien uno de los motivos es que nos vemos obligados a realizar una investigación interna. Por ejemplo, debe determinar los privilegios de acceso que realmente necesita un equipo o usuario y, a continuación, implementarlos. Para muchas organizaciones, esta tarea podría parecer inicialmente una gran cantidad de trabajo; Sin embargo, es un paso esencial para proteger correctamente el entorno de red.
> "Se deben conceder a todos los usuarios del Administrador de dominio sus privilegios de dominio en el concepto de privilegios mínimos. Por ejemplo, si un administrador inicia sesión con una cuenta con privilegios y sin darse cuenta ejecuta un programa antivirus, el virus tiene acceso administrativo al equipo local y a todo el dominio. Si el administrador hubiera iniciado la sesión con una cuenta sin privilegios (no administrativa), ámbito de los virus de los daños solo sería el equipo local porque se ejecuta como un usuario del equipo local.
> "En otro ejemplo, las cuentas a las que concede derechos de administrador de nivel de dominio deben no tienen derechos elevados en otro bosque, incluso si hay una relación de confianza entre los bosques. Esta táctica ayuda a impedir daños se extiendan si un atacante consigue poner en peligro un bosque administrado. Las organizaciones deben auditar periódicamente su red para protegerse frente a no autorizado elevación de privilegios."  

El extracto siguiente procede de la [Microsoft Windows Security Resource Kit](https://www.microsoftpressstore.com/store/microsoft-windows-security-resource-kit-9780735621749), primera 2005 publicado en:  

> "Siempre pensar en seguridad en términos de conceder la menor cantidad de privilegios necesarios para llevar a cabo la tarea. Si debe estar en peligro una aplicación con muchos privilegios, el atacante podría ser capaz expandir el ataque más allá de lo que sería si hubiera sido la aplicación en la cantidad mínima de privilegios posible. Por ejemplo, examine las consecuencias de un administrador de red sin darse cuenta abrir datos adjuntos por correo electrónico que se inicia un virus. Si ha iniciado el administrador sobre el uso de la cuenta de administrador de dominio, el virus tendrá privilegios de administrador en todos los equipos del dominio y, por tanto, el acceso ilimitado a casi todos los datos en la red. Si el administrador ha iniciado sesión con una cuenta de administrador local, el virus tendrá privilegios de administrador en el equipo local y, por tanto, podría acceder a los datos en el equipo e instalar el software malintencionado, como software de trazo de la clave de registro en el equipo. Si el administrador ha iniciado sesión con una cuenta de usuario normal, el virus tendrá acceso únicamente a los datos del administrador y no podrá instalar el software malintencionado. Mediante el uso de los privilegios mínimos necesarios para leer el correo electrónico, en este ejemplo, el alcance del riesgo potencial se reduce enormemente."  

## <a name="the-privilege-problem"></a>El problema de privilegios

Los principios descritos en los extractos anteriores no han cambiado, sin embargo, en la evaluación de las instalaciones de Active Directory, descubrimos invariablemente número excesivo de las cuentas que se han concedido derechos y permisos más allá de los requeridos para realizar diarias trabajo. El tamaño del entorno afecta a los números sin procesar de las cuentas con privilegios demasiado, pero no los directorios proportionmidsized pueden tener docenas de cuentas en los grupos con privilegios más elevados, instalaciones de gran tamaño pueden tener cientos o incluso miles de personas. Con pocas excepciones, independientemente de la sofisticación de habilidades y el arsenal, un atacante los atacantes suelen seguir la ruta de acceso de menor resistencia. Aumentan la complejidad de sus herramientas y el enfoque únicamente cuando más sencillo fallan de mecanismos o se frustra este tipo por defensores.  

Por desgracia, la ruta de acceso de menor resistencia en muchos entornos ha demostrado para ser el uso excesivo de las cuentas con privilegio amplia y profunda. Privilegios amplios son derechos y permisos que permiten una cuenta realizar actividades específicas a través de una sección transversal de gran tamaño del entorno, por ejemplo, personal del departamento puede tener permisos de permitirles restablecer las contraseñas en varias cuentas de usuario.  

Privilegios profundos son privilegios eficaces que se aplican a un segmento de la población, por ejemplo, dando a un ingeniero de derechos de administrador en un servidor para que puedan realizar reparaciones estrecho. Privilegio amplio ni privilegios profundo están necesariamente peligroso, pero cuando muchas cuentas en el dominio permanentemente se conceden privilegios muy extenso y, si solo una de las cuentas se pone en peligro, rápidamente se puede usar para volver a configurar el entorno para el efectos del atacante o incluso para destruir grandes segmentos de la infraestructura.  

Los ataques PASS-the-hash, que son un tipo de ataque de robo de credenciales, son omnipresentes porque las herramientas para llevarlas a cabo están libremente disponible y es fácil de usar, y dado que muchos entornos son vulnerables a los ataques. Los ataques PASS-the-hash, sin embargo, no son el problema real. El quid del problema es doble:  

1. Normalmente es fácil para un atacante obtener privilegios profundo en un único equipo y, a continuación, propagar ese privilegio ampliamente a otros equipos.  
2. Normalmente hay demasiadas cuentas permanentes con altos niveles de privilegios en todo el entorno informático.

Incluso si se eliminan los ataques pass-the-hash, los atacantes simplemente usaría diversas tácticas, no una estrategia diferente. En lugar de plantación malware que contiene las herramientas de robo de credenciales, que es posible que Plante malware que registra las pulsaciones de teclas o aprovechar cualquier cantidad de otros métodos para capturar las credenciales que son eficaces en el entorno. Independientemente de las tácticas, los destinos siguen siendo los mismos: cuentas con privilegios amplia y profunda.  

Concesión de privilegios excesivos no sólo se encuentra en Active Directory en entornos en peligro. Cuando una organización ha desarrollado el hábito de conceder más privilegios del que es necesario, se encuentra normalmente en toda la infraestructura, como se describe en las secciones siguientes.  

## <a name="in-active-directory"></a>En Active Directory

En Active Directory, es común encontrar que los grupos EA, DA y BA contienen un número excesivo de cuentas. Con más frecuencia, el grupo EA de la organización contiene a menos miembros, DA grupos suelen contengan un multiplicador del número de usuarios en el grupo EA y grupos de los administradores suelen contengan a más miembros que la población de los otros grupos que se combinan. Esto suele deberse a la creencia de que los administradores de algún modo son "menos privilegios" que DAs o EAs. Mientras los derechos y permisos concedidos a cada uno de estos grupos son diferentes, que deben eficazmente considerarse como igualmente eficaz grupos porque un miembro de uno puede convertirse en miembro de los otros dos.  

## <a name="on-member-servers"></a>En los servidores miembro

Cuando se recupera de la pertenencia a grupos de administradores locales en los servidores miembro en muchos entornos, encontramos pertenencia comprendido entre un puñado de cuentas locales y de dominio, y decenas de grupos anidados que, cuando se expande, revelar cientos, incluso miles de cuentas con privilegios de administrador local en los servidores. En muchos casos, los grupos de dominio con pertenencia a grande están anidados en grupos locales de administradores los servidores miembros, sin tener en cuenta el hecho de que cualquier usuario que puede modificar la pertenencia a los grupos del dominio puede obtener el control administrativo de todos los sistemas en el que el grupo tiene ha anidados en un grupo de administradores local.  

## <a name="on-workstations"></a>En las estaciones de trabajo

Aunque las estaciones de trabajo suelen tengan muchas menos miembros en sus grupos de administradores locales de los servidores miembro lo hacen, en muchos entornos, los usuarios reciben la pertenencia al grupo local Administradores en sus equipos personales. Cuando esto ocurre, incluso si UAC está habilitado, los usuarios presentan un riesgo para la integridad de sus estaciones de trabajo con privilegios elevados.  

> [!IMPORTANT]  
> Debe considerar detenidamente si los usuarios necesitan derechos administrativos en sus estaciones de trabajo y, si es así, puede ser un mejor enfoque crear una cuenta local independiente en el equipo que sea miembro del grupo Administradores. Cuando los usuarios necesitan elevación, que pueden presentar las credenciales de dicha cuenta local de elevación, pero dado que la cuenta es local, no se puede usar para poner en peligro otros equipos o acceder a los recursos del dominio. Al igual que con cualquier cuenta local, sin embargo, las credenciales para la cuenta con privilegios local deben ser únicas; Si crea una cuenta local con las mismas credenciales en varias estaciones de trabajo, exponen los equipos a los ataques pass-the-hash.  

## <a name="in-applications"></a>En las aplicaciones

En los ataques en el que el destino es la propiedad intelectual de la organización, las cuentas que se han concedido privilegios eficaces en las aplicaciones pueden ir dirigidas para permitir la filtración de datos. Aunque las cuentas que tienen acceso a datos confidenciales es posible que se han concedido privilegios no elevados en el dominio o el sistema operativo, las cuentas que puede manipular la configuración de una aplicación o proporciona acceso a la información de la aplicación riesgo está presente.  

## <a name="in-data-repositories"></a>En los repositorios de datos

Como sucede con otros destinos, los atacantes que buscan el acceso a la propiedad intelectual en forma de documentos y otros archivos pueden tener como destino las cuentas que controlar el acceso al archivo almacena las cuentas que tengan acceso directo a los archivos, o incluso grupos o roles que tienen acceso a los archivos. Por ejemplo, si se usa un servidor de archivos para almacenar documentos de contrato y se concede acceso a los documentos mediante el uso de un grupo de Active Directory, si un atacante puede modificar la pertenencia del grupo de puede agregar las cuentas en peligro al grupo y tener acceso al contrato documentos. En los casos en los que se proporciona acceso a documentos en aplicaciones como SharePoint, los atacantes pueden dirigirse a las aplicaciones como se describió anteriormente.  

## <a name="reducing-privilege"></a>Reducción de privilegios

El mayor y más complejo un entorno, más difícil es administrar y proteger. En organizaciones pequeñas, revisar y reducir los privilegios pueden ser una propuesta relativamente sencilla, pero cada servidor adicional, estación de trabajo, la cuenta de usuario y aplicación en uso en una organización agregan otro objeto que debe protegerse. Porque puede ser difícil o incluso imposible correctamente proteger todos los aspectos de una organización de la infraestructura de TI deben centrarse los esfuerzos en primer lugar en las cuentas cuya privilegio de crear el mayor riesgo, que normalmente son las cuentas con privilegios integradas y grupos en Active Directory y con privilegios de las cuentas locales en servidores miembro y estaciones de trabajo.  

### <a name="securing-local-administrator-accounts-on-workstations-and-member-servers"></a>Protección de cuentas de administrador Local en servidores miembro y estaciones de trabajo

Aunque este documento se centra en la protección de Active Directory, tal como se ha explicado anteriormente, la mayoría de los ataques contra el directorio de inicio como los ataques contra los hosts individuales. No se puede proporcionar directrices completas para proteger grupos locales en los sistemas de miembro, pero se pueden usar las siguientes recomendaciones para ayudarle a proteger las cuentas de administrador locales en servidores miembro y estaciones de trabajo.  

#### <a name="securing-local-administrator-accounts"></a>Protección de cuentas de administrador Local

En todas las versiones de Windows actualmente en soporte estándar, la cuenta de administrador local está deshabilitada de forma predeterminada, lo que hace que la cuenta de que quede inutilizable para pass-the-hash y otros ataques de robo de credenciales. Sin embargo, en los dominios que contienen los sistemas operativos heredados o se han habilitado las cuentas de administrador locales, estas cuentas pueden utilizarse como se describió anteriormente para propagar compromiso entre los servidores miembro y estaciones de trabajo. Por este motivo, se recomiendan los siguientes controles para todas las cuentas de administrador locales en los sistemas unidos a un dominio.  

Se proporcionan instrucciones detalladas para implementar estos controles en [Apéndice H: Protección de cuentas de administrador Local y grupos](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md). Antes de implementar esta configuración, sin embargo, asegúrese de que las cuentas de administrador locales no actualmente sirven en el entorno para ejecutar servicios en equipos o realizar otras actividades que estas cuentas no se deben usar. Pruebe estas opciones antes de implementarlos en un entorno de producción.  

#### <a name="controls-for-local-administrator-accounts"></a>Controles para las cuentas de administrador Local

Las cuentas de administrador integrada nunca deben utilizarse como cuentas de servicio en los servidores miembro, ni se deben usar para iniciar sesión en equipos locales (excepto en el modo seguro, lo que se permite incluso si la cuenta está deshabilitada). El objetivo de implementar la configuración descrita aquí es evitar que la cuenta de administrador local de cada equipo que se puede usar a menos que primero se invierten los controles de protección. Al implementar estos controles y supervisión de cuentas de administrador para los cambios, se pueden reducir significativamente la probabilidad de éxito de un ataque que las cuentas de administrador local de los destinos.  

##### <a name="configuring-gpos-to-restrict-administrator-accounts-on-domain-joined-systems"></a>Configuración de GPO para restringir las cuentas de administrador en los sistemas unidos a dominio

En uno o varios GPO que crear y vincular a la estación de trabajo y cada servidor miembro unidades organizativas en cada dominio, agregue la cuenta de administrador a los siguientes derechos de usuario en **Policies\ de seguridad\Directivas de Windows\Configuración de configuración del equipo\Directivas\Configuración de equipo Las asignaciones de derechos de usuario**:  

- Denegar el acceso desde la red a este equipo
- Denegar el inicio de sesión como trabajo por lotes
- Denegar el inicio de sesión como servicio
- Denegar inicio de sesión a través de Servicios de Escritorio remoto

Al agregar las cuentas de administrador a estos derechos de usuario, especifique si va a agregar la cuenta de administrador local o el administrador del dominio de la cuenta a la forma que etiqueta la cuenta. Por ejemplo agregar la cuenta de administrador del dominio NWTRADERS a estos denegar derechos de propiedad, escriba la cuenta como **NWTRADERS\Administrador**, o busque la cuenta de administrador para el dominio NWTRADERS. Para asegurarse de que restringe la cuenta de administrador local, escriba **administrador** en estas opciones en el Editor de objetos de directiva de grupo de derechos de usuario.  

> [!NOTE]  
> Incluso si se cambia el nombre de las cuentas de administrador locales, las directivas se seguirán aplicando.  

Esta configuración se asegurará de que cuenta de administrador de un equipo no puede utilizarse para conectarse a los otros equipos, incluso si está forma accidental o malintencionada habilitado. No se puede deshabilitar completamente los inicios de sesión local con la cuenta de administrador local, ni debe intentar hacerlo, porque la cuenta de administrador local del equipo está diseñado para usarse en escenarios de recuperación ante desastres.  

Debe un servidor miembro o estación de trabajo se convierten en separará del dominio con ningún otro privilegio concedido administrativas de las cuentas locales, se puede arrancar el equipo en el modo seguro, se puede habilitar la cuenta de administrador y, a continuación, se puede usar la cuenta para efecto reparaciones en el equipo. Cuando se completan las reparaciones, nuevo debe deshabilitarse la cuenta de administrador.  

### <a name="securing-local-privileged-accounts-and-groups-in-active-directory"></a>Protección de cuentas con privilegios locales y grupos en Active Directory

*Ley número seis: Un equipo sólo es tan seguro como el administrador es de confianza.* - [10 leyes inmutables de seguridad (versión 2.0)](https://technet.microsoft.com/security/hh278941.aspx)  

La información proporcionada aquí está diseñada para proporcionar directrices generales para proteger las cuentas integradas de privilegio más altas y grupos en Active Directory. También se proporcionan instrucciones paso a paso detalladas en [apéndice D: Protección de cuentas de administrador integrada en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md), [Apéndice E: Protección de grupos de administradores de empresa en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md), [Apéndice F: Protección de grupos de administradores de dominio de Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)y en [apéndice G: Protección de grupos de administradores en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md).  

Antes de implementar cualquiera de estas opciones, también debe probar todas las configuraciones exhaustivamente para determinar si son apropiadas para su entorno. No todas las organizaciones podrán implementar estas opciones.  

#### <a name="securing-built-in-administrator-accounts-in-active-directory"></a>Protección de cuentas de administrador integrada en Active Directory

En cada dominio de Active Directory, se crea una cuenta de administrador como parte de la creación del dominio. Esta cuenta es de forma predeterminada, un miembro de los grupos de administrador en el dominio y administradores de dominio y, si el dominio es el dominio raíz del bosque, la cuenta también es un miembro del grupo Administradores de empresas. Uso de la cuenta de administrador local de un dominio se debe reservar solo para las actividades de compilación inicial y, posiblemente, escenarios de recuperación ante desastres. Para asegurarse de que puede utilizarse una cuenta de administrador integrada para llevar a cabo reparaciones en caso de que no se puede usar ninguna otra cuenta, no debe cambiar la pertenencia predeterminada de la cuenta de administrador en cualquier dominio del bosque. En su lugar, debe seguir las directrices para ayudar a proteger la cuenta de administrador en cada dominio del bosque. Se proporcionan instrucciones detalladas para implementar estos controles en [apéndice D: Protección de cuentas de administrador integrada en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

#### <a name="controls-for-built-in-administrator-accounts"></a>Controles para las cuentas de administrador integrada

El objetivo de la implementación de la configuración descrita aquí es evitar que la cuenta de administrador de cada dominio (no un grupo) que se puede usar a menos que se invierte una serie de controles. Al implementar estos controles y supervisión de las cuentas de administrador de cambios, puede reducir significativamente la probabilidad de éxito de los ataques mediante el aprovechamiento de la cuenta de administrador de un dominio. Para la cuenta de administrador en cada dominio del bosque, debe establecer la siguiente configuración.  

##### <a name="enable-the-account-is-sensitive-and-cannot-be-delegated-flag-on-the-account"></a>Habilitar la marca "Cuenta es importante y no se puede delegar" en la cuenta

De forma predeterminada, todas las cuentas en Active Directory se pueden delegar. La delegación permite que un equipo o un servicio para presentar las credenciales de una cuenta que se ha autenticado en el equipo o un servicio en otros equipos para obtener servicios en nombre de la cuenta. Cuando se habilita la **cuenta es importante y no se puede delegar** atributo en una cuenta de dominio, no se pueden presentar las credenciales de cuenta a otros equipos o servicios en la red, lo que limita los ataques que aprovechan delegación para usar las credenciales de cuenta en otros sistemas.  

##### <a name="enable-the-smart-card-is-required-for-interactive-logon-flag-on-the-account"></a>Habilitar la marca "tarjeta inteligente es necesaria para el inicio de sesión interactivo" en la cuenta

Cuando se habilita la **tarjeta inteligente es necesaria para el inicio de sesión interactivo** atributo en una cuenta de Windows restablece la contraseña de la cuenta con un valor aleatorio de 120 caracteres. Al establecer esta marca en las cuentas de administrador integradas, asegúrese de que la contraseña de la cuenta no sólo es largo y complejo, pero no se conoce a cualquier usuario. No es técnicamente necesario crear tarjetas inteligentes para las cuentas antes de habilitar este atributo, pero si es posible, se debe crear tarjetas inteligentes para cada cuenta de administrador antes de configurar las restricciones y las tarjetas inteligentes se deben almacenar en ubicaciones seguras.  

Aunque al establecer el **tarjeta inteligente es necesaria para el inicio de sesión interactivo** marca restablece la contraseña de la cuenta, no impide que un usuario con derechos para restablecer la contraseña de la cuenta de configuración de la cuenta en un valor conocido y el uso de la nombre de cuenta y nueva contraseña para acceder a recursos de la red. Por este motivo, debe implementar los siguientes controles adicionales en la cuenta.  

##### <a name="configuring-gpos-to-restrict-domains-administrator-accounts-on-domain-joined-systems"></a>Configuración de GPO para restringir las cuentas de administrador de dominios en los sistemas unidos a dominio

Aunque deshabilitar la cuenta de administrador en un dominio hace que la cuenta de que puede usar de forma eficaz, debe implementar restricciones adicionales en la cuenta en caso de que la cuenta está habilitada de forma accidental o malintencionada. Aunque en última instancia, se pueden invertir estos controles mediante la cuenta de administrador, el objetivo es crear controles que reducir el progreso de un atacante y limitar el daño de la cuenta puede infligir.  

En uno o varios GPO que crear y vincular a la estación de trabajo y cada servidor miembro unidades organizativas en cada dominio, agregar cuenta de administrador de cada dominio a los siguientes derechos de usuario en **seguridad\Directivas de Windows\Configuración de configuración del equipo\Directivas\Configuración de equipo Locales\Asignación de derechos de asignaciones**:  

- Denegar el acceso desde la red a este equipo  
- Denegar el inicio de sesión como trabajo por lotes  
- Denegar el inicio de sesión como servicio  
- Denegar inicio de sesión a través de Servicios de Escritorio remoto  

> [!NOTE]  
> Al agregar las cuentas de administrador locales para esta configuración, debe especificar si va a configurar las cuentas de administrador locales o las cuentas de administrador de dominio. Por ejemplo, para agregar la cuenta de administrador local del dominio NWTRADERS a estos denegar derechos, debe escribir la cuenta como **NWTRADERS\Administrador**, o busque la cuenta de administrador local para el dominio NWTRADERS. Si escribe **administrador** en esta configuración de derechos de usuario en el Editor de objetos de directiva de grupo, restringirá la cuenta de administrador local en cada equipo al que se aplica el GPO.  
>
> Se recomienda limitar las cuentas de administrador locales en los servidores miembro y estaciones de trabajo en la misma manera que las cuentas de administrador basado en dominio. Por lo tanto, generalmente debe agregar la cuenta de administrador para cada dominio del bosque y la cuenta de administrador para los equipos locales para esta configuración de derechos de usuario. 

##### <a name="configuring-gpos-to-restrict-administrator-accounts-on-domain-controllers"></a>Configuración de GPO para restringir las cuentas de administrador en los controladores de dominio

En cada dominio del bosque, se debe modificar la directiva predeterminada de controladores de dominio o una directiva vinculada a la unidad organizativa controladores de dominio para agregar la cuenta de administrador de cada dominio a los siguientes derechos de usuario en **configuración del equipo Windows Windows\Configuración configuración de seguridad\Directivas locales\Asignación de derechos de asignaciones**:  

- Denegar el acceso desde la red a este equipo  
- Denegar el inicio de sesión como trabajo por lotes  
- Denegar el inicio de sesión como servicio  
- Denegar inicio de sesión a través de Servicios de Escritorio remoto  

> [!NOTE]  
> Esta configuración se asegurará de que la cuenta de administrador local no puede utilizarse para conectarse a un controlador de dominio, aunque la cuenta, si está habilitada, puede iniciar sesión localmente en los controladores de dominio. Dado que esta cuenta solo se debería habilitar y usar en escenarios de recuperación ante desastres, se prevé que el acceso físico al menos un controlador de dominio esté disponible o que pueden ser otras cuentas con permisos de acceso remoto a los controladores de dominio usar.  

##### <a name="configure-auditing-of-built-in-administrator-accounts"></a>Configurar la auditoría de cuentas de administrador integrada

Cuando haya protegido de la cuenta de administrador de cada dominio y deshabilitado, debe configurar la auditoría para supervisar los cambios en la cuenta. Si la cuenta está habilitada, se restablece su contraseña o cualquier otra modificación se realiza en la cuenta, las alertas se enviarán a los usuarios o los equipos responsables de la administración de AD DS, además de los equipos de respuesta a incidentes de su organización.  

### <a name="securing-administrators-domain-admins-and-enterprise-admins-groups"></a>Protección de los administradores, administradores de dominio y grupos de administradores de empresa

#### <a name="securing-enterprise-admin-groups"></a>Protección de grupos de administración empresarial

El grupo de administradores de empresas, que se aloja en el dominio raíz del bosque, no debe contener ningún usuario en forma diaria, con la excepción posible de la cuenta de administrador local del dominio, siempre que se protege a medida que se ha descrito anteriormente y en [ Apéndice D: Protección de cuentas de administrador integrada en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

Cuando se requiere acceso EA, los usuarios cuyas cuentas necesitan permisos y derechos EA se deben colocar temporalmente en el grupo de administradores de empresas. Aunque los usuarios utilizan las cuentas con privilegios elevados, sus actividades se deben auditar y si es posible realizar con un usuario realiza los cambios y observar los cambios de otro usuario para minimizar la posibilidad de uso incorrecto accidental o una configuración incorrecta . Cuando se han completado las actividades, las cuentas deben quitarse del grupo de EA. Esto se consigue a través de procedimientos manuales y documenta los procesos, software management (PIM/PAM) de identidad y acceso con privilegios de terceros o una combinación de ambos. Instrucciones para crear las cuentas que puede usarse para controlar la pertenencia a grupos con privilegios en Active Directory se proporcionan en [cuentas atractivas para el robo de credenciales](../../../ad-ds/plan/security-best-practices/Attractive-Accounts-for-Credential-Theft.md) y se proporcionan instrucciones detalladas en [Apéndice I: Creación de cuentas de administración para proteger cuentas y grupos en Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  

Son administradores de empresas, de forma predeterminada, los miembros del grupo de administradores integrado en cada dominio del bosque. Quitar el grupo de administradores de organización de los grupos de administradores en cada dominio es una modificación inadecuada porque en el caso de un escenario de recuperación ante desastres de bosque, derechos EA probablemente será necesarios. Si se ha quitado el grupo de administradores de organización de los grupos de administradores en un bosque, que debe agregarse al grupo de administradores en cada dominio y deben implementarse los siguientes controles adicionales:  

- Como se describió anteriormente, el grupo de administradores de organización no debe contener ningún usuario en forma diaria, con la excepción posible de la cuenta de administrador del dominio raíz del bosque, que debe protegerse como se describe en [apéndice D: Protección de cuentas de administrador integrada en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
- En los GPO vinculados a unidades organizativas que contiene los servidores miembro y estaciones de trabajo en cada dominio, el grupo de contrato Enterprise debe agregarse a los siguientes derechos de usuario:  
   - Denegar el acceso desde la red a este equipo  
   - Denegar el inicio de sesión como trabajo por lotes  
   - Denegar el inicio de sesión como servicio  
   - Denegar el inicio de sesión localmente  
   - Denegar inicio de sesión a través de servicios de escritorio remoto.  

Esto impedirá que los miembros del grupo de EA de inicio de sesión en estaciones de trabajo y servidores miembro. Si los servidores de salto se usan para administrar controladores de dominio y Active Directory, asegúrese de que los servidores de salto se encuentran en una unidad organizativa a la que no están vinculados los GPO restrictivos.  

- La auditoría debe configurarse para enviar alertas si se realizan modificaciones en las propiedades o la pertenencia al grupo EA. Estas alertas se enviarán, como mínimo, a los usuarios o los equipos responsables de la respuesta de incidente y la administración de Active Directory. También debe definir los procesos y procedimientos para rellenar el grupo EA, incluidos los procedimientos de notificación cuando se realiza un rellenado legítimo del grupo de temporalmente.  
  
#### <a name="securing-domain-admins-groups"></a>Protección de grupos de administradores de dominio

Como sucede con el grupo de administradores de empresas, la pertenencia a grupos de administradores de dominio deben solo en escenarios de compilación o recuperación ante desastres. No debe haber ninguna cuenta de usuario diarias en el grupo DA con la excepción de la cuenta de administrador local para el dominio, si se protegió como se describe en [apéndice D: Protección de cuentas de administrador integrada en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
Cuando se requiere DA acceso, se deben colocar temporalmente las cuentas que necesitan este nivel de acceso en el grupo de Acelerador de desarrollo para el dominio en cuestión. Aunque los usuarios utilizan las cuentas con privilegios elevados, las actividades se deben auditar y si es posible realizar con un usuario realiza los cambios y observar los cambios de otro usuario para minimizar la posibilidad de uso incorrecto accidental o una configuración incorrecta. Cuando se han completado las actividades, las cuentas deben quitarse del grupo Admins. del dominio. Esto se consigue a través de procedimientos manuales y documenta los procesos a través de software management (PIM/PAM) de identidad y acceso con privilegios de terceros, o una combinación de ambos. Instrucciones para crear las cuentas que puede usarse para controlar la pertenencia a grupos con privilegios en Active Directory se proporcionan en [apéndice I: Creación de cuentas de administración para proteger cuentas y grupos en Active Directory](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
Administradores de dominio son, de forma predeterminada, los miembros de los grupos de administradores locales en todos los servidores miembro y estaciones de trabajo en sus dominios respectivos. No se debe modificar este anidamiento predeterminada porque afecta a las opciones de recuperación ante desastres y compatibilidad. Si se quitaron los grupos Admins. del dominio de los grupos de administradores locales en los servidores miembro, se debe agregar al grupo de administradores en cada servidor miembro y la estación de trabajo en el dominio a través de la configuración de grupos restringidos en los GPO vinculados. Los siguientes controles generales se describen con más detalle en [Apéndice F: Protección de grupos de administradores de dominio de Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md) también debe implementarse.  

Para el grupo de administradores de dominio en cada dominio del bosque:  

1. Quite todos los miembros del grupo DA, con la excepción posible de la cuenta de administrador integrada para el dominio, siempre que se protegió como se describe en [apéndice D: Protección de cuentas de administrador integrada en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
2. En los GPO vinculados a unidades organizativas que contiene los servidores miembro y estaciones de trabajo en cada dominio, debe agregarse al grupo DA a los siguientes derechos de usuario:  
   - Denegar el acceso desde la red a este equipo  
   - Denegar el inicio de sesión como trabajo por lotes  
   - Denegar el inicio de sesión como servicio  
   - Denegar el inicio de sesión localmente  
   - Denegar inicio de sesión a través de Servicios de Escritorio remoto  
  
   Esto impedirá que los miembros del grupo DA inicio de sesión en estaciones de trabajo y servidores miembro. Si los servidores de salto se usan para administrar controladores de dominio y Active Directory, asegúrese de que los servidores de salto se encuentran en una unidad organizativa a la que no están vinculados los GPO restrictivos.  

3. La auditoría debe configurarse para enviar alertas si se realizan las modificaciones en las propiedades o la pertenencia al grupo de DA. Estas alertas se enviarán, como mínimo, a los usuarios o los equipos responsables de la respuesta de incidentes y administración de AD DS. También debe definir los procesos y procedimientos para rellenar el grupo DA, incluidos los procedimientos de notificación cuando se realiza un rellenado legítimo del grupo de temporalmente.  

#### <a name="securing-administrators-groups-in-active-directory"></a>Protección de grupos de administradores en Active Directory

Como sucede con los grupos de EA y DA, deben pertenecer al grupo de administradores (BA) solo en escenarios de compilación o recuperación ante desastres. No debe haber ninguna cuenta de usuario diarias en el grupo de administradores con la excepción de la cuenta de administrador local para el dominio, si se protegió como se describe en [apéndice D: Protección de cuentas de administrador integrada en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

Cuando se requiere acceso de los administradores, se deben colocar temporalmente las cuentas que necesitan este nivel de acceso en el grupo de administradores del dominio en cuestión. Aunque los usuarios utilizan las cuentas con privilegios elevados, actividades deberían auditar y, si es posible, se realiza con un usuario que realiza los cambios y observar los cambios de otro usuario para minimizar la posibilidad de uso incorrecto accidental o una configuración incorrecta. Cuando se han completado las actividades, las cuentas deben quita inmediatamente del grupo de administradores. Esto se consigue a través de procedimientos manuales y documenta los procesos a través de software management (PIM/PAM) de identidad y acceso con privilegios de terceros, o una combinación de ambos.  

Los administradores son, de forma predeterminada, los propietarios de la mayoría de los objetos de AD DS en sus dominios respectivos. Pertenencia a este grupo puede ser necesario en escenarios de recuperación ante desastres y la compilación en el que se requiere la propiedad o la capacidad de tomar posesión de objetos. Además, DAs y EAs heredan un número de sus derechos y permisos en virtud de su pertenencia predeterminado en el grupo Administradores. De manera predeterminada el anidamiento de grupos para grupos con privilegios en Active Directory no deben modificarse y como se describe en grupo de administradores de cada dominio que se debe proteger [apéndice G: Protección de grupos de administradores en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)y en las siguientes instrucciones generales.  

1. Quitar todos los miembros del grupo de administradores, con la excepción posible de la cuenta de administrador local para el dominio, siempre que se protegió como se describe en [apéndice D: Protección de cuentas de administrador integrada en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
2. Los miembros del grupo de administradores del dominio nunca deberían ser necesario iniciar sesión en servidores miembros o estaciones de trabajo. Uno o varios GPO vinculados a la estación de trabajo y cada servidor miembro unidades organizativas en cada dominio, debe agregar el grupo de administradores a los siguientes derechos de usuario:  
   - Denegar el acceso desde la red a este equipo  
   - Denegar inicio de sesión como un trabajo por lotes,  
   - Denegar el inicio de sesión como servicio  
   - Esto impide que los miembros del grupo de administradores que se usa para iniciar sesión o conectarse a servidores miembros o estaciones de trabajo (a menos que primero se traspasan varios controles), donde sus credenciales podrían almacenarse en caché y, por tanto, puesto en peligro. Nunca se debe usar una cuenta con privilegios para iniciar sesión en un sistema con menos privilegios, y aplicar estos controles ofrece protección frente a un número de ataques.  

3. En los controladores de dominio se debe conceder el usuario siguiente unidad organizativa en cada dominio del bosque, el grupo de administradores de derechos (si ya no tienen estos derechos), lo que permitirá a los miembros del grupo Administradores para realizar las funciones necesarias para un escenario de recuperación ante desastres de todo el bosque:  
   - Obtener acceso a este equipo desde la red  
   - Permitir el inicio de sesión local  
   - Permitir inicio de sesión a través de Servicios de Escritorio remoto  

4. La auditoría debe configurarse para enviar alertas si se realizan modificaciones en las propiedades o la pertenencia del grupo Administradores. Estas alertas se enviarán, como mínimo, a los miembros del equipo responsable de la administración de AD DS. También se deben enviar alertas a los miembros del equipo de seguridad y procedimientos deben definirse para modificar la pertenencia del grupo Administradores. En concreto, estos procesos deben incluir un procedimiento por el que se notifica al equipo de seguridad cuando el grupo de administradores se va a modificarse para que cuando se envían alertas, se espera y no se desencadena una alarma. Además, se deben implementar procesos para notificar al equipo de seguridad cuando se ha completado el uso del grupo de administradores y las cuentas que usa se han quitado del grupo.  

> [!NOTE]  
> Cuando implementa restricciones en el grupo de administradores en los GPO, Windows aplica la configuración a los miembros del grupo de administradores local del equipo además de grupo de administradores del dominio. Por lo tanto, se debe tener cuidado al implementar restricciones en el grupo de administradores. Aunque la prohibición de inicios de sesión por lotes y servicio de red para los miembros del grupo Administradores es, recomienda siempre que lo sea factible implementar, no restringe inicios de sesión locales o los inicios de sesión a través de servicios de escritorio remoto. El bloqueo de estos tipos de inicio de sesión puede impedir administración legítima de un equipo por los miembros del grupo Administradores local. Captura de pantalla siguiente muestra los valores de configuración que bloquean el uso indebido de integrados local y dominio de cuentas de administrador, además de uso indebido del grupo Administradores integrado del dominio o local. Tenga en cuenta que el **Denegar inicio de sesión a través de servicios de escritorio remoto** derecho de usuario no incluye el grupo de administradores, ya que incluirlo en esta configuración también bloquearía estos inicios de sesión para las cuentas que son miembros del equipo local Grupo de administradores. Si los servicios en los equipos están configurados para ejecutarse en el contexto de cualquiera de los grupos con privilegios descritos en esta sección, implementación de esta configuración puede provocar aplicaciones y servicios a un error. Por lo tanto, al igual que con todas las recomendaciones de esta sección, debe probar exhaustivamente la configuración para la aplicación en su entorno.  
>
> ![modelos de administración de privilegios mínimos](media/Implementing-Least-Privilege-Administrative-Models/SAD_3.gif)  

### <a name="role-based-access-controls-rbac-for-active-directory"></a>Controles de acceso basado en roles (RBAC) de Active Directory

Por lo general, los controles de acceso basado en roles (RBAC) son un mecanismo para agrupar usuarios y proporcionar acceso a los recursos según las reglas de negocios. En el caso de Active Directory, la implementación de RBAC para AD DS es el proceso de creación de roles a los que se delegan los derechos y permisos para permitir a los miembros del rol realizar tareas administrativas diarias sin concederles privilegios excesivos. RBAC para Active Directory se diseña y se implementa a través de las herramientas nativas y las interfaces, mediante el aprovechamiento de software, es posible que ya tienes, mediante la adquisición de productos de terceros, o cualquier combinación de estos enfoques. En esta sección no proporciona instrucciones paso a paso para implementar RBAC para Active Directory, pero en su lugar, se describe los factores que considere la posibilidad de elegir un enfoque para implementar RBAC en las instalaciones de AD DS.  

#### <a name="native-approaches-to-rbac-for-active-directory"></a>Enfoques nativos de RBAC para Active Directory

En la implementación más sencilla de RBAC, puede implementar roles como grupos de AD DS y delegar derechos y permisos a los grupos que les permiten realizar la administración diaria dentro del ámbito designado de la función.  

En algunos casos, los grupos de seguridad existentes en Active Directory se pueden usar para conceder derechos y permisos adecuados para una función de trabajo. Por ejemplo, si los empleados específicos de su organización de TI son responsables de la administración y mantenimiento de registros y zonas DNS, delegar las responsabilidades puede ser tan sencillo como crear una cuenta para cada administrador DNS y agregarlo al DNS Grupo de administradores en Active Directory. El grupo de administradores de DNS, a diferencia de los grupos con privilegios más elevados, tiene pocos derechos eficaces a través de Active Directory, aunque los miembros de este grupo han sido permisos delegados que les permiten administrar DNS y es estando sujetas a poner en peligro y el uso indebido podría producir elevación de privilegios.

En otros casos, es posible que deba crear grupos de seguridad y delegar derechos y permisos a objetos de Active Directory, los objetos del sistema de archivos y objetos del registro para permitir a los miembros de los grupos para realizar tareas administrativas designadas. Por ejemplo, si los operadores del departamento de soporte encargados de restablecimiento de contraseñas olvidadas, ayudando a los usuarios con problemas de conectividad y solución de problemas de configuración de la aplicación, deberá combinar la configuración de delegación en los objetos de usuario en Active Directory con privilegios que permiten a los usuarios del departamento de soporte técnico conectarse remotamente a los usuarios para ver o modificar la configuración de los usuarios. Para cada rol que definirá, debe identificar:  

1. Realizan los miembros de las tareas del rol en forma diaria y las tareas que se realizan con menos frecuencia.  
2. En qué sistemas y en las aplicaciones que los miembros de una función deben tener derechos y permisos.  
3. Los usuarios que deben concederse la pertenencia a un rol.  
4. ¿Cómo se realizará la administración de las pertenencias a roles.  

En muchos entornos, la creación manual de los controles de acceso basado en roles para la administración de un entorno de Active Directory puede resultar complicado para implementar y mantener. Si ha definido claramente roles y responsabilidades para la administración de su infraestructura de TI, desea aprovechar herramientas adicionales para facilitar la creación de una implementación nativa de RBAC fáciles de administrar. Por ejemplo, si se usa en su entorno de Forefront Identity Manager (FIM), puede usar FIM para automatizar la creación y el rellenado de los roles administrativos, que puede facilitar la administración continuada. Si usa System Center Configuration Manager (SCCM) y System Center Operations Manager (SCOM), puede usar roles específicos de la aplicación para delegar la administración y las funciones de supervisión y también aplicar una configuración coherente y la auditoría en todos los sistemas el dominio. Si ha implementado una infraestructura de clave pública (PKI), puede emitir y exigir tarjetas inteligentes para el personal de TI responsable de administrar el entorno. Con administración de credenciales de FIM (FIM CM), incluso puede combinar la administración de roles y las credenciales para el personal administrativo.  

En otros casos, puede ser preferible para una organización considere la posibilidad de implementar el software RBAC de terceros que proporciona la funcionalidad de "out-of-box". Comercial (COTS) soluciones ya preparadas para RBAC para Active Directory, Windows y los directorios que no son Windows y sistemas operativos se ofrecen por un número de proveedores. Al elegir entre las soluciones nativas y productos de terceros, debe considerar los factores siguientes:  

1. Presupuesto: Al invertir en el desarrollo de RBAC con software y herramientas que es posible que ya posee, puede reducir los costos de software necesarios para implementar una solución. Sin embargo, a menos que el personal con experiencia en crear e implementar soluciones RBAC nativas, es posible que deba interactuar con recursos de consultoría para desarrollar la solución. Debe sopesar detenidamente los costos anticipados de una solución personalizada con los costos para implementar una solución "out-of-box", especialmente si el presupuesto es limitado.  
2. Composición del entorno de TI: Si su entorno está formado principalmente los sistemas de Windows, o si ya están aprovechando Active Directory para la administración de cuentas y los sistemas que no sean Windows, las soluciones personalizadas nativas pueden proporcionar la mejor solución para sus necesidades. Si su infraestructura contiene muchos sistemas que no ejecutan Windows y no están administrados por Active Directory, deberá tener en cuenta las opciones para la administración de sistemas que no sean Windows por separado desde el entorno de Active Directory.  
3. Modelo de privilegio de la solución: Si un producto se basa en la selección de ubicación de sus cuentas de servicio en grupos con privilegios elevados en Active Directory y concede al software de RBAC no ofrecen opciones que no requieren privilegios excesivos, no ha reducido realmente el ataque de Active Directory superficie solo cambió la composición de los grupos con más privilegios en el directorio. A menos que un proveedor de la aplicación puede proporcionar controles para las cuentas de servicio que minimizan la probabilidad de que las cuentas que se está en peligro y usa de forma malintencionada, puede tener en cuenta otras opciones.  

### <a name="privileged-identity-management"></a>Privileged Identity Management

Privileged identity management (PIM), de a veces conocido como administración de cuentas con privilegios (PAM) o administración de credenciales con privilegios (PCM) es el diseño, construcción, y privilegios de implementación de metodologías para la administración de cuentas en su infraestructura. Por lo general, PIM proporciona mecanismos por el que las cuentas se les conceden derechos temporales y los permisos necesarios para realizar la compilación o salto corrección funciones, en lugar de dejar conectados permanentemente a las cuentas de privilegios. Si la funcionalidad PIM se crea manualmente o se implementa a través de la implementación de software de terceros una o varias de las siguientes características puede estar disponible:  
  
- "Almacenes", donde las contraseñas de cuentas con privilegios son "desproteger" y asigna una contraseña inicial y, después, "protegidas" cuando las actividades se han completado, momento en el que vuelva a restablecer las contraseñas en las cuentas de credenciales.  
- Restricciones de límite de tiempo en el uso de credenciales con privilegios  
- Credenciales de un uso  
- Generado por el flujo de trabajo de concesión de privilegio con supervisión e informes de las actividades realizadas y la eliminación automática de los privilegios cuando se completan o se asigna tiempo de actividades ha expirado  
- Reemplazo de credenciales codificadas de forma rígida, como los nombres de usuario y contraseñas en secuencias de comandos con interfaces de programación de aplicaciones (API) que permiten que las credenciales que deben recuperarse de los almacenes según sea necesario  
- Administración automática de las credenciales de cuenta de servicio  

### <a name="creating-unprivileged-accounts-to-manage-privileged-accounts"></a>Creación de cuentas sin privilegios para administrar las cuentas con privilegios

Uno de los desafíos en la administración de cuentas con privilegios es que, de forma predeterminada, las cuentas que puede administrar cuentas con privilegios y protegidas y grupos tienen privilegios y cuentas protegidas. Si decide implementar soluciones RBAC y PIM adecuadas para la instalación de Active Directory, las soluciones pueden incluir métodos que permiten vacíen eficazmente la pertenencia de los grupos con más privilegios en el directorio, rellenar los grupos solo temporalmente y cuando sea necesario.  

Sin embargo, si implementa nativo RBAC y PIM, debe considerar la creación de cuentas que no tienen ningún privilegio y con la única función de rellenar y vaciar con privilegios de grupos en Active Directory cuando sea necesario. [Apéndice I: Creación de cuentas de administración para las cuentas protegidas y grupos en Active Directory](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md) proporciona instrucciones paso a paso que puede usar para crear cuentas para este propósito.  

### <a name="implementing-robust-authentication-controls"></a>Implementación de controles de autenticación sólida

*Ley número seis: Realmente no hay alguien fuera que intenta adivinar las contraseñas.* - [10 leyes inmutables de la administración de seguridad](https://technet.microsoft.com/library/cc722488.aspx)  

PASS-the-hash y otros ataques de robo de credenciales no son específicas para sistemas operativos de Windows, ni son nuevos. El primer ataque pass-the-hash se creó en 1997. Históricamente, sin embargo, estos ataques requiere herramientas personalizadas, eran hit-or-miss en su éxito y requiere que los atacantes tienen un relativamente alto grado de aptitud. La introducción de las herramientas disponibles de forma gratuita, fácil de usar que extrae las credenciales de forma nativa dio como resultado un aumento exponencial del número y el éxito de los ataques de robo de credenciales en los últimos años. Sin embargo, los ataques de robo de credenciales en ningún caso son los mecanismos solo por el que las credenciales son dirigidas y puesto en peligro.  

Aunque debería implementar controles para ayudarle a protegerse contra ataques de robo de credenciales, debe también identificar las cuentas en su entorno que suelen ser objetivo de los atacantes e implementar controles de autenticación sólida para aquellos cuentas. Si las cuentas con privilegios más están usando la autenticación de factor único, como los nombres de usuario y contraseñas (ambos son "algo que sabe," que es un factor de autenticación), esas cuentas están protegidas débil. Todo lo que un atacante necesidades tiene conocimiento del nombre de usuario y conocer la contraseña asociada con la cuenta y ataques pass-the-hash no son necesarios el atacante puede autenticarse como el usuario para todos los sistemas que acepte credenciales de factor único.  

Aunque la implementación de la autenticación multifactor no le protege contra los ataques pass-the-hash, la implementación de la autenticación multifactor en combinación con los sistemas protegidos pueden. Se proporciona más información acerca de cómo implementar sistemas protegidos en [implementar Hosts administrativos seguros](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md), y se describen las opciones de autenticación en las secciones siguientes.  

#### <a name="general-authentication-controls"></a>Controles de autenticación general

Si ya no ha implementado la autenticación multifactor como tarjetas inteligentes, considere la posibilidad de hacerlo. Las tarjetas inteligentes implementan la protección aplicada por hardware de las claves privadas en un par de claves pública y privada, impidiendo la clave privada de un usuario de que tiene acceso o utilizar a menos que el usuario presenta el PIN adecuado, código de acceso o un identificador biométrico correspondiente a la tarjeta inteligente. Incluso si se intercepta el PIN o contraseña de un usuario un registrador de pulsaciones de teclas en un equipo en peligro, un atacante volver a usar el PIN o contraseña, la tarjeta también debe estar presente físicamente.

En los casos en que las contraseñas largas y complejas han demostrado ser difíciles de implementar debido a la resistencia del usuario, las tarjetas inteligentes ofrecen un mecanismo por el que los usuarios pueden implementar relativamente simple PIN o códigos de acceso sin las credenciales queden vulnerables fuerza bruta o ataques de tabla arco iris. Los PIN de tarjeta inteligente no se almacenan en Active Directory o en bases de datos SAM locales, aunque los hashes de credenciales pueden seguir almacenados en la memoria protegida de LSASS en los equipos en los que se han usado para la autenticación de tarjetas inteligentes.  

#### <a name="additional-controls-for-vip-accounts"></a>Controles adicionales para las cuentas de VIP

Otra ventaja de la implementación de tarjetas inteligentes u otros mecanismos de autenticación basada en certificados es la capacidad de aprovechar la comprobación del mecanismo de autenticación para proteger los datos confidenciales que son accesibles a usuarios VIP. Comprobación del mecanismo de autenticación está disponible en los dominios en el que se establece el nivel funcional a Windows Server 2012 o Windows Server 2008 R2. Cuando se habilita, la comprobación del mecanismo de autenticación agrega pertenencia a un administrador designado grupo global al token de Kerberos de un usuario cuando se autentican las credenciales del usuario durante el inicio de sesión mediante un método de inicio de sesión basada en certificados.  

Esto hace posible que los administradores de recursos controlar el acceso a recursos, como archivos, carpetas e impresoras, en función de si el usuario se inicia sesión con un método de inicio de sesión basada en certificados, además del tipo de certificado que se usa. Por ejemplo, cuando un usuario inicia sesión con una tarjeta inteligente, el acceso del usuario a los recursos de la red puede especificarse como diferentes de lo que el acceso es cuando el usuario no utiliza una tarjeta inteligente (es decir, cuando el usuario inicia sesión, escriba un nombre de usuario y contraseña). Para obtener más información acerca de la comprobación del mecanismo de autenticación, consulte el [comprobación del mecanismo de autenticación para AD DS en la Guía paso a paso de Windows Server 2008 R2](https://technet.microsoft.com/library/dd378897.aspx).  

#### <a name="configuring-privileged-account-authentication"></a>Configurar la autenticación de cuentas con privilegios

En Active Directory para todas las cuentas administrativas, habilitar la **necesita una tarjeta inteligente para el inicio de sesión interactivo** atributo y auditoría para que los cambios (como mínimo), cualquiera de los atributos en el **cuenta** pestaña la cuenta (por ejemplo, cn, nombre, sAMAccountName, userPrincipalName y userAccountControl) objetos de usuario administrativo.  

Aunque al establecer el **necesita una tarjeta inteligente para el inicio de sesión interactivo** en cuentas restablece la contraseña de la cuenta con un valor aleatorio de 120 caracteres y requiere que las tarjetas inteligentes para los inicios de sesión interactivo, el atributo se puede sobrescribir los usuarios con permisos que permiten cambiar las contraseñas de las cuentas y las cuentas, a continuación, pueden utilizarse para establecer los inicios de sesión no interactivo con solo el nombre de usuario y contraseña.  

En otros casos, según la configuración de cuentas en la configuración de Active Directory y el certificado en los servicios de certificados de Active Directory (AD CS) o una PKI de terceros, el nombre Principal de usuario (UPN) los atributos de administrativas o cuentas VIP pueden tener como destino para un tipo concreto de ataque, como se describe aquí.  

##### <a name="upn-hijacking-for-certificate-spoofing"></a>Secuestro de UPN para la suplantación de identidad de certificado

Aunque una discusión detallada de los ataques contra las infraestructuras de clave pública (PKI) está fuera del ámbito de este documento, los ataques contra las PKI públicos y privados han aumentado exponencialmente desde el año 2008. Las infracciones de PKI públicas han se ha anunciado ampliamente, pero quizás son incluso más prolíficos ataques de PKI interna de la organización. Un ataque de este tipo utiliza Active Directory y certificados para permitir que un atacante suplantar las credenciales de otras cuentas de manera que puede ser difícil de detectar.  

Cuando se presenta un certificado para la autenticación en un sistema unido al dominio, el contenido del asunto o el atributo de nombre alternativo de sujeto (SAN) en el certificado se usa para asignar el certificado a un objeto de usuario en Active Directory. Según el tipo de certificado y cómo se construye, el atributo sujeto de un certificado normalmente contiene un nombre de usuario común (CN), como se muestra en la captura de pantalla siguiente.  

![modelos de administración de privilegios mínimos](media/Implementing-Least-Privilege-Administrative-Models/SAD_4.gif)  

De forma predeterminada, Active Directory crea CN un usuario concatenando el nombre de la cuenta + "" + nombre de la última. Sin embargo, los componentes CN de objetos de usuario en Active Directory no se requiere o no se garantiza que sea único y mover una cuenta de usuario a una ubicación diferente en el directorio cambia el nombre distintivo (DN) de la cuenta, que es la ruta de acceso completa al objeto en el directorio, como se muestra en el panel inferior de la captura de pantalla anterior.  

Dado que los nombres de asunto del certificado no se garantiza que sea estática o único, el contenido del nombre alternativo del firmante a menudo se usa para buscar el objeto de usuario en Active Directory. El atributo de SAN para los certificados emitidos a los usuarios de entidades de certificación empresarial (CA integrado en Active Directory) contiene normalmente la dirección de correo electrónico o UPN del usuario. Dado que los UPN se garantiza que sean únicos en un bosque de AD DS, la ubicación de un objeto de usuario por UPN se realiza normalmente como parte de la autenticación, con o sin los certificados necesarios en el proceso de autenticación.  

El uso de UPN en atributos SAN de certificados de autenticación se puede aprovechar los atacantes obtener certificados fraudulentos. Si un atacante ha puesto en peligro una cuenta que tenga la capacidad de leer y escribir el UPN en objetos de usuario, el ataque se implementa como sigue:  

El atributo UPN de un objeto de usuario (por ejemplo, un usuario VIP) se cambia temporalmente a un valor diferente. El atributo de nombre de cuenta SAM y CN también pueden cambiarse en este momento, aunque esto no suele ser necesario por las razones descritas anteriormente.  

Cuando se ha cambiado el atributo UPN en la cuenta de destino, se cambia una cuenta de usuario habilitada, obsoletos o atributo UPN de la cuenta de usuario recién creado en el valor que se asignó originalmente a la cuenta de destino. Las cuentas de usuario habilitada, obsoletos son cuentas que no han iniciado sesión durante largos períodos de tiempo, pero no se han deshabilitado. Que se destinen a los atacantes que pretenden se "oculta a plena vista" de los siguientes motivos:  

1. Dado que la cuenta está habilitada, pero no se ha usado recientemente, con la cuenta no suele desencadenar alertas la manera en que puede habilitar una cuenta de usuario deshabilitado.  
2. Uso de una cuenta existente no requiere la creación de una nueva cuenta de usuario que es posible que pueda percibirlo personal administrativo.  
3. Cuentas de usuario obsoleta que todavía están habilitadas son normalmente miembros de distintos grupos de seguridad y se les concede acceso a los recursos de la red, lo que simplifica el acceso y "fusión" para una población de usuarios existente.  

La cuenta de usuario en el que ha configurado el UPN de destino se usa para solicitar certificados de uno o varios de los servicios de certificados de Active Directory.  

Cuando se hayan obtenido certificados para la cuenta del atacante, el UPN de la cuenta "nueva" y la cuenta de destino se devuelven a sus valores originales.  

Ahora, el atacante tiene uno o más certificados que se pueden presentar para la autenticación a los recursos y aplicaciones como si el usuario es el usuario VIP cuya cuenta se ha modificado temporalmente. Aunque una discusión completa de todas las formas en que los certificados y la PKI pueden tener como destino los atacantes está fuera del ámbito de este documento, este mecanismo de ataque se proporciona para ilustrar por qué se debe supervisar con privilegios y las cuentas de VIP en AD DS para cambios , especialmente para los cambios realizados en cualquiera de los atributos en el **cuenta** pestaña de la cuenta (por ejemplo, cn, nombre, sAMAccountName, userPrincipalName y userAccountControl). Además de supervisar las cuentas, debe restringir quién puede modificar las cuentas con como pequeños de un conjunto de usuarios administrativos como sea posible. Del mismo modo, las cuentas de usuarios administrativos deben ser protegidas y supervisar los cambios no autorizados.  
