---
ms.assetid: 350aa5a3-5938-4921-93dc-289660f26bad
title: Novedades en clústeres de conmutación por error de Windows Server
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: get-started-article
manager: dongill
author: JasonGerend
ms.author: jgerend
ms.date: 10/18/2018
ms.openlocfilehash: b4fa59aa62acba5c89f20c191da2c3c1b776b1ca
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884756"
---
# <a name="whats-new-in-failover-clustering"></a>What's new in Failover Clustering (Novedades de los clústeres de conmutación por error)

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual)

En este tema se explica la funcionalidad nueva y modificada en la conmutación por error de agrupación en clústeres de Windows Server de 2019, Windows Server 2016, y libera el canal semianual de Windows Server.

## <a name="whats-new-in-windows-server-2019"></a>Novedades de Windows Server 2019

- **Conjuntos de clústeres**

    Conjuntos de clúster le permiten aumentar el número de servidores en una solución única datacenter definidas por software (SDDC) más allá de los límites actuales de un clúster. Esto se logra mediante la agrupación de varios clústeres en un conjunto de clústeres--una agrupación de acoplamiento de varios clústeres de conmutación por error: proceso, almacenamiento e hiperconvergidas.
    Con los conjuntos de clúster, puede mover máquinas virtuales en línea (migración en vivo) entre clústeres dentro del clúster establecido.

    Para obtener más información, consulte [conjuntos de clústeres](../storage/storage-spaces/cluster-sets.md).

- **Clústeres de Azure**

    Clústeres de conmutación por error ahora detectan automáticamente cuando se están ejecutando en máquinas virtuales de IaaS de Azure y optimizar la configuración para proporcionar conmutación por error proactiva y el registro de eventos de mantenimiento planeado de Azure para lograr los niveles más altos de disponibilidad. También se simplifica la implementación mediante la eliminación de la necesidad de configurar el equilibrador de carga con nombre dinámico de red para el nombre del clúster.

- **Migración de clúster entre dominios**

    Clústeres de conmutación por error puede ahora mover dinámicamente desde un dominio de Active Directory a otro, lo que simplifica la consolidación de dominios y permite que los clústeres se creadas por socios de hardware y se unan al dominio del cliente más adelante.
- **Testigo USB**

    Ahora puede usar una unidad USB simple conectada a un conmutador de red como un testigo para determinar el quórum para un clúster. Este comando extiende el testigo de recurso compartido de archivos para admitir cualquier dispositivo compatible con SMB2.

- **Mejoras en la infraestructura de clúster**

    Ahora, la caché de CSV está habilitada de forma predeterminada para mejorar el rendimiento de la máquina virtual. MSDTC ahora admite Volúmenes compartidos de clúster, para permitir la implementación de cargas de trabajo de MSDTC en espacios de almacenamiento directo, como con SQL Server. Lógica mejorada para detectar nodos con particiones con recuperación automática para devolver los nodos a la pertenencia al clúster. Detección de rutas de red de clúster mejorada y recuperación automática.

- **Actualización compatible con clústeres es compatible con espacios de almacenamiento directo**

    La Actualización compatible con clústeres (CAU) ahora está integrada y es compatible con Espacios de almacenamiento directo, validando y garantizando que la resincronización de datos se completa en cada nodo. Actualización compatible con clústeres inspecciona las actualizaciones de forma inteligente reinicie si es necesario. Esto permite la coordinación de reinicios de todos los servidores del clúster para el mantenimiento planeado.

- **Mejoras de testigo de recurso compartido de archivos** habilitamos el uso de un testigo de recurso compartido de archivos en los siguientes escenarios: 
    - Falta o muy deficiente acceso a Internet debido a una ubicación remota, impidiendo así el uso de un testigo en la nube. 
    - Falta de unidades compartidas para un testigo de disco. Podría tratarse de una configuración de espacios de almacenamiento directo hiperconvergido un SQL Server siempre en disponibilidad grupos (AG), o un * Exchange base de datos de grupo de disponibilidad (DAG), ninguno de los cuales usa discos compartidos. 
    - Ausencia de una conexión de controlador de dominio porque el clúster está detrás de una red Perimetral. 
    - Un grupo de trabajo o entre dominios clúster para el que existe no es ningún objeto de nombre de clúster (CNO) de Active Directory. Obtener más información sobre estas mejoras en la siguiente entrada en los Blogs de administración y servidor: Testigo de recurso compartido de archivos de clúster de conmutación por error y DFS.
    
    Ahora nos explícitamente también bloquea el uso de un recurso compartido de espacios de nombres DFS como una ubicación. Agregar un testigo del recurso compartido de archivos a un DFS recurso compartido puede causar problemas de estabilidad del clúster y nunca se admite esta configuración. Hemos agregado lógica para detectar si un recurso compartido usa espacios de nombres DFS, y si se detecta los espacios de nombres DFS, Administrador de clústeres de conmutación por error bloquea la creación del testigo y muestra un mensaje de error sobre no se admite.
- **Protección de clúster**

    La comunicación dentro del clúster a través del Bloque de mensajes del servidor (SMB) para volúmenes compartidos de clúster y Espacios de almacenamiento directo ahora aprovecha certificados para proporcionar la plataforma más segura. Esto permite a los clústeres de conmutación por error funcionar sin dependencias en NTLM y habilitar líneas base de seguridad.
- **Clúster de conmutación por error ya no utiliza la autenticación NTLM**

    Clústeres de conmutación por error ya no use la autenticación NTLM. En su lugar, Kerberos y la autenticación basada en certificados se utiliza exclusivamente. No hay ningún cambio requerido por el usuario o las herramientas de implementación, para aprovechar esta mejora de seguridad. También permite a los clústeres de conmutación por error para implementarse en entornos donde se deshabilitó NTLM. 


## <a name="whats-new-in-windows-server-2016"></a>Novedades de Windows Server 2016

### <a name="BKMK_RollingUpgrade"></a>Actualización gradual del sistema operativo del clúster

Actualización gradual de clúster del sistema operativo permite a un administrador actualizar el sistema operativo de los nodos del clúster de Windows Server 2012 R2 a una versión más reciente sin detener la función Hyper-V o las cargas de trabajo de servidor de archivos de escalabilidad horizontal. Con esta característica, se pueden evitar las penalizaciones de tiempo de inactividad en los acuerdos de nivel de servicio (SLA). 

**¿Qué valor aporta este cambio?**  

Actualizar un clúster de Hyper-V o el servidor de archivos de escalabilidad horizontal de Windows Server 2012 R2 a Windows Server 2016 ya no requiere tiempo de inactividad. El clúster seguirá funcionando en un nivel de Windows Server 2012 R2 hasta que todos los nodos del clúster ejecutan Windows Server 2016. El nivel funcional del clúster se actualiza a Windows Server 2016 mediante el cmdlet de Windows PowerShell `Update-ClusterFunctionalLevel`. 

> [!WARNING]  
> -   Después de actualizar el nivel funcional del clúster, no podrá volver a un nivel funcional del clúster de Windows Server 2012 R2. 
> -   Hasta que el `Update-ClusterFunctionalLevel` cmdlet se ejecuta, el proceso es reversible y se pueden agregar nodos de Windows Server 2012 R2 y se pueden quitar los nodos de Windows Server 2016. 

**¿Qué funciona de manera diferente?**  

Un clúster de conmutación por error de Hyper-V o el servidor de archivos de escalabilidad horizontal puede ahora actualizarse fácilmente sin tiempo de inactividad o necesita para crear un nuevo clúster con nodos que ejecutan el sistema operativo Windows Server 2016. Migración de clústeres a Windows Server 2012 R2 implicados desconectar el clúster existente y volver a instalar el nuevo sistema operativo para cada nodos y, a continuación, poner el clúster en línea. El proceso anterior era complicada y requiere tiempo de inactividad. Sin embargo, en Windows Server 2016, no necesita el clúster se quede sin conexión en cualquier momento. 

Los sistemas operativos de clúster para la actualización en las fases son los siguientes para cada nodo en un clúster:  
-   El nodo está en pausa y completamente agotado todas las máquinas virtuales que se ejecutan en él. 
-   Las máquinas virtuales (u otras cargas de trabajo de clúster) se migran a otro nodo del clúster. 
-   El sistema operativo existente se quita y se realiza una instalación limpia del sistema operativo Windows Server 2016 en el nodo. 
-   El nodo que ejecuta el sistema operativo Windows Server 2016 se agrega al clúster. 
-   En este momento, el clúster se dice que se ejecutan en modo mixto, porque los nodos del clúster ejecutan Windows Server 2012 R2 o Windows Server 2016. 
-   El nivel funcional del clúster se mantiene en Windows Server 2012 R2. En este nivel funcional, nuevas características en Windows Server 2016 que afectan a la compatibilidad con versiones anteriores del sistema operativo no estará disponibles. 
-   Finalmente, todos los nodos se actualizan a Windows Server 2016. 
-   A continuación, se cambia el nivel funcional del clúster a Windows Server 2016 mediante el cmdlet de Windows PowerShell `Update-ClusterFunctionalLevel`. En este momento, puede aprovechar las características de Windows Server 2016. 

Para obtener más información, consulte [actualización gradual de clúster del sistema operativo](cluster-operating-system-rolling-upgrade.md). 

### <a name="BKMK_SR"></a>Réplica de almacenamiento  
Réplica de almacenamiento es una nueva característica que permite a independiente del almacenamiento, la replicación sincrónica de nivel de bloque entre servidores o clústeres para recuperación ante desastres, así como la ampliación de un clúster de conmutación por error entre sitios. La replicación sincrónica permite el reflejo de datos en sitios físicos con volúmenes coherentes frente a bloqueos para asegurar que no se produce absolutamente ninguna pérdida de datos en el nivel de sistema de archivos. La replicación asincrónica permite la extensión de sitios más allá del área metropolitana con la posibilidad de pérdida de datos. 

**¿Qué valor aporta este cambio?**  

Réplica de almacenamiento le permite hacer lo siguiente:  

-   Especificar una sola solución de recuperación ante desastres de proveedores para las interrupciones planificadas y no planificadas de cargas de trabajo esenciales. 

-   Utilizar el transporte SMB3 con rendimiento, escalabilidad y confiabilidad probada. 

-   Extender los clústeres de conmutación por error de Windows a distancias metropolitanas. 

-   Usar software integral de Microsoft para el almacenamiento y la agrupación en clústeres, como Hyper-V, réplica de almacenamiento, espacios de almacenamiento, clúster, Scale-Out File Server, SMB3, desduplicación de datos y ReFS/NTFS. 

-   Ayudar a reducir el costo y la complejidad como sigue:  

    -   Es independiente del hardware, y no requiere una configuración del almacenamiento específica como SAN o DAS. 

    -   Permite las tecnologías de red y el almacenamiento de productos. 

    -   Presenta facilidad de administración gráfica para nodos y clústeres individuales mediante el Administrador de clústeres de conmutación por error. 

    -   Incluye opciones de scripting completas y a gran escala a través de Windows PowerShell. 

-   Ayudar a reducir el tiempo de inactividad y aumentar la confiabilidad y productividad intrínseca de Windows. 

-   Proporcionar funcionalidades de diagnóstico, métricas de rendimiento y compatibilidad. 

Para más información, consulte [Storage Replica in Windows Server 2016](../storage/storage-replica/storage-replica-overview.md) (Réplica de almacenamiento en Windows Server 2016). 


### <a name="BKMK_CloudWitness"></a>Testigo en la nube  
Testigo en la nube es un nuevo tipo de testigo de cuórum de clúster de conmutación por error en Windows Server 2016 que utiliza Microsoft Azure como punto de arbitraje. El testigo en la nube, como cualquier otro testigo de cuórum, obtiene un voto y pueden participar en los cálculos de cuórum. Puede configurar el testigo en la nube como un testigo de cuórum el Asistente para configurar un cuórum de clúster. 

**¿Qué valor aporta este cambio?**  

Con el testigo en la nube como un clúster de conmutación por error de testigo de quórum que proporciona las siguientes ventajas:  

-   Aprovecha Microsoft Azure y elimina la necesidad de un tercer centro de datos independiente. 

-   Utiliza el estándar públicamente disponible Microsoft Azure Blob Storage que elimina la sobrecarga de mantenimiento adicionales de las máquinas virtuales hospedadas en una nube pública. 

-   Misma cuenta de almacenamiento de Microsoft Azure puede utilizarse para varios clústeres (archivo de un blob por clúster; Id. exclusivo de clúster usado como nombre de archivo de blob). 

-   Proporciona un muy bajo costo en curso para la cuenta de almacenamiento (muy pequeños datos escritos por el archivo de blob, archivo de blob se actualiza solo una vez cuando se cambia el estado de los nodos del clúster). 

Para obtener más información, consulte [implementar una nube testigo para un clúster de conmutación](deploy-cloud-witness.md). 

**¿Qué funciona de manera diferente?**  

Esta funcionalidad es nueva en Windows Server 2016. 

### <a name="BKMK_VMs"></a>Resistencia de la máquina virtual  
**Resistencia de proceso** Windows Server 2016 incluye la resistencia de proceso de aumento de las máquinas virtuales para ayudar a reducir los problemas de comunicación dentro del clúster en el clúster de proceso como sigue: 

-   **Opciones de resistencia disponibles para las máquinas virtuales:**  Ahora puede configurar las opciones de resistencia de máquina virtual que definen el comportamiento de las máquinas virtuales durante los errores transitorios:  

    -   **Nivel de resistencia:** Le ayuda a definir cómo se controlan los errores transitorios. 

    -   **Período de resistencia:**  Le ayuda a definir cuánto todas las máquinas virtuales se pueden ejecutar aislado. 

-   **Cuarentena de nodos en mal estado:** Nodos en mal estado se ponen en cuarentena y ya no se permiten para unirse al clúster. Esto impide que los nodos oscilación afectando negativamente a otros nodos y el clúster en general. 

Para obtener más información máquina virtual proceso resistencia flujo de trabajo y el nodo de cuarentena configuraciones que controlan cómo se coloca el nodo en aislamiento o poner en cuarentena, consulte [resistencia de máquina Virtual de proceso en Windows Server 2016](http://blogs.msdn.com/b/clustering/archive/2015/06/03/10619308.aspx). 

**Resistencia de almacenamiento** en Windows Server 2016, las máquinas virtuales están más resistentes a errores transitorios de almacenamiento. La resistencia de máquinas virtuales mejorada le ayuda a conservar los Estados de sesión de máquina virtual de inquilino si se produce una interrupción de almacenamiento. Esto se logra mediante la máquina virtual inteligente y rápida respuesta a problemas de infraestructura de almacenamiento. 

Cuando una máquina virtual se desconecta de su almacenamiento subyacente, se detiene y espera a que el almacenamiento recuperar. Mientras está en pausa, la máquina virtual conserve el contexto de aplicaciones que se ejecutan en él. Cuando se restaura la conexión de la máquina virtual para su almacenamiento, la máquina virtual se vuelve a su estado de ejecución. Como resultado, se conserva el estado de sesión de la máquina inquilino en la recuperación. 

En Windows Server 2016, resistencia del almacenamiento de máquina virtual es compatible con y optimizado para clústeres invitados demasiado. 

### <a name="BKMK_Diagnostics"></a>Mejoras de diagnósticos en clústeres de conmutación por error  
Para ayudar a diagnosticar problemas con los clústeres de conmutación por error, Windows Server 2016 incluye lo siguiente:  

-   Varias mejoras en los archivos de registro de clúster (por ejemplo, el registro de información de zona horaria y DiagnosticVerbose) que hace es más fácil solucionar problemas de agrupación en clústeres de conmutación por error. Para obtener más información, consulte [2016 conmutación por error de clúster de solución de problemas de mejoras de Windows Server - registro de clúster](http://blogs.msdn.com/b/clustering/archive/2015/05/15/10614930.aspx). 

-   Un nuevo tipo de un volcado de **volcado de memoria activa**, que filtra la mayoría de las páginas de memoria asignada a las máquinas virtuales y, por lo tanto, hace que el memory.dmp mucho más pequeño y fácil guardar o copiar. Para obtener más información, consulte [2016 conmutación por error de clúster de solución de problemas de mejoras de Windows Server - de volcado de memoria activa](http://blogs.msdn.com/b/clustering/archive/2015/05/18/10615526.aspx). 

### <a name="BKMK_SiteAware"></a>Clústeres de conmutación por error con reconocimiento de sitios  
Windows Server 2016 incluye clústeres de conmutación consciente de sitio que permiten a los nodos de grupo de clústeres extendidos según su ubicación física (sitio). Reconocimiento de sitios de clúster mejora las operaciones clave durante el ciclo de vida del clúster como comportamiento de conmutación por error, las directivas de colocación, latidos entre los nodos y el comportamiento de cuórum. Para obtener más información, consulte [clústeres de conmutación por error con reconocimiento de sitios en Windows Server 2016](http://blogs.msdn.com/b/clustering/archive/2015/08/19/10636304.aspx). 

### <a name="BKMK_multidomainclusters"></a>Grupo de trabajo y de dominios múltiples clústeres  
En Windows Server 2012 R2 y versiones anteriores, sólo se puede crear un clúster entre los nodos miembro Unidos al mismo dominio. Windows Server 2016 rompe estas barreras y presenta la capacidad para crear un clúster de conmutación por error sin dependencias de Active Directory. Ahora puede crear clústeres de conmutación por error en las siguientes configuraciones:  

-   **Clústeres de dominio único.** Clústeres con todos los nodos Unidos al mismo dominio. 

-   **Clústeres de varios dominios.** Clústeres con nodos que son miembros de dominios diferentes. 

-   **Clústeres de grupo de trabajo.** Los clústeres con nodos que son servidores miembros o grupos de trabajo (no unidos a un dominio). 

Para obtener más información, consulte [grupo de trabajo y de dominios múltiples clústeres en Windows Server 2016](http://blogs.msdn.com/b/clustering/archive/2015/08/17/10635825.aspx)  
### <a name="BKMK_VMLoadBalancing"></a>Equilibrio de carga de máquina virtual  
Equilibrio de carga en la máquina virtual es una nueva característica de clústeres de conmutación por error que facilita el equilibrio de carga sin problemas de máquinas virtuales entre los nodos en un clúster. Nodos sobrecargados se identifican en función de la máquina virtual de memoria y uso de CPU en el nodo. Las máquinas virtuales, a continuación, se mueven (migrar en vivo) desde un nodo comprometido a nodos con un ancho de banda disponible (si procede). La agresividad de compensación puede optimizarse para garantizar un rendimiento óptimo del clúster y uso. Equilibrio de carga está habilitada de forma predeterminada en Windows Server 2016 Technical Preview. Sin embargo, el equilibrio de carga está deshabilitado cuando se habilita la optimización dinámica de SCVMM. 

### <a name="BKMK_VMStartOrder"></a>Orden de inicio de la máquina virtual  
Orden de inicio de máquina virtual es una nueva característica de clústeres de conmutación por error que presenta la orquestación de orden de inicio para las máquinas virtuales (y todos los grupos) en un clúster. Ahora se pueden agrupar las máquinas virtuales en niveles y las dependencias de orden de inicio se pueden crear entre distintos niveles. Esto garantiza que las máquinas virtuales más importantes (por ejemplo, los controladores de dominio o la utilidad de máquinas virtuales) se inició en primer lugar. Las máquinas virtuales no se inicia hasta que también se inician las máquinas virtuales que tienen una dependencia en. 

### <a name="BKMK_SMBMultiChannel"></a> SMB multicanal simplificada y redes de clústeres de varias NIC  
Las redes de clústeres de conmutación por error ya no se limitan a una sola NIC por cada subred de red. Con simplificado SMB multicanal y redes de clústeres de varias NIC, configuración de red es automática y todas las NIC en la subred se puede usar para el tráfico de clúster y la carga de trabajo. Esta mejora permite a los clientes a maximizar el rendimiento de la red de Hyper-V, instancia de clúster de conmutación por error de SQL Server y otras cargas de trabajo SMB. 

Para obtener más información, consulte [simplificado SMB multicanal y redes de clústeres de varias NIC](smb-multichannel.md).

## <a name="see-also"></a>Vea también  
* [Almacenamiento](../storage/storage.md)  
* [Novedades de almacenamiento en Windows Server 2016](../storage/whats-new-in-storage.md)  
