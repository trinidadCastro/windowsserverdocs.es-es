---
title: Conjuntos de clústeres
ms.prod: windows-server
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: johnmarlin-msft
ms.author: johnmar
ms.date: 01/30/2019
description: En este artículo se describe el escenario de conjuntos de clústeres
ms.localizationpriority: medium
ms.openlocfilehash: 3c7ddef1831a82f7fc068ec4241bb1a72bd888bd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861048"
---
# <a name="cluster-sets"></a>Conjuntos de clústeres

> Se aplica a: Windows Server 2019

Conjuntos de clústeres es la nueva tecnología de escalado horizontal en la nube de la versión 2019 de Windows Server que aumenta el número de nodos de clúster en una nube de centro de datos definido por software (SDDC) por orden de magnitud. Un conjunto de clústeres es una agrupación débilmente acoplada de varios clústeres de conmutación por error: compute, Storage o hiperconvergido. La tecnología de conjuntos de clústeres habilita la fluidez de máquinas virtuales en los clústeres de miembros de un conjunto de clústeres y un espacio de nombres de almacenamiento unificado en el conjunto de compatibilidad con la fluidez de máquinas virtuales.

A la vez que se conservan las experiencias de administración de clústeres de conmutación por error existentes en los clústeres de miembros, una instancia de conjunto de clústeres ofrece también casos de uso clave para la administración del ciclo de vida Esta guía de evaluación del escenario de Windows Server 2019 proporciona la información básica necesaria junto con instrucciones paso a paso para evaluar la tecnología de conjuntos de clústeres con PowerShell.

## <a name="technology-introduction"></a>Introducción a la tecnología

La tecnología de conjuntos de clústeres se desarrolla para satisfacer las solicitudes de cliente específicas de las nubes de centros de información definidas por software (SDDC) a escala. La propuesta de valor de conjunto de clústeres se puede resumir como sigue:  

- Incremente significativamente el nivel de nube de SDDC compatible para ejecutar máquinas virtuales de alta disponibilidad mediante la combinación de varios clústeres más pequeños en un solo tejido grande, incluso manteniendo el límite de errores de software en un único clúster.
- Administre todo el ciclo de vida del clúster de conmutación por error, incluidos los clústeres de incorporación y retirada, sin afectar a la disponibilidad de las máquinas virtuales de los inquilinos, a través de la migración fluida de máquinas virtuales a través de este tejido grande
- Cambie fácilmente la relación de proceso a almacenamiento en su hiperconvergido
- Benefíciese de los [dominios de error y los conjuntos de disponibilidad de Azure](htttps://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) a través de los clústeres en la ubicación inicial de la máquina virtual y la migración posterior de máquinas virtuales
- Mezcle y combine diferentes generaciones de hardware de CPU en el mismo tejido de conjunto de clústeres, aunque mantenga dominios de error individuales homogéneos para obtener la máxima eficacia.  Tenga en cuenta que la recomendación del mismo hardware todavía está presente en cada clúster individual, así como en todo el conjunto de clústeres.

En una vista de alto nivel, esto es lo que pueden tener los conjuntos de clústeres.

![Vista de solución de conjuntos de clústeres](media/Cluster-sets-Overview/Cluster-sets-solution-View.png)

A continuación se proporciona un resumen rápido de cada uno de los elementos de la imagen anterior:

**Clúster de administración**

El clúster de administración de un conjunto de clústeres es un clúster de conmutación por error que hospeda el plano de administración de alta disponibilidad del conjunto de clústeres completo y el Servidor de archivos de escalabilidad horizontal de referencia de espacio de nombres de almacenamiento unificado (espacio de nombres de conjunto de clústeres) (SOFS). Un clúster de administración se desacopla lógicamente de los clústeres de miembros que ejecutan las cargas de trabajo de máquina virtual. Esto hace que el plano de administración del conjunto de clústeres sea resistente a cualquier error localizado en todo el clúster, por ejemplo, pérdida de energía de un clúster de miembros.   

**Clúster de miembros**

Un clúster miembro de un conjunto de clústeres es normalmente un clúster hiperconvergido tradicional que ejecuta cargas de trabajo de máquinas virtuales y Espacios de almacenamiento directo. Varios clústeres de miembros participan en una única implementación de conjunto de clústeres, formando el tejido de nube de SDDC más grande. Los clústeres de miembros se diferencian de un clúster de administración en dos aspectos clave: los clústeres miembros participan en construcciones de conjunto de disponibilidad y dominio de error, y los clústeres de miembros también tienen el tamaño para hospedar máquinas virtuales y cargas de trabajo de Espacios de almacenamiento directo. Las máquinas virtuales del conjunto de clústeres que se mueven a través de los límites de clúster en un conjunto de clústeres no deben estar hospedadas en el clúster de administración por este motivo.

**Referencia de espacio de nombres de conjunto de clúster SOFS**

Una referencia de espacio de nombres de conjunto de clústeres (espacio de nombres de conjunto de clústeres) SOFS es una Servidor de archivos de escalabilidad horizontal donde cada recurso compartido de SMB en el espacio de nombres del conjunto de clústeres SOFS es un recurso compartido de referencia, de tipo "SimpleReferral" recién introducido en Windows Server 2019. Esta referencia permite a los clientes de bloque de mensajes del servidor (SMB) tener acceso al recurso compartido de SMB de destino hospedado en el clúster de miembros SOFS. El SOFS de referencia de espacio de nombres del conjunto de clústeres es un mecanismo de referencia ligero y, como tal, no participa en la ruta de acceso de e/s. Las referencias SMB se almacenan en caché de forma perpetua en cada uno de los nodos de cliente y el espacio de nombres de los conjuntos de clústeres actualiza dinámicamente estas referencias según sea necesario.

**Clúster de conjunto maestro**

En un conjunto de clústeres, la comunicación entre los clústeres miembros se acopla de manera flexible y se coordina mediante un nuevo recurso de clúster denominado "cluster Set Master" (CS-Master). Al igual que cualquier otro recurso de clúster, CS-Master tiene alta disponibilidad y resistencia a errores de clúster de miembros individuales o a errores de nodo de clúster de administración. A través de un nuevo proveedor de WMI de conjunto de clústeres, CS-Master proporciona el punto de conexión de administración para todas las interacciones de administración del conjunto de clústeres.

**Trabajo de conjunto de clústeres**

En una implementación de conjunto de clústeres, CS-Master interactúa con un nuevo recurso de clúster en los clústeres miembros denominados "cluster Set Worker" (CS-worker). CS-Worker actúa como el único enlace en el clúster para orquestar las interacciones de clústeres locales según lo solicite el CS-Master. Algunos ejemplos de estas interacciones incluyen la selección de ubicación de máquinas virtuales y el inventario de recursos locales del clúster. Solo hay una instancia de CS-Worker para cada uno de los clústeres miembros de un conjunto de clústeres. 

**Dominio de error**

Un dominio de error es la agrupación de artefactos de software y hardware que el administrador determina que pueden fallar conjuntamente cuando se produce un error.  Aunque un administrador podría designar uno o varios clústeres como un dominio de error, cada nodo podría participar en un dominio de error en un conjunto de disponibilidad. Los conjuntos de clústeres por diseño dejan la decisión de determinación del límite del dominio de error al administrador, que se ha seguido con las consideraciones de la topología del centro de datos, por ejemplo, PDU y redes, que comparten los clústeres de miembros. 

**Conjunto de disponibilidad**

Un conjunto de disponibilidad ayuda al administrador a configurar la redundancia deseada de las cargas de trabajo en clúster en los dominios de error, al organizarlos en un conjunto de disponibilidad e implementar cargas de trabajo en ese conjunto de disponibilidad. Supongamos que, si va a implementar una aplicación de dos niveles, se recomienda configurar al menos dos máquinas virtuales en un conjunto de disponibilidad para cada nivel, lo que garantizará que, cuando un dominio de error en ese conjunto de disponibilidad deje de funcionar, la aplicación tendrá al menos una máquina virtual en cada nivel hospedado en un dominio de error diferente del mismo conjunto de disponibilidad.

## <a name="why-use-cluster-sets"></a>Por qué usar conjuntos de clústeres

Los conjuntos de clústeres ofrecen la ventaja de escala sin sacrificar la resistencia.  

Los conjuntos de clústeres permiten agrupar varios clústeres en clústeres para crear un tejido grande, mientras que cada clúster sigue siendo independiente de la resistencia.  Por ejemplo, tiene varios clústeres HCI de 4 nodos que ejecutan máquinas virtuales.  Cada clúster proporciona la resistencia necesaria para sí misma.  Si el almacenamiento o la memoria comienza a llenarse, el escalado vertical es el paso siguiente.  Con el escalado vertical, existen algunas opciones y consideraciones.

1. Agregue más almacenamiento al clúster actual.  Con Espacios de almacenamiento directo, esto puede ser complicado, ya que es posible que las mismas unidades de modelo o firmware no estén disponibles.  También es necesario tener en cuenta la consideración de los tiempos de recompilación.
2. Agregue más memoria.  ¿Qué ocurre si está sobrecargado en la memoria que pueden controlar los equipos?  ¿Qué ocurre si todas las ranuras de memoria disponibles están llenas?
3. Agregue nodos de proceso adicionales a las unidades en el clúster actual.  Esto nos lleva a la opción 1 que debe tenerse en cuenta.
4. Comprar un clúster completamente nuevo

Aquí es donde los conjuntos de clústeres ofrecen la ventaja de escalado.  Si agrego mis clústeres a un conjunto de clústeres, puedo aprovechar el almacenamiento o la memoria que puede haber disponible en otro clúster sin ninguna compra adicional.  Desde el punto de vista de la resistencia, agregar nodos adicionales a un Espacios de almacenamiento directo no va a proporcionar votos adicionales para el cuórum.  Como se mencionó [aquí](drive-symmetry-considerations.md), un clúster de espacios de almacenamiento directo puede sobrevivir a la pérdida de 2 nodos antes de la baja.  Si tiene un clúster de HCl de 4 nodos, los tres nodos dejarán de funcionar todo el clúster.  Si tiene un clúster de 8 nodos, los tres nodos dejarán de funcionar todo el clúster.  Con los conjuntos de clústeres que tienen dos clústeres HCI de 4 nodos en el conjunto, se desactivan 2 nodos de una HCl y 1 nodo en el otro HCl, ambos clústeres permanecen al mismo tiempo.  ¿Es mejor crear un clúster de Espacios de almacenamiento directo de 16 nodos de gran tamaño o dividirlo en cuatro clústeres de 4 nodos y usar conjuntos de clústeres?  Tener cuatro clústeres de 4 nodos con conjuntos de clústeres proporciona la misma escala, pero una mayor resistencia en el caso de que varios nodos de proceso se desactivan (de forma inesperada o por mantenimiento) y la producción se mantiene.

## <a name="considerations-for-deploying-cluster-sets"></a>Consideraciones para la implementación de conjuntos de clústeres

A la hora de considerar si el conjunto de clústeres es algo que necesita usar, tenga en cuenta estas preguntas:

- ¿Necesita ir más allá de los límites de escala de proceso y almacenamiento de HCI actuales?
- ¿Todos los procesos y el almacenamiento no son idénticos?
- ¿Migra en vivo máquinas virtuales entre clústeres?
- ¿Le gustaría los conjuntos de disponibilidad de equipos similares a Azure y los dominios de error en varios clústeres?
- ¿Necesita dedicar tiempo a examinar todos los clústeres para determinar dónde se deben colocar las nuevas máquinas virtuales?

Si la respuesta es sí, el conjunto de clústeres es lo que necesita.

Hay algunos otros elementos a tener en cuenta en los casos en los que un SDDC más grande puede cambiar las estrategias generales del centro de datos.  SQL Server es un buen ejemplo.  ¿El traslado de SQL Server máquinas virtuales entre clústeres requiere la ejecución de la licencia de SQL para ejecutarse en nodos adicionales?  

## <a name="scale-out-file-server-and-cluster-sets"></a>Conjuntos de clústeres y servidores de archivos de escalabilidad horizontal

En Windows Server 2019, hay un nuevo rol de servidor de archivos de escalabilidad horizontal denominado infraestructura Servidor de archivos de escalabilidad horizontal (SOFS). 

Las siguientes consideraciones se aplican a un rol de SOFS de infraestructura:

1.    Solo puede haber un rol de clúster de SOFS de infraestructura en un clúster de conmutación por error. El rol de SOFS de infraestructura se crea especificando el parámetro de modificador " **-Infrastructure**" para el cmdlet **Add-ClusterScaleOutFileServerRole** .  Por ejemplo:

        Add-ClusterScaleoutFileServerRole-name "my_infra_sofs_name"-Infrastructure

2.    Cada volumen CSV creado en la conmutación por error desencadena automáticamente la creación de un recurso compartido de SMB con un nombre generado automáticamente basado en el nombre del volumen CSV. Un administrador no puede crear ni modificar directamente recursos compartidos de SMB con un rol de SOFS, excepto a través de operaciones de creación y modificación de volumen CSV.

3.    En las configuraciones hiperconvergidas, una infraestructura SOFS permite a un cliente SMB (host de Hyper-V) comunicarse con disponibilidad continua garantizada (CA) en el servidor de infraestructura SOFS SMB. Esta CA de bucle invertido de SMB hiperconvergido se consigue a través de máquinas virtuales que acceden a sus archivos de disco virtual (VHDx), donde se reenvía la identidad de la máquina virtual propietaria entre el cliente y el servidor. Este reenvío de identidad permite la realización de archivos VHDx de la ACL como en las configuraciones de clúster hiperconvergidas estándar como antes.

Una vez creado un conjunto de clústeres, el espacio de nombres del conjunto de clústeres se basa en un SOFS de infraestructura en cada uno de los clústeres miembros y, además, en una infraestructura SOFS en el clúster de administración.

En el momento en que se agrega un clúster de miembros a un conjunto de clústeres, el administrador especifica el nombre de un SOFS de infraestructura en ese clúster, si ya existe uno. Si el SOFS de infraestructura no existe, esta operación crea un nuevo rol de SOFS de infraestructura en el nuevo clúster de miembros. Si ya existe un rol de SOFS de infraestructura en el clúster de miembros, la operación de agregar lo cambia implícitamente al nombre especificado según sea necesario. El conjunto de clústeres deja sin uso los servidores SMB de singleton existentes o los roles de SOFS que no son de infraestructura en los clústeres miembros. 

En el momento en que se crea el conjunto de clústeres, el administrador tiene la opción de usar un objeto de equipo de AD ya existente como la raíz del espacio de nombres en el clúster de administración. Las operaciones de creación de conjuntos de clústeres crean el rol de clúster de SOFS de infraestructura en el clúster de administración o cambia el nombre del rol de SOFS de infraestructura existente tal como se ha descrito anteriormente para los clústeres de miembros. La infraestructura SOFS en el clúster de administración se usa como la referencia de espacio de nombres del conjunto de clústeres (espacio de nombres del conjunto de clústeres) SOFS. Simplemente significa que cada recurso compartido de SMB en el espacio de nombres del conjunto de clústeres SOFS es un recurso compartido de referencia, de tipo "SimpleReferral", que se presentó recientemente en Windows Server 2019.  Esta referencia permite a los clientes SMB acceder al recurso compartido de SMB de destino hospedado en el clúster de miembros SOFS. El SOFS de referencia de espacio de nombres del conjunto de clústeres es un mecanismo de referencia ligero y, como tal, no participa en la ruta de acceso de e/s. Las referencias SMB se almacenan en caché de forma perpetua en cada uno de los nodos de cliente y el espacio de nombres de los conjuntos de clústeres actualiza dinámicamente estas referencias según sea necesario.

## <a name="creating-a-cluster-set"></a>Creación de un conjunto de clústeres

### <a name="prerequisites"></a>Requisitos previos

Al crear un conjunto de clústeres, se recomiendan los siguientes requisitos previos:

1. Configure un cliente de administración que ejecute Windows Server 2019.
2. Instale las herramientas de clúster de conmutación por error en este servidor de administración.
3. Crear miembros del clúster (al menos dos clústeres con al menos dos volúmenes compartidos de clúster en cada clúster)
4. Cree un clúster de administración (físico o invitado) que se enparte de los clústeres miembros.  Este enfoque garantiza que el plano de administración de los conjuntos de clústeres sigue estando disponible a pesar de posibles errores de clúster de miembros.

### <a name="steps"></a>Pasos

1. Cree un nuevo conjunto de clústeres a partir de tres clústeres, tal y como se define en los requisitos previos.  En el gráfico siguiente se proporciona un ejemplo de clústeres que se van a crear.  El nombre del conjunto de clústeres en este ejemplo será **CSMASTER**.

   | Nombre del clúster               | Nombre de SOFS de infraestructura que se usará más adelante | 
   |----------------------------|-------------------------------------------|
   | SET-CLUSTER                | SOFS-CLUSTERSET                           |
   | CLUSTER1                   | SOFS-CLUSTER1                             |
   | CLUSTER2                   | SOFS-CLUSTER2                             |

2. Una vez que se haya creado el clúster, use los siguientes comandos para crear el grupo de clústeres maestro.

        New-ClusterSet -Name CSMASTER -NamespaceRoot SOFS-CLUSTERSET -CimSession SET-CLUSTER

3. Para agregar un servidor de clúster al conjunto de clústeres, se utilizará lo siguiente.

        Add-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER1
        Add-ClusterSetMember -ClusterName CLUSTER2 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER2

   > [!NOTE]
   > Si usa un esquema de direcciones IP estáticas, debe incluir *-StaticAddress x* . x. x en el comando **New-ClusterSet** .

4. Una vez que haya creado el conjunto de clústeres fuera de los miembros del clúster, puede enumerar el conjunto de nodos y sus propiedades.  Para enumerar todos los clústeres miembros del conjunto de clústeres:

        Get-ClusterSetMember -CimSession CSMASTER

5. Para enumerar todos los clústeres miembros del conjunto de clústeres, incluidos los nodos de clúster de administración:

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterNode

6. Para enumerar todos los nodos de los clústeres miembros:

        Get-ClusterSetNode -CimSession CSMASTER

7. Para enumerar todos los grupos de recursos en el conjunto de clústeres:

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterGroup 

8. Para comprobar que el proceso de creación del conjunto de clústeres creó un recurso compartido de SMB (identificado como volume1 o cualquier carpeta CSV con la etiqueta nombre del servidor de archivos de infraestructura y la ruta de acceso como ambos) en el SOFS de infraestructura para el volumen CSV de cada miembro del clúster:

        Get-SmbShare -CimSession CSMASTER

8. Los conjuntos de clústeres tienen registros de depuración que se pueden recopilar para su revisión.  Tanto el conjunto de clústeres como los registros de depuración de clúster se pueden recopilar para todos los miembros y el clúster de administración.

        Get-ClusterSetLog -ClusterSetCimSession CSMASTER -IncludeClusterLog -IncludeManagementClusterLog -DestinationFolderPath <path>

9. Configure la [delegación restringida](https://techcommunity.microsoft.com/t5/virtualization/live-migration-via-constrained-delegation-with-kerberos-in/ba-p/382334) de Kerberos entre todos los miembros del conjunto de clústeres.

10. Configure el tipo de autenticación de migración en vivo de máquinas virtuales entre clústeres en Kerberos en cada nodo del conjunto de clústeres.

        foreach($h in $hosts){ Set-VMHost -VirtualMachineMigrationAuthenticationType Kerberos -ComputerName $h }

11. Agregue el clúster de administración al grupo de administradores locales en cada nodo del conjunto de clústeres.

        foreach($h in $hosts){ Invoke-Command -ComputerName $h -ScriptBlock {Net localgroup administrators /add <management_cluster_name>$} }

## <a name="creating-new-virtual-machines-and-adding-to-cluster-sets"></a>Crear nuevas máquinas virtuales y agregar a conjuntos de clústeres

Después de crear el conjunto de clústeres, el paso siguiente consiste en crear nuevas máquinas virtuales.  Normalmente, cuando es el momento de crear máquinas virtuales y agregarlas a un clúster, debe realizar algunas comprobaciones en los clústeres para ver cuál es el mejor en ejecutarse.  Estas comprobaciones podrían incluir:

- ¿Cuánta memoria está disponible en los nodos del clúster?
- ¿Cuánto espacio en disco hay disponible en los nodos del clúster?
- ¿La máquina virtual requiere requisitos de almacenamiento específicos (es decir, quiero que mis SQL Server máquinas virtuales vayan a un clúster que ejecuta unidades más rápidas) o bien, mi máquina virtual de infraestructura no es tan crítica y se puede ejecutar en unidades más lentas.

Una vez que se responde a estas preguntas, debe crear la máquina virtual en el clúster que necesita.  Una de las ventajas de los conjuntos de clústeres es que los conjuntos de clústeres realizan dichas comprobaciones y colocan la máquina virtual en el nodo más óptimo.

Los comandos siguientes identificarán el clúster óptimo e implementarán la máquina virtual en él.  En el ejemplo siguiente, se crea una nueva máquina virtual que especifica que al menos 4 gigabytes de memoria están disponibles para la máquina virtual y que necesitará utilizar 1 procesador virtual.

- Asegúrese de que 4 GB están disponibles para la máquina virtual
- establecer el procesador virtual usado en 1
- Asegúrese de que hay al menos un 10% de CPU disponible para la máquina virtual

   ```PowerShell
   # Identify the optimal node to create a new virtual machine
   $memoryinMB=4096
   $vpcount = 1
   $targetnode = Get-ClusterSetOptimalNodeForVM -CimSession CSMASTER -VMMemory $memoryinMB -VMVirtualCoreCount $vpcount -VMCpuReservation 10
   $secure_string_pwd = convertto-securestring "<password>" -asplaintext -force
   $cred = new-object -typename System.Management.Automation.PSCredential ("<domain\account>",$secure_string_pwd)

   # Deploy the virtual machine on the optimal node
   Invoke-Command -ComputerName $targetnode.name -scriptblock { param([String]$storagepath); New-VM CSVM1 -MemoryStartupBytes 3072MB -path $storagepath -NewVHDPath CSVM.vhdx -NewVHDSizeBytes 4194304 } -ArgumentList @("\\SOFS-CLUSTER1\VOLUME1") -Credential $cred | Out-Null
   
   Start-VM CSVM1 -ComputerName $targetnode.name | Out-Null
   Get-VM CSVM1 -ComputerName $targetnode.name | fl State, ComputerName
   ```

Cuando se complete, se le proporcionará la información sobre la máquina virtual y dónde se colocó.  En el ejemplo anterior, se mostraría como:

        State         : Running
        ComputerName  : 1-S2D2

Si no tiene suficiente memoria, CPU o espacio en disco para agregar la máquina virtual, recibirá el error:

      Get-ClusterSetOptimalNodeForVM : A cluster node is not available for this operation.  

Una vez creada la máquina virtual, se mostrará en el administrador de Hyper-V en el nodo específico especificado.  Para agregarlo como una máquina virtual de conjunto de clústeres y en el clúster, el comando se muestra a continuación.  

        Register-ClusterSetVM -CimSession CSMASTER -MemberName $targetnode.Member -VMName CSVM1

Cuando finalice, la salida será:

         Id  VMName  State  MemberName  PSComputerName
         --  ------  -----  ----------  --------------
          1  CSVM1      On  CLUSTER1    CSMASTER

Si ha agregado un clúster con máquinas virtuales existentes, las máquinas virtuales también tendrán que registrarse con conjuntos de clústeres, por lo que debe registrar todas las máquinas virtuales a la vez. el comando que se va a usar es:

        Get-ClusterSetMember -name CLUSTER3 -CimSession CSMASTER | Register-ClusterSetVM -RegisterAll -CimSession CSMASTER

Sin embargo, el proceso no está completo, ya que la ruta de acceso a la máquina virtual debe agregarse al espacio de nombres del conjunto de clústeres.

Por ejemplo, se agrega un clúster existente y tiene máquinas virtuales preconfiguradas que residen en el Volumen compartido de clúster local (CSV), la ruta de acceso del VHDX sería algo similar a "C:\ClusterStorage\Volume1\MYVM\Virtual Hard Disks\MYVM.vhdx.  Una migración de almacenamiento debe realizarse como las rutas de acceso de CSV son de diseño local a un solo clúster de miembros. Por lo tanto, no será accesible a la máquina virtual una vez que se migren en vivo a través de los clústeres de miembros. 

En este ejemplo, CLUSTER3 se agregó al conjunto de clústeres mediante Add-ClusterSetMember con la infraestructura Servidor de archivos de escalabilidad horizontal como SOFS-CLUSTER3.  Para migrar la configuración y el almacenamiento de la máquina virtual, el comando es:

        Move-VMStorage -DestinationStoragePath \\SOFS-CLUSTER3\Volume1 -Name MYVM

Una vez que se complete, recibirá una advertencia:

        WARNING: There were issues updating the virtual machine configuration that may prevent the virtual machine from running.  For more information view the report file below.
        WARNING: Report file location: C:\Windows\Cluster\Reports\Update-ClusterVirtualMachineConfiguration '' on date at time.htm.

Esta advertencia se puede omitir porque la advertencia es "no se detectó ningún cambio en la configuración de almacenamiento del rol de máquina virtual".  La razón de la advertencia como la ubicación física real no cambia; solo las rutas de acceso de configuración. 

Para obtener más información sobre Move-VMStorage, consulte este [vínculo](https://docs.microsoft.com/powershell/module/hyper-v/move-vmstorage?view=win10-ps). 

La migración en vivo de una máquina virtual entre clústeres de conjuntos de clústeres diferentes no es la misma que en el pasado. En escenarios de conjuntos que no son de clúster, los pasos serían los siguientes:

1. Quite el rol de máquina virtual del clúster.
2. Migre en vivo la máquina virtual a un nodo de miembro de otro clúster.
3. Agregue la máquina virtual al clúster como un nuevo rol de máquina virtual.

Con los conjuntos de clústeres, estos pasos no son necesarios y solo se necesita un comando.  En primer lugar, debe configurar todas las redes para que estén disponibles para la migración con el comando:

    Set-VMHost -UseAnyNetworkForMigration $true

Por ejemplo, quiero cambiar una máquina virtual del conjunto de clústeres de CLUSTER1 a NODO2-CL3 en CLUSTER3.  El único comando sería:

        Move-ClusterSetVM -CimSession CSMASTER -VMName CSVM1 -Node NODE2-CL3

Tenga en cuenta que esto no mueve los archivos de configuración o el almacenamiento de la máquina virtual.  Esto no es necesario, ya que la ruta de acceso a la máquina virtual permanece como \\SOFS-CLUSTER1\VOLUME1.  Una vez que una máquina virtual se ha registrado con conjuntos de clústeres tiene la ruta de acceso del recurso compartido del servidor de archivos de infraestructura, las unidades y la máquina virtual no necesitan estar en el mismo equipo que la máquina virtual.

## <a name="creating-availability-sets-fault-domains"></a>Creación de conjuntos de disponibilidad dominios de error

Tal como se describe en la introducción, los dominios de error y los conjuntos de disponibilidad similares a Azure se pueden configurar en un conjunto de clústeres.  Esto es beneficioso para las ubicaciones de máquinas virtuales iniciales y las migraciones entre clústeres.  

En el ejemplo siguiente, hay cuatro clústeres que participan en el conjunto de clústeres.  Dentro del conjunto, se creará un dominio de error lógico con dos de los clústeres y un dominio de error creado con los otros dos clústeres.  Estos dos dominios de error incluirán el conjunto disponibilidad. 

En el ejemplo siguiente, CLUSTER1 y CLUSTER2 estarán en un dominio de error llamado **FD1** mientras que CLUSTER3 y CLUSTER4 estarán en un dominio de error denominado **FD2**.  El conjunto de disponibilidad se denominará **CSMASTER-as** y constará de los dos dominios de error.

Para crear los dominios de error, los comandos son:

        New-ClusterSetFaultDomain -Name FD1 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER1,CLUSTER2 -Description "This is my first fault domain"

        New-ClusterSetFaultDomain -Name FD2 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER3,CLUSTER4 -Description "This is my second fault domain"

Para asegurarse de que se han creado correctamente, se puede ejecutar Get-ClusterSetFaultDomain con la salida que se muestra.

        PS C:\> Get-ClusterSetFaultDomain -CimSession CSMASTER -FdName FD1 | fl *

        PSShowComputerName    : True
        FaultDomainType       : Logical
        ClusterName           : {CLUSTER1, CLUSTER2}
        Description           : This is my first fault domain
        FDName                : FD1
        Id                    : 1
        PSComputerName        : CSMASTER

Ahora que se han creado los dominios de error, es necesario crear el conjunto de disponibilidad.

        New-ClusterSetAvailabilitySet -Name CSMASTER-AS -FdType Logical -CimSession CSMASTER -ParticipantName FD1,FD2

Para validar que se ha creado, use:

        Get-ClusterSetAvailabilitySet -AvailabilitySetName CSMASTER-AS -CimSession CSMASTER

Al crear nuevas máquinas virtuales, debe usar el parámetro-conjunto como parte de la determinación del nodo óptimo.  Por lo tanto, tendría un aspecto similar al siguiente:

        # Identify the optimal node to create a new virtual machine
        $memoryinMB=4096
        $vpcount = 1
        $av = Get-ClusterSetAvailabilitySet -Name CSMASTER-AS -CimSession CSMASTER
        $targetnode = Get-ClusterSetOptimalNodeForVM -CimSession CSMASTER -VMMemory $memoryinMB -VMVirtualCoreCount $vpcount -VMCpuReservation 10 -AvailabilitySet $av
        $secure_string_pwd = convertto-securestring "<password>" -asplaintext -force
        $cred = new-object -typename System.Management.Automation.PSCredential ("<domain\account>",$secure_string_pwd)

Quitar un clúster de los conjuntos de clústeres debido a varios ciclos de vida. Hay ocasiones en las que un clúster debe quitarse de un conjunto de clústeres. Como procedimiento recomendado, todas las máquinas virtuales del conjunto de clústeres se deben sacar del clúster. Esto puede realizarse mediante los comandos **Move-ClusterSetVM** y **Move-VMStorage** .

Sin embargo, si las máquinas virtuales no se van a migrar también, los conjuntos de clústeres ejecutan una serie de acciones para proporcionar un resultado intuitivo al administrador.  Cuando el clúster se quita del conjunto, todas las máquinas virtuales del conjunto de clústeres restantes hospedadas en el clúster que se va a quitar simplemente se convertirán en máquinas virtuales de alta disponibilidad enlazadas a ese clúster, suponiendo que tengan acceso a su almacenamiento.  Los conjuntos de clústeres también actualizarán automáticamente su inventario mediante:

- Ya no se realiza el seguimiento del estado del clúster quitado ahora y de las máquinas virtuales que se ejecutan en él.
- Quita del espacio de nombres del conjunto de clúster y todas las referencias a recursos compartidos hospedados en el clúster que se quita ahora

Por ejemplo, el comando para quitar el clúster de CLUSTER1 de los conjuntos de clústeres sería:

        Remove-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER

## <a name="frequently-asked-questions-faq"></a>Preguntas más frecuentes (P+F)

**Pregunta:** En mi conjunto de clústeres, ¿estoy limitado a usar solo clústeres hiperconvergidos? <br>
**Respuesta:** No.  Puede mezclar Espacios de almacenamiento directo con los clústeres tradicionales.

**Pregunta:** ¿Puedo administrar mi conjunto de clústeres a través de System Center Virtual Machine Manager? <br>
**Respuesta:** En la actualidad, System Center Virtual Machine Manager no admite conjuntos de clústeres <br><br> **Pregunta:** ¿Pueden coexistir los clústeres de Windows Server 2012 R2 o 2016 en el mismo conjunto de clústeres? <br>
**Pregunta:** ¿Puedo migrar las cargas de trabajo de los clústeres de Windows Server 2012 R2 o 2016 con solo que esos clústeres se unan al mismo conjunto de clústeres? <br>
**Respuesta:** Conjuntos de clústeres es una nueva tecnología que se introduce en Windows Server 2019, por lo que no existe en versiones anteriores. Los clústeres basados en el sistema operativo de nivel inferior no pueden unirse a un conjunto de clústeres. Sin embargo, la tecnología de las actualizaciones graduales del sistema operativo del clúster debe proporcionar la funcionalidad de migración que está buscando mediante la actualización de estos clústeres a Windows Server 2019.

**Pregunta:** ¿Pueden los conjuntos de clústeres permitirme escalar el almacenamiento o el proceso (solo)? <br>
**Respuesta:** Sí, simplemente agregando un espacio de almacenamiento directo o un clúster de Hyper-V tradicional. Con los conjuntos de clústeres, es un cambio sencillo de la relación de proceso a almacenamiento, incluso en un conjunto de clúster hiperconvergido.

**Pregunta:** ¿Qué son las herramientas de administración para conjuntos de clústeres? <br>
**Respuesta:** PowerShell o WMI en esta versión.

**Pregunta:** ¿Cómo funcionará la migración en vivo entre clústeres con procesadores de diferentes generaciones?  <br>
**Respuesta:** Los conjuntos de clústeres no solucionan las diferencias de procesador y sustituyen a lo que actualmente admite Hyper-V.  Por lo tanto, se debe usar el modo de compatibilidad de procesador con las migraciones rápidas.  La recomendación para conjuntos de clústeres es usar el mismo hardware de procesador dentro de cada clúster individual, así como todo el conjunto de clústeres para migraciones en vivo entre los clústeres.

**Pregunta:** ¿Puede mi clúster establecer automáticamente la conmutación por error de máquinas virtuales en un clúster?  <br>
**Respuesta:** En esta versión, las máquinas virtuales del conjunto de clústeres solo se pueden migrar en vivo de forma manual a través de los clústeres. pero no puede realizar la conmutación por error automáticamente. 

**Pregunta:** ¿Cómo se garantiza que el almacenamiento sea resistente a los errores de clúster? <br>
**Respuesta:** Use la solución de réplica de almacenamiento entre clústeres (SR) en los clústeres miembro para obtener la resistencia de almacenamiento a los errores de clúster.

**Pregunta:** Utilizo réplica de almacenamiento (SR) para replicar a través de los clústeres de miembros. ¿Las rutas UNC de almacenamiento de espacio de nombres del conjunto de clúster cambian en la conmutación por error de SR al destino de réplica Espacios de almacenamiento directo clúster? <br>
**Respuesta:** En esta versión, este cambio de referencia de espacio de nombres de conjunto de clústeres no se produce con la conmutación por error de SR. Deje que Microsoft sepa si este escenario es fundamental para usted y cómo planea usarlo.

**Pregunta:** ¿Es posible realizar una conmutación por error de máquinas virtuales a través de dominios de error en una situación de recuperación ante desastres (por ejemplo, el dominio de error completo dejó de funcionar)? <br>
**Respuesta:** No, tenga en cuenta que todavía no se admite la conmutación por error entre clústeres dentro de un dominio de error lógico. 

**Pregunta:** ¿Puede mi clúster establecer clústeres de intervalos en varios sitios (o dominios DNS)? <br> 
**respuesta:** se trata de un escenario no probado y no planeado de inmediato para el soporte de producción. Deje que Microsoft sepa si este escenario es fundamental para usted y cómo planea usarlo.

**Pregunta:** ¿Funciona el conjunto de clústeres con IPv6? <br>
**Respuesta:** Tanto IPv4 como IPv6 son compatibles con los conjuntos de clústeres que con los clústeres de conmutación por error.

**Pregunta:** ¿Cuáles son los requisitos de bosque de Active Directory para conjuntos de clústeres? <br>
**Respuesta:** Todos los clústeres miembros deben estar en el mismo bosque de AD.

**Pregunta:** ¿Cuántos clústeres o nodos pueden formar parte de un único conjunto de clústeres? <br>
**Respuesta:** En Windows Server 2019, los conjuntos de clústeres se han probado y se admite hasta 64 nodos de clúster en total. Sin embargo, la arquitectura de los conjuntos de clústeres se escala a límites mucho mayores y no es algo codificado para un límite. Deje que Microsoft sepa si la escala es más importante para usted y cómo planea usarla.

**Pregunta:** ¿Todos los clústeres de Espacios de almacenamiento directo de un conjunto de clústeres forman un solo grupo de almacenamiento? <br>
**Respuesta:** No. Espacios de almacenamiento directo tecnología sigue funcionando en un solo clúster y no en los clústeres de miembros de un conjunto de clústeres.

**Pregunta:** ¿El espacio de nombres del conjunto de clústeres es altamente disponible? <br>
**Respuesta:** Sí, el espacio de nombres del conjunto de clústeres se proporciona a través de un servidor de espacio de nombres SOFS de referencia disponible continuamente (CA) que se ejecuta en el clúster de administración. Microsoft recomienda tener un número suficiente de máquinas virtuales de los clústeres miembro para que sea resistente a errores localizados en todo el clúster. Sin embargo, para tener en cuenta los errores catastróficos imprevistos (por ejemplo, todas las máquinas virtuales del clúster de administración se desactivan al mismo tiempo), la información de referencia se almacena en caché de forma persistente en cada nodo del conjunto de clústeres, incluso entre reinicios.
 
**Pregunta:** ¿El clúster establece el acceso de almacenamiento basado en el espacio de nombres para reducir el rendimiento del almacenamiento en un conjunto de clústeres? <br>
**Respuesta:** No. Cluster Set namespace ofrece un espacio de nombres de referencia superpuesto dentro de un conjunto de clústeres, conceptualmente como Sistema de archivos distribuido espacios de nombres (DFSN). Y, a diferencia de DFSN, todos los metadatos de referencia de espacio de nombres del conjunto de clúster se rellenan automáticamente y se actualizan automáticamente en todos los nodos sin intervención del administrador, por lo que no hay casi ninguna sobrecarga de rendimiento en la ruta de acceso de almacenamiento. 

**Pregunta:** ¿Cómo puedo realizar copias de seguridad de metadatos del conjunto de clústeres? <br>
**Respuesta:** Esta guía es la misma que la del clúster de conmutación por error. La copia de seguridad del estado del sistema también realizará una copia de seguridad del estado del clúster.  A través de Copias de seguridad de Windows Server, puede realizar restauraciones de solo la base de datos de clúster de un nodo (que nunca debe ser necesaria debido a una serie de lógica de recuperación automática que tenemos) o realizar una restauración autoritativa para revertir toda la base de datos del clúster en todos los nodos. En el caso de los conjuntos de clústeres, Microsoft recomienda realizar una restauración autoritativa primero en el clúster de miembros y, a continuación, en el clúster de administración si es necesario.
