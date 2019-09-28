---
title: Información general sobre Réplica de almacenamiento
ms.prod: windows-server
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 4/26/2019
ms.assetid: e9b18e14-e692-458a-a39f-d5b569ae76c5
ms.openlocfilehash: 620ab75fc5f44af7cd754847e3e5b717eece5057
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393821"
---
# <a name="storage-replica-overview"></a>Información general sobre Réplica de almacenamiento

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual)

Réplica de almacenamiento es una tecnología de Windows Server que permite la replicación de volúmenes entre servidores o clústeres para la recuperación ante desastres. También permite crear clústeres de conmutación por error ampliados que abarcan dos sitios y que tienen todos los nodos sincronizados.

La Réplica de almacenamiento admite la replicación sincrónica y asincrónica:

* La **replicación sincrónica** refleja los datos en un sitio de red de baja latencia con volúmenes coherentes frente a bloqueos para asegurar que no se produce absolutamente ninguna pérdida de datos en el nivel de sistema de archivos durante un error.
* La **Replicación asincrónica** refleja los datos entre sitios más allá de los intervalos metropolitanos a través de enlaces de red con latencias superiores, pero sin ninguna garantía de que ambos sitios tengan copias idénticas de los datos en el momento del error.

## <a name="why-use-storage-replica"></a>¿Por qué usar Réplica de almacenamiento?

Réplica de almacenamiento ofrece capacidades de recuperación ante desastres y preparación en Windows Server. Windows Server ofrece la tranquilidad de que no haya pérdida de datos, con la posibilidad de proteger los datos de forma sincrónica en diferentes bastidores, plantas, edificios, campus, condados y ciudades. Una vez que se produce un desastre, todos los datos existen en cualquier parte sin posibilidad de pérdida. Lo mismo se aplica *antes de* que se produzca un desastre; Réplica de almacenamiento ofrece la posibilidad de cambiar las cargas de trabajo a ubicaciones seguras antes de que lleguen las catástrofes cuando se tiene constancia de estas con antelación: una vez más, sin pérdida de datos.  

Réplica de almacenamiento permite un uso más eficaz de varios centros de datos. Al extender o replicar los clústeres, las cargas de trabajo se pueden ejecutar en varios centros de datos para un acceso más rápido para los usuarios y las aplicaciones de proximidad local, y también para una mejor distribución de la carga y uso de los recursos de proceso. Si un desastre deja un centro de datos sin conexión, puede mover sus cargas de trabajo habituales temporalmente a otro sitio.  

Con Réplica de almacenamiento, puede retirar los sistemas de replicación de archivos existentes, como Replicación DFS, que se sentían presionados a rendir a un nivel cuya condición de soluciones de recuperación ante desastres de gama baja no les permitía alcanzar. Aunque Replicación DFS funciona bien a través de redes de ancho de banda extremadamente bajo, la latencia es muy alta (suele medirse en horas o días). Esto se debe al requisito establecido de cerrar los archivos y sus limitaciones artificiales diseñadas para impedir la congestión de la red. Con estas características de diseño, los archivos más recientes y más utilizados de una Replicación DFS son los que cuentan con menos probabilidades de replicarse. Réplica de almacenamiento funciona por debajo del nivel de archivo y no tiene ninguna de estas restricciones.  

Réplica de almacenamiento también admite la replicación asincrónica durante intervalos más prolongados y redes de latencia mayor. Dado que no está basado en puntos de control y, en su lugar, se replica continuamente, la diferencia de los cambios tiende a ser mucho menor que los productos basados en instantáneas. Además, Réplica de almacenamiento opera en el nivel de partición y, por tanto, replica todas las instantáneas de VSS creadas por Windows Server o el software de copia de seguridad; esto permite el uso de instantáneas de datos coherentes con la aplicación para la recuperación en un momento del tiempo, especialmente datos de usuarios no estructurados replicados asincrónicamente.  

## <a name="BKMK_SRSupportedScenarios"></a>Configuraciones admitidas

Puede implementar réplica de almacenamiento en un clúster extendido, entre un clúster y una configuración de servidor a servidor (consulte las figuras 1-3).

**Clúster extendido** permite la configuración de equipos y almacenamiento en un único clúster, donde algunos nodos comparten un conjunto de almacenamiento asimétrico y otros nodos comparten otro y, luego, se replican de manera sincrónica o asincrónica con el reconocimiento de sitios. Este escenario puede utilizar Espacios de almacenamiento con almacenamiento SAS compartido, SAN y LUC asociados a iSCSI. Se administra con PowerShell y la herramienta gráfica del Administrador de clústeres de conmutación por error, y permite la conmutación por error de carga de trabajo automatizada.  

![Diagrama que muestra dos nodos de clúster en Nueva York que usan la Réplica de almacenamiento para replicar su almacenamiento con dos nodos en Nueva Jersey](./media/Storage-Replica-Overview/Storage_SR_StretchCluster.png)  

@NO__T 0FIGURE 1: Replicación de almacenamiento en un clúster extendido con réplica de almacenamiento @ no__t-0  

**Clúster a clúster** permite la replicación entre dos clústeres independientes, donde un clúster replica de forma sincrónica o asincrónica con otro clúster. Este escenario puede utilizar Espacios de almacenamiento directo, espacios de almacenamiento con almacenamiento SAS compartido, SAN y LUN asociados a iSCSI. Se administra con el centro de administración de Windows y PowerShell, y requiere la intervención manual para la conmutación por error. 

![Diagrama que muestra un clúster en Los Ángeles que usa la Réplica de almacenamiento para replicar su almacenamiento a un clúster distinto en Las Vegas](./media/Storage-Replica-Overview/Storage_SR_ClustertoCluster.png)  

@NO__T 0FIGURE 2: Replicación de almacenamiento de clúster a clúster mediante la réplica de almacenamiento @ no__t-0  

**Servidor a servidor** permite la replicación sincrónica y asincrónica entre dos servidores independientes, usando Espacios de almacenamiento con almacenamiento SAS compartido, SAN y LUN asociados a iSCSI y unidades locales. Se administra con el centro de administración de Windows y PowerShell, y requiere la intervención manual para la conmutación por error.  

![Diagrama que muestra un servidor en la replicación de la compilación 5 con un servidor de la compilación 9](./media/Storage-Replica-Overview/Storage_SR_ServertoServer.png)  

@NO__T 0FIGURE 3: Replicación de almacenamiento de servidor a servidor con réplica de almacenamiento @ no__t-0  

> [!NOTE]
> También puede configurar la replicación de servidor al propio dispositivo, usando cuatro volúmenes independientes en un equipo. Sin embargo, esta guía no incluye este escenario.  

## <a name="BKMK_SR2"></a> Características de réplica de almacenamiento  

* **Cero pérdida de datos, replicación a nivel de bloque**. Con replicación sincrónica, no hay ninguna posibilidad de pérdida de datos. Con la replicación a nivel de bloque, no hay ninguna posibilidad de bloqueo de archivos.  

* **Implementación y administración sencillas**. Réplica de almacenamiento tiene un mandato de diseño de uso sencillo. La creación de una asociación de replicación entre dos servidores puede utilizarse en el centro de administración de Windows. La implementación de clústeres extendidos usa un asistente intuitivo en la conocida herramienta Administrador de clústeres de conmutación por error.   

* **Invitado y host**. Todas las funcionalidades de Réplica de almacenamiento se exponen en implementaciones virtualizadas basadas en host y en invitado. Esto significa que los invitados pueden replicar sus volúmenes de datos incluso si se ejecutan en plataformas de virtualización que no son de Windows o en nubes públicas, siempre que se use Windows Server en el invitado.  

* **Base en SMB3**. Réplica de almacenamiento utiliza la tecnología probada y asentada de SMB 3, publicada por primera vez en Windows Server 2012. Esto significa que todas las características avanzadas de SMB, como la compatibilidad directa en SMB y multicanal en tarjetas de red RoCE, iWARP e InfiniBand RDMA, están disponibles para Réplica de almacenamiento.   

* **Seguridad**. A diferencia de los productos de muchos proveedores, Réplica de almacenamiento tiene incorporada la tecnología de seguridad líder del sector. Esto incluye la firma de paquetes, el cifrado completo de datos de AES-128-GCM, la compatibilidad con la aceleración del cifrado de Intel AES-NI y la prevención de ataque de tipo "Man in the middle" mediante la integridad por autenticación previa. Réplica de almacenamiento utiliza Kerberos AES256 para toda la autenticación entre los nodos.  

* **Sincronización inicial de alto rendimiento**. Réplica de almacenamiento admite una primera sincronización inicializada, donde un subconjunto de datos ya existe en un destino procedente de copias más antiguas, copias de seguridad o unidades enviadas. La replicación inicial solo copia los bloques distintos, lo que puede acortar el tiempo de sincronización inicial y evitar que los datos usen ancho de banda limitado. El hecho de que las réplicas de almacenamiento bloqueen el cálculo y el agregado de la suma de comprobación significa que el rendimiento de la sincronización inicial solo está limitado por la velocidad del almacenamiento y la red.  

* **Grupos de coherencia**. La ordenación de escritura garantiza que las aplicaciones como Microsoft SQL Server pueden escribir en varios volúmenes replicados y saber que los datos se escriben en el servidor de destino de forma secuencial.  

* **Delegación de usuarios**. Es posible delegar permisos a los usuarios para administrar la replicación sin que sean miembros del grupo de administradores integrados en los nodos replicados, limitando así el acceso a las zonas no relacionadas.  

* **Restricción de red**. Réplica de almacenamiento puede limitarse a redes individuales por servidor y volúmenes replicados, con el fin de proporcionar ancho de banda de aplicación, copia de seguridad y software de administración.  

* **Aprovisionamiento fino**. Se admite el aprovisionamiento fino en Espacios de almacenamiento y dispositivos SAN, con el fin de proporcionar tiempos de replicación iniciales casi instantáneos en muchas circunstancias.  

La réplica de almacenamiento incluye las siguientes características:  

| Característica | Detalles |
| ----------- | ----------- |  
| Tipo | Base en host |
| Sincrónico | Sí |
| Asincrónico | Sí |
| Independiente del hardware de almacenamiento | Sí |
| Unidad de replicación | Volumen (partición) |
| Creación de clústeres de stretch de Windows Server | Sí |
| Replicación de servidor a servidor | Sí |
| Replicación de clúster a clúster | Sí |
| Transporte | SMB3 |
| Red | TCP/IP o RDMA |
| Compatibilidad con restricción de red | Sí |
| RDMA* | iWARP, InfiniBand, RoCE v2 |
| Requisitos de firewall de puertos de red de replicación | Puerto único de IANA (TCP 445 o 5445) |
| Múltiples rutas o multicanal | Sí (SMB3) |
| Compatibilidad con Kerberos | Sí (SMB3) |
| Cifrado y firma por cable|Sí (SMB3) |
| Conmutaciones por error permitidas por cada volumen | Sí |
| Compatibilidad con almacenamiento de aprovisionamiento fino | Sí |
| Interfaz de usuario de administración incluida | PowerShell, Administrador de clústeres de conmutación por error |

*Puede requerir equipos y cableado adicionales de largo alcance.  

## <a name="BKMK_SR3"></a>Requisitos previos de réplica de almacenamiento

* Bosque de Active Directory Domain Services.
* Espacios de almacenamiento con JBOD SAS, Espacios de almacenamiento directo, SAN de canal de fibra, VHDX compartido, destino iSCSI o almacenamiento SAS/SCSI/SATA local. SSD o más rápido recomendado para las unidades de registro de replicación. Microsoft recomienda que el almacenamiento de registro sea más rápido que el almacenamiento de datos. Nunca se debe utilizar volúmenes de registro para otras cargas de trabajo.
* Al menos una conexión de Ethernet/TCP en cada servidor para replicación sincrónica, pero preferiblemente RDMA.
* Al menos 2GB de RAM y dos núcleos por servidor.
* Una red entre servidores con ancho de banda suficiente para contener la carga de trabajo de escritura de E/S y un promedio de 5 ms de latencia de ida y vuelta o menos para la replicación sincrónica. La replicación asincrónica no tiene una recomendación de latencia.
* Windows Server, Datacenter Edition o Windows Server, Standard Edition. La réplica de almacenamiento que se ejecuta en Windows Server, Standard Edition, tiene las siguientes limitaciones:

  * Debe usar Windows Server 2019 o posterior.
  * Réplica de almacenamiento replica un volumen único en lugar de un número ilimitado de volúmenes.
  * Los volúmenes pueden tener un tamaño de hasta 2 TB en lugar de un tamaño ilimitado.

##  <a name="BKMK_SR4"></a> Información general

Esta sección incluye información sobre los términos de la industria de alto nivel, la replicación sincrónica y asincrónica y los comportamientos principales.

### <a name="high-level-industry-terms"></a>Términos de la industria de alto nivel

Recuperación ante desastres hace referencia a un plan de contingencia para recuperarse de catástrofes en los sitios para que el negocio siga funcionando. La recuperación ante desastres de datos implica varias copias de datos de producción en una ubicación física diferente. Por ejemplo, un clúster extendido, donde la mitad de los nodos está en un sitio y la otra mitad en otro. Preparación ante desastres hace referencia a un plan de contingencia para mover cargas de trabajo de manera preventiva a una ubicación diferente antes de que ocurra un desastre próximo, como un huracán.  

Los contratos de nivel de servicio definen la disponibilidad de las aplicaciones de un negocio y su tolerancia a la inactividad y la pérdida de datos durante interrupciones planeadas y no planeadas. El objetivo de tiempo de recuperación define cuánto tiempo puede tolerar el negocio la inaccesibilidad total a los datos. El objetivo de punto de recuperación (RPO) define la cantidad de datos que el negocio puede permitirse perder.  

### <a name="synchronous-replication"></a>Replicación sincrónica

La replicación sincrónica garantiza que la aplicación escribe los datos en dos ubicaciones a la vez antes de la finalización de la operación de E/S. Esta replicación es más adecuada para los datos indispensables, ya que requiere inversiones en red y almacenamiento y conlleva el riesgo de que se degrade el rendimiento de la aplicación.  

Cuando se producen escrituras de la aplicación en la copia de datos de origen, el almacenamiento que se origina no reconoce inmediatamente la operación de E/S. En su lugar, estos cambios de datos se replican en la copia de destino remota y devuelven una confirmación. Solo entonces la aplicación recibe la confirmación de E/S. Esto garantiza la sincronización constante del sitio remoto con el sitio de origen, ampliando de hecho la E/S de almacenamiento a través de la red. En caso de error del sitio de origen, las aplicaciones pueden conmutar por error al sitio remoto y reanudar sus operaciones con la garantía de cero pérdida de datos.  

| Modo | Diagrama | Pasos |
| -------- | ----------- | --------- |
| **Sincrónica**<br /><br />Cero pérdida de datos<br /><br />RPO | ![Diagrama que muestra cómo la Réplica de almacenamiento escribe datos en la replicación sincrónica](./media/Storage-Replica-Overview/Storage_SR_SynchronousV2.png) | 1.  La aplicación escribe los datos.<br />2.  Se escriben los datos de registro y estos se replican en el sitio remoto.<br />3.  Se escriben los datos de registro en el sitio remoto.<br />4.  Confirmación del sitio remoto.<br />5.  Confirmación de escritura en la aplicación.<br /><br />t & T1: Datos vaciados en el volumen, los registros siempre escriben |

### <a name="asynchronous-replication"></a>Replicación asincrónica

Por el contrario, la replicación asincrónica significa que, cuando la aplicación escribe los datos, esos datos se replican en el sitio remoto sin garantías de confirmación inmediata. Este modo permite un tiempo de respuesta más rápido para la aplicación, así como una solución de recuperación ante desastres que funciona geográficamente.  

Cuando la aplicación escribe los datos, el motor de replicación captura la escritura y confirma inmediatamente a la aplicación. Los datos capturados se replican entonces a la ubicación remota. El nodo remoto procesa la copia de los datos y confirma de forma diferida en la copia de origen. Puesto que el rendimiento de replicación ya no está en la ruta de acceso de E/S de la aplicación, la capacidad de respuesta y la distancia del sitio remoto son factores menos importantes. No hay riesgo de pérdida de datos si se perdieran los datos de origen y la copia de los datos de destino permaneciera aún en búfer sin abandonar el origen.  

Con su RPO mayor que cero, la replicación asincrónica es menos apropiada para soluciones de alta disponibilidad como clústeres de conmutación por error, ya que están diseñadas para un funcionamiento continuo con redundancia y sin pérdida de datos.  

| Modo | Diagrama | Pasos |
| -------- | ----------- | --------- |
| **Asincrónica**<br /><br />Pérdida de datos de casi cero<br /><br />(depende de varios factores)<br /><br />RPO | ![Diagrama que muestra cómo la Réplica de almacenamiento escribe datos en la replicación asincrónica](./media/Storage-Replica-Overview/Storage_SR_AsynchronousV2.png)|1.  La aplicación escribe los datos.<br />2.  Datos de registro escritos.<br />3.  Confirmación de escritura en la aplicación.<br />4.  Datos replicados en el sitio remoto.<br />5.  Datos de registro escritos en el sitio remoto.<br />6.  Confirmación del sitio remoto.<br /><br />t & T1: Datos vaciados en el volumen, los registros siempre escriben |

### <a name="key-evaluation-points-and-behaviors"></a>Puntos de evaluación y comportamientos clave  

-   Ancho de banda de la red y latencia con almacenamiento más rápido. Hay limitaciones físicas en torno a la replicación sincrónica. Dado que Réplica de almacenamiento implementa un mecanismo de filtrado de E/S mediante registros y el requerimiento de recorridos de ida y vuelta de red, es posible que la replicación sincrónica provoque que la aplicación escriba con más lentitud. Mediante el uso de redes de baja latencia y alto ancho de banda, así como subsistemas de disco de alto rendimiento para los registros, se minimiza la sobrecarga de rendimiento.  

-   El volumen de destino no es accesible durante la replicación en Windows Server 2016. Al configurar la replicación, se desmonta el volumen de destino, pasando a ser inaccesible para cualquier escritura o lectura de los usuarios. Su letra de unidad puede estar visible en interfaces tradicionales, tales como el Explorador de archivos, pero una aplicación no puede tener acceso al volumen. Las tecnologías de replicación a nivel de bloque son incompatibles con el hecho de permitir el acceso al sistema de archivos montado del objetivo de destino en un volumen; NTFS y ReFS no admiten que los usuarios escriban datos en el volumen mientras los bloques cambian en un nivel inferior. 

El cmdlet **Test-failover** en Windows Server, versión 1709 y también se incluyó en windows Server 2019. Esto ahora permite montar temporalmente una instantánea de lectura y escritura del volumen de destino para las copias de seguridad, las pruebas, etc. Consulte https://aka.ms/srfaq para obtener más información.

-   La implementación de Microsoft de la replicación asincrónica es diferente a la de la mayoría. La mayoría de las implementaciones de la industria de la replicación asincrónica se fundamentan en la replicación basada en instantáneas, donde hay transferencias diferenciales periódicas que se desplazan a otro nodo y se fusionan. La replicación asincrónica de Réplica de almacenamiento funciona igual que la replicación sincrónica, salvo que elimina el requisito de una confirmación sincrónica serializada desde el destino. Esto significa que Réplica de almacenamiento teóricamente tiene un menor RPO, ya que se replica continuamente. Sin embargo, esto también significa que se basa en garantías de coherencia de aplicación internas en lugar de usar instantáneas para forzar la coherencia en los archivos de la aplicación. Réplica de almacenamiento garantiza la coherencia de bloqueo en todos los modos de replicación  

-   Muchos clientes usan Replicación DFS como una solución de recuperación ante desastres incluso si a menudo es un escenario poco práctico: Replicación DFS no puede replicar archivos abiertos y está diseñado para minimizar el uso de ancho de banda a costa del rendimiento, provocando grandes diferencias en el punto de recuperación. Réplica de almacenamiento puede permitirle retirar la Replicación DFS de algunos de estos tipos de funciones de recuperación ante desastres.  

-   Réplica de almacenamiento no es una solución de copia de seguridad. Algunos entornos de TI implementan sistemas de replicación como soluciones de copia de seguridad, debido a sus opciones de cero pérdida de datos en comparación con las copias de seguridad diarias. Réplica de almacenamiento replica todos los cambios en todos los bloques de datos en el volumen, independientemente del tipo de cambio. Si un usuario elimina todos los datos de un volumen, réplica de almacenamiento Replica la eliminación al instante en el otro volumen, quitando de forma irrevocable los datos de ambos servidores. No use Réplica de almacenamiento para sustituir una solución de copia de seguridad a un momento dado.  

-   Réplica de almacenamiento no es una Réplica de Hyper-V ni Grupos de disponibilidad AlwaysOn de Microsoft SQL. Réplica de almacenamiento es un motor de propósito general independiente del almacenamiento. Por definición, no puede ajustar su comportamiento tan idealmente como la replicación a nivel de aplicación. Esto puede originar determinadas carencias de características que le recomendamos que implemente o mantenga en tecnologías de replicación de aplicaciones específicas.  

> [!NOTE]
> Este documento contiene una lista de [problemas conocidos](storage-replica-known-issues.md) y comportamientos esperados, así como una sección de [preguntas más frecuentes](storage-replica-frequently-asked-questions.md).
 
### <a name="storage-replica-terminology"></a>Terminología de Réplica de almacenamiento  
En esta guía se usan con frecuencia los términos siguientes:  

-   El origen es el volumen de un equipo que permite escrituras locales y replica hacia fuera. También se conoce como "principal".  

-   El destino es el volumen de un equipo que no permite escrituras locales y replica hacia dentro. También se conoce como "secundario".   

-   Una asociación de replicación es la relación de sincronización entre un equipo de origen y destino para uno o varios volúmenes y utiliza un único registro.  

-   Un grupo de replicación es la organización de los volúmenes y su configuración de replicación dentro de una asociación, en cada uno de los servidores. Un grupo puede contener uno o varios volúmenes.  

### <a name="whats-new-for-storage-replica"></a>Novedades de réplica de almacenamiento

Para obtener una lista de las nuevas características de réplica de almacenamiento en Windows Server 2019, consulte [what's New in Storage (novedades de almacenamiento](../whats-new-in-storage.md#storage-replica2019) ).

## <a name="see-also"></a>Vea también

- [Replicación de clúster extendido con almacenamiento compartido](stretch-cluster-replication-using-shared-storage.md)  
- [Replicación de almacenamiento de servidor a servidor](server-to-server-storage-replication.md)  
- [Replicación de almacenamiento de clúster a clúster](cluster-to-cluster-storage-replication.md)  
- [Réplica de almacenamiento: Problemas conocidos](storage-replica-known-issues.md)  
- [Réplica de almacenamiento: Preguntas más frecuentes](storage-replica-frequently-asked-questions.md)  
- [Espacios de almacenamiento directo en Windows Server 2016](../storage-spaces/storage-spaces-direct-overview.md)
- [Soporte técnico de profesionales de TI de Windows](https://www.microsoft.com/itpro/windows/support)
