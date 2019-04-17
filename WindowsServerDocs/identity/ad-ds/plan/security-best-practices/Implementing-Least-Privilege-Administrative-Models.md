---
ms.assetid: 7a7ab95c-9cb3-4a7b-985a-3fc08334cf4f
title: "Implementación de modelos de privilegios mínimos administrativos"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9716d442446b2705dfd2803d061cb884a5e72fbe
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="implementing-least-privilege-administrative-models"></a>Implementación de modelos de privilegios mínimos administrativos

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Es el siguiente fragmento de [el Administrador de cuentas de seguridad Guía de planificación](https://technet.microsoft.com/library/cc162797.aspx), se publicó por primera vez el 1 de abril de 1999:  
  
> "Más relacionadas con la seguridad cursos de formación y la documentación hablar sobre la implementación de un principio del menor privilegio, pero las organizaciones siguen rara vez. El principio es simple y el impacto de la aplicación de correctamente en gran medida aumenta la seguridad y reduce el riesgo. El principio de los Estados de que todos los usuarios deben iniciar sesión con una cuenta de usuario que tenga los permisos mínimos necesarios para completar la tarea actual y nada más. Si lo haces, proporciona protección contra código malintencionado, entre otros ataques. Este principio se aplica a equipos y los usuarios de esos equipos.   
> "Una razón que este principio funciona igual de bien es que te obliga a realizar una investigación interna. Por ejemplo, debes determinar los privilegios de acceso que un equipo o usuario realmente necesita y luego implementarlos. Para muchas organizaciones, esta tarea podría parecer inicialmente una gran cantidad de trabajo; Sin embargo, es un paso esencial para proteger correctamente el entorno de red.   
> "Debe conceder a todos los usuarios del Administrador de dominio sus privilegios de dominio en el concepto de privilegios mínimos. Por ejemplo, si un administrador inicia sesión con una cuenta con privilegios y accidentalmente ejecuta un programa antivirus, el virus tiene acceso administrativo al equipo local y a todo el dominio. Si el administrador hubiera iniciado la sesión con una cuenta sin privilegios (no administrativa), alcance de virus de los daños solo sería el equipo local porque se ejecuta como usuario de un equipo local.   
> "En otro ejemplo, cuentas a la que se otorgan derechos de administrador de dominio deben no tener derechos elevados de otro bosque, incluso si hay una relación de confianza entre los bosques. Esta táctica ayuda a evitar daños generalizados si un atacante poner en peligro un bosque administrado. Las organizaciones deben auditar periódicamente su red para protegerse contra no autorizado escalación de privilegios."  
  
Es el siguiente fragmento de la [Kit de recursos de seguridad de Microsoft Windows](https://www.microsoft.com/learning/en/us/book.aspx?ID=6815&locale=en-us), primera 2005 publicadas en:  
  
> "Siempre piensa de seguridad en cuanto a la concesión de la menor cantidad de privilegios necesarios para llevar a cabo la tarea. Si debe estar en peligro una aplicación que tiene demasiados privilegios, el atacante posible expandir el ataque más allá de lo que lo haría si se hubiera la aplicación en la menor cantidad de privilegios posibles. Por ejemplo, examina las consecuencias de un administrador de red sin darse cuenta abrir un archivo adjunto de correo electrónico que se inicia un virus. Si el administrador inicia sesión usando la cuenta de administrador de dominio, el virus tendrá privilegios de administrador en todos los equipos del dominio y, por consiguiente, el acceso ilimitado a casi todos los datos de la red. Si el administrador inicia sesión con una cuenta de administrador local, el virus tendrá privilegios de administrador en el equipo local y por lo tanto, sería capaz de acceder a los datos en el equipo e instalar software malintencionado, como software de trazo de clave de registro en el equipo. Si el administrador ha iniciado sesión con una cuenta de usuario normal, el virus tendrá acceso solo a los datos del administrador y no se puede instalar software malintencionado. Usando los privilegios mínimos necesarios para leer el correo electrónico, en este ejemplo, el alcance potencial del riesgo se reduce considerablemente."  
  
## <a name="the-privilege-problem"></a>El problema de privilegios  
Los principios que se describe en los fragmentos anteriores no han cambiado, pero en la evaluación de instalaciones de Active Directory, encontramos inexorablemente cantidades excesivas de las cuentas que se han concedido derechos y permisos más allá de los necesarios para realizar el trabajo diario. El tamaño del entorno afecta a los números sin procesar de cuentas con privilegios demasiado, pero no los directorios proportionmidsized pueden tener docenas de cuentas en los grupos con privilegios más elevados, mientras que pueden tener grandes instalaciones cientos o miles de incluso. Con algunas excepciones, independientemente de la sofisticación de habilidades y los arsenal, un atacante los atacantes suelen siguen la ruta de acceso de menos de resistencia. Solo si mecanismos errores o que son más sencillos frustra este tipo de Defender aumentan la complejidad de sus herramientas y el enfoque.  
  
Desafortunadamente, la ruta de acceso de resistencia menos en muchos entornos ha demostrado para ser el uso excesivo de cuentas con privilegios amplios y profundos. Privilegios amplia son derechos y permisos que permiten una cuenta para llevar a cabo actividades específicas en una sección grande de entorno, por ejemplo, personal del servicio de asistencia puede tener permisos que permitan restablecer las contraseñas en muchas cuentas de usuario.  
  
Privilegios profundos privilegios eficaces que se aplican a un segmento de la población estrecho, dicho lo que da un ingeniero de derechos de administrador en un servidor para que puedan realizar reparaciones. Privilegios amplia ni privilegios profunda están necesariamente peligroso, pero cuando muchas cuentas en el dominio se permanentemente privilegio muy extenso y, si solo una de las cuentas se ve comprometida, puede rápidamente usarse para volver a configurar el entorno para los fines del atacante o incluso destruir los segmentos de gran tamaño de la infraestructura.  
  
Los ataques PASS-the-hash, que son un tipo de ataques de robo de credenciales, son ubicuos debido a que las herramientas para realizarlas están libremente disponible y fácil de usar, y muchos entornos son vulnerables a los ataques. Ataques PASS-the-hash, sin embargo, no son el problema real. La piedra del problema es doble:  
  
1.  Normalmente es fácil para que un atacante obtenga privilegios profunda en un único equipo y, a continuación, propagar ese privilegio ampliamente en otros equipos.  
  
2.  Por lo general hay demasiadas permanente cuentas con privilegios elevados en el entorno informático.  
  
Incluso si se eliminan los ataques pass-the-hash, los atacantes simplemente usaría tácticas diferentes, no una estrategia diferente. En lugar de plantar malware que contiene las herramientas de robo de credenciales, podría Plante malware que registre las pulsaciones de teclas o aprovechar cualquier número de otros métodos para capturar las credenciales que son eficaces en el entorno. Independientemente de las acciones adecuadas, los destinos permanecen iguales: cuentas con privilegios amplios y profundos.  
  
Concesión de uso de privilegios excesivo no solo se encuentra en Active Directory en entornos en peligro. Cuando una organización ha desarrollado adquirir el hábito de concesión de privilegios más de lo necesario, normalmente se encuentra en toda la infraestructura como se explica en las siguientes secciones.  
  
## <a name="in-active-directory"></a>En Active Directory  
En Active Directory, es común que los grupos EA, DA y BA contienen un número excesivo de cuentas de encontrar. Normalmente, el grupo EA de una organización contiene a los miembros de menor, DA grupos normalmente contienen un multiplicador del número de usuarios del grupo EA y grupos de administradores normalmente contienen a más miembros que las poblaciones de los grupos de combinar. Esto suele ser debido a la creencia de que los administradores de alguna manera son "menos privilegios" que DAs o EAs. Mientras los derechos y permisos concedidos a cada uno de estos grupos varían, que deberían eficazmente considerarse igualmente eficaces grupos como parte de una puede convertirse en un miembro de los otros dos.  
  
## <a name="on-member-servers"></a>En los servidores miembro  
Cuando se recupera la pertenencia a grupos de administradores locales en los servidores miembro en muchos entornos, encontramos pertenencia comprendido entre las plumas de las cuentas locales y de dominio, y docenas de grupos anidados que, cuando se expande, mostrar cientos, incluso miles de cuentas con privilegios de administrador local en los servidores. En muchos casos, los grupos de dominio con grandes pertenencias están anidados en grupos de administradores locales los servidores miembro, sin tener en consideración el hecho de que cualquier usuario que se puede modificar la pertenencia a grupos en el dominio puede tener control administrativo de todos los sistemas en el que estaba anidado un grupo de administradores local en el grupo.  
  
## <a name="on-workstations"></a>En estaciones de trabajo  
Aunque las estaciones de trabajo normalmente tienen muchos menos miembros en el grupo de administradores locales que tienen los servidores miembro, en muchos entornos, se conceden a los usuarios la pertenencia a grupo de administradores locales en sus equipos personales. Cuando esto ocurre, incluso si UAC está habilitado, los usuarios presentan un riesgo para la integridad de las estaciones de trabajo con privilegios elevados.  
  
> [!IMPORTANT]  
> Debe considerar detenidamente si los usuarios requieren derechos administrativos en sus estaciones de trabajo y, si es así, puede ser un mejor enfoque para crear una cuenta local independiente en el equipo que sea miembro del grupo de administradores. Cuando los usuarios requieren elevación, pueden presentar las credenciales de esa cuenta local de elevación, pero debido a la cuenta es local, no puede usarse para poner en peligro otros equipos o tener acceso a recursos de dominio. Al igual que con cualquier cuentas locales, sin embargo, las credenciales de la cuenta con privilegios local deben ser únicas; Si creas una cuenta local con las mismas credenciales en varias estaciones de trabajo, exponen los equipos a los ataques pass-the-hash.  
  
## <a name="in-applications"></a>En las aplicaciones  
En los ataques en el que el destino es la propiedad intelectual de la organización, las cuentas que se han concedido privilegios eficaces en las aplicaciones pueden destinarse para permitir exfiltration de datos. Aunque las cuentas que tienen acceso a datos confidenciales no concedidas privilegios elevados en el dominio o el sistema operativo, las cuentas que se pueden manipular la configuración de una aplicación o el acceso a la información de la aplicación proporciona riesgo presente.  
  
## <a name="in-data-repositories"></a>En los repositorios de datos  
Como sucede con otros destinos, solicitando acceso a la propiedad intelectual en forma de documentos y otros archivos de los atacantes pueden dirigirse a las cuentas que controlar el acceso al archivo almacena las cuentas que tienen acceso directo a los archivos, o incluso grupos o funciones que tienen acceso a los archivos. Por ejemplo, si se usa un servidor de archivos para almacenar documentos del contrato y conceder el acceso a los documentos mediante el uso de un grupo de Active Directory, un atacante puede modificar la pertenencia del grupo puede agregar cuentas al grupo y acceso a los documentos del contrato. En casos en que las aplicaciones como SharePoint proporciona acceso a documentos, los atacantes pueden identificar las aplicaciones, como se describió anteriormente.  
  
## <a name="reducing-privilege"></a>Reducción de privilegios  
El mayor y más complejo un entorno más difícil es para administrar y proteger. Las organizaciones pequeñas, revisa y reducir los privilegios pueden ser una propuesta lo suficientemente relativamente sencillo, pero cada servidor adicionales, la estación de trabajo, la cuenta de usuario y la aplicación en uso en una organización agregan otro objeto que se debe proteger. Porque puede resultar difícil o incluso imposible correctamente seguro todos los aspectos de una organización de la infraestructura de TI, debes centrarte esfuerzos primero en las cuentas cuyos privilegios crear el mayor riesgo, que normalmente son integrado privilegiado cuentas y grupos en Active Directory, y con privilegios cuentas locales en estaciones de trabajo y los servidores miembro.  
  
### <a name="securing-local-administrator-accounts-on-workstations-and-member-servers"></a>Seguridad de las cuentas de administrador Local en los servidores miembro y estaciones de trabajo  
Aunque este documento se centra en la seguridad de Active Directory, tal como se ha explicado anteriormente, la mayoría de ataques contra el directorio comienzan como ataques contra los hosts individuales. No se proporcionan directrices completas para proteger los grupos locales en los sistemas de miembro, pero las siguientes recomendaciones pueden usarse para ayudarle a proteger las cuentas de administrador locales en estaciones de trabajo y los servidores miembro.  
  
#### <a name="securing-local-administrator-accounts"></a>Seguridad de las cuentas de administrador Local  
En todas las versiones de Windows actualmente en el soporte estándar, la cuenta de administrador local está deshabilitada de manera predeterminada, lo que hace que la cuenta que no se puede utilizar para pass-the-hash y otros ataques de robo de credenciales. Sin embargo, en los dominios que contienen los sistemas operativos heredados o qué cuentas de administrador locales se han habilitado, estas cuentas pueden usarse como se describió anteriormente para propagar compromiso en estaciones de trabajo y los servidores miembro. Por este motivo, se recomiendan los siguientes controles para todas las cuentas de administrador locales en sistemas unidos a un dominio.  
  
Se proporcionan instrucciones detalladas para implementar estos controles en [Apéndice H: proteger cuentas de administrador Local y grupos](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md). Antes de implementar estas opciones de configuración, sin embargo, asegúrate de que las cuentas de administrador locales no usan actualmente en el entorno para ejecutar servicios en equipos o realizar otras actividades que no deben usarse estas cuentas. Prueba estas configuraciones completamente antes de implementarlos en un entorno de producción.  
  
#### <a name="controls-for-local-administrator-accounts"></a>Controles para cuentas de administrador Local  
Cuentas de administrador predefinida nunca deben usarse como cuentas de servicio en servidores miembro, así como tampoco debe se usa para iniciar sesión en los equipos locales (excepto en el modo seguro, lo que se permite incluso si la cuenta está deshabilitada). El objetivo de la implementación de la configuración que se describen aquí es evitar que la cuenta de administrador local de cada equipo se pueden usar a menos que primero se invierten los controles de protección. Al implementar estos controles y supervisión de cambios de las cuentas de administrador, se pueden reducir significativamente la probabilidad de éxito de un ataque que las cuentas de administrador local de destinos.  
  
##### <a name="configuring-gpos-to-restrict-administrator-accounts-on-domain-joined-systems"></a>Configuración de GPO para restringir las cuentas de administrador en sistemas unidos a un dominio  
En uno o más GPO que crean y vincular a la estación de trabajo y cada servidor miembro unidades organizativas en cada dominio, agrega la cuenta de administrador a los siguientes derechos de usuario en **asignaciones de derechos de seguridad\asignación de seguridad\Directivas de equipo equipo\Directivas\Configuración Windows\Configuración**:  
  
-   Denegar el acceso a este equipo desde la red  
  
-   Denegar inicio de sesión como trabajo por lotes  
  
-   Denegar inicio de sesión como servicio  
  
-   Denegar inicio de sesión a través de servicios de escritorio remoto  
  
Al agregar cuentas de administrador a estos derechos de usuario, especificar si vas a agregar la cuenta de administrador local o el administrador del dominio de la cuenta a la forma etiquetar la cuenta. Por ejemplo agregar la cuenta de administrador del dominio NWTRADERS estos denegar derechos de propiedad, debes escribir la cuenta como **NWTRADERS\Administrador**, o ir a la cuenta de administrador para el dominio NWTRADERS. Para garantizar que restringir la cuenta de administrador local, escribe **administrador** en estas opciones en el Editor de objetos de directiva de grupo de derechos de usuario.  
  
> [!NOTE]  
> Incluso si se cambia el nombre de cuentas de administrador locales, las directivas se seguirá aplicando.  
  
Estas opciones de configuración se garantizarán que cuenta de administrador de un equipo no puede usarse para conectarse a los otros equipos, incluso si está accidentalmente o de manera malintencionada habilitado. Los inicios de sesión local con la cuenta de administrador local no se puede deshabilitar completamente, ni tampoco debe intentar hacer esto, dado cuenta de administrador local de un equipo está diseñado para usarse en escenarios de recuperación ante desastres.  
  
Debe un servidor miembro o estación de trabajo se convierten en separado del dominio sin privilegios administrativos sentado de otras cuentas locales, el equipo puede arrancar en modo seguro, se puede habilitar la cuenta de administrador y la cuenta se pueden usar para realizar reparaciones en el equipo. Cuando se completen las reparaciones, debe deshabilitarse vuelve a la cuenta de administrador.  
  
### <a name="securing-local-privileged-accounts-and-groups-in-active-directory"></a>Proteger las cuentas locales con privilegios y los grupos de Active Directory  
*Ley número seis: un equipo solo es tan seguro como el administrador es de confianza.* - [Diez inmutables de seguridad (versión 2.0)](https://technet.microsoft.com/security/hh278941.aspx)  
  
La información provista aquí se pretende dar directrices generales para proteger las cuentas integradas de privilegios más altas y los grupos de Active Directory. También se proporcionan instrucciones detalladas en [apéndice D: proteger predefinida de administrador cuentas en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md), [Apéndice E: proteger Enterprise administradores grupos en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md), [Apéndice F: proteger administradores grupos de dominio de Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)y en [apéndice G: proteger los administradores de grupos de Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md).  
  
Antes de implementar cualquiera de estas opciones de configuración, también debes probar todas las configuraciones exhaustiva para determinar si son adecuados para el entorno. No todas las organizaciones podrán implementar estas opciones de configuración.  
  
#### <a name="securing-built-in-administrator-accounts-in-active-directory"></a>Seguridad de las cuentas de administrador integrada en Active Directory  
En cada dominio de Active Directory, se crea una cuenta de administrador como parte de la creación del dominio. Esta cuenta es de manera predeterminada, un miembro de los grupos de administrador en el dominio y administradores de dominio y, si el dominio es el dominio raíz, la cuenta también es un miembro del grupo Administradores de empresa. Uso de la cuenta de administrador local de un dominio debe reservarse solo para las actividades de la compilación inicial y, posiblemente, los escenarios de recuperación ante desastres. Para garantizar que la cuenta predefinida de administrador puede usarse para realizar reparaciones en caso de que ninguna otra cuenta puede usarse, no debe cambiar la pertenencia predeterminada de la cuenta de administrador en cualquier dominio del bosque. En su lugar, debe seguir directrices para ayudar a proteger la cuenta Administrador en cada dominio del bosque. Se proporcionan instrucciones detalladas para implementar estos controles en [apéndice D: proteger predefinida de administrador cuentas en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
#### <a name="controls-for-built-in-administrator-accounts"></a>Controles para las cuentas de administrador predefinida  
El objetivo de la implementación de la configuración que se describen aquí es evitar que la cuenta de administrador de cada dominio (no a un grupo) se pueden usar a menos que se invierte una serie de controles. Al implementar estos controles y supervisión de cambios de las cuentas de administrador, puede reducir significativamente la probabilidad de que un ataque con éxito aprovechando la cuenta de administrador de un dominio. Para que la cuenta de administrador en cada dominio del bosque, debes configurar la siguiente configuración.  
  
##### <a name="enable-the-account-is-sensitive-and-cannot-be-delegated-flag-on-the-account"></a>Habilitar la marca "Cuenta es confidencial y no se puede delegar" en la cuenta  
De manera predeterminada, pueden delegarse todas las cuentas en Active Directory. La delegación permite a un equipo o el servicio para presentar las credenciales de cuenta que se ha autenticado al equipo o servicio en otros equipos para obtener servicios en nombre de la cuenta. Al habilitar la **cuenta es confidencial y no se puede delegar** atributo en una cuenta de dominio, las credenciales de cuenta no se pueden aparecer en otros equipos o servicios en la red, lo que limita los ataques que aprovechan la delegación para usar las credenciales de cuenta en otros sistemas.  
  
##### <a name="enable-the-smart-card-is-required-for-interactive-logon-flag-on-the-account"></a>Habilita el indicador de "tarjeta inteligente es necesaria para el inicio de sesión interactivo" en la cuenta  
Al habilitar la **tarjeta inteligente es necesaria para el inicio de sesión interactivo** atributo en una cuenta, Windows restablece la contraseña de la cuenta en un valor de 120 caracteres aleatorio. Al establecer esta marca en las cuentas de administrador integradas, asegúrate de que la contraseña de la cuenta es no solo largas y complejas, pero no se conoce a ningún usuario. No es necesario técnicamente crear tarjetas inteligentes para las cuentas antes de habilitar este atributo, pero si es posible, se debe crear tarjetas inteligentes para cada cuenta de administrador antes de configurar las restricciones de cuenta y las tarjetas inteligentes deben almacenarse en ubicaciones seguras.  
  
Aunque establecer la **tarjeta inteligente es necesaria para el inicio de sesión interactivo** marca restablece la contraseña de la cuenta, no impide que un usuario con derechos para restablecer la contraseña de la cuenta desde la configuración de la cuenta en un valor conocido y con nombre y la nueva contraseña para acceder a los recursos de la cuenta en la red. Por este motivo, debes implementar los siguientes controles adicionales en la cuenta.  
  
##### <a name="disable-the-account"></a>Deshabilitar la cuenta  
Si la cuenta de administrador ya no está deshabilitada, deshabilitarlo cuando se ha completado la configuración de propiedades de la cuenta. Esto impide que la cuenta que se usarán con ningún fin a menos que primero está habilitado. En un escenario de recuperación ante desastres en los que no está disponible para realizar reparaciones del entorno de AD DS ninguna cuenta, puedes arrancar un controlador de dominio en el modo seguro, inicio de sesión local con la cuenta predefinida de administrador (que nunca se bloquea de inicio de sesión local) y habilitar la cuenta de administrador de dominio, si es necesario.  
  
##### <a name="configuring-gpos-to-restrict-domains-administrator-accounts-on-domain-joined-systems"></a>Configuración de GPO para restringir las cuentas de administrador de dominios en los sistemas unidos a un dominio  
Aunque deshabilitar la cuenta de administrador en un dominio hace que la cuenta de manera eficaz inutilizable, debería implementar restricciones adicionales de la cuenta en caso de que se habilita la cuenta sin darse cuenta o de forma malintencionada. Aunque estos controles en última instancia se pueden deshacer la cuenta de administrador, el objetivo es crear controles que ralentizan el progreso de un atacante y puede producir límite el daño que la cuenta.  
  
En uno o más GPO que crean y vincular a la estación de trabajo y cada servidor miembro unidades organizativas en cada dominio, agrega la cuenta de administrador de cada dominio a los siguientes derechos de usuario en **asignaciones de derechos de seguridad\asignación de seguridad\Directivas de equipo equipo\Directivas\Configuración Windows\Configuración**:  
  
-   Denegar el acceso a este equipo desde la red  
  
-   Denegar inicio de sesión como trabajo por lotes  
  
-   Denegar inicio de sesión como servicio  
  
-   Denegar inicio de sesión a través de servicios de escritorio remoto  
  
> [!NOTE]  
> Al agregar cuentas de administrador locales a esta configuración, debes especificar si va a configurar cuentas de administrador locales o cuentas de administrador de dominio. Por ejemplo agregar la cuenta de administrador local del dominio NWTRADERS estos denegar derechos de propiedad, o bien debes escribir la cuenta como **NWTRADERS\Administrador**, o ir a la cuenta de administrador local para el dominio NWTRADERS. Si escribes **administrador** en estas opciones de derechos de usuario en el Editor de objetos de directiva de grupo, restringirá la cuenta de administrador local en cada equipo al que se aplica el GPO.  
>   
> Se recomienda limitar las cuentas de administrador locales en servidores miembro y estaciones de trabajo en la misma manera que las cuentas de administrador de dominio. Por lo tanto, por lo general, debes agregar la cuenta de administrador para cada dominio en el bosque y la cuenta de administrador para los equipos locales para estas opciones de configuración de derechos de usuario. Captura de pantalla siguiente muestra un ejemplo de configurar estos derechos de usuario para bloquear las cuentas de administrador locales y cuenta de administrador de un dominio de la realización de inicios de sesión que no deberían ser necesario para estas cuentas.  

>   
> ![modelos de menor privilegio Administrador](media/Implementing-Least-Privilege-Administrative-Models/SAD_20.gif)  
  
##### <a name="configuring-gpos-to-restrict-administrator-accounts-on-domain-controllers"></a>Configuración de GPO para restringir las cuentas de administrador en controladores de dominio  
En cada dominio del bosque, se debe modificar la directiva de controladores de dominio predeterminada o una directiva vinculada a la unidad organizativa de controladores de dominio para agregar la cuenta de administrador de cada dominio a los siguientes derechos de usuario en **asignaciones de derechos de seguridad\asignación de seguridad\Directivas de equipo equipo\Directivas\Configuración Windows\Configuración**:  
  
-   Denegar el acceso a este equipo desde la red  
  
-   Denegar inicio de sesión como trabajo por lotes  
  
-   Denegar inicio de sesión como servicio  
  
-   Denegar inicio de sesión a través de servicios de escritorio remoto  
  
> [!NOTE]  
> Estas opciones de configuración se garantizarán que la cuenta de administrador local no puede usarse para conectarse a un controlador de dominio, aunque la cuenta, si se habilita, puede iniciar sesión localmente en controladores de dominio. Dado que esta cuenta solo debe habilitada y usarse en escenarios de recuperación ante desastres, se prevé que esté disponible el acceso físico al menos un controlador de dominio o que se pueden utilizar otras cuentas con permisos de acceso a controladores de dominio de forma remota.  
  
##### <a name="configure-auditing-of-built-in-administrator-accounts"></a>Configurar la auditoría de cuentas de administrador predefinida  
Cuando se protege la cuenta de administrador de cada dominio y se deshabilitado, debe configurar la auditoría para supervisar los cambios en la cuenta. Si se habilita la cuenta, se restablece la contraseña o cualquier otra modificación se realiza en la cuenta, se deben enviar alertas a los usuarios o equipos responsables de administración de AD DS, además de los equipos de respuesta a incidentes de la organización.  
  
### <a name="securing-administrators-domain-admins-and-enterprise-admins-groups"></a>Los administradores de seguridad, administradores de dominio y grupos de administradores de empresa  
  
#### <a name="securing-enterprise-admin-groups"></a>Protección de los grupos de administradores de empresa  
El grupo de administradores de empresa, que se almacena en el dominio raíz, no debe contener ningún usuario diariamente, con la posible excepción de la cuenta de administrador local del dominio, siempre está protegido como se describió anteriormente y en [apéndice D: proteger predefinida de administrador cuentas en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
Cuando se necesita EA access, los usuarios cuyas cuentas requieren EA derechos y permisos deben colocarse temporalmente en el grupo de administradores de empresa. Aunque los usuarios están utilizando las cuentas con privilegios elevados, sus actividades deben auditarse y realiza preferiblemente con un usuario realizar los cambios y otro usuario observar los cambios para reducir la probabilidad de uso inadecuado involuntaria o de configuración. Cuando se han completado las actividades, las cuentas se deben quitar del grupo de EA. Esto se consigue mediante procedimientos manuales y documentado procesos, software de administración (PIM/PAM) de identidad y acceso con privilegios de terceros o una combinación de ambas. Directrices para crear cuentas que puede usarse para controlar la pertenencia a grupos con privilegios en Active Directory se proporcionan en [cuentas atractiva para el robo de credenciales](../../../ad-ds/plan/security-best-practices/Attractive-Accounts-for-Credential-Theft.md) y se proporcionan instrucciones detalladas en [apéndice I: creación de administración de cuentas para las cuentas protegidas y los grupos de Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
Administradores de empresa son, de manera predeterminada, los miembros del grupo integrado Administradores en cada dominio en el bosque. Quitar el grupo de administradores de organización de los grupos de administradores en cada dominio es una modificación inapropiada porque en caso de un escenario de recuperación ante desastres bosque, derechos EA probablemente sea necesarios. Si se ha quitado el grupo de administradores de empresa de grupos de administradores en un bosque, se debe agregar al grupo Administradores en cada dominio y deben implementarse los siguientes controles adicionales:  
  
-   Como se describió anteriormente, el grupo de administradores de empresa no debe contener ningún usuario diariamente, con la posible excepción de la cuenta de administrador de dominio raíz del bosque, que se debe proteger como se describe en [apéndice D: proteger predefinida de administrador cuentas en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
-   En los GPO vinculados a las unidades organizativas que contienen los servidores miembro y estaciones de trabajo en cada dominio, el grupo EA debe agregarse a los siguientes derechos de usuario:  
  
    -   Denegar el acceso a este equipo desde la red  
  
    -   Denegar inicio de sesión como trabajo por lotes  
  
    -   Denegar inicio de sesión como servicio  
  
    -   Denegar el inicio de sesión localmente  
  
    -   Denegar el inicio de sesión a través de servicios de escritorio remoto.  
  
Esto evitará que los miembros del grupo EA iniciar sesión en estaciones de trabajo y los servidores miembro. Si los servidores de accesos directos se usan para administrar controladores de dominio y Active Directory, asegúrate de que los servidores de accesos directos se encuentran en una unidad organizativa a la que no están vinculados los GPO restrictivos.  
  
-   Auditoría debe configurarse para enviar alertas si se realizan todas las modificaciones en las propiedades o la pertenencia al grupo EA. Estas alertas deben enviarse, como mínimo, a usuarios o equipos responsables de respuesta de administración y los accidentes de Active Directory. También debes definir procesos y procedimientos para rellenar temporalmente el grupo EA, incluidos los procedimientos de notificación cuando se realiza la población legítimo del grupo.  
  
#### <a name="securing-domain-admins-groups"></a>Proteger los grupos de administradores de dominio  
Como sucede con el grupo de administradores de empresa, la pertenencia a grupos de administradores de dominio deben únicamente en escenarios de recuperación ante desastres o de compilación. No debería haber ninguna cuenta de usuario diarias en el grupo DA a excepción de la cuenta de administrador local para el dominio, si ha protegido como se describe en [apéndice D: proteger predefinida de administrador cuentas en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
Cuando se necesita DA acceso, deben colocarse temporalmente las cuentas que necesitan este nivel de acceso en el grupo DA para el dominio en cuestión. Aunque los usuarios utilizan las cuentas con privilegios elevados, actividades deben auditarse y realiza preferiblemente con un usuario realizar los cambios y otro usuario observar los cambios para reducir la probabilidad de uso inadecuado involuntaria o de configuración. Cuando se han completado las actividades, las cuentas se deben quitar del grupo Administradores de dominio. Esto puede lograrse mediante procedimientos manuales y documentado procesos, mediante el software de administración (PIM/PAM) de identidad y acceso con privilegios de terceros, o una combinación de ambas. Directrices para crear cuentas que puede usarse para controlar la pertenencia a grupos con privilegios en Active Directory se proporcionan en [apéndice I: creación de administración de cuentas para las cuentas protegidas y los grupos de Active Directory](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
Administradores de dominio son, de manera predeterminada, los miembros del grupo de administradores locales en todos los servidores miembro y estaciones de trabajo en sus respectivos dominios. No se debe modificar este anidamiento predeterminado porque afecta a las opciones de recuperación ante desastres y compatibilidad. Si se han quitado los grupos Administradores de dominio de los grupos de administradores locales en los servidores miembro, se debe agregar al grupo Administradores en cada servidor miembro y la estación de trabajo en el dominio a través de configuración de grupo restringido en los GPO vinculados. Los siguientes controles generales que se describen detalladamente en [Apéndice F: proteger administradores grupos de dominio de Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md) también deben implementarse.  
  
Para el grupo de administradores de dominio en cada dominio del bosque:  
  
1.  Quitar todos los miembros del grupo DA, con la posible excepción de la cuenta predefinida de administrador para el dominio, siempre ha sido protegido como se describe en [apéndice D: proteger predefinida de administrador cuentas en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
2.  En los GPO vinculados a las unidades organizativas que contienen los servidores miembro y estaciones de trabajo en cada dominio, debe agregarse el grupo DA a los siguientes derechos de usuario:  
  
    -   Denegar el acceso a este equipo desde la red  
  
    -   Denegar inicio de sesión como trabajo por lotes  
  
    -   Denegar inicio de sesión como servicio  
  
    -   Denegar el inicio de sesión localmente  
  
    -   Denegar inicio de sesión a través de servicios de escritorio remoto  
  
    Esto evitará que los miembros del grupo DA iniciar sesión en estaciones de trabajo y los servidores miembro. Si los servidores de accesos directos se usan para administrar controladores de dominio y Active Directory, asegúrate de que los servidores de accesos directos se encuentran en una unidad organizativa a la que no están vinculados los GPO restrictivos.  
  
3.  Auditoría debe configurarse para enviar alertas si se realizan todas las modificaciones en las propiedades o la pertenencia al grupo DA. Estas alertas deben enviarse, como mínimo, a usuarios o equipos responsables de respuesta de administración y los accidentes de AD DS. También debes definir procesos y procedimientos para rellenar temporalmente el grupo DA, incluidos los procedimientos de notificación cuando se realiza la población legítimo del grupo.  
  
#### <a name="securing-administrators-groups-in-active-directory"></a>Proteger el grupo de administradores en Active Directory  
Como sucede con los grupos de atributo extendido y DA, únicamente en escenarios de compilación o recuperación ante desastres deben pertenencia al grupo de administradores (BA). No debería haber ninguna cuenta de usuario diarias en el grupo de administradores a excepción de la cuenta de administrador local para el dominio, si ha protegido como se describe en [apéndice D: proteger predefinida de administrador cuentas en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
Cuando se necesita acceso a los administradores, las cuentas que necesitan este nivel de acceso deben colocarse temporalmente en el grupo de administradores del dominio en cuestión. Aunque los usuarios utilizan las cuentas con privilegios elevados, actividades deben auditarse y, preferiblemente, lleva a cabo con un usuario realiza los cambios y otro usuario observar los cambios para reducir la probabilidad de uso inadecuado involuntaria o de configuración. Cuando se han completado las actividades, deben quitarse inmediatamente las cuentas del grupo Administradores. Esto puede lograrse mediante procedimientos manuales y documentado procesos, mediante el software de administración (PIM/PAM) de identidad y acceso con privilegios de terceros, o una combinación de ambas.  
  
Los administradores son, de manera predeterminada, los propietarios de la mayoría de los objetos de AD DS en sus respectivos dominios. Pertenencia a este grupo puede ser necesaria en escenarios de recuperación ante desastres y compilación en el que se requiere la propiedad o la capacidad de tomar posesión de objetos. Además, DAs y EAs heredan un número de sus derechos y permisos en virtud de su pertenencia predeterminado en el grupo de administradores. De manera predeterminada el anidamiento de grupos para que no se deben modificar grupos con privilegios en Active Directory y grupo de administradores de cada dominio se debe proteger como se describe en [apéndice G: proteger los administradores de grupos de Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)y en las siguientes instrucciones generales.  
  
1.  Quitar todos los miembros del grupo de administradores, con la posible excepción de la cuenta de administrador local para el dominio, siempre ha sido protegido como se describe en [apéndice D: proteger predefinida de administrador cuentas en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
2.  Los miembros del grupo de administradores del dominio nunca tendrían que iniciar sesión en los servidores miembro o estaciones de trabajo. En uno o más GPO vinculados a la estación de trabajo y cada servidor miembro unidades organizativas en cada dominio, el grupo de administradores debe agregarse a los siguientes derechos de usuario:  
  
    -   Denegar el acceso a este equipo desde la red  
  
    -   Denegar inicio de sesión como trabajo por lotes,  
  
    -   Denegar inicio de sesión como servicio  
  
    -   Esto evitará que los miembros del grupo Administradores se usa para iniciar sesión o conectarse a los servidores miembro o estaciones de trabajo (a menos que primero se traspasan varios controles), donde sus credenciales podrían se almacenan en caché y lo que se ve comprometidas. Una cuenta con privilegios nunca debe usarse para iniciar sesión en un sistema con privilegios menos y cumplimiento de estos controles ofrece protección contra un número de ataques.  
  
3.  En los controladores de dominio, unidad organizativa en cada dominio en el bosque, del grupo Administradores debe concederse el siguiente usuario derechos (si ya no tienen estos derechos), lo que te permitirá a los miembros del grupo de administradores para realizar funciones necesarias para un escenario de recuperación ante desastres de todo el bosque:  
  
    -   Acceso a este equipo desde la red  
  
    -   Permitir el registro local  
  
    -   Permitir el inicio de sesión en a través de servicios de escritorio remoto  
  
4.  Auditoría debe configurarse para enviar alertas si se realizan todas las modificaciones en las propiedades o la pertenencia al grupo de administradores. Estas alertas deben enviarse, como mínimo, a los miembros del equipo responsable de administración de AD DS. También se deben enviar alertas a los miembros del equipo de seguridad y procedimientos que deben definirse para modificar la pertenencia del grupo de administradores. En concreto, estos procesos deben incluir un procedimiento por el que el equipo de seguridad se notifica cuando el grupo de administradores se va a modificarse para que cuando se envían alertas, se esperan y no se genera una alarma. Además, se deben implementar procesos para notificar al grupo de seguridad cuando se ha completado el uso del grupo de administradores y se han quitado las cuentas que se usa en el grupo.  
  
> [!NOTE]  
> Cuando se implementan restricciones en el grupo de administradores en los GPO, Windows aplica la configuración a los miembros del grupo de administradores local de un equipo además de grupo de administradores del dominio. Por lo tanto, debes tener cuidado al implementar restricciones en el grupo de administradores. Aunque prohibir los inicios de sesión de red, el lote y el servicio para los miembros del grupo Administradores es recomendable donde es factible implementar, no restringir los inicios de sesión local o los inicios de sesión a través de servicios de escritorio remoto. Bloqueo de estos tipos de inicio de sesión, puede impedir legítima administración de un equipo por miembros del grupo de administradores local. Captura de pantalla siguiente muestra las opciones de configuración que bloquear el uso incorrecto de integrados locales y cuentas Administrador de dominio, además de uso inadecuado de los grupos de administradores de dominio o local integrados. Ten en cuenta que la **Denegar inicio de sesión a través de servicios de escritorio remoto** derecho de usuario no incluye el grupo de administradores, porque se incluye en esta configuración también bloquearía estos inicios de sesión para las cuentas que son miembros del grupo Administradores del equipo local. Si los servicios en los equipos se configuran para ejecutarse en el contexto de cualquiera de los grupos con privilegios, que se describe en esta sección, implementar estas opciones de configuración puede ocasionar servicios y aplicaciones a un error. Por lo tanto, al igual que con todas las recomendaciones indicadas en esta sección, debe probar la configuración de aplicación en su entorno.  

>   
> ![modelos de menor privilegio Administrador](media/Implementing-Least-Privilege-Administrative-Models/SAD_3.gif)  
  
### <a name="role-based-access-controls-rbac-for-active-directory"></a>Controles de acceso basadas en roles (RBAC) de Active Directory  
Por lo general, los controles de acceso basadas en roles (RBAC) son un mecanismo de agrupación de los usuarios y proporcionar acceso a recursos en función de las reglas empresariales. En el caso de Active Directory, la implementación RBAC de AD DS es el proceso de creación de funciones a la que se deleguen derechos y permisos para permitir que los miembros de la función realizar tareas administrativas diarias sin concesión de uso de privilegios excesivo. Se diseña e implementan a través de herramientas nativa e interfaces, aprovechando el software que puede ya tienes, al comprar productos de terceros, o cualquier combinación de estos enfoques RBAC para Active Directory. En esta sección no se proporciona instrucciones paso a paso para implementar RBAC para Active Directory, pero en su lugar, se describe los factores que debes tener en cuenta al elegir un enfoque para implementar RBAC en las instalaciones de AD DS.  
  
#### <a name="native-approaches-to-rbac-for-active-directory"></a>Enfoques nativos RBAC para Active Directory  
En la implementación de RBAC más simple, puedes implementar funciones como grupos de AD DS y delegar derechos y permisos a los grupos que les permiten realizar administración diaria dentro del ámbito designado de la función.  
  
En algunos casos, los grupos de seguridad existente de Active Directory pueden usarse para conceder derechos y permisos correspondientes a una función de trabajo. Por ejemplo, si los empleados específicos de la organización de TI están responsables de la administración y mantenimiento de zonas y registros DNS, delegar las responsabilidades puede ser tan simple como crear una cuenta para cada administrador DNS y agregarlo al grupo de administradores de DNS en Active Directory. El grupo de administradores de DNS, a diferencia de grupos con privilegios más elevados, tiene algunos derechos eficaces a través de Active Directory, aunque miembros de este grupo han sido delegados los permisos que permiten administrar DNS.  
  
En otros casos, debes crear grupos de seguridad y el delegado derechos y permisos en objetos de Active Directory, los objetos de sistema de archivos y los objetos del registro para permitir que los miembros de los grupos para realizar tareas administrativas designadas. Por ejemplo, si los operadores de servicio de asistencia son responsables de restablecimiento de contraseñas olvidadas, ayudan a los usuarios con problemas de conectividad y solución de problemas de configuración de la aplicación, necesitarás combinar la configuración de la delegación en objetos de usuario de Active Directory con privilegios que permiten a los usuarios del servicio de asistencia conectarse de forma remota a los usuarios para ver o modificar las opciones de configuración de los usuarios. Para cada rol que defines, debes identificar:  
  
1.  Los miembros de las tareas de la función realizan en un día a día y las tareas que se realizan con menos frecuencia.  
  
2.  En los sistemas y en qué aplicaciones miembros de una función se deben conceder derechos y permisos.  
  
3.  Los usuarios que deben concederse pertenencia a una función.  
  
4.  ¿Cómo se realizará la administración de suscripciones de rol.  
  
En muchos entornos, crear manualmente los controles de acceso basada en rol para la administración de un entorno de Active Directory puede resultar desafiante para implementar y mantener. Si has definido claramente funciones y las responsabilidades para la administración de la infraestructura de TI, quieres aprovechar herramientas adicionales para que le ayudarán a crear una implementación de RBAC nativa manejable. Por ejemplo, si Forefront Identity Manager (FIM) está en uso en el entorno, puedes usar FIM para automatizar la creación y la población de funciones administrativas, que pueden facilitar la administración continua. Si usas System Center Configuration Manager (SCCM) y System Center Operations Manager (SCOM), puedes usar roles específicos de la aplicación al delegar la administración y supervisión de funciones y, también aplicar una configuración coherente y auditoría en sistemas en el dominio. Si se ha implementado una infraestructura de clave pública (PKI), puede emitir y requerir tarjetas inteligentes para el personal de TI responsable de administrar el entorno. Con administración de credenciales de FIM (FIM CM), puedes combinar incluso administración de roles y las credenciales para su personal administrativo.  
  
En otros casos, es posible preferible para una organización que considerar la distribución de software RBAC de terceros que proporciona la funcionalidad de "out-of-box". Se ofrecen soluciones comerciales comerciales (comerciales) para RBAC para Active Directory, Windows y no sean Windows directorios y sistemas operativos por una serie de proveedores. Al elegir entre las soluciones nativas y productos de terceros, debe tener en cuenta los siguientes factores:  
  
1.  Presupuesto: Invirtiendo en desarrollo de RBAC con software y herramientas que puede que ya tienes, puedes reducir los costos de software relacionados con la implementación de una solución. Sin embargo, a menos que tengas personal con experiencia en la creación e implementación de soluciones RBAC nativas, necesitarás atraer a los recursos de consulta para desarrollar su solución. Se deben considerar detenidamente los costos anticipados para una solución personalizadas con los costos para implementar una solución "out-of-box", especialmente si el presupuesto es limitado.  
  
2.  Composición del entorno de TI: si tu entorno está compuesto principalmente por sistemas Windows, o si ya están aprovechando Active Directory para la administración de cuentas y no sean Windows sistemas, nativas soluciones personalizadas de pueden proporcionar la mejor solución para tus necesidades. Si tu infraestructura contiene muchos sistemas que no se están ejecutando Windows y no se administran mediante Active Directory, deberás tener en cuenta opciones para la administración de sistemas no son de Windows por separado desde el entorno de Active Directory.  
  
3.  Modelo de privilegios en la solución: si un producto se basa en la colocación de sus cuentas de servicio en grupos con privilegios elevados en Active Directory y pueden no opciones de oferta que no requieren privilegios excesivo se concede al software RBAC, no se reduce realmente Active Directory ataque surfaceyou solo cambió la composición de los grupos más privilegiadas en el directorio. A menos que un proveedor de la aplicación puede proporcionar controles de las cuentas de servicio que reducir la probabilidad de que las cuentas que se ve comprometida y usa de forma malintencionada, es aconsejable considerar otras opciones.  

  
### <a name="privileged-identity-management"></a>Administración de identidades con privilegios  
Privilegiadas (PIM) de administración de identidades, a veces conocidos para como cuenta con privilegios de administración (PAM) o administración de credenciales con privilegios (PCM) es el diseño, construcción, y privilegiadas de implementación de métodos para administrar las cuentas en tu infraestructura. Por lo general, PIM proporciona mecanismos que cuentas se otorgan derechos temporales y los permisos necesarios para realizar la compilación o salto corrección funciones, en lugar de salir de privilegios están conectados permanentemente a las cuentas. Si la funcionalidad PIM se crea de forma manual o se implementa mediante la implementación de software de terceros uno o más de las siguientes características puede estar disponible:  
  

-   "Almacenes", donde las contraseñas de cuentas con privilegios son "desprotegidas" y le asigna una contraseña inicial, "protegidas" cuando actividades que se completaron, en cuyo momento vuelve a restablecer las contraseñas de las cuentas de credenciales.  
  
-   Controlado por tiempo restricciones sobre el uso de credenciales con privilegios  
  
-   Credenciales de un uso  
  
-   Generado por el flujo de trabajo de concesión de privilegio con supervisión e informes de actividades realizadas y eliminación automática de privilegios cuando actividades se completó o asignadas tiempo ha caducado  
  
-   Reemplazo de credenciales codificadas de forma rígida, como nombres de usuario y contraseñas en scripts con interfaces de programación de aplicaciones (API) que permiten que las credenciales que deben recuperarse desde depósitos según sea necesario  
  
-   Administración automática de las credenciales de cuenta de servicio  
  
### <a name="creating-unprivileged-accounts-to-manage-privileged-accounts"></a>Creación de cuentas sin privilegios para administrar las cuentas con privilegios  
Uno de los retos en la administración de cuentas con privilegios es que, de manera predeterminada, las cuentas que pueden administrar cuentas con privilegios y protegidas y tienen privilegios grupos y cuentas de protección. Si implementas soluciones RBAC y PIM adecuadas para la instalación de Active Directory, las soluciones pueden incluir métodos que permiten vacíen eficazmente la pertenencia de los grupos más privilegiadas en el directorio, rellenar los grupos solo temporalmente y cuando sea necesarios.  
  
Sin embargo, si implementas RBAC y PIM nativas, deberías considerar crear cuentas que no tienen privilegios y con la función de rellenar y vaciar privilegios única agrupa en Active Directory cuando sea necesario. [Apéndice I: administración crear cuentas para las cuentas protegidas y los grupos de Active Directory](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md) proporciona instrucciones paso a paso que puedes usar para crear cuentas para este propósito.  
  
### <a name="implementing-robust-authentication-controls"></a>Implementación de controles de autenticación sólida  
*Ley número seis: realmente hay personas fuera que intenta adivinar las contraseñas.* - [10 inmutables de administración de seguridad](https://technet.microsoft.com/library/cc722488.aspx)  
  
PASS-the-hash y otros ataques de robo de credenciales no son específicos de los sistemas operativos Windows, ni son nuevos. El primer ataque pass-the-hash se creó en 1997. Históricamente, sin embargo, estos ataques requiere herramientas personalizadas, estaban hit-or-miss en su éxito y necesita los atacantes tienen un relativamente alto grado de conocimientos. La introducción de las herramientas disponibles de forma gratuita y fácil de usar que extrae las credenciales de forma nativa ha provocado un aumento exponencial en el número y el éxito de ataques de robo de credenciales en los últimos años. Sin embargo, los ataques de robo de credenciales no son los mecanismos solo por el cual las credenciales de destino y puesto en peligro.  
  
Aunque debes implementar controles para ayudar a protegerte contra ataques de robo de credenciales, también debe identificar las cuentas en tu entorno que están más probable que el objetivo de los atacantes e implementar controles de autenticación sólida de esas cuentas. Si las cuentas con privilegios más a usar la autenticación de factor único, como nombres de usuario y contraseñas (ambos son "algo que sabes," que es un factor de autenticación), estas cuentas están protegidas flexible. Todo lo que el atacante necesita es conocimientos del nombre de usuario y conocimientos de la contraseña asociada con la cuenta y ataques pass-the-hash no son requiredthe atacante pueda autenticar como el usuario para todos los sistemas que aceptar las credenciales de factor único.  
  
Aunque la implementación de la autenticación multifactor no protege contra los ataques pass-the-hash, implementar la autenticación multifactor en combinación con sistemas protegidos puede. Para obtener más información acerca de cómo implementar sistemas protegidos se proporciona en [implementar Hosts administrativas seguro](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md), y opciones de autenticación se describen en las siguientes secciones.  
  
#### <a name="general-authentication-controls"></a>Controles de autenticación generales  
Si ya no hayan implementado la autenticación multifactor como tarjetas inteligentes, considera la posibilidad de hacerlo. Las tarjetas inteligentes implementar hardware aplicada la protección de claves privadas en un par de claves pública y privada, evitar que la clave privada de un usuario de que tiene acceso o usar a menos que el usuario presenta el PIN correcto, la contraseña o el identificador biométrico para la tarjeta inteligente. Incluso si un registrador de pulsaciones de teclas en un equipo en peligro, para volver a usar el PIN o contraseña, un atacante intercepta PIN o contraseña de un usuario también debe estar físicamente presente la tarjeta.  
  
En casos en que las contraseñas largas y complejas han demostrado ser difíciles de implementar debido a la resistencia del usuario, las tarjetas inteligentes proporcionan un mecanismo por el que los usuarios pueden implementar PIN relativamente simples o contraseñas sin las credenciales que se está susceptibles de sufrir ataques de fuerza bruta o los ataques de tabla arco iris. PIN de tarjeta inteligente no se almacenan en Active Directory o en bases de datos de SAM locales, aunque los valores de hash de credenciales aún pueden almacenarse en la memoria LSASS protegidos en los equipos en los que se usaron tarjetas inteligentes para la autenticación.  
  
#### <a name="additional-controls-for-vip-accounts"></a>Controles adicionales para cuentas VIP  
Otra ventaja de la implementación de tarjetas inteligentes u otros mecanismos de autenticación basada en certificados es la capacidad de aprovechar el mecanismo de autenticación para proteger datos confidenciales que son accesibles para usuarios muy importantes. Mecanismo de autenticación está disponible en dominios en el que se establece el nivel funcional en Windows Server 2012 o Windows Server 2008 R2. Cuando está habilitado, mecanismo de autenticación agrega pertenencia a un grupo global administrador designado al token de Kerberos de un usuario cuando se autentican las credenciales del usuario durante el inicio de sesión con un método de inicio de sesión basado en certificados.  
  
Esto hace posible para los administradores de recursos controlar el acceso a recursos, como archivos, carpetas, impresoras, en función de si el usuario inicia sobre el uso de un método de inicio de sesión basado en certificados, además del tipo de certificado que se usa. Por ejemplo, cuando un usuario inicia sesión con una tarjeta inteligente, el acceso del usuario a los recursos de la red se puede especificar como diferentes desde el acceso de qué es cuando el usuario no usa una tarjeta inteligente (es decir, cuando el usuario inicie sesión escribiendo un nombre de usuario y contraseña). Para obtener más información sobre el mecanismo de autenticación, consulta el [mecanismo de autenticación de AD DS en la Guía paso a paso de Windows Server 2008 R2](https://technet.microsoft.com/library/dd378897.aspx).  
  
#### <a name="configuring-privileged-account-authentication"></a>Configurar la autenticación de cuenta con privilegios  
En Active Directory para todas las cuentas administrativas, habilitar la **requerir tarjeta inteligente para el inicio de sesión interactivo** atributo y auditoría para cambios (como mínimo), cualquiera de los atributos en el **cuenta** pestaña de la cuenta (por ejemplo, cn, nombre, sAMAccountName, userPrincipalName y userAccountControl) objetos de usuario administrativos.  
  
Aunque establecer la **requerir tarjeta inteligente para el inicio de sesión interactivo** en cuentas restablece la contraseña de la cuenta en un valor de 120 caracteres aleatorio y requiere las tarjetas inteligentes para inicios de sesión interactivo, el atributo todavía se puede sobrescribir con los usuarios con permisos que permiten que se cambien las contraseñas de las cuentas y las cuentas se pueden usar para establecer los inicios de sesión no interactiva con solo el nombre de usuario y contraseña.  
  
En otros casos, según la configuración de cuentas en la configuración de Active Directory y certificados en los servicios de certificados de Active Directory (AD CS) o una PKI de terceros, el nombre Principal de usuario (UPN) atributos para administrativas o cuentas VIP pueden ser el objetivo de un determinado tipo de ataque, como se describe aquí.  
  
##### <a name="upn-hijacking-for-certificate-spoofing"></a>UPN secuestro de suplantación de certificado  
Aunque una explicación detallada de ataques contra infraestructuras de clave públicas (PKI) está fuera del ámbito de este documento, los ataques contra PKI públicas y privadas han aumentado exponencialmente desde 2008. Han sido hace público infracciones de PKI públicas ampliamente, pero ataques contra PKI interna de la organización quizás son aún más habituales. Un ataque de dicho aprovecha Active Directory y certificados para permitir que un atacante suplante las credenciales de otras cuentas de manera que pueden ser difíciles de detectar.  
  
Cuando se presenta un certificado para la autenticación a un sistema Unidos a un dominio, el contenido del asunto o el atributo de asunto alternativa nombre (SAN) en el certificado se usa para asignar el certificado a un objeto de usuario en Active Directory. Según el tipo de certificado y cómo se ha creado, el atributo de asunto en un certificado por lo general contiene un nombre de usuario común (CN), como se muestra en la siguiente captura de pantalla.  
  
![modelos de menor privilegio Administrador](media/Implementing-Least-Privilege-Administrative-Models/SAD_4.gif)  
  
De manera predeterminada, Active Directory construye concatenando el nombre de la cuenta CN de un usuario + "" + apellido. Sin embargo, los componentes CN de objetos de usuario en Active Directory no se necesiten o garantiza que sean únicos y mover una cuenta de usuario a una ubicación diferente en el directorio cambia el nombre completo de la cuenta (DN), que es la ruta de acceso completa al objeto en el directorio, como se muestra en el panel inferior de la captura de pantalla anterior.  
  
Dado que no se garantiza que los nombres de firmantes de certificado ser estáticos o único, el contenido del nombre del sujeto alternativo se suelen usar para localizar el objeto de usuario en Active Directory. El atributo de SAN para certificados emitidos a los usuarios de entidades de certificación (CA integrado en Active Directory) por lo general contiene la dirección del usuario UPN o el correo electrónico. Porque UPN se garantiza que sean únicos en un bosque de AD DS, buscar un objeto de usuario por UPN generalmente, se realiza como parte de la autenticación, con o sin certificados que participan en el proceso de autenticación.  
  
El uso de UPN en atributos de SAN certificados de autenticación se puede aprovechar los atacantes para obtener certificados fraudulentos. Si un atacante haya puesto en peligro una cuenta que tiene la capacidad de leer y escribir el UPN en objetos de usuario, el ataque se implementa como sigue:  
  
El atributo UPN en un objeto de usuario (por ejemplo, un usuario VIP) temporalmente se cambia a un valor diferente. El atributo de nombre de cuenta SAM y el CN también pueden cambiarse en este momento, aunque esto no es necesario por los motivos que se ha descrito anteriormente.  
  
Cuando se ha cambiado el atributo UPN de la cuenta de destino, cambia una cuenta de usuario obsoletos, habilitado o atributo UPN de la cuenta de usuario recién creado en el valor que originalmente se asignó a la cuenta de destino. Las cuentas de usuario obsoletos y habilitado son que no han iniciado sesión durante largos períodos de tiempo, pero no se han deshabilitado. Están dirigidos los atacantes que intenten "ocultar a plena vista" de los siguientes motivos:  
  
1.  Porque la cuenta está habilitada, pero no se ha usado recientemente, con la cuenta de la forma poco probable para disparar las alertas que puede habilitar una cuenta de usuario deshabilitado.  
  
2.  Uso de una cuenta existente no requiere la creación de una nueva cuenta de usuario que puede que notado por personal administrativo.  
  
3.  Cuentas de usuario obsoletos que aún están habilitadas por lo general son miembros de grupos de seguridad y se conceden acceso a los recursos de la red, a simplificar el acceso y "fusión" a una población de usuario existente.  
  
La cuenta de usuario en el que ha configurado el UPN de destino se usa para solicitar uno o más certificados de servicios de certificados de Active Directory.  
  
Cuando se hayan obtenido certificados para la cuenta del atacante, el UPN de la cuenta "nueva" y el destino se devuelven a sus valores originales.  
  
Ahora, el atacante tiene uno o más certificados que se pueden representar para la autenticación a los recursos y aplicaciones como si el usuario es el usuario VIP cuya cuenta se modificó temporalmente. Aunque una descripción completa de todas las formas en que los certificados y PKI pueden ser el objetivo los atacantes está fuera del ámbito de este documento, este mecanismo de ataque se proporciona para ilustrar por qué se debe supervisar con privilegios y cuentas VIP en AD DS para cambios, especialmente para los cambios en cualquiera de los atributos de la **cuenta** pestaña de la cuenta (por ejemplo cn, nombre, sAMAccountName, userPrincipalName y userAccountControl). Además de supervisar las cuentas, debe restringir quién puede modificar las cuentas que se como pequeño un conjunto de usuarios administrativos como sea posible. De igual modo, las cuentas de usuarios administrativos deben protegidas y se supervisan los cambios no autorizados.  
  


