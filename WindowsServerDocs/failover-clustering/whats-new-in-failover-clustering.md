---
ms.assetid: 350aa5a3-5938-4921-93dc-289660f26bad
title: "Novedades en el clúster de conmutación por error en Windows Server"
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: get-started-article
manager: dongill
author: JasonGerend
ms.author: jgerend
ms.date: 10/11/2016
ms.openlocfilehash: a4330f62095e13f2f4736f15924ed31fb4893e7a
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="whats-new-in-failover-clustering-in-windows-server-2016"></a>Novedades en el clúster de conmutación por error en Windows Server 2016
> Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Este tema explica las funcionalidades nuevas y modificadas en el clúster de conmutación por error en Windows Server 2016.  

## <a name="BKMK_RollingUpgrade"></a>Actualización del sistema operativo de giro de clúster  
Una nueva característica de clústeres de conmutación por error, clúster de giro actualización del sistema operativo, permite que un administrador actualizar el sistema operativo de los nodos del clúster de Windows Server 2012 R2 a Windows Server 2016 sin detener la Hyper-V o las cargas de trabajo de servidor de archivos de escalado. Con esta característica, pueden evitarse las sanciones contra los acuerdos de nivel de servicio (SLA) de tiempo de inactividad.  

**¿Qué valor agregar este cambio?**  

Actualizar un clúster Hyper-V o el servidor de archivos de escalado de Windows Server 2012 R2 a Windows Server 2016 ya no requiere el tiempo de inactividad. El clúster seguirán funcionando en un nivel de Windows Server 2012 R2 hasta que todos los nodos del clúster ejecutan Windows Server 2016. El nivel funcional del clúster se actualiza a Windows Server 2016 mediante la cmdlt de Windows PowerShell `Update-ClusterFunctionalLevel`.  

> [!WARNING]  
> -   Después de actualizar el nivel funcional del clúster, no se puede volver a un nivel funcional del clúster de Windows Server 2012 R2.  
> -   Hasta que la `Update-ClusterFunctionalLevel` cmdlet se ejecuta, el proceso es reversible y se pueden agregar nodos de Windows Server 2012 R2 y Windows Server 2016 nodos pueden eliminarse.  

**¿Qué funciona de manera diferente?**  

Un clúster de conmutación por error Hyper-V o el servidor de archivos de escalado ahora se pueden actualizar fácilmente sin ningún tiempo de inactividad o necesitas para crear un nuevo clúster con los nodos que se ejecutan el sistema operativo de Windows Server 2016. Clústeres de migrar a Windows Server 2012 R2 era necesario desconectar el clúster existente y volver a instalar el nuevo sistema operativo para cada nodos y, a continuación, poner el clúster en línea. El proceso anterior era incómodo y requiere el tiempo de inactividad. Sin embargo, en Windows Server 2016, el clúster no debas quedarte sin conexión en cualquier momento.  

Los sistemas operativos de clúster para la actualización en fases son para cada nodo en un clúster:  
-   El nodo está en pausa y descargado de todas las máquinas virtuales que se ejecutan en él.  
-   Las máquinas virtuales (o en otras cargas de trabajo de clúster) se migran a otro nodo del clúster. Las máquinas virtuales se migran a otro nodo del clúster.  
-   Se quita el sistema operativo existente y se realiza una instalación limpia del sistema operativo Windows Server 2016 en el nodo.  
-   Se agrega el nodo que se ejecutan el sistema operativo de Windows Server 2016 nuevo al clúster.  
-   En este punto, el clúster se dice que se ejecutan en modo mixto, porque los nodos del clúster ejecutan Windows Server 2012 R2 o Windows Server 2016.  
-   El nivel funcional del clúster permanece en Windows Server 2012 R2. En este nivel funcional, nuevas características de Windows Server 2016 que afectan a la compatibilidad con versiones anteriores del sistema operativo no estará disponibles.  
-   Finalmente, todos los nodos se actualizan a Windows Server 2016.  
-   Nivel funcional del clúster, a continuación, se cambia a Windows Server 2016 mediante el cmdlet de Windows PowerShell `Update-ClusterFunctionalLevel`. En este punto, puedes sacar provecho de las características de Windows Server 2016.  

Para obtener más información, consulta [clúster giro actualización del sistema operativo](cluster-operating-system-rolling-upgrade.md).  

## <a name="BKMK_SR"></a>Réplica de almacenamiento  
Réplica de almacenamiento es una nueva característica que permite el almacenamiento independiente, la replicación de nivel de bloque, sincrónica entre servidores o clústeres de recuperación ante desastres, así como la expansión de un clúster de conmutación por error entre sitios. Replicación sincrónica permite la creación de reflejo de datos de sitios físicos con volúmenes de bloqueo coherente para garantizar la pérdida de datos en el nivel de sistema de archivos. Replicación asincrónica permite que la extensión de sitio más allá de los intervalos metropolitanas con la posibilidad de pérdida de datos.  

**¿Qué valor agregar este cambio?**  

Réplica de almacenamiento te permite hacer lo siguiente:  

-   Proporciona una única solución de recuperación ante desastres de proveedor para planificado e interrupciones de cargas de trabajo críticos misión.  

-   Usar para PYMES3 transporte con el rendimiento, la escalabilidad y la confiabilidad comprobada.  

-   Ampliar los clústeres de conmutación por error de Windows a distancias metropolitanas.  

-   Utilizar principio a fin de software de Microsoft para el almacenamiento y clústeres como Hyper-V, réplica de almacenamiento, espacios de almacenamiento, clúster, servidor de archivos de escalado, para PYMES3, desduplicación de datos y ReFS/NTFS.  

-   Ayudar a reducir los costos y la complejidad como sigue:  

    -   Es independiente del hardware, sin el requisito para una configuración de almacenamiento específico como DAS o SAN.  

    -   Permite que las mercancías tecnologías de almacenamiento y redes.  

    -   Facilidad de características de administración de gráficos para los nodos individuales y clústeres mediante el Administrador de clúster de conmutación por error.  

    -   Incluye opciones de scripts completas y a gran escala a través de Windows PowerShell.  

-   Ayudar a reducir el tiempo de inactividad y aumentar la confiabilidad y la productividad de los intrínseca de Windows.  

-   Proporcionar compatibilidad, las métricas de rendimiento y diagnóstico.  

Para obtener más información, consulta el [réplica de almacenamiento en Windows Server 2016](../storage/storage-replica/storage-replica-overview.md).  


## <a name="BKMK_CloudWitness"></a>Auxiliar de nube  
Nube auxiliar es un nuevo tipo de testigo de quórum de clúster de conmutación por error en Windows Server 2016 que saca provecho de Microsoft Azure como punto de arbitraje. El testigo de nube, como cualquier otro testigo quórum, obtiene un voto y participar en los cálculos de quórum. Puedes configurar a testigo de nube como testigo quórum con el clúster quórum Asistente para configurar un.  

**¿Qué valor agregar este cambio?**  

El uso de nube auxiliar como un clúster de conmutación por error auxiliares quórum ofrece las siguientes ventajas:  

-   Saca provecho de Microsoft Azure y elimina la necesidad de un tercer centro de datos independiente.  

-   Usa el estándar disponibles públicamente Microsoft Azure Blob de almacenamiento que elimina la sobrecarga de mantenimiento adicionales de máquinas virtuales alojados en la nube pública.  

-   Misma cuenta de Microsoft Azure almacenamiento puede usarse para varios clústeres (archivo de un blob por clúster; utilizado como nombre de archivo del blob de identificador único de clúster).  

-   Proporciona un coste muy bajo de continua a la cuenta de almacenamiento (muy pequeños datos escritos por cada archivo de blob, actualiza el archivo de blob solo una vez cuando cambia el estado de los nodos del clúster).  

Para obtener más información, consulta [implementar una nube auxiliar para un clúster de conmutación](deploy-cloud-witness.md).  

**¿Qué funciona de manera diferente?**  

Esta funcionalidad es nueva en Windows Server 2016.  

## <a name="BKMK_VMs"></a>Resistencia de máquina virtual  
**Calcular la resistencia** Windows Server 2016 incluye una mayor máquinas virtuales resistencia de cálculo para ayudar a reducir los problemas de comunicación dentro del clúster del clúster de cálculo como sigue: 

-   **Opciones de resistencia disponibles para las máquinas virtuales:** ahora puedes configurar opciones de resistencia de máquina virtual que definen el comportamiento de las máquinas virtuales durante fallas transitorias:  

    -   **Nivel de resistencia:** le ayuda a definir cómo se controlan los errores transitorios.  

    -   **Período de resistencia:** le ayuda a definir cuánto todas las máquinas virtuales se pueden ejecutar aislado.  

-   **Poner en cuarentena de nodos incorrectos:** nodos incorrectos se puso en cuarentena y ya no tienen permiso para unirse al clúster. Esto impide que los nodos aleteo afecte negativamente otros nodos y el clúster general.  

Para más información máquina virtual cálculo resistencia flujo de trabajo y el nodo de configuración de cuarentena que controlan cómo se coloca el nodo en cuarentena o aislamiento, consulta [resistencia calcular de máquina Virtual en Windows Server 2016](http://blogs.msdn.com/b/clustering/archive/2015/06/03/10619308.aspx).  

**Resistencia de almacenamiento** en Windows Server 2016, las máquinas virtuales son más resistentes a errores de almacenamiento temporal. El estado de resistencia mejorada máquina virtual ayuda a conservar los Estados de sesión de máquina virtual de inquilino en caso de una interrupción de almacenamiento. Esto se logra por respuesta inteligente y rápido máquina virtual problemas de infraestructura de almacenamiento.  

Cuando se desconecta una máquina virtual de su almacenamiento subyacente, pausa y espera almacenamiento recuperar. Mientras está en pausa, la máquina virtual conserva el contexto de las aplicaciones que se ejecutan en él. Cuando se restaura la conexión de la máquina virtual para su almacenamiento, la máquina virtual se devuelve a su estado de ejecución. Como resultado, se conserva el estado de sesión del equipo de inquilino en recuperación.  

En Windows Server 2016, resistencia de almacenamiento de máquina virtual es consciente y optimizadas para clústeres de invitado demasiado.  

## <a name="BKMK_Diagnostics"></a>Mejoras de diagnósticos de clústeres de conmutación por error  
Para ayudar a diagnosticar problemas de clústeres de conmutación por error, Windows Server 2016 incluye lo siguiente:  

-   Varias mejoras en los archivos de registro de clúster (por ejemplo, el registro de información de zona horaria y DiagnosticVerbose) que hace que sea más fácil solucionar problemas clústeres de conmutación por error. Para obtener más información, consulta [mejoras Windows Server 2016 conmutación por error clúster solución de problemas - registro del clúster](http://blogs.msdn.com/b/clustering/archive/2015/05/15/10614930.aspx).  

-   Un nuevo tipo de un volcado de memoria de **volcado de memoria activa**, que filtra la mayoría de las páginas memoria asignada a máquinas virtuales y, por lo tanto, hace que la memory.dmp mucho más pequeños y fáciles de guardar o copiar.  Para obtener más información, consulta [mejoras Windows Server 2016 conmutación por error clúster solución de problemas - volcar Active](http://blogs.msdn.com/b/clustering/archive/2015/05/18/10615526.aspx).  

## <a name="BKMK_SiteAware"></a>Tenga en cuenta el sitio de conmutación por error clústeres  
Windows Server 2016 incluye sitio con reconocimiento de conmutación por error del clúster que permiten a los nodos de grupo en los clústeres estirados en función de su ubicación física (sitio). Reconocimiento de sitios de clúster mejora operaciones clave durante el ciclo de vida de clúster, como el comportamiento de conmutación por error, las políticas de ubicación, latido entre los nodos y el comportamiento de quórum. Para obtener más información, consulta [con reconocimiento de sitio clústeres de conmutación por error en Windows Server 2016](http://blogs.msdn.com/b/clustering/archive/2015/08/19/10636304.aspx).  

## <a name="BKMK_multidomainclusters"></a>Grupo de trabajo y varios dominios clústeres  
En Windows Server 2012 R2 y versiones anteriores, solo puede crear un clúster entre los nodos de miembro Unidos al mismo dominio.   Windows Server 2016 rompe estas barreras y presenta la capacidad para crear un clúster de conmutación por error sin dependencias de Active Directory. Ahora puedes crear clústeres de conmutación por error en las siguientes configuraciones:  

-   **Clústeres solo dominio.** Clústeres con todos los nodos Unidos al mismo dominio.  

-   **Clústeres de varios dominios.** Clústeres con los nodos que son miembros de distintos dominios.  

-   **Clústeres de grupo de trabajo.** Clústeres con los nodos que son servidores miembro o grupo de trabajo (no unidos a un dominio).  

Para obtener más información, consulta [clústeres de grupo de trabajo y de varios dominios en Windows Server 2016](http://blogs.msdn.com/b/clustering/archive/2015/08/17/10635825.aspx)  
## <a name="BKMK_VMLoadBalancing"></a>Equilibrio de carga de máquina virtual  
Equilibrio de carga de máquina virtual es una nueva característica de clústeres de conmutación por error que facilita el equilibrio de carga fluida de máquinas virtuales en todos los nodos de un clúster. Nodos exceso comprometidos se identifican en función de la máquina virtual de memoria y el uso de CPU en el nodo. Máquinas virtuales, a continuación, se mueven (live migrado) desde un nodo exceso compromete a nodos con un ancho de banda disponible (si procede). Pueden ajustar la agresividad del equilibrio para garantizar la utilización y un rendimiento óptimo del clúster.  Equilibrio de carga está habilitada de manera predeterminada en Windows Server 2016 Technical Preview. Sin embargo, el equilibrio de carga está deshabilitado cuando se habilita la optimización SCVMM dinámica.  

## <a name="BKMK_VMStartOrder"></a>Orden de inicio de máquina virtual  
El orden de inicio de máquina virtual es una nueva característica de clústeres de conmutación por error que presenta la organización de orden de inicio para las máquinas virtuales (y todos los grupos) en un clúster. Ahora se pueden agrupar máquinas virtuales en niveles, y se pueden crear dependencias de orden de inicio entre diferentes niveles. Esto garantiza que las máquinas virtuales más importantes (como una utilidad o controladores de dominio de máquinas virtuales) se inician primeros. Máquinas virtuales no se inician hasta que las máquinas virtuales que tienen una dependencia en también se inician.  

## <a name="BKMK_SMBMultiChannel"></a>Simplificada multicanal SMB y NIC de varias redes de clúster  
Ya no están limitadas a una única NIC por subred redes del clúster de conmutación por error o de red. Con simplificado SMB multicanal y NIC de varias redes de clúster, configuración de red es automática y cada NIC en la subred puede usarse para el tráfico de clúster y carga de trabajo. Esta mejora permite a los clientes maximizar el rendimiento de red para Hyper-V, instancia de clúster de conmutación por error de SQL Server y otras cargas de trabajo SMB.  

Para obtener más información, consulta [simplificado SMB multicanal y redes del clúster Multi-NIC](smb-multichannel.md).

## <a name="see-also"></a>Consulta también  
* [Almacenamiento](../storage/storage.md)  
* [Novedades en el almacenamiento en Windows Server 2016](../storage/whats-new-in-storage.md)  
