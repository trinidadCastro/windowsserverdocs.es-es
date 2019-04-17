---
title: Conjuntos de clústeres
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: johnmarlin-msft
ms.date: 01/30/2019
description: Este artículo describe el escenario de conjuntos de clúster
ms.localizationpriority: medium
ms.openlocfilehash: 2deeb6968f910e80bacb2354ad2e575060a7797a
ms.sourcegitcommit: 07aefbdbb0eedb42aaed3d195c2429310c761da0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "9041011"
---
# Conjuntos de clústeres

> Se aplica a: Compilación de Windows Server Insider Preview 17650 y versiones posterior

Conjuntos de clúster es la nueva tecnología de escalabilidad horizontal en la nube en esta versión preliminar que aumenta el número de nodos de clúster en una única en la nube de centro de datos definido por Software (SDDC) órdenes de magnitud. Un conjunto de clúster es una agrupación imprecisa de varios clústeres de conmutación por error: cálculo, almacenamiento o hiperconvergida. Clúster establece tecnología permite fluidez de máquina virtual en clústeres de miembro dentro de un conjunto de clúster y un espacio de nombres de almacenamiento de información unificado del conjunto para apoyar la fluidez de máquina virtual. 

Aunque conservación existente de la administración de clústeres de conmutación por error experiencias en clústeres de miembro, una instancia del conjunto de clúster ofrece además casos de uso clave alrededor de la administración del ciclo de vida en el agregado. Esta guía de evaluación de escenario de versión preliminar de Windows Server proporciona la información necesaria en segundo plano junto con instrucciones paso a paso para evaluar la tecnología de conjuntos de clúster de uso de PowerShell. 

## Introducción de tecnología

Tecnología de conjuntos de clústeres se desarrolla para satisfacer las solicitudes de cliente específicas operativo nubes de centros de datos definido por Software (SDDC) a una escala. Propuesta de valor de conjuntos de clúster en esta versión preliminar puede resumirse como lo siguiente:  

- Aumentar significativamente la escala de nube SDDC compatible para ejecutar máquinas virtuales altamente disponibles mediante la combinación de varios clústeres más pequeños en un solo tejido de gran tamaño, incluso mientras mantiene el límite de tolerancia de software en un único clúster
- Administrar todo el clúster de conmutación por error de ciclo de vida incluidos incorporación y retirada de clústeres, sin afectar a la disponibilidad de máquina virtual de inquilino, a través de la migración de forma fluida máquinas virtuales a través de este fabric grande
- Cambiar fácilmente la relación de cálculo para almacenamiento en la hiperconvergida I
- Beneficiarse de [conjuntos de dominios de error similar de Azure y disponibilidad](htttps://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) entre clústeres en la colocación de la máquina virtual inicial y la migración de máquina virtual posteriores
- Coincidencia de mezcla de distintas generaciones de hardware de CPU en el mismo clúster establece tejido, incluso manteniendo dominios de error individuales homogénea por motivos de eficacia máximo.  Ten en cuenta que las recomendaciones del mismo hardware aún está presente en cada clúster individual, así como el conjunto de todo el clúster.

Desde una vista de alto nivel, esto es lo que clúster conjuntos pueden tener el siguiente aspecto.

![Clúster establece la vista de la solución](media\Cluster-sets-Overview\Cluster-sets-solution-View.png)

A continuación proporciona un resumen rápido de cada uno de los elementos en la imagen anterior:

**Clúster de administración**

Clúster de administración en un conjunto de clúster es un clúster de conmutación por error que hospeda el plano de administración de alta disponibilidad del conjunto de todo el clúster y la referencia de espacio de nombres (Namespace establecer clúster) de almacenamiento de información unificado servidor de archivos de escalabilidad horizontal (SOFS). Un clúster de administración es lógicamente desacoplado de clústeres de miembro que ejecutan las cargas de trabajo de máquina virtual. Esto hace que el clúster establece plano de administración resistente a errores localizados en todo el clúster, por ejemplo, la pérdida de alimentación de un clúster de miembro.   

**Clúster de miembro**

Un clúster de miembro en un conjunto de clúster suele ser un clúster tradicional hiperconvergida ejecuta máquinas virtuales y cargas de trabajo de espacios de almacenamiento directo. Varios clústeres de miembro participan en una implementación de conjunto único clúster, que forman al tejido en la nube SDDC más grande. Clústeres de miembro en qué se diferencian de un clúster de administración en dos aspectos fundamentales: clústeres miembro participen en el dominio de error y construcciones de conjunto de disponibilidad y, también se ajusta el tamaño de los clústeres de miembro a la máquina virtual del host y cargas de trabajo de espacios de almacenamiento directo. Máquinas virtuales de clúster conjunto que se mueven en los límites de clúster en un conjunto de clúster no debe estar alojadas en el clúster de administración por este motivo.

**Referencia de espacio de nombres SOFS de conjunto de clústeres**

Una referencia de espacio de nombres de conjunto de clúster (clúster establece Namespace) SOFS es un servidor de archivos de escalabilidad horizontal donde cada recurso compartido de SMB en los SOFS establecer Namespace de clúster es un recurso compartido de referencia – de tipo 'SimpleReferral' recién introducida en esta versión preliminar.  Esta referencia permite a los clientes de bloque de mensajes del servidor (SMB) acceso al destino de recurso compartido SMB alojado en el clúster de miembro SOFS. El clúster establece referencias de espacio de nombres SOFS es un mecanismo de referencia ligera y por lo tanto, no participa en la ruta de acceso de E/S. Se almacenan en caché las referencias de SMB permanente en cada uno de los nodos del cliente y el espacio de nombres de conjuntos de clúster dinámicamente actualiza automáticamente estas referencias según sea necesario.

**Maestro de conjunto de clústeres**

En un conjunto de clúster, la comunicación entre los clústeres de miembro es imprecisa y está coordinada por un nuevo recurso de clúster denominado "Establecer maestro de clúster" (CS-Master). Al igual que cualquier otro recurso de clúster, CS-Master es altamente disponible y resistente a errores de clúster de miembros individuales o los errores de nodo de clúster de administración. A través de un nuevo proveedor de WMI de conjunto de clúster, CS-Master proporciona el punto de conexión de administración para todas las interacciones de facilidad de uso conjunto de clústeres.

**Trabajador de conjunto de clústeres**

En una implementación de conjunto de clústeres, el patrón de CS interactúa con un nuevo recurso de clúster en el miembro clústeres denominados "Clúster establece trabajador" (CS-trabajador). CS trabajador actúa como el enlace único en el clúster para organizar las interacciones de clúster local según lo solicitado por el patrón de CS. Colocación de la máquina virtual y realizar un inventario de los recursos locales del clúster son ejemplos de dichas interacciones. Hay solo una instancia de trabajo de CS para cada uno de los miembros de clústeres en un conjunto de clúster. 

**Dominio de error**

Un dominio de error es la agrupación de software y los artefactos de hardware que determina el administrador podrían producirse un error juntos cuando se produce un error.  Mientras que un administrador puede designar uno o varios clústeres juntos como un dominio de error, cada nodo puede participar en un dominio de error en un conjunto de disponibilidad. Clúster establece mediante hojas de diseño la decisión de determinación de límites de dominio de error en el administrador que está bien muchos conocimientos con consideraciones de topología de centro de datos: p. ej., PDU, redes: que comparten clústeres de miembro. 

**Conjunto de disponibilidad**

Un conjunto de disponibilidad ayuda al administrador configurar redundancia deseado de cargas de trabajo en clúster entre dominios de error, organizando aquellos a un conjunto de disponibilidad y la implementación de cargas de trabajo en ese conjunto de disponibilidad. Supongamos que si vas a implementar una aplicación de dos niveles, te recomendamos que configurar al menos dos máquinas virtuales en una disponibilidad para cada franja que garantiza que cuando un dominio de error en un conjunto de disponibilidad deja de funcionar, la aplicación al menos tenga una máquina virtual en cada nivel hospedado en un dominio de error diferentes de ese mismo conjunto de disponibilidad.

## ¿Por qué usar conjuntos de clúster

Conjuntos de clúster proporciona la ventaja de escala sin sacrificar la resistencia.  

Conjuntos de clústeres permite clústeres de varios clústeres juntos para crear un tejido de gran tamaño, mientras que cada clúster permanece independiente para resistencia.  Por ejemplo, tienes un HCI de 4 nodos varios clústeres de máquinas virtuales en ejecución.  Cada clúster proporciona la resistencia necesaria para el propio.  Si el almacenamiento o la memoria se inicia rellenar, escalado vertical es el siguiente paso.  Con el escalado, hay algunas opciones y consideraciones.

1. Agregar más capacidad de almacenamiento al clúster actual.  Con espacios de almacenamiento directo, esto puede ser difícil que las mismas unidades de modelo o firmware exacto pueden no estar disponibles.  El examen de los tiempos de reconstrucción también debe tenerse en cuenta.
2. Agregar más memoria.  ¿Qué ocurre si están sobrepasa en la memoria que pueden controlar las máquinas?  ¿Qué ocurre si todas las ranuras de memoria disponible son completas?
3. Agregar nodos de proceso adicional con las unidades en el clúster actual.  Esto nos lleva volver a la opción 1 necesidad de tener en cuenta.
4. Comprar un clúster completamente nuevo

Esto es donde conjuntos de clúster proporciona la ventaja de ajuste de escala.  Si puedo agregar Mis clústeres en un conjunto de clúster, puedo sacar provecho de almacenamiento o de memoria que podrían estar disponible en otro clúster sin cualquier compra adicional.  Desde una perspectiva de resistencia, agregar nodos adicionales a un espacios de almacenamiento directo no va a proporcionar votos adicionales para cuórum.  Como se mencionó [aquí](drive-symmetry-considerations.md), un clúster de directo de espacios de almacenamiento puede sobrevivir la pérdida de 2 nodos antes de ir hacia abajo.  Si tienes un clúster HCI de 4 nodos, 3 nodos bloquearse bloqueará todo el clúster.  Si tienes un clúster de 8 nodos, 3 nodos bloquearse bloqueará todo el clúster.  Con conjuntos de clúster que tiene dos clústeres HCI de 4 nodos en el conjunto, 2 nodos en una HCI ir hacia abajo y 1 nodo en el otro HCI ir hacia abajo, ambos clústeres permanecen arriba.  ¿Es mejor para crear un clúster de espacios de almacenamiento directo de 16 nodos grande o dividir en cuatro clústeres de 4 nodos y usar los conjuntos de clúster?  Tener cuatro clústeres de 4 nodos con clúster conjuntos ofrece la misma escala, pero la resistencia mejor que varios nodos de cálculo pueden ir hacia abajo (inesperadamente o mantenimiento) y permanece de producción.

## Consideraciones para la implementación de conjuntos de clúster

Al considerar si conjuntos de clúster es algo que necesitas para usar, ten en cuenta estas preguntas:

- ¿Necesitas ir más allá de la actual HCI cálculo y almacenamiento escala límites?
- ¿Todos los cálculo y almacenamiento no son idénticos los mismos?
- ¿Dinámicos migrar máquinas virtuales entre clústeres?
- ¿Le gustaría conjuntos de disponibilidad del equipo similar de Azure y dominios de error en varios clústeres?
- ¿Necesitas el tiempo para ver todos los clústeres para determinar donde es necesario colocar todas las máquinas virtuales nuevas?

Si la respuesta es "Sí", los conjuntos de clúster es lo que necesitas.

Hay algunos otros elementos a tener en cuenta que una mayor SDDC puede cambiar sus estrategias de centros de datos general.  SQL Server es un buen ejemplo.  ¿Mover SQL Server las máquinas virtuales entre clústeres requiere licencias de SQL que se ejecute en nodos adicionales?  

## Servidor de archivos de escalabilidad horizontal y los conjuntos de clúster

En Windows Server 2019, hay un nuevo rol de servidor de archivos de escalabilidad horizontal llamado servidor de archivos de escalabilidad horizontal (SOFS) de infraestructura. 

Las consideraciones siguientes se aplican a un rol de infraestructura SOFS:

1.  Puede haber a lo sumo solo un rol de clúster de infraestructura SOFS en un clúster de conmutación por error. Rol de infraestructura SOFS se crea especificando la "**-infraestructura**" pasar parámetros al cmdlet **Add-ClusterScaleOutFileServerRole** .  Por ejemplo:

        Add-ClusterScaleoutFileServerRole -Name "my_infra_sofs_name" -Infrastructure

2.  Cada volumen CSV creado automáticamente en la conmutación por error, desencadena la creación de un recurso compartido SMB con un nombre generado automáticamente en función del nombre de volumen CSV. Un administrador directamente no se puede crear o modificar recursos compartidos SMB en una función SOFS, distinto a través de operaciones de crear o modificar de volumen CSV.

3.  En configuraciones hiperconvergida, un SOFS de infraestructura permite que a un cliente SMB (host de Hyper-V) para comunicarse con garantizado continua disponibilidad (CA) para el servidor SMB SOFS de infraestructura. Este bucle invertido hiperconvergida de SMB CA se logra a través de las máquinas virtuales acceso a sus archivos de disco virtual (VHDx) donde se reenvía la identidad de la máquina virtual propietario entre el cliente y servidor. El reenvío de identidad permite que los archivos de ACL retroactiva VHDx al igual que en configuraciones estándar clústeres como antes.

Una vez que se crea un conjunto de clúster, el espacio de nombres del conjunto de clúster se basa en un SOFS de infraestructura en cada uno de los clústeres de miembro y, además, un SOFS infraestructura del clúster de administración.

En el momento de un clúster de miembro se agrega a un conjunto de clúster, el administrador especifica el nombre de una infraestructura de SOFS en ese clúster si ya existe una. Si los SOFS infraestructura no existe, se crea una nueva función de infraestructura SOFS en el nuevo clúster de miembros por esta operación. Si ya existe una función de infraestructura SOFS en el clúster de miembro, la operación de agregar implícitamente cambia su nombre para el nombre especificado, según sea necesario. Cualquier servidores SMB de singleton existentes, o roles que no sean infraestructura SOFS en el miembro quedan clústeres no utilizado por el conjunto de clúster. 

En el momento en que se creó el conjunto de clúster, el administrador tiene la opción de usar un objeto de equipo de AD ya existente como la raíz de espacio de nombres en el clúster de administración. Las operaciones de creación del conjunto de clúster crear el rol de clúster de infraestructura SOFS en el clúster de administración o cambia el nombre de la función SOFS infraestructura existente solo según lo descrita anteriormente para clústeres de miembro. Los SOFS infraestructura en el clúster de administración se usa como el clúster establece referencias de espacio de nombres (clúster establece Namespace) SOFS. Esto significa que cada recurso compartido de SMB en el clúster configurado SOFS es un recurso compartido de referencia – de tipo 'SimpleReferral' - recién introducido en esta versión preliminar del espacio de nombres.  Esta referencia permite el acceso de los clientes SMB en el destino de recurso compartido SMB alojado en el clúster de miembro SOFS. El clúster establece referencias de espacio de nombres SOFS es un mecanismo de referencia ligera y por lo tanto, no participa en la ruta de acceso de E/S. Se almacenan en caché las referencias de SMB permanente en cada uno de los nodos del cliente y el espacio de nombres de conjuntos de clúster dinámicamente actualiza automáticamente estas referencias según sea necesario

## Crear un conjunto de clúster

### Requisitos previos

Cuando se establece la creación de un clúster, se recomienda seguir los requisitos previos:

1. Configurar a un cliente de administración que ejecutan la versión más reciente de Windows Server Insider.
2. Instala las herramientas de clúster de conmutación por error en este servidor de administración.
3. Crear a los miembros del clúster (al menos dos clústeres con al menos dos volúmenes compartidos de clúster en cada clúster)
4. Crear un clúster de administración (físico o de invitado) que incluya los clústeres de miembro.  Este enfoque garantiza que los conjuntos de clúster seguirá estando disponible a pesar de los errores de clúster de miembro posible plano de administración.

### Pasos

1. Crear un nuevo clúster de conjunto de clústeres de tres según se define en los requisitos previos.  El siguiente gráfico proporciona un ejemplo de clústeres de crear.  El nombre del clúster establecido en este ejemplo será **CSMASTER**.

   | Nombre de clúster               | Infraestructura SOFS nombre que se usará más adelante | 
   |----------------------------|-------------------------------------------|
   | CONJUNTO DE CLÚSTER                | SOFS CLUSTERSET                           |
   | CLUSTER1                   | SOFS CLUSTER1                             |
   | CLUSTER2                   | SOFS CLUSTER2                             |

2. Una vez que se han creado todos los clústeres, usa los siguientes comandos para crear el clúster principal conjunto.

        New-ClusterSet -Name CSMASTER -NamespaceRoot SOFS-CLUSTERSET -CimSession SET-CLUSTER

3. Para agregar un servidor de clúster a clúster establecido, el a continuación se usaría.

        Add-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER1
        Add-ClusterSetMember -ClusterName CLUSTER2 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER2

   > [!NOTE]
   > Si estás usando un esquema de dirección IP estático, debes incluir *x.x.x.x - StaticAddress* en el comando **New-ClusterSet** .

4. Una vez que hayas creado el clúster establecido fuera de los miembros del clúster, puede enumerar el conjunto de nodos y sus propiedades.  Enumerar todos los clústeres de miembro en el conjunto de clúster:

        Get-ClusterSetMember -CimSession CSMASTER

5. Enumerar todos los clústeres de miembro en el conjunto incluidos los nodos de clúster de administración de clústeres:

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterNode

6. Para enumerar todos los nodos de los clústeres de miembros:

        Get-ClusterSetNode -CimSession CSMASTER

7. Para enumerar todos los grupos de recursos en el conjunto de clúster:

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterGroup 

8. Para comprobar el clúster establece proceso de creación de crear un recurso compartido SMB (identificado como Volume1 o cualquier la carpeta CSV se etiqueta con la ScopeName está el nombre del servidor de archivos de infraestructura y la ruta de acceso como) en los SOFS de infraestructura para cada miembro de clúster Volumen CSV:

        Get-SmbShare -CimSession CSMASTER

8. Conjuntos de clúster tiene que se pueden recopilar para revisar los registros de depuración.  Tanto la configuración del clúster y se pueden recopilar los registros de depuración de clúster para todos los miembros y el clúster de administración.

        Get-ClusterSetLog -ClusterSetCimSession CSMASTER -IncludeClusterLog -IncludeManagementClusterLog -DestinationFolderPath <path>

9. Configurar Kerberos [delegación restringida](https://blogs.technet.microsoft.com/virtualization/2017/02/01/live-migration-via-constrained-delegation-with-kerberos-in-windows-server-2016/) entre todos los miembros del conjunto de clúster.

10. Configurar el tipo de autenticación de la migración en vivo de máquina virtual de clúster de entre a Kerberos en cada nodo en el conjunto de clúster.

        foreach($h in $hosts){ Set-VMHost -VirtualMachineMigrationAuthenticationType Kerberos -ComputerName $h }

11. Agregar el clúster de administración al grupo Administradores local en cada nodo en el conjunto de clúster.

        foreach($h in $hosts){ Invoke-Command -ComputerName $h -ScriptBlock {Net localgroup administrators /add <management_cluster_name>$} }

## Creación de nuevas máquinas virtuales y agrega a conjuntos de clúster

Después de crear el clúster, el siguiente paso es crear nuevas máquinas virtuales.  Normalmente, cuando es el momento de creación de máquinas virtuales y agregarlos a un clúster, debes hacer algunas comprobaciones en los clústeres para ver qué, quizás sea mejor que se ejecute en.  Estos controles se incluyen:

- ¿Cuánta memoria está disponible en los nodos del clúster?
- ¿Cuánto espacio está disponible en los nodos del clúster?
- La máquina virtual requiere que los requisitos de almacenamiento específico (es decir, quiero Mis máquinas virtuales de SQL Server para ir a un clúster que ejecuta unidades más rápidas; o bien, mi máquina virtual de infraestructura no es tan importante y puede ejecutarse en unidades más lentas).

Una vez que esta preguntas, crear la máquina virtual en el clúster que necesitas para que sea.  Una de las ventajas de los conjuntos de clúster es que los conjuntos de clúster hacer las comprobaciones y colocar la máquina virtual en el nodo óptimo.

Los comandos siguientes ambos identificará el clúster óptimo e implementar la máquina virtual a ella.  En el ejemplo siguiente, se crea una nueva máquina virtual especifica que está disponible para la máquina virtual al menos 4 gigabytes de memoria y que deberá usar 1 procesador virtual.

- Asegúrate de que está disponible para la máquina virtual de 4gb
- establece el procesador virtual que se usan en 1
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

Cuando se completa, se le ofrecerá la información sobre la máquina virtual y donde se colocó.  En el ejemplo anterior, se mostraría como:

        State         : Running
        ComputerName  : 1-S2D2

Si tuviera que no tienen suficiente memoria, cpu o espacio en disco para agregar la máquina virtual, recibirás el error:

      Get-ClusterSetOptimalNodeForVM : A cluster node is not available for this operation.  

Una vez que se ha creado la máquina virtual, se mostrará en el Administrador de Hyper-V en el nodo específico especificado.  Para agregarlo como un clúster de conjunto de máquina virtual y en el clúster, el comando es a continuación.  

        Register-ClusterSetVM -CimSession CSMASTER -MemberName $targetnode.Member -VMName CSVM1

Cuando se completa, el resultado será:

         Id  VMName  State  MemberName  PSComputerName
         --  ------  -----  ----------  --------------
          1  CSVM1      On  CLUSTER1    CSMASTER

Si has agregado un clúster con las máquinas virtuales existentes, las máquinas virtuales que también se registre en clúster conjuntos, por lo tanto, registrar todas las máquinas virtuales a la vez, el comando para usar es:

        Get-ClusterSetMember -name CLUSTER3 -CimSession CSMASTER | Register-ClusterSetVM -RegisterAll -CimSession CSMASTER

Sin embargo, el proceso no está completado, ya que la ruta de acceso a la máquina virtual debe agregarse al espacio de nombres del conjunto de clúster.

Por lo tanto, por ejemplo, se agrega un clúster existente y tiene máquinas virtuales preconfiguradas ubican en el volumen de compartidos de clúster de local (CSV), la ruta de acceso para el VHDX sería algo parecido a "C:\ClusterStorage\Volume1\MYVM\Virtual Disks\MYVM.vhdx de disco duro.  Una migración de almacenamiento sería necesario llevar a cabo como rutas de acceso CSV están por diseño local a un clúster único miembro. Por lo tanto, no serán accesibles en la máquina virtual una vez que son dinámicos migró a través de clústeres de miembro. 

En este ejemplo, se ha agregado CLUSTER3 al clúster establecido mediante Agregar ClusterSetMember con el servidor de archivos de escalabilidad horizontal de infraestructura como SOFS CLUSTER3.  Para mover la configuración de máquina virtual y el almacenamiento, el comando es:

        Move-VMStorage -DestinationStoragePath \\SOFS-CLUSTER3\Volume1 -Name MYVM

Una vez que se completa, recibirás una advertencia:

        WARNING: There were issues updating the virtual machine configuration that may prevent the virtual machine from running.  For more information view the report file below.
        WARNING: Report file location: C:\Windows\Cluster\Reports\Update-ClusterVirtualMachineConfiguration '' on date at time.htm.

Esta advertencia puede omitirse que sea la advertencia "se han detectado ningún cambio en la configuración de almacenamiento de rol de máquina virtual".  El motivo de la advertencia como la ubicación física real no cambia; las rutas de configuración. 

Para obtener más información sobre el movimiento VMStorage, revisa este [vínculo](https://docs.microsoft.com/powershell/module/hyper-v/move-vmstorage?view=win10-ps). 

Dinámicos de migración de una máquina virtual entre clústeres de conjunto de otro clúster es no igual que en el pasado. En escenarios que no sean de clúster establecido, sería los pasos:

1. quitar el rol de máquina virtual del clúster.
2. migración en vivo la máquina virtual a un nodo de miembro de un clúster diferente.
3. agregar la máquina virtual en el clúster como un nuevo rol de máquina virtual.

Con conjuntos de clúster estos pasos no son necesarios y se necesita un solo comando.  Por ejemplo, quiero mover una máquina virtual de conjunto de clústeres de CLUSTER1 a nodo 2 CL3 en CLUSTER3.  El único comando sería:

        Move-ClusterSetVM -CimSession CSMASTER -VMName CSVM1 -Node NODE2-CL3

Ten en cuenta que no se mueve los archivos de almacenamiento o la configuración de la máquina virtual.  Esto no es necesario que la ruta de acceso a la máquina virtual se mantenga como \\SOFS-CLUSTER1\VOLUME1.  Una vez que se haya registrado una máquina virtual con conjuntos de clúster tiene la ruta de acceso de recurso compartido de servidor de archivos de infraestructura, las unidades y la máquina virtual no necesitan estar en el mismo equipo que la máquina virtual.

## Disponibilidad de creación de conjuntos de dominios de error

Como se describe en la introducción, se pueden configurar los dominios de error similar de Azure y los conjuntos de disponibilidad en un conjunto de clúster.  Esto resulta útil para las ubicaciones de la máquina virtual inicial y las migraciones entre clústeres.  

En el siguiente ejemplo, hay cuatro clústeres que participan en el conjunto de clúster.  En el conjunto, se creará un dominio de error lógico con dos de los clústeres y un dominio de error que se haya creado con los otros dos clústeres.  Estos dominios de dos error incluirán el conjunto de Availabiilty. 

En el ejemplo siguiente, CLUSTER1 y CLUSTER2 estará en un dominio de error llamado **FD1** mientras CLUSTER3 y CLUSTER4 estará en un dominio de error que se denomina **FD2**.  El conjunto de disponibilidad se llamará **CSMASTER-AS** y comprender los dominios de error de dos.

Para crear los dominios de error, los comandos son:

        New-ClusterSetFaultDomain -Name FD1 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER1,CLUSTER2 -Description "This is my first fault domain"

        New-ClusterSetFaultDomain -Name FD2 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER3,CLUSTER4 -Description "This is my second fault domain"

Para garantizar que se han creado correctamente, se puede ejecutar Get-ClusterSetFaultDomain con su salida que se muestra.

        PS C:\> Get-ClusterSetFaultDomain -CimSession CSMASTER -FdName FD1 | fl *

        PSShowComputerName    : True
        FaultDomainType       : Logical
        ClusterName           : {CLUSTER1, CLUSTER2}
        Description           : This is my first fault domain
        FDName                : FD1
        Id                    : 1
        PSComputerName        : CSMASTER

Ahora que se han creado los dominios de error, la disponibilidad establece deben crearse.

        New-ClusterSetAvailabilitySet -Name CSMASTER-AS -FdType Logical -CimSession CSMASTER -ParticipantName FD1,FD2

Para validar se ha creado, a continuación, utilizar:

        Get-ClusterSetAvailabilitySet -AvailabilitySetName CSMASTER-AS -CimSession CSMASTER

Al crear nuevas máquinas virtuales, deberá usar el parámetro - AvailabilitySet como parte de determinar el nodo óptimo.  Por lo que, a continuación, sería similar al siguiente:

        # Identify the optimal node to create a new virtual machine
        $memoryinMB=4096
        $vpcount = 1
        $av = Get-ClusterSetAvailabilitySet -Name CSMASTER-AS -CimSession CSMASTER
        $targetnode = Get-ClusterSetOptimalNodeForVM -CimSession CSMASTER -VMMemory $memoryinMB -VMVirtualCoreCount $vpcount -VMCpuReservation 10 -AvailabilitySet $av
        $secure_string_pwd = convertto-securestring "<password>" -asplaintext -force
        $cred = new-object -typename System.Management.Automation.PSCredential ("<domain\account>",$secure_string_pwd)

Quitar un clúster de clúster establece debido a diversas ciclos de vida. Hay ocasiones en que un clúster debe de quitarla de un conjunto de clúster. Como procedimiento recomendado, todas las máquinas virtuales de conjunto de clúster debe ser trasladadas fuera del clúster. Esto puede hacerse con los comandos de **Movimiento ClusterSetVM** y **VMStorage de movimiento** .

Sin embargo, si las máquinas virtuales no se moverán también, conjuntos de clúster se ejecuta una serie de acciones para proporcionar un resultado intuitivo al administrador.  Cuando el clúster se quita del conjunto, todas las restantes clúster conjunto de máquinas virtuales alojadas en el clúster que se está quitando simplemente se convertirá en máquinas virtuales altamente disponibles enlazadas a dicho clúster, suponiendo que tienen acceso a su almacenamiento.  Conjuntos de clúster actualizarán automáticamente su inventario por:

- Ya no realizar el seguimiento del estado de clúster quitado ahora y las máquinas virtuales que se está ejecutando
- Quita del espacio de nombres de conjunto de clúster y todas las referencias a recursos compartidos alojados en el clúster quitado ahora

Por ejemplo, el comando para quitar el clúster CLUSTER1 de conjuntos de clúster sería:

        Remove-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER

## Preguntas frecuentes (FAQ)

**Pregunta:** ¿En mi conjunto de clúster, he limitado a usar únicamente clústeres hiperconvergidos? <br>
**Respuesta:** No.  Se pueden mezclar espacios de almacenamiento directo con clústeres tradicionales.

**Pregunta:** ¿Puedo administrar mi conjunto de clústeres a través de System Center Virtual Machine Manager? <br>
**Respuesta:** System Center Virtual Machine Manager no admite actualmente los conjuntos de clúster <br><br> **Pregunta:** ¿Pueden Windows Server 2012 R2 o clústeres de 2016 coexistir en el mismo conjunto de clúster? <br>
**Pregunta:** ¿Puedo migrar las cargas de trabajo desactivar Windows Server 2012 R2 o 2016 clústeres por tener simplemente los clústeres de unir el mismo conjunto de clústeres? <br>
**Respuesta:** Conjuntos de clúster es una nueva tecnología que se introdujo en Windows Server Preview compilaciones, por lo tanto, por lo tanto, no existe en versiones anteriores. Clústeres de nivel inferior en función del sistema operativo no pueden unirse a un conjunto de clúster. Sin embargo, la tecnología de actualizaciones sucesiva de sistema operativo del clúster debe proporcionar la funcionalidad de migración que estás buscando actualizando estos clústeres a Windows Server 2019.

**Pregunta:** ¿Pueden conjuntos de Clústerme permite escalar de almacenamiento o calcular (solamente)? <br>
**Respuesta:** Sí, simplemente agregando un espacio de almacenamiento directo o el clúster de Hyper-V tradicional. Con conjuntos de clúster, es un sencillo cambio de relación de cálculo para almacenamiento incluso en un conjunto de clústeres.

**Pregunta:** ¿Qué es la herramienta de administración para conjuntos de clúster <br>
**Respuesta:** PowerShell o WMI en esta versión.

**Pregunta:** ¿Cómo funcionará la migración en vivo entre clústeres con procesadores de distintas generaciones?  <br>
**Respuesta:** Conjuntos de clústeres no funciona con diferencias de procesador y sustituir a lo que Hyper-V actualmente admite.  Por lo tanto, se debe usar el modo de compatibilidad de procesador con migraciones rápidas.  La recomendación para conjuntos de clúster es usar el mismo hardware de procesador en cada clúster individual, así como el conjunto completo de clúster para las migraciones entre clústeres que se produzca.

**Pregunta:** ¿Puede mi conjunto de clúster virtual de máquinas automáticamente en un error de clúster de conmutación por error?  <br>
**Respuesta:** En esta versión, las máquinas virtuales de conjunto de clúster solo puede ser manualmente live migrados entre clústeres; pero no automáticamente la conmutación por error. 

**Pregunta:** ¿Cómo garantizamos almacenamiento es resistente a errores de clúster? <br>
**Respuesta:** Usar soluciones de réplica de almacenamiento (SR) entre clúster en clústeres de miembro tener en cuenta la resistencia de almacenamiento a errores de clúster.

**Pregunta:** Puedo usar réplica de almacenamiento (SR) para replicar entre clústeres de miembro. ¿Clúster almacenamiento de espacio de nombres de conjunto de cambio de rutas de acceso UNC en SR conmutación por error para el clúster de espacios de almacenamiento directo de destino de réplica? <br>
**Respuesta:** En esta versión, este clúster conjunto de espacio de nombres referencia cambio no ocurre con SR conmutación por error. Por favor, informar a Microsoft si este escenario es fundamental para usted y cómo pretende usarla.

**Pregunta:** ¿Es posible que las máquinas virtuales de conmutación por error a través de los dominios de error en una situación de recuperación ante desastres (por ejemplo, el dominio de error todo ha funcionado hacia abajo)? <br>
**Respuesta:** No, ten en cuenta que el clúster de conmutación por error dentro de un error de lógico dominio aún no se admite. 

**Pregunta:** ¿Mi clúster establecer span clústeres en varios sitios (o los dominios DNS)? <br> 
**Respuesta:** Esto es un escenario no se han probado y no inmediatamente prevista para soporte de producción. Por favor, informar a Microsoft si este escenario es fundamental para usted y cómo pretende usarla.

**Pregunta:** ¿Conjunto de trabajo con IPv6 clústeres? <br>
**Respuesta:** IPv4 e IPv6 son compatibles con conjuntos de clúster al igual que con los clústeres de conmutación por error.

**Pregunta:** ¿Cuáles son los requisitos de bosque de Active Directory de clúster establece <br>
**Respuesta:** Todos los clústeres de miembro deben estar en el mismo bosque de AD.

**Pregunta:** ¿Cuántos clústeres o nodos puede parte de un único clúster establecerse? <br>
**Respuesta:** En la vista previa de conjuntos de clústeres se han probado y admite hasta 64 nodos del clúster total. Sin embargo, clúster establece la arquitectura escala mucho mayor límites y no es algo que es codificados de forma rígida de un límite. Por favor, informar a Microsoft si escala más grande es fundamental para usted y cómo pretende usarla.

**Pregunta:** ¿Todos los clústeres de espacios de almacenamiento directo en un conjunto de clúster forman un grupo de almacenamiento único? <br>
**Respuesta:** No. Tecnología de espacios de almacenamiento funciona aún dentro de un único clúster y no a través de clústeres de miembro en un conjunto de clúster.

**Pregunta:** ¿Es la configuración del clúster alta disponibilidad del espacio de nombres? <br>
**Respuesta:** Sí, se proporciona el espacio de nombres del conjunto de clúster a través de un servidor de espacio de nombres SOFS de referencia de continuamente disponibles (CA) que se ejecutan en el clúster de administración. Microsoft recomienda tener suficiente número de máquinas virtuales de clústeres de miembro para que sea resistente a errores de todo el clúster localizados. Sin embargo, para tener en cuenta catástrofes imprevistas: por ejemplo, todas las máquinas virtuales en el clúster de administración deja de funcionar al mismo tiempo, la información de referencia es además de forma persistente en la memoria caché en cada nodo del conjunto de clúster, incluso los reinicios.
 
**Pregunta:** ¿El clúster configurar el acceso de almacenamiento basado en el espacio de nombres ralentizar el rendimiento de almacenamiento en un conjunto de clúster? <br>
**Respuesta:** No. Espacio de nombres de conjunto de clúster ofrece un espacio de nombres de referencia de superposición dentro de un conjunto de clúster: conceptualmente como archivo sistema distribuido espacios de nombres (DFSN). Y, a diferencia de DFSN y todos los metadatos de referencia de espacio de nombres de conjunto de clúster son rellenan automáticamente y se actualiza automáticamente en todos los nodos sin la intervención del administrador, por lo que no hay prácticamente ninguna sobrecarga de rendimiento en la ruta de acceso de almacenamiento. 

**Pregunta:** ¿Cómo puedo backup metadatos del conjunto de clúster? <br>
**Respuesta:** Esta guía es el mismo que del clúster de conmutación por error. La copia de seguridad del sistema estado realizará copia de seguridad, así como el estado del clúster.  A través de la copia de seguridad de Windows Server, puedes hacer restaura de solo de un nodo clúster base de datos (que nunca debería ser necesario debido a un montón de recuperación automática lógica tenemos) o realizar una restauración autorizada para revertir la base de datos de todo el clúster en todos los nodos. En el caso de los conjuntos de clúster, Microsoft recomienda hacer tales una restauración en primer lugar en el miembro y, a continuación, la administración clúster si es necesario. 
