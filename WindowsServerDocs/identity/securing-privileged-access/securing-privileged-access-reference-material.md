---
title: Protección del material de referencia de acceso con privilegios
description: Controles de seguridad operativa para los dominios de Active Directory de Windows Server
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 22ee9a77-4872-4c54-82d9-98fc73a378c0
ms.date: 02/14/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: mas
ms.openlocfilehash: bcc06a3ccc4e95fa43a7f8f0ef7d110fd427f5a0
ms.sourcegitcommit: cd12ace92e7251daaa4e9fabf1d8418632879d38
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66501650"
---
# <a name="active-directory-administrative-tier-model"></a>Modelo de nivel administrativo de Active Directory

>Se aplica a: Windows Server

El propósito de este modelo de niveles es proteger los sistemas de identidad con un conjunto de zonas de búfer entre el control total del entorno (nivel 0) y los recursos de la estación de trabajo de alto riesgo que los atacantes comprometen con frecuencia.

![Diagrama que muestra las tres capas del modelo en niveles](../media/securing-privileged-access-reference-material/PAW_RM_Fig1.JPG)

El modelo de niveles se compone de tres niveles y solo incluye las cuentas administrativas no las cuentas de usuario estándar:

- **Nivel 0**: control directo de las identidades de empresa en el entorno. El nivel 0 incluye cuentas, grupos y otros recursos que tienen control administrativo directo o indirecto de los bosques, dominios o controladores de dominio de Active Directory y todos los recursos que haya en él. La sensibilidad de seguridad de todos los recursos de nivel 0 es equivalente, ya que todos se controlan de forma efectiva entre sí.
- **Nivel 1**: control de aplicaciones y servidores empresariales. Los recursos de nivel 1 incluyen sistemas operativos de servidor, servicios en la nube y aplicaciones empresariales. Las cuentas Administrador de nivel 1 tienen control administrativo de una cantidad considerable de valor empresarial que se hospeda en estos recursos. Una función de ejemplo común son los administradores de servidores que mantienen estos sistemas operativos con la capacidad de afectar a todos los servicios de empresa.
- **Nivel 2**: control de los dispositivos y estaciones de trabajo de usuarios. Las cuentas Administrador de nivel 2 tienen el control administrativo de una cantidad considerable de valor empresarial que se hospeda en dispositivos y estaciones de trabajo de usuarios. Algunos ejemplos son el departamento de soporte técnico y los administradores de soporte técnico del equipo porque pueden afectar a la integridad de casi cualquier dato de usuario.

> [!NOTE]
> Los niveles también sirven como mecanismo de priorización básico para proteger los recursos administrativos, pero es importante tener en cuenta que un atacante con control de todos los recursos en cualquier nivel puede acceder a la mayoría o a todos los recursos de negocio. La razón de su utilidad como mecanismo básico de la asignación de prioridades es que la dificultad y costo del atacante. Es más fácil para un atacante actuar con control total de todas las identidades (nivel 0) o servidores y servicios en la nube (nivel 1) que si accede a cada estación de trabajo individual o a un dispositivo de usuario (nivel 2) para obtener los datos de su organización.

## <a name="containment-and-security-zones"></a>Zonas de seguridad y de contención

Los niveles son relativos a una zona de seguridad específica. Aunque reciben varios nombres, las zonas de seguridad constituyen un enfoque bien establecido que proporciona contención de las amenazas de seguridad mediante el aislamiento de nivel de red entre ellas. El modelo de nivel de complementa el aislamiento proporcionando contención de los adversarios dentro de una zona de seguridad donde el aislamiento de red no es efectivo. Las zonas de seguridad pueden abarcar una infraestructura local y de nube, como en el ejemplo donde los controladores de dominio y los miembros del dominio en el mismo dominio se hospedan localmente y en Azure.

![Diagrama que muestra cómo las zonas de seguridad pueden abarcar la infraestructura local y en la nube](../media/securing-privileged-access-reference-material/PAW_RM_Fig2.JPG)

El modelo de niveles evita el escalamiento de privilegios restringiendo lo que los administradores pueden controlar y dónde pueden iniciar sesión (puesto que inician sesión en un equipo que concede control de esas credenciales y a todos los recursos administrados por dichas credenciales).

### <a name="control-restrictions"></a>Restricciones de control

Las restricciones de control se muestran en la ilustración siguiente:

![Diagrama de restricciones de control](../media/securing-privileged-access-reference-material/PAW_RM_Fig3.JPG)

### <a name="primary-responsibilities-and-critical-restrictions"></a>Restricciones críticas y responsabilidades principales

**Administrador de nivel 0**: administrar el almacén de identidades y un pequeño número de sistemas que lo controlan eficazmente, y:

- Puede administrar y controlar recursos en cualquier nivel según sea necesario.
- Solo hay que iniciar sesión interactivamente o acceder a recursos de confianza en el nivel 0.

**Administrador de nivel 1**: administrar servidores empresariales, servicios y aplicaciones, y:

- Solo puede administrar y controlar recursos de nivel 1 o 2.
- Solo puede acceder a los recursos (a través del tipo de inicio de sesión de red) que son de confianza en los niveles 0 o 1.
- Solo puede iniciar sesión interactivamente en recursos de confianza en el nivel 1.

**Administrador de nivel 2**: administrar escritorios empresariales, equipos portátiles, impresoras y otros dispositivos de usuario, y:

- Solo puede administrar y controlar recursos en el nivel 2
- Puede acceder a los recursos (a través del tipo de inicio de sesión de red) en cualquier nivel según sea necesario
- Solo puede iniciar sesión interactivamente en los recursos de confianza en el nivel 2

### <a name="logon-restrictions"></a>Restricciones de inicio de sesión

En la siguiente ilustración se muestran las restricciones de inicio de sesión:

![Diagrama de restricciones de inicio de sesión](../media/securing-privileged-access-reference-material/PAW_RM_Fig4.JPG)

> [!NOTE]
> Tenga en cuenta que algunos recursos pueden afectar al nivel 0 a la disponibilidad del entorno, pero no afectar directamente a la confidencialidad o integridad de los recursos. Estos incluyen el servicio del servidor DNS y dispositivos de red críticos como servidores proxy de Internet.

## <a name="clean-source-principle"></a>Principio de origen limpio

El principio de origen limpio requiere que todas las dependencias de seguridad sean tan confiables como el objeto que se protege.

![Diagrama que muestra cómo un sujeto con el control de un objeto es una dependencia de seguridad de dicho objeto](../media/securing-privileged-access-reference-material/PAW_RM_Fig5.JPG)

Cualquier sujeto con el control de un objeto es una dependencia de seguridad de dicho objeto. Si un adversario puede controlar cualquier cosa de un control efectivo de un objeto de destino, podrá controlar ese objeto de destino. Por este motivo, debe asegurarse de que las comprobaciones para todas las dependencias de seguridad están en el nivel de seguridad deseado del propio objeto, o por encima de él.

Aunque es sencillo en principio, su aplicación requiere comprender las relaciones de control de un recurso de interés (objeto) y realizar un análisis de dependencia de él para detectar todas las dependencias de seguridad (sujetos).

Como el control es transitivo, este principio debe repetirse recursivamente. Por ejemplo, si A controla a B y B controla a C, entonces A también controla indirectamente a C.

![Diagrama que muestra cómo, si A controla a B y B controla a C, entonces A también controla indirectamente a C](../media/securing-privileged-access-reference-material/PAW_RM_Fig6.JPG)

Un atacante que pone en peligro a A accede a todo lo que controla A (incluido B) y a todo lo que controla B (incluido C). Con el lenguaje de dependencias de seguridad en este mismo ejemplo, tanto B y A son dependencias de seguridad de C y deben protegerse en el nivel de seguridad deseado de C para que C tenga ese nivel de garantía.

Para los sistemas de identidad y la infraestructura de TI, este principio debe aplicarse a la forma más común de control, incluido el hardware donde se instalan los sistemas, los medios de instalación de los sistemas, la arquitectura y la configuración del sistema y las operaciones diarias.

## <a name="clean-source-for-installation-media"></a>Origen limpio para medios de instalación

![Diagrama que muestra un origen limpio para medios de instalación](../media/securing-privileged-access-reference-material/PAW_RM_CSIM.JPG)

La aplicación del principio de origen limpio a los medios de instalación requiere que garantice que los medios de instalación no se han alterado, ya que se han liberado por el fabricante (lo mejor que se pueda determinar). Esta figura muestra a un atacante que utiliza esta ruta de acceso para poner en peligro un equipo:

![Ilustración que muestra a un atacante que utiliza esta ruta de acceso para poner en peligro un equipo](../media/securing-privileged-access-reference-material/PAW_RM_Fig8.JPG)

La aplicación del principio de origen limpio a los medios de instalación requiere validar la integridad del software durante el ciclo que posee, incluso durante la adquisición, almacenamiento y transferencia hasta que se utiliza.

### <a name="software-acquisition"></a>Adquisición de software

El origen del software se debe validar a través de uno de los siguientes medios:

- El software se obtiene de medios físicos que se sabe que proceden del fabricante o de una fuente de confianza, normalmente fabricado y enviado desde un proveedor.
- El software se obtiene de Internet y se valida con los hashes de archivo proporcionado por el proveedor.
- El software procede de Internet y se valida mediante la descarga y comparación de dos copias independientes:
   - Descargue los dos hosts sin relación de seguridad (no están en el mismo dominio y no están administrados por las mismas herramientas), preferiblemente de conexiones de Internet independientes.
   - Compare los archivos descargados con una utilidad como certutil:  `certutil -hashfile <filename>`

Cuando sea posible, todo el software de aplicación, como instaladores de aplicaciones y herramientas, debe estar firmado digitalmente y comprobado con Windows Authenticode con la herramienta [Windows Sysinternals](https://www.microsoft.com/sysinternals), *sigcheck.exe*, con comprobación de revocación. Se pueden requerir algunos programas de software cuando el proveedor no pueda proporcionar este tipo de firma digital.

### <a name="software-storage-and-transfer"></a>Transferencia y almacenamiento de software

Después de obtener el software, deben almacenarse en una ubicación que esté protegida contra modificaciones, especialmente con hosts conectados a Internet o personal de confianza, en un nivel inferior a los sistemas donde se instalará el software o el sistema operativo. Este almacenamiento puede ser un medio físico o una ubicación electrónica segura.

### <a name="software-usage"></a>Uso de software

Idealmente, el software debe validarse en el momento en que se utiliza, como cuando se instala manualmente, se empaqueta para una herramienta de administración de configuración o se importa en una herramienta de administración de configuración.

## <a name="clean-source-for-architecture-and-design"></a>Origen limpio para arquitectura y diseño

La aplicación del principio de origen limpio a la arquitectura del sistema requiere que se garantice que el sistema no es dependiente en sistemas de confianza menor. Un sistema puede ser dependiente en un sistema de confianza mayor, pero no en un sistema de confianza menor con estándares de seguridad inferiores.

Por ejemplo, es aceptable que Active Directory controle un escritorio de usuario estándar, pero es una escalación importante de riesgo de privilegio que un escritorio de usuario estándar tenga el control de Active Directory.

![Diagrama que muestra cómo un sistema puede ser dependiente en un sistema de confianza mayor, pero no en un sistema de confianza menor con estándares de seguridad inferiores](../media/securing-privileged-access-reference-material/PAW_RM_Fig09.JPG)

La relación de control puede introducirse a través de diversos medios, incluida la seguridad de las listas de control de acceso (ACL) de objetos como sistemas de archivo, pertenencia al grupo de administradores local en un equipo o agentes instalados en un equipo que se ejecuta como sistema (con la capacidad de ejecutar código arbitrario y scripts).

Un ejemplo que con frecuencia se pasa por alto es la exposición mediante el inicio de sesión, que crea una relación de control al exponer las credenciales administrativas de un sistema a otro. Esta es la razón subyacente de por qué pasar los ataques de robo de credenciales como pasar el hash son muy potentes. Cuando un administrador inicia sesión en un escritorio de usuario estándar con las credenciales de nivel 0, expone dichas credenciales a ese escritorio, permitiendo el control de AD y creando un escalamiento de la ruta de privilegios a Active Directory. Para más información sobre estos ataques, consulte [esta página](https://technet.microsoft.com/security/dn785092).

Debido a la gran cantidad de recursos que dependen de sistemas de identidad como Active Directory, debe minimizar el número de sistemas que dependen de Active Directory y de controladores de dominio.

![Diagrama que muestra que se debe minimizar el número de sistemas de los que Active Directory y los controladores de dominio dependen](../media/securing-privileged-access-reference-material/PAW_RM_Fig010.JPG)

Para más información sobre la consolidación de los riesgos más importantes de Active Directory, consulte [esta página](http://aka.ms/hardenAD).

## <a name="operational-standards-based-on-clean-source-principle"></a>Estándares operativos basados en el principio de origen limpio

En esta sección se describen los estándares operativos y expectativas para el personal administrativo. Estos estándares están pensados para proteger el control administrativo de los sistemas de tecnología de información de una organización contra los riesgos que pueden crear los procesos y las prácticas operativas.

![Diagrama que muestra cómo los estándares están pensados para proteger el control administrativo de los sistemas de tecnología de información de una organización contra los riesgos que pueden crear los procesos y las prácticas operativas](../media/securing-privileged-access-reference-material/PAW_RM_Fig11.JPG)

### <a name="integrating-the-standards"></a>Integración de estándares

Puede integrar estos estándares en los estándares y procedimientos recomendados generales de su organización. Puede adaptarlos a los requisitos específicos, herramientas disponibles y deseo de riesgo de su organización, pero solo se recomiendan modificaciones mínimas para reducir el riesgo. Se recomienda usar los valores predeterminados en esta guía como referencia para el estado final ideal y administrar los diferenciales como excepciones que se abordarán en orden de prioridad.

La guía de estándares se organiza en las siguientes secciones:

- Suposiciones
- Comité de evaluación de cambios
- Prácticas operativas
   - Resumen
   - Detalles de estándares

### <a name="assumptions"></a>Suposiciones

Los estándares de esta sección asumen que la organización tiene los siguientes atributos:

- La mayoría, o todos, los servidores y estaciones de trabajo que se van a tratar están unidos a Active Directory.
- Todos los servidores que se van a administrar ejecutan Windows Server 2008 R2 o una versión posterior y tienen habilitado el modo RestrictedAdmin de RDP.
- Todas las estaciones de trabajo que se van a administrar ejecutan Windows 7 o una versión posterior y tienen habilitado el modo RestrictedAdmin de RDP.

   > [!NOTE]
   > Para habilitar el modo RestrictedAdmin de RDP, consulte [esta página](http://aka.ms/RDPRA).

- Las tarjetas inteligentes están disponibles y se emiten a todas las cuentas administrativas.
- *Builtin\Administrator* de cada dominio se ha designado como una cuenta de acceso de emergencia.
- Se implementa una solución de administración de identidades de empresa.
- [LAP](http://aka.ms/laps) se ha implementado en servidores y estaciones de trabajo para administrar la contraseña de la cuenta Administrador local
- Hay una solución instalada de administración del acceso con privilegios, como Microsoft Identity Manager, o hay un plan para adoptar una.
- Se asigna personal para supervisar las alertas de seguridad y responder a ellas.
- Está disponible la funcionalidad técnica para aplicar rápidamente las actualizaciones de seguridad de Microsoft.
- No se utilizan los controladores de administración de placa base en servidores, o se adhieren a estrictos controles de seguridad.
- Las cuentas Administrador y los grupos para servidores (administradores de nivel 1) y estaciones de trabajo (administradores de nivel 2) las administrarán los administradores de dominio (nivel 0).
- Hay un comité de evaluación de cambios (CAB) u otra autoridad designada para aprobar los cambios de Active Directory.

### <a name="change-advisory-board"></a>Comité de evaluación de cambios

Un comité de evaluación de cambios (CAB) es la autoridad de aprobación y el foro de discusión para aquellos cambios que podrían afectar al perfil de seguridad de la organización. Las excepciones a estos estándares se deben enviar al CAB con una evaluación del riesgo y su justificación.

Cada estándar de este documento se desglosa en función de su importancia en cumplir el estándar para un nivel determinado.

![Diagrama que muestra el estándar para niveles en capas determinados](../media/securing-privileged-access-reference-material/PAW_RM_Fig12.JPG)

Todas las excepciones para los elementos obligatorios (marcados con octágono rojo o un triángulo naranja en este documento) se consideran temporales y deben ser aprobadas por el CAB. Las instrucciones son:

- La solicitud inicial requiere la aceptación del riesgo de justificación firmada por el supervisor inmediato del personal y caduca al cabo de seis meses.
- Las renovaciones requieren la justificación y la aceptación del riesgo firmada por un director de unidad de negocio y caducan después de seis meses.

Todas las excepciones para los elementos recomendados (marcados con un círculo amarillo en este documento) se consideran temporales y deben ser aprobadas por el CAB. Las instrucciones son:

- La solicitud inicial requiere la aceptación del riesgo de justificación firmada por el supervisor inmediato del personal y caduca al cabo de doce meses.
- Las renovaciones requieren la justificación y la aceptación del riesgo firmada por un director de unidad de negocio y caducan después de doce meses.

### <a name="operational-practices-standards-summary"></a>Resumen de estándares de prácticas operativas

Las columnas de nivel de esta tabla hacen referencia al nivel de la cuenta administrativa, cuyo control normalmente afecta a todos los recursos de ese nivel.

![Tabla que muestra los niveles en capas de la cuenta administrativa](../media/securing-privileged-access-reference-material/PAW_RM_Fig13.JPG)

Las decisiones operativas que se realizan con regularidad son esenciales para el mantenimiento de la postura de seguridad del entorno. Estos estándares para procesos y prácticas ayudan a garantizar que un error de funcionamiento no da lugar a una vulnerabilidad operativa aprovechable en el entorno.

#### <a name="administrator-enablement-and-accountability"></a>Responsabilidad y habilitación del administrador

Los administradores deben estar informados, habilitados, formados y ser responsables para operar el entorno de la manera más segura posible.

##### <a name="administrative-personnel-standards"></a>Estándares del personal administrativo

El personal administrativo asignado debe ser examinado para asegurarse de que es de confianza y necesita privilegios administrativos:

- Realice las comprobaciones de los antecedentes del personal antes de asignar privilegios administrativos.
- Revise los privilegios administrativos cada trimestre para determinar qué personal todavía tiene una necesidad legítima de negocio para el acceso administrativo.

##### <a name="administrative-security-briefing-and-accountability"></a>Responsabilidad y sesión informativa de seguridad administrativa

Los administradores deben estar informados y ser responsables de los riesgos para la organización y de su rol en administrar ese riesgo. Los administradores deben recibir entrenamiento anual en:

- Entorno de amenazas general
   - Determinados adversarios
   - Técnicas de ataques, como pass-the-hash y el robo de credenciales
- Incidentes y amenazas específicas de la organización
- Roles de administrador para la protección contra ataques
   - Administración de la exposición de credenciales con el modelo de nivel
   - Uso de estaciones de trabajo administrativas
   - Uso del modo RestrictedAdmin de Protocolo de escritorio remoto
- Prácticas administrativas específicas de la organización
   - Revisión de todas las directrices operativas en este estándar
   - Implemente las reglas de claves siguientes:
      - No use cuentas administrativas en ningún sitio salvo en estaciones de trabajo administrativas.
      - No deshabilite ni anule los controles de seguridad en su cuenta o estaciones de trabajo (por ejemplo, las restricciones de inicio de sesión o los atributos necesarios para tarjetas inteligentes).
      - Informe de problemas o de actividad inusual.

Para proporcionar la responsabilidad, todo el personal con cuentas administrativas debe firmar un documento de código de conducta con privilegio administrativo en el que se afirma que van a seguir las prácticas de directivas administrativa específicas de la organización.

##### <a name="provisioning-and-deprovisioning-processes-for-administrative-accounts"></a>Aprovisionamiento y desaprovisionamiento de procesos para cuentas administrativas

Deben cumplirse los siguientes estándares para satisfacer los requisitos del ciclo de vida.

- Todas las cuentas administrativas deben estar aprobadas por la autoridad de aprobación que se describe en la tabla siguiente.
   - La aprobación solo debe concederse si el personal tiene una necesidad empresarial legítima de privilegios administrativos.
   - La aprobación de privilegios de administrador no debe superar los seis meses.
- El acceso a los privilegios administrativos debe ser desaprovisionado inmediatamente cuando:
   - El personal cambia de puestos de trabajo.
   - El personal deja la organización.
- Las cuentas se deben deshabilitar inmediatamente después de que el personal abandone la organización.
- Las cuentas deshabilitadas deben eliminarse en un plazo máximo de seis meses y se debe especificar el registro de su eliminación en los registros del comité de aprobación de cambios.
- Revise mensualmente todas las pertenencias de cuentas con privilegios para asegurarse de que no se les ha concedido ningún permiso no autorizado. Esto se puede reemplazar por una herramienta automatizada que alerte de los cambios.

|Nivel de privilegios de cuenta|Autoridad de aprobación|Frecuencia de la revisión de pertenencia|
|--------------|------------|----------------|
|Administrador de nivel 0|Comité de aprobación de cambios|Mensual o automatizado|
|Administrador de nivel 1|Administradores de nivel 0 o de seguridad|Mensual o automatizado|
|Administrador de nivel 2|Administradores de nivel 0 o de seguridad|Mensual o automatizado|

#### <a name="operationalize-least-privilege"></a>Operacionalización con privilegios mínimos

Estos estándares ayudan a lograr los privilegios mínimos al reducir el número de administradores en el rol y la cantidad de tiempo que tienen privilegios.

> [!NOTE]
> Lograr el mínimo privilegio en la organización requiere la descripción de los roles organizativos, sus requisitos y sus mecanismos de diseño para asegurarse de que son capaces de realizar su trabajo con privilegios mínimos. Alcanzar un estado de privilegio mínimo en un modelo administrativo requiere con frecuencia el uso de varios enfoques:
>
> - Limitar el número de administradores o miembros de grupos con privilegios
> - Delegar menos privilegios a cuentas
> - Proporcionar los privilegios limitados en tiempo a petición
> - Permitir que otros miembros del personal realicen tareas (un enfoque de soporte)
> - Proporcionar procesos para escenarios de uso poco frecuente y acceso de emergencia

##### <a name="limit-count-of-administrators"></a>Límite en el número de administradores

Debe asignarse un mínimo de dos miembros del personal cualificados a cada rol administrativo para garantizar la continuidad del negocio.

Si el personal asignado a cualquier función es superior a dos, el comité de aprobación de cambios debe aprobar las razones específicas para asignar privilegios a cada miembro individual (incluidos los dos originales). La justificación para la aprobación debe incluir:

- ¿Qué tareas técnicas realizan los administradores que requieren privilegios administrativos?
- Frecuencia de realización de las tareas
- El motivo específico por el que las tareas no pueden realizar por otro administrador en su nombre
- Documentación de todos los enfoques alternativos conocidos a conceder los privilegios y por qué cada uno de ellos no es aceptable

##### <a name="dynamically-assign-privileges"></a>Asignar dinámicamente los privilegios

Los administradores deben obtener permisos "just-in-time" para utilizarlos cuando realizan las tareas. Ningún permiso se asignará permanentemente a las cuentas administrativas.

> [!NOTE]
> Los privilegios administrativos asignados de forma permanente crean naturalmente una estrategia de "máximo privilegio", porque el personal administrativo requiere un acceso rápido a los permisos para mantener la disponibilidad operativa si hay un problema. Los permisos Just-In-Time proporcionan la capacidad de:
>
> - Asignar permisos de forma más granular, acercándose a los privilegios mínimos
> - Reducir el tiempo de exposición de los privilegios
> - Realizar el seguimiento del uso de los permisos para detectar el abuso o ataques

#### <a name="manage-risk-of-credential-exposure"></a>Administrar el riesgo de exposición de credenciales

Utilice los siguientes procedimientos para administrar correctamente el riesgo de exposición de credenciales.

##### <a name="separate-administrative-accounts"></a>Cuentas administrativas separadas

Todos los usuarios que están autorizados a poseer privilegios administrativos deben tener cuentas separadas para funciones administrativas que son distintas de las cuentas de usuario.

- **Cuentas de usuario estándar**: privilegios de usuario estándar concedidos para las tareas de usuario estándar, como correo electrónico, exploración web y el uso de aplicaciones de línea de negocio. Estas cuentas no deben tener privilegios administrativos.
- **Cuentas administrativas**: cuentas separadas creadas para el personal que está asignado a los privilegios administrativos apropiados. El administrador que tiene que administrar los recursos de cada nivel debe tener una cuenta independiente para cada nivel. Estas cuentas no deben tener ningún acceso al correo electrónico o a Internet pública.

##### <a name="administrator-logon-practices"></a>Procedimientos de inicio de sesión de administrador

Antes de que un administrador pueda iniciar sesión en un host de forma interactiva (localmente a través de RDP estándar, mediante el uso de RunAs o mediante la consola de virtualización), ese host debe cumplir o superar el estándar para el nivel de la cuenta Administrador (o un nivel superior).

Los administradores solo pueden iniciar sesión en estaciones de trabajo de administración con sus cuentas administrativas. Los administradores solo inician sesión en los recursos administrados mediante la tecnología de soporte aprobada que se describe en la sección siguiente.

> [!NOTE]
> Esto es necesario porque el inicio de sesión en un host concede de forma interactiva el control de las credenciales a ese host.
>
> Consulte la sección [Herramientas administrativas y tipos de inicio de sesión](http://aka.ms/admintoolsecurity) para más información sobre los tipos de inicio de sesión, herramientas de administración comunes y exposición de credenciales.

##### <a name="use-of-approved-support-technology-and-methods"></a>Uso de métodos y de tecnología de soporte aprobados

Los administradores que admiten usuarios y sistemas remotos deben seguir estas directrices para evitar que un adversario que tenga el control del equipo remoto robe sus credenciales administrativas.

- Deben utilizarse las opciones de soporte técnico principal si están disponibles.
- Las opciones de soporte técnico secundario solo deben usarse si no está disponible la opción de soporte principal.
- Nunca se pueden utilizar métodos de soporte técnico prohibido.
- Sin explorar Internet o acceder al correo electrónico por cualquier cuenta administrativa en cualquier momento.

###### <a name="tier-0-forest-domain-and-dc-administration"></a>Bosque de nivel 0, dominio y administración de DC

Asegúrese de que se aplican las prácticas siguientes a este escenario:

- **Soporte técnico del servidor remoto**: al acceder de manera remota a un servidor, los administradores de nivel 0 deben seguir estas directrices:
  - **Principal (herramienta)** : herramientas remotas que usan inicios de sesión de red (tipo 3). Para más información, consulte [Herramientas administrativas y tipos de inicio de sesión](http://aka.ms/admintoolsecurity).
  - **Principal (interactiva)** : use RDP RestrictedAdmin o una sesión RDP estándar desde una estación de trabajo de administración con una cuenta de dominio

    > [!NOTE]
    > Si tiene una solución de administración con privilegios de nivel 0, agregue "que usa los permisos obtuvieron just-in-time de una solución de administración del acceso con privilegios".

- **Soporte técnico del servidor físico**: cuando estén físicamente presentes en la consola de servidor o en una consola de máquina virtual (herramientas de Hyper-V o VMWare), estas cuentas no tienen restricciones de uso de herramientas administrativas específicas, solo las restricciones generales de las tareas de usuario estándar, como correo electrónico y exploración en Internet abierta.

   > [!NOTE]
   > La administración de nivel 0 es diferente de la administración de otros niveles porque todos los recursos de nivel 0 ya tienen control directo o indirecto de todos los recursos. Por ejemplo, un atacante que tenga el control de un controlador de dominio no tiene necesidad de robar las credenciales de administradores que han iniciado sesión ya que tienen acceso a todas las credenciales del dominio en la base de datos.

###### <a name="tier-1-server-and-enterprise-application-support"></a>Soporte técnico de aplicaciones empresariales y servidor de nivel 1

Asegúrese de que se aplican las prácticas siguientes a este escenario:

- **Soporte técnico del servidor remoto**: al acceder de manera remota a un servidor, los administradores de nivel 1 deben seguir estas directrices:
   - **Principal (herramienta)** : herramientas remotas que usan inicios de sesión de red (tipo 3). Para más información, consulte [Mitigating Pass-the-Hash and Other Credential Theft](https://www.microsoft.com/pth) v1 (pp 42 47) (Mitigación de Pass-the-Hash y otros robos de credenciales).
   - **Principal (interactiva)** : use RestrictedAdmin de RDP en una estación de trabajo de administración con una cuenta de dominio que usa permisos obtenidos just-in-time en una solución de administración del acceso con privilegios.
   - **Secundario**: inicie sesión en el servidor mediante una contraseña de cuenta local se establece LAPS desde una estación de trabajo de administración.
   - **Prohibido**: el RDP estándar no puede usarse con una cuenta de dominio.
   - **Prohibido**: con las credenciales de la cuenta de dominio mientras se encuentra en la sesión (por ejemplo, mediante *RunAs* o la autenticación de un recurso compartido). Esto expone las credenciales de inicio de sesión a un riesgo de robo.
- **Soporte técnico del servidor físico**: cuando estén físicamente presentes en una consola de un servidor o en una consola de máquina virtual (herramientas de Hyper-V o VMWare), los administradores de nivel 1 deben recuperar la contraseña de la cuenta local desde LAPS antes acceder al servidor.
   - **Principal**: recupere la contraseña de cuenta local establecida por LAPS desde una estación de trabajo de administración antes de iniciar sesión en el servidor.
   - **Prohibido**: no se permite el inicio de sesión con una cuenta de dominio en este escenario.
   - **Prohibido**: con las credenciales de la cuenta de dominio mientras se encuentra en la sesión (por ejemplo, RunAs o autenticación de un recurso compartido). Esto expone las credenciales de inicio de sesión a un riesgo de robo.

###### <a name="tier-2-help-desk-and-user-support"></a>Departamento de soporte técnico y soporte de usuario de nivel 2

Las organizaciones de soporte técnico de usuario y el departamento de soporte técnico realizan soporte técnico para los usuarios finales (que no requieren privilegios administrativos) y para las estaciones de trabajo de usuario (que requieren privilegios administrativos).

**Soporte técnico de usuario**: entre las tareas se incluyen la ayuda a los usuarios a realizar tareas que no requieren ninguna modificación en la estación de trabajo, mostrándoles con frecuencia cómo utilizar una característica de la aplicación o una característica del sistema operativo.

- **Soporte técnico del usuario en el puesto de trabajo**: el personal de soporte técnico del nivel 2 está físicamente en el área de trabajo del usuario.
   - **Principal**: se puede proporcionar soporte con "consentimiento temporal" sin herramientas.
   - **Prohibido**: no se permite en este escenario iniciar sesión con credenciales administrativas de la cuenta de dominio. Cambie el soporte de la estación de trabajo del puesto de trabajo si se requieren privilegios administrativos.
- **Soporte técnico de usuario remoto**: el personal de soporte técnico de nivel 2 está físicamente alejado del usuario.
   - **Principal**: se puede usar la asistencia remota, Skype empresarial o uso compartido de pantalla de usuario similar. Para más información, consulte [¿Qué es la asistencia remota de Windows?](https://windows.microsoft.com/en-us/windows/what-is-windows-remote-assistance)
   - **Prohibido**: no se permite en este escenario iniciar sesión con credenciales administrativas de la cuenta de dominio. Cambie al soporte técnico de la estación de trabajo si se requieren privilegios administrativos.
- **Soporte técnico de la estación de trabajo**: las tareas incluyen el mantenimiento de la estación de trabajo o la solución de problemas que requieren acceso a un sistema para ver los registros, instalar software o actualizar controladores, entre otras.
   - **Soporte técnico de la estación de trabajo en el puesto de trabajo**: el personal de soporte técnico del nivel 2 está físicamente en la estación de trabajo del usuario.
      - **Principal**: recupere la contraseña de cuenta local establecida por LAPS desde una estación de trabajo de administración antes de conectarse a la estación de trabajo del usuario.
      - **Prohibido**: no se permite en este escenario iniciar sesión con credenciales administrativas de la cuenta de dominio.
   - **Soporte técnico de la estación de trabajo remota**: el personal de soporte técnico del nivel 2 está físicamente alejado de la estación de trabajo.
      - **Principal**: use RestrictedAdmin de RDP desde una estación de trabajo de administración con una cuenta de dominio que usa permisos obtenidos just-in-time de una solución de administración del acceso con privilegios.
      - **Secundario**: recupere una contraseña de cuenta local establecida por LAPS desde una estación de trabajo de administración antes de conectarse a la estación de trabajo del usuario.
      - **Prohibido**: use RDP estándar con una cuenta de dominio.

###### <a name="no-browsing-the-public-internet-with-admin-accounts-or-from-admin-workstations"></a>Sin explorar Internet pública con cuentas de administrador o desde estaciones de administración

El personal administrativo no puede explorar Internet abierta mientras inicia sesión con una cuenta administrativa o mientras ha iniciado sesión en una estación de trabajo administrativa. Las únicas excepciones autorizadas son el uso de un explorador web para administrar un servicio basado en la nube.

###### <a name="no-accessing-email-with-admin-accounts-or-from-admin-workstations"></a>Sin acceder al correo electrónico con las cuentas de administrador o con estaciones de trabajo de administración

El personal administrativo no puede acceder al correo electrónico mientras inicia sesión con una cuenta administrativa o mientras inicia sesión en una estación de trabajo administrativa.

###### <a name="store-service-and-application-account-passwords-in-a-secure-location"></a>Almacenar las contraseñas de la cuenta de servicio y de aplicación en una ubicación segura

Las siguientes directrices se deben usar para los procesos de seguridad física que controlan el acceso a la contraseña:

- Guarde las contraseñas de cuenta de servicio en lugar seguro físico.
- Asegúrese de que solo el personal de confianza en o por encima de la clasificación de nivel de la cuenta tiene acceso a la contraseña de cuenta.
- Limite el número de personas que tienen acceso a las contraseñas al mínimo por responsabilidad.
- Asegúrese de que todo el acceso a la contraseña lo ha registrado, seguido y supervisado una entidad desinteresada, por ejemplo, un administrador que no está capacitado para realizar la administración de TI.

##### <a name="strong-authentication"></a>Autenticación segura

Utilice los procedimientos siguientes para configurar correctamente una autenticación segura.

###### <a name="enforce-smartcard-multi-factor-authentication-mfa-for-all-admin-accounts"></a>Forzar Multi-Factor Authentication (MFA) de tarjeta inteligente para todas las cuentas de administración

No se permite a ninguna cuenta administrativa usar una contraseña para la autenticación. Las únicas excepciones autorizadas son las cuentas de acceso de emergencia que están protegidas por los procesos adecuados.

Vincule todas las cuentas administrativas con una tarjeta inteligente y habilite el atributo "**La tarjeta inteligente es necesaria para un inicio de sesión interactivo**".

Debe implementarse un script para restablecer automática y periódicamente el valor de hash de la contraseña aleatoria mediante la deshabilitación y habilitación inmediata del atributo "**La tarjeta inteligente es necesaria para un inicio de sesión interactivo**".

No permita excepciones para las cuentas utilizadas por el personal más allá de las cuentas de acceso de emergencia.

###### <a name="enforce-multi-factor-authentication-for-all-cloud-admin-accounts"></a>Forzar Multi-Factor Authentication para todas las cuentas de administración de la nube

Todas las cuentas con privilegios administrativos en un servicio en la nube, como Microsoft Azure y Office 365, deben utilizar la autenticación multifactor.

##### <a name="rare-use-emergency-procedures"></a>Procedimientos de emergencia de uso poco frecuente

Las prácticas operativas deben admitir los estándares siguientes:

- Asegúrese de que las interrupciones se resuelven rápidamente.
- Asegúrese de que se pueden completar tareas poco frecuentes de alto nivel de privilegios según sea necesario.
- Asegúrese de que se usan procedimientos seguros para proteger las credenciales y los privilegios.
- Asegúrese de que se siguen los procesos de aprobación y seguimiento adecuados.

###### <a name="correctly-follow-appropriate-processes-for-all-emergency-access-accounts"></a>Seguir correctamente los procesos adecuados para todas las cuentas de acceso de emergencia

Asegúrese de que cada cuenta de acceso de emergencia tiene una hoja de seguimiento en el lugar seguro.

Debe seguir el procedimiento documentado en la hoja de seguimiento de contraseñas para cada cuenta, que incluye cambiar la contraseña después de cada uso y cerrar sesión en las estaciones de trabajo o servidores tras la finalización.

Todo el uso de las cuentas de acceso de emergencia deben ser aprobadas por el comité de aprobación de cambios por adelantado o a posteriori como un uso de emergencia aprobado.

###### <a name="restrict-and-monitor-usage-of-emergency-access-accounts"></a>Restringir y supervisar el uso de cuentas de acceso de emergencia

Para usar todas las cuentas de acceso de emergencia:

- Los administradores de dominio autorizado solo pueden tener acceso a las cuentas de acceso de emergencia con privilegios de administrador de dominio.
- Las cuentas de acceso de emergencia únicamente pueden utilizarse en controladores de dominio y otros hosts de nivel 0.
- Esta cuenta debe usarse solo para:
  - Realizar la solución de problemas y la corrección de los problemas técnicos que impiden el uso de las cuentas administrativas correctas.
  - Realizar tareas poco frecuentes, como:
    - Administración de esquema
    - Tareas de todo el bosque que requieren privilegios de administrador de empresa

      > [!NOTE]
      > Administración de topologías, incluida la administración de sitios y subredes de Active Directory se delega a limitar el uso de estos privilegios.

- El uso de una de estas cuentas debería contar con autorización escrita por el responsable del grupo de seguridad
- El procedimiento de la hoja de seguimiento para cada cuenta de acceso de emergencia requiere que la contraseña se cambie en cada uso. Un miembro del equipo de seguridad debe validar que se ha realizado correctamente.

###### <a name="temporarily-assign-enterprise-admin-and-schema-admin-membership"></a>Asignar temporalmente la pertenencia del administrador del esquema y del administrador de empresa

Deben agregarse privilegios cuando se precise y eliminarse después. La cuenta de emergencia debe tener estos privilegios asignados mientras la tarea se complete y durante un máximo de 10 horas. Se debe capturar todo el uso y la duración de estos privilegios en el registro de comité de aprobación de cambios una vez completada la tarea.

## <a name="esae-administrative-forest-design-approach"></a>Enfoque de diseño de bosque administrativo ESAE

Esta sección contiene un enfoque para un bosque administrativo basado en la arquitectura de referencia mejorada del entorno administrativo de seguridad mejorada (ESAE) implementado por los equipos de servicios profesionales de ciberseguridad de Microsoft para proteger a los clientes frente a ataques de ciberseguridad.

Los bosques administrativos dedicados permiten que las organizaciones hospeden cuentas administrativas, estaciones de trabajo y grupos en un entorno que tiene controles de seguridad más seguros que los del entorno de producción.

Esta arquitectura permite contar con diversos controles de seguridad que no serían posibles o que no podrían configurarse fácilmente en una arquitectura de un solo bosque, incluso uno administrado con Estaciones de trabajo de acceso con privilegios (PAW). Este enfoque permite el aprovisionamiento de cuentas como usuarios estándar sin privilegios en el bosque administrativo y que cuentan con altos privilegios en el entorno de producción, lo que permite un mayor cumplimiento técnico de la gobernanza. Esta arquitectura también permite usar la característica de autenticación selectiva de una confianza como un medio de restringir los inicios de sesión (y la exposición de credenciales) solo a los hosts autorizados. En situaciones en las que se desea un mayor nivel de control para el bosque de producción sin el costo y la complejidad que implica una recompilación total, un bosque administrativo puede proporcionar un entorno que aumente el nivel de control del entorno de producción.

Aunque este enfoque agrega un bosque a un entorno de Active Directory, el costo y la complejidad están limitados por el diseño fijo, la superficie pequeña de hardware y software y un pequeño número de usuarios.

> [!NOTE]
> Este enfoque funciona bien para administrar Active Directory, pero muchas aplicaciones no son compatibles con la que están administrando las cuentas de un bosque externo mediante una relación de confianza estándar.

Esta ilustración muestra un bosque ESAE usado para la administración de recursos de nivel 0 y un bosque PRIV configurado para su uso con la funcionalidad Privileged Access Management de Microsoft Identity Manager. Para más información acerca de cómo implementar una instancia de MIM PAM, consulte el artículo [Privileged Identity Management para Active Directory Domain Services (AD DS)](https://technet.microsoft.com/library/mt150258.aspx).

![Ilustración que muestra un bosque ESAE usado para la administración de recursos de nivel 0 y un bosque PRIV configurado para su uso con la funcionalidad Privileged Access Management de Microsoft Identity Manager.](../media/securing-privileged-access-reference-material/PAW_RM_Fig14.JPG)

Un bosque administrativo dedicado es un bosque de Active Directory de dominio único estándar dedicado a la función de administración de Active Directory. Los dominios y bosques administrativos se pueden proteger más rigurosamente que los bosques de producción, debido a los casos de uso limitados.

El diseño de un bosque administrativo debe incluir las consideraciones siguientes:

- **Ámbito limitado**: el valor principal de un bosque administrativo es el gran nivel de aseguramiento de calidad y una menor superficie expuesta a ataques, lo que genera un menor riesgo residual. El bosque se puede usar para albergar aplicaciones y funciones de administración adicionales, pero cada aumento en el ámbito aumentará la superficie del bosque y sus recursos expuesta a ataques. El objetivo es limitar las funciones del bosque y los usuarios administradores existentes para que la superficie expuesta a ataques sea la mínima posible; por tanto, cada aumento del ámbito se debe considerar cuidadosamente.
- **Configuraciones de confianza**: configure la confianza desde los dominios o bosques administrados al bosque administrativo.
   - Se requiere una confianza unidireccional desde el entorno de producción al bosque administrativo. Puede tratarse de una confianza de dominio o una confianza de bosque. No es necesario que el dominio del bosque administrativo confíe en los bosques o dominios administrados para poder administrar Active Directory; sin embargo, algunas aplicaciones adicionales podrían requerir una relación de confianza bidireccional, validación de la seguridad y prueba.
   - Se debe utilizar la autenticación selectiva para restringir las cuentas en el bosque administrativo para que solo inicie sesión en los hosts de producción adecuados. Para mantener los controladores de dominio y delegar derechos en Active Directory, normalmente se requiere otorgar el derecho "Se permite el inicio de sesión" de los controladores de dominio a las cuentas de administración designadas de nivel 0 en el bosque administrativo. Consulte Configuración de la autenticación selectiva para más información.
- **Privilegios y sistema de protección de dominios:** el bosque administrativo debe estar configurado en el menor nivel de privilegio según los requisitos para la administración de Active Directory.
   - La concesión de derechos para administrar controladores de dominio y delegar permisos requiere agregar cuentas de administrador de bosque al grupo local de dominio BUILTIN\Administrators. Esto es debido a que el grupo global de administradores del dominio no puede tener miembros de un dominio externo.
   - Una salvedad al uso de este grupo para conceder derechos es que, de forma predeterminada, no tiene acceso administrativo a los nuevos objetos de directiva de grupo. Esto se puede cambiar siguiendo el procedimiento descrito en [este artículo de Knowledge Base](https://support.microsoft.com/kb/321476) para cambiar los permisos predeterminados del esquema.
   - Las cuentas existentes en el bosque administrativo que se usan para administrar el entorno de producción no deben contar con privilegios administrativos para el bosque administrativo o los dominios o estaciones de trabajo que existen en él.
   - Un proceso sin conexión controlará rigurosamente los privilegios administrativos sobre el bosque administrativo, con el fin de disminuir la posibilidad de que un atacante o un infiltrado malintencionado borre los registros de auditoría. Esto también ayuda a asegurarse de que el personal con cuentas de administración de producción no flexibilice las restricciones sobre sus cuentas y aumente el riesgo para la organización.
   - El bosque administrativo debe respetar las configuraciones de línea de base de cumplimiento de seguridad de Microsoft (SCB) para el dominio, incluidas configuraciones seguras para los protocolos de autenticación.
   - Las actualizaciones de seguridad se deben aplicar automáticamente a todos los hosts del bosque administrativo. Aunque esto puede generar un riesgo de interrumpir las operaciones de mantenimiento de los controladores de dominio, permite mitigar, en gran medida, el riesgo de seguridad de las vulnerabilidades a las que no se aplicó una revisión.

      > [!NOTE]
      > Es posible configurar una instancia de Windows Server Update Services dedicada para que apruebe automáticamente las actualizaciones. Para más información, consulte la sección "Aprobación automática de las actualizaciones para instalación" en Aprobación de las actualizaciones.

- **Sistema de protección de estación de trabajo**: cree las estaciones de trabajo administrativas mediante las [estaciones de trabajo de acceso con privilegios](../securing-privileged-access/privileged-access-workstations.md) (a través de la fase 3), pero cambie la pertenencia al dominio en el bosque administrativo en lugar de en el entorno de producción.
- **Servidor y sistema de protección del controlador de dominio**: para todos los controladores de dominio y servidores del bosque administrativo:
   - Asegúrese de que todos los medios se validen siguiendo la [guía de origen limpio para medios de instalación](http://aka.ms/cleansource).
   - Asegúrese de que los servidores de bosque administrativos tengan los últimos sistemas operativos instalados, aunque esto no sea posible en producción.
   - Las actualizaciones de seguridad se deben aplicar automáticamente a los hosts del bosque administrativo.

      > [!NOTE]
      > Es posible configurar Windows Server Update Services para que apruebe automáticamente las actualizaciones. Para más información, consulte la sección "Aprobación automática de las actualizaciones para instalación" en Aprobación de las actualizaciones.

   - Las líneas base de seguridad se deben usar como configuraciones iniciales.

      > [!NOTE]
      > Los clientes pueden usar el Kit de herramientas de compatibilidad de seguridad (SCT) de Microsoft para configurar las líneas de base en los hosts administrativos.

   - Arranque seguro, para mitigar el riesgo de atacantes o malware que intenten cargar código sin firma en el proceso de arranque.

      > [!NOTE]
      > Esta característica se presentó en Windows 8 para usar Unified Extensible Firmware Interface (UEFI).

   - Cifrado del volumen completo, para mitigar la pérdida física de equipos, como equipos portátiles administrativos que se usan de forma remota.

      > [!NOTE]
      > Para más información, consulte [BitLocker](https://technet.microsoft.com/library/dn641993.aspx).

   - Restricciones USB, para proteger contra vectores de infección físicos.

      > [!NOTE]
      > Consulte [Control Read or Write Access to Removable Devices or Media](https://technet.microsoft.com/library/cc730808(v=ws.10).aspx) (Control de acceso de lectura o escritura en dispositivos o medios extraíbles) para más información.

   - Aislamiento de la red, para proteger contra ataques a la red y acciones administrativas involuntarias. Los firewalls de host deben bloquear todas las conexiones entrantes, excepto las que se requieren de forma explícita, y bloquear todo el acceso a Internet saliente.

   - Antimalware, para proteger contra malware y amenazas conocidas.

   - Análisis de la superficie expuesta a ataques, para evitar el ingreso de nuevos vectores de ataque a Windows durante la instalación de software nuevo.

      > [!NOTE]
      > El uso de herramientas como el [analizador de superficie expuesta a ataques (ASA)](https://www.microsoft.com/en-us/download/details.aspx?id=24487) ayudará a evaluar las opciones de configuración de un host y a identificar los vectores de ataque que ingresaron debido a los cambios en la configuración o un software.

- Sistema de protección de la cuenta
   - Debe configurarse la autenticación multifactor para todas las cuentas en el bosque de administración, excepto una cuenta. Al menos una cuenta administrativa debe estar basada en contraseña para asegurar que el acceso funcionará en caso de que se rompa el proceso de la autenticación multifactor. Esta cuenta debe protegerse mediante un estricto proceso de control físico.
   - Las cuentas configuradas para la autenticación multifactor deben configurarse regularmente para establecer un nuevo hash de NTLM en las cuentas. Esto se puede realizar mediante la deshabilitación y habilitación del atributo La tarjeta inteligente es necesaria para un inicio de sesión interactivo.

      > [!NOTE]
      > Esto puede interrumpir las operaciones en curso que usan esta cuenta, por lo que el proceso solo se debe iniciar cuando los administradores no va a usar la cuenta, como por la noche o los fines de semana.

- Controles de detección
   - Se deben diseñar controles de detección para el bosque administrativo con el fin de alertar anomalías existentes en el bosque administrativo. La cantidad limitada de actividades y escenarios autorizados puede ayudar a ajustar estos controles de manera más precisa que en el entorno de producción.

Para más información concerniente a los servicios de Microsoft que se usan para diseñar e implementar un ESAE para su entorno, consulte [esta página](https://www.microsoft.com/services).

## <a name="tier-0-equivalency"></a>Equivalencia de nivel 0

La mayoría de las organizaciones controlan la pertenencia a grupos de Active Directory de nivel 0 eficaces, como administradores, administradores de dominio y administradores de empresa.  Muchas organizaciones pasan por alto el riesgo de otros grupos que son eficazmente equivalentes con privilegios de un entorno típico de Active Directory. Estos grupos ofrecen una ruta de acceso de escalamiento relativamente sencilla para un atacante los mismos privilegios de nivel 0 explícitos mediante varios métodos de ataque diferentes.

Por ejemplo, un operador de servidor podría obtener acceso a un medio de copia de seguridad de un controlador de dominio y extraer todas las credenciales de los archivos de ese medio y usarlas para aumentar los privilegios.

Las organizaciones deben controlar y supervisar la pertenencia en todos los grupos de nivel 0 (incluida la pertenencia anidada), incluyendo:

- Administradores de empresas
- Admins. del dominio
- Administrador de esquema
- BUILTIN\Administrators
- Operadores de cuentas
- Operadores de copia de seguridad
- Opers. de impresión
- Operadores de servidores
- Controladores de dominio
- Controladores de dominio de solo lectura
- Propietarios del creador de directivas de grupo
- Operadores criptográficos
- Usuarios de COM distribuido
- Otros grupos delegados - grupos personalizados que pueden ser creados por la organización para administrar las operaciones de directorio que también pueden tener acceso de nivel 0 efectivo.

## <a name="administrative-tools-and-logon-types"></a>Herramientas administrativas y tipos de inicio de sesión

Se trata de información de referencia para ayudar a identificar el riesgo de exposición de la credencial asociada con el uso de diferentes herramientas administrativas para la administración remota.

En un escenario de administración remota, las credenciales siempre se exponen en el equipo de origen, de tal forma que siempre se recomienda una estación de trabajo de acceso con privilegios de confianza (PAW) para las cuentas confidenciales o de alto impacto. Si las credenciales están expuestas a posibles robos en el equipo de destino (remoto), depende principalmente del tipo de inicio de sesión de Windows utilizado por el método de conexión.

En esta tabla se incluye orientación para las herramientas administrativas más comunes y los métodos de conexión:

|Método de conexión|Tipo de inicio de sesión|Credenciales reutilizables en destino|Observaciones|
|-----------|-------|--------------------|------|
|Iniciar sesión en consola|Interactive (Interactivo)|v|Incluye el acceso remoto de hardware y apaga los KVM de red y de tarjetas.|
|RUNAS|Interactive (Interactivo)|v||
|RUNAS /NETWORK|NewCredentials|v|Clona la sesión actual de LSA para el acceso local, pero usa nuevas credenciales al conectarse a recursos de red.|
|Escritorio remoto (correcto)|RemoteInteractive|v|Si el cliente de Escritorio remoto está configurado para compartir recursos y dispositivos locales, también pueden verse comprometidos.|
|Escritorio remoto (error: el tipo de inicio de sesión se ha denegado)|RemoteInteractive|-|De forma predeterminada, si se produce un error de sesión de RDP, solo se almacena durante un breve período. Esto puede no ser el caso si el equipo está en peligro.|
|NET use * \\\SERVER|Red|-||
|Net use * \\\SERVER /u:user|Red|-||
|Complementos MMC para un equipo remoto|Red|-|Por ejemplo: Equipo de administración, el Visor de eventos, Administrador de dispositivos, servicios|
|PowerShell WinRM|Red|-|Por ejemplo: Enter-PSSession server|
|PowerShell WinRM con CredSSP|NetworkClearText|v|New-PSSession<br />-Authentication Credssp<br />-Credential cred|
|PsExec sin credenciales explícitas|Red|-|Por ejemplo: PsExec \\\server cmd|
|PsExec con credenciales explícitas|Red + interactivo|v|PsExec \\\server -u user -p pwd cmd<br />Crea varias sesiones de inicio de sesión.|
|Registro remoto|Red|-||
|Puerta de enlace de Escritorio remoto|Red|-|Autenticación en Puerta de enlace de Escritorio remoto.|
|Tarea programada|Batch|v|La contraseña también se guardará como secreto de LSA en el disco.|
|Ejecutar herramientas como un servicio|Servicio|v|La contraseña también se guardará como secreto de LSA en el disco.|
|Detectores de vulnerabilidades|Red|-|La mayoría de los detectores usan de forma predeterminada los inicios de sesión de red, aunque algunos proveedores pueden implementar inicios de sesión sin conexión de red y presentan mayor riesgo de robo de credenciales.|

Para la autenticación web, utilice la referencia de la tabla siguiente:

|Método de conexión|Tipo de inicio de sesión|Credenciales reutilizables en destino|Observaciones|
|-----------|-------|--------------------|------|
|IIS "Autenticación básica"|NetworkCleartext<br />(IIS 6.0+)<br /><br />Interactive (Interactivo)<br />(anteriores a IIS 6.0)|v||
|IIS "Autenticación de Windows integrada"|Red|-|Proveedores de NTLM y Kerberos.|

Definiciones de columna:

- **Tipo de inicio de sesión** identifica el tipo de inicio de sesión iniciado por la conexión.
- **Credenciales reutilizables en destino** indica que los siguientes tipos de credenciales se almacenarán en la memoria de proceso LSASS en el equipo de destino, donde la cuenta especificada ha iniciado sesión localmente:
   - Hashes de LM y NT
   - TGT de Kerberos
   - Contraseña en texto sin formato (si corresponde).

Los símbolos de esta tabla se definen como sigue:

- (-) indica cuándo las credenciales no están expuestas.
- (v) indica cuándo las credenciales están expuestas.

Para las aplicaciones de administración que no están en esta tabla, puede determinar el tipo de inicio de sesión en el campo de tipo de inicio de sesión en los eventos de inicio de sesión de auditoría. Para más información, consulte [Auditar sucesos de inicio de sesión](https://technet.microsoft.com/library/cc787567(v=ws.10).aspx).

En los equipos basados en Windows, se procesan todas las autenticaciones como uno de los distintos tipos de inicio de sesión, con independencia del protocolo de autenticación o autenticador que se utilice. En esta tabla se incluyen los tipos más comunes de inicio de sesión y sus atributos en relación con el robo de credenciales:

|Tipo de inicio de sesión|#|Autenticadores aceptados|Credenciales reutilizables en sesión LSA|Ejemplos|
|-------|---|--------------|--------------------|------|
|Interactivo (conocido como inicio de sesión local)|2|Contraseña, tarjeta inteligente,<br />otro|Sí|Inicio de sesión de la consola;<br />RUNAS;<br />Soluciones de control remoto de hardware (como KVM de red o Acceso remoto / apagado de tarjeta en el servidor)<br />Autenticación básica de IIS (antes de IIS 6.0)|
|Red|3|Contraseña,<br />Hash de NT,<br />Vale de Kerberos|N (excepto si la delegación está habilitada, vales de Kerberos presentes)|USO DE NET;<br />Llamadas RPC;<br />Registro remoto;<br />Autenticación de Windows integrada IIS;<br />Autenticación de Windows SQL;|
|Batch|4|Contraseña (almacenada normalmente como secreto de LSA)|Sí|Tareas programadas|
|Servicio|5|Contraseña (almacenada normalmente como secreto de LSA)|Sí|Servicios de Windows|
|NetworkCleartext|8|Contraseña|Sí|Autenticación básica de IIS (IIS 6.0 y posterior);<br />Windows PowerShell con CredSSP|
|NewCredentials|9|Contraseña|Sí|RUNAS /NETWORK|
|RemoteInteractive|10|Contraseña, tarjeta inteligente,<br />otro|Sí|Escritorio remoto (anteriormente conocido como "Terminal Services")|

Definiciones de columna:

- **Tipo de inicio de sesión** es el tipo de inicio de sesión solicitado.
- **#** es el identificador numérico para el tipo de inicio de sesión que se notifica los eventos de auditoría en el registro de eventos de seguridad.
- **Autenticadores aceptados** indica qué tipos de autenticadores podrán iniciar sesión de este tipo.
- **Credenciales reutilizables** en la sesión de LSA indica si el tipo de inicio de sesión se produce en la sesión LSA que contiene las credenciales, como contraseñas de texto sin formato, hashes de NT o vales de Kerberos se podrían usar para autenticarse en otros recursos de red.
- **Ejemplos** muestra los escenarios comunes en los que se utiliza el tipo de inicio de sesión.

> [!NOTE]
> Para más información acerca de los tipos de inicio de sesión, consulte [Enumeración SECURITY_LOGON_TYPE](https://technet.microsoft.com/library/aa380129(VS.85).aspx).
