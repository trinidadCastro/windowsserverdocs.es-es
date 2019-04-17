---
ms.assetid: 50bd2566-e03c-4884-b5c4-895c8aab80aa
title: "Identificar a los participantes del proyecto de implementación"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 57c24c2b2b48c712445ef7dca453de586d33a130
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="identifying-the-deployment-project-participants"></a>Identificar a los participantes del proyecto de implementación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Es el primer paso para establecer un proyecto de implementación de servicios de dominio de Active Directory (AD DS) establecer el diseño y el ciclo de equipos de proyectos de implementación que se encarga de administrar las fases de diseño e implementación del proyecto de Active Directory. Además, debe identificar los usuarios y grupos que serán responsables de la propiedad y mantenimiento el directorio después de completa la implementación.  
  
-   [Definir roles específicos del proyecto](#BKMK_1)  
  
-   [Establecer los propietarios y los administradores](#BKMK_2)  
  
-   [Equipos de proyectos de creación](#BKMK_3)  
  
## <a name="BKMK_1"></a>Definir roles específicos del proyecto  
Un paso importante en el establecimiento de los equipos del proyecto es identificar a las personas que se mantenga funciones específicas del proyecto. Estos incluyen el patrocinador ejecutivo, arquitecto de proyecto y el Administrador de proyecto. Estas personas son responsables de ejecutar el proyecto de implementación de Active Directory.  
  
Después de designar la arquitecto del proyecto y gerente de proyecto, estas personas establecen canales de comunicación en toda la organización, elaborar programaciones e identifican a los individuos que deben ser miembros de los equipos del proyecto, a partir de los propietarios de diversas.  
  
### <a name="executive-sponsor"></a>Ejecutivo  
Implementar una infraestructura de AD DS puede tener un gran impacto en una organización. Por este motivo, es importante que tengas un patrocinador ejecutivo que comprenda el valor empresarial de la implementación, es compatible con el proyecto de los ejecutivos y puede ayudar a resolver los conflictos en toda la organización.  
  
### <a name="project-architect"></a>Arquitecto de proyecto  
Cada proyecto de implementación de Active Directory requiere un arquitecto de proyecto administrar el proceso de toma de decisiones de diseño e implementación de Active Directory. El arquitecto proporciona una experiencia técnica para ayudar con el proceso de diseño e implementación de AD DS.  
  
> [!NOTE]  
> Si no personal existentes en la organización dispone de diseño del directorio experiencia, quieres se contrata a un asesor externo que es un experto en Active Directory diseño e implementación.  
  
Las responsabilidades del arquitecto de proyecto de Active Directory incluyen lo siguiente:  
  
-   Posee el diseño de Active Directory  
  
-   Comprender y grabar la razón por la clave de las decisiones de diseño  
  
-   Asegurarse de que el diseño cumple las necesidades empresariales de la organización  
  
-   Establecimiento de consenso entre equipos de operaciones, la implementación y diseño  
  
-   Descripción de las necesidades de aplicaciones de AD DS integrado  
  
El diseño de Active Directory final debe reflejar una combinación de objetivos empresariales y decisiones técnicas. Por lo tanto, el arquitecto proyecto debe revisar las decisiones de diseño para asegurarte de que se alinean con los objetivos empresariales.  
  
### <a name="project-manager"></a>Administrador de proyectos  
El Administrador de proyecto facilita la cooperación en unidades de negocios y entre grupos de administración de la tecnología. En teoría, el Administrador de proyecto de implementación de Active Directory es alguien de la organización que está familiarizada con ambas directivas el funcionamiento del grupo de TI y los requisitos de diseño para los grupos que están preparando para implementar AD DS. El Administrador de proyecto supervisa el proyecto de implementación completo, a partir de diseño y continuar con la implementación y garantiza que el proyecto se mantenga en programación y el presupuesto. Las responsabilidades del Administrador de proyecto incluyen lo siguiente:  
  
-   Proporcionar básica del proyecto planeación como la programación y presupuesto  
  
-   Ir en coche progreso en el proyecto de diseño e implementación de Active Directory  
  
-   Garantizar que las personas que corresponda son implicadas en cada parte del proceso de diseño  
  
-   Sirve como punto de contacto para el proyecto de implementación de Active Directory único  
  
-   Establecer una comunicación entre el diseño, la implementación y equipos de operaciones  
  
-   Establecer y mantener la comunicación con el patrocinador ejecutivo durante todo el proyecto de implementación  
  
## <a name="BKMK_2"></a>Establecer los propietarios y los administradores  
En un proyecto de implementación de Active Directory, las personas propietarios se mantienen responsables mediante administración para asegurarte de que se completen las tareas de implementación y diseño Active Directory especificaciones de satisfacen las necesidades de la organización. Los propietarios no necesariamente tienen acceso a o manipular directamente de la infraestructura de directorios. Los administradores son los responsables de completar las tareas de implementación necesarios. Los administradores tienen el acceso a la red y los permisos necesarios para manipular el directorio y su infraestructura.  
  
El rol del propietario es directivos y estratégica. Los propietarios están responsables de comunicar a los administradores de las tareas necesarias para la implementación del diseño, como la creación de nuevos controladores de dominio en el bosque de Active Directory. Los administradores son responsables de implementar el diseño de la red según las especificaciones de diseño.  
  
En las grandes organizaciones distintas personas rellenen propietario y el Administrador de roles; Sin embargo, en algunas organizaciones pequeñas, la misma persona puede actuar como el propietario y el administrador.  
  
### <a name="service-and-data-owners"></a>Propietarios de servicio y los datos  
Administración de AD DS diariamente implica a dos tipos de propietarios:  
  
-   Propietarios de servicio que están responsables de mantenimiento a largo plazo y planificación de la infraestructura de Active Directory y para garantizar que el directorio sigue funcionando y que se mantienen los objetivos establecidos en los acuerdos de nivel de servicio  
  
-   Propietarios de datos que son responsables del mantenimiento de la información almacenada en el directorio. Esto incluye el usuario y la administración de cuentas de equipo y la administración de recursos locales, como los servidores miembro y estaciones de trabajo.  
  
Es importante identificar los propietarios de servicio y los datos de Active Directory desde el principio para que pueden participar en como gran parte del proceso de diseño como sea posible. Dado que los propietarios de servicio y los datos son responsables de mantenimiento a largo plazo del directorio de una vez finalizado el proyecto de implementación, es importante para estas personas para proporcionar información relativa a las necesidades de organización y estar familiarizado con cómo y por qué se realizan ciertas decisiones de diseño. Los propietarios de servicio incluyen el propietario del bosque, el propietario de Active Directory de nomenclatura sistema dominio (DNS) y el propietario de la topología de sitio. Los propietarios de datos incluyen los propietarios de unidad organizativa (OU).  
  
### <a name="service-and-data-administrators"></a>Administradores de servicio y los datos  
La operación de AD DS implica dos tipos de administradores: los administradores y los administradores de datos de servicio. Los administradores de servicio implementan Directiva las decisiones de los propietarios de servicio y controlan las tareas diarias asociadas con el mantenimiento de la infraestructura y el servicio de directorio. Esto incluye la administración de los controladores de dominio que hospedan el servicio de directorio, administración de otros servicios de red como DNS que son necesarias para AD DS, controlar la configuración de la configuración de todo el bosque y asegurarte de que el directorio siempre está disponible.  
  
Los administradores de servicio están también responsables de completar tareas de implementación de Active Directory en curso que se necesitan una vez completado el proceso de implementación inicial de Active Directory de Windows Server 2008. Por ejemplo, como las demandas en el aumento de directorio, los administradores de servicio crear controladores de dominio adicionales y establecerán o quitar confianzas entre dominios, según sea necesario. Por este motivo, el equipo de la implementación de Active Directory debe incluir los administradores de servicio.  
  
Debe tener cuidado para asignar roles de administrador de servicio solo a los individuos de confianza de la organización. Dado que estas personas tienen la capacidad de modificar los archivos del sistema en controladores de dominio, pueden cambiar el comportamiento de AD DS. Debes asegurarte de que los administradores de servicios en la organización son personas que están familiarizadas con el funcionamiento de las directivas de seguridad que están en lugar de la red y que conozcan la necesidad de aplicar las directivas.  
  
Los administradores de datos son usuarios dentro de un dominio responsables tanto para mantener los datos que se almacenan en AD DS, por ejemplo, cuentas de usuario y grupo y mantenimiento de los equipos que pertenecen a su dominio. Los administradores de datos controlan los subconjuntos de los objetos dentro del directorio y no tienen control sobre la instalación o la configuración del servicio de directorio.  
  
Cuentas de administrador de datos no se proporcionan de manera predeterminada. Después de que el equipo de diseño determina la forma en que los recursos de administrarse para la organización, los propietarios de dominio deben crear cuentas de administrador de datos y delegado de los permisos adecuados según el conjunto de objetos para que los administradores deben estar responsable.  
  
Es mejor limitar el número de administradores de servicio de la organización para el número mínimo necesario para garantizar que la infraestructura sigue funcionando. La mayoría de las tareas administrativas puede llevar a cabo los administradores de datos. Los administradores de servicios requieren un mucho más amplio conjunto de habilidades porque son responsables de mantener el directorio y la infraestructura que admite. Los administradores de datos solo requieren los conocimientos necesarios para administrar su parte del directorio. Dividir las asignaciones de trabajo de este modo produce ahorro de la organización porque solo un pequeño número de administradores que debas aprender a operar y mantener todo el directorio y su infraestructura.  
  
Por ejemplo, un administrador de servicios necesita comprender cómo agregar un dominio a un bosque. Esto incluye cómo instalar el software para convertir a un servidor en un controlador de dominio y cómo se manipulan el entorno de DNS para que el controlador de dominio se puede combinar sin problemas en el entorno de Active Directory. Un administrador de datos solo necesita saber cómo administrar los datos específicos que son responsables de como la creación de nuevas cuentas de usuario para los empleados de nuevo en su departamento.  
  
Implementación de AD DS requiere coordinación y la comunicación entre varios grupos de diferentes implicados en la operación de la infraestructura de red. Estos grupos deben designar a los propietarios de servicio y los datos que son responsables de representar los distintos grupos durante el proceso de diseño e implementación.  
  
Una vez completado el proyecto de implementación, seguirán estos propietarios de servicio y los datos que es responsable de la parte de la infraestructura que administrados por su grupo. En un entorno de Active Directory, estos propietarios son el propietario de bosque, el DNS de propietario de AD DS, el propietario de la topología de sitio y el propietario de la unidad organizativa. Las funciones de los propietarios de servicio y los datos se explican en las siguientes secciones.  
  
#### <a name="forest-owner"></a>Propietario del bosque  
El propietario del bosque suele ser un administrador de TI información en la organización que es responsable del proceso de implementación de Active Directory y quién es el último responsable de mantener la prestación de servicios en el bosque después de completar la implementación. El propietario de bosque asigna individuos para rellenar las demás funciones de posesión identificando personal clave dentro de la organización que pueden contribuir a la información necesaria acerca de la infraestructura de red y necesidades administrativas. El propietario del bosque es responsable de las siguientes acciones:  
  
-   Implementación de dominio raíz del bosque para crear el bosque  
  
-   Implementación de primer controlador de dominio en cada dominio para crear los dominios necesarios para el bosque  
  
-   Pertenencia a los grupos de administrador de servicios en todos los dominios del bosque  
  
-   Creación del diseño de la estructura de unidad organizativa para cada dominio en el bosque  
  
-   Delegación de autoridad administrativa a los propietarios de unidad organizativa  
  
-   Cambios en el esquema  
  
-   Cambios de configuración de todo el bosque  
  
-   Implementación de determinadas configuraciones de directiva de directiva de grupo, como directivas de cuenta de usuario de dominio como la directiva de bloqueo de cuenta y la contraseña muy específica  
  
-   Configuración de directiva de empresa que se aplican a los controladores de dominio  
  
-   Cualquier otra configuración de directiva de grupo que se aplica en el nivel de dominio  
  
El propietario del bosque tiene autoridad sobre todo el bosque. Es responsabilidad del propietario de bosque para establecer la directiva de grupo y las directivas de empresa y seleccionar a las personas que son administradores de servicio. El propietario de bosque es el propietario de un servicio.  
  
#### <a name="dns-for-ad-ds-owner"></a>DNS de propietario de AD DS  
El DNS de propietario de AD DS es la persona que tiene un conocimiento avanzado de la infraestructura DNS y el espacio de nombres existente de la organización.  
  
El DNS de propietario de AD DS es responsable de las siguientes acciones:  
  
-   Que actúa como un enlace entre el equipo de diseño y el grupo de TI que posee actualmente la infraestructura DNS  
  
-   Proporcionar la información sobre el espacio de nombres DNS existente de la organización para ayudar en la creación de nuevo espacio de nombres de Active Directory  
  
-   Trabajar con el equipo de distribución para asegurarte de que se ha implementado según las especificaciones del equipo de diseño de la nueva infraestructura DNS y que funciona correctamente  
  
-   Administrar el DNS para la infraestructura de AD DS, incluidos los datos DNS y servicios de servidor DNS  
  
El DNS de propietario de AD DS es el propietario de un servicio.  
  
#### <a name="site-topology-owner"></a>Propietario de la topología de sitio  
El propietario de la topología de sitio está familiarizado con la estructura física de la red de la organización, incluida la asignación de subredes individuales, los enrutadores y áreas de la red que están conectadas mediante vínculos lentos. El propietario de la topología de sitio es responsable de las siguientes acciones:  
  
-   Descripción de la topología de red física y cómo afecta a AD DS  
  
-   Comprender cómo la implementación de Active Directory afectará a la red  
  
-   Determinar los sitios de Active Directory lógicos que deben crearse  
  
-   Actualizar los objetos de sitio para controladores de dominio cuando se agrega una subred, modificado o eliminado  
  
-   Crear vínculos a sitios, puentes y objetos de conexión manual  
  
El propietario de la topología de sitio es el propietario de un servicio.  
  
#### <a name="ou-owner"></a>Propietario de la unidad organizativa  
El propietario de la unidad organizativa es responsable de administrar los datos almacenados en el directorio. Esta persona debe estar familiarizado con las operaciones y las directivas de seguridad que se encuentran en lugar de la red. Los propietarios de unidad organizativa pueden realizar únicamente las tareas delegadas a ellos por los administradores de servicio, y pueden realizar únicamente las tareas en las unidades organizativas a la que se asignan. Las tareas que se puede asignar al propietario de la unidad organizativa incluyen lo siguiente:  
  
-   Realizar todas las tareas de administración de cuenta dentro de su unidad organizativa asignado  
  
-   Administración de las estaciones de trabajo y los servidores miembro que pertenecen a su unidad organizativa asignado  
  
-   Delegar la autoridad a los administradores locales en sus unidades Organizativas asignado  
  
El propietario de la unidad organizativa es un propietario de datos.  
  
## <a name="BKMK_3"></a>Equipos de proyectos de creación  
Equipos de Active Directory proyecto son grupos temporales que están responsables de completar las tareas de diseño e implementación de Active Directory. Una vez completado el proyecto de implementación de Active Directory, los propietarios asumen la responsabilidad por el directorio y los equipos de proyectos pueden Desintegrar.  
  
El tamaño de los equipos del proyecto varía según el tamaño de la organización. En organizaciones pequeñas, una sola persona puede abarcar varias áreas de responsabilidad en un equipo del proyecto y participar en más de una fase de la implementación. Las grandes organizaciones pueden requerir más grandes equipos con distintas personas o incluso diferentes equipos, que abarca las distintas áreas de responsabilidad. El tamaño de los equipos no es importante, siempre que se asignan en todas las áreas de responsabilidad y se cumplen los objetivos de diseño de la organización.  
  
### <a name="identifying-potential-forest-owners"></a>Identificar a los propietarios de bosque posibles  
Identificar los grupos dentro de la organización que el propietario y controlan los recursos necesarios para proporcionar servicios de directorio a los usuarios a la red. Estos grupos se consideran propietarios de bosque potenciales.  
  
La separación de datos y el servicio de administración en AD DS hace posible para la infraestructura de TI (o los grupos) de una organización para administrar el servicio de directorio, mientras que los administradores locales en cada grupo de administran los datos que pertenecen a sus propios grupos. Los propietarios de bosque posibles tienen la autoridad necesaria sobre la infraestructura de red para implementar y admitir AD DS.  
  
Para las organizaciones que tienen una infraestructura centralizada grupo de TI, el grupo de TI suele ser el propietario de bosque y, por lo tanto, el propietario de bosque posibles para las implementaciones futuras. Las organizaciones que incluyen un número de independientes de infraestructura grupos tienen un número de posibles propietarios de bosque. Si la organización ya tiene una infraestructura de Active Directory en su lugar, los propietarios de bosque actual también son los propietarios de bosque posibles para las implementaciones de nuevo.  
  
Selecciona uno de los propietarios de bosque posibles para que actúe como el propietario del bosque para cada bosque que estés considerando para la implementación. Estos propietarios de bosque posibles son responsables de trabajar con el equipo de diseño para determinar si se va a implementar su bosque realmente o si un curso de acción (por ejemplo, la unión de otro bosque existente) alternativo es un mejor uso de los recursos disponibles y aún satisfaga sus necesidades. El propietario del bosque (o propietarios) en la organización son miembros del equipo de diseño de Active Directory.  
  
### <a name="establishing-a-design-team"></a>Establecimiento de un equipo de diseño  
El equipo de diseño de Active Directory es responsable de recopilar toda la información necesaria para tomar decisiones sobre el diseño de la estructura lógica de Active Directory.  
  
Las responsabilidades del equipo de diseño incluyen lo siguiente:  
  
-   Determinar el número de dominios y bosques son necesarios y ¿cuáles son las relaciones entre los dominios y bosques  
  
-   Trabajar con los propietarios de datos para garantizar que cumple con el diseño de la seguridad y los requisitos administrativos  
  
-   Trabajar con los administradores de red actual para garantizar que la infraestructura de red actual admite el diseño y que el diseño no afectará negativamente existentes aplicaciones implementadas en la red  
  
-   Trabajar con los representantes del grupo de seguridad de la organización para garantizar que el diseño cumple con las directivas de seguridad establecidas  
  
-   Diseñar las estructuras de unidad organizativa que permiten que los niveles de protección adecuados y la delegación de autoridad adecuada a los propietarios de datos  
  
-   Trabajar con el equipo de distribución para probar el diseño en un laboratorio entorno para asegurarse de que funciona como está previsto y modificar el diseño que necesite para solucionar los problemas que se producen  
  
-   Crear un diseño de la topología de sitio que cumpla los requisitos de replicación del bosque y evita que la sobrecarga de ancho de banda disponible. Para obtener más información sobre el diseño de la topología de sitios, consulta [diseñar el sitio topología para Windows Server 2008 AD DS](https://technet.microsoft.com/library/cc772013.aspx).  
  
-   Trabajar con el equipo de implementación para garantizar que el diseño se implementa correctamente  
  
El equipo de diseño incluye a los siguientes miembros:  
  
-   Propietarios de bosque potenciales  
  
-   Arquitecto de proyecto  
  
-   Administrador de proyectos  
  
-   Personas que son responsables de establecer y mantener directivas de seguridad en la red  
  
Durante el proceso de diseño de la estructura lógica, el equipo de diseño identifica los propietarios de otros. Estas personas deben empezar a participar en el proceso de diseño en cuanto se identifican. Después de que se entregue el proyecto de implementación para el equipo de distribución, el equipo de diseño es responsable de supervisar el proceso de implementación para garantizar que el diseño se implementa correctamente. El equipo de diseño también realiza cambios en el diseño en función de los comentarios de las pruebas.  
  
### <a name="establishing-a-deployment-team"></a>Establecimiento de un equipo de distribución  
El equipo de la implementación de Active Directory es responsable de probar e implementar el diseño de la estructura lógica de Active Directory. Esto incluye las siguientes tareas:  
  
-   Establecimiento de un entorno de prueba que lo suficientemente emula el entorno de producción  
  
-   Probar el diseño mediante la implementación de la estructura de bosque y dominio propuesta en un entorno de laboratorio para comprobar que cumple los objetivos de cada propietario de la función  
  
-   Desarrollar y probar los escenarios de migración propuestos por el diseño en un entorno de laboratorio  
  
-   Asegúrate de que cada propietario inicia sesión en el proceso de prueba para garantizar que se está probando las características de diseño correcto  
  
-   Pruebas de la operación de implementación en un entorno piloto  
  
Cuando se completan el diseño y las tareas de prueba, el equipo de distribución realiza las siguientes tareas:  
  
-   Crea los bosques y dominios según el diseño de la estructura lógica de Active Directory  
  
-   Crea los sitios y objetos vínculos a sitios según sea necesario en función del diseño de la topología de sitio  
  
-   Garantiza que la infraestructura DNS está configurada para admitir AD DS y que los nuevos espacios de nombres están integrados en el espacio de nombres existente de la organización  
  
El equipo de la implementación de Active Directory incluye a los siguientes miembros:  
  
-   Propietario del bosque  
  
-   DNS de propietario de AD DS  
  
-   Propietario de la topología de sitio  
  
-   Propietarios de unidad organizativa  
  
El equipo de distribución funciona con los administradores de servicio y los datos durante la fase de implementación para garantizar que los miembros del equipo de operaciones están familiarizados con el nuevo diseño. Esto ayuda a garantizar una transición suave de propiedad cuando se complete la operación de implementación. Al final del proceso de implementación, la responsabilidad para mantener el nuevo entorno de Active Directory se pasa al equipo de operaciones.  
  
### <a name="documenting-the-design-and-deployment-teams"></a>Documentar los equipos de diseño e implementación  
Documentar los nombres e información de contacto para las personas que participarán en el diseño e implementación de AD DS. Identificar quién será responsable de cada rol en los equipos de diseño e implementación. Inicialmente, esta lista incluye los propietarios de bosque posibles, el Administrador de proyecto y el arquitecto del proyecto. Al determinar el número de bosques que se va a implementar, es posible que debas crear nuevos equipos de diseño para bosques adicionales. Ten en cuenta que tendrás que actualizar la documentación a medida que cambia de equipo y se identifican los diversos propietarios de Active Directory durante el proceso de diseño. Para que una hoja de cálculo que le ayudarán a documentar los equipos de diseño e implementación para cada bosque, descarga Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip de trabajo Aid para Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)) y abre "Diseño y distribución información del equipo" (DSSLOGI_1.doc).  
  


