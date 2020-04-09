---
ms.assetid: 50bd2566-e03c-4884-b5c4-895c8aab80aa
title: Determinación de los participantes en el proyecto de implementación
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 516c37165952e46c8e6e76499909e90851e305ce
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822318"
---
# <a name="identifying-the-deployment-project-participants"></a>Determinación de los participantes en el proyecto de implementación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El primer paso para establecer un proyecto de implementación para Dominio de Active Directory Service (AD DS) es establecer los equipos de proyecto de diseño e implementación que serán responsables de administrar la fase de diseño y la fase de implementación del ciclo de proyecto de Active Directory. Además, debe identificar los usuarios y grupos que serán responsables de poseer y mantener el directorio una vez completada la implementación.  
  
-   [Definir roles específicos del proyecto](#BKMK_1)  
  
-   [Establecimiento de propietarios y administradores](#BKMK_2)  
  
-   [Compilar equipos de proyecto](#BKMK_3)  
  
## <a name="defining-project-specific-roles"></a><a name="BKMK_1"></a>Definir roles específicos del proyecto  
Un paso importante en el establecimiento de los equipos del proyecto es identificar a las personas que van a contener roles específicos del proyecto. Entre ellos se incluyen el Patrocinador Ejecutivo, el arquitecto del proyecto y el jefe de proyecto. Estas personas son responsables de ejecutar el proyecto de implementación de Active Directory.  
  
Después de nombrar al arquitecto de proyectos y al jefe de proyecto, estas personas establecen canales de comunicación en toda la organización, compilan programaciones del proyecto e identifican a los usuarios que serán miembros de los equipos del proyecto, empezando por los distintos propietarios.  
  
### <a name="executive-sponsor"></a>Patrocinador Ejecutivo  
La implementación de una infraestructura como AD DS puede tener un gran impacto en una organización. Por esta razón, es importante tener un Patrocinador Ejecutivo que comprenda el valor empresarial de la implementación, admita el proyecto en el nivel ejecutivo y pueda ayudar a resolver conflictos en toda la organización.  
  
### <a name="project-architect"></a>Arquitecto de proyecto  
Cada proyecto de implementación de Active Directory requiere un arquitecto de proyecto para administrar el proceso de toma de decisiones de implementación y diseño Active Directory. El arquitecto proporciona conocimientos técnicos que le ayudarán en el proceso de diseño e implementación de AD DS.  
  
> [!NOTE]  
> Si ningún personal existente de su organización tiene experiencia de diseño de directorios, es posible que desee contratar a un consultor externo que sea un experto en Active Directory diseño e implementación.  
  
Entre las responsabilidades del arquitecto de proyectos de Active Directory se incluyen las siguientes:  
  
-   Propietario del diseño de Active Directory  
  
-   Comprensión y grabación de la justificación de las decisiones de diseño clave  
  
-   Asegurarse de que el diseño cumple las necesidades empresariales de la organización  
  
-   Establecer el consenso entre los equipos de diseño, implementación y operaciones  
  
-   Descripción de las necesidades de las aplicaciones integradas en AD DS  
  
El diseño final de Active Directory debe reflejar una combinación de objetivos empresariales y decisiones técnicas. Por lo tanto, el arquitecto del proyecto debe revisar las decisiones de diseño para asegurarse de que se alinean con los objetivos empresariales.  
  
### <a name="project-manager"></a>Jefe de proyecto  
El jefe de proyecto facilita la cooperación entre las unidades de negocio y entre los grupos de administración de tecnología. Idealmente, el administrador de proyectos de Active Directory Deployment es alguien de dentro de la organización que está familiarizado con las directivas operativas del grupo de ti y con los requisitos de diseño de los grupos que se están preparando para implementar AD DS. El jefe de proyecto supervisa todo el proyecto de implementación, comenzando por el diseño y continuando la implementación, y se asegura de que el proyecto permanece en programación y dentro del presupuesto. Entre las responsabilidades del administrador de proyectos se incluyen las siguientes:  
  
-   Proporcionar planeación básica del proyecto, como la programación y el presupuesto  
  
-   Promoviendo el progreso en el proyecto de diseño e implementación de Active Directory  
  
-   Asegurarse de que las personas adecuadas participan en cada parte del proceso de diseño  
  
-   Servir como punto de contacto único para el proyecto de implementación de Active Directory  
  
-   Establecer la comunicación entre los equipos de diseño, implementación y operaciones  
  
-   Establecer y mantener la comunicación con el Patrocinador Ejecutivo a lo largo del proyecto de implementación  
  
## <a name="establishing-owners-and-administrators"></a><a name="BKMK_2"></a>Establecimiento de propietarios y administradores  
En un proyecto de implementación de Active Directory, los usuarios que son propietarios son responsables de la administración para asegurarse de que se completan las tareas de implementación y de que Active Directory especificaciones de diseño satisfacen las necesidades de la organización. Los propietarios no tienen necesariamente acceso o manipulan directamente la infraestructura de directorios. Los administradores son los responsables de completar las tareas de implementación necesarias. Los administradores tienen el acceso a la red y los permisos necesarios para manipular el directorio y su infraestructura.  
  
El rol del propietario es estratégico y directivo. Los propietarios son responsables de comunicarse con los administradores de las tareas necesarias para la implementación del Active Directory diseño, como la creación de nuevos controladores de dominio en el bosque. Los administradores son responsables de implementar el diseño en la red de acuerdo con las especificaciones de diseño.  
  
En organizaciones de gran tamaño, las distintas personas rellenan los roles de propietario y de administrador. sin embargo, en algunas organizaciones pequeñas, la misma persona podría actuar como propietario y como administrador.  
  
### <a name="service-and-data-owners"></a>Servicio y propietarios de datos  
La administración de AD DS diaria implica dos tipos de propietarios:  
  
-   Los propietarios de servicios que sean responsables de la planificación y el mantenimiento a largo plazo de la infraestructura de Active Directory y de garantizar que el directorio siga funcionando y de que se mantengan los objetivos establecidos en los acuerdos de nivel de servicio.  
  
-   Propietarios de datos que son responsables del mantenimiento de la información almacenada en el directorio. Esto incluye la administración de cuentas de usuario y equipo, y la administración de recursos locales, como servidores miembro y estaciones de trabajo.  
  
Es importante identificar el servicio de Active Directory y los propietarios de datos al principio para que puedan participar en todo el proceso de diseño posible. Dado que el servicio y los propietarios de los datos son responsables del mantenimiento a largo plazo del directorio una vez finalizado el proyecto de implementación, es importante que estos usuarios proporcionen información sobre las necesidades de la organización y estén familiarizados con cómo y por qué se realizan ciertas decisiones de diseño. Los propietarios de servicios incluyen el propietario del bosque, el propietario del sistema de nombres Dominio de Active Directory (DNS) y el propietario de la topología del sitio. Los propietarios de datos incluyen propietarios de unidades organizativas (OU).  
  
### <a name="service-and-data-administrators"></a>Administradores de datos y servicios  
La operación de AD DS implica dos tipos de administradores: administradores de servicios y administradores de datos. Los administradores de servicios implementan decisiones de directivas tomadas por los propietarios de servicios y controlan las tareas cotidianas asociadas al mantenimiento del servicio de directorio y la infraestructura. Esto incluye la administración de los controladores de dominio que hospedan el servicio de directorio, la administración de otros servicios de red como DNS necesarios para AD DS, el control de la configuración de la configuración de todo el bosque y la garantía de que el directorio está siempre disponible.  
  
Los administradores de servicios también son responsables de llevar a cabo tareas de implementación Active Directory en curso que son necesarias una vez completado el proceso de implementación inicial de Windows Server 2008 Active Directory. Por ejemplo, a medida que aumenta el directorio, los administradores de servicios crean controladores de dominio adicionales y establecen o quitan confianzas entre dominios, según sea necesario. Por esta razón, el equipo de implementación de Active Directory debe incluir administradores de servicios.  
  
Debe tener cuidado de asignar roles de administrador de servicios solo a usuarios de confianza de la organización. Dado que estas personas tienen la capacidad de modificar los archivos del sistema en los controladores de dominio, pueden cambiar el comportamiento de AD DS. Debe asegurarse de que los administradores de servicios de su organización son usuarios familiarizados con las directivas operativas y de seguridad que están en su lugar en la red y que comprenden la necesidad de aplicar esas directivas.  
  
Los administradores de datos son usuarios de un dominio que son responsables de mantener los datos almacenados en AD DS como cuentas de usuario y de grupo, y para mantener los equipos que son miembros de su dominio. Los administradores de datos controlan subconjuntos de objetos dentro del directorio y no tienen ningún control sobre la instalación o configuración del servicio de directorio.  
  
Las cuentas de administrador de datos no se proporcionan de forma predeterminada. Una vez que el equipo de diseño determina cómo se van a administrar los recursos para la organización, los propietarios del dominio deben crear cuentas de administrador de datos y delegar los permisos adecuados en función del conjunto de objetos para los que los administradores vayan a ser responsables.  
  
Es mejor limitar el número de administradores de servicios de su organización al número mínimo necesario para asegurarse de que la infraestructura siga funcionando. Los administradores de datos pueden completar la mayoría del trabajo administrativo. Los administradores de servicios requieren un conjunto de aptitudes mucho más amplio porque son responsables de mantener el directorio y la infraestructura que lo admiten. Los administradores de datos solo requieren los conocimientos necesarios para administrar su parte del directorio. Dividir las asignaciones de trabajo de esta manera supone un ahorro en el costo de la organización, ya que solo es necesario entrenar a un pequeño número de administradores para que funcionen y mantengan todo el directorio y su infraestructura.  
  
Por ejemplo, un administrador de servicios debe comprender cómo agregar un dominio a un bosque. Esto incluye cómo instalar el software para convertir un servidor en un controlador de dominio y cómo manipular el entorno DNS para que el controlador de dominio se pueda combinar sin problemas en el entorno de Active Directory. Un administrador de datos solo necesita saber cómo administrar los datos específicos de los que son responsables, como la creación de nuevas cuentas de usuario para los nuevos empleados de su departamento.  
  
La implementación de AD DS requiere la coordinación y la comunicación entre muchos grupos diferentes implicados en el funcionamiento de la infraestructura de red. Estos grupos deben designar a los propietarios de datos y servicios que son responsables de representar los distintos grupos durante el proceso de diseño e implementación.  
  
Una vez completado el proyecto de implementación, estos servicios y propietarios de datos seguirán siendo responsables de la parte de la infraestructura administrada por su grupo. En un entorno de Active Directory, estos propietarios son el propietario del bosque, el DNS de AD DS propietario, el propietario de la topología del sitio y el propietario de la unidad organizativa. Los roles de estos servicios y propietarios de datos se explican en las secciones siguientes.  
  
#### <a name="forest-owner"></a>Propietario del bosque  
El propietario del bosque es normalmente un administrador de tecnología de la información (TI) Senior en la organización responsable del proceso de implementación de Active Directory y que, en última instancia, es responsable de mantener la entrega del servicio dentro del bosque una vez completada la implementación. El propietario del bosque asigna usuarios para que rellenen los demás roles de propiedad mediante la identificación del personal clave dentro de la organización que puede contribuir a la información necesaria sobre la infraestructura de red y las necesidades administrativas. El propietario del bosque es responsable de lo siguiente:  
  
-   Implementación del dominio raíz del bosque para crear el bosque  
  
-   Implementación del primer controlador de dominio en cada dominio para crear los dominios necesarios para el bosque  
  
-   Pertenencias de los grupos de administradores de servicios en todos los dominios del bosque  
  
-   Creación del diseño de la estructura de la unidad organizativa para cada dominio del bosque  
  
-   Delegación de la autoridad administrativa a los propietarios de la unidad organizativa  
  
-   Cambios en el esquema  
  
-   Cambios en las opciones de configuración de todo el bosque  
  
-   Implementación de ciertas opciones de configuración de directiva de directiva de grupo, incluidas directivas de cuenta de usuario de dominio, como la Directiva de bloqueo de cuentas y contraseña específica  
  
-   Configuración de la Directiva de negocios que se aplica a los controladores de dominio  
  
-   Cualquier otro valor de directiva de grupo que se aplique en el nivel de dominio  
  
El propietario del bosque tiene autoridad sobre todo el bosque. Es responsabilidad del propietario del bosque establecer directiva de grupo y las directivas empresariales y seleccionar las personas que son administradores de servicios. El propietario del bosque es un propietario del servicio.  
  
#### <a name="dns-for-ad-ds-owner"></a>DNS para AD DS propietario  
El DNS de AD DS propietario es un individuo que tiene un conocimiento exhaustivo de la infraestructura de DNS existente y el espacio de nombres existente de la organización.  
  
El DNS de AD DS propietario es responsable de lo siguiente:  
  
-   Actuar como enlace entre el equipo de diseño y el grupo de ti que posee actualmente la infraestructura DNS  
  
-   Proporcionar información sobre el espacio de nombres DNS existente de la organización para ayudar en la creación del nuevo espacio de nombres de Active Directory  
  
-   Trabajar con el equipo de implementación para asegurarse de que la nueva infraestructura DNS se implementa de acuerdo con las especificaciones del equipo de diseño y de que funciona correctamente  
  
-   Administración de DNS para la infraestructura de AD DS, incluido el servicio servidor DNS y los datos DNS  
  
El DNS de AD DS propietario es un propietario del servicio.  
  
#### <a name="site-topology-owner"></a>Propietario de la topología de sitio  
El propietario de la topología de sitio está familiarizado con la estructura física de la red de la organización, incluida la asignación de subredes individuales, enrutadores y áreas de red que se conectan mediante vínculos de baja velocidad. El propietario de la topología de sitio es responsable de lo siguiente:  
  
-   Descripción de la topología de red física y cómo afecta a AD DS  
  
-   Descripción de cómo la implementación de Active Directory afectará a la red  
  
-   Determinar el Active Directory sitios lógicos que se deben crear  
  
-   Actualización de objetos de sitio para controladores de dominio cuando se agrega, modifica o quita una subred  
  
-   Crear vínculos a sitios, puentes de vínculos a sitios y objetos de conexión manual  
  
El propietario de la topología de sitio es el propietario de un servicio.  
  
#### <a name="ou-owner"></a>Propietario de la unidad organizativa  
El propietario de la unidad organizativa es responsable de administrar los datos almacenados en el directorio. Este individuo debe estar familiarizado con las directivas operativas y de seguridad que están en su lugar en la red. Los propietarios de las unidades organizativas pueden realizar solo las tareas que los administradores de servicios han delegado y pueden realizar solo las tareas en las unidades organizativas a las que están asignadas. Entre las tareas que se pueden asignar al propietario de la unidad organizativa se incluyen las siguientes:  
  
-   Realización de todas las tareas de administración de cuentas en la unidad organizativa asignada  
  
-   Administración de estaciones de trabajo y servidores miembro que son miembros de la unidad organizativa asignada  
  
-   Delegación de autoridad en administradores locales dentro de la unidad organizativa asignada  
  
El propietario de la unidad organizativa es un propietario de datos.  
  
## <a name="building-project-teams"></a><a name="BKMK_3"></a>Compilar equipos de proyecto  
Active Directory los equipos de proyecto son grupos temporales que son responsables de completar Active Directory tareas de diseño e implementación. Una vez completado el proyecto de implementación de Active Directory, los propietarios asumen la responsabilidad del directorio y los equipos del proyecto pueden Disband.  
  
El tamaño de los equipos del proyecto varía según el tamaño de la organización. En organizaciones pequeñas, una sola persona puede abarcar varias áreas de responsabilidad en un equipo de proyecto y participar en más de una fase de la implementación. Es posible que las organizaciones de gran tamaño requieran equipos más grandes con individuos diferentes o incluso distintos equipos que cubran las distintas áreas de responsabilidad. El tamaño de los equipos no es importante, siempre y cuando se asignen todas las áreas de responsabilidad y se cumplan los objetivos de diseño de la organización.  
  
### <a name="identifying-potential-forest-owners"></a>Identificación de posibles propietarios de bosques  
Identifique los grupos de su organización que poseen y controlan los recursos necesarios para proporcionar servicios de directorio a los usuarios de la red. Estos grupos se consideran posibles propietarios de bosques.  
  
La separación del servicio y la administración de datos en AD DS permite que el grupo de TI de la infraestructura (o grupos) de una organización administre el servicio de directorio, mientras que los administradores locales de cada grupo administran los datos que pertenecen a sus propios grupos. Los posibles propietarios de bosques tienen la autoridad necesaria a través de la infraestructura de red para implementar y admitir AD DS.  
  
En el caso de las organizaciones que tienen un grupo de TI de infraestructura centralizada, el grupo de ti es generalmente el propietario del bosque y, por lo tanto, el posible propietario del bosque para futuras implementaciones. Las organizaciones que incluyen un número de grupos de TI de infraestructura independiente tienen una serie de posibles propietarios de bosque. Si su organización ya tiene una infraestructura de Active Directory implementada, los propietarios de bosque actuales también son posibles propietarios de bosque para nuevas implementaciones.  
  
Seleccione uno de los posibles propietarios de bosque para que actúe como el propietario del bosque para cada bosque que esté pensando en la implementación. Estos posibles propietarios de bosque son responsables de trabajar con el equipo de diseño para determinar si se implementará o no su bosque realmente o si un curso alternativo de acción (como unirse a otro bosque existente) es un mejor uso de los recursos disponibles y sigue cumpliendo sus necesidades. El propietario (o propietarios) del bosque de su organización son miembros del equipo de diseño de Active Directory.  
  
### <a name="establishing-a-design-team"></a>Establecer un equipo de diseño  
El equipo de diseño de Active Directory es responsable de recopilar toda la información necesaria para tomar decisiones sobre el diseño de la estructura lógica de Active Directory.  
  
Entre las responsabilidades del equipo de diseño se incluyen las siguientes:  
  
-   Determinar cuántos bosques y dominios son necesarios y cuáles son las relaciones entre los bosques y los dominios  
  
-   Trabajar con los propietarios de datos para asegurarse de que el diseño cumple sus requisitos administrativos y de seguridad  
  
-   Trabajar con los administradores de red actuales para asegurarse de que la infraestructura de red actual admita el diseño y que el diseño no afecte negativamente a las aplicaciones existentes implementadas en la red  
  
-   Trabajar con representantes del grupo de seguridad de la organización para asegurarse de que el diseño cumple las directivas de seguridad establecidas  
  
-   Diseñar estructuras de unidades organizativas que permitan los niveles de protección adecuados y la delegación adecuada de autoridad para los propietarios de datos  
  
-   Trabajar con el equipo de implementación para probar el diseño en un entorno de laboratorio para asegurarse de que funciona según lo previsto y modificar el diseño según sea necesario para solucionar los problemas que se producen  
  
-   Crear un diseño de topología de sitio que cumpla los requisitos de replicación del bosque a la vez que se evita la sobrecarga de ancho de banda disponible. Para obtener más información sobre el diseño de la topología de sitio, consulte [diseño de la topología de sitio para Windows Server 2008 AD DS](https://technet.microsoft.com/library/cc772013.aspx).  
  
-   Trabajar con el equipo de implementación para asegurarse de que el diseño se implementa correctamente  
  
El equipo de diseño incluye los siguientes miembros:  
  
-   Posibles propietarios de bosque  
  
-   Arquitecto de proyecto  
  
-   Jefe de proyecto  
  
-   Personas responsables de establecer y mantener las directivas de seguridad en la red  
  
Durante el proceso de diseño de la estructura lógica, el equipo de diseño identifica a los demás propietarios. Estas personas deben empezar a participar en el proceso de diseño en cuanto se identifican. Una vez que el proyecto de implementación se entrega al equipo de implementación, el equipo de diseño es responsable de supervisar el proceso de implementación para asegurarse de que el diseño se implementa correctamente. El equipo de diseño también realiza cambios en el diseño en función de los comentarios de las pruebas.  
  
### <a name="establishing-a-deployment-team"></a>Establecimiento de un equipo de implementación  
El equipo de implementación de Active Directory es responsable de probar e implementar el diseño de la estructura lógica Active Directory. Esto implica las siguientes tareas:  
  
-   Establecimiento de un entorno de prueba que eMule suficientemente el entorno de producción  
  
-   Probar el diseño implementando el bosque y la estructura de dominio propuestos en un entorno de laboratorio para comprobar que cumple los objetivos de cada propietario de rol  
  
-   Desarrollar y probar los escenarios de migración propuestos por el diseño en un entorno de laboratorio  
  
-   Asegurarse de que cada propietario cierre la sesión en el proceso de prueba para asegurarse de que se prueban las características de diseño correctas  
  
-   Prueba de la operación de implementación en un entorno piloto  
  
Una vez completadas las tareas de diseño y prueba, el equipo de implementación realiza las siguientes tareas:  
  
-   Crea los bosques y los dominios según el diseño de la estructura lógica Active Directory  
  
-   Crea los sitios y los objetos de vínculo a sitios según sea necesario según el diseño de la topología de sitio  
  
-   Garantiza que la infraestructura DNS esté configurada para admitir AD DS y que los nuevos espacios de nombres estén integrados en el espacio de nombres existente de la organización.  
  
El equipo de implementación de Active Directory incluye los siguientes miembros:  
  
-   Propietario del bosque  
  
-   DNS para AD DS propietario  
  
-   Propietario de la topología de sitio  
  
-   Propietarios de Uo  
  
El equipo de implementación trabaja con el servicio y los administradores de datos durante la fase de implementación para asegurarse de que los miembros del equipo de operaciones estén familiarizados con el nuevo diseño. Esto ayuda a garantizar una transición fluida de la propiedad cuando se completa la operación de implementación. Al finalizar el proceso de implementación, la responsabilidad de mantener el nuevo entorno de Active Directory pasa al equipo de operaciones.  
  
### <a name="documenting-the-design-and-deployment-teams"></a>Documentar los equipos de diseño e implementación  
Documente los nombres y la información de contacto de las personas que participarán en el diseño y la implementación de AD DS. Identifique quién será responsable de cada rol en los equipos de diseño e implementación. Inicialmente, esta lista incluye los posibles propietarios del bosque, el jefe de proyecto y el arquitecto del proyecto. Cuando determine el número de bosques que va a implementar, es posible que necesite crear nuevos equipos de diseño para otros bosques. Tenga en cuenta que deberá actualizar la documentación a medida que los miembros del equipo cambien y a medida que identifique los distintos propietarios de Active Directory durante el proceso de diseño. Para obtener una hoja de cálculo que le ayude a documentar los equipos de diseño e implementación de cada bosque, descargue Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip de la ayuda del trabajo para el kit de implementación de Windows Server 2003 ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)) y abra "información del equipo de diseño e implementación" (DSSLOGI_1. doc).  
  


