---
ms.assetid: 50bd2566-e03c-4884-b5c4-895c8aab80aa
title: Determinación de los participantes en el proyecto de implementación
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 962825494c613dfef808ce54dfba22727abf6878
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834656"
---
# <a name="identifying-the-deployment-project-participants"></a>Determinación de los participantes en el proyecto de implementación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Es el primer paso para establecer un proyecto de implementación de servicio de dominio de Active Directory (AD DS) establecer el diseño y el ciclo de equipos de proyecto de implementación serán responsables de administrar la fase de diseño y la fase de implementación del proyecto de Active Directory. Además, debe identificar los usuarios y grupos que serán responsables de la propiedad y mantenimiento del directorio de una vez completada la implementación.  
  
-   [Definición de las funciones específicas del proyecto](#BKMK_1)  
  
-   [Establecimiento de los propietarios y administradores](#BKMK_2)  
  
-   [Equipos de proyecto de creación](#BKMK_3)  
  
## <a name="BKMK_1"></a>Definición de las funciones específicas del proyecto  
Es un paso importante en el establecimiento de los equipos de proyecto identificar a las personas que van a contener roles específicos del proyecto. Estos incluyen el patrocinador ejecutivo, el arquitecto del proyecto y el jefe de proyecto. Estos individuos responsables de ejecutar el proyecto de implementación de Active Directory.  
  
Después de que designe el arquitecto del proyecto y el jefe de proyecto, estos individuos establecer canales de comunicación a través de la organización, crear planes de proyecto e identificar las personas que sean miembros de los equipos de proyecto, empezando por el distintos propietarios.  
  
### <a name="executive-sponsor"></a>Patrocinador ejecutivo  
Implementar una infraestructura de AD DS puede tener un impacto de amplio alcance en una organización. Por este motivo, es importante contar con un patrocinador ejecutivo que comprenda el valor empresarial de la implementación, es compatible con el proyecto en el nivel ejecutivo y puede ayudar a resolver los conflictos en toda la organización.  
  
### <a name="project-architect"></a>Arquitecto de proyectos  
Cada proyecto de implementación de Active Directory requiere un arquitecto de proyecto administrar el proceso de toma de decisiones de diseño e implementación de Active Directory. El arquitecto proporciona conocimientos técnicos para ayudarle en el proceso de diseño e implementación de AD DS.  
  
> [!NOTE]  
> Si ningún personal existente en su organización tiene el diseño de Active experimentar, desea contratar a un consultor externo que sea un experto en diseño de Active Directory y la implementación.  
  
Las responsabilidades del arquitecto del proyecto de Active Directory incluyen lo siguiente:  
  
-   Posee el diseño de Active Directory  
  
-   Descripción y grabar la lógica de decisiones de diseño clave  
  
-   Asegurarse de que el diseño cumple las necesidades empresariales de la organización  
  
-   Establecimiento de consenso entre diseño, implementación y los equipos de operaciones  
  
-   Descripción de las necesidades de aplicaciones de AD DS integrada  
  
El diseño de Active Directory final debe reflejar una combinación de los objetivos empresariales y decisiones técnicas. Por lo tanto, el arquitecto del proyecto debe revisar las decisiones de diseño para asegurarse de que se alinean con los objetivos empresariales.  
  
### <a name="project-manager"></a>Jefe de proyecto  
El jefe de proyecto facilita la cooperación entre las unidades de negocio y entre los grupos de administración de tecnología. Idealmente, el jefe de proyecto de implementación de Active Directory es alguien de la organización que está familiarizada con las directivas tanto operativas de los grupos de TI y los requisitos de diseño para los grupos que se están preparando para la implementación de AD DS. El jefe de proyecto supervisa el proyecto de implementación completa, empezando por el diseño y continuando hasta la implementación y asegura que el proyecto se mantiene en programación y dentro del presupuesto. Las responsabilidades del jefe de proyecto incluyen lo siguiente:  
  
-   Proporciona planificación como la programación y presupuestos de proyecto básico  
  
-   Progreso de conducción en el proyecto de diseño e implementación de Active Directory  
  
-   Asegurarse de que las personas correspondientes estén implicadas en cada parte del proceso de diseño  
  
-   Que actúa como único punto de contacto para el proyecto de implementación de Active Directory  
  
-   Establecer la comunicación entre el diseño, implementación y los equipos de operaciones  
  
-   Establecer y mantener la comunicación con el patrocinador ejecutivo en todo el proyecto de implementación  
  
## <a name="BKMK_2"></a>Establecimiento de los propietarios y administradores  
En un proyecto de implementación de Active Directory, las personas que son propietarios son responsables por la administración para asegurarse de que se hayan completado las tareas de la implementación y que las especificaciones de satisfacen las necesidades de la organización del diseño de Active Directory. Los propietarios no necesariamente tienen acceso a o manipular directamente la infraestructura de directorios. Los administradores son los responsables de completar las tareas de implementación necesarias. Los administradores tienen el acceso a la red y los permisos necesarios para manipular el directorio y su infraestructura.  
  
El rol del propietario es directivo y estratégicas. Los propietarios son responsables de comunicar a los administradores de las tareas necesarias para la implementación del diseño de Active Directory como la creación de nuevos controladores de dominio del bosque. Los administradores responsables de implementar el diseño de la red según las especificaciones de diseño.  
  
En organizaciones grandes, distintas personas cubrir roles de administrador o propietario; Sin embargo, en algunas organizaciones pequeñas, la misma persona puede actuar como el propietario y el administrador.  
  
### <a name="service-and-data-owners"></a>Propietarios de servicio y los datos  
Administración de AD DS a diario implica a dos tipos de propietarios:  
  
-   Propietarios de servicio que son responsables de garantizar que el directorio sigue funcionando y que se mantienen los objetivos establecidos en los acuerdos de nivel de servicio y mantenimiento a largo plazo y planificación de la infraestructura de Active Directory  
  
-   Propietarios de datos que son responsables del mantenimiento de la información almacenada en el directorio. Esto incluye la administración de recursos locales como servidores miembro y estaciones de trabajo y administración de cuentas de equipo y usuario.  
  
Es importante identificar los propietarios de servicio y los datos de Active Directory al principio, por lo que puede participar en como gran parte del proceso de diseño como sea posible. Dado que los propietarios de servicio y los datos son responsables del mantenimiento a largo plazo del directorio después de que finalice el proyecto de implementación, es importante para estos usuarios para proporcionar entradas relativas a las necesidades de organización y estar familiarizado con cómo y por qué se realizan ciertas decisiones de diseño. Los propietarios de servicio incluyen el propietario del bosque, el propietario de Active Directory de nomenclatura sistema dominio (DNS) y el propietario de topología del sitio. Los propietarios de datos incluyen los propietarios de la unidad organizativa (OU).  
  
### <a name="service-and-data-administrators"></a>Administradores de servicio y los datos  
La operación de AD DS implica dos tipos de administradores: los administradores y los administradores de datos del servicio. Los administradores de servicios implementan las decisiones de directiva tomadas por los propietarios de servicio y administrar las tareas diarias asociadas con el mantenimiento de la infraestructura y el servicio de directorio. Esto incluye administrar los controladores de dominio que se hospeda el servicio de directorio, administrar otros servicios de red como DNS que son necesarias para AD DS, controlar la configuración de la configuración de todo el bosque y asegurarse de que siempre es el directorio está disponible.  
  
Los administradores de servicios también son responsables de completar las tareas de implementación de Active Directory en curso que son necesarias una vez completado el proceso de implementación inicial de Windows Server 2008 Active Directory. Por ejemplo, como las demandas sobre el aumento del directorio, los administradores de servicios creación controladores de dominio adicionales y establecen o quitar las confianzas entre dominios, según sea necesario. Por este motivo, el equipo de implementación de Active Directory debe incluir los administradores de servicios.  
  
Debe tener cuidado para asignar roles de administrador de servicio solo a las personas de confianza de la organización. Dado que estas personas tienen la capacidad de modificar los archivos del sistema en los controladores de dominio, puede cambiar el comportamiento de AD DS. Debe asegurarse de que los administradores de servicios en su organización son las personas que están familiarizadas con las operaciones y las directivas de seguridad que están en vigor en la red y que comprenden la necesidad de aplicar esas directivas.  
  
Los administradores de datos son usuarios dentro de un dominio que son responsables de tanto para el mantenimiento de datos que se almacenan en AD DS como cuentas de usuario y grupo y para mantener los equipos que son miembros de su dominio. Los administradores de datos controlan los subconjuntos de objetos dentro del directorio y no tienen ningún control sobre la instalación o configuración del servicio de directorio.  
  
Las cuentas de administrador de datos no se proporcionan de forma predeterminada. Después de que el equipo de diseño determina la forma en que los recursos se pueden administrar para la organización, los propietarios del dominio deben crear cuentas de administrador de datos y les delegar los permisos adecuados según el conjunto de objetos para el que los administradores son responsables de .  
  
Es mejor limitar el número de administradores de servicios en su organización para el número mínimo necesario para asegurarse de que la infraestructura sigue funcionando. Los administradores de datos se puede completar la mayoría del trabajo administrativo. Los administradores de servicios requieren un mucho más amplio conjunto de habilidades porque son responsables de mantener el directorio y la infraestructura que lo admite. Los administradores de datos solo requieren las habilidades necesarias para administrar su parte del directorio. Al dividir las asignaciones de este modo da como resultado un ahorro de costos para la organización porque sólo un pequeño número de administradores deba estar capacitado para operar y mantener su infraestructura y todo el directorio.  
  
Por ejemplo, un administrador de servicios debe entender cómo agregar un dominio a un bosque. Esto incluye cómo instalar el software para convertir a un servidor en un controlador de dominio y cómo manipular el entorno de DNS para que el controlador de dominio se puede combinar sin problemas en el entorno de Active Directory. Un administrador de datos sólo debe saber cómo administrar los datos específicos que son responsables como la creación de nuevas cuentas de usuario para nuevos empleados del departamento.  
  
Implementación de AD DS requiere la coordinación y la comunicación entre varios grupos de diferentes implicados en la operación de la infraestructura de red. Estos grupos deben designar a los propietarios de servicio y los datos que son responsables de que representa los distintos grupos durante el proceso de diseño e implementación.  
  
Una vez completado el proyecto de implementación, estos propietarios de servicio y los datos siguen siendo responsables de la parte de la infraestructura administrada por su grupo. En un entorno de Active Directory, estos propietarios son el propietario del bosque, el DNS para el propietario de AD DS, el propietario de topología del sitio y el propietario de la unidad organizativa. Las funciones de estos propietarios de servicio y los datos se explican en las secciones siguientes.  
  
#### <a name="forest-owner"></a>Propietario del bosque  
El propietario del bosque suele ser un administrador de tecnología (TI) de jefe de información de la organización quién es responsable del proceso de implementación de Active Directory y quién es el último responsable de mantener la prestación de servicios en el bosque después de la implementación completada. El propietario del bosque asigna los individuos para rellenar los demás roles de la propiedad mediante la identificación de personal clave dentro de la organización que pueden contribuir a la información necesaria acerca de la infraestructura de red y necesidades administrativas. El propietario del bosque es responsable de lo siguiente:  
  
-   Implementación del dominio raíz del bosque para crear el bosque  
  
-   Implementación del primer controlador de dominio en cada dominio para crear los dominios necesarios para el bosque  
  
-   Pertenencia a los grupos de administrador de servicio en todos los dominios del bosque  
  
-   Creación del diseño de la estructura de OU para cada dominio del bosque  
  
-   Delegación de autoridad administrativa a los propietarios de la unidad organizativa  
  
-   Cambios en el esquema  
  
-   Cambios de configuración de todo el bosque  
  
-   Implementación de ciertas opciones de directiva de directiva de grupo, incluidas las directivas de cuenta de usuario de dominio, como la directiva de bloqueo de cuenta y contraseña específica  
  
-   Configuración de directiva de negocio que se aplica a los controladores de dominio  
  
-   Otras configuraciones de directiva de grupo que se aplican en el nivel de dominio  
  
El propietario del bosque tiene autoridad sobre todo el bosque. Es responsabilidad del propietario bosque para establecer la directiva de grupo y las directivas empresariales y para seleccionar a las personas que son administradores de servicio. El propietario del bosque es un servicio.  
  
#### <a name="dns-for-ad-ds-owner"></a>DNS para el propietario de AD DS  
El DNS para el propietario de AD DS es una persona que tiene un conocimiento exhaustivo de la infraestructura DNS existente y el espacio de nombres existente de la organización.  
  
El DNS para el propietario de AD DS es responsable de lo siguiente:  
  
-   Que actúa como un vínculo entre el equipo de diseño y el grupo de TI que actualmente posee la infraestructura DNS  
  
-   Proporcionar la información sobre el espacio de nombres DNS existente de la organización para ayudar en la creación del nuevo espacio de nombres de Active Directory  
  
-   Trabajar con el equipo de implementación para asegurarse de que se implementa la nueva infraestructura DNS según las especificaciones del equipo de diseño y que funciona correctamente  
  
-   Administrar el DNS para la infraestructura de AD DS, incluido el servicio servidor DNS y los datos DNS  
  
El DNS para el propietario de AD DS es el propietario de un servicio.  
  
#### <a name="site-topology-owner"></a>Propietario de topología de sitio  
El propietario de la topología de sitio está familiarizado con la estructura física de la red de la organización, incluida la asignación de áreas de la red que están conectadas mediante vínculos lentos, enrutadores y subredes individuales. El propietario de topología del sitio es responsable de lo siguiente:  
  
-   Descripción de la topología de red físico y cómo afecta a AD DS  
  
-   Descripción de cómo afectará a la red de la implementación de Active Directory  
  
-   Determinación de los sitios lógicos de Active Directory que deben crearse  
  
-   Actualizar objetos de sitio para los controladores de dominio cuando se agrega una subred, modificado o quitado  
  
-   Creación de vínculos a sitios, puentes de vínculos de sitio y los objetos de conexión manual  
  
El propietario de topología del sitio es el propietario de un servicio.  
  
#### <a name="ou-owner"></a>Propietario de la unidad organizativa  
El propietario de la unidad organizativa es responsable de administrar los datos almacenados en el directorio. Esta persona debe estar familiarizado con las operaciones y las directivas de seguridad que están configuradas en la red. Los propietarios de la unidad organizativa pueden realizar únicamente aquellas tareas que los administradores de servicios se han delegado a ellos y pueden realizar solo esas tareas en las unidades organizativas a los que están asignadas. Las tareas que puedan estar asignadas al propietario de la unidad organizativa incluyen lo siguiente:  
  
-   Realizar todas las tareas de administración de cuenta dentro de su unidad organizativa asignado  
  
-   Administración de estaciones de trabajo y los servidores miembro que son miembros de su unidad organizativa asignado  
  
-   Delegación de autoridad a los administradores locales en sus unidades Organizativas asignado  
  
El propietario de la unidad organizativa es un propietario de datos.  
  
## <a name="BKMK_3"></a>Equipos de proyecto de creación  
Los equipos de proyecto de Active Directory son grupos temporales que son responsables de completar las tareas de diseño e implementación de Active Directory. Cuando se complete el proyecto de implementación de Active Directory, los propietarios asumen la responsabilidad para el directorio y los equipos de proyecto pueden Desintegrar.  
  
El tamaño de los equipos de proyecto varía según el tamaño de la organización. En organizaciones pequeñas, una sola persona puede abarcar varias áreas de responsabilidad en un equipo de proyecto y participar en más de una fase de la implementación. Las grandes organizaciones podrían requerir los equipos más grandes con distintas personas o incluso diferentes equipos que abarcan las diferentes áreas de responsabilidad. El tamaño de los equipos no es importante, siempre se asignan todas las áreas de responsabilidad, y se cumplen los objetivos de diseño de la organización.  
  
### <a name="identifying-potential-forest-owners"></a>Identificación de posibles propietarios de bosque  
Identifique los grupos dentro de su organización que poseen y controlan los recursos necesarios para proporcionar servicios de directorio a los usuarios de la red. Estos grupos se consideran los propietarios posibles de bosque.  
  
La separación de la administración de datos y el servicio en AD DS permite a la infraestructura de TI (o los grupos) de una organización para administrar el servicio de directorio, mientras que los administradores locales en cada grupo de administran los datos que pertenecen a sus propios grupos. Posibles propietarios del bosque tienen la autoridad necesaria a través de la infraestructura de red para implementar y admitir AD DS.  
  
Para las organizaciones que tienen una infraestructura centralizada grupo de TI, el grupo de TI suele ser el propietario del bosque y, por lo tanto, el propietario del bosque posibles para las implementaciones futuras. Las organizaciones que incluyen una serie de independiente de infraestructura grupos tienen un número de posibles propietarios del bosque. Si su organización ya tiene una infraestructura de Active Directory en su lugar, los propietarios del bosque actual también son posibles propietarios de bosques para las implementaciones nuevas.  
  
Seleccione uno de los posibles propietarios de bosque para que actúe como el propietario del bosque para cada bosque que está considerando para la implementación. Estos propietarios posibles de bosque son responsables de trabajar con el equipo de diseño para determinar si realmente se implementará su bosque o si un curso de acción (por ejemplo, unirse a otro bosque existente) alternativo es un mejor uso de los recursos disponibles y se ajustan a sus necesidades. El propietario del bosque (o los propietarios) en su organización son miembros del equipo de diseño de Active Directory.  
  
### <a name="establishing-a-design-team"></a>Establecimiento de un equipo de diseño  
El equipo de diseño de Active Directory es responsable de recopilar toda la información necesaria para tomar decisiones sobre el diseño de la estructura lógica de Active Directory.  
  
Las responsabilidades del equipo de diseño incluyen lo siguiente:  
  
-   Determinar cuántos bosques y dominios son necesarios y cuáles son las relaciones entre los bosques y dominios  
  
-   Trabajar con los propietarios de datos para asegurarse de que el diseño cumple sus requisitos administrativos y seguridad  
  
-   Trabajar con los administradores de red actual para asegurarse de que la infraestructura de red actual admite el diseño y que el diseño no afecta negativamente a las aplicaciones existentes que se implementan en la red  
  
-   Trabajar con los representantes del grupo de seguridad de la organización para asegurarse de que el diseño cumple las directivas de seguridad establecido  
  
-   Diseño de estructuras de unidades Organizativas que permiten que los niveles adecuados de protección y la delegación de autoridad adecuada a los propietarios de datos  
  
-   Trabajar con el equipo de implementación para el diseño de pruebas en un laboratorio de entorno para asegurarse de que funciona como planeada y modificar el diseño como sea necesario para solucionar los problemas que se producen  
  
-   Crear un diseño de topología de sitio que cumpla los requisitos de replicación del bosque y evita la sobrecarga del ancho de banda disponible. Para obtener más información sobre cómo diseñar la topología del sitio, consulte [diseñar el sitio de topología para Windows Server 2008 AD DS](https://technet.microsoft.com/library/cc772013.aspx).  
  
-   Trabajar con el equipo de implementación para asegurarse de que el diseño se ha implementado correctamente  
  
El equipo de diseño incluye a los siguientes miembros:  
  
-   Posibles propietarios de bosque  
  
-   Arquitecto de proyectos  
  
-   Jefe de proyecto  
  
-   Personas que son responsables de establecer y mantener directivas de seguridad en la red  
  
Durante el proceso de diseño de la estructura lógica, el equipo de diseño identifica los otros propietarios. Deben iniciar estos individuos que participan en el proceso de diseño, en cuanto se identifican. Después de que el proyecto de implementación se pasa al equipo de implementación, el equipo de diseño es responsable de supervisar el proceso de implementación para asegurarse de que el diseño se ha implementado correctamente. El equipo de diseño también realiza cambios en el diseño según los comentarios de la prueba.  
  
### <a name="establishing-a-deployment-team"></a>Establecimiento de un equipo de implementación  
El equipo de implementación de Active Directory es responsable de probar e implementar el diseño de la estructura lógica de Active Directory. Esto implica las siguientes tareas:  
  
-   Establecimiento de un entorno de prueba que lo suficientemente emula el entorno de producción  
  
-   Probar el diseño mediante la implementación de la estructura propuesta de bosque y dominio en un entorno de laboratorio para comprobar que cumple los objetivos de cada propietario de la función  
  
-   Desarrollar y probar los escenarios de migración propuestos por el diseño en un entorno de laboratorio  
  
-   Asegurarse de que cada propietario de la firma en el proceso de prueba para asegurarse de que se están probando las características de diseño correcto  
  
-   Probar la operación de implementación en un entorno piloto  
  
Cuando haya completado el diseño y las tareas de prueba, el equipo de implementación realiza las siguientes tareas:  
  
-   Crea los bosques y dominios según el diseño de la estructura lógica de Active Directory  
  
-   Crea los sitios y objetos de vínculo de sitio según sea necesario basándose en el diseño de topología de sitio  
  
-   Garantiza que la infraestructura DNS está configurada para admitir AD DS y que los nuevos espacios de nombres están integradas en el espacio de nombres existente de la organización  
  
El equipo de implementación de Active Directory incluye a los siguientes miembros:  
  
-   Propietario del bosque  
  
-   DNS para el propietario de AD DS  
  
-   Propietario de topología de sitio  
  
-   Propietarios de la unidad organizativa  
  
El equipo de implementación funciona con los administradores de servicio y los datos durante la fase de implementación para asegurarse de que los miembros del equipo de operaciones están familiarizados con el nuevo diseño. Esto ayuda a garantizar una transición sin problemas de propiedad cuando se completa la operación de implementación. Al finalizar el proceso de implementación, la responsabilidad de mantener el nuevo entorno de Active Directory se pasa al equipo de operaciones.  
  
### <a name="documenting-the-design-and-deployment-teams"></a>Documentar los equipos de diseño e implementación  
Los nombres de documentos e información de contacto para las personas que participarán en el diseño e implementación de AD DS. Identifique quién será responsable de cada rol en los equipos de diseño e implementación. Inicialmente, esta lista incluye los posibles propietarios del bosque, el jefe de proyecto y el arquitecto del proyecto. Al determinar el número de bosques que va a implementar, es posible que deba crear nuevos equipos de diseño para bosques adicionales. Tenga en cuenta que deberá actualizar la documentación de la medida que cambian las pertenencias de equipo y a medida que identifique los distintos propietarios de Active Directory durante el proceso de diseño. Para que una hoja de cálculo que le ayudarán a documentar los equipos de diseño e implementación para cada bosque, descargue Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip de trabajo ayudas para Windows Server 2003 Deployment Kit ([ https://go.microsoft.com/fwlink/?LinkID=102558 ](https://go.microsoft.com/fwlink/?LinkID=102558)) y abra el "Diseño y equipo de información de implementación" (DSSLOGI_1.doc).  
  


