---
title: Preguntas frecuentes acerca de Réplica de almacenamiento
ms.prod: windows-server-threshold
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 04/26/2019
ms.assetid: 12bc8e11-d63c-4aef-8129-f92324b2bf1b
ms.openlocfilehash: 89676ba821b99d44865bc6f45c34c05edb771d9d
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865258"
---
# <a name="frequently-asked-questions-about-storage-replica"></a>Preguntas frecuentes acerca de Réplica de almacenamiento

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual)

Este tema contiene respuestas a las preguntas frecuentes (P+F) acerca de Réplica de almacenamiento.

## <a name="FAQ1"></a>¿Se admite la réplica de almacenamiento en Azure?
Sí. Puede usar los siguientes escenarios con Azure:

1. Replicación de servidor a servidor dentro de Azure (de forma sincrónica o asincrónica entre máquinas virtuales de IaaS en uno o dos dominios de error del centro de recursos, o de forma asincrónica entre dos regiones independientes)
2. Replicación asincrónica de servidor a servidor entre Azure y el entorno local (mediante VPN o Azure ExpressRoute)
3. Replicación de clúster a clúster dentro de Azure (de forma sincrónica o asincrónica entre máquinas virtuales de IaaS en uno o dos dominios de error del centro de recursos, o de forma asincrónica entre dos regiones independientes)
4. Replicación asincrónica de clúster a clúster entre Azure y el entorno local (mediante VPN o Azure ExpressRoute)

Puede encontrar más notas sobre la agrupación en clústeres invitados en Azure en: [Implementación de clústeres invitados de máquinas virtuales de IaaS en Microsoft Azure](https://techcommunity.microsoft.com/t5/Failover-Clustering/Deploying-IaaS-VM-Guest-Clusters-in-Microsoft-Azure/ba-p/372126).

Notas importantes:

1. Azure no admite clústeres invitados de VHDX compartidos, por lo que las máquinas virtuales de clúster de conmutación por error de Windows deben usar destinos iSCSI para la agrupación en clústeres o Espacios de almacenamiento directo de reserva de disco persistente de almacenamiento compartido clásico.
2. Hay Azure Resource Manager plantillas para la agrupación en clústeres de réplica de almacenamiento basado en Espacios de almacenamiento directo en [creación de clústeres de espacios de almacenamiento directo sofs con réplica de almacenamiento para la recuperación ante desastres en regiones de Azure](https://aka.ms/azure-storage-replica-cluster).  
3. La comunicación RPC de clúster a clúster en Azure (requerida por las API de clúster para conceder acceso entre el clúster) requiere configurar el acceso a la red para el CNO. Debe permitir el puerto TCP 135 y el intervalo dinámico por encima del puerto TCP 49152. Referencia [sobre la creación de clústeres de conmutación por error de Windows Server en una máquina virtual de IaaS de Azure: red y creación](https://blogs.technet.microsoft.com/askcore/2015/06/24/building-windows-server-failover-cluster-on-azure-iaas-vm-part-2-network-and-creation/).  
4. Es posible usar clústeres invitados de dos nodos, donde cada nodo usa iSCSI de bucle invertido para un clúster asimétrico replicado por réplica de almacenamiento. Pero esto probablemente tendrá un rendimiento muy deficiente y debe usarse solo para cargas de trabajo o pruebas muy limitadas.  

## <a name="FAQ2"></a>Cómo ver el progreso de la replicación durante la sincronización inicial  
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

## <a name="FAQ3"></a>¿Se pueden especificar interfaces de red específicas para su uso en la replicación?  

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

    Set-SRNetworkConstraint -SourceComputerName sr-cluster01 -SourceRGName group1 -SourceNWInterface "Cluster Network 1","Cluster Network 2" -DestinationComputerName sr-cluster02 -DestinationRGName group2 -DestinationNWInterface "Cluster Network 1","Cluster Network 2"

## <a name="FAQ4"></a>¿Puedo configurar la replicación de uno a varios o la replicación transitiva (a a B)?  
No, réplica de almacenamiento solo admite una replicación de un nodo de clúster de servidor, clúster o extendido. Esto puede cambiar en una versión posterior. Puede configurar la replicación entre varios servidores de un par de volumen específico, en cualquier dirección. Por ejemplo, el servidor 1 puede replicar su volumen D en el servidor 2, y su volumen E desde el servidor 3.

## <a name="FAQ5"></a>¿Puedo aumentar o reducir los volúmenes replicados replicados por réplica de almacenamiento?  
Puedes ampliar (expandir) volúmenes, pero no reducirlos. De manera predeterminada, Réplica de almacenamiento impide que los administradores amplíen volúmenes replicados; usa la opción `Set-SRGroup -AllowVolumeResize $TRUE` en el grupo de origen antes de cambiar el tamaño. Por ejemplo:

1. Usar en el equipo de origen:`Set-SRGroup -Name YourRG -AllowVolumeResize $TRUE`
2. Amplía el volumen con cualquier técnica que prefieras
3. Usar en el equipo de origen:`Set-SRGroup -Name YourRG -AllowVolumeResize $FALSE` 

## <a name="FAQ6"></a>¿Se puede poner en línea un volumen de destino para el acceso de solo lectura?  
No en Windows Server 2016. La réplica de almacenamiento desmonta el volumen de destino cuando comienza la replicación. 

Sin embargo, en el canal semianual de Windows Server 2019 y Windows Server a partir de la versión 1709, la opción para montar el almacenamiento de destino ahora es posible: esta característica se denomina "conmutación por error de prueba". Para ello, debes tener un volumen formateado NTFS o ReFS sin usar que no se replica actualmente en el destino. Después puedes montar una instantánea del almacenamiento replicado temporalmente para realizar pruebas o copias de seguridad. 

Por ejemplo, para crear una conmutación por error de prueba, donde se replica un volumen "D:" en el grupo de replicación "RG2" del servidor de destino "SRV2" y se dispone de una unidad "T:" en SRV2 que no se replica:

 `Mount-SRDestination -Name RG2 -Computername SRV2 -TemporaryPath T:\`
 
Ahora se puede tener acceso al volumen D: replicado en SRV2. Puede leer y escribir en él normalmente, copiar archivos en él o ejecutar una copia de seguridad en línea que guarde en otro lugar para protegerlo, en la ruta D:. El volumen T: solo contendrá datos de registro.

Para quitar la instantánea de conmutación por error de prueba y descartar sus cambios:

 `Dismount-SRDestination -Name RG2 -Computername SRV2`
 
Solo debes usar la característica de conmutación por error de prueba para realizar operaciones temporales a corto plazo. No está destinada a su uso a largo plazo. Cuando está en uso, la replicación continúa en el volumen de destino real. 

## <a name="FAQ7"></a>¿Puedo configurar el servidor de archivos de escalabilidad horizontal (SOFS) en un clúster extendido?  
Aunque técnicamente posible, esta no es una configuración recomendada debido a la falta de reconocimiento de sitios en los nodos de proceso que se pone en contacto con el SOFS. Si usa redes de campus-distancia, donde las latencias suelen ser submilisegundos, esta configuración normalmente funciona sin problemas.   

Si configura la replicación de clúster a clúster, Réplica de almacenamiento es totalmente compatible con los Servidores de archivos de escalabilidad horizontal, incluido el uso de Espacios de almacenamiento directo, al replicar entre dos clústeres.  

## <a name="FAQ7.5"></a>¿Se requiere CSV para replicar en un clúster extendido o entre clústeres?  
No. Puede replicar con CSV o reserva de disco persistente (PDR) propiedad de un recurso de clúster, como un rol de servidor de archivos. 

Si configura la replicación de clúster a clúster, Réplica de almacenamiento es totalmente compatible con los Servidores de archivos de escalabilidad horizontal, incluido el uso de Espacios de almacenamiento directo, al replicar entre dos clústeres.  

## <a name="FAQ8"></a>¿Puedo configurar Espacios de almacenamiento directo en un clúster extendido con réplica de almacenamiento?  
Esta no es una configuración admitida en Windows Server. Esto puede cambiar en una versión posterior. Si configura la replicación de clúster a clúster, Réplica de almacenamiento es totalmente compatible con los Servidores de archivos de escalabilidad horizontal y los servidores de Hyper-V, incluido el uso de Espacios de almacenamiento directo.  

## <a name="FAQ9"></a>Cómo configurar la replicación asincrónica  

Especifique `New-SRPartnership -ReplicationMode` y proporcione el argumento **Asincrónico**. De forma predeterminada, toda la replicación en Réplica de almacenamiento es sincrónica. También puede cambiar el modo con `Set-SRPartnership -ReplicationMode`.  

## <a name="FAQ10"></a>Cómo evitar la conmutación automática por error de un clúster extendido  
Para evitar la conmutación automática por error, puede usar PowerShell para configurar `Get-ClusterNode -Name "NodeName").NodeWeight=0`. Esto quita el voto de cada nodo en el sitio de recuperación ante desastres. Puede usar `Start-ClusterNode -PreventQuorum` en los nodos del sitio primario y `Start-ClusterNode -ForceQuorum` en los nodos del sitio para desastres a fin de forzar la conmutación por error. No hay ninguna opción gráfica para evitar la conmutación por error automática, y esta opción no es recomendable.  

## <a name="FAQ11"></a>¿Cómo deshabilitar la resistencia de la máquina virtual?
Para evitar la ejecución de la nueva característica de resistencia de máquinas virtuales de Hyper-V y, por lo tanto, pausar máquinas virtuales en lugar de conmutarlas por error al sitio de recuperación ante desastres, ejecute`(Get-Cluster).ResiliencyDefaultPeriod=0`  

## <a name="FAQ12"></a>¿Cómo se puede reducir el tiempo de sincronización inicial?

Puede utilizar el almacenamiento de aprovisionamiento fino como una manera de acelerar los tiempos de sincronización inicial. Réplica de almacenamiento consulta el almacenamiento de aprovisionamiento fino y lo utiliza automáticamente, incluidos los Espacios de almacenamiento no agrupados en clúster, los discos dinámicos de Hyper-V y los LUN de SAN.  

También puede usar volúmenes de datos inicializados para reducir el uso de ancho de banda y, a veces, el tiempo, asegurándose de que el volumen de destino tiene algún subconjunto de datos del `New-SRPartnership`principal mediante la opción seedd en Administrador de clústeres de conmutación por error o. Si el volumen está principalmente vacío, mediante la sincronización de la inicialización puede reducir el uso de ancho de banda y el tiempo. Hay varias maneras de inicializar los datos, con distintos grados de eficacia:

1. Replicación anterior: mediante la replicación de la sincronización inicial normal localmente entre los nodos que contienen los discos y volúmenes, la eliminación de la replicación, el envío de los discos de destino en otro lugar y la adición de la replicación con la opción seedd. Este es el método más eficaz, ya que réplica de almacenamiento garantiza un reflejo de copia de bloques y lo único que se debe replicar son los bloques Delta.
2. Instantánea restaurada o copia de seguridad basada en instantánea restaurada: mediante la restauración de una instantánea basada en volumen en el volumen de destino, debe haber diferencias mínimas en el diseño de bloque. Este es el método más eficaz, ya que es probable que los bloques coincidan gracias a que las instantáneas de volumen son imágenes reflejadas.
3. Archivos copiados: al crear un nuevo volumen en el destino que nunca se ha usado antes y realizar una copia completa del árbol/MIR de los datos, es probable que se produzcan coincidencias de bloque. El uso del explorador de archivos de Windows o la copia de parte del árbol no creará muchas coincidencias de bloque. La copia manual de archivos es el método menos eficaz de propagación.

## <a name="FAQ13"></a>¿Puedo delegar usuarios para administrar la replicación?  

Puede usar el `Grant-SRDelegation` cmdlet. Esto le permite configurar usuarios específicos en escenarios de replicación de servidor a servidor, de clúster a clúster y de clúster extendido con el permiso de crear, modificar o quitar la replicación, sin formar parte del grupo de administradores global. Por ejemplo:  

    Grant-SRDelegation -UserName contso\tonywang  

El cmdlet le recordará que el usuario debe cerrar sesión y volver a abrirla en el servidor que tiene previsto administrar para que el cambio surta efecto. Puede utilizar `Get-SRDelegation` y `Revoke-SRDelegation` para tener un mayor control.  

## <a name="FAQ13"></a>¿Cuáles son las opciones de copia de seguridad y restauración para volúmenes replicados?
Réplica de almacenamiento admite la copia de seguridad y la restauración del volumen de origen. También admite la creación y la restauración de instantáneas del volumen de origen. No puede hacer copias de seguridad o restaurar el volumen de destino mientras esté protegido por Réplica de almacenamiento, ya que no está montado ni es accesible. Si se produce un desastre por el que el volumen de origen se pierde, el uso de `Set-SRPartnership` para promover el volumen de destino anterior al momento actual será un origen de lectura/escritura que le permitirá hacer una copia de seguridad o restauración de ese volumen. También puede quitar la replicación con `Remove-SRPartnership` y `Remove-SRGroup` para volver a montar dicho volumen como de lectura/escritura.
Para crear instantáneas coherentes de aplicación periódicas, puede usar VSSADMIN. EXE en el servidor de origen para tomar la instantánea de los volúmenes de datos replicados. Por ejemplo, donde está replicando el volumen F: con Réplica de almacenamiento:

    vssadmin create shadow /for=F:
A continuación, después de cambiar la dirección de la replicación, quitar la replicación o simplemente tomar la instantánea en el mismo volumen de origen, puede restaurar la instantánea a su punto en el tiempo. Por ejemplo, tome la instantánea usando F:

    vssadmin list shadows
     vssadmin revert shadow /shadow={shadown copy ID GUID listed previously}
También puede programar esta herramienta para que se ejecute periódicamente mediante una tarea programada. Para más información sobre el uso de VSS, revise [Vssadmin](../../administration/windows-commands/vssadmin.md). No es necesario ni sirve de nada realizar una copia de seguridad de los volúmenes de registros. Si intenta hacerlo, VSS lo ignorará.
El uso de Copias de seguridad de Windows Server, Microsoft Azure Backup, Microsoft DPM u otra instantánea, VSS, máquina virtual o tecnologías basadas en archivos es compatible con Réplica de almacenamiento siempre que trabajen en el nivel de volumen. Réplica de almacenamiento no admite la copia de seguridad y restauración basada en bloques.

## <a name="FAQ14"></a>¿Se puede configurar la replicación para restringir el uso de ancho de banda?
Sí, mediante el limitador de ancho de banda de SMB. Esto es una configuración global para todo el tráfico de Réplica de almacenamiento y, por tanto, afecta a toda la replicación desde este servidor. Normalmente, es necesaria solo con la configuración de sincronización inicial de Réplica de almacenamiento, donde se deben transferir todos los datos del volumen. Si se necesita después de la sincronización inicial, el ancho de banda de red es demasiado bajo para la carga de trabajo de E/S; reduzca el flujo de E/S o aumente el ancho de banda.

Solo debe utilizarse con la replicación asincrónica (nota: la sincronización inicial siempre es asincrónica, incluso si ha especificado la opción sincrónica).
También puede utilizar las directivas de calidad de servicio de red para ajustar el tráfico de Réplica de almacenamiento. El uso de la replicación de Réplica de almacenamiento inicializada con un alto nivel de coincidencia también reducirá considerablemente el uso global del ancho de banda en la sincronización inicial.


Para establecer el límite de ancho de banda, use:

    Set-SmbBandwidthLimit  -Category StorageReplication -BytesPerSecond x

Para ver el límite de ancho de banda, use:

    Get-SmbBandwidthLimit -Category StorageReplication

Para quitar el límite de ancho de banda, use:

    Remove-SmbBandwidthLimit -Category StorageReplication
    
## <a name="FAQ15"></a>¿Qué puertos de red requiere la réplica de almacenamiento?
Réplica de almacenamiento se basa en SMB y WSMAN para su replicación y administración. Esto significa que se necesitan los siguientes puertos:

 445 (Protocolo de transporte de replicación SMB, protocolo de administración de RPC de clúster) 5445 (iWARP SMB-solo es necesario cuando se usa la red iWARP RDMA) 5985 (Protocolo de administración de WSManHTTP para WMI/CIM/PowerShell)

Nota: El cmdlet test-SRTopology requiere ICMPv4/ICMPv6, pero no para la replicación o la administración.

## <a name="FAQ15.5"></a>¿Cuáles son los procedimientos recomendados para el volumen de registro?
El tamaño óptimo del registro varía considerablemente en función del entorno y de la carga de trabajo, y viene determinado por la cantidad de e/s de escritura que realiza la carga de trabajo. 

1.  Un registro mayor o menor no hace que sea más rápido o más lento
2.  Un registro mayor o menor no tiene ningún espacio en el volumen de datos de 10 GB en lugar de un volumen de datos de 10 TB, por ejemplo

Un registro de mayor tamaño simplemente recopila y conserva las E/S de escritura antes de enviarlas a su destino. Esto permite que una interrupción en el servicio entre el equipo de origen y de destino, como por ejemplo, una interrupción de la red o una desconexión del equipo de destino, se prolongue más. Si el registro puede conservar 10 horas de escrituras y la red se desconecta durante 2 horas, cuando la red se vuelve a conectar, el equipo de origen puede simplemente volver a reproducir la diferencia de los cambios no sincronizados al equipo de destino y volverás a estar protegido de forma muy rápida. Si el registro conserva 10 horas y la interrupción tiene una duración de 2 días, el equipo de origen ahora tiene que reproducir desde un registro distinto denominado mapa de bits y probablemente tardará más en sincronizarse. Una vez sincronizado, vuelve a usar el registro.

La Réplica de almacenamiento depende del registro del rendimiento de escritura. El rendimiento del registro es fundamental para el rendimiento de la replicación. Debes asegurarte de que el volumen de registro funciona mejor que el volumen de datos, ya que el registro creará series y secuencias de todas las E/S de escritura. En los volúmenes de registro siempre debes usar medios flash, como SSD. Nunca permitas que en el volumen de registro se ejecuten otras cargas de trabajo, del mismo modo nunca permitirías que otras cargas de trabajo se ejecutaran en volúmenes de registro de bases de datos SQL. 

Nuevo Microsoft recomienda encarecidamente que el almacenamiento de registros sea más rápido que el almacenamiento de datos y que los volúmenes de registro no se deben usar nunca para otras cargas de trabajo.

Puede obtener recomendaciones sobre el tamaño del registro mediante la ejecución de la herramienta test-SRTopology. Como alternativa, puede usar los contadores de rendimiento en los servidores existentes para hacer una valoración del tamaño del registro. La fórmula es sencilla: supervise el rendimiento del disco de datos (bytes de escritura promedio/seg.) en la carga de trabajo y úselo para calcular la cantidad de tiempo que se tardará en rellenar el registro de diferentes tamaños. Por ejemplo, el rendimiento de los discos de datos de 50 MB/s provocará que el registro de 120 GB se ajuste a 120 GB/50 MB/2400 segundos o 40 minutos. Por lo tanto, la cantidad de tiempo que el servidor de destino podría ser inaccesible antes de que el registro ajustado sea de 40 minutos. Si el registro se ajusta pero el destino vuelve a ser accesible, el origen reproduciría bloques a través del registro de mapa de bits en lugar del registro principal. El tamaño del registro no tiene ningún efecto en el rendimiento.

SOLO se debe realizar una copia de seguridad del disco de datos del clúster de origen. NO se deben realizar copias de seguridad de los discos de registro de réplica de almacenamiento, ya que una copia de seguridad puede entrar en conflicto con las operaciones de réplica de almacenamiento.

## <a name="FAQ16"></a>¿Por qué elegiría un clúster extendido en lugar de clúster a clúster frente a la topología de servidor a servidor?  
La réplica de almacenamiento viene en tres configuraciones principales: clúster extendido, de clúster a clúster y de servidor a servidor. Existen distintas ventajas para cada uno.

La topología de clúster extendido es ideal para cargas de trabajo que requieren conmutación por error automática con orquestación, como por ejemplo, clústeres de nube privada Hyper-V y FCI de SQL Server. También tiene una interfaz gráfica integrada con el Administrador de clústeres de conmutación por error. Usa la arquitectura clásica de almacenamiento compartido en clústeres asimétricos de Espacios de almacenamiento, SAN, iSCSI y RAID a través de la reserva persistente. Puedes ejecutar esta opción con tan solo 2 nodos.

La topología de clúster de clúster usa dos clústeres independientes y es ideal para administradores que busquen conmutación por error manual, especialmente cuando el segundo sitio se aprovisiona para la recuperación ante desastres y un uso no cotidiano. La orquestación es manual. A diferencia del clúster extendido, se pueden usar Espacios de almacenamiento directo en esta configuración (con advertencias; consulte las preguntas más frecuentes sobre réplica de almacenamiento y la documentación de clúster a clúster). Puedes ejecutar esta opción con tan solo 4 nodos. 

La topología de servidor a servidor es ideal para clientes con hardware que no puede agruparse en clústeres. Son necesarias una orquestación y una conmutación por error manual. Es ideal para implementaciones económicas entre sucursales y centros de datos centrales, especialmente cuando se usa la replicación asincrónica. Esta configuración suele sustituir instancias de servidores de archivos protegidos mediante DFSR que se usan para escenarios de recuperación ante desastres de maestro único.

En todos los casos, las topologías admiten la ejecución en hardware físico y en máquinas virtuales. En el caso de máquinas virtuales, el hipervisor subyacente no requiere Hyper-V; puede ser VMware, KVM, Xen, etc.

Réplica de almacenamiento también tiene un modo de servidor al propio dispositivo en el que puedes asignar la replicación a dos volúmenes diferentes del mismo equipo.

## <a name="FAQ18"></a>¿Se admite desduplicación de datos con réplica de almacenamiento?

Sí, se admite la Deduplcation de datos con réplica de almacenamiento. Habilite la desduplicación de datos en un volumen del servidor de origen y, durante la replicación, el servidor de destino recibirá una copia desduplicada del volumen.

Aunque debe *instalar* la desduplicación de datos en los servidores de origen y de destino (consulte [instalación y habilitación de la desduplicación de datos](../data-deduplication/install-enable.md)), es importante no *Habilitar* la desduplicación de datos en el servidor de destino. Réplica de almacenamiento permite la escritura solo en el servidor de origen. Dado que la desduplicación de datos realiza operaciones de escritura en el volumen, solo debe ejecutarse en el servidor de origen. 

## <a name="FAQ19"></a>¿Puedo replicar entre Windows Server 2019 y Windows Server 2016?

Desafortunadamente, no se admite la creación de una *nueva* asociación entre windows Server 2019 y windows Server 2016. Puede actualizar de forma segura un servidor o clúster que ejecute Windows Server 2016 a Windows Server 2019 y las asociaciones *existentes* seguirán funcionando.

Sin embargo, para obtener el mejor rendimiento de la replicación de Windows Server 2019, todos los miembros de la Asociación deben ejecutar Windows Server 2019 y debe eliminar las asociaciones existentes y los grupos de replicación asociados y, a continuación, volver a crearlos con los datos inicializados ( al crear la asociación en el centro de administración de Windows o con el cmdlet New-SRPartnership).

## <a name="FAQ17"></a>Cómo notificar un problema con la réplica de almacenamiento o esta guía?  
Para obtener asistencia técnica con réplica de almacenamiento, puede publicar en [los foros de Microsoft TechNet](https://social.technet.microsoft.com/Forums/windowsserver/en-US/home?forum=WinServerPreview). También puede enviar un correo electrónico a srfeed@microsoft.com para formular preguntas acerca de la Réplica de almacenamiento o plantear problemas relacionados con esta documentación. El <https://windowsserver.uservoice.com> sitio es el preferido para las solicitudes de cambio de diseño, ya que permite que los clientes puedan proporcionar soporte técnico y comentarios para sus ideas.



## <a name="related-topics"></a>Temas relacionados  
- [Información general sobre réplica de almacenamiento](storage-replica-overview.md) 
- [Replicación de clúster extendido con almacenamiento compartido](stretch-cluster-replication-using-shared-storage.md)  
- [Replicación de almacenamiento de servidor a servidor](server-to-server-storage-replication.md)  
- [Replicación de almacenamiento de clúster a clúster](cluster-to-cluster-storage-replication.md)  
- [Réplica de almacenamiento: Problemas conocidos](storage-replica-known-issues.md)  

## <a name="see-also"></a>Vea también  
- [Información general de almacenamiento](../storage.md)  
- [Espacios de almacenamiento directo](../storage-spaces/storage-spaces-direct-overview.md)  
