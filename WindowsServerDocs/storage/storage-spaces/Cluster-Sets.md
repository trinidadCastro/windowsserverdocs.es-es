---
title: Conjuntos de clústeres
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: johnmarlin-msft
ms.date: 01/30/2019
description: En este artículo se describe el escenario de conjuntos de clúster
ms.localizationpriority: medium
ms.openlocfilehash: 349b69835ae68c626e886cad30f4d5a89d358372
ms.sourcegitcommit: a3958dba4c2318eaf2e89c7532e36c78b1a76644
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2019
ms.locfileid: "66719707"
---
# <a name="cluster-sets"></a>Conjuntos de clústeres

> Se aplica a: Windows Server 2019

Conjuntos de clúster es la nueva tecnología de escalabilidad horizontal en la nube en la versión de Windows Server 2019 que aumenta el número de nodos de clúster en una centro de datos definida de Software (SDDC) en la nube en órdenes de magnitud. Un conjunto de clústeres es una agrupación de acoplamiento de varios clústeres de conmutación por error: proceso, almacenamiento o hiperconvergida. Clúster establece tecnología permite fluidez de la máquina virtual a través de clústeres de miembro dentro de un conjunto de clústeres y un espacio de nombres unificado de almacenamiento en todo el conjunto para respaldar la fluidez de la máquina virtual.

Mientras experimenta conservación administración del clúster de conmutación por error existente en clústeres de miembro, una instancia del conjunto de clúster ofrece además casos de uso clave en torno a la administración del ciclo de vida en el agregado. Esta guía de evaluación de Windows Server 2019 escenario proporciona la información en segundo plano necesarias, así como instrucciones paso a paso para evaluar la tecnología de conjuntos de clúster mediante PowerShell.

## <a name="technology-introduction"></a>Introducción de tecnología

Tecnología de conjuntos de clúster se desarrolla para satisfacer las solicitudes de cliente específico operativo nubes del centro de datos definido por Software (SDDC) a escala. Propuesta de valor de conjuntos de clúster puede resumirse de la forma siguiente:  

- Aumentar significativamente la escala de nube SDDC admitida para la ejecución de máquinas virtuales de alta disponibilidad mediante la combinación de varios clústeres más pequeños en un único tejido grande, incluso mientras mantiene el límite de errores de software en un solo clúster
- Administrar el ciclo de vida de clúster de conmutación por error incluida la incorporación y retirada de clústeres, sin afectar a la disponibilidad de máquina virtual de inquilino, a través de migración de forma fluida de toda las máquinas virtuales a través de este tejido grande
- Cambiar fácilmente la proporción de proceso para el almacenamiento en su hiperconvergido me
- Beneficiarse de [conjuntos de disponibilidad y dominios de error similar a Azure](htttps://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) a través de clústeres en la selección de máquina virtual inicial y migración de máquinas virtuales posteriores
- Combinar y hacer coincidir diferentes generaciones de hardware de CPU en el mismo clúster establecer fabric, incluso mientras se mantiene dominios de error individuales homogéneos para lograr la máxima eficacia.  Tenga en cuenta que la recomendación del mismo hardware aún está presente en cada clúster individual, así como el conjunto de todo el clúster.

Desde una vista de alto nivel, se trata qué clúster pueden parecer conjuntos.

![Clúster establece la vista de la solución](media/Cluster-sets-Overview/Cluster-sets-solution-View.png)

El siguiente proporciona un resumen rápido de cada uno de los elementos de la imagen anterior:

**Clúster de administración**

Clúster de administración en un conjunto de clústeres es un clúster de conmutación por error que hospeda el plano de administración de alta disponibilidad del conjunto completo de clúster y la referencia de espacio de nombres (Namespace establecer clúster) servidor de archivos de escalabilidad horizontal (SOFS) de almacenamiento unificado. Un clúster de administración se lógicamente separa de los clústeres de miembro que se ejecutan las cargas de trabajo de máquina virtual. Esto hace que el clúster conjunto plano de administración resistente a errores localizados todo el clúster, por ejemplo, la pérdida de alimentación de un clúster de miembro.   

**Clúster de miembro**

Un clúster de miembro en un conjunto de clústeres es normalmente un clúster hiperconvergido tradicional que ejecutando la máquina virtual y las cargas de trabajo de espacios de almacenamiento directo. Varios clústeres de miembro participan en una implementación del conjunto de clúster único, que forman al tejido de nube SDDC mayor. Los clústeres miembros difieren de un clúster de administración en dos aspectos clave: clústeres miembro participan en el dominio de error y construcciones de conjunto de disponibilidad y clústeres de miembro también tamaño se ajusta a la máquina virtual del host y las cargas de trabajo de espacios de almacenamiento directo. Máquinas virtuales del conjunto de clúster que se mueven a través de límites de clúster en un conjunto de clústeres no deben hospedarse en el clúster de administración por este motivo.

**Clúster establecido la referencia de espacio de nombres SOFS**

Una referencia de espacio de nombres de conjunto de clúster (clúster establecer Namespace) SOFS es un servidor de archivos de escalabilidad horizontal en la que cada recurso compartido de SMB en el SOFS Namespace establecido de clúster es un recurso compartido de referencia: del tipo 'SimpleReferral' recientemente introducida en Windows Server 2019. Esta referencia permite que los clientes de bloque de mensajes del servidor (SMB) acceso al destino de recurso compartido de SMB hospedado en el clúster SOFS de miembro. El clúster de establece la referencia de espacio de nombres SOFS es un mecanismo de referencia de peso ligero y por lo tanto, no participa en la ruta de acceso de E/S. Las referencias de SMB se almacenan en caché permanente en cada uno de los nodos de cliente y el espacio de nombres de conjuntos de clúster actualiza dinámicamente automáticamente estas referencias según sea necesario.

**Establecer principal del clúster**

En un conjunto de clústeres, la comunicación entre los clústeres de miembro tiene un acoplamiento flexible y se coordina mediante un nuevo recurso de clúster denominado "Cluster establecer Master" (CS-Master). Al igual que cualquier otro recurso de clúster, CS-Master es altamente disponible y resistente a errores de miembros individuales del clúster o los errores de nodo de clúster de administración. A través de un nuevo proveedor de WMI de clúster establecido, CS-Master proporciona el punto de conexión de administración para todas las interacciones de facilidad de uso establecido de clúster.

**Establezca el clúster de trabajo**

En una implementación de clúster establecido, el maestro de CS interactúa con un nuevo recurso de clúster en el miembro clústeres denominados "Cluster establecer Worker" (CS-Worker). CS-trabajo actúa como el enlace solo en el clúster para orquestar las interacciones del clúster local, como se solicitó el maestro de CS. Ejemplos de estas interacciones incluyen la selección de máquina virtual y realizar un inventario de recursos de clúster local. Solo hay una instancia de CS de trabajo para cada uno de los miembros de clústeres en un conjunto de clústeres. 

**dominio de error**

Un dominio de error es la agrupación de software y los artefactos de hardware que determina el administrador podrían generar un error conjuntamente cuando se produce un error.  Mientras que un administrador podría designar uno o varios clústeres juntos como un dominio de error, cada nodo puede participar en un dominio de error en un conjunto de disponibilidad. Clúster establece por diseño deja la decisión de determinación de límites de dominio de error para los administradores que se conoce bien su con consideraciones de topología de centro de datos – p. ej., PDU, redes, que comparten los clústeres de miembro. 

**Conjunto de disponibilidad**

Un conjunto de disponibilidad ayuda al administrador configurar redundancia deseado de las cargas de trabajo en clúster entre dominios de error, mediante la los conjunto de disponibilidad organización de e implementación de cargas de trabajo en ese conjunto de disponibilidad. Supongamos que si va a implementar una aplicación de dos niveles, se recomienda que configure al menos dos máquinas virtuales en un conjunto de disponibilidad para cada nivel que garantice que cuando un dominio de error en un conjunto de disponibilidad deja de funcionar, tendrá la aplicación al menos una máquina virtual en cada nivel hospedado en un dominio de error diferentes de ese mismo conjunto de disponibilidad.

## <a name="why-use-cluster-sets"></a>¿Por qué usar conjuntos de clúster

Conjuntos de clúster proporciona la ventaja de escala sin sacrificar la resistencia.  

Conjuntos de clúster permite para los clústeres de varios clústeres juntos para crear un tejido de gran tamaño, mientras que cada clúster sigue siendo independiente para proporcionar resistencia.  Por ejemplo, tiene un HCI 4 nodos varios clústeres de máquinas virtuales en ejecución.  Cada clúster proporciona la resistencia necesaria para sí mismo.  Si el almacenamiento o memoria empieza a llenarse, el escalado vertical es el siguiente paso.  Con el escalado, hay algunas consideraciones y opciones.

1. Agregar más almacenamiento al clúster actual.  Con espacios de almacenamiento directo, esto puede ser complicado según las mismas unidades de modelo o firmware exacto pueden no estar disponibles.  También debe tenerse en cuenta la consideración de tiempos de reconstrucción.
2. Agregue más memoria.  ¿Qué ocurre si se sobrepasa en la memoria que pueden controlar las máquinas?  ¿Qué ocurre si todas las ranuras de memoria disponibles están llenos?
3. Agregar nodos de proceso adicionales con las unidades en el clúster actual.  Esto nos remite a la opción 1 necesidad de tener en cuenta.
4. Comprar un clúster completamente nuevo

Esto es donde los conjuntos de clúster proporciona la ventaja de escalado.  Si agrego mi clústeres en un conjunto de clústeres, puedo aprovechar las ventajas de almacenamiento o memoria que puede estar disponible en otro clúster sin compras adicionales.  Desde una perspectiva de resistencia, agregar nodos adicionales a un espacios de almacenamiento directo no va a proporcionar más votos de quórum.  Como se mencionó [aquí](drive-symmetry-considerations.md), un clúster de espacios de almacenamiento directo puede sobrevivir la pérdida de 2 nodos antes de pasar hacia abajo.  Si tiene un clúster HCI 4, 3 nodos dejan de funcionar provoquen todo el clúster.  Si tiene un clúster de 8 nodos, 3 nodos dejan de funcionar provoquen todo el clúster.  Con los conjuntos de clúster que tiene dos clústeres de 4 nodos HCI en el juego, 2 nodos en un HCI dejen de funcionar y 1 nodo en el otro HCI dejan de funcionar, ambos clústeres permanecen activas.  ¿Es mejor crear un clúster de espacios de almacenamiento directo de 16 nodos grande o dividir en cuatro clústeres de 4 nodos y utilizar conjuntos de clúster?  Tener cuatro clústeres de 4 nodos con clúster conjuntos ofrece la misma escala, pero una mejor resistencia que varios nodos de proceso pueden dejar de funcionar (o de forma inesperada para el mantenimiento) y sigue siendo de producción.

## <a name="considerations-for-deploying-cluster-sets"></a>Consideraciones para implementar conjuntos de clúster

Al considerar si los conjuntos de clúster es algo que debe utilizar, tenga en cuenta estas preguntas:

- ¿Necesita ir más allá de los actuales HCI compute y storage límites de escala?
- ¿Todos los procesos y almacenamiento no son exactamente igual la misma?
- ¿En vivo migrar máquinas virtuales entre clústeres?
- ¿Desea que el equipo de Azure similar a los conjuntos de disponibilidad y dominios de error entre varios clústeres?
- ¿Necesita dedicar tiempo a buscar en todos los clústeres para determinar dónde deben ubicarse las máquinas virtuales nuevas?

Si su respuesta es Sí, los conjuntos de clúster es lo que necesita.

Hay algunos otros elementos a tener en cuenta que una mayor SDDC podría cambiar las estrategias de centro de datos general.  SQL Server es un buen ejemplo.  ¿Mover máquinas virtuales de SQL Server entre clústeres requiere licencias de SQL para ejecutar en nodos adicionales?  

## <a name="scale-out-file-server-and-cluster-sets"></a>Servidor de archivos de escalabilidad horizontal y conjuntos de clúster

En Windows Server 2019, hay un nuevo rol de servidor de archivos de escalabilidad horizontal llamado servidor de archivos de escalabilidad horizontal (SOFS) de infraestructura. 

Las siguientes consideraciones se aplican a un rol de infraestructura SOFS:

1.  Puede haber a lo sumo un rol de clúster de SOFS de infraestructura en un clúster de conmutación por error. Se crea el rol SOFS de infraestructura mediante la especificación de la " **-infraestructura**" cambiar el parámetro a la **agregar ClusterScaleOutFileServerRole** cmdlet.  Por ejemplo:

        Add-ClusterScaleoutFileServerRole -Name "my_infra_sofs_name" -Infrastructure

2.  Cada volumen CSV que se crean automáticamente en la conmutación por error desencadena la creación de un recurso compartido SMB con un nombre generado automáticamente basándose en el nombre del volumen CSV. Un administrador directamente no se puede crear o modificar recursos compartidos de SMB en un rol de SOFS, que a través de operaciones de crear o modificar el volumen CSV.

3.  En configuraciones hiperconvergidas, un SOFS de infraestructura permite que un cliente SMB (host de Hyper-V) para comunicarse con garantizado disponibilidad constante (CA) al servidor SMB de SOFS de infraestructura. Este bucle invertido hiperconvergido SMB CA se logra a través de las máquinas virtuales que tenga acceso a sus archivos de disco virtual (VHDx) donde se reenvía la identidad de máquina virtual propietario entre el cliente y servidor. El reenvío de identidad permite archivos VHDx ACL trabajaran igual que en las configuraciones de clúster hiperconvergido estándar como antes.

Una vez que se crea un conjunto de clústeres, el espacio de nombres del conjunto de clústeres se basa en un SOFS de infraestructura en cada uno de los clústeres de miembro y además un SOFS de infraestructura en el clúster de administración.

En el momento en un clúster de miembro se agrega a un conjunto de clústeres, el administrador especifica el nombre de un SOFS de infraestructura en ese clúster si ya existe uno. Si no existe la infraestructura de SOFS, se crea un nuevo rol de infraestructura SOFS en el nuevo clúster de miembro esta operación. Si ya existe un rol de infraestructura SOFS en el clúster de miembro, la operación de adición implícitamente cambia su nombre para el nombre especificado, según sea necesario. Los servidores SMB de singleton existentes, o funciones no infraestructura SOFS en el miembro clústeres se dejan sin usar por el conjunto de clústeres. 

En el momento en que se crea el conjunto de clústeres, el administrador tiene la opción para usar un objeto de equipo de AD existente como la raíz del espacio de nombres en el clúster de administración. Las operaciones de creación del conjunto de clúster crear el rol de clúster SOFS de infraestructura del clúster de administración o cambia el nombre de la función de infraestructura SOFS existente solo como se describió anteriormente para los clústeres de miembro. Infraestructura de SOFS en el clúster de administración se usa como el clúster del conjunto de referencia de espacio de nombres (clúster establecer Namespace) SOFS. Simplemente significa que cada recurso compartido de SMB en el clúster de establecer espacio de nombres que SOFS es un recurso compartido de referencia: del tipo 'SimpleReferral' - recientemente introducida en Windows Server 2019.  Esta referencia se permite el acceso de los clientes SMB al destino de recurso compartido de SMB hospedado en el clúster SOFS de miembro. El clúster de establece la referencia de espacio de nombres SOFS es un mecanismo de referencia de peso ligero y por lo tanto, no participa en la ruta de acceso de E/S. Las referencias de SMB se almacenan en caché permanente en cada uno de los nodos de cliente y el espacio de nombres de conjuntos de clúster actualiza dinámicamente automáticamente estas referencias según sea necesario

## <a name="creating-a-cluster-set"></a>Creación de un conjunto de clústeres

### <a name="prerequisites"></a>Requisitos previos

Cuando se establece la creación de un clúster, se recomienda que los requisitos previos siguientes:

1. Configurar a un cliente de administración que ejecuta Windows Server 2019.
2. Instalar las herramientas de clúster de conmutación por error en este servidor de administración.
3. Creación de los miembros del clúster (al menos dos clústeres con al menos dos volúmenes compartidos de clúster en cada clúster)
4. Crear un clúster de administración (físico o invitado) que une los clústeres de miembro.  Este enfoque garantiza los conjuntos de clúster sigue estando disponible a pesar de los errores de clúster de miembro posibles plano de administración.

### <a name="steps"></a>Pasos

1. Crear un nuevo clúster establecido en tres clústeres tal como se define en los requisitos previos.  El siguiente gráfico proporciona un ejemplo de clústeres para crear.  El nombre del clúster establecido en este ejemplo será **CSMASTER**.

   | Nombre del clúster               | Nombre de SOFS de infraestructura que se usará más adelante | 
   |----------------------------|-------------------------------------------|
   | CONJUNTO DE CLÚSTER                | SOFS CLUSTERSET                           |
   | CLUSTER1                   | CLUSTER1 DE SOFS                             |
   | CLUSTER2                   | CLUSTER2 DE SOFS                             |

2. Una vez que se han creado todos los clústeres, use los siguientes comandos para crear el clúster principal conjunto.

        New-ClusterSet -Name CSMASTER -NamespaceRoot SOFS-CLUSTERSET -CimSession SET-CLUSTER

3. Para agregar un servidor de clúster para el conjunto de clústeres, la continuación se utilizaría.

        Add-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER1
        Add-ClusterSetMember -ClusterName CLUSTER2 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER2

   > [!NOTE]
   > Si está utilizando una combinación de dirección IP estática, debe incluir *x.x.x.x - StaticAddress* en el **New ClusterSet** comando.

4. Una vez haya creado el clúster fuera de los miembros del clúster, puede enumerar el conjunto de nodos y sus propiedades.  Para enumerar todos los clústeres de miembro en el conjunto de clústeres:

        Get-ClusterSetMember -CimSession CSMASTER

5. Para enumerar todos los clústeres miembros del clúster establecer incluidos los nodos de clúster de administración:

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterNode

6. Para enumerar todos los nodos de los clústeres de miembro:

        Get-ClusterSetNode -CimSession CSMASTER

7. Para mostrar todos los grupos de recursos en el conjunto de clústeres:

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterGroup 

8. Para comprobar el clúster de establece el proceso de creación creado un recurso compartido SMB (identificado como Volume1 o que la carpeta CSV se etiqueta con el ScopeName es el nombre del servidor de archivos de infraestructura y la ruta de acceso como) en el SOFS de infraestructura para cada miembro de clúster Volumen CSV:

        Get-SmbShare -CimSession CSMASTER

8. Conjuntos de clúster tiene los registros de depuración que se pueden recopilar para su revisión.  Configurar el clúster y se pueden recopilar los registros de depuración de clúster para todos los miembros y el clúster de administración.

        Get-ClusterSetLog -ClusterSetCimSession CSMASTER -IncludeClusterLog -IncludeManagementClusterLog -DestinationFolderPath <path>

9. Configuración de Kerberos [la delegación restringida](https://blogs.technet.microsoft.com/virtualization/2017/02/01/live-migration-via-constrained-delegation-with-kerberos-in-windows-server-2016/) entre un clúster de todos los miembros del conjunto.

10. Configure el tipo de autenticación de la migración en vivo máquina virtual de entre clústeres a Kerberos en cada nodo en el conjunto de clúster.

        foreach($h in $hosts){ Set-VMHost -VirtualMachineMigrationAuthenticationType Kerberos -ComputerName $h }

11. Agregue el clúster de administración al grupo de administradores local en cada nodo en el conjunto de clústeres.

        foreach($h in $hosts){ Invoke-Command -ComputerName $h -ScriptBlock {Net localgroup administrators /add <management_cluster_name>$} }

## <a name="creating-new-virtual-machines-and-adding-to-cluster-sets"></a>Crear nuevas máquinas virtuales y agregar a los conjuntos de clúster

Después de crear el clúster establecido, el siguiente paso es crear nuevas máquinas virtuales.  Normalmente, cuando llega el momento de crear las máquinas virtuales y agregarlos a un clúster, deberá hacer algunas comprobaciones en los clústeres para ver qué puede ser mejor ejecutar en.  Estas comprobaciones se incluyen:

- ¿Cuánta memoria está disponible en los nodos del clúster?
- ¿Cuánto espacio en disco está disponible en los nodos del clúster?
- La máquina virtual requiere los requisitos de almacenamiento específico (es decir, quiero que Mis máquinas virtuales de SQL Server para ir a un clúster que ejecuta las unidades de disco más rápidos; o bien, mi máquina virtual de infraestructura no es tan importante y puede ejecutar en unidades más lentas).

Una vez que este preguntas, crear la máquina virtual en el clúster que lo necesite.  Una de las ventajas de los conjuntos de clúster es que los conjuntos de clúster hacer esas comprobaciones por usted y colocan la máquina virtual en el nodo más óptimo.

Los siguientes comandos tanto identificará el clúster óptimo e implementar la máquina virtual en él.  En el ejemplo siguiente, se crea una nueva máquina virtual, especifica que al menos de 4 gigabytes de memoria está disponible para la máquina virtual y que deberá utilizar 1 procesador virtual.

- Asegúrese de que está disponible para la máquina virtual de 4gb
- establecer el procesador virtual utilizado en 1
- Asegúrese de que hay al menos un 10% CPU disponible para la máquina virtual

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

Cuando se complete, se le ofrecerá la información acerca de la máquina virtual y en el que se encuentra.  En el ejemplo anterior, mostrarían como:

        State         : Running
        ComputerName  : 1-S2D2

Si tuviera que no tiene suficiente memoria, cpu o espacio en disco para agregar la máquina virtual, recibirá el error:

      Get-ClusterSetOptimalNodeForVM : A cluster node is not available for this operation.  

Una vez creada la máquina virtual, se mostrará en el Administrador de Hyper-V en el nodo específico especificado.  Para agregarlo como un clúster de máquina virtual del conjunto y en el clúster, el comando está por debajo.  

        Register-ClusterSetVM -CimSession CSMASTER -MemberName $targetnode.Member -VMName CSVM1

Cuando se complete, la salida será:

         Id  VMName  State  MemberName  PSComputerName
         --  ------  -----  ----------  --------------
          1  CSVM1      On  CLUSTER1    CSMASTER

Si ha agregado un clúster con las máquinas virtuales existentes, las máquinas virtuales también necesitará registrarse en conjuntos, por lo que registrar todas las máquinas virtuales al mismo tiempo, el comando para usar el clúster:

        Get-ClusterSetMember -name CLUSTER3 -CimSession CSMASTER | Register-ClusterSetVM -RegisterAll -CimSession CSMASTER

Sin embargo, el proceso no es completando como la ruta de acceso a la máquina virtual debe agregarse al espacio de nombres del conjunto de clúster.

Por ejemplo, se agrega un clúster existente y tiene máquinas virtuales preconfiguradas el residen en el volumen de compartidos de clúster de local (CSV), la ruta de acceso para el VHDX sería algo parecido a "C:\ClusterStorage\Volume1\MYVM\Virtual Disks\MYVM.vhdx de disco duro.  Una migración de almacenamiento deberá realizarse como rutas de acceso CSV se realizan mediante diseño local a un clúster único miembro. Por lo tanto, no será accesible a la máquina virtual una vez que estén en vivo migra entre clústeres de miembro. 

En este ejemplo, CLUSTER3 se agregó al clúster establecido mediante Add-ClusterSetMember con el servidor de archivos de escalabilidad horizontal de infraestructura como SOFS CLUSTER3.  Para mover la configuración de máquina virtual y el almacenamiento, el comando es:

        Move-VMStorage -DestinationStoragePath \\SOFS-CLUSTER3\Volume1 -Name MYVM

Cuando haya terminado, recibirá una advertencia:

        WARNING: There were issues updating the virtual machine configuration that may prevent the virtual machine from running.  For more information view the report file below.
        WARNING: Report file location: C:\Windows\Cluster\Reports\Update-ClusterVirtualMachineConfiguration '' on date at time.htm.

Esta advertencia puede omitirse sea la advertencia "se han detectado ningún cambio en la configuración de almacenamiento del rol de máquina virtual".  El motivo de la advertencia como la ubicación física real no cambia; las rutas de configuración. 

Para obtener más información sobre Move-VMStorage, revise esta [vínculo](https://docs.microsoft.com/powershell/module/hyper-v/move-vmstorage?view=win10-ps). 

En vivo de migración de una máquina virtual entre clústeres de conjunto de diferentes clústeres es no igual que en el pasado. En escenarios que no son clúster establecido, los pasos serían:

1. quitar el rol de máquina virtual del clúster.
2. migración en vivo la máquina virtual a un nodo de miembro de un clúster diferente.
3. agregar la máquina virtual en el clúster como un nuevo rol de máquina virtual.

Estos pasos con conjuntos de clúster no son necesarios y se necesita un solo comando.  En primer lugar, debe establecer todas las redes estén disponibles para la migración con el comando:

    Set-VMHost -UseAnyNetworkMigration $true

Por ejemplo, quiero mover una máquina virtual de clúster pone de CLUSTER1 a NODE2 CL3 en CLUSTER3.  El comando sería:

        Move-ClusterSetVM -CimSession CSMASTER -VMName CSVM1 -Node NODE2-CL3

Tenga en cuenta que esto no mueve los archivos de almacenamiento o la configuración de máquina virtual.  Esto no es necesario que la ruta de acceso a la máquina virtual permanece como \\CLUSTER1\VOLUME1 de SOFS.  Una vez que una máquina virtual se ha registrado con conjuntos de clúster tiene la ruta de acceso del recurso compartido de servidor de archivos de infraestructura, las unidades y la máquina virtual no requieren que se está en el mismo equipo que la máquina virtual.

## <a name="creating-availability-sets-fault-domains"></a>Disponibilidad de creación de conjuntos de dominios de error

Como se describe en la introducción, dominios de error similar a Azure y conjuntos de disponibilidad pueden configurarse en un conjunto de clústeres.  Esto es beneficioso para ubicaciones de máquinas virtuales inicial y las migraciones entre clústeres.  

En el ejemplo siguiente, hay cuatro clústeres que participan en el conjunto de clústeres.  Dentro del conjunto, se creará un dominio de error lógico con dos de los clústeres y un dominio de error creados con los otros dos clústeres.  Estos dos dominios de error, perderá el conjunto de disponibilidad. 

En el ejemplo siguiente, CLUSTER1 y CLUSTER2 estará en un dominio de error denominado **FD1** mientras CLUSTER3 y CLUSTER4 estará en un dominio de error denominado **FD2**.  El conjunto de disponibilidad se llamará **CSMASTER-AS** y estar formado por los dos dominios de error.

Para crear los dominios de error, los comandos son:

        New-ClusterSetFaultDomain -Name FD1 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER1,CLUSTER2 -Description "This is my first fault domain"

        New-ClusterSetFaultDomain -Name FD2 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER3,CLUSTER4 -Description "This is my second fault domain"

Para asegurarse de que se ha creado correctamente, se puede ejecutar Get-ClusterSetFaultDomain con su salida que se muestra.

        PS C:\> Get-ClusterSetFaultDomain -CimSession CSMASTER -FdName FD1 | fl *

        PSShowComputerName    : True
        FaultDomainType       : Logical
        ClusterName           : {CLUSTER1, CLUSTER2}
        Description           : This is my first fault domain
        FDName                : FD1
        Id                    : 1
        PSComputerName        : CSMASTER

Ahora que se han creado los dominios de error, el conjunto de disponibilidad deben crearse.

        New-ClusterSetAvailabilitySet -Name CSMASTER-AS -FdType Logical -CimSession CSMASTER -ParticipantName FD1,FD2

Para comprobar que se ha creado, a continuación, use:

        Get-ClusterSetAvailabilitySet -AvailabilitySetName CSMASTER-AS -CimSession CSMASTER

Al crear nuevas máquinas virtuales, a continuación, deberá usar el parámetro - conjunto de disponibilidad como parte de determinar el nodo óptimo.  Por lo que, a continuación, sería algo parecido a esto:

        # Identify the optimal node to create a new virtual machine
        $memoryinMB=4096
        $vpcount = 1
        $av = Get-ClusterSetAvailabilitySet -Name CSMASTER-AS -CimSession CSMASTER
        $targetnode = Get-ClusterSetOptimalNodeForVM -CimSession CSMASTER -VMMemory $memoryinMB -VMVirtualCoreCount $vpcount -VMCpuReservation 10 -AvailabilitySet $av
        $secure_string_pwd = convertto-securestring "<password>" -asplaintext -force
        $cred = new-object -typename System.Management.Automation.PSCredential ("<domain\account>",$secure_string_pwd)

Eliminación de un clúster de clúster establece debido a varios ciclos de vida. Hay veces cuando se necesita un clúster va a quitar de un conjunto de clústeres. Como práctica recomendada, todas las máquinas virtuales de clúster conjunto deben moverse fuera del clúster. Esto puede realizarse mediante la **movimiento ClusterSetVM** y **Move-VMStorage** comandos.

Sin embargo, si así no se moverá las máquinas virtuales, conjuntos de clúster se ejecuta una serie de acciones para proporcionar un resultado intuitivo para el administrador.  Cuando el clúster se quita del conjunto, todos los restantes conjunto de máquinas virtuales del clúster hospedadas en el clúster que se va a quitar simplemente se convertirá en máquinas virtuales altamente disponibles enlazadas a ese clúster, suponiendo que tengan acceso a su almacenamiento.  Conjuntos de clúster también actualizará automáticamente su inventario por:

- Ya no seguimiento del estado del clúster quitado por ahora y las máquinas virtuales que se ejecutan en él
- Quita del conjunto de nombres del clúster y todas las referencias a recursos compartidos hospedados en el clúster ahora-quitar

Por ejemplo, el comando para quitar el clúster clúster1 de conjuntos de clúster sería:

        Remove-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER

## <a name="frequently-asked-questions-faq"></a>Preguntas más frecuentes

**Pregunta:** En mi conjunto de clústeres, ¿estoy limitado a solo mediante clústeres hiperconvergidos? <br>
**Respuesta:** No.  Puede mezclar espacios de almacenamiento directo con los clústeres tradicionales.

**Pregunta:** ¿Puedo administrar mi clúster establecido a través de System Center Virtual Machine Manager? <br>
**Respuesta:** System Center Virtual Machine Manager no admite actualmente los conjuntos de clúster <br><br> **Pregunta:** ¿Pueden Windows Server 2012 R2 o 2016 clústeres coexistir en el mismo conjunto de clústeres? <br>
**Pregunta:** ¿Puedo migrar las cargas de trabajo fuera de Windows Server 2012 R2 o 2016 clústeres por el solo hecho de tener esos clústeres unir el mismo conjunto de clúster? <br>
**Respuesta:** Conjuntos de clúster es una nueva tecnología que se introdujo en Windows Server 2019, así que por lo tanto, no existe en las versiones anteriores. Los clústeres basados en sistema operativo de nivel inferior no pueden unirse a un conjunto de clústeres. Sin embargo, la tecnología de las actualizaciones gradual de sistema operativo del clúster debe proporcionar la funcionalidad de migración que buscan actualizando estos clústeres a Windows Server 2019.

**Pregunta:** ¿Pueden conjuntos Clústerme permite escalar el almacenamiento o de proceso (solo)? <br>
**Respuesta:** Sí, agregando simplemente un clúster de Hyper-V tradicional o espacio de almacenamiento directo. Con los conjuntos de clúster es un sencillo cambio de relación de almacenamiento de Compute incluso en un conjunto de clúster hiperconvergido.

**Pregunta:** ¿Qué es que las herramientas de administración para los conjuntos de clúster <br>
**Respuesta:** PowerShell o WMI en esta versión.

**Pregunta:** ¿Cómo funcionarán con procesadores de diferentes generaciones de la migración en vivo entre clústeres?  <br>
**Respuesta:** Conjuntos de clústeres solucionar diferencias de procesador y sustituyen a lo que actualmente admite Hyper-V no.  Por lo tanto, se debe usar el modo de compatibilidad de procesador con migraciones rápidas.  La recomendación para los conjuntos de clúster es usar el mismo hardware de procesador en cada clúster individual, así como el conjunto completo de clúster para las migraciones en vivo entre clústeres que se produzca.

**Pregunta:** ¿Puede mi conjunto de clústeres virtuales automáticamente equipos en un error de clúster de conmutación por error?  <br>
**Respuesta:** En esta versión, las máquinas virtuales de conjunto de clúster solo puede ser manualmente migran en vivo entre clústeres; pero no automáticamente la conmutación por error. 

**Pregunta:** ¿Cómo nos aseguramos de almacenamiento es resistente a errores de clúster? <br>
**Respuesta:** Usar soluciones de réplica de almacenamiento (SR) entre clústeres a través de clústeres de miembro para sacar partido de la resistencia de almacenamiento de clústeres.

**Pregunta:** Usar réplica de almacenamiento (SR) para replicar entre clústeres de miembro. ¿Almacenamiento del espacio de nombres de conjuntos de cambios de las rutas de acceso UNC del clúster al clúster de espacios de almacenamiento directo de destino de réplica en conmutación por error de SR? <br>
**Respuesta:** En esta versión, este clúster conjunto espacio de nombres referencia cambio no se produce con SR conmutación por error. Por favor, informar a Microsoft si este escenario es fundamental y cómo planea usarla.

**Pregunta:** ¿Es posible a las máquinas virtuales de conmutación por error entre dominios de error en una situación de recuperación ante desastres (es decir, no se quitó el dominio de error completo)? <br>
**Respuesta:** No, tenga en cuenta que entre clústeres de conmutación por error dentro de un error lógico dominio todavía no se admite. 

**Pregunta:** ¿Puede mi clúster establecer intervalo de clústeres en varios sitios (o dominios DNS)? <br> 
**Respuesta:** Esto es un escenario no probado e inmediatamente no planeado para la compatibilidad de producción. Por favor, informar a Microsoft si este escenario es fundamental y cómo planea usarla.

**Pregunta:** ¿Establece el clúster de trabajo con IPv6? <br>
**Respuesta:** IPv4 e IPv6 se admiten con conjuntos de clúster, al igual que con los clústeres de conmutación por error.

**Pregunta:** ¿Cuáles son los requisitos de bosque de Active Directory para el clúster establece <br>
**Respuesta:** Todos los clústeres de miembro deben estar en el mismo bosque de AD.

**Pregunta:** ¿Cuántos clústeres o nodos puede ser parte de un único clúster establecer? <br>
**Respuesta:** Conjuntos de clústeres en Windows Server 2019, se han probado y admite hasta 64 nodos total del clúster. Sin embargo, clúster establece arquitectura escala mucho mayor límites y no es algo que está codificado para un límite. Por favor, informar a Microsoft si mayor escala es fundamental y cómo planea usarla.

**Pregunta:** ¿Todos los clústeres de espacios de almacenamiento directo en un conjunto de clústeres forman un único grupo de almacenamiento? <br>
**Respuesta:** No. Tecnología de almacenamiento de espacios directo todavía funciona dentro de un clúster y no a través de clústeres de miembro en un conjunto de clústeres.

**Pregunta:** ¿Es el clúster de establecer espacio de nombres de alta disponibilidad? <br>
**Respuesta:** Sí, se proporciona el espacio de nombres del conjunto de clúster a través de un servidor de espacio de nombres SOFS de referencia continuamente disponibles (CA) que se ejecutan en el clúster de administración. Microsoft recomienda tener suficiente número de máquinas virtuales de clústeres de miembro para que sea resistente a errores localizados de todo el clúster. Sin embargo, para tener en cuenta los errores catastróficos imprevistos: por ejemplo, todas las máquinas virtuales en el clúster de administración deja de funcionar al mismo tiempo: la información de referencia además de forma persistente se almacena en cada nodo del conjunto de clúster, incluso entre reinicios.
 
**Pregunta:** ¿Se el clúster establecido ralentizar el rendimiento del almacenamiento de acceso de almacenamiento basado en el espacio de nombres en un conjunto de clústeres? <br>
**Respuesta:** No. Conjunto de nombres del clúster ofrece un espacio de nombres de referencia de superposición dentro de un conjunto de clústeres: conceptualmente como archivo de sistema distribuido espacios de nombres (DFSN). Y, a diferencia de DFSN, todos los metadatos de referencia de espacio de nombres de conjunto de clúster están rellena automáticamente y se actualiza automáticamente en todos los nodos sin intervención del administrador, por lo que hay casi ninguna sobrecarga de rendimiento en la ruta de acceso de almacenamiento. 

**Pregunta:** ¿Cómo puedo backup los metadatos del conjunto de clúster? <br>
**Respuesta:** Esta guía es igual que el del clúster de conmutación por error. La copia de seguridad sistema de estado de una copia de seguridad del estado del clúster.  A través de la copia de seguridad de Windows Server, puede realizar restauraciones de solo de un nodo clúster base de datos (que nunca debería ser necesario debido a un montón de lógica de recuperación automática tenemos) o realizar una restauración autoritativa para revertir la base de datos de todo el clúster en todos los nodos. En el caso de conjuntos de clúster, Microsoft recomienda hacer este tipo de una restauración autoritativa en primer lugar en el clúster de miembro y, a continuación, en el clúster de administración si es necesario.
