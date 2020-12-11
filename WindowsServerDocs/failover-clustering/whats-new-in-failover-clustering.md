---
description: Más información acerca de las novedades de los clústeres de conmutación por error
ms.assetid: 350aa5a3-5938-4921-93dc-289660f26bad
title: Novedades de los clústeres de conmutación por error en Windows Server
ms.topic: get-started-article
manager: lizross
author: JasonGerend
ms.author: jgerend
ms.date: 10/18/2018
ms.openlocfilehash: 5ffe286cecc8500d70df00289a9694847e7d0994
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97040474"
---
# <a name="whats-new-in-failover-clustering"></a>What's new in Failover Clustering (Novedades de los clústeres de conmutación por error)

> Se aplica a: Windows Server 2019, Windows Server 2016

En este tema se explica la funcionalidad nueva y modificada de los clústeres de conmutación por error para Windows Server 2019 y Windows Server 2016.

## <a name="whats-new-in-windows-server-2019"></a>Novedades de Windows Server 2019

- **Conjuntos de clústeres**

    Los conjuntos de clústeres le permiten aumentar el número de servidores en una única solución de centro de centros de bits definido por software (SDDC) más allá de los límites actuales de un clúster. Esto se consigue mediante la agrupación de varios clústeres en un conjunto de clústeres: una agrupación de varios clústeres de conmutación por error: proceso, almacenamiento e hiperconvergido.
    Con los conjuntos de clústeres, puede trasladar las máquinas virtuales en línea (migración en vivo) entre los clústeres del conjunto de clústeres.

    Para obtener más información, consulte [conjuntos de clústeres](../storage/storage-spaces/cluster-sets.md).

- **Clústeres con reconocimiento de Azure**

    Los clústeres de conmutación por error ahora detectan automáticamente cuando se ejecutan en máquinas virtuales de IaaS de Azure y optimizan la configuración para proporcionar conmutación por error y registro proactivos de eventos de mantenimiento planeado de Azure para lograr los niveles más altos de disponibilidad. La implementación también se simplifica mediante la eliminación de la necesidad de configurar el equilibrador de carga con el nombre de red dinámico para el nombre del clúster.

- **Migración de clúster entre dominios**

    Los clústeres de conmutación por error ahora pueden moverse dinámicamente de un dominio Active Directory a otro, lo que simplifica la consolidación de dominios y permite que los asociados de hardware los creen y se unan al dominio del cliente más adelante.

- **Testigo USB**

    Ahora puede usar una unidad USB simple conectada a un conmutador de red como testigo para determinar el cuórum de un clúster. Esto extiende el testigo del recurso compartido de archivos para admitir cualquier dispositivo compatible con SMB2.

- **Mejoras de infraestructura de clústeres**

    La caché de CSV está habilitada de forma predeterminada para mejorar el rendimiento de las máquinas virtuales. MSDTC ahora admite volúmenes compartidos de clúster, para permitir la implementación de cargas de trabajo de MSDTC en Espacios de almacenamiento directo como con SQL Server. Lógica mejorada para detectar nodos con particiones con recuperación automática para devolver nodos a la pertenencia al clúster. Detección de rutas de red de clúster mejorada y recuperación automática.

- **La actualización con reconocimiento de clúster es compatible con Espacios de almacenamiento directo**

    La actualización compatible con clústeres (CAU) ahora está integrada y es consciente de Espacios de almacenamiento directo, validando y asegurándose de que la resincronización de datos se completa en cada nodo. La actualización compatible con clústeres inspecciona las actualizaciones para que se reinicien de forma inteligente solo si es necesario. Esto permite a los reinicios de orquestación de todos los servidores del clúster el mantenimiento planeado.

- **Mejoras de testigo de recurso compartido de archivo**

    Habilitamos el uso de un testigo de recurso compartido de archivos en los escenarios siguientes:
  - Acceso a Internet ausente o extremadamente deficiente debido a una ubicación remota, lo que impide el uso de un testigo en la nube.
  - Falta de unidades compartidas para un testigo de disco. Puede tratarse de una configuración Espacios de almacenamiento directo hiperconvergida, una SQL Server Always On grupos de disponibilidad (AG) o un grupo de disponibilidad de base de datos de Exchange (DAG), ninguno de los cuales usa discos compartidos.
  - Falta de una conexión de controlador de dominio debido a que el clúster está detrás de una red perimetral.
  - Un clúster de grupo de trabajo o entre dominios para el que no hay Active Directory objeto de nombre de clúster (CNO). Obtenga más información sobre estas mejoras en la siguiente entrada de blog de administración de & de servidor: testigo de recurso compartido de archivos de clúster de conmutación por error y DFS.

    Ahora también se bloquea explícitamente el uso de un recurso compartido de espacios de nombres DFS como ubicación. La adición de un testigo de recurso compartido de archivos a un recurso compartido DFS puede provocar problemas de estabilidad para el clúster, y esta configuración no se ha admitido nunca. Hemos agregado lógica para detectar si un recurso compartido usa espacios de nombres DFS y, si se detectan espacios de nombres DFS, Administrador de clústeres de conmutación por error bloquea la creación del testigo y muestra un mensaje de error que indica que no se admite.

- **Refuerzo de clústeres**

    La comunicación dentro del clúster en el bloque de mensajes del servidor (SMB) para volúmenes compartidos de clúster y Espacios de almacenamiento directo ahora aprovecha los certificados para proporcionar la plataforma más segura. Esto permite a los clústeres de conmutación por error funcionar sin dependencias en NTLM y habilitar las líneas de base de seguridad.

- **Clúster de conmutación por error ya no usa autenticación NTLM**

    Los clústeres de conmutación por error ya no usan la autenticación NTLM. En su lugar, Kerberos y la autenticación basada en certificados se utilizan exclusivamente. No es necesario que el usuario o las herramientas de implementación realicen ningún cambio para aprovechar esta mejora de seguridad. También permite implementar clústeres de conmutación por error en entornos en los que se ha deshabilitado NTLM.

## <a name="whats-new-in-windows-server-2016"></a>Novedades de Windows Server 2016

### <a name="cluster-operating-system-rolling-upgrade"></a><a name="BKMK_RollingUpgrade"></a>Actualización gradual del sistema operativo del clúster

La actualización gradual del sistema operativo de clústeres permite a un administrador actualizar el sistema operativo de los nodos del clúster de Windows Server 2012 R2 a una versión más reciente sin detener las cargas de trabajo de Hyper-V o del servidor de archivos Scale-Out. Con esta característica, se pueden evitar las penalizaciones de tiempo de inactividad en los acuerdos de nivel de servicio (SLA).

**¿Qué valor aporta este cambio?**

La actualización de un clúster de servidores de archivos de Scale-Out de Hyper-V de Windows Server 2012 R2 a Windows Server 2016 ya no requiere tiempo de inactividad. El clúster seguirá funcionando en el nivel de Windows Server 2012 R2 hasta que todos los nodos del clúster ejecuten Windows Server 2016. El nivel funcional del clúster se actualiza a Windows Server 2016 mediante el cmdlt de Windows PowerShell `Update-ClusterFunctionalLevel` .

> [!WARNING]
> - Después de actualizar el nivel funcional del clúster, no puede volver a un nivel funcional del clúster de Windows Server 2012 R2.
>
> - Hasta que `Update-ClusterFunctionalLevel` se ejecuta el cmdlet, el proceso es reversible y se pueden agregar nodos de Windows server 2012 R2 y se pueden quitar los nodos de Windows server 2016.

**¿Qué funciona de manera diferente?**

Ahora se puede actualizar fácilmente un clúster de conmutación por error de Hyper-V o un servidor de archivos de Scale-Out, sin que sea necesario crear un nuevo clúster con los nodos que ejecutan el sistema operativo Windows Server 2016. La migración de clústeres a Windows Server 2012 R2 implica desconectar el clúster existente y reinstalar el nuevo sistema operativo para cada nodo y, a continuación, volver a poner el clúster en línea. El proceso anterior era engorroso y el tiempo de inactividad era necesario. Sin embargo, en Windows Server 2016, el clúster no tiene que desconectarse en ningún momento.

Los sistemas operativos de clúster para la actualización en fases son los siguientes para cada nodo de un clúster:
-   El nodo se pausa y se purga de todas las máquinas virtuales que se ejecutan en él.
-   Las máquinas virtuales (u otra carga de trabajo de clúster) se migran a otro nodo del clúster.
-   Se quita el sistema operativo existente y se realiza una instalación limpia del sistema operativo Windows Server 2016 en el nodo.
-   El nodo que ejecuta el sistema operativo Windows Server 2016 se vuelve a agregar al clúster.
-   En este momento, se dice que el clúster se está ejecutando en modo mixto, porque los nodos del clúster ejecutan Windows Server 2012 R2 o Windows Server 2016.
-   El nivel funcional del clúster se mantiene en Windows Server 2012 R2. En este nivel funcional, las nuevas características de Windows Server 2016 que afectan a la compatibilidad con versiones anteriores del sistema operativo no estarán disponibles.
-   Finalmente, todos los nodos se actualizan a Windows Server 2016.
-   A continuación, se cambia el nivel funcional del clúster a Windows Server 2016 mediante el cmdlet de Windows PowerShell `Update-ClusterFunctionalLevel` . Llegados a este punto, puede aprovechar las características de Windows Server 2016.

Para obtener más información, consulte [actualización gradual del sistema operativo del clúster](cluster-operating-system-rolling-upgrade.md).

### <a name="storage-replica"></a><a name="BKMK_SR"></a>Réplica de almacenamiento
Réplica de almacenamiento es una nueva característica que permite la replicación sincrónica independiente del almacenamiento y a nivel de bloque entre servidores o clústeres para la recuperación ante desastres, así como la ampliación de un clúster de conmutación por error entre sitios. La replicación sincrónica permite el reflejo de datos en sitios físicos con volúmenes coherentes frente a bloqueos para asegurar que no se produce absolutamente ninguna pérdida de datos en el nivel de sistema de archivos. La replicación asincrónica permite la extensión de sitios más allá del área metropolitana con la posibilidad de pérdida de datos.

**¿Qué valor aporta este cambio?**

Réplica de almacenamiento permite hacer lo siguiente:

-   Especificar una sola solución de recuperación ante desastres de proveedores para las interrupciones planificadas y no planificadas de cargas de trabajo esenciales.

-   Utilizar el transporte SMB3 con rendimiento, escalabilidad y confiabilidad probada.

-   Extender los clústeres de conmutación por error de Windows a distancias metropolitanas.

-   Use software de Microsoft de extremo a extremo para almacenamiento y agrupación en clústeres, como Hyper-V, réplica de almacenamiento, espacios de almacenamiento, clúster, servidor de archivos de Scale-Out, SMB3, desduplicación de datos y ReFS/NTFS.

-   Ayudar a reducir el costo y la complejidad como sigue:

    -   Es independiente del hardware, y no requiere una configuración del almacenamiento específica como SAN o DAS.

    -   Permite las tecnologías de red y el almacenamiento de productos.

    -   Presenta facilidad de administración gráfica para nodos y clústeres individuales mediante el Administrador de clústeres de conmutación por error.

    -   Incluye opciones de scripting completas y a gran escala a través de Windows PowerShell.

-   Ayudar a reducir el tiempo de inactividad y aumentar la confiabilidad y productividad intrínseca de Windows.

-   Proporcionar funcionalidades de diagnóstico, métricas de rendimiento y compatibilidad.

Para más información, consulte [Storage Replica in Windows Server 2016](../storage/storage-replica/storage-replica-overview.md) (Réplica de almacenamiento en Windows Server 2016).

### <a name="cloud-witness"></a><a name="BKMK_CloudWitness"></a>Testigo en la nube

Testigo en la nube es un nuevo tipo de testigo de cuórum de clúster de conmutación por error en Windows Server 2016 que utiliza Microsoft Azure como punto de arbitraje. El testigo en la nube, como cualquier otro testigo de cuórum, obtiene un voto y pueden participar en los cálculos de cuórum. Puede configurar el testigo en la nube como un testigo de cuórum el Asistente para configurar un cuórum de clúster.

**¿Qué valor aporta este cambio?**

El uso de un testigo en la nube como testigo de cuórum del clúster de conmutación por error proporciona las siguientes ventajas:

-   Aprovecha Microsoft Azure y elimina la necesidad de un tercer centro de recursos independiente.

-   Usa el Microsoft Azure estándar públicamente disponible Blob Storage que elimina la sobrecarga de mantenimiento adicional de las máquinas virtuales hospedadas en una nube pública.

-   Se puede usar la misma cuenta de Microsoft Azure Storage para varios clústeres (un archivo de BLOB por clúster; identificador único de clúster usado como nombre de archivo de BLOB).

-   Proporciona un costo muy bajo de la cuenta de almacenamiento (datos muy pequeños escritos por cada archivo de BLOB, el archivo de BLOB se actualiza solo una vez cuando cambia el estado de los nodos del clúster).

Para obtener más información, consulte [implementación de un testigo en la nube para un clúster de conmutación por error](deploy-cloud-witness.md).

**¿Qué funciona de manera diferente?**

Esta funcionalidad es nueva en Windows Server 2016.

### <a name="virtual-machine-resiliency"></a><a name="BKMK_VMs"></a>Resistencia de la máquina virtual

**Resistencia de proceso** Windows Server 2016 incluye una mayor resistencia de proceso de máquinas virtuales para ayudar a reducir los problemas de comunicación dentro del clúster en el clúster de proceso de la siguiente manera:

-   **Opciones de resistencia disponibles para las máquinas virtuales:**  Ahora puede configurar las opciones de resistencia de la máquina virtual que definen el comportamiento de las máquinas virtuales durante los errores transitorios:

    -   **Nivel de resistencia:** Ayuda a definir cómo se controlan los errores transitorios.

    -   **Período de resistencia:**  Ayuda a definir el tiempo durante el que se permite la ejecución de todas las máquinas virtuales aisladas.

-   **Poner en cuarentena los nodos en mal estado:** Los nodos incorrectos están en cuarentena y ya no se les permite unirse al clúster. Esto evita que los nodos oscilantes afecten negativamente a otros nodos y al clúster global.

Para obtener más información sobre el flujo de trabajo de resistencia de proceso de máquina virtual y la configuración de cuarentena de nodo que controlan el modo en que el nodo se coloca en aislamiento o en cuarentena, consulte [resistencia de proceso de máquina virtual en Windows Server 2016](https://blogs.msdn.com/b/clustering/archive/2015/06/03/10619308.aspx).

**Resistencia de almacenamiento** En Windows Server 2016, las máquinas virtuales son más resistentes a los errores de almacenamiento transitorios. La mejora de la resistencia de las máquinas virtuales ayuda a conservar los Estados de sesión de máquina virtual del inquilino en caso de que se produzca una interrupción del almacenamiento. Esto se logra mediante una respuesta de máquina virtual rápida y inteligente a los problemas de la infraestructura de almacenamiento.

Cuando una máquina virtual se desconecta de su almacenamiento subyacente, se pausa y espera a que se recupere el almacenamiento. Mientras se pausa, la máquina virtual conserva el contexto de las aplicaciones que se ejecutan en ella. Cuando se restaura la conexión de la máquina virtual con su almacenamiento, la máquina virtual vuelve a su estado de ejecución. Como resultado, el estado de sesión del equipo del inquilino se conserva en la recuperación.

En Windows Server 2016, la resistencia de almacenamiento de máquinas virtuales es consciente y optimizada también para los clústeres invitados.

### <a name="diagnostic-improvements-in-failover-clustering"></a><a name="BKMK_Diagnostics"></a>Mejoras de diagnóstico en los clústeres de conmutación por error

Para ayudar a diagnosticar problemas con los clústeres de conmutación por error, Windows Server 2016 incluye lo siguiente:

- Varias mejoras en los archivos de registro del clúster (como información de zona horaria y registro de DiagnosticVerbose) que facilitan la solución de problemas de clústeres de conmutación por error. Para obtener más información, vea [mejoras en la solución de problemas del clúster de conmutación por error de Windows Server 2016: registro de clúster](https://techcommunity.microsoft.com/t5/failover-clustering/windows-server-2016-failover-cluster-troubleshooting/ba-p/372005).

- Un nuevo tipo de volcado de **memoria activo**, que filtra la mayoría de las páginas de memoria asignadas a las máquinas virtuales y, por tanto, hace que Memory. DMP sea mucho más pequeño y más fácil de guardar o copiar. Para obtener más información, consulte [mejoras en la solución de problemas del clúster de conmutación por error de Windows Server 2016: volcado activo](https://techcommunity.microsoft.com/t5/failover-clustering/windows-server-2016-failover-cluster-troubleshooting/ba-p/372008).

### <a name="site-aware-failover-clusters"></a><a name="BKMK_SiteAware"></a>Clústeres de conmutación por error con reconocimiento de sitios

Windows Server 2016 incluye clústeres de conmutación por error que reconocen el sitio y que habilitan nodos de grupo en clústeres extendidos, en función de su ubicación física (sitio). El reconocimiento de sitios de clúster mejora las operaciones clave durante el ciclo de vida del clúster, como el comportamiento de la conmutación por error, las directivas de colocación, el latido entre los nodos y el comportamiento del cuórum. Para obtener más información, consulte [clústeres de conmutación por error que reconocen el sitio en Windows Server 2016](https://techcommunity.microsoft.com/t5/failover-clustering/site-aware-failover-clusters-in-windows-server-2016/ba-p/372060).

### <a name="workgroup-and-multi-domain-clusters"></a><a name="BKMK_multidomainclusters"></a>Clústeres de grupo de trabajo y de dominios múltiples

En Windows Server 2012 R2 y versiones anteriores, un clúster solo se puede crear entre nodos de miembro Unidos al mismo dominio. Windows Server 2016 rompe estas barreras y presenta la capacidad para crear un clúster de conmutación por error sin dependencias de Active Directory. Ahora puede crear clústeres de conmutación por error en las siguientes configuraciones:

-   **Clústeres de un solo dominio.** Clústeres con todos los nodos Unidos al mismo dominio.

-   **Clústeres de varios dominios.** Clústeres con nodos que son miembros de distintos dominios.

-   **Clústeres de grupo de trabajo.** Clústeres con nodos que son servidores miembro/Grupo de trabajo (no Unidos a un dominio).

Para obtener más información, consulte [clústeres de grupos de trabajo y varios dominios en Windows Server 2016](https://techcommunity.microsoft.com/t5/failover-clustering/workgroup-and-multi-domain-clusters-in-windows-server-2016/ba-p/372059)

### <a name="virtual-machine-load-balancing"></a><a name="BKMK_VMLoadBalancing"></a>Equilibrio de carga de máquinas virtuales

El equilibrio de carga de máquinas virtuales es una característica nueva de los clústeres de conmutación por error que facilita el equilibrio de carga sin problemas de las máquinas virtuales entre los nodos de un clúster. Los nodos sobrecargados se identifican en función de la memoria de la máquina virtual y el uso de CPU en el nodo. Después, las máquinas virtuales se mueven (se migran en vivo) desde un nodo sobrecargado a los nodos con el ancho de banda disponible (si procede). La agresividad del equilibrio se puede optimizar para garantizar un rendimiento óptimo del clúster y el uso. El equilibrio de carga está habilitado de forma predeterminada en Windows Server 2016 Technical Preview. Sin embargo, el equilibrio de carga se deshabilita cuando está habilitada la optimización dinámica de SCVMM.

### <a name="virtual-machine-start-order"></a><a name="BKMK_VMStartOrder"></a>Orden de inicio de la máquina virtual

El orden de inicio de la máquina virtual es una característica nueva de los clústeres de conmutación por error que presenta la orquestación de inicio de las máquinas virtuales (y todos los grupos) de un clúster. Las máquinas virtuales ahora se pueden agrupar en niveles y las dependencias de orden de inicio se pueden crear entre diferentes niveles. Esto garantiza que las máquinas virtuales más importantes (como los controladores de dominio o las máquinas virtuales de la utilidad) se inician en primer lugar. Las máquinas virtuales no se inician hasta que también se inician las máquinas virtuales en las que tienen una dependencia.

### <a name="simplified-smb-multichannel-and-multi-nic-cluster-networks"></a><a name="BKMK_SMBMultiChannel"></a> Redes de clústeres de varias NIC y de SMB multicanal simplificado

Las redes de clústeres de conmutación por error ya no se limitan a una única NIC por subred o red. Con las redes de clústeres de varias NIC y de SMB simplificado, la configuración de red es automática y todas las NIC de la subred se pueden usar para el tráfico de clústeres y cargas de trabajo. Esta mejora permite a los clientes maximizar el rendimiento de la red de Hyper-V, SQL Server instancia de clúster de conmutación por error y otras cargas de trabajo de SMB.

Para más información, consulte [redes de clústeres de varias NIC y SMB multicanal simplificado](smb-multichannel.md).

## <a name="see-also"></a>Consulte también

* [Storage](../storage/storage.yml)
* [Novedades de almacenamiento en Windows Server 2016](../storage/whats-new-in-storage.md)
