---
description: 'Más información sobre: preguntas más frecuentes sobre réplica de almacenamiento'
title: Preguntas frecuentes acerca de Réplica de almacenamiento
manager: siroy
ms.author: nedpyle
ms.topic: how-to
author: nedpyle
ms.date: 04/15/2020
ms.assetid: 12bc8e11-d63c-4aef-8129-f92324b2bf1b
ms.openlocfilehash: 51bd95800ed7658a5123d00d6d04c6fb7b95d111
ms.sourcegitcommit: 7674bbe49517bbfe0e2c00160e08240b60329fd9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98603394"
---
# <a name="frequently-asked-questions-about-storage-replica"></a>Preguntas frecuentes acerca de Réplica de almacenamiento

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual)

Este tema contiene respuestas a las preguntas frecuentes (P+F) acerca de Réplica de almacenamiento.

## <a name="is-storage-replica-supported-on-azure"></a><a name="FAQ1"></a>¿Se admite la réplica de almacenamiento en Azure?

Sí. Puede usar los siguientes escenarios con Azure:

1. Replicación de servidor a servidor dentro de Azure (de forma sincrónica o asincrónica entre máquinas virtuales de IaaS en uno o dos dominios de error del centro de recursos, o de forma asincrónica entre dos regiones independientes)
2. Replicación asincrónica de servidor a servidor entre Azure y el entorno local (mediante VPN o Azure ExpressRoute)
3. Replicación de clúster a clúster dentro de Azure (de forma sincrónica o asincrónica entre máquinas virtuales de IaaS en uno o dos dominios de error del centro de recursos, o de forma asincrónica entre dos regiones independientes)
4. Replicación asincrónica de clúster a clúster entre Azure y el entorno local (mediante VPN o Azure ExpressRoute)

Puede encontrar más notas sobre la agrupación en clústeres invitados en Azure en: [implementación de clústeres invitados de máquinas virtuales de IaaS en Microsoft Azure](https://techcommunity.microsoft.com/t5/Failover-Clustering/Deploying-IaaS-VM-Guest-Clusters-in-Microsoft-Azure/ba-p/372126).

Notas importantes:

1. Azure no admite clústeres invitados de VHDX compartidos, por lo que las máquinas virtuales de clúster de conmutación por error de Windows deben usar destinos iSCSI para la agrupación en clústeres o Espacios de almacenamiento directo de reserva de disco persistente de almacenamiento compartido clásico.
2. Hay Azure Resource Manager plantillas para la agrupación en clústeres de réplica de almacenamiento basado en Espacios de almacenamiento directo en [creación de clústeres de espacios de almacenamiento directo sofs con réplica de almacenamiento para la recuperación ante desastres en regiones de Azure](https://aka.ms/azure-storage-replica-cluster).
3. La comunicación RPC de clúster a clúster en Azure (requerida por las API de clúster para conceder acceso entre el clúster) requiere configurar el acceso a la red para el CNO. Debe permitir el puerto TCP 135 y el intervalo dinámico por encima del puerto TCP 49152. Referencia [sobre la creación de clústeres de conmutación por error de Windows Server en una máquina virtual de IaaS de Azure: red y creación](/archive/blogs/askcore/building-windows-server-failover-cluster-on-azure-iaas-vm-part-2-network-and-creation).
4. Es posible usar clústeres invitados de dos nodos, donde cada nodo usa iSCSI de bucle invertido para un clúster asimétrico replicado por réplica de almacenamiento. Pero esto probablemente tendrá un rendimiento muy deficiente y debe usarse solo para cargas de trabajo o pruebas muy limitadas.

## <a name="how-do-i-see-the-progress-of-replication-during-initial-sync"></a><a name="FAQ2"></a> Cómo ver el progreso de la replicación durante la sincronización inicial
Los mensajes de evento 1237 que aparecen en el registro de eventos de administración de Réplica de almacenamiento del servidor de destino muestran el número de bytes copiados y los bytes restantes cada 10 segundos. También puede utilizar el contador de rendimiento de Réplica de almacenamiento en el que aparece **\Estadística de Réplica de almacenamiento\Total de bytes recibidos** para uno o varios volúmenes replicados. Además, puede consultar el grupo de replicación mediante Windows PowerShell. Por ejemplo, este comando obtiene el nombre de los grupos en el destino y luego consulta un grupo denominado **Replication 2** cada 10 segundos para mostrar el progreso:

```
Get-SRGroup

do{
    $r=(Get-SRGroup -Name "Replication 2").replicas
    [System.Console]::Write("Number of remaining bytes {0}`n", $r.NumOfBytesRemaining)
    Start-Sleep 10
}until($r.ReplicationStatus -eq 'ContinuouslyReplicating')
Write-Output "Replica Status: "$r.replicationstatus

```

## <a name="can-i-specify-specific-network-interfaces-to-be-used-for-replication"></a><a name="FAQ3"></a>¿Se pueden especificar interfaces de red específicas para usarse para replicación?

Sí, mediante `Set-SRNetworkConstraint`. Este cmdlet funciona en el nivel de interfaz y se utiliza en escenarios de clúster y de no clúster.
Por ejemplo, con un servidor independiente (en cada nodo):

```
Get-SRPartnership

Get-NetIPConfiguration
```
Observe la información de la interfaz y la puerta de enlace (en ambos servidores) y las instrucciones de la asociación. A continuación, ejecute:

```
Set-SRNetworkConstraint -SourceComputerName sr-srv06 -SourceRGName rg02 -
SourceNWInterface 2 -DestinationComputerName sr-srv05 -DestinationNWInterface 3 -DestinationRGName rg01

Get-SRNetworkConstraint

Update-SmbMultichannelConnection

```

Para configurar restricciones de red en un clúster extendido:

```
Set-SRNetworkConstraint -SourceComputerName sr-cluster01 -SourceRGName group1 -SourceNWInterface "Cluster Network 1","Cluster Network 2" -DestinationComputerName sr-cluster02 -DestinationRGName group2 -DestinationNWInterface "Cluster Network 1","Cluster Network 2"
```

## <a name="can-i-configure-one-to-many-replication-or-transitive-a-to-b-to-c-replication"></a><a name="FAQ4"></a> ¿Puedo configurar la replicación de uno a varios o la replicación transitiva (a a B)?
No, réplica de almacenamiento solo admite una replicación de un nodo de clúster de servidor, clúster o extendido. Esto puede cambiar en una versión posterior. Puede configurar la replicación entre varios servidores de un par de volumen específico, en cualquier dirección. Por ejemplo, el servidor 1 puede replicar su volumen D en el servidor 2, y su volumen E desde el servidor 3.

## <a name="can-i-grow-or-shrink-replicated-volumes-replicated-by-storage-replica"></a><a name="FAQ5"></a> ¿Puedo aumentar o reducir los volúmenes replicados replicados por réplica de almacenamiento?
Puede aumentar (ampliar) volúmenes, pero no reducirlos. De forma predeterminada, réplica de almacenamiento impide que los administradores extiendan los volúmenes replicados. Use la `Set-SRGroup -AllowVolumeResize $TRUE` opción en el grupo de origen, antes de cambiar el tamaño. Por ejemplo:

1. Usar en el equipo de origen: `Set-SRGroup -Name YourRG -AllowVolumeResize $TRUE`
2. Amplíe el volumen con la técnica que prefiera.
3. Usar en el equipo de origen: `Set-SRGroup -Name YourRG -AllowVolumeResize $FALSE`

## <a name="can-i-bring-a-destination-volume-online-for-read-only-access"></a><a name="FAQ6"></a>¿Se puede conectar un volumen de destino para el acceso de solo lectura?
No en Windows Server 2016. Réplica de almacenamiento desmonta el volumen de destino cuando comienza la replicación.

Sin embargo, en Windows Server 2019 y Windows Server Semi-Annual canal a partir de la versión 1709, ahora es posible montar el almacenamiento de destino. esta característica se denomina "conmutación por error de prueba". Para ello, debe tener un volumen con formato NTFS o ReFS sin usar que no se esté replicando actualmente en el destino. A continuación, puede montar temporalmente una instantánea del almacenamiento replicado con fines de prueba o de copia de seguridad.

Por ejemplo, para crear una conmutación por error de prueba en la que se va a replicar un volumen "D:" en el grupo de replicación "RG2" en el servidor de destino "SRV2" y tener una unidad "T:" en SRV2 que no se está replicando:

 `Mount-SRDestination -Name RG2 -Computername SRV2 -TemporaryPath T:\`

El volumen replicado D: ahora es accesible en SRV2. Puede leer y escribir en él normalmente, copiar archivos en él o ejecutar una copia de seguridad en línea que guarde en otro lugar para protegerlo, en la ruta D:. El volumen T: solo contendrá datos de registro.

Para quitar la instantánea de conmutación por error de prueba y descartar los cambios:

 `Dismount-SRDestination -Name RG2 -Computername SRV2`

Solo debe usar la característica de conmutación por error de prueba para las operaciones temporales a corto plazo. No está diseñado para el uso a largo plazo. Cuando se usa, la replicación continúa en el volumen de destino real.

## <a name="can-i-configure-scale-out-file-server-sofs-in-a-stretch-cluster"></a><a name="FAQ7"></a> ¿Puedo configurar el servidor de archivos de escalabilidad horizontal (SOFS) en un clúster extendido?
Aunque técnicamente posible, esta no es una configuración recomendada debido a la falta de reconocimiento de sitios en los nodos de proceso que se pone en contacto con el SOFS. Si usa redes de campus-distancia, donde las latencias suelen ser submilisegundos, esta configuración normalmente funciona sin problemas.

Si configura la replicación de clúster a clúster, Réplica de almacenamiento es totalmente compatible con los Servidores de archivos de escalabilidad horizontal, incluido el uso de Espacios de almacenamiento directo, al replicar entre dos clústeres.

## <a name="is-csv-required-to-replicate-in-a-stretch-cluster-or-between-clusters"></a><a name="FAQ7.5"></a> ¿Se requiere CSV para replicar en un clúster extendido o entre clústeres?
No. Puede replicar con CSV o reserva de disco persistente (PDR) propiedad de un recurso de clúster, como un rol de servidor de archivos.

Si configura la replicación de clúster a clúster, Réplica de almacenamiento es totalmente compatible con los Servidores de archivos de escalabilidad horizontal, incluido el uso de Espacios de almacenamiento directo, al replicar entre dos clústeres.

## <a name="can-i-configure-storage-spaces-direct-in-a-stretch-cluster-with-storage-replica"></a><a name="FAQ8"></a>¿Se pueden configurar los Espacios de almacenamiento directo en un clúster extendido con Réplica de almacenamiento?
Esta no es una configuración admitida en Windows Server. Esto puede cambiar en una versión posterior. Si configura la replicación de clúster a clúster, Réplica de almacenamiento es totalmente compatible con los Servidores de archivos de escalabilidad horizontal y los servidores de Hyper-V, incluido el uso de Espacios de almacenamiento directo.

## <a name="how-do-i-configure-asynchronous-replication"></a><a name="FAQ9"></a>¿Cómo se configura la replicación asincrónica?

Especifique `New-SRPartnership -ReplicationMode` y proporcione el argumento **Asincrónico**. De forma predeterminada, toda la replicación en Réplica de almacenamiento es sincrónica. También puede cambiar el modo con `Set-SRPartnership -ReplicationMode`.

## <a name="how-do-i-prevent-automatic-failover-of-a-stretch-cluster"></a><a name="FAQ10"></a>¿Cómo se impide la conmutación automática por error de un clúster extendido?
Para evitar la conmutación automática por error, puede usar PowerShell para configurar `Get-ClusterNode -Name "NodeName").NodeWeight=0`. Esto quita el voto de cada nodo en el sitio de recuperación ante desastres. Puede usar `Start-ClusterNode -PreventQuorum` en los nodos del sitio primario y `Start-ClusterNode -ForceQuorum` en los nodos del sitio para desastres a fin de forzar la conmutación por error. No hay ninguna opción gráfica para evitar la conmutación por error automática, y esta opción no es recomendable.

## <a name="how-do-i-disable-virtual-machine-resiliency"></a><a name="FAQ11"></a>¿Cómo se deshabilita la resistencia de la máquina virtual?

Para evitar la ejecución de la nueva característica de resistencia de máquinas virtuales de Hyper-V y, por lo tanto, pausar máquinas virtuales en lugar de conmutarlas por error al sitio de recuperación ante desastres, ejecute `(Get-Cluster).ResiliencyDefaultPeriod=0`

## <a name="how-can-i-reduce-time-for-initial-synchronization"></a><a name="FAQ12"></a> ¿Cómo se puede reducir el tiempo de sincronización inicial?

Puede utilizar el almacenamiento de aprovisionamiento fino como una manera de acelerar los tiempos de sincronización inicial. Réplica de almacenamiento consulta el almacenamiento de aprovisionamiento fino y lo utiliza automáticamente, incluidos los Espacios de almacenamiento no agrupados en clúster, los discos dinámicos de Hyper-V y los LUN de SAN.

También puede usar volúmenes de datos inicializados para reducir el uso de ancho de banda y, a veces, el tiempo, asegurándose de que el volumen de destino tiene algún subconjunto de datos del principal mediante la opción Seedd en Administrador de clústeres de conmutación por error o `New-SRPartnership` . Si el volumen está principalmente vacío, mediante la sincronización de la inicialización puede reducir el uso de ancho de banda y el tiempo. Hay varias maneras de inicializar los datos, con distintos grados de eficacia:

1. Replicación anterior: mediante la replicación de la sincronización inicial normal localmente entre los nodos que contienen los discos y volúmenes, la eliminación de la replicación, el envío de los discos de destino en otro lugar y la adición de la replicación con la opción seedd. Este es el método más eficaz, ya que réplica de almacenamiento garantiza un reflejo de copia de bloques y lo único que se debe replicar son los bloques Delta.
2. Instantánea restaurada o copia de seguridad basada en instantánea restaurada: mediante la restauración de una instantánea basada en volumen en el volumen de destino, debe haber diferencias mínimas en el diseño de bloque. Este es el método más eficaz, ya que es probable que los bloques coincidan gracias a que las instantáneas de volumen son imágenes reflejadas.
3. Archivos copiados: al crear un nuevo volumen en el destino que nunca se ha usado antes y realizar una copia completa del árbol/MIR de los datos, es probable que se produzcan coincidencias de bloque. El uso del explorador de archivos de Windows o la copia de parte del árbol no creará muchas coincidencias de bloque. La copia manual de archivos es el método menos eficaz de propagación.

## <a name="can-i-delegate-users-to-administer-replication"></a><a name="FAQ13"></a> ¿Puedo delegar usuarios para administrar la replicación?

Puede usar el `Grant-SRDelegation` cmdlet. Esto le permite configurar usuarios específicos en escenarios de replicación de servidor a servidor, de clúster a clúster y de clúster extendido con el permiso de crear, modificar o quitar la replicación, sin formar parte del grupo de administradores global. Por ejemplo:

```
Grant-SRDelegation -UserName contso\tonywang
```

El cmdlet le recordará que el usuario debe cerrar sesión y volver a abrirla en el servidor que tiene previsto administrar para que el cambio surta efecto. Puede utilizar `Get-SRDelegation` y `Revoke-SRDelegation` para tener un mayor control.

## <a name="what-are-my-backup-and-restore-options-for-replicated-volumes"></a><a name="FAQ13"></a> ¿Cuáles son las opciones de copia de seguridad y restauración para volúmenes replicados?

Réplica de almacenamiento admite la copia de seguridad y la restauración del volumen de origen. También admite la creación y la restauración de instantáneas del volumen de origen. No puede hacer copias de seguridad o restaurar el volumen de destino mientras esté protegido por Réplica de almacenamiento, ya que no está montado ni es accesible. Si se produce un desastre por el que el volumen de origen se pierde, el uso de `Set-SRPartnership` para promover el volumen de destino anterior al momento actual será un origen de lectura/escritura que le permitirá hacer una copia de seguridad o restauración de ese volumen. También puede quitar la replicación con `Remove-SRPartnership` y `Remove-SRGroup` para volver a montar dicho volumen como de lectura/escritura.

Para crear instantáneas coherentes de aplicación periódicas, puede usar VSSADMIN. EXE en el servidor de origen para tomar la instantánea de los volúmenes de datos replicados. Por ejemplo, donde está replicando el volumen F: con Réplica de almacenamiento:

```
vssadmin create shadow /for=F:
```

A continuación, después de cambiar la dirección de la replicación, quitar la replicación o simplemente tomar la instantánea en el mismo volumen de origen, puede restaurar la instantánea a su punto en el tiempo. Por ejemplo, tome la instantánea usando F:

```
vssadmin list shadows
vssadmin revert shadow /shadow={shadown copy ID GUID listed previously}
```

También puede programar esta herramienta para que se ejecute periódicamente mediante una tarea programada. Para más información sobre el uso de VSS, revise [Vssadmin](../../administration/windows-commands/vssadmin.md). No es necesario ni sirve de nada realizar una copia de seguridad de los volúmenes de registros. Si intenta hacerlo, VSS lo ignorará.

El uso de Copias de seguridad de Windows Server, Microsoft Azure Backup, Microsoft DPM u otra instantánea, VSS, máquina virtual o tecnologías basadas en archivos es compatible con Réplica de almacenamiento siempre que trabajen en el nivel de volumen. Réplica de almacenamiento no admite la copia de seguridad y restauración basada en bloques.

## <a name="what-network-ports-does-storage-replica-require"></a><a name="FAQ15"></a>¿Qué puertos de red requiere la réplica de almacenamiento?

Réplica de almacenamiento se basa en SMB e WSMAN para su replicación y administración. Esto significa que se requieren los siguientes puertos:

- 445 (Protocolo de transporte de replicación SMB, protocolo de administración de RPC de clúster)
- 5445 (iWARP SMB: solo es necesario cuando se usa la red iWARP RDMA)
- 5985 (Protocolo de administración de WSManHTTP para WMI/CIM/PowerShell)

> ! Tenga en cuenta El cmdlet Test-SRTopology requiere ICMPv4/ICMPv6, pero no para la replicación o la administración.

## <a name="what-are-the-log-volume-best-practices"></a><a name="FAQ15.5"></a>¿Cuáles son los procedimientos recomendados para el volumen de registro?

El tamaño óptimo del registro varía considerablemente en función del entorno y de la carga de trabajo, y viene determinado por la cantidad de e/s de escritura que realiza la carga de trabajo.

1. Un registro mayor o menor no hace que sea más rápido o más lento
2. Un registro mayor o menor no tiene ningún espacio en el volumen de datos de 10 GB en lugar de un volumen de datos de 10 TB, por ejemplo

Un registro mayor simplemente recopila y conserva más e/s de escritura antes de que se ajusten. Esto permite una interrupción en el servicio entre el equipo de origen y el de destino (por ejemplo, una interrupción de la red o la desconexión del destino). Si el registro puede contener 10 horas de escritura y la red deja de funcionar durante 2 horas, cuando la red devuelve el origen, puede simplemente reproducir la diferencia de los cambios no sincronizados en el destino muy rápido y se vuelve a proteger muy rápidamente. Si el registro contiene 10 horas y la interrupción es de 2 días, ahora el origen tiene que reproducirse desde un registro diferente denominado mapa de bits, y probablemente será más lento volver a sincronizar. Una vez que se sincroniza, vuelve a usar el registro.

Réplica de almacenamiento se basa en el registro para todo el rendimiento de escritura. Rendimiento de registro crítico para el rendimiento de la replicación. Debe asegurarse de que el volumen de registro funciona mejor que el volumen de datos, ya que el registro serializará y realizará la secuenciación de todas las e/s de escritura. Siempre debe usar Flash Media como SSD en los volúmenes de registro. Nunca debe permitir que otras cargas de trabajo se ejecuten en el volumen de registro, de la misma manera que nunca permitiría que otras cargas de trabajo se ejecutaran en volúmenes de registro de base de datos SQL.

De nuevo: Microsoft recomienda encarecidamente que el almacenamiento de registros sea más rápido que el almacenamiento de datos y que los volúmenes de registro no se deben usar nunca para otras cargas de trabajo.

Puede obtener recomendaciones sobre el tamaño del registro mediante la ejecución de la herramienta Test-SRTopology. Como alternativa, puede usar los contadores de rendimiento en los servidores existentes para hacer una valoración del tamaño del registro. La fórmula es sencilla: supervise el rendimiento del disco de datos (bytes de escritura promedio/seg.) en la carga de trabajo y úselo para calcular la cantidad de tiempo que se tardará en rellenar el registro de diferentes tamaños. Por ejemplo, el rendimiento de los discos de datos de 50 MB/s provocará que el registro de 120 GB se ajuste a 120 GB/50 MB/2400 segundos o 40 minutos. Por lo tanto, la cantidad de tiempo que el servidor de destino podría ser inaccesible antes de que el registro ajustado sea de 40 minutos. Si el registro se ajusta pero el destino vuelve a ser accesible, el origen reproduciría bloques a través del registro de mapa de bits en lugar del registro principal. El tamaño del registro no tiene ningún efecto en el rendimiento.

SOLO se debe realizar una copia de seguridad del disco de datos del clúster de origen. NO se deben realizar copias de seguridad de los discos de registro de réplica de almacenamiento, ya que una copia de seguridad puede entrar en conflicto con las operaciones de réplica de almacenamiento.

## <a name="why-would-you-choose-a-stretch-cluster-versus-cluster-to-cluster-versus-server-to-server-topology"></a><a name="FAQ16"></a> ¿Por qué elegiría un clúster extendido en lugar de clúster a clúster frente a la topología de servidor a servidor?

La réplica de almacenamiento viene en tres configuraciones principales: clúster extendido, de clúster a clúster y de servidor a servidor. Cada una de ellas tiene diferentes ventajas.

La topología de clúster extendido es ideal para cargas de trabajo que requieren conmutación por error automática con orquestación, como clústeres de nube privada de Hyper-V y FCI de SQL Server. También tiene una interfaz gráfica integrada que usa Administrador de clústeres de conmutación por error. Emplea la arquitectura de almacenamiento compartido del clúster asimétrico clásico de espacios de almacenamiento, SAN, iSCSI y RAID a través de la reserva persistente. Puede ejecutarlo con tan solo 2 nodos.

La topología de clúster a clúster usa dos clústeres independientes y es ideal para los administradores que desean una conmutación por error manual, especialmente cuando el segundo sitio se aprovisiona para la recuperación ante desastres y no para el uso cotidiano. La orquestación es manual. A diferencia del clúster extendido, se pueden usar Espacios de almacenamiento directo en esta configuración (con advertencias; consulte las preguntas más frecuentes sobre réplica de almacenamiento y la documentación de clúster a clúster). Puede ejecutarlo con tan solo cuatro nodos.

La topología de servidor a servidor es ideal para los clientes que ejecutan hardware que no se pueden agrupar. Requiere la conmutación por error manual y la orquestación. Es ideal para implementaciones económicas entre sucursales y centros de datos centrales, especialmente cuando se usa la replicación asincrónica. Esta configuración a menudo puede reemplazar instancias de servidores de archivos protegidos por DFSR que se usan para escenarios de recuperación ante desastres de un solo maestro.

En todos los casos, las topologías admiten tanto la ejecución en el hardware físico como en las máquinas virtuales. En las máquinas virtuales, el hipervisor subyacente no requiere Hyper-V; puede ser VMware, KVM, Xen, etc.

Réplica de almacenamiento también tiene un modo de servidor a propio, donde se apunta la replicación a dos volúmenes diferentes en el mismo equipo.

## <a name="is-data-deduplication-supported-with-storage-replica"></a><a name="FAQ18"></a> ¿Se admite desduplicación de datos con réplica de almacenamiento?

Sí, se admite la Deduplcation de datos con réplica de almacenamiento. Habilite la desduplicación de datos en un volumen del servidor de origen y, durante la replicación, el servidor de destino recibirá una copia desduplicada del volumen.

Aunque debe *instalar* la desduplicación de datos en los servidores de origen y de destino (consulte [instalación y habilitación de la desduplicación de datos](../data-deduplication/install-enable.md)), es importante no *Habilitar* la desduplicación de datos en el servidor de destino. Réplica de almacenamiento permite la escritura solo en el servidor de origen. Dado que la desduplicación de datos realiza operaciones de escritura en el volumen, solo debe ejecutarse en el servidor de origen.

## <a name="can-i-replicate-between-windows-server-2019-and-windows-server-2016"></a><a name="FAQ19"></a> ¿Puedo replicar entre Windows Server 2019 y Windows Server 2016?

Desafortunadamente, no se admite la creación de una *nueva* asociación entre windows Server 2019 y windows Server 2016. Puede actualizar de forma segura un servidor o clúster que ejecute Windows Server 2016 a Windows Server 2019 y las asociaciones *existentes* seguirán funcionando.

Sin embargo, para obtener el mejor rendimiento de replicación de Windows Server 2019, todos los miembros de la Asociación deben ejecutar Windows Server 2019 y debe eliminar las asociaciones existentes y los grupos de replicación asociados y volver a crearlos con los datos inicializados (ya sea al crear la asociación en el centro de administración de Windows o con el cmdlet New-SRPartnership).

## <a name="how-do-i-report-an-issue-with-storage-replica-or-this-guide"></a><a name="FAQ17"></a> Cómo notificar un problema con la réplica de almacenamiento o esta guía?

Para obtener asistencia técnica con réplica de almacenamiento, puede publicar en los [foros de Microsoft](/answers/index.html). También puede enviar por correo electrónico srfeed@microsoft.com preguntas sobre réplica de almacenamiento o problemas con esta documentación. El [sitio de comentarios de Windows Server general](https://windowsserver.uservoice.com/forums/295047-general-feedback) es preferible para las solicitudes de cambio de diseño, ya que permite que sus colegas proporcionen soporte técnico y comentarios para sus ideas.

## <a name="can-storage-replica-be-configured-to-replicate-in-both-directions"></a><a name="FAQ18"></a> ¿Se puede configurar la réplica de almacenamiento para que se replique en ambas direcciones?

Réplica de almacenamiento es una tecnología de replicación unidireccional.  Solo se replicará desde el origen al destino en cada volumen.  Esta dirección se puede invertir en cualquier momento, pero sigue siendo solo en una dirección.  Sin embargo, eso no significa que no pueda tener un conjunto de volúmenes (origen y destino) replicarse en una dirección y un conjunto diferente de unidades (origen y destino) se replican en la dirección opuesta.  Por ejemplo, si desea tener configurada la replicación de servidor a servidor.  Server1 y server2 tienen cada una las letras de unidad L:, M:, N: y O: y desea replicar la unidad M: de server1 a server2, pero la unidad O: se replica de server2 a server1.  Esto puede hacerse siempre y cuando haya diferentes unidades de registro para cada uno de los grupos. (Por ejemplo:

- Servidor1 unidad de origen M: con la unidad de registro de origen L: replicando en la unidad de destino de servidor2 M: con la unidad de registro de destino L:
- Servidor2 unidad de origen O: con la unidad de registro de origen N: replicando en la unidad de destino Servidor1 O: con la unidad de registro de destino N:

## <a name="related-topics"></a>Temas relacionados
- [Información general sobre Réplica de almacenamiento](storage-replica-overview.md)
- [Replicación de clúster extendido con almacenamiento compartido](stretch-cluster-replication-using-shared-storage.md)
- [Replicación de almacenamiento de servidor a servidor](server-to-server-storage-replication.md)
- [Replicación de almacenamiento de clúster a clúster](cluster-to-cluster-storage-replication.md)
- [Réplica de almacenamiento: problemas conocidos](storage-replica-known-issues.md)

## <a name="see-also"></a>Consulte también
- [Información general de almacenamiento](../storage.yml)
- [Espacios de almacenamiento directo](../storage-spaces/storage-spaces-direct-overview.md)
