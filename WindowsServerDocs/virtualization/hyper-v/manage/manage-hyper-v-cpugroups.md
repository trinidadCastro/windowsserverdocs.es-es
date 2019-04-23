---
title: Controles de recursos de máquina virtual
description: Uso de grupos de CPU de máquina virtual
keywords: windows 10, hyper-v
author: allenma
ms.date: 06/18/2018
ms.topic: article
ms.prod: windows-10-hyperv
ms.service: windows-10-hyperv
ms.assetid: cc7bb88e-ae75-4a54-9fb4-fc7c14964d67
ms.openlocfilehash: 7c4ddf3e5d2ff58eef844c50960327c27a3e0a3d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854766"
---
>Se aplica a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server, Microsoft Hyper-V Server 2019 de 2019

# <a name="virtual-machine-resource-controls"></a>Controles de recursos de máquina virtual

En este artículo se describe los controles de recursos y el aislamiento de Hyper-V para máquinas virtuales.  Estas capacidades, que nos referiremos a como grupos de CPU de la máquina Virtual, o simplemente "grupos de CPU", se introdujeron en Windows Server 2016.  Grupos de CPU permiten a los administradores de Hyper-V mejor administrar y asignan recursos de CPU del host a través de las máquinas virtuales invitadas.  Uso de grupos de CPU, los administradores de Hyper-V pueden:

* Crear grupos de máquinas virtuales, con cada grupo tiene distintas asignaciones de recursos de CPU total del host de virtualización, compartidas en todo el grupo. Esto permite al administrador de host implementar las clases de servicio para los distintos tipos de máquinas virtuales.

* Establezca los límites de recursos de CPU en grupos específicos. Este límite de"grupo" establece el límite superior para el host de los recursos de CPU que puede consumir todo el grupo, aplicar eficazmente la clase deseada del servicio para ese grupo.

* Restringir un grupo de CPU para ejecutar solo en un conjunto específico de procesadores del sistema host. Esto puede usarse para aislar las máquinas virtuales que pertenecen a diferentes grupos de CPU entre sí.

## <a name="managing-cpu-groups"></a>Administración de grupos de CPU

Grupos de CPU se administran a través del servicio de proceso de Host de Hyper-V o HCS. Una excelente descripción de la HCS, su genesis, hay vínculos a las API de HCS y mucho más en el Microsoft Virtualization blog del equipo en el registro [introducir el servicio de proceso de Host (HCS)](https://blogs.technet.microsoft.com/virtualization/2017/01/27/introducing-the-host-compute-service-hcs/).

>[!NOTE] 
>Solo el HCS puede utilizarse para crear y administrar grupos de CPU; applet Administrador de Hyper-V, las interfaces de administración de WMI y PowerShell no son compatibles con grupos de CPU.

Microsoft proporciona una línea de comandos utilidad, cpugroups.exe, en el [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=865968) que utiliza la interfaz HCS para administrar grupos de CPU.  Esta utilidad también puede mostrar la topología de la CPU de un host.

## <a name="how-cpu-groups-work"></a>Cómo funcionan los grupos de CPU

Se aplica la asignación de recursos de proceso de host a través de grupos de CPU por el hipervisor de Hyper-V, con un límite de grupo de CPU calculado. El límite de grupo de la CPU es una fracción de la capacidad total de CPU para un grupo de CPU. El valor del extremo del grupo depende de la clase de grupo o asignado. El límite de grupo calculada puede considerarse como "número de LP's que vale la pena de tiempo de CPU". Este presupuesto del grupo es compartido, por lo que si solo una sola máquina virtual estaban activos, podría usar la asignación de CPU de todo el grupo por sí misma.

El límite de grupo de CPU se calcula como G = *n* x *C*, donde:

    *G* is the amount of host LP we'd like to assign to the group
    *n* is the total number of logical processors (LPs) in the group
    *C* is the maximum CPU allocation — that is, the class of service desired for the group, expressed as a percentage of the system’s total compute capacity

Por ejemplo, considere la posibilidad de un grupo de CPU configurado con 4 procesadores lógicos (LPs) y un límite de 50%.

    G = n * C
    G = 4 * 50%
    G = 2 LP's worth of CPU time for the entire group

En este ejemplo, está asignado por el grupo de CPU G 2 LP's que vale la pena de tiempo de CPU.  

Tenga en cuenta que el límite de grupo se aplica independientemente del número de máquinas virtuales o enlazados al grupo de procesadores virtuales y con independencia del estado (por ejemplo, apagado o iniciado) de las máquinas virtuales asignadas al grupo de CPU. Por lo tanto, cada máquina virtual enlazada al mismo grupo de CPU, recibirán una fracción de la asignación de CPU total del grupo, y esta operación cambiará con el número de máquinas virtuales que se enlaza al grupo de CPU. Por lo tanto, como máquinas virtuales están enlazadas o desenlaza las máquinas virtuales a un grupo de CPU, el límite de grupo general de CPU debe reajustará y establecido para mantener el límite por máquina virtual resultante deseado. La administración de administrador o virtualización de host de máquina virtual es responsable de administrar los límites de grupo según sea necesario para lograr la asignación de recursos de CPU deseado por máquina virtual la capa de software.

## <a name="example-classes-of-service"></a>Clases de ejemplo de servicio

Echemos un vistazo a algunos ejemplos sencillos. Para empezar, supongamos que desea que el administrador del host de Hyper-V admitir dos niveles de servicio para las máquinas virtuales invitadas:

1. Un nivel de "C" low-end. Se asignará a este nivel 10% de los recursos de proceso del host completo.

1. Un nivel de gama media "B". Este nivel se asigna el 50% de los recursos de proceso del host completo.

En este momento en nuestro ejemplo se a afirmar que ningún otros los controles de recursos de CPU están en uso, como extremos de máquina virtual individuales, ponderaciones y se reserva.
Sin embargo, los límites de máquina virtual individuales son importantes, como veremos más adelante.

Para las cosas simplificar, supongamos que cada máquina virtual tiene 1 VP y que nuestro host tiene 8 LPs. Comenzaremos con un host vacío.

Para crear el nivel "B", el host de adminstartor establece el límite de grupo en el 50%:

    G = n * C
    G = 8 * 50%
    G = 4 LP's worth of CPU time for the entire group

El administrador del host agrega una única máquina virtual de nivel "B".
En este momento, nuestro nivel "B" máquina virtual puede usar a lo sumo que vale la pena el 50% de CPU del host o el equivalente de 4 LPs en nuestro sistema de ejemplo.

Ahora, el administrador agrega un segundo "nivel B" máquina virtual. Asignación del grupo de la CPU, se divide uniformemente entre todas las máquinas virtuales. Tenemos un total de 2 máquinas virtuales en el grupo B, por lo que cada máquina virtual ahora obtiene la mitad del total del grupo B de 50%, 25% o el equivalente de 2 LPs vale la pena de tiempo de proceso.

## <a name="setting-cpu-caps-on-individual-vms"></a>Configurar los límites de CPU en las máquinas virtuales individuales

Además el límite de grupo, cada máquina virtual puede tener también un individuales "extremo de máquina virtual". Los controles de recursos de CPU por máquina virtual, incluido el límite de CPU, peso y reserva, han sido una parte de Hyper-V desde su presentación.
Cuando se combina con un límite de grupo, un extremo de máquina virtual especifica la cantidad máxima de CPU que puede llegar a cada VP, incluso si el grupo tiene los recursos de CPU disponibles.

Por ejemplo, el administrador del host desea colocar un extremo de máquina virtual de 10% en máquinas virtuales de "C".
De este modo, aunque la mayoría VPs "C" están inactivos, cada VP no podría obtener nunca más del 10%.
Sin un límite de la máquina virtual, máquinas virtuales de "C" según la ocasión podrían lograr un rendimiento más allá de niveles permitido por su nivel.

## <a name="isolating-vm-groups-to-specific-host-processors"></a>Aislar los grupos de máquinas virtuales a los procesadores de Host específico

Los administradores del host de Hyper-V también deseará poder dedicar los recursos de proceso a una máquina virtual.
Por ejemplo, imagine que el administrador quería ofrecer una prima "A" máquina virtual que tiene un límite de clase del 100%.
Estas máquinas virtuales de premium también requieren la programación menor latencia y vibración posibles; es decir, no es pueden anular programar por cualquier otra máquina virtual.
Para lograr esta separación, también puede configurarse un grupo de la CPU con una asignación de afinidad determinada LP.

Por ejemplo, para ajustarse a una máquina virtual "A" en el host en nuestro ejemplo, el administrador podría crear un nuevo grupo de CPU y establecer la afinidad del procesador del grupo a un subconjunto de LPs del host.
Los grupos B y C tendrían el LPs restantes.
El administrador podría crear una única VM en grupo A, que tendría acceso exclusivo a LPs todas en un grupo, mientras que los grupos de nivel inferiores supuestamente B y C compartirían la LPs restantes.

## <a name="segregating-root-vps-from-guest-vps"></a>Separación VPs raíz desde VPs de invitado

De forma predeterminada, Hyper-V creará un Vicepresidente de raíz en cada LP físico subyacente.
Estos VPs raíz son estrictamente asignado 1:1 con el sistema de LPs y no se migran, es decir, cada raíz VP siempre se ejecutará en el mismo LP físico.
Invitado VPs se pueden ejecutar en cualquier LP disponible y compartirán la ejecución con raíz VPs.

Sin embargo, sería conveniente a raíz de forma totalmente independiente actividad Vicepresidente de VPs de invitado.
Considere el ejemplo anterior donde se implementa una máquina virtual de nivel de premium "A".
Para asegurarse de que VPs "A" la máquina virtual tienen la menor latencia posible y "vibración" o la variación de programación, nos gustaría ejecutar en un conjunto dedicado de LPs y asegúrese de que la raíz no se ejecuta en estas LPs.

Esto puede realizarse mediante una combinación de la configuración de "minroot", lo que limita la partición del sistema operativo host para que se ejecutan en un subconjunto de los procesadores lógicos total del sistema, junto con uno o más afinidad con grupos de CPU.

El host de virtualización puede configurarse para restringir la partición del host a LPs específicos, con uno o varios grupos de CPU con afinidad con los restantes LPs.
De esta manera, las particiones raíz y el invitado se pueden ejecutar en los recursos de CPU dedicados y completamente aislada sin uso compartido de CPU.

Para obtener más información sobre la configuración de "minroot", consulte [administración de recursos de CPU de Host de Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-minroot-2016).

## <a name="using-the-cpugroups-tool"></a>Mediante la herramienta CpuGroups

Echemos un vistazo a algunos ejemplos de cómo usar la herramienta CpuGroups.

>[!NOTE] 
>Se pasan los parámetros de línea de comandos para la herramienta CpuGroups utilizando solamente espacios como delimitadores. No '/' o '-' caracteres deben continuar el modificador de línea de comandos deseadas.

### <a name="discovering-the-cpu-topology"></a>Detectar la topología de la CPU

Ejecutar CpuGroups con el GetCpuTopology devuelve información sobre el sistema actual, tal como se muestra a continuación, incluido el índice LP, el nodo NUMA al que pertenece el LP, el paquete y los identificadores de núcleo y el índice de Vicepresidente de raíz.

El ejemplo siguiente muestra un sistema con 2 sockets de la CPU y nodos NUMA, un total de 32 LPs y subprocesamiento múltiple habilitado y configuran para permitir que Minroot con 8 raíz VPs, 4 de cada nodo NUMA.
El LPs que tienen VPs raíz tienen un RootVpIndex > = 0; LPs con un RootVpIndex de -1 no están disponibles para la partición raíz, pero se siguen administrando el hipervisor y ejecutarán el invitado VPs según lo permitido por otras opciones de configuración.

```console
C:\vm\tools>CpuGroups.exe GetCpuTopology

LpIndex NodeNumber PackageId CoreId RootVpIndex
------- ---------- --------- ------ -----------
      0          0         0      0           0
      1          0         0      0           1
      2          0         0      1           2
      3          0         0      1           3
      4          0         0      2          -1
      5          0         0      2          -1
      6          0         0      3          -1
      7          0         0      3          -1
      8          0         0      4          -1
      9          0         0      4          -1
     10          0         0      5          -1
     11          0         0      5          -1
     12          0         0      6          -1
     13          0         0      6          -1
     14          0         0      7          -1
     15          0         0      7          -1
     16          1         1     16           4
     17          1         1     16           5
     18          1         1     17           6
     19          1         1     17           7
     20          1         1     18          -1
     21          1         1     18          -1
     22          1         1     19          -1
     23          1         1     19          -1
     24          1         1     20          -1
     25          1         1     20          -1
     26          1         1     21          -1
     27          1         1     21          -1
     28          1         1     22          -1
     29          1         1     22          -1
     30          1         1     23          -1
     31          1         1     23          -1
```

### <a name="example-2--print-all-cpu-groups-on-the-host"></a>Ejemplo 2: imprimir todos los grupos de CPU del host

En este caso, también tocaremos todos los grupos de CPU en el host actual, su GroupId, límite de CPU del grupo y los índices de LPs asignados a ese grupo.

Tenga en cuenta que los valores de límite de CPU válidos están en el intervalo [0, 65536], y estos valores express el límite de grupo en porcentaje (por ejemplo, 32768 = 50%).

```console
C:\vm\tools>CpuGroups.exe GetGroups

CpuGroupId                          CpuCap  LpIndexes
------------------------------------ ------ --------
36AB08CB-3A76-4B38-992E-000000000002 32768  4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536  24,25,26,27,28,29,30,31
```

### <a name="example-3--print-a-single-cpu-group"></a>Ejemplo 3: imprimir un único grupo de CPU

En este ejemplo, se consulta un único grupo de CPU mediante GroupId como filtro.

```console
C:\vm\tools>CpuGroups.exe GetGroups /GroupId:36AB08CB-3A76-4B38-992E-000000000003
CpuGroupId                          CpuCap   LpIndexes
------------------------------------ ------ ----------
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
```

### <a name="example-4--create-a-new-cpu-group"></a>Ejemplo 4: crear un nuevo grupo de CPU

En este caso, vamos a crear un nuevo grupo de CPU, especificando el identificador de grupo y el conjunto de LPs para asignar al grupo.

```console
C:\vm\tools>CpuGroups.exe CreateGroup /GroupId:36AB08CB-3A76-4B38-992E-000000000001 /GroupAffinity:0,1,16,17
```

Mostrar ahora nuestro grupo recién agregado.

```console
C:\vm\tools>CpuGroups.exe GetGroups
CpuGroupId                          CpuCap LpIndexes
------------------------------------ ------ ---------
36AB08CB-3A76-4B38-992E-000000000001 65536 0,1,16,17
36AB08CB-3A76-4B38-992E-000000000002 32768 4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536 12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536 24,25,26,27,28,29,30,31
```

### <a name="example-5--set-the-cpu-group-cap-to-50"></a>Ejemplo 5: establecer el límite de grupo de CPU al 50%

En este caso, también se establece el límite de grupo de CPU al 50%.

```console
C:\vm\tools>CpuGroups.exe SetGroupProperty /GroupId:36AB08CB-3A76-4B38-992E-000000000001 /CpuCap:32768
```

Ahora vamos a confirmar nuestra configuración mostrando el grupo que acaba de actualizar.

```console
C:\vm\tools>CpuGroups.exe GetGroups /GroupId:36AB08CB-3A76-4B38-992E-000000000001

CpuGroupId                          CpuCap LpIndexes
------------------------------------ ------ ---------
36AB08CB-3A76-4B38-992E-000000000001 32768 0,1,16,17
```

### <a name="example-6--print-cpu-group-ids-for-all-vms-on-the-host"></a>Ejemplo 6: los identificadores de grupo de CPU de impresión para todas las máquinas virtuales en el host

```console
C:\vm\tools>CpuGroups.exe GetVmGroup

VmName                                 VmId                           CpuGroupId
------ ------------------------------------ ------------------------------------
    G2 4ABCFC2F-6C22-498C-BB38-7151CE678758 36ab08cb-3a76-4b38-992e-000000000002
    P1 973B9426-0711-4742-AD3B-D8C39D6A0DEC 36ab08cb-3a76-4b38-992e-000000000003
    P2 A593D93A-3A5F-48AB-8862-A4350E3459E8 36ab08cb-3a76-4b38-992e-000000000004
    G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200 36ab08cb-3a76-4b38-992e-000000000002
    G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC 36ab08cb-3a76-4b38-992e-000000000002
```

### <a name="example-7--unbind-a-vm-from-the-cpu-group"></a>Ejemplo 7: desenlazar una máquina virtual desde el grupo de CPU

Para quitar una máquina virtual de un grupo de CPU, establezca CpuGroupId de la máquina virtual en el GUID de NULL. Esto desenlaza la máquina virtual desde el grupo de CPU.

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:00000000-0000-0000-0000-000000000000

C:\vm\tools>CpuGroups.exe GetVmGroup
VmName                                 VmId                           CpuGroupId
------ ------------------------------------ ------------------------------------
    G2 4ABCFC2F-6C22-498C-BB38-7151CE678758 36ab08cb-3a76-4b38-992e-000000000002
    P1 973B9426-0711-4742-AD3B-D8C39D6A0DEC 36ab08cb-3a76-4b38-992e-000000000003
    P2 A593D93A-3A5F-48AB-8862-A4350E3459E8 36ab08cb-3a76-4b38-992e-000000000004
    G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200 36ab08cb-3a76-4b38-992e-000000000002
    G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC 00000000-0000-0000-0000-000000000000
```

### <a name="example-8--bind-a-vm-to-an-existing-cpu-group"></a>Ejemplo 8: enlazar a una máquina virtual a un grupo existente de la CPU

En este caso, vamos a agregar una máquina virtual a un grupo existente de la CPU.
Tenga en cuenta que la máquina virtual no debe estar enlazada a ningún grupo de CPU existente o se producirá un error de Id. de grupo de CPU de configuración.

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:36AB08CB-3A76-4B38-992E-000000000001
```

Ahora, confirme que la G1 de máquina virtual está en el grupo de CPU deseado.

```console
C:\vm\tools>CpuGroups.exe GetVmGroup
VmName                                 VmId                           CpuGroupId
------ ------------------------------------ ------------------------------------
    G2 4ABCFC2F-6C22-498C-BB38-7151CE678758 36ab08cb-3a76-4b38-992e-000000000002
    P1 973B9426-0711-4742-AD3B-D8C39D6A0DEC 36ab08cb-3a76-4b38-992e-000000000003
    P2 A593D93A-3A5F-48AB-8862-A4350E3459E8 36ab08cb-3a76-4b38-992e-000000000004
    G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200 36ab08cb-3a76-4b38-992e-000000000002
    G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC 36AB08CB-3A76-4B38-992E-000000000001
```

### <a name="example-9--print-all-vms-grouped-by-cpu-group-id"></a>Ejemplo 9: imprimir todas las máquinas virtuales agrupadas por Id. de grupo de CPU

```console
C:\vm\tools>CpuGroups.exe GetGroupVms
CpuGroupId                           VmName                                 VmId
------------------------------------ ------ ------------------------------------
36AB08CB-3A76-4B38-992E-000000000001     G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC
36ab08cb-3a76-4b38-992e-000000000002     G2 4ABCFC2F-6C22-498C-BB38-7151CE678758
36ab08cb-3a76-4b38-992e-000000000002     G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200
36ab08cb-3a76-4b38-992e-000000000003     P1 973B9426-0711-4742-AD3B-D8C39D6A0DEC
36ab08cb-3a76-4b38-992e-000000000004     P2 A593D93A-3A5F-48AB-8862-A4350E3459E8
```

### <a name="example-10--print-all-vms-for-a-single-cpu-group"></a>Ejemplo 10: imprimir todas las máquinas virtuales para un único grupo de CPU

```console
C:\vm\tools>CpuGroups.exe GetGroupVms /GroupId:36ab08cb-3a76-4b38-992e-000000000002

CpuGroupId                           VmName                                VmId
------------------------------------ ------ ------------------------------------
36ab08cb-3a76-4b38-992e-000000000002     G2 4ABCFC2F-6C22-498C-BB38-7151CE678758
36ab08cb-3a76-4b38-992e-000000000002     G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200
```

### <a name="example-11--attempting-to-delete-a-non-empty-cpu-group"></a>Ejemplo 11: si intenta eliminar un grupo de la CPU no vacío

Vaciar solo grupos de CPU, es decir, los grupos de CPU no enlazado las máquinas virtuales, se puede eliminar.
Se producirá un error al intentar eliminar un grupo de la CPU no esté vacío.

```console
C:\vm\tools>CpuGroups.exe DeleteGroup /GroupId:36ab08cb-3a76-4b38-992e-000000000001
(null)
Failed with error 0xc0350070
```

### <a name="example-12--unbind-the-only-vm-from-a-cpu-group-and-delete-the-group"></a>Ejemplo 12: desenlazar la única máquina virtual de un grupo de CPU y eliminar el grupo

En este ejemplo, se deberá usar varios comandos para examinar un grupo de CPU, quite la máquina virtual única que pertenecen a ese grupo y luego eliminar el grupo.

En primer lugar, vamos a enumerar las máquinas virtuales en nuestro grupo.

```console
C:\vm\tools>CpuGroups.exe GetGroupVms /GroupId:36AB08CB-3A76-4B38-992E-000000000001
CpuGroupId                           VmName                                VmId
------------------------------------ ------ ------------------------------------
36AB08CB-3A76-4B38-992E-000000000001     G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC
```

Podemos ver que sólo una única máquina virtual, denominada G1, pertenece a este grupo.
Vamos a quitar la VM G1 de nuestro grupo estableciendo el identificador de grupo de la máquina virtual en NULL.

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:00000000-0000-0000-0000-000000000000
```

Así como comprobar nuestro cambio...

```console
C:\vm\tools>CpuGroups.exe GetVmGroup /VmName:g1
VmName                                 VmId                           CpuGroupId
------ ------------------------------------ ------------------------------------
    G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC 00000000-0000-0000-0000-000000000000
```

Ahora que el grupo está vacío, nos podemos eliminarlo de forma segura.

```console
C:\vm\tools>CpuGroups.exe DeleteGroup /GroupId:36ab08cb-3a76-4b38-992e-000000000001
```

Y confirme que nuestro grupo ya no existe.

```console
C:\vm\tools>CpuGroups.exe GetGroups
CpuGroupId                          CpuCap                     LpIndexes
------------------------------------ ------ -----------------------------
36AB08CB-3A76-4B38-992E-000000000002 32768  4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536 24,25,26,27,28,29,30,31
```

### <a name="example-13--bind-a-vm-back-to-its-original-cpu-group"></a>Ejemplo 13: enlazar a una máquina virtual a su grupo original de la CPU

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:36AB08CB-3A76-4B38-992E-000000000002

C:\vm\tools>CpuGroups.exe GetGroupVms
CpuGroupId VmName VmId
------------------------------------ -------------------------------- ------------------------------------
36ab08cb-3a76-4b38-992e-000000000002 G2 4ABCFC2F-6C22-498C-BB38-7151CE678758
36ab08cb-3a76-4b38-992e-000000000002 G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200
36AB08CB-3A76-4B38-992E-000000000002 G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC
36ab08cb-3a76-4b38-992e-000000000003 P1 973B9426-0711-4742-AD3B-D8C39D6A0DEC
36ab08cb-3a76-4b38-992e-000000000004 P2 A593D93A-3A5F-48AB-8862-A4350E3459E8
```
