---
title: Proteger el Material de referencia con privilegios de acceso
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 22ee9a77-4872-4c54-82d9-98fc73a378c0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: ee5769a3ed0225ebdd31645027bace38f0bff1b9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="securing-privileged-access-reference-material"></a>Proteger el Material de referencia con privilegios de acceso

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


Esta sección contiene información de referencia para proteger el acceso a privilegios incluidos:

-   [Modelo de Active Directory niveles administrativos](#ADATM_BM)

-   [Principio de origen limpio](#CSP_BM)

-   [Enfoque de diseño de bosque administrativas ESAE](#ESAE_BM)

-   [Equivalencia de nivel 0](#T0E_BM)

-   [Herramientas administrativas y tipos de inicio de sesión](#ATLT_BM)

## <a name="ADATM_BM"></a>Modelo de Active Directory niveles administrativos
El propósito de este modelo de nivel es proteger los sistemas de identidad con un conjunto de zonas de búfer entre pleno control del entorno (nivel 0) y los activos de la estación de trabajo de alto riesgo que los atacantes con frecuencia poner en riesgo.

![Diagrama que muestra los tres niveles del modelo de niveles](../media/securing-privileged-access-reference-material/PAW_RM_Fig1.JPG)

El modelo de niveles se compone de tres niveles e incluye solo las cuentas administrativas, cuentas de usuario no estándar:

-   **Nivel 0** -Control directo de identidades de empresa en el entorno. Nivel 0 incluye cuentas, grupos y otros activos que tienen el control administrativo directo o indirecto de bosque de Active Directory, dominios o controladores de dominio y todos los activos en él. La sensibilidad de seguridad de todos los activos de nivel 0 es equivalente como todas de forma efectiva en un control de entre sí.

-   **Nivel 1** -Control de aplicaciones y los servidores de la empresa. Activos de nivel 1 que incluyen sistemas operativos de servidor, servicios en la nube y las aplicaciones de empresa. Cuentas de administrador 1 de capa tienen control administrativo de un período de valor empresarial que está hospedado en estos activos. Una función de ejemplo común es que los administradores de servidor que mantienen estos sistemas operativos con la capacidad de afectar a todos los servicios de empresa.

-   **Nivel 2** -Control de las estaciones de trabajo del usuario y dispositivos. Cuentas de administrador 2 de nivel tienen control administrativo de un período de valor empresarial que está hospedado en estaciones de trabajo del usuario y dispositivos. Algunos ejemplos incluyen el servicio de asistencia y el equipo admita los administradores porque puede afectar a la integridad de casi cualquier dato de usuario.

> [!NOTE]
> Los niveles también sirven como un mecanismo de prioridad básico para proteger los activos administrativos, pero es importante tener en cuenta que un atacante con el control de todos los activos de cualquier nivel puede acceder a la mayoría o todos los activos de negocio. El motivo es útil como un mecanismo de priorización básica es atacante dificultad y costo. Es más fácil para que un atacante para que funcione con el control total de todas las identidades (nivel 0) o servidores y servicios en la nube (nivel 1) que es si debe acceder a cada estación de trabajo individual o el dispositivo del usuario (nivel 2) para obtener datos de la organización.

### <a name="containment-and-security-zones"></a>Zonas de seguridad y la contención
Los niveles son relativos a una zona de seguridad específicos. Mientras que han pasado por muchos nombres, las zonas de seguridad son un enfoque consolidado que proporcionan la contención de amenazas de seguridad a través de aislamiento de la capa de red entre ellos. El modelo de niveles complementa el aislamiento proporcionando la contención de adversarios dentro de una zona de seguridad que el aislamiento de red no es eficaz. Zonas de seguridad pueden abarcar tanto local y en la nube infraestructura, como en el ejemplo se donde los controladores de dominio y los miembros del dominio en el mismo dominio local hospedado y en Azure.

![Diagrama que muestra cómo se pueden abarcar tanto local y en la nube infraestructura zonas de seguridad](../media/securing-privileged-access-reference-material/PAW_RM_Fig2.JPG)

El modelo de niveles impide que escalación de privilegios al restringir los administradores pueden controlar y dónde se puede iniciar sesión (como iniciar sesión en un equipo concede el control de las credenciales y todos los activos administrados por esas credenciales).

### <a name="control-restrictions"></a>Restricciones de control
Restricciones del control se muestran en la siguiente ilustración:

![Diagrama de restricciones de Control](../media/securing-privileged-access-reference-material/PAW_RM_Fig3.JPG)

### <a name="primary-responsibilities-and-critical-restrictions"></a>Principales responsabilidades en cuanto y restricciones críticas
**Administrador de nivel 0** -administrar el almacén de identidades y un pequeño número de sistemas que están en un control eficaz, y:

-   Administrar y controlar activos en cualquier nivel según sea necesario

-   Solo se inicie sesión interactivamente o acceder a recursos de confianza en el nivel de nivel 0

**Administrador de nivel 1** -administrar los servidores de empresa, servicios y aplicaciones, y:

-   Solo se pueden administrar y controlar activos en el nivel de nivel 1 o 2 de nivel

-   Puede solo acceso a activos (a través de tipo de inicio de sesión de red) que son de confianza en los niveles de nivel 0 o de nivel 1

-   Solo interactivamente puede iniciar sesión en recursos de confianza en el nivel de nivel 1

**Administrador de nivel 2** -administrar escritorios de empresa, portátiles, impresoras y otros dispositivos del usuario, y:

-   Solo se pueden administrar y controlar activos en el nivel de nivel 2

-   Puede obtener acceso a activos (a través de tipo de inicio de sesión de red) en cualquier nivel según sea necesario

-   Solo interactivamente puede iniciar sesión en recursos de confianza en el nivel de nivel 2

### <a name="logon-restrictions"></a>Restricciones de inicio de sesión
Restricciones de inicio de sesión se muestran en la siguiente ilustración:

![Diagrama de restricciones de inicio de sesión](../media/securing-privileged-access-reference-material/PAW_RM_Fig4.JPG)

> [!NOTE]
> Ten en cuenta que algunos activos pueden tener un impacto de nivel 0 en la disponibilidad del entorno, pero no directamente impacto la confidencialidad o la integridad de los activos. Estos incluyen el servicio de servidor DNS y dispositivos de red críticos como servidores proxy de Internet.

## <a name="CSP_BM"></a>Principio de origen limpio
El principio de origen limpio requiere que todas las dependencias de seguridad de confianza como como el objeto que se protege.

![Diagrama que muestra cómo un tema en el control de un objeto es una dependencia de seguridad de ese objeto](../media/securing-privileged-access-reference-material/PAW_RM_Fig5.JPG)

Cualquier asunto de control de un objeto es una dependencia de seguridad de ese objeto. Si un adversario puede controlar nada en un control eficaz de un objeto de destino, pueden controlar ese objeto de destino. Por este motivo, debes asegurarte de que las garantías para todas las dependencias de seguridad son igual o superior el nivel de seguridad del objeto.

Mientras simple en principio, aplicar esto es necesario comprender las relaciones de control de un activo de interés (objeto) y realizar un análisis de dependencia de descubrir todas las dependencias de seguridad (Subject(s)).

Como control es transitivo, este principio se tiene que estar recursivamente repetido. Para los controles de ejemplo, si un controles B y B C, a continuación, un también controla indirectamente C.

![Diagrama que muestra cómo si un controles B y B controles C, a continuación, un también controla indirectamente C](../media/securing-privileged-access-reference-material/PAW_RM_Fig6.JPG)

Un atacante que pone en peligro un obtiene acceso a todo un controles (como por ejemplo B) y todo lo B controles (como C). Con el idioma de las dependencias de seguridad en este mismo ejemplo, B y A son dependencias de seguridad de C y tengan que estar protegidas en el nivel de garantía deseado de C en orden para C tener ese nivel de garantía.

Para los sistemas de identidad y la infraestructura de TI, este principio debe aplicarse en la forma más habitual de incluidos el hardware donde se instalan los sistemas de control, los medios de instalación para los sistemas, la arquitectura y configuración del sistema y las operaciones diarias.

### <a name="clean-source-for-installation-media"></a>Limpiar el origen de medios de instalación
![Diagrama que muestra un origen medios de instalación limpio](../media/securing-privileged-access-reference-material/PAW_RM_CSIM.JPG)

Aplicación del principio de origen limpio para medios de instalación requiere que garantizar que los medios de instalación no se ha alterado desde que se lanza por el fabricante (lo mejor es capaz de determinar). Esta figura muestra un atacante que use esta ruta de acceso en peligro un equipo:

![Ilustración que muestra un atacante que use una ruta de acceso en peligro un equipo](../media/securing-privileged-access-reference-material/PAW_RM_Fig8.JPG)

Aplicación del principio de origen limpio para medios de instalación requiere validar la integridad de software durante el ciclo de poseen incluidos durante la adquisición, almacenamiento y transferir hacia arriba hasta que se usa.

#### <a name="software-acquisition"></a>Adquisición de software
El origen del software debe validarse mediante una de las siguientes maneras:

-   Software se obtiene desde medios físicos que se sabe que provienen del fabricante o una fuente de confianza, normalmente fabricado multimedia se envió desde un proveedor.

-   Software es obtenido a partir de Internet y validado con hash de archivo suministradas por el proveedor.

-   Software es obtenido a partir de Internet y validar mediante la descarga y comparar dos copias independientes:

    -   Descargar en dos hosts sin relación de seguridad (no en el mismo dominio y no administrados por las mismas herramientas), preferiblemente de conexiones de Internet independientes.

    -   Compara los archivos descargados con una herramienta como certutil:

        `certutil -hashfile <filename>`

Cuando sea posible, todos los software de aplicación, como los instaladores de aplicaciones y herramientas deben estar firmados digitalmente y comprueba mediante Authenticode de Windows con la [Windows Sysinternal](https://www.microsoft.com/sysinternals)herramienta s, *sigcheck.exe*, con la comprobación de revocación. Algunos programas pueden requerir que el proveedor no puede proporcionar este tipo de firma digital.

#### <a name="software-storage-and-transfer"></a>Software almacenamiento y transferencia de
Después de obtener el software, deben almacenarse en una ubicación que protección frente a modificaciones, especialmente al host conectado a internet o personal en un nivel inferior que los sistemas que se instalará el sistema operativo o el software de confianza. Este almacenamiento puede ser medios físicos o una ubicación segura electrónica.

#### <a name="software-usage"></a>Uso de software
En teoría, el software debe validarse en el momento en que se usa, como se manualmente instalado, empaquetarla para una herramienta de administración de configuración o importado en una herramienta de administración de configuración.

### <a name="clean-source-for-architecture-and-design"></a>Origen limpio para la arquitectura y el diseño
Aplicación del principio de origen limpio para la arquitectura del sistema debe asegurarse de que el sistema no es dependiente en sistemas de menor confianza. Un sistema puede ser dependiente en un sistema de confianza más alto, pero no en un sistema de confianza inferior con los estándares de seguridad más bajos.

Por ejemplo, su aceptable en Active Directory para controlar un equipo de escritorio de usuario estándar, pero es una importante escalación de riesgo de privilegios para un equipo de escritorio de usuario estándar en el control de Active Directory.

![Diagrama que muestra cómo un sistema puede ser dependiente en un sistema mayor confianza, pero no en un sistema de confianza inferior con los estándares de seguridad inferior](../media/securing-privileged-access-reference-material/PAW_RM_Fig09.JPG)

Se pueden introducir la relación de control a través de muchos medios incluidos seguridad listas de Control de acceso (ACL) para objetos como sistemas de archivos, pertenencia al grupo de administradores locales en un equipo o agentes instalados en un equipo que se ejecutan como sistema (con la capacidad de ejecutar scripts y código arbitrario).

Un ejemplo con frecuencia se pasa por alto es exponer a través de inicio de sesión, que crea una relación de control de exponer las credenciales administrativas de un sistema a otro sistema. Esto es la razón subyacente por qué los ataques, como pasar el hash de robo de credenciales son tan eficaz. Cuando un administrador inicia sesión un equipo de escritorio de usuario estándar con credenciales de nivel 0, expone esas credenciales en ese escritorio, colocarlo en un control de anuncios y la creación de una extensión de ruta de acceso de privilegios a AD. Para obtener más información sobre estos ataques, consulte [esta página](https://technet.microsoft.com/en-us/security/dn785092).

Debido al gran número de activos que dependen de los sistemas de identidad como Active Directory, te recomendamos que reduzcas el número de sistemas que dependen de Active Directory y los controladores de dominio.

![Diagrama que muestra que se debe minimizar el número de sistemas de Active Directory y dependen de los controladores de dominio](../media/securing-privileged-access-reference-material/PAW_RM_Fig010.JPG)

Para obtener más información sobre la consolidación de los riesgos principales de active directory, consulte [esta página](http://aka.ms/hardenAD).

### <a name="OSBCS_BM"></a>Estándares operativos basados en principio origen limpio
Esta sección describe los estándares y operativos las expectativas del personal administrativo. Estas normas están diseñadas para proteger el control administrativo de sistemas de tecnología de información de la organización frente a riesgos de que se pueden crear en los procesos y procedimientos operativos.

![Diagrama que muestra cómo estándares están diseñados para proteger los sistemas de tecnología frente a riesgos de que se pueden crear en los procesos y prácticas operativas control administrativo de información de la organización](../media/securing-privileged-access-reference-material/PAW_RM_Fig11.JPG)

#### <a name="integrating-the-standards"></a>Integración de los estándares
Puedes integrar estos estándares en estándares y procedimientos recomendados generales de la organización. Se pueden adaptar a los requisitos específicos, herramientas disponibles y gustos de riesgo de su organización, pero te recomendamos que solo mínimas modificaciones para reducir el riesgo. Te recomendamos usar los valores predeterminados en esta guía como punto de referencia para el estado final ideal y administrar cualquier deltas como excepciones que abordarse en orden de prioridad.

Las instrucciones de estándares se organizan en estas secciones:

-   Suposiciones

-   Comité

-   Procedimientos operativos

    -   Resumen

    -   Detalles de estándares

#### <a name="assumptions"></a>Suposiciones
Los estándares de esta sección se supone que la organización tiene los siguientes atributos:

-   La mayoría o todos los servidores y estaciones de trabajo en el ámbito se unen a Active Directory.

-   Todos los servidores administrarse ejecutan Windows Server 2008 R2 o posterior y tienen habilitado el modo RDP RestrictedAdmin.

-   Todas las estaciones de trabajo para que se administren ejecutan Windows 7 o versiones posteriores y tienen habilitado el modo RDP RestrictedAdmin.

    > [!NOTE]
    > Para habilitar el modo de RestrictedAdmin RDP, consulta [esta página](http://aka.ms/RDPRA).

-   Las tarjetas inteligentes están disponibles y emitido para las cuentas administrativas todo.

-   La *Builtin\Administrator* para cada dominio se ha designado como una cuenta de acceso de emergencia

-   Se implementa una solución de administración de identidad de empresa.

-   [VUELTAS](http://aka.ms/laps) se haya implementado en servidores y estaciones de trabajo para administrar la contraseña de cuenta de administrador local

-   Hay una solución de administración de acceso con privilegios, como Microsoft Identity Manager, en su lugar, o si hay un plan de adoptar una.

-   Personal se asigna a supervisar las alertas de seguridad y responder a ellos.

-   La capacidad técnica rápidamente aplicar las actualizaciones de seguridad de Microsoft está disponible.

-   Controladores de administración de la placa base en los servidores no se usará o se adhieren a los controles de seguridad estricta.

-   Cuentas de administrador y los grupos de servidores (nivel 1 administradores) y estaciones de trabajo (administradores de nivel 2) se administran por administradores de dominio (nivel 0).

-   Hay un cambio Advisory Board (archivo CAB) o a otra autoridad designada en su lugar para aprobar cambios de Active Directory.

#### <a name="change-advisory-board"></a>Comité
Placa aviso de cambio (CAB) es la autoridad de aprobación y el foro de análisis para los cambios que puede afectar el perfil de seguridad de la organización. Cualquier excepción a estos estándares debe enviarse para el archivo CAB con una justificación y evaluación de riesgos.

Cada estándar en este documento se divide por la importancia de cumplir el estándar para un determinado nivel.

![Diagrama que muestra el estándar para dar niveles](../media/securing-privileged-access-reference-material/PAW_RM_Fig12.JPG)

Todas las excepciones para los elementos obligatorios (marcados con octágono rojo o un triángulo anaranjado en este documento) se consideran temporales y que necesitan para ser aprobados por el archivo CAB. Las directrices incluyen:

-   La solicitud inicial requiere la aceptación de riesgo de justificación firmado por supervisor inmediato del personal, y que caduque después de seis meses.

-   Renovaciones requieren justificación y aceptación de riesgo firmados por un director de unidades empresariales y caduque después de seis meses.

Todas las excepciones para los elementos de recomendada (marcados con un círculo amarillo en este documento) se consideran temporales y deben aprobar el archivo CAB. Las directrices incluyen:

-   La solicitud inicial requiere la aceptación de riesgo de justificación firmado por supervisor inmediato del personal y que caduque después de 12 meses.

-   Renovaciones requieren justificación y aceptación de riesgo firmados por un director de unidades empresariales y caduque después de 12 meses.

#### <a name="operational-practices-standards-summary"></a>Normas prácticas operativas resumen
Consulte el nivel de la cuenta administrativa, el control de los cuales generalmente afecta a todos los activos en ese nivel de las columnas de nivel en esta tabla.

![Tabla que muestra los niveles de la cuenta admninistrative](../media/securing-privileged-access-reference-material/PAW_RM_Fig13.JPG)

Operativas decisiones que se realizan de forma regular son esenciales para el mantenimiento de la posición de seguridad del entorno. Estos estándares para los procesos y prácticas de ayudar a garantizar que una vulnerabilidad operativa siendo aprovechable en el entorno no provocar un error de funcionamiento.

##### <a name="administrator-enablement-and-accountability"></a>Responsabilidad y la habilitación de administrador
Los administradores deben informados, habilitados, capacitados y les puede pedir a operar el entorno de forma más segura como sea posible.

###### <a name="administrative-personnel-standards"></a>Estándares personal administrativo
Debe ser examinado asignado personal administrativo para garantizar que son de confianza y tengan privilegios administrativos:

-   Realizar comprobaciones en segundo plano en personal antes de asignar privilegios administrativos.

-   Revisa cada trimestre de privilegios administrativos para determinar qué personal aún tiene un negocio legítimo necesita acceso administrativo.

###### <a name="administrative-security-briefing-and-accountability"></a>Responsabilidad y la información de seguridad administrativa
Los administradores deben estar informados y responsable de los riesgos para la organización y su función en la administración de riesgo. Los administradores deben aprender anualmente en:

-   Entorno de amenazas general

    -   Determinados adversarios

    -   Atacar técnicas incluidos pass-the-hash y el robo de credenciales

-   Incidentes y amenazas específicas de la organización

-   Roles de administrador en la protección contra ataques

    -   Administrar la exposición de credencial con el modelo de niveles

    -   Uso de estaciones de trabajo administrativas

    -   Uso del modo de RestrictedAdmin de protocolo de escritorio remoto

-   Prácticas administrativas específicas de la organización

    -   Revisar todas las directrices operativas en este estándar

    -   Implementar las siguientes reglas claves:

        -   No uses las cuentas administrativas en como estaciones de trabajo administrativas

        -   No deshabilite o Desmontes los controles de seguridad en tu cuenta o las estaciones de trabajo (por ejemplo, las restricciones de inicio de sesión o atributos necesarios para tarjetas inteligentes)

        -   Informe de problemas o de una actividad inusual

Para proporcionar la responsabilidad, todas las personas con las cuentas administrativas deben firmar un documento administrativas privilegios código de conducta que dice que pretenden sigue los procedimientos de directiva administrativas específicas de la organización.

###### <a name="provisioning-and-deprovisioning-processes-for-administrative-accounts"></a>Aprovisionamiento y baja procesos para las cuentas administrativas
Se deben cumplir los siguientes estándares para cumplir los requisitos de ciclo de vida.

-   Todas las cuentas administrativas se deben aprobar la autoridad de aprobación se describe en la siguiente tabla.

    -   Aprobación solo debe concederse si el personal de tener un legítimos necesidad empresarial de privilegios administrativos.

    -   Aprobación para privilegios administrativos no debe superar los seis meses.

-   Acceso a privilegios administrativos debe ser de baja inmediatamente cuando:

    -   Personal cambia posiciones.

    -   Personal deja la organización.

-   Cuentas deben estar deshabilitadas inmediatamente después de salir de la organización de personal.

-   Las cuentas deshabilitadas deben eliminarse en seis meses y debe especificarse el registro de su eliminación en registros de tablero de aprobación de cambio.

-   Revisa todos los miembros de cuenta con privilegios mensualmente para asegurarse de que se han concedido ningún permiso no autorizado. Esto puede sustituirse por una herramienta automatizada que alerta cambios.

|Nivel de privilegios de cuenta|Autoridad de aprobación|Frecuencia de revisión de suscripción|
|--------------|------------|----------------|
|Administrador de nivel 0|Cambiar el tablero de aprobación|Mensual o automatizada|
|Administrador de nivel 1|Los administradores de nivel 0 o de seguridad|Mensual o automatizada|
|Administrador de nivel 2|Los administradores de nivel 0 o de seguridad|Mensual o automatizada|

##### <a name="operationalize-least-privilege"></a>Controle los privilegios mínimos
Estos estándares ayudan a lograr menor privilegio reduciendo el número de administradores de función y la cantidad de tiempo que tienen privilegios.

> [!NOTE]
> Alcanzar los privilegios mínimos en la organización requerirá comprender las funciones de la organización, sus requisitos y los mecanismos de diseño para asegurarte de que estén llevar a cabo su trabajo mediante el uso de privilegios mínimos. Lograr un estado de privilegios mínimos en un modelo administrativo con frecuencia, requiere el uso de varios enfoques:
>
> -   Limitar el número de administradores o a los miembros de grupos con privilegios
> -   Delegado menos privilegios a cuentas
> -   Proporcionar privilegios controlado por tiempo a petición
> -   Permitir que otros personal realizar tareas (un enfoque de conserje)
> -   Proporcionar procesos para escenarios de uso poco frecuente y acceso de emergencia

###### <a name="limit-count-of-administrators"></a>Recuento de límite de administradores
Un mínimo de dos personal cualificado debe asignarse a cada función administrativa para garantizar la continuidad del negocio.

Si el número de personal asignado a ninguna función supera dos, el tablero de aprobación de cambio debe aprobar las razones para asignar privilegios a cada miembro individual (incluidos los dos originales). La justificación de autorización debe incluir:

-   ¿Qué técnica se realizan los administradores que requieren privilegios administrativos

-   ¿Con qué frecuencia se realizan las tareas

-   ¿Por qué no se puede realizar las tareas por otro administrador en su nombre de razón específica

-   Todos los otros métodos alternativos conocidos a conceder el privilegio y por qué cada uno no aceptable del documento

###### <a name="dynamically-assign-privileges"></a>Asignar dinámicamente privilegios
Los administradores deben obtener permisos "just-in-time" para utilizarlos como realizan tareas. No se permanentemente asignar permisos a las cuentas administrativas.

> [!NOTE]
> Privilegios administrativos permanentemente asignados naturalmente crean una estrategia de "privilegios mayoría" porque personal administrativo requiere acceso rápido a los permisos para mantener la disponibilidad operativa si hay un problema. Permisos de Just-in-time proporcionan la capacidad de:
>
> -   Asigne permisos más granular, vaya acercando a menor privilegio.
> -   Reducir el tiempo de exposición de privilegios
> -   Permisos de seguimiento se usan para detectar los abusos o los ataques.

##### <a name="manage-risk-of-credential-exposure"></a>Administrar los riesgos de la exposición de credenciales
Usa los siguientes procedimientos para administrar el riesgo de exposición de credenciales adecuado.

###### <a name="separate-administrative-accounts"></a>Cuentas administrativas independientes
Todas las personas que están autorizadas a poseen privilegios administrativos deben tener cuentas separadas para funciones administrativas que son distintas de cuentas de usuario.

-   **Cuentas de usuario estándar** -concedido privilegios de usuario estándar para las tareas de usuario estándar, como correo electrónico, exploración web o el uso de aplicaciones de línea de negocio. Estas cuentas no deben concederse privilegios administrativos.

-   **Las cuentas administrativas** -separar las cuentas creadas para personal que se asigna los privilegios administrativos adecuados. Un administrador que es necesario para administrar activos en cada nivel debe tener una cuenta independiente para cada nivel. Estas cuentas deben no tienen acceso a Internet pública o de correo electrónico.

###### <a name="administrator-logon-practices"></a>Procedimientos recomendados de inicio de sesión de administrador
Antes de que un administrador puede iniciar sesión en un host de manera interactiva (localmente a través de RDP estándar, usando RunAs, o mediante la consola de virtualización), debe cumplir con ese host o se superan el estándar para la cuenta de administrador nivel (o en un nivel superior).

Los administradores pueden solo inicia sesión en estaciones de trabajo de administración con sus cuentas administrativas. Los administradores solo iniciar sesión en recursos administrados mediante la tecnología de asistencia aprobadas que se describe en la siguiente sección.

> [!NOTE]
> Esto es necesario porque el inicio de sesión en un host de manera interactiva concede el control de las credenciales a ese host.
>
> Consulta la [herramientas administrativas y tipos de inicio de sesión](http://aka.ms/admintoolsecurity) para obtener más información acerca de los tipos de inicio de sesión, herramientas comunes de administración y la exposición de credenciales.

###### <a name="use-of-approved-support-technology-and-methods"></a>Uso de tecnología de asistencia aprobadas y métodos
Los administradores que admiten los usuarios y sistemas remotos deben seguir estas directrices para evitar que un control del equipo remoto robar sus credenciales administrativas.

-   Las opciones de soporte técnico principal deben usarse si están disponibles.

-   Las opciones de soporte secundaria solo deben usarse si la opción de soporte técnico principal no está disponible.

-   Métodos de compatibilidad prohibidas nunca pueden usarse.

-   Acceso a correo electrónico o la exploración de internet no puede realizarse por cualquier cuenta de administrador en cualquier momento.

###### <a name="tier-0-forest-domain-and-dc-administration"></a>Bosque de nivel 0, dominio y la administración de DC
Asegúrate de que se aplican los siguientes procedimientos para este escenario:

-   **Compatibilidad con el servidor remoto** -al acceso remoto a un servidor, los administradores de nivel 0 deben seguir estas directrices:

    -   **Primary (herramienta)** -remoto de las herramientas que inicios de sesión de red de uso (tipo 3). Para obtener más información, consulta [herramientas administrativas y tipos de inicio de sesión](http://aka.ms/admintoolsecurity).

    -   **Primary (interactiva)** -RestrictedAdmin RDP de uso o una sesión de RDP estándar de una estación de trabajo de administración con una cuenta de dominio

    > [!NOTE]
    > Si tienes una solución de administración de nivel 0 privilegio, agrega "que usan permisos obtienen just-in-time de una solución de administración con privilegios de acceso".

-   **Compatibilidad con el servidor físico** : cuando presentes físicamente en una consola del servidor o en una consola de máquina virtual (Hyper-V o VMWare tools), estas cuentas no tienen restricciones de uso de herramientas administrativas específicas, solo las restricciones generales de tareas de usuario estándar como correo electrónico y navegar por internet abiertas.

    > [!NOTE]
    > Administración de nivel 0 es diferente de administración de otros niveles porque todos los activos de nivel 0 ya tienen el control directo o indirecto de todos los activos. Por ejemplo, un atacante en un control de un controlador de dominio no tiene necesidad roban las credenciales de los administradores de sesión que ya tienen acceso a todas las credenciales de dominio en la base de datos.

###### <a name="tier-1-server-and-enterprise-application-support"></a>Servidor de nivel 1 y compatibilidad con aplicaciones de empresa
Asegúrate de que se aplican los siguientes procedimientos para este escenario:

-   **Compatibilidad con el servidor remoto** -al acceso remoto a un servidor, los administradores de nivel 1 deben seguir estas directrices:

    -   **Primary (herramienta)** -remoto de las herramientas que inicios de sesión de red de uso (tipo 3). Para obtener más información, consulta [Mitigating Pass-the-Hash y otros robo de credenciales](https://www.microsoft.com/pth) v1 (páginas 42 47).

    -   **Primary (interactiva)** -RestrictedAdmin RDP de uso de una estación de trabajo de administración con una cuenta de dominio que usa permisos obtenido just-in-time de una solución de administración de acceso con privilegios.

    -   **Secundario** -iniciar sesión en el servidor mediante una contraseña de cuenta local que se establece mediante PARCIALES desde una estación de trabajo de administrador.

    -   **Prohibido** -RDP estándar no puede usarse con una cuenta de dominio.

    -   **Prohibido** -con la cuenta de dominio de credenciales mientras se encuentra en la sesión (por ejemplo, mediante *RunAs* o autenticar en un recurso compartido). Esto expone las credenciales de inicio de sesión que el riesgo de robo.

-   **Compatibilidad con el servidor físico** : cuando presentes físicamente en una consola del servidor o en una consola de máquina virtual (Hyper-V o VMWare tools), los administradores de nivel 1 deben recuperar la contraseña de cuenta local de VUELTAS antes de obtener acceso al servidor.

    -   **Principales** -recuperar la contraseña de cuenta local establecida por PARCIALES desde una estación de trabajo de administrador antes de iniciar sesión en el servidor.

    -   **Prohibido** -iniciar sesión con una cuenta de dominio no se permite en este escenario.

    -   **Prohibido** -con la cuenta de dominio de credenciales mientras se encuentra en la sesión (por ejemplo, RunAs o a un recurso compartido de autenticación). Esto expone las credenciales de inicio de sesión que el riesgo de robo.

###### <a name="tier-2-help-desk-and-user-support"></a>Nivel 2 departamento de soporte y soporte técnico de usuario
Ayuda de usuario y servicio de soporte técnico organizaciones realizan soporte técnico para los usuarios finales (que no requieren privilegios administrativos) y las estaciones de trabajo de usuario (que se requieren privilegios administrativos).

**Compatibilidad con el usuario** -tareas incluyen ayudan a los usuarios con la realización de tareas que no requieren la estación de trabajo, se modifica con frecuencia mostrar cómo usar una característica de la aplicación o característica del sistema operativo.

-   **Compatibilidad con el usuario junto al escritorio** -2 de nivel el personal de soporte técnico está físicamente en el área de trabajo del usuario.

    -   **Principales** -"sobre los hombros" se puede proporcionar compatibilidad con ninguna de las herramientas.

    -   **Prohibido** -iniciar sesión con credenciales administrativas de la cuenta de dominio no se permite en este escenario. Cambiar a soporte técnico de la estación de trabajo del lado del escritorio si se requieren privilegios administrativos.

-   **Soporte técnico del usuario remoto** -personal de soporte técnico el nivel 2 es físicamente remoto al usuario.

    -   **Principales** -puede usar Asistencia remota, Skype empresarial o el uso compartido de pantalla de usuario similares. Para obtener más información, consulta [¿qué es Asistencia remota de Windows?](https://windows.microsoft.com/en-us/windows/what-is-windows-remote-assistance)

    -   **Prohibido** -iniciar sesión con credenciales administrativas de la cuenta de dominio no se permite en este escenario. Cambiar a soporte técnico de la estación de trabajo si se requieren privilegios administrativos.

-   **Soporte técnico de la estación de trabajo** : tareas incluyen la realización de mantenimiento de la estación de trabajo o solución de problemas que requiere acceso a un sistema para ver los registros, instalar software, actualizar los controladores y así sucesivamente.

    -   **Soporte técnico de la estación de trabajo junto al escritorio** -2 de nivel el personal de soporte técnico está físicamente en la estación de trabajo del usuario.

        -   **Principales** -recuperar la contraseña de cuenta local establecida por PARCIALES desde una estación de trabajo de administrador antes de conectarse a la estación de trabajo del usuario.

        -   **Prohibido** -iniciar sesión con credenciales administrativas de la cuenta de dominio no se permite en este escenario.

    -   **Soporte técnico de la estación de trabajo remota** -personal de soporte técnico el nivel 2 es físicamente remoto a la estación de trabajo.

        -   **Principales** -RestrictedAdmin RDP de uso de una estación de trabajo de administración con una cuenta de dominio que usa permisos obtenido just-in-time de una solución de administración de acceso con privilegios.

        -   **Secundario** -recuperar una contraseña de cuenta local establecidos por PARCIALES desde una estación de trabajo de administrador antes de conectarse a la estación de trabajo del usuario.

        -   **Prohibido** -usar RDP estándar con una cuenta de dominio.

###### <a name="no-browsing-the-public-internet-with-admin-accounts-or-from-admin-workstations"></a>No navegar por Internet con cuentas de administrador o de las estaciones de trabajo de administración pública
Personal administrativo no puede navegar por Internet abierta durante una sesión iniciada con una cuenta de administrador o mientras la sesión iniciada en una estación de trabajo. La única excepción autorizada es el uso de un explorador web para administrar un servicio basado en la nube, como Microsoft Azure, servicios Web de Amazon, Microsoft Office 365 o empresa Gmail.

###### <a name="no-accessing-email-with-admin-accounts-or-from-admin-workstations"></a>Sin acceso a correo con cuentas de administrador o de las estaciones de trabajo de administración
Personal administrativo no puede acceder al correo electrónico durante una sesión iniciada con una cuenta de administrador o mientras la sesión iniciada en una estación de trabajo.

###### <a name="store-service-and-application-account-passwords-in-a-secure-location"></a>Servicio de la tienda y contraseñas en una ubicación segura de la cuenta de aplicación
Deben usarse las siguientes directrices para la seguridad física procesa que controlan el acceso a la contraseña:

-   Bloquear las contraseñas de cuentas de servicio en una caja de seguridad física.

-   Garantiza que solo el personal de confianza a o encima de la clasificación de nivel de la cuenta tienen acceso a la contraseña de cuenta.

-   Limita el número de personas que tienen acceso a las contraseñas en un número mínimo y de responsabilidad.

-   Asegúrate de que todo el acceso a la contraseña está iniciado, realiza un seguimiento y controlado por una entidad disinterested, como un administrador que no está preparado para realizar la administración de TI.

##### <a name="strong-authentication"></a>Autenticación firme
Usa los siguientes procedimientos para configurar la autenticación firme adecuado.

###### <a name="enforce-smartcard-multi-factor-authentication-mfa-for-all-admin-accounts"></a>Aplicar la tarjeta inteligente la autenticación multifactor (AMF) para todas las cuentas de administrador
Ninguna cuenta de administrador puede usar una contraseña para la autenticación. Las únicas excepciones autorizadas son las cuentas de acceso de emergencia que están protegidas por los procesos apropiados.

Vincular todas las cuentas administrativas a una tarjeta inteligente y habilitar el atributo "**tarjeta inteligente es necesaria para el inicio de sesión interactivo**."

Un script que debe implementarse para automática y periódicamente restablecer el valor de hash de contraseña aleatoria al deshabilitar y volver a habilitar inmediatamente el atributo "**tarjeta inteligente es necesaria para el inicio de sesión interactivo**."

No permitir ninguna excepción para cuentas usadas por el personal de humano más allá de las cuentas de acceso de emergencia.

###### <a name="enforce-multi-factor-authentication-for-all-cloud-admin-accounts"></a>Aplicar la autenticación multifactor para todas las cuentas de administrador de la nube
Todas las cuentas con privilegios administrativos en un servicio de nube, como Microsoft Azure y Office 365, deben usar la autenticación multifactor.

##### <a name="rare-use-emergency-procedures"></a>Procedimientos de emergencia de uso poco frecuentes
Procedimientos operativos deben ser compatible con los estándares siguientes:

-   Asegúrate de que las interrupciones se pueden resolver rápidamente.

-   Asegúrate de que pueden realizar tareas de alto privilegios raras según sea necesario.

-   Asegúrate de que se usan procedimientos seguros para proteger las credenciales y privilegios.

-   Asegúrate de que se tratan los procesos de seguimiento y aprobación adecuados.

###### <a name="correctly-follow-appropriate-processes-for-all-emergency-access-accounts"></a>Sigue correctamente los procedimientos adecuados de todas las cuentas de acceso de emergencia
Asegúrate de que cada cuenta de acceso de emergencia tiene una hoja de seguimiento en la caja de seguridad.

Debes seguir el procedimiento explicado en la hoja de seguimiento de contraseña para cada cuenta, que incluye cambiar la contraseña después de cada uso y cerrar la sesión en las estaciones de trabajo o servidores usados tras la finalización.

Todas las usan cuentas deben aprobarlo el tablero de aprobación de cambio en avanzada o después a los hechos como un uso de emergencia aprobado de acceso de emergencia.

###### <a name="restrict-and-monitor-usage-of-emergency-access-accounts"></a>Restringir y supervisar el uso de cuentas de acceso de emergencia
Para el uso de cuentas de acceso de emergencia:

-   Los administradores de dominio autorizado solo pueden tener acceso a las cuentas de acceso de emergencia con privilegios de administrador de dominio.

-   Las cuentas de acceso de emergencia pueden usarse únicamente en controladores de dominio y otros hosts de nivel 0.

-   Esta cuenta debe usarse solo para:

    -   Realizar la solución de problemas y la corrección de problemas técnicos que impiden el uso de las cuentas administrativas correctos.

    -   Realizar tareas poco frecuentes, como por ejemplo:

        -   Administración del esquema

        -   Tareas de todo el bosque que requieren privilegios administrativos empresariales Ten en cuenta que administración topología incluida la administración de sitio y de subred de Active Directory se delega a limitar el uso de estos privilegios.

-   Todo el uso de una de estas cuentas debería haber escrito autorización por el cliente potencial de grupo de seguridad

-   El procedimiento de la hoja de seguimiento para cada cuenta de acceso de emergencia requiere la contraseña cambiarse para cada uso. Debes validar un miembro del equipo de seguridad que se ha realizado correctamente.

###### <a name="temporarily-assign-enterprise-admin-and-schema-admin-membership"></a>Asignar temporalmente la pertenencia de administrador de administrador y esquema enterprise
Privilegios deben agregarse como sea necesario y eliminó después de uso. La cuenta de emergencia debe tener estos privilegios asignados para solo la duración de la tarea que se complete y para un máximo de 10 horas. Todo uso y la duración de estos privilegios se deben capturar en el registro de tablero de aprobación de cambios después de completar la tarea.

## <a name="ESAE_BM"></a>Enfoque de diseño de bosque administrativas ESAE
Esta sección contiene un enfoque para un bosque administrativo basado en la arquitectura de referencia mejorado seguridad administrativas entorno (ESAE) implementada por equipos de servicios profesionales la ciberseguridad de Microsoft para proteger a los clientes la ciberseguridad ataques.

Dedicado bosques administrativas permiten a las organizaciones host, las estaciones de trabajo, grupos y cuentas administrativas en un entorno que tiene controles de seguridad más seguros que el entorno de producción.

Esta arquitectura permite a un número de controles de seguridad que no son posibles o configurado fácilmente en una arquitectura de bosque único, incluso uno que administrados con estaciones de trabajo con privilegios de acceso (garras). Este enfoque permite el aprovisionamiento de cuentas como sin privilegios usuarios estándar en el bosque administrativo que tienen muchos privilegios en el entorno de producción, lo que permite mayor obligatoriedad técnica de control. Esta arquitectura también permite el uso de la característica de autenticación selectiva de confianza como un medio para restringir los inicios de sesión (y la exposición de credenciales) solo los hosts autorización. En situaciones en que se desea un mayor nivel de garantía para el bosque de producción sin incurrir en el costo y la complejidad de una reconstrucción completa, un bosque administrativo puede proporcionar un entorno que aumenta el nivel de seguridad del entorno de producción.

Aunque este enfoque agregar un bosque en un entorno de Active Directory, el costo y la complejidad están limitadas por el diseño fijo, la superficie de hardware y software pequeño y un número reducido de usuarios.

> [!NOTE]
> Este enfoque funciona bien para la administración de Active Directory, pero muchas aplicaciones no son compatibles con la que se está administrando las cuentas de un bosque externo con una relación de confianza estándar.

Esta figura muestra un bosque ESAE usado para la administración de activos de nivel 0 y un bosque PRIV configurado para usarlos con la funcionalidad de administración con privilegios de acceso de Microsoft Identity Manager. Para obtener más información sobre cómo implementar una instancia de MIM PAM, consulta [con privilegios de administración de identidades de los servicios de dominio de Active Directory (AD DS)](https://technet.microsoft.com/en-us/library/mt150258.aspx) artículo.

![Ilustración que muestra un bosque ESAE usado para la administración de activos de nivel 0 y un bosque PRIV configurado para usarlos con la funcionalidad de administración con privilegios de acceso de Microsoft Identity Manager](../media/securing-privileged-access-reference-material/PAW_RM_Fig14.JPG)

Un bosque administrativo dedicado es un solo dominio estándar dedicado a la función de la administración de Active Directory de bosque de Active Directory. Dominios y bosques administrativas pueden reforzados modo más riguroso bosques de producción debido a la limitación de casos de uso.

Un diseño de bosque administrativas debe incluir las siguientes consideraciones:

-   **Limita el ámbito** -el valor principal de un bosque de administrador es el alto nivel de garantía de la seguridad y la superficie de ataque reducida, lo que reduce el riesgo residual. El bosque puede usarse para albergar más funciones de administración y las aplicaciones, pero cada incremento en el ámbito aumentará la superficie de ataque del bosque y sus recursos. El objetivo es limitar las funciones de los usuarios de bosque y el administrador dentro de mantener la superficie de ataque mínima, por lo que se debe considerar cada incremento de ámbito cuidadosamente.

-   **Configuraciones de confianza** -configurar la confianza de bosques administrados (s) o dominios en el bosque administrativo

    -   Una confianza unidireccional es necesaria desde el entorno de producción al bosque de administrador. Esto puede ser una confianza de dominio o una confianza de bosque. No es necesario confiar en los dominios y bosques administrados para administrar Active Directory, aunque las aplicaciones adicionales pueden requerir una relación de confianza bidireccional, la validación de seguridad, el dominio o bosque de administración y las pruebas.

    -   Autenticación selectiva debe usarse para restringir las cuentas en el bosque de administrador para iniciar sesión en los hosts de producción apropiados solo. Para mantener los controladores de dominio y delegación de derechos en Active Directory, esto normalmente requiere conceder el "Permitir inicio de sesión" adecuado para controladores de dominio para cuentas de administrador 0 nivel designados en el bosque de administrador. Para obtener más información, consulte Configurar opciones de autenticación selectiva.

-   **Privilegios y refuerzo de dominio** -el bosque administrativo debe estar configurado para los privilegios mínimos en función de los requisitos para la administración de Active Directory.

    -   Conceder derechos para administrar controladores de dominio y el delegado permisos requiere agregar cuentas de administrador bosque grupo BUILTIN\Administrators dominio local. Esto es porque el grupo de administradores de dominio global no puede tener miembros de un dominio externo.

    -   Una advertencia para usar este grupo para conceder derechos es que no tienen acceso administrativo a los nuevos objetos de directiva de grupo de manera predeterminada. Esto se puede cambiar el siguiente procedimiento de [este artículo de knowledge base](https://support.microsoft.com/kb/321476) para cambiar los permisos predeterminados de esquema.

    -   Cuentas en el bosque de administrador que se usan para administrar el entorno de producción no deben concederse privilegios administrativos para su del bosque de administrador de dominios en él, o estaciones de trabajo en él.

    -   Privilegios administrativos en el bosque de administrador deben estar estrictamente controlados por un proceso sin conexión para reducir la posibilidad de un atacante o malintencionado insider para borrar los registros de auditoría. Esto ayuda a garantizar que el personal con cuentas de administrador de producción no se relájate las restricciones en sus cuentas y aumenta el riesgo para la organización.

    -   El bosque administrativo debe seguir las configuraciones de línea base de cumplimiento de seguridad de Microsoft (SCB) para el dominio, incluidas las configuraciones seguras para los protocolos de autenticación.

    -   Todos los hosts de bosque de administrador deben actualizarse automáticamente con las actualizaciones de seguridad. Si bien esto puede crear riesgo de interrumpir las operaciones de mantenimiento del controlador de dominio, proporciona una significativo mitigación de riesgos de seguridad de las vulnerabilidades sin revisión.

        > [!NOTE]
        > Una instancia de Windows Server Update Services dedicada puede configurarse para aprobar automáticamente las actualizaciones. Para obtener más información, consulta la sección de "Automáticamente aprobar actualizaciones para la instalación" en la aprobación de actualizaciones.

-   **Estación de trabajo refuerzo** -crear las estaciones de trabajo administrativas con la [estaciones de trabajo con privilegios de acceso](../securing-privileged-access/privileged-access-workstations.md) (a través de la fase 3), pero cambiar la pertenencia a dominio en el bosque administrativo en lugar del entorno de producción.

-   **Servidor y refuerzo de DC** - para todos los controladores de dominio y servidores en el bosque administrativo:

    -   Asegúrate de que todos los medios se valida con las instrucciones en [origen limpio medios de instalación](http://aka.ms/cleansource)

    -   Asegúrate de que los servidores de bosque administrativas deben tener los sistemas operativos más recientes instalados, incluso si esto no es factible en producción.

    -   Los hosts de bosque de administrador deben actualizarse automáticamente con actualizaciones de seguridad.

        > [!NOTE]
        > Windows Server Update Services puede configurarse para aprobar automáticamente las actualizaciones. Para obtener más información, consulta la sección de "Automáticamente aprobar actualizaciones para la instalación" en la aprobación de actualizaciones.

    -   Líneas de base de seguridad deben usarse como a partir de las configuraciones.

        > [!NOTE]
        > Los clientes pueden usar el Kit de herramientas de cumplimiento de seguridad de Microsoft (SCT) para configurar las líneas de base en los hosts administrativos.

    -   Arranque a mitigar contra los atacantes o malware intenta cargar el código sin firmar en el proceso de arranque seguro.

        > [!NOTE]
        > Esta característica se presentó en Windows 8 para sacar partido de Unified Extensible Firmware Interface (UEFI).

    -   Cifrado de volumen completo para mitigar contra la pérdida de física de los equipos, como los portátiles administrativos que se usan de forma remota.

        > [!NOTE]
        > Consulta [BitLocker](https://technet.microsoft.com/en-us/library/dn641993.aspx) para obtener más información.

    -   Restricciones de USB para protegerse contra la infección físico.

        > [!NOTE]
        > Consulta [Control de acceso de lectura o escritura en dispositivos extraíbles o en medios](https://technet.microsoft.com/en-us/library/cc730808(v=ws.10).aspx) para obtener más información.

    -   Aislamiento de red para protegerse contra ataques de red y las acciones de administrador involuntaria. Firewall de host deben bloquear todas las conexiones entrantes, excepto los explícitamente requeridos y bloquear todo el acceso saliente a Internet.

    -   Antimalware para proteger contra malware y las amenazas conocidas.

    -   Análisis de la superficie de ataque para evitar la introducción de los vectores de ataque nueva a Windows durante la instalación de software nuevo.

        > [!NOTE]
        > Uso de herramientas como el [analizador de superficie de ataque (ASA)](https://www.microsoft.com/en-us/download/details.aspx?id=24487) te ayudará a evaluar las opciones de configuración en un host e identificar los vectores de ataque introducidos por los cambios de software y configuración.

-   Refuerzo de la cuenta

    -   La autenticación multifactor debe estar configurada para todas las cuentas en el bosque de administrador, excepto una cuenta. Debe ser al menos una cuenta administrativa basadas en para garantizar un acceso contraseña funcionará en caso de que la autenticación multifactor procesar saltos. Esta cuenta debe estar protegida por un proceso estrictos control físico.

    -   Cuentas configuradas para la autenticación multifactor deben configurarse para establecer un nuevo NTLM hash en cuentas con regularidad. Esto se puede lograr al deshabilitar y habilitar el atributo de cuenta tarjeta inteligente es necesario para el inicio de sesión interactivo.

        > [!NOTE]
        > Esto puede interrumpir las operaciones en curso que usan esta cuenta, por lo que se debe iniciar este proceso solo cuando los administradores no vayas a usar la cuenta, como por la noche o los fines de semana.

-   Controles investigue

    -   Investigue controles para el bosque administrativo deben estar diseñados para enviarte una alerta en anomalías en el bosque de administrador. El número limitado de escenarios autorizados y actividades puede ayudar a ajustar estos controles con mayor exactitud que el entorno de producción.

Para obtener más información atractiva acerca de los servicios de Microsoft para diseñar e implementar una ESAE para el entorno, consulta [esta página](https://www.microsoft.com/services).

## <a name="T0E_BM"></a>Equivalencia de nivel 0
La mayoría de las organizaciones controlan la pertenencia a grupos de Active Directory de nivel 0 eficaces como administradores, administradores de dominio y administradores de empresa.  Muchas organizaciones pase por alto el riesgo de otros grupos que son realmente el equivalente de privilegios en un entorno típico de active directory. Estos grupos ofrecen una ruta de acceso de escalación relativamente fácil para un atacante los mismos privilegios explícitos de nivel 0 mediante diversos métodos de ataque diferentes.

Por ejemplo, un operador de servidor podría tener acceso a un medio de copia de seguridad de un controlador de dominio y extraer todas las credenciales de los archivos en el que el contenido multimedia y usarlos para elevar privilegios.

Las organizaciones deben controlar y supervisar la pertenencia a todos los grupos de nivel 0 (incluido pertenencia anidada) como:

-   Administradores de empresa

-   Administradores de dominio

-   Administradores de esquema

-   BUILTIN\Administrators

-   Operadores de cuentas

-   Operadores de copia de seguridad

-   Operadores de impresión

-   Operadores de servidor

-   Controladores de dominio

-   Controladores de dominio de solo lectura

-   Propietarios de creadores de directivas de grupo

-   Operadores criptográficos

-   Usuarios de COM distribuido

-   Otros delegado grupos

    > [!NOTE]
    > "Otros grupos delegadas" se refiere a los grupos que pueden ser creados por la organización para administrar las operaciones de directorio que también pueden tener acceso de nivel 0 eficaces.

## <a name="ATLT_BM"></a>Herramientas administrativas y tipos de inicio de sesión
Se trata de información de referencia para ayudar a identificar el riesgo de exposición de credenciales asociada con el uso de las herramientas administrativas para la administración remota.

En un escenario de administración remota, siempre se exponen las credenciales en el equipo de origen para que una estación de trabajo de confianza acceso con privilegios (PATA) siempre se recomienda para las cuentas con información confidencial o alto impacto. Si las credenciales se exponen a posible robo del equipo de destino (remoto) depende principalmente el tipo de inicio de sesión de windows usado el método de conexión.

Esta tabla incluye instrucciones para las herramientas administrativas más comunes y los métodos de conexión:

|Conexión<br />método|Tipo de inicio de sesión|Credenciales reutilizables de destino|Comentarios|
|-----------|-------|--------------------|------|
|Iniciar sesión en la consola|Interactivo|v|Incluye el acceso remoto de hardware / Lights-Out KVM de red y tarjetas.|
|RUNAS|Interactivo|v||
|NETWORK RUNAS|NewCredentials|v|Clona la sesión actual de LSA para el acceso local, pero usa credenciales nuevo al conectarse a recursos de red.|
|Escritorio remoto (correcto)|RemoteInteractive|v|Si el cliente de escritorio remoto está configurado para compartir recursos y dispositivos locales, las pueden verse comprometidas también.|
|Escritorio remoto (error: el tipo de inicio de sesión se ha denegado)|RemoteInteractive|-|De manera predeterminada, si se produce un error de inicio de sesión RDP credenciales solo se almacenan brevemente. Esto puede no ser el caso si el equipo está en peligro.|
|NET use * \\\SERVER|Red|-||
|NET use * \\\SERVER u:|Red|-||
|Complementos MMC para el equipo remoto|Red|-|Ejemplo: Equipo administración, Visor de eventos, el Administrador de dispositivos servicios|
|PowerShell WinRM|Red|-|Ejemplo: PSSession escribe server|
|PowerShell WinRM con CredSSP|NetworkClearText|v|Servidor de nuevo PSSession<br />-Credssp autenticación<br />: Proveedores de credenciales|
|PsExec sin credenciales explícitas|Red|-|Ejemplo: PsExec \\\server cmd|
|PsExec con credenciales explícitas|Red + interactivo|v|PsExec \\\server -u usuario -p pwd cmd<br />Crea varias sesiones de inicio de sesión.|
|Registro remoto|Red|-||
|Puerta de enlace de escritorio remoto|Red|-|La autenticación en la puerta de enlace de escritorio remoto.|
|Tareas programadas|Por lotes|v|Contraseña también se guardarán como secreto de LSA en el disco.|
|Ejecuta herramientas como un servicio|Servicio|v|Contraseña también se guardarán como secreto de LSA en el disco.|
|Escáneres de vulnerabilidad|Red|-|La mayoría escáneres utilizan de forma predeterminada los inicios de sesión de red, aunque algunos proveedores pueden implementar los inicios de sesión de fuera de la red y presentan mayor riesgo de robo de credenciales.|

Para la autenticación web, usa la referencia de la tabla siguiente:

|Conexión<br />método|Tipo de inicio de sesión|Credenciales reutilizables de destino|Comentarios|
|-----------|-------|--------------------|------|
|IIS "Autenticación básica"|NetworkCleartext<br />(IIS) 6.0 +<br /><br />Interactivo<br />(antes de IIS 6.0)|v||
|IIS "autenticación integrada de Windows"|Red|-|Proveedores de Kerberos y NTLM.|

Definiciones de columna:

-   **Tipo de inicio de sesión** identifica el tipo de inicio de sesión iniciado por la conexión.

-   **Las credenciales reutilizables de destino** indica que los siguientes tipos de credenciales se almacenarán en la memoria del proceso LSASS en el equipo de destino donde la cuenta especificada ha iniciado sesión localmente:

    -   Hash de LM y NT

    -   Vales Kerberos

    -   Contraseña de texto no cifrado (si procede).

    -

Los símbolos en esta tabla definida del siguiente modo:

-   (-) indica cuando las credenciales no están expuestas.

-   (v) denota cuando se exponen las credenciales.

Aplicaciones de administración que no están en esta tabla, puedes determinar el tipo de inicio de sesión del campo de tipo de inicio de sesión de los eventos de inicio de sesión de auditoría. Para obtener más información, consulta [auditar eventos de inicio de sesión](https://technet.microsoft.com/en-us/library/cc787567(v=ws.10).aspx).

En equipos basados en Windows, se procesan todas las autenticaciones como uno de los distintos tipos de inicio de sesión, independientemente de que la autenticación se usa un protocolo o la autenticación. En esta tabla incluye los tipos de inicio de sesión más comunes y sus atributos en relación con el robo de credenciales:

|Tipo de inicio de sesión|#|Autenticadores aceptados|Credenciales reutilizables en sesión LSA|Ejemplos|
|-------|---|--------------|--------------------|------|
|Interactivo (es decir, inicio de sesión local)|2|Contraseña de la tarjeta inteligente,<br />otros|Sí|Inicio de sesión de la consola;<br />RUNAS;<br />Soluciones de control remoto de hardware (como el acceso remoto o de red KVM Lights-Out tarjeta en el servidor)<br />Autenticación básica de IIS (antes de IIS 6.0)|
|Red|3|Contraseña,<br />Hash de NT,<br />Vale Kerberos|No (excepto si delegación está habilitada, Kerberos vales presente)|NET USE;<br />Llamadas RPC;<br />Registro remoto;<br />IIS integrado de autenticación de Windows;<br />Autenticación de Windows de SQL;|
|Por lotes|4|Contraseña (que normalmente se almacenan como secreto de LSA)|Sí|Tareas programadas|
|Servicio|5|Contraseña (que normalmente se almacenan como secreto de LSA)|Sí|Servicios de Windows|
|NetworkCleartext|8|Contraseña|Sí|Autenticación básica de IIS (IIS 6.0 y versiones posteriores);<br />Windows PowerShell con CredSSP|
|NewCredentials|9|Contraseña|Sí|NETWORK RUNAS|
|RemoteInteractive|10|Contraseña de la tarjeta inteligente,<br />otros|Sí|Escritorio remoto (anteriormente conocido como "Terminal Services")|

Definiciones de columna:

-   **Tipo de inicio de sesión** es el tipo de inicio de sesión solicitado.

-   **#**es el identificador numérico para el tipo de inicio de sesión que se notifica en eventos de auditoría en el registro de eventos de seguridad.

-   **Autenticadores aceptados** indica qué tipos de autenticadores están capaz de iniciar sesión de este tipo.

-   **Las credenciales reutilizables** de LSA sesión indica si el tipo de inicio de sesión se produce en la sesión LSA manteniendo las credenciales, como las contraseñas de texto no cifrado, los valores de hash de NT o vales Kerberos que pueden usarse para autenticar a otros recursos de red.

-   **Ejemplos** lista de escenarios comunes en el que se usa el tipo de inicio de sesión.

> [!NOTE]
> Para obtener más información acerca de los tipos de inicio de sesión, consulta [enumeración SECURITY_LOGON_TYPE](https://technet.microsoft.com/en-us/library/aa380129(VS.85).aspx).
