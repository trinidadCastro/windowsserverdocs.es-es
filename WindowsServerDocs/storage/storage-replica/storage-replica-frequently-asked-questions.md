---
title: "Preguntas frecuentes acerca de Réplica de almacenamiento"
ms.prod: windows-server-threshold
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 07/20/2017
ms.assetid: 12bc8e11-d63c-4aef-8129-f92324b2bf1b
ms.openlocfilehash: f29ca24a39e00f5142fe700e6abb09d1673acf7b
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="frequently-asked-questions-about-storage-replica"></a>Preguntas frecuentes acerca de Réplica de almacenamiento

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Este tema contiene respuestas a las preguntas frecuentes (P+F) acerca de Réplica de almacenamiento.

## <a name="FAQ1"></a> ¿La réplica de almacenamiento se admite en Nano Server?  
Sí.  

> [!NOTE]
> Debe utilizar el paquete **Almacenamiento** de Nano Server durante la instalación. Para más información acerca la implementación de Nano Server, consulte [Getting Started with Nano Server](https://technet.microsoft.com/library/mt126167.aspx) (Introducción a Nano Server).  

Instale Réplica de almacenamiento en Nano Server con la comunicación remota de PowerShell como sigue:  

1. Agregue el servidor Nano Server a la lista de confianza del cliente.  
   > [!NOTE]
   > Este paso solo es necesario si el equipo no forma parte de un bosque de Active Directory Domain Services o está en un bosque que no es de confianza. Agrega compatibilidad con NTLM para la comunicación remota de PSSession, que está deshabilitada de forma predeterminada por motivos de seguridad. Para más información, consulte [Consideraciones de seguridad de comunicación remota de PowerShell](https://msdn.microsoft.com/powershell/scriptiwinrmsecurity).  

   ```  
       Set-Item WSMan:\localhost\Client\TrustedHosts "<computer name of Nano Server>"  
   ```  
2.  Para instalar la característica de Réplica de almacenamiento, ejecute el siguiente cmdlet desde un equipo de administración:  

    ```  
    Install-windowsfeature -Name storage-replica,RSAT-Storage-Replica -ComputerName <nano server> -Restart -IncludeManagementTools  
    ```  

    El uso del cmdlet `Test-SRTopology` con Nano Server en Windows Server 2016 requiere la invocación de script remoto con CredSSP. A diferencia de otros cmdlets de Réplica de almacenamiento, `Test-SRTopology` requiere que se ejecute localmente en el servidor de origen.   
En el servidor de Nano Server (a través de una sesión remota de PSSession):  

    >[!NOTE]
    >CREDSSP se necesita para la compatibilidad de doble salto de Kerberos en el cmdlet `Test-SRTopology`, y no requiere otros cmdlets de Réplica de almacenamiento, que administran las credenciales de sistema distribuido automáticamente. No se recomienda usar CREDSSP en circunstancias normales. Una alternativa a CREDSSP, revise la siguiente entrada de blog de Microsoft: "PowerShell Remoting Kerberos Double Hop Solved Securely" (Conexión remota Kerberos de dos saltos de PowerShell que se soluciona de forma segura”, en https://blogs.technet.microsoft.com/ashleymcglone/2016/08/30/powershell-remoting-kerberos-double-hop-solved-securely/ 

        Enable-WSManCredSSP -role server       

    En el equipo de administración:  

         Enable-WSManCredSSP Client -DelegateComputer <remote server name>  

         $CustomCred = Get-Credential  

         Invoke-Command -ComputerName sr-srv01 -ScriptBlock { Test-SRTopology <commands> } -Authentication Credssp -Credential $CustomCred  

       A continuación, copie los resultados en el equipo de administración o comparta la ruta de acceso. Dado que Nano carece de las bibliotecas gráficas necesarias, puede utilizar Test-SRTopology para procesar los resultados y conseguir un archivo de informe con gráficos. Por ejemplo:  

        Test-SRTopology -GenerateReport -DataPath \\sr-srv05\c$\temp  


## <a name="FAQ2"></a> ¿Cómo se puede ver el progreso de una replicación durante la sincronización inicial?  
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

## <a name="FAQ3"></a>¿Se pueden especificar interfaces de red específicas para usarse para replicación?  

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

    Set-SRNetworkConstraint -SourceComputerName sr-srv01 -SourceRGName group1 -SourceNWInterface "Cluster Network 1","Cluster Network 2" -DestinationComputerName sr-srv03 -DestinationRGName group2 -DestinationNWInterface "Cluster Network 1","Cluster Network 2"  

## <a name="FAQ4"></a> ¿Se puede configurar la replicación de uno a varios o la replicación transitiva (de A a B a C)?  
No en Windows Server 2016. Esta versión solo admite la replicación de uno a uno de un servidor, un clúster o un nodo de clúster extendido. Esto puede cambiar en una versión posterior. Puede configurar la replicación entre varios servidores de un par de volumen específico, en cualquier dirección. Por ejemplo, el servidor 1 puede replicar su volumen D en el servidor 2, y su volumen E desde el servidor 3.

## <a name="FAQ5"></a> ¿Se pueden ampliar o reducir los volúmenes replicados por Réplica de almacenamiento?  
Puedes ampliar (expandir) volúmenes, pero no reducirlos. De manera predeterminada, Réplica de almacenamiento impide que los administradores amplíen volúmenes replicados; usa la opción `Set-SRGroup -AllowVolumeResize $TRUE` en el grupo de origen antes de cambiar el tamaño. Por ejemplo:

1. Usa con el equipo de origen: `Set-SRGroup -Name YourRG -AllowVolumeResize $TRUE`
2. Amplía el volumen con cualquier técnica que prefieras
3. Usa con el equipo de origen: `Set-SRGroup -Name YourRG -AllowVolumeResize $FALSE` 

## <a name="FAQ6"></a>¿Se puede conectar un volumen de destino para el acceso de solo lectura?  
No en Windows Server 2016 RTM, también conocida como versión "RS1". La réplica de almacenamiento desmonta el volumen de destino cuando comienza la replicación. 

Sin embargo, en Windows Server, versión 1709, la opción para montar el almacenamiento de destino es ahora posible: esta característica se denomina "Conmutación por error de prueba". Para ello, debes tener un volumen formateado NTFS o ReFS sin usar que no se replica actualmente en el destino. Después puedes montar una instantánea del almacenamiento replicado temporalmente para realizar pruebas o copias de seguridad. 

Por ejemplo, para crear una conmutación por error de prueba, donde se replica un volumen "D:" en el grupo de replicación "RG2" del servidor de destino "SRV2" y se dispone de una unidad "T:" en SRV2 que no se replica:

 `Mount-SRDestination -Name RG2 -Computername SRV2 -TemporaryPath T:\`
 
Ahora se puede tener acceso al volumen D: replicado en SRV2. Por lo general, puedes leer y escribir en él, copiar archivos fuera de él o ejecutar una copia de seguridad en línea que se guarda en otro lugar para tenerla a buen recaudo.

Para quitar la instantánea de conmutación por error de prueba y descartar sus cambios:

 `Dismount-SRDestination -Name RG2 -Computername SRV2`
 
Solo debes usar la característica de conmutación por error de prueba para realizar operaciones temporales a corto plazo. No está destinada a su uso a largo plazo. Cuando está en uso, la replicación continúa en el volumen de destino real. 

## <a name="FAQ7"></a>¿Puedo configurar el servidor de archivos de escalabilidad horizontal (SOFS) en un clúster extendido?  
Aunque es técnicamente posible, no es una configuración recomendada en Windows Server 2016 debido a la falta de reconocimiento de sitios en los nodos de proceso al ponerse en contacto con los SOFS. Si usa redes de campus-distancia, donde las latencias normalmente son inferiores a milisegundos, esta configuración suele funcionar sin problema.   

Si configura la replicación de clúster a clúster, Réplica de almacenamiento es totalmente compatible con los Servidores de archivos de escalabilidad horizontal, incluido el uso de Espacios de almacenamiento directo, al replicar entre dos clústeres.  

## <a name="FAQ8"></a>¿Se pueden configurar los Espacios de almacenamiento directo en un clúster extendido con Réplica de almacenamiento?  
No es una configuración compatible con Windows Server 2016.  Esto puede cambiar en una versión posterior. Si configura la replicación de clúster a clúster, Réplica de almacenamiento es totalmente compatible con los Servidores de archivos de escalabilidad horizontal y los servidores de Hyper-V, incluido el uso de Espacios de almacenamiento directo.  

## <a name="FAQ9"></a>¿Cómo se configura la replicación asincrónica?  

Especifique `New-SRPartnership -ReplicationMode` y proporcione el argumento **Asincrónico**. De forma predeterminada, toda la replicación en Réplica de almacenamiento es sincrónica. También puede cambiar el modo con `Set-SRPartnership -ReplicationMode`.  

## <a name="FAQ10"></a>¿Cómo se impide la conmutación automática por error de un clúster extendido?  
Para evitar la conmutación automática por error, puede usar PowerShell para configurar `Get-ClusterNode -Name "NodeName").NodeWeight=0`. Esto quita el voto de cada nodo en el sitio de recuperación ante desastres. Puede usar `Start-ClusterNode -PreventQuorum` en los nodos del sitio primario y `Start-ClusterNode -ForceQuorum` en los nodos del sitio para desastres a fin de forzar la conmutación por error. No hay ninguna opción gráfica para evitar la conmutación por error automática, y esta opción no es recomendable.  

## <a name="FAQ11"></a>¿Cómo se deshabilita la resistencia de la máquina virtual?  
Para evitar la ejecución de la nueva característica de resistencia de máquina virtual de Hyper-V y, por tanto, poner en pausa las máquinas virtuales en lugar de conmutarlas por error al sitio de recuperación ante desastres, ejecute `(Get-Cluster).ResiliencyDefaultPeriod=0`  

## <a name="FAQ12"></a> ¿Cómo se reduce la duración de la sincronización inicial?  

Puede utilizar el almacenamiento de aprovisionamiento fino como una manera de acelerar los tiempos de sincronización inicial. Réplica de almacenamiento consulta el almacenamiento de aprovisionamiento fino y lo utiliza automáticamente, incluidos los Espacios de almacenamiento no agrupados en clúster, los discos dinámicos de Hyper-V y los LUN de SAN.  

También puede usar los volúmenes de datos inicializados para reducir el uso de ancho de banda y, a veces, también el tiempo, asegurándose de que el volumen de destino tenga un subconjunto de datos del principal —a través de una copia de seguridad restaurada, una instantánea antigua, la replicación anterior, los archivos copiados, etc.— y, a continuación, usando la opción Inicializado del Administrador de clústeres de conmutación por error o `New-SRPartnership`. Si el volumen está principalmente vacío, mediante la sincronización de la inicialización puede reducir el uso de ancho de banda y el tiempo.

## <a name="FAQ13"></a> ¿Se puede delegar en los usuarios la administración de la replicación?  

Puede usar el cmdlet `Grant-SRDelegation` en Windows Server 2016. Esto le permite configurar usuarios específicos en escenarios de replicación de servidor a servidor, de clúster a clúster y de clúster extendido con el permiso de crear, modificar o quitar la replicación, sin formar parte del grupo de administradores global. Por ejemplo:  

    Grant-SRDelegation -UserName contso\tonywang  

El cmdlet le recordará que el usuario debe cerrar sesión y volver a abrirla en el servidor que tiene previsto administrar para que el cambio surta efecto. Puede utilizar `Get-SRDelegation` y `Revoke-SRDelegation` para tener un mayor control.  

## <a name="FAQ13"></a> ¿Cuáles son las opciones de copia de seguridad y restauración para volúmenes replicados?
Réplica de almacenamiento admite la copia de seguridad y la restauración del volumen de origen. También admite la creación y la restauración de instantáneas del volumen de origen. No puede hacer copias de seguridad o restaurar el volumen de destino mientras esté protegido por Réplica de almacenamiento, ya que no está montado ni es accesible. Si se produce un desastre por el que el volumen de origen se pierde, el uso de `Set-SRPartnership` para promover el volumen de destino anterior al momento actual será un origen de lectura/escritura que le permitirá hacer una copia de seguridad o restauración de ese volumen. También puede quitar la replicación con `Remove-SRPartnership` y `Remove-SRGroup` para volver a montar dicho volumen como de lectura/escritura.
Para crear instantáneas coherentes de aplicación periódicas, puede usar VSSADMIN. EXE en el servidor de origen para tomar la instantánea de los volúmenes de datos replicados. Por ejemplo, donde está replicando el volumen F: con Réplica de almacenamiento:

    vssadmin create shadow /for=F:
A continuación, después de cambiar la dirección de la replicación, quitar la replicación o simplemente tomar la instantánea en el mismo volumen de origen, puede restaurar la instantánea a su punto en el tiempo. Por ejemplo, tome la instantánea usando F:

    vssadmin list shadows
     vssadmin revert shadow /shadow={shadown copy ID GUID listed previously}
También puede programar esta herramienta para que se ejecute periódicamente mediante una tarea programada. Para más información sobre el uso de VSS, revise [Vssadmin](https://technet.microsoft.com/library/cc754968.aspx). No es necesario ni sirve de nada realizar una copia de seguridad de los volúmenes de registros. Si intenta hacerlo, VSS lo ignorará.
El uso de Copias de seguridad de Windows Server, Microsoft Azure Backup, Microsoft DPM u otra instantánea, VSS, máquina virtual o tecnologías basadas en archivos es compatible con Réplica de almacenamiento siempre que trabajen en el nivel de volumen. Réplica de almacenamiento no admite la copia de seguridad y restauración basada en bloques.

## <a name="FAQ14"></a> ¿Se puede configurar la replicación para restringir el uso de ancho de banda?
Sí, mediante el limitador de ancho de banda de SMB. Esto es una configuración global para todo el tráfico de Réplica de almacenamiento y, por tanto, afecta a toda la replicación desde este servidor. Normalmente, es necesaria solo con la configuración de sincronización inicial de Réplica de almacenamiento, donde se deben transferir todos los datos del volumen. Si se necesita después de la sincronización inicial, el ancho de banda de red es demasiado bajo para la carga de trabajo de E/S; reduzca el flujo de E/S o aumente el ancho de banda.

Solo debe utilizarse con la replicación asincrónica (nota: la sincronización inicial siempre es asincrónica, incluso si ha especificado la opción sincrónica).
También puede utilizar las directivas de calidad de servicio de red para ajustar el tráfico de Réplica de almacenamiento. El uso de la replicación de Réplica de almacenamiento inicializada con un alto nivel de coincidencia también reducirá considerablemente el uso global del ancho de banda en la sincronización inicial.


Para establecer el límite de ancho de banda, use:

    Set-SmbBandwidthLimit  -Category StorageReplication -BytesPerSecond x

Para ver el límite de ancho de banda, usa:

    Get-SmbBandwidthLimit -Category StorageReplication

Para quitar el límite de ancho de banda, usa:

    Remove-SmbBandwidthLimit -Category StorageReplication
    
## <a name="FAQ15"></a>¿Qué puertos de red requiere Réplica de almacenamiento?
Réplica de almacenamiento se basa en SMB y WSMAN para su replicación y administración. Esto significa que se necesitan los siguientes puertos:

   445 (SMB, protocolo de transporte de replicación), 5445 (iWARP SMB, solo es necesario cuando se usa la conexión en red iWARP RDMA), 5895 (WSManHTTP, protocolo de administración de WMI/CIM/PowerShell)

Nota: El cmdlet Test-SRTopology requiere ICMPv4 o ICMPv6, pero no para la replicación ni la administración.

## <a name="FAQ15"></a>¿Cuáles son los procedimientos recomendados para el volumen de registro?
El tamaño óptimo del registro varía considerablemente según el entorno y la carga de trabajo y se determina según la carga de trabajo de E/S de escritura que realices. 

1.  Un registro de mayor o menor tamaño no hace que vayas más rápido o más lento
2.  Un registro de mayor o menor tamaño no tiene ningún impacto en un volumen de datos de 10GB frente a un volumen de datos de 10TB, por ejemplo

Un registro de mayor tamaño simplemente recopila y conserva las E/S de escritura antes de enviarlas a su destino. Esto permite que una interrupción en el servicio entre el equipo de origen y de destino, como por ejemplo, una interrupción de la red o una desconexión del equipo de destino, se prolongue más. Si el registro puede conservar 10 horas de escrituras y la red se desconecta durante 2 horas, cuando la red se vuelve a conectar, el equipo de origen puede simplemente volver a reproducir la diferencia de los cambios no sincronizados al equipo de destino y volverás a estar protegido de forma muy rápida. Si el registro conserva 10 horas y la interrupción tiene una duración de 2 días, el equipo de origen ahora tiene que reproducir desde un registro distinto denominado mapa de bits y probablemente tardará más en sincronizarse. Una vez sincronizado, vuelve a usar el registro.

Hay contadores de rendimiento SR que te indicarán la velocidad a la que el registro está renovándose, lo que te permite realizar algunas valoraciones. El cmdlet Test-SRTopology también realiza esta función. Básicamente estás comprobando la E/S de escritura de una carga de trabajo existente para decidir el número de E/S que probablemente el registro llevará a cabo por minuto, hora o día.

La Réplica de almacenamiento depende del registro del rendimiento de escritura. El rendimiento del registro es fundamental para el rendimiento de la replicación. Debes asegurarte de que el volumen de registro funciona mejor que el volumen de datos, ya que el registro creará series y secuencias de todas las E/S de escritura. En los volúmenes de registro siempre debes usar medios flash, como SSD. Nunca permitas que en el volumen de registro se ejecuten otras cargas de trabajo, del mismo modo nunca permitirías que otras cargas de trabajo se ejecutaran en volúmenes de registro de bases de datos SQL. 

Recordatorio: Microsoft recomienda encarecidamente que el almacenamiento de registro sea más rápido que el almacenamiento de datos y que los volúmenes de registro nunca deben usarse para otras cargas de trabajo.

## <a name="FAQ16"></a>¿Por qué deberías elegir un clúster extendido frente a una topología de clúster a clúster frente a servidor a servidor?  
Réplica de almacenamiento incluye tres configuraciones principales: clúster extendido, clúster a clúster y servidor a servidor. Existen distintas ventajas para cada uno.

La topología de clúster extendido es ideal para cargas de trabajo que requieren conmutación por error automática con orquestación, como por ejemplo, clústeres de nube privada Hyper-V y FCI de SQLServer. También tiene una interfaz gráfica integrada con el Administrador de clústeres de conmutación por error. Usa la arquitectura clásica de almacenamiento compartido en clústeres asimétricos de Espacios de almacenamiento, SAN, iSCSI y RAID a través de la reserva persistente. Puedes ejecutar esta opción con tan solo 2 nodos.

La topología de clúster de clúster usa dos clústeres independientes y es ideal para administradores que busquen conmutación por error manual, especialmente cuando el segundo sitio se aprovisiona para la recuperación ante desastres y un uso no cotidiano. La orquestación es manual. A diferencia del clúster extendido, los Espacios de almacenamiento directo pueden usarse en esta configuración. Puedes ejecutar esta opción con tan solo 4 nodos. 

La topología de servidor a servidor es ideal para clientes con hardware que no puede agruparse en clústeres. Son necesarias una orquestación y una conmutación por error manual. Es ideal para implementaciones económicas entre sucursales y centros de datos centrales, especialmente al usar una replicación asincrónica. Esta configuración suele sustituir instancias de servidores de archivos protegidos mediante DFSR que se usan para escenarios de recuperación ante desastres de maestro único.

En todos los casos, las topologías admiten la ejecución en hardware físico y en máquinas virtuales. En el caso de máquinas virtuales, el hipervisor subyacente no requiere Hyper-V; puede ser VMware, KVM, Xen, etc.

Réplica de almacenamiento también tiene un modo de servidor al propio dispositivo en el que puedes asignar la replicación a dos volúmenes diferentes del mismo equipo.

## <a name="FAQ17"></a> ¿Cómo se informa de un problema de la Réplica de almacenamiento o de esta guía?  
Para obtener asistencia técnica para la Réplica de almacenamiento, puedes publicar en [los foros de Microsoft TechNet](https://social.technet.microsoft.com/Forums/windowsserver/en-US/home?forum=WinServerPreview). También puede enviar un correo electrónico a srfeed@microsoft.com para formular preguntas acerca de la Réplica de almacenamiento o plantear problemas relacionados con esta documentación. El sitio https://windowsserver.uservoice.com es el preferido para las solicitudes de cambio de diseño, ya que permite a los clientes proporcionar soporte y comentarios para sus ideas.



## <a name="related-topics"></a>Temas relacionados  
- [Información general sobre Réplica de almacenamiento](storage-replica-overview.md) 
- [Replicación de clúster extendido con almacenamiento compartido](stretch-cluster-replication-using-shared-storage.md)  
- [Replicación de almacenamiento de servidor a servidor](server-to-server-storage-replication.md)  
- [Replicación de almacenamiento de clúster a clúster](cluster-to-cluster-storage-replication.md)  
- [Réplica de almacenamiento: problemas conocidos](storage-replica-known-issues.md)  

## <a name="see-also"></a>Consulte también  
- [Introducción al almacenamiento](../storage.md)  
- [Espacios de almacenamiento directo en Windows Server 2016](../storage-spaces/storage-spaces-direct-overview.md)  
