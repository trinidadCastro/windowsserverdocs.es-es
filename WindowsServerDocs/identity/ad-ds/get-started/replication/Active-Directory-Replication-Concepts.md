---
ms.assetid: 4cc9c16c-1928-4dce-a3a8-6229be28eb65
title: Conceptos de replicación de Active Directory
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: c1392d813497980d6060cc22fe35afa17c4043e0
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969952"
---
# <a name="active-directory-replication-concepts"></a>Conceptos de replicación de Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Antes de diseñar la topología del sitio, familiarícese con algunos Active Directory conceptos de replicación.

-   [Objeto de conexión](#BKMK_1)

-   [KCC](#BKMK_2)

-   [Funcionalidad de conmutación por error](#BKMK_3)

-   [Subred](#BKMK_4)

-   [Sitio](#BKMK_5)

-   [Vínculo a sitios](#BKMK_6)

-   [Puente de vínculo a sitios](#BKMK_7)

-   [Transitividad de vínculo de sitio](#BKMK_8)

-   [Servidor de catálogo global](#BKMK_9)

-   [Almacenamiento en caché de pertenencia al grupo universal](#BKMK_10)

## <a name="connection-object"></a><a name="BKMK_1"></a>Objeto de conexión
Un objeto de conexión es un objeto Active Directory que representa una conexión de replicación de un controlador de dominio de origen a un controlador de dominio de destino. Un controlador de dominio es miembro de un único sitio y se representa en el sitio mediante un objeto de servidor en Active Directory Domain Services (AD DS). Cada objeto de servidor tiene un objeto de configuración NTDS secundario que representa el controlador de dominio de replicación en el sitio.

El objeto de conexión es un elemento secundario del objeto de configuración NTDS en el servidor de destino. Para que se produzca la replicación entre dos controladores de dominio, el objeto de servidor de uno debe tener un objeto de conexión que represente la replicación de entrada desde el otro. Todas las conexiones de replicación de un controlador de dominio se almacenan como objetos de conexión en el objeto de configuración NTDS. El objeto de conexión identifica el servidor de origen de replicación, contiene una programación de replicación y especifica un transporte de replicación.

El comprobador de coherencia de la información (KCC) crea los objetos de conexión automáticamente, pero también se pueden crear manualmente. Los objetos de conexión creados por el KCC aparecen en el complemento sitios y servicios de Active Directory como **<automatically generated>** y se consideran adecuados en condiciones de funcionamiento normales. Los objetos de conexión creados por un administrador son objetos de conexión creados manualmente. Un objeto de conexión creado manualmente se identifica mediante el nombre asignado por el administrador cuando se creó. Cuando se modifica un **<automatically generated>** objeto de conexión, se convierte en un objeto de conexión modificado administrativamente y el objeto aparece en forma de GUID. El KCC no realiza cambios en los objetos de conexión manuales o modificados.

## <a name="kcc"></a><a name="BKMK_2"></a>KCC
El KCC es un proceso integrado que se ejecuta en todos los controladores de dominio y genera una topología de replicación para el bosque de Active Directory. El KCC crea topologías de replicación independientes en función de si la replicación se está produciendo en un sitio (dentro del sitio) o entre sitios (entre sitios). El KCC también ajusta dinámicamente la topología para dar cabida a la adición de nuevos controladores de dominio, la eliminación de controladores de dominio existentes, el movimiento de controladores de dominio hacia y desde sitios, el cambio de costos y programaciones y controladores de dominio que no están disponibles temporalmente o en un estado de error.

Dentro de un sitio, las conexiones entre los controladores de dominio de escritura siempre están organizadas en un anillo bidireccional, con conexiones de acceso directo adicionales para reducir la latencia en sitios de gran tamaño. Por otro lado, la topología entre sitios es una capa de árboles de expansión, lo que significa que una conexión entre sitios existe entre dos sitios cualesquiera para cada partición de directorio y, por lo general, no contiene conexiones de acceso directo. Para obtener más información acerca de la expansión de árboles y Active Directory topología de replicación, consulte Active Directory referencia técnica de la topología de replicación ( [https://go.microsoft.com/fwlink/?LinkID=93578](https://go.microsoft.com/fwlink/?LinkID=93578) ).

En cada controlador de dominio, el KCC crea rutas de replicación mediante la creación de objetos de conexión de entrada unidireccionales que definen conexiones de otros controladores de dominio. En el caso de los controladores de dominio del mismo sitio, el KCC crea objetos de conexión automáticamente sin intervención administrativa. Si tiene más de un sitio, configure los vínculos de sitio entre los sitios y un solo KCC en cada sitio creará automáticamente las conexiones entre sitios.

### <a name="kcc-improvements-for-windows-server-2008-rodcs"></a>Mejoras de KCC para Windows Server 2008 RODC

Hay una serie de mejoras de KCC para alojar el controlador de dominio de solo lectura (RODC) recién disponible en Windows Server 2008. Un escenario de implementación típico de RODC es la sucursal. La Active Directory topología de replicación más común en este escenario se basa en un diseño de concentrador y radio, donde los controladores de dominio de la sucursal en varios sitios se replican con un pequeño número de servidores de cabeza de puente en un sitio concentrador.

Una de las ventajas de implementar RODC en este escenario es la replicación unidireccional. No es necesario que los servidores cabeza de puente se repliquen desde el RODC, lo que reduce el uso de la red y la administración.

Sin embargo, un desafío administrativo resaltado por la topología de concentrador-radio en versiones anteriores del sistema operativo Windows Server es que, después de agregar un nuevo controlador de dominio de cabeza de puente en el concentrador, no hay ningún mecanismo automático para redistribuir las conexiones de replicación entre los controladores de dominio de la sucursal y los controladores de dominio del concentrador para aprovechar el nuevo controlador

En el caso de los controladores de dominio de Windows Server 2003, puede reequilibrar la carga de trabajo mediante una herramienta como Adlb.exe de la guía de implementación de sucursales de Windows Server 2003 ( [https://go.microsoft.com/fwlink/?LinkID=28523](https://go.microsoft.com/fwlink/?LinkID=28523) ).

En el caso de los RODC de Windows Server 2008, el funcionamiento normal del KCC proporciona algún reequilibrio, lo que elimina la necesidad de usar una herramienta adicional como Adlb.exe. La nueva funcionalidad está habilitada de forma predeterminada. Puede deshabilitarlo agregando el siguiente conjunto de claves del registro en el RODC:

**HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services\NTDS\Parameters**

**"Se permite el equilibrio de BH aleatorio"** 
 **1 = habilitado (valor predeterminado), 0 = deshabilitado**

Para obtener más información acerca de cómo funcionan estas mejoras de KCC, consulte planear e implementar Active Directory Domain Services para sucursales ( [https://go.microsoft.com/fwlink/?LinkId=107114](https://go.microsoft.com/fwlink/?LinkId=107114) ).

## <a name="failover-functionality"></a><a name="BKMK_3"></a>Funcionalidad de conmutación por error
Los sitios aseguran que la replicación se enrute en torno a errores de red y controladores de dominio sin conexión. El KCC se ejecuta a intervalos especificados para ajustar la topología de replicación de los cambios que se producen en AD DS, como cuando se agregan nuevos controladores de dominio y se crean nuevos sitios. El KCC revisa el estado de replicación de las conexiones existentes para determinar si las conexiones no funcionan. Si una conexión no funciona debido a un error en un controlador de dominio, el KCC crea automáticamente conexiones temporales a otros asociados de replicación (si están disponibles) para asegurarse de que se produce la replicación. Si todos los controladores de dominio de un sitio no están disponibles, el KCC crea automáticamente las conexiones de replicación entre los controladores de dominio de otro sitio.

## <a name="subnet"></a><a name="BKMK_4"></a>Subnet
Una subred es un segmento de una red TCP/IP a la que se asigna un conjunto de direcciones IP lógicas. Las subredes agrupan los equipos de forma que identifiquen su proximidad física en la red. Los objetos de subred de AD DS identifican las direcciones de red que se usan para asignar equipos a sitios.

## <a name="site"></a><a name="BKMK_5"></a>Sitio
Los sitios son Active Directory objetos que representan una o más subredes TCP/IP con conexiones de red rápidas y de alta confiabilidad. La información del sitio permite a los administradores configurar Active Directory el acceso y la replicación para optimizar el uso de la red física. Los objetos de sitio están asociados a un conjunto de subredes y cada controlador de dominio de un bosque está asociado a un sitio de Active Directory según su dirección IP. Los sitios pueden hospedar controladores de dominio de más de un dominio y un dominio puede representarse en más de un sitio.

## <a name="site-link"></a><a name="BKMK_6"></a>Vínculo a sitios
Los vínculos a sitios son Active Directory objetos que representan rutas de acceso lógicas que el KCC usa para establecer una conexión para la replicación de Active Directory. Un objeto de vínculo a sitios representa un conjunto de sitios que pueden comunicarse a un costo uniforme a través de un transporte entre sitios especificado.

Todos los sitios contenidos en el vínculo de sitio se consideran conectados mediante el mismo tipo de red. Los sitios deben vincularse manualmente a otros sitios mediante vínculos a sitios para que los controladores de dominio de un sitio puedan replicar los cambios de directorio desde los controladores de dominio de otro sitio. Dado que los vínculos a sitios no corresponden a la ruta de acceso real tomada por los paquetes de red de la red física durante la replicación, no es necesario crear vínculos de sitios redundantes para mejorar el Active Directory la eficacia de la replicación.

Cuando dos sitios están conectados mediante un vínculo a sitios, el sistema de replicación crea automáticamente conexiones entre controladores de dominio específicos en cada sitio denominado servidores cabeza de puente. En Windows Server 2008, todos los controladores de dominio de un sitio que hospedan la misma partición de directorio son candidatos para ser seleccionados como servidores cabeza de puente. Las conexiones de replicación creadas por el KCC se distribuyen aleatoriamente entre todos los servidores cabeza de puente candidatos de un sitio para compartir la carga de trabajo de replicación. De forma predeterminada, el proceso de selección aleatoria solo tiene lugar una vez, cuando los objetos de conexión se agregan por primera vez al sitio.

## <a name="site-link-bridge"></a><a name="BKMK_7"></a>Puente de vínculo a sitios
Un puente de vínculos A sitios es un objeto Active Directory que representa un conjunto de vínculos a sitios, todos cuyos sitios pueden comunicarse mediante un transporte común. Los puentes de vínculos a sitios permiten que los controladores de dominio que no están conectados directamente mediante un vínculo de comunicación se repliquen entre sí. Normalmente, un puente de vínculo a sitios se corresponde con un enrutador (o un conjunto de enrutadores) en una red IP.

De forma predeterminada, el KCC puede formar una ruta transitiva a través de todos los vínculos de sitios que tienen algunos sitios en común. Si este comportamiento está deshabilitado, cada vínculo de sitio representa su propia red distinta y aislada. Los conjuntos de vínculos a sitios que se pueden tratar como una sola ruta se expresan a través de un puente de vínculos a sitios. Cada puente representa un entorno de comunicación aislado para el tráfico de red.

Los puentes de vínculos a sitios son un mecanismo para representar lógicamente la conectividad física transitiva entre sitios. Un puente de vínculo a sitios permite al KCC usar cualquier combinación de los vínculos de sitio incluidos para determinar la ruta menos costosa para interconectar las particiones de directorio que se mantienen en esos sitios. El puente de vínculo a sitios no proporciona conectividad real a los controladores de dominio. Si se quita el puente de vínculo a sitios, la replicación a través de los vínculos de sitio combinados continuará hasta que el KCC Quite los vínculos.

Los puentes de vínculos a sitios solo son necesarios si un sitio contiene un controlador de dominio que hospeda una partición de directorio que no se hospeda también en un controlador de dominio de un sitio adyacente, pero un controlador de dominio que hospeda esa partición de directorio se encuentra en uno o varios sitios del bosque. Los sitios adyacentes se definen como dos o más sitios incluidos en un solo vínculo a sitios.

Un puente de vínculo a sitios crea una conexión lógica entre dos vínculos a sitios, proporcionando una ruta de acceso transitiva entre dos sitios desconectados mediante un sitio provisional. Para los fines del generador de topología entre sitios (ISTG), el puente implica conectividad física mediante el sitio provisional. El puente no implica que un controlador de dominio del sitio provisional proporcione la ruta de replicación. Sin embargo, este sería el caso si el sitio provisional contenía un controlador de dominio que hospedaba la partición de directorio que se va a replicar, en cuyo caso no es necesario un puente de vínculo a sitio.

Se agrega el costo de cada vínculo de sitio, lo que crea un costo sumado para el trazado resultante. El puente de vínculo a sitios se utilizaría si el sitio provisional no contiene un controlador de dominio que hospede la partición de directorio y no existe un vínculo de menor costo. Si el sitio provisional contenía un controlador de dominio que hospeda la partición de directorio, dos sitios desconectados configurarían las conexiones de replicación con el controlador de dominio provisional y no usarán el puente.

## <a name="site-link-transitivity"></a><a name="BKMK_8"></a>Transitividad de vínculo de sitio
De forma predeterminada, todos los vínculos a sitios son transitivos o "conectados". Cuando los vínculos a sitios están enlazados y las programaciones se superponen, el KCC crea conexiones de replicación que determinan los asociados de replicación del controlador de dominio entre sitios, donde los sitios no están directamente conectados mediante vínculos a sitios sino que están conectados de forma transitiva a través de un conjunto de sitios comunes. Esto significa que puede conectar cualquier sitio a cualquier otro sitio a través de una combinación de vínculos a sitios.

En general, para una red totalmente enrutada, no es necesario crear ningún puente de vínculo a sitios a menos que desee controlar el flujo de cambios de replicación. Si la red no está totalmente enrutada, deben crearse puentes de vínculo de sitio para evitar intentos de replicación imposibles. Todos los vínculos a sitios para un transporte específico pertenecen implícitamente a un puente de vínculo de sitio único para ese transporte. El puente predeterminado para los vínculos a sitios se produce automáticamente y ningún objeto Active Directory representa ese puente. La configuración de **puente todos los vínculos a sitios** , que se encuentra en las propiedades de los contenedores de transporte entre sitios IP y Protocolo simple de transferencia de correo (SMTP), implementa el puente de vínculo de sitio automático.

> [!NOTE]
> La replicación SMTP no se admitirá en versiones futuras de AD DS; por lo tanto, no se recomienda crear objetos de vínculos de sitio en el contenedor de SMTP.

## <a name="global-catalog-server"></a><a name="BKMK_9"></a>Servidor de catálogo global
Un servidor de catálogo global es un controlador de dominio que almacena información sobre todos los objetos del bosque, de modo que las aplicaciones pueden buscar AD DS sin hacer referencia a controladores de dominio específicos que almacenan los datos solicitados. Al igual que todos los controladores de dominio, un servidor de catálogo global almacena réplicas completas y de escritura de las particiones de directorio de esquema y de configuración, y una réplica completa de escritura de la partición de directorio de dominio para el dominio que hospeda. Además, un servidor de catálogo global almacena una réplica parcial de solo lectura de todos los demás dominios del bosque. Las réplicas de dominio parciales de solo lectura contienen todos los objetos del dominio, pero solo un subconjunto de los atributos (los atributos que se usan con más frecuencia para buscar el objeto).

## <a name="universal-group-membership-caching"></a><a name="BKMK_10"></a>Almacenamiento en caché de pertenencia al grupo universal
El almacenamiento en caché de pertenencia al grupo universal permite al controlador de dominio almacenar en caché información de pertenencia al grupo universal para los usuarios. Puede habilitar los controladores de dominio que ejecutan Windows Server 2008 para almacenar en caché las pertenencias a grupos universales mediante el complemento sitios y servicios de Active Directory.

La habilitación del almacenamiento en caché de la pertenencia a grupos universales elimina la necesidad de un servidor de catálogo global en todos los sitios de un dominio, lo que minimiza el uso de ancho de banda de red porque un controlador de dominio no necesita replicar todos los objetos ubicados en el bosque. También reduce los tiempos de inicio de sesión porque los controladores de dominio de autenticación no siempre necesitan acceder a un catálogo global para obtener información de pertenencia al grupo universal. Para obtener más información acerca de Cuándo usar el almacenamiento en caché de pertenencia a grupos universales, consulte [planear la selección de ubicación del servidor de catálogo global](../../../ad-ds/plan/Planning-Global-Catalog-Server-Placement.md).



