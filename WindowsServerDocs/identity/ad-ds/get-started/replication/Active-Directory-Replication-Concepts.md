---
ms.assetid: 4cc9c16c-1928-4dce-a3a8-6229be28eb65
title: Conceptos de replicación de Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 087dee7c914b81d97c6cf3a2ea985d809cb57fe6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812356"
---
# <a name="active-directory-replication-concepts"></a>Conceptos de replicación de Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Antes de diseñar la topología del sitio, a familiarizarse con algunos conceptos de replicación de Active Directory.  
  
-   [Objeto de conexión](#BKMK_1)  
  
-   [KCC](#BKMK_2)  
  
-   [Funcionalidad de conmutación por error](#BKMK_3)  
  
-   [Subnet](#BKMK_4)  
  
-   [Sitio](#BKMK_5)  
  
-   [Vínculo a sitios](#BKMK_6)  
  
-   [Puente de vínculo de sitio](#BKMK_7)  
  
-   [Transitividad de vínculo de sitio](#BKMK_8)  
  
-   [Servidor de catálogo global](#BKMK_9)  
  
-   [Caché de pertenencia al grupo universal](#BKMK_10)  
  
## <a name="BKMK_1"></a>Objeto de conexión  
Un objeto de conexión es un objeto de Active Directory que representa una conexión de replicación desde un controlador de dominio de origen a un controlador de dominio de destino. Un controlador de dominio es un miembro de un solo sitio y se representa en el sitio mediante un objeto de servidor en Active Directory Domain Services (AD DS). Cada objeto de servidor tiene el objeto de configuración NTDS que representa el controlador de dominio de replicación en el sitio secundario.  
  
El objeto de conexión es un elemento secundario del objeto configuración NTDS en el servidor de destino. Para que la replicación entre dos controladores de dominio, el objeto de servidor de uno debe tener un objeto de conexión que representa la replicación de entrada desde la otra. Todas las conexiones de replicación para un controlador de dominio se almacenan como objetos de conexión en el objeto configuración NTDS. El objeto de conexión identifica el servidor de origen de replicación, contiene una programación de replicación y especifica un transporte de replicación.  
  
El Comprobador de coherencia de la información (KCC) crea los objetos de conexión automáticamente, pero también pueden crearse manualmente. Los objetos de conexión creados por el KCC aparecen en los sitios de Active Directory y el complemento Servicios como **<automatically generated>** y se considera adecuada en condiciones de funcionamiento normales. Los objetos de conexión creados por un administrador se crean manualmente objetos de conexión. Un objeto de conexión creados manualmente se identifica mediante el nombre asignado por el administrador cuando se creó. Cuando se modifica un **<automatically generated>** de objeto de conexión, se convierte en un objeto de conexión administrativamente modificado y el objeto aparece en el formulario de un GUID. KCC no realizar cambios en los objetos de conexión manual o modificados.  
  
## <a name="BKMK_2"></a>KCC  
KCC es un proceso integrado que se ejecuta en todos los controladores de dominio y genera la topología de replicación para el bosque de Active Directory. Crea topologías de replicación diferente dependiendo de si se está produciendo la replicación dentro de un sitio (entre sitios) o entre sitios (entre sitios). KCC también ajusta dinámicamente la topología para dar cabida a la adición de nuevos controladores de dominio, la eliminación de controladores de dominio existentes, el movimiento de los controladores de dominio a y desde sitios, cambiar los costos y programaciones y controladores de dominio no está disponible temporalmente o en un estado de error.  
  
Dentro de un sitio, las conexiones entre los controladores de dominio de escritura siempre se organizan en un anillo bidireccional, con las conexiones de método abreviado adicional para reducir la latencia en sitios de gran tamaño. Por otro lado, la topología entre sitios es una disposición en capas de expansión de árboles, lo que significa que una conexión entre sitios no existe entre dos sitios para cada partición de directorio y por lo general no tiene conexiones de acceso directo. Para obtener más información acerca de la expansión árboles y topología de replicación de Active Directory, vea replicación topología referencia técnica Active Directory ([https://go.microsoft.com/fwlink/?LinkID=93578](https://go.microsoft.com/fwlink/?LinkID=93578)).  
  
En cada controlador de dominio, el KCC crea las rutas de replicación mediante la creación de objetos de conexión de entrada unidireccional que definen las conexiones desde otros controladores de dominio. Para los controladores de dominio en el mismo sitio, el KCC crea objetos de conexión automáticamente sin intervención administrativa. Cuando haya más de un sitio, configurar vínculos de sitios entre sitios y una única KCC en cada sitio crea automáticamente las conexiones entre sitios también.  
  
### <a name="kcc-improvements-for-windows-server-2008-rodcs"></a>Mejoras de KCC para los RODC de Windows Server 2008  

Hay una serie de mejoras de KCC para acomodar el controlador de dominio recién disponible de solo lectura (RODC) en Windows Server 2008. Un escenario de implementación típica de RODC es la sucursal. La topología de replicación de Active Directory que se implementan con más frecuencia en este escenario se basa en un diseño de concentrador y radio, donde los controladores de dominio de sucursal en varios sitios replican con un número pequeño de servidores de cabeza de puente en un sitio concentrador.  
  
Una de las ventajas de la implementación de RODC en este escenario es la replicación unidireccional. Servidores cabeza de puente no son necesarios para replicar desde el RODC, lo que reduce la administración y uso de la red.  
  
Sin embargo, un desafío administrativo resaltado por la topología de concentrador y radio en versiones anteriores del sistema operativo Windows Server es que después de agregar un nuevo controlador de dominio de cabeza de puente en el concentrador, no hay ningún mecanismo automático para redistribuir el conexiones de replicación entre controladores de dominio de sucursal y los controladores de dominio de concentrador para aprovechar el nuevo controlador de dominio de concentrador.  
  
Para los controladores de dominio de Windows Server 2003, puede equilibrar la carga de trabajo mediante una herramienta como Adlb.exe de la Guía de implementación de Windows Server 2003 Branch Office ([https://go.microsoft.com/fwlink/?LinkID=28523](https://go.microsoft.com/fwlink/?LinkID=28523)).  
  
Para los RODC de Windows Server 2008, el funcionamiento normal del KCC proporciona algunos reequilibrio, lo que elimina la necesidad de usar una herramienta como Adlb.exe adicional. La nueva funcionalidad está habilitada de forma predeterminada. Puede deshabilitarlo mediante la adición de la siguiente clave del registro establecido en el RODC:  
  
**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters**  
  
**"Equilibrio de carga aleatoria BH permitido"**  
**1 = habilitado (predeterminado), 0 = deshabilitado**  
  
Para obtener más información acerca de cómo funcionan estas mejoras de KCC, consulte la planeamiento e implementación de Active Directory Domain Services para las sucursales ([https://go.microsoft.com/fwlink/?LinkId=107114](https://go.microsoft.com/fwlink/?LinkId=107114)).  
  
## <a name="BKMK_3"></a>Funcionalidad de conmutación por error  
Sitios de asegurarse de que la replicación se enruta en torno a los errores de red y controladores de dominio sin conexión. Se ejecuta a intervalos especificados para ajustar la topología de replicación para los cambios producidos en AD DS, por ejemplo, cuando se agregan nuevos controladores de dominio y nuevos sitios se crean. KCC revisa el estado de replicación de las conexiones existentes para determinar si las conexiones no funcionan. Si una conexión no funciona debido a un controlador de dominio con errores, el KCC automáticamente crea temporales conexiones a otros asociados de replicación (si está disponible) para asegurarse de que la replicación se produce. Si no están disponibles todos los controladores de dominio en un sitio, el KCC crea automáticamente las conexiones de replicación entre controladores de dominio de otro sitio.  
  
## <a name="BKMK_4"></a>Subred  
Una subred es un segmento de una red TCP/IP a la que se asigna un conjunto de direcciones IP lógicas. Subredes de grupo equipos de forma que identifica su proximidad física en la red. Los objetos de subred en AD DS identifican las direcciones de red que se usan para asignar equipos a sitios.  
  
## <a name="BKMK_5"></a>Sitio  
Los sitios son objetos de Active Directory que representan uno o más subredes TCP/IP con conexiones de red altamente confiable y rápida. Información del sitio permite a los administradores configurar el acceso a Active Directory y la replicación para optimizar el uso de la red física. Objetos de sitio están asociados con un conjunto de subredes, y cada controlador de dominio en un bosque está asociado con un sitio de Active Directory según su dirección IP. Los sitios pueden alojar controladores de dominio de más de un dominio y un dominio puede representarse en más de un sitio.  
  
## <a name="BKMK_6"></a>Vínculo a sitios  
Vínculos a sitios son objetos de Active Directory que representan rutas de acceso lógicas que el KCC se utiliza para establecer una conexión de replicación de Active Directory. Un objeto de vínculo de sitio representa un conjunto de sitios que se pueden comunicar a un costo uniforme mediante un protocolo de transporte entre sitios especificado.  
  
Todos los sitios incluidos en el vínculo a sitios se consideran se conecten por medio del mismo tipo de red. Sitios deben vincularse manualmente a otros sitios mediante el uso de vínculos a sitios para que los controladores de dominio en un sitio pueden replicar cambios de directorio de controladores de dominio en otro sitio. Como vínculos a sitios no corresponden a la ruta de acceso real realizada por los paquetes de red en la red física durante la replicación, no es necesario crear vínculos a sitios redundantes para mejorar la eficiencia de la replicación de Active Directory.  
  
Cuando dos sitios están conectados mediante un vínculo a sitios, el sistema de replicación crea automáticamente las conexiones entre controladores de dominio específico en cada sitio que se denominan servidores cabeza de puente. En Windows Server 2008, todos los controladores de dominio en un sitio que hospedan la misma partición de directorio son candidatos para la que se selecciona como servidores cabeza de puente. Las conexiones de replicación creadas por el KCC se distribuyen aleatoriamente entre todos los servidores de cabeza de puente de candidato en un sitio para compartir la carga de trabajo de replicación. De forma predeterminada, el proceso de selección aleatoria se lleva a cabo solo una vez, cuando se agregan primero los objetos de conexión para el sitio.  
  
## <a name="BKMK_7"></a>Puente de vínculo de sitio  
Un puente de vínculos de sitio es un objeto de Active Directory que representa un conjunto de vínculos a sitios, todos cuyos sitios pueden comunicarse mediante el uso de un transporte común. Los puentes de vínculos permiten a los controladores de dominio que no están conectados directamente mediante un vínculo de comunicación para replicar entre sí. Normalmente, un puente de vínculo a sitios corresponde a un enrutador (o un conjunto de enrutadores) en una red IP.  
  
De forma predeterminada, el KCC puede formar una ruta transitiva a través de vínculos a todos los sitios que tienen en común algunos sitios. Si se deshabilita este comportamiento, cada vínculo de sitio representa su propia red distinta y aislada. Conjuntos de vínculos a sitios que se pueden tratar como una sola ruta se expresan a través de un puente de vínculos de sitio. Cada puente representa un entorno aislado de comunicación para el tráfico de red.  
  
Los puentes de vínculos son un mecanismo para representar lógicamente transitiva conectividad física entre sitios. Un puente de vínculos de sitio permite el funcionamiento de KCC utilice cualquier combinación de los vínculos a sitios incluidos para determinar la ruta menos costosa para interconectar las particiones de directorio almacenadas en esos sitios. El puente de vínculos de sitio no proporciona conectividad real con los controladores de dominio. Si se quita el puente de vínculo de sitio, la replicación a través de los vínculos a sitios combinado continuará hasta que el KCC quita los vínculos.  
  
Los puentes de vínculos sólo son necesarios si un sitio contiene un controlador de dominio que aloja una partición de directorio que no se hospeda también en un controlador de dominio en un sitio adyacente, pero un controlador de dominio hospeda esa partición de directorio se encuentra en uno o varios sitios en el bosque. Los sitios adyacentes se definen como cualquier dos o más sitios incluidos en un vínculo de sitio único.  
  
Un puente de vínculos de sitio crea una conexión lógica entre dos vínculos a sitios, que proporciona una ruta de acceso transitiva entre dos sitios sin conexión mediante el uso de un sitio provisional. Para los fines de generador de topología entre sitios (ISTG), el puente implica conectividad física mediante el sitio provisional. El puente no implica que un controlador de dominio en el sitio provisional proporcionará la ruta de replicación. Sin embargo, esto podría ser el caso si el sitio provisional contenía un controlador de dominio que hospeda la partición de directorio se repliquen, en cuyo caso un puente de vínculos de sitio no es necesario.  
  
Se agrega el costo de cada vínculo de sitio, crear un costo sumado para la ruta de acceso resultante. Si el sitio provisional no contiene un controlador de dominio que aloja la partición de directorio y un vínculo de costo inferior no existe, se usaría el puente de vínculos de sitio. Si el sitio provisional contenía un controlador de dominio que hospeda la partición de directorio, dos sitios desconectados podrían configurar las conexiones de replicación para el controlador de dominio provisional y no usar el puente.  
  
## <a name="BKMK_8"></a>Transitividad de vínculo de sitio  
De forma predeterminada todos los vínculos de sitio son transitivas, o "puente". Cuando están conectados de vínculos a sitios y las programaciones se superponen, crea las conexiones de replicación que determinan los asociados de replicación del controlador de dominio entre sitios, donde los sitios no están directamente conectados mediante vínculos a sitios, pero están conectados de manera transitiva a través un conjunto de sitios comunes. Esto significa que cualquier sitio puede conectarse a cualquier otro sitio mediante una combinación de vínculos a sitios.  
  
En general, para una red completamente enrutada, es necesario crear puentes de vínculos de cualquier sitio a menos que desee controlar el flujo de replicación de los cambios. Si la red no está totalmente enrutada, se deben crear puentes de vínculos de sitio para evitar intentos de replicación imposible. Todos los vínculos a sitios para un transporte específico implícitamente pertenecen a un puente de vínculos de sitio único para ese transporte. Los puentes predeterminados para los vínculos a sitios se produce automáticamente y no hay ningún objeto de Active Directory representa ese puente. El **enlazar todos los vínculos a sitios** la configuración, se encontró en las propiedades de los contenedores de transporte entre sitios de la dirección IP y el protocolo Simple de transferencia de correo (SMTP), implementa el puente de vínculos de sitio automática.  
  
> [!NOTE]  
> Replicación de SMTP no se admitirán en versiones futuras de AD DS; por lo tanto, no se recomienda la creación de objetos de vínculos de sitio en el contenedor de SMTP.  
  
## <a name="BKMK_9"></a>Servidor de catálogo global  
Un servidor de catálogo global es un controlador de dominio que almacena información sobre todos los objetos en el bosque, por lo que las aplicaciones puedan buscar AD DS sin hacer referencia a los controladores de dominio específicos que almacenan los datos solicitados. Al igual que todos los controladores de dominio, un servidor de catálogo global almacena réplicas completas, puede escribir el esquema y la configuración de particiones de directorio y una réplica de escritura completa de la partición de directorio de dominio para el dominio que hospeda. Además, un servidor de catálogo global almacena una réplica parcial de solo lectura de todos los demás dominios del bosque. Réplicas de dominio parciales de sólo lectura contienen todos los objetos en el dominio, pero solo un subconjunto de los atributos (los atributos que se usan normalmente para buscar el objeto).  
  
## <a name="BKMK_10"></a>Caché de pertenencia al grupo universal  
Caché de pertenencia al grupo universal permite que el controlador de dominio en caché la información de pertenencia al grupo universal para los usuarios. Puede permitir que los controladores de dominio que ejecutan Windows Server 2008 en caché la pertenencia a grupos universales mediante el complemento Servicios y sitios de Active Directory.  
  
Activación de la caché de pertenencia a grupos universales, elimina la necesidad de un servidor de catálogo global en todos los sitios en un dominio, lo que minimiza el uso de ancho de banda de red porque no es necesario replicar todos los objetos que se encuentra en el bosque de un controlador de dominio. También reduce los tiempos de inicio de sesión porque los controladores de dominio de autenticación no es siempre necesario tener acceso a un catálogo global para obtener información de pertenencia a grupos universales. Para obtener más información sobre cuándo usar la caché de pertenencia al grupo universal, consulte [planear la ubicación del servidor de catálogo Global](../../../ad-ds/plan/Planning-Global-Catalog-Server-Placement.md).  
  


