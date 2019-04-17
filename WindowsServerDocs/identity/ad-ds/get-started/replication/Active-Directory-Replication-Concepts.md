---
ms.assetid: 4cc9c16c-1928-4dce-a3a8-6229be28eb65
title: "Conceptos de réplica de Active Directory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 45753c048f9bb1cb174daade1d408b88b3686b4b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="active-directory-replication-concepts"></a>Conceptos de réplica de Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Antes de diseñar la topología de sitios, familiarizarse con algunos conceptos de réplica de Active Directory.  
  
-   [Objeto de conexión](#BKMK_1)  
  
-   [KCC](#BKMK_2)  
  
-   [Funcionalidad de conmutación por error](#BKMK_3)  
  
-   [Subred](#BKMK_4)  
  
-   [Sitio](#BKMK_5)  
  
-   [Vínculo de sitio](#BKMK_6)  
  
-   [Puente](#BKMK_7)  
  
-   [Transitividad del vínculo de sitio](#BKMK_8)  
  
-   [Servidor de catálogo global](#BKMK_9)  
  
-   [El almacenamiento en caché de la pertenencia a grupos universales](#BKMK_10)  
  
## <a name="BKMK_1"></a>Objeto de conexión  
Un objeto de conexión es un objeto de Active Directory que representa una conexión de replicación desde un controlador de dominio de origen a un controlador de dominio de destino. Un controlador de dominio es un miembro de un único sitio y se representa en el sitio mediante un objeto de servidor en servicios de dominio de Active Directory (AD DS). Cada objeto de servidor tiene un elemento secundario configuración NTDS objeto que representa el controlador de dominio de replicación en el sitio.  
  
El objeto de conexión es un elemento secundario del objeto configuración NTDS en el servidor de destino. Para que la replicación entre dos controladores de dominio, el objeto de servidor de uno debe tener un objeto de conexión que representa la replicación de entrada de la otra. Todas las conexiones de replicación para un controlador de dominio se almacenan como objetos de conexión en el objeto configuración NTDS. El objeto de conexión identifica el servidor de origen de replicación, contiene un programa de replicación y especifica un transporte de replicación.  
  
Conocimiento coherencia Comprobador () crea objetos de conexión automáticamente, pero también pueden crearse manualmente. Los objetos de conexión creados por el KCC aparecen en los sitios de Active Directory y el complemento Servicios como **<automatically generated>** y se consideran suficientes en condiciones normales de uso. Creado por un administrador de objetos de conexión manualmente se crean objetos de conexión. Un objeto de conexión creado manualmente se identifica por el nombre asignado por el administrador cuando se creó. Al modificar una **<automatically generated>** objeto de conexión, se convierte en un objeto de conexión administrativa modificado y el objeto aparece en la forma de GUID. El KCC no realizar cambios en objetos de conexión manual o modificado.  
  
## <a name="BKMK_2"></a>KCC  
El KCC es un proceso integrado que se ejecuta en todos los controladores de dominio y genera la topología de replicación de bosque de Active Directory. Crea las topologías de replicación diferente dependiendo de si se produce la replicación dentro de un sitio (dentro de un sitio), o entre sitios (entre sitios). El KCC también ajusta dinámicamente la topología para dar cabida a la adición de nuevos controladores de dominio, la eliminación de controladores de dominio existentes, el movimiento de los controladores de dominio desde y hacia los sitios, cambia los costos y planes y controladores de dominio que no están disponibles temporalmente o en un estado de error.  
  
Dentro de un sitio, las conexiones entre controladores de dominio grabable siempre se organizan en un círculo de bidireccional con conexiones de accesos directos adicionales para reducir la latencia en sitios de gran tamaño. Por otro lado, la topología es una disposición en capas de expansión de árboles, lo que significa que una conexión entre sitios existe entre las dos sitios para cada partición de directorio y por lo general, no contiene conexiones de acceso directo. Para obtener más información acerca de expansión de árboles y topología de réplica de Active Directory, consulte la replicación topología referencia técnica Active Directory ([https://go.microsoft.com/fwlink/?LinkID=93578](https://go.microsoft.com/fwlink/?LinkID=93578)).  
  
En cada controlador de dominio, crea las rutas de replicación mediante la creación de objetos de conexión entrante unidireccional que definen las conexiones de otros controladores de dominio. Controladores de dominio en el mismo sitio, crea objetos de conexión automáticamente sin intervención administrativa. Si tienes más de un sitio, configurar vínculos a sitios entre sitios y un único en cada sitio automáticamente crea conexiones entre sitios también.  
  
### <a name="kcc-improvements-for-windows-server-2008-rodcs"></a>Mejoras de KCC para Windows Server 2008 RODC  

Hay una serie de mejoras de KCC para acomodar el controlador de dominio de solo lectura recientemente disponibles (RODC) en Windows Server 2008. Un escenario de implementación típico de RODC es la sucursal. La topología de réplica de Active Directory más frecuentemente implementada en este escenario se basa en un diseño de concentrador y radio, donde los controladores de dominio de rama de varios sitios replican con un pequeño número de servidores cabeza de un sitio de concentrador.  
  
Una de las ventajas de implementar RODC en este escenario es unidireccional replicación. Servidores cabeza de puente no están necesarios replicar del RODC, lo que reduce la administración y el uso de la red.  
  
Sin embargo, uno de los retos administrativa resaltado por la topología radial en versiones anteriores del sistema operativo Windows Server es que después de agregar un nuevo controlador de dominio de cabeza de puente en el hub, no hay ningún mecanismo automático para redistribuir las conexiones de replicación entre los controladores de dominio de la rama y los controladores de dominio de navegación centralizada para aprovechar el nuevo controlador de dominio de concentrador.  
  
Para los controladores de dominio de Windows Server 2003, puede equilibrar la carga de trabajo mediante una herramienta como Adlb.exe de la Guía de implementación de Windows Server 2003 rama de Office ([https://go.microsoft.com/fwlink/?LinkID=28523](https://go.microsoft.com/fwlink/?LinkID=28523)).  
  
Para Windows Server 2008 RODC, el funcionamiento normal del KCC proporciona algún proceso, lo que elimina la necesidad de usar una herramienta como Adlb.exe adicional. La nueva funcionalidad está habilitada de manera predeterminada. Se puede deshabilitar agregando la siguiente clave del registro se establece en el RODC:  
  
**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters**  
  
**"BH aleatorio balanceo de la misma permitido"**  
**1 = habilitado (predeterminado), 0 = deshabilitado**  
  
Para obtener más información sobre cómo funcionan estas mejoras KCC, consulte Diseño e implementación de Active Directory Domain Services para las sucursales ([https://go.microsoft.com/fwlink/?LinkId=107114](https://go.microsoft.com/fwlink/?LinkId=107114)).  
  
## <a name="BKMK_3"></a>Funcionalidad de conmutación por error  
Sitios asegurarse de que la replicación se enruta alrededor de los errores de red y controladores de dominio sin conexión. Se ejecuta en los intervalos especificados para ajustar la topología de replicación de los cambios que se producen en AD DS, por ejemplo, cuando se agregan los nuevos controladores de dominio y nuevos se crean los sitios. El KCC revisa el estado de replicación de las conexiones existentes para determinar si las conexiones no funciona. Si una conexión no funciona debido a un controlador de dominio con errores, KCC crea automáticamente las conexiones temporales a otros partners de replicación (si está disponible) para asegurarse de que la replicación ocurre. Si todos los controladores de dominio de un sitio no están disponibles, automáticamente crea conexiones de replicación entre controladores de dominio de otro sitio.  
  
## <a name="BKMK_4"></a>Subred  
Una subred es un segmento de una red TCP/IP a la que se asignan a un conjunto de direcciones IP lógicas. Las subredes agrupan los equipos de manera que identifica su proximidad física en la red. Los objetos de subred en AD DS identifican las direcciones de red que se usan para asignar equipos a los sitios.  
  
## <a name="BKMK_5"></a>Sitio  
Los sitios son objetos de Active Directory que representan una o varias subredes TCP/IP con conexiones de red extremadamente rápida y confiable. Información del sitio permite a los administradores configurar acceso y Active Directory replicación para optimizar el uso de la red física. Los objetos de sitio se asocian con un conjunto de subredes y cada controlador de dominio en un bosque está asociado con un sitio de Active Directory según su dirección IP. Sitios pueden hospedar los controladores de dominio desde más de un dominio, y un dominio puede representarse en más de un sitio.  
  
## <a name="BKMK_6"></a>Vínculo de sitio  
Vínculos a sitios son objetos de Active Directory que representan las rutas de acceso lógicas que KCC se usa para establecer una conexión para la replicación de Active Directory. Un objeto de enlace de sitio representa un conjunto de sitios que se pueden comunicar a un costo uniforme a través de un transporte especificado.  
  
Todos los sitios incluidos en el vínculo de sitio se consideran estar conectado mediante el mismo tipo de red. Los sitios deben vincularse manualmente a otros sitios mediante el uso de vínculos a sitios para que los controladores de dominio de un sitio pueden replicar los cambios de directorio de controladores de dominio en otro sitio. Dado que los vínculos a sitios no corresponden a la ruta de acceso real seguida por los paquetes de red en la red física durante la duplicación, no es necesario crear vínculos a sitios redundante para mejorar la eficiencia de réplica de Active Directory.  
  
Cuando dos sitios están conectados mediante un vínculo a sitios, el sistema de replicación crea automáticamente las conexiones entre los controladores de dominio específico de cada sitio que se denominan servidores cabeza de puente. En Windows Server 2008, todos los controladores de dominio en un sitio que hospedan la misma partición de directorio son candidatos para seleccionado como servidores cabeza. Las conexiones de replicación creadas por el KCC aleatoriamente se distribuyen entre todos los servidores de cabeza candidato en un sitio para compartir la carga de trabajo de replicación. De manera predeterminada, la toma del proceso de selección aleatoria coloca una sola vez, cuando se agregan primero los objetos de conexión al sitio.  
  
## <a name="BKMK_7"></a>Puente  
Un puente es un objeto de Active Directory que representa un conjunto de vínculos a sitios, todos cuyos sitios pueden comunicarse mediante un transporte común. Puentes habilitar los controladores de dominio que no están conectados directamente mediante un vínculo de comunicación para replicar entre sí. Por lo general, un puente corresponde a un enrutador (o un conjunto de enrutadores) en una red IP.  
  
De manera predeterminada, el KCC puede formar una ruta transitiva a través de vínculos a todos los sitios que tienen en común de algunos sitios. Si se deshabilita este comportamiento, cada vínculo representa su propia red aislada y clara. Conjuntos de vínculos a sitios que pueden tratarse como una única ruta se expresan mediante un puente. Cada puente representa un entorno de comunicación aislado para el tráfico de red.  
  
Puentes son un mecanismo para representar lógicamente transitiva conectividad física entre sitios. Un puente permite KCC usar cualquier combinación de los vínculos a sitios incluidos para determinar la ruta menos costosa para interconexión de particiones de directorio contenidas en esos sitios. El puente no proporciona conectividad real a los controladores de dominio. Si se quita el puente, replicación a través de los vínculos a sitios combinada continuará hasta que el KCC quita los vínculos.  
  
Puentes solo son necesarias si un sitio contiene un controlador de dominio aloja una partición de directorio que no también está hospedada en un controlador de dominio en un sitio adyacente, pero un controlador de dominio aloja esa partición de directorio se encuentra en uno o más sitios en el bosque. Sitios adyacentes se definen como cualquier dos o más sitios incluidos en el vínculo de un único sitio.  
  
Un puente crea una conexión entre dos vínculos a sitios, que proporciona una ruta de acceso transitiva entre dos sitios desconectados mediante el uso de un sitio provisional lógica. Para los fines de generador de la topología (ISTG), el puente implica conectividad física mediante el sitio provisional. El puente no implica que un controlador de dominio en el sitio provisional proporcionará la ruta de acceso de replicación. Sin embargo, esto podría ser el caso si el sitio provisional contenía un controlador de dominio que hospeda la partición de directorio para replicar, en cuyo caso un puente no es necesario.  
  
Se agrega el costo de cada vínculo del sitio, crear un coste sumado de la ruta de acceso resultante. Si el sitio provisional no contiene un controlador de dominio que hospeda la partición de directorio y no existe un vínculo de costo más bajo, se usaría el puente. Si el sitio provisional encuentra un controlador de dominio que hospeda la partición de directorio, dos sitios desconectados serían establecer conexiones de replicación con el controlador de dominio provisional y no usar el puente.  
  
## <a name="BKMK_8"></a>Transitividad del vínculo de sitio  
De manera predeterminada, todos los vínculos son transitivas o "puente". Cuando están conectados de vínculos a sitios y las programaciones superponer, crea las conexiones de replicación que determinan los asociados de replicación de controlador de dominio entre sitios, que los sitios no están conectados directamente mediante vínculos a sitios, pero se conectan transitivamente a través de un conjunto de sitios comunes. Esto significa que cualquier sitio puede conectarse a cualquier otro sitio mediante una combinación de vínculos a sitios.  
  
En general, para una red totalmente enrutada, no es necesario crear cualquier sitio puentes de vínculos a menos que quieras controlar el flujo de cambios de replicación. Si la red no se enruta totalmente, puentes deben crearse para evitar los intentos de replicación imposible. Todos los vínculos a sitios para un transporte específico implícitamente pertenecen a un puente único para ese transporte. Los puentes predeterminados para vínculos a sitios se produce automáticamente, y ningún objeto de Active Directory representa ese puente. La **enlazar todos los vínculos a sitios** configuración, puedes encontrar en las propiedades de contenedores de transporte entre sitios el IP y el protocolo Simple de transferencia de correo (SMTP), implementa automática puentes de vínculos.  
  
> [!NOTE]  
> Replicación SMTP no se admitirán en versiones futuras de AD DS; por lo tanto, no se recomienda crear objetos de vínculos a sitios en el contenedor SMTP.  
  
## <a name="BKMK_9"></a>Servidor de catálogo global  
Un servidor de catálogo global es un controlador de dominio que almacena información sobre todos los objetos en el bosque, para que las aplicaciones puedan buscar AD DS sin hacer referencia a los controladores de dominio específico que almacenan los datos solicitados. Como todos los controladores de dominio, un servidor de catálogo global almacena réplicas completas, puede escribir el esquema y la configuración de particiones de directorio y una réplica de escritura completa de la partición de directorio de dominio del dominio que lo aloja. Además, un servidor de catálogo global almacena una réplica parcial de solo lectura de todos los demás dominios del bosque. Réplicas dominio parcial de solo lectura contienen todos los objetos en el dominio, pero solo un subconjunto de los atributos (los atributos que se usan normalmente para buscar el objeto).  
  
## <a name="BKMK_10"></a>El almacenamiento en caché de la pertenencia a grupos universales  
El almacenamiento en caché de la pertenencia a grupos universales, permite que el controlador de dominio en caché la información de pertenencia a grupos universales para los usuarios. Puedes habilitar los controladores de dominio que ejecutan Windows Server 2008 en caché la pertenencia a grupos universales mediante el complemento Servicios y sitios de Active Directory.  
  
Habilitar el almacenamiento en caché de la pertenencia a grupos universales, elimina la necesidad de un servidor de catálogo global de cada sitio en un dominio, lo que reduce el uso de ancho de banda de red porque no es necesario replicar todos los objetos que se encuentra en el bosque de un controlador de dominio. Además, reduce los tiempos de inicio de sesión porque los controladores de dominio de autenticación no siempre es necesario tener acceso a un catálogo global para obtener información de pertenencia a grupos universales. Para obtener más información sobre cuándo usar el almacenamiento en caché de la pertenencia a grupos universales, consulta [planeación de la ubicación del servidor de catálogo Global](../../../ad-ds/plan/Planning-Global-Catalog-Server-Placement.md).  
  


