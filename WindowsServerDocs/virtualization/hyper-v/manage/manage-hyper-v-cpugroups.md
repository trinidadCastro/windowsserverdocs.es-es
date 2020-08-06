---
title: Controles de recursos de máquina virtual
description: Uso de grupos de CPU de VM
author: allenma
ms.date: 06/18/2018
ms.topic: article
ms.prod: windows-server
ms.service: windows-10-hyperv
ms.assetid: cc7bb88e-ae75-4a54-9fb4-fc7c14964d67
ms.openlocfilehash: bcae278caf088bc544fb6686450eacdfdf88237b
ms.sourcegitcommit: acfdb7b2ad283d74f526972b47c371de903d2a3d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/05/2020
ms.locfileid: "87769603"
---
# <a name="virtual-machine-resource-controls"></a>Controles de recursos de máquina virtual

> Se aplica a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

En este artículo se describen los controles de aislamiento y recursos de Hyper-V para máquinas virtuales.  Estas funcionalidades, a las que haremos referencia como grupos de CPU de máquinas virtuales o simplemente "grupos de CPU", se introdujeron en Windows Server 2016.  Los grupos de CPU permiten a los administradores de Hyper-V administrar mejor y asignar los recursos de CPU del host entre las máquinas virtuales invitadas.  Mediante el uso de grupos de CPU, los administradores de Hyper-V pueden:

* Cree grupos de máquinas virtuales, donde cada grupo tiene diferentes asignaciones de recursos de CPU totales del host de virtualización, que se comparten en todo el grupo. Esto permite al administrador del host implementar clases de servicio para diferentes tipos de máquinas virtuales.

* Establezca límites de recursos de CPU en grupos específicos. Este "límite de grupo" establece el límite superior para los recursos de CPU del host que puede consumir todo el grupo, aplicando eficazmente la clase de servicio deseada para ese grupo.

* Restrinja un grupo de CPU para que se ejecute solo en un conjunto específico de procesadores del sistema host. Se puede usar para aislar las máquinas virtuales que pertenecen a diferentes grupos de CPU entre sí.

## <a name="managing-cpu-groups"></a>Administración de grupos de CPU

Los grupos de CPU se administran a través del servicio de proceso del host de Hyper-V o HCS. En el blog del equipo de virtualización de Microsoft, encontrará una excelente descripción de HCS, su Genesis, vínculos a las API de HCS y más información en el blog del equipo de virtualización de Microsoft, en la publicación que [presenta el servicio de proceso de host (HCS)](https://blogs.technet.microsoft.com/virtualization/2017/01/27/introducing-the-host-compute-service-hcs/).

>[!NOTE]
>Solo se puede usar HCS para crear y administrar grupos de CPU. el applet de administrador de Hyper-V, las interfaces de administración de WMI y de PowerShell no admiten grupos de CPU.

Microsoft proporciona una utilidad de línea de comandos, cpugroups.exe, en el [centro de descarga de Microsoft](https://go.microsoft.com/fwlink/?linkid=865968) que usa la interfaz HCS para administrar grupos de CPU.  Esta utilidad también puede mostrar la topología de CPU de un host.

## <a name="how-cpu-groups-work"></a>Cómo funcionan los grupos de CPU

El hipervisor de Hyper-V aplica la asignación de recursos de proceso de host entre grupos de CPU, con un límite de grupo de CPU calculado. El límite de grupo de CPU es una fracción de la capacidad total de CPU para un grupo de CPU. El valor del límite de grupo depende de la clase de grupo o del nivel de prioridad asignado. El límite de grupo calculado se puede considerar como "número de tiempo de CPU de LP". Esta cotización de grupo está compartida, por lo que si solo estaba activa una única máquina virtual, podría usar la asignación de CPU del grupo completo para sí misma.

El límite del grupo de CPU se calcula como G = *n* x *C*, donde:

- *G* es la cantidad de host LP que nos gustaría asignar al grupo
- *n* es el número total de procesadores lógicos (LPS) del grupo
- *C* es la asignación de CPU máxima, es decir, la clase de servicio deseada para el grupo, expresada como un porcentaje de la capacidad total de proceso del sistema.

Por ejemplo, Imagine un grupo de CPU configurado con 4 procesadores lógicos (LPs) y un límite del 50%.

- G = n * C
- G = 4 * 50%
- G = 2 el tiempo de CPU de LP para todo el grupo

En este ejemplo, se asigna al grupo de CPU G el tiempo de CPU de LP.

Tenga en cuenta que el límite de grupo se aplica independientemente del número de máquinas virtuales o de procesadores virtuales enlazados al grupo, independientemente del estado (por ejemplo, apagado o iniciado) de las máquinas virtuales asignadas al grupo de CPU. Por lo tanto, cada máquina virtual enlazada al mismo grupo de CPU recibirá una fracción de la asignación total de CPU del grupo, lo que cambiará con el número de máquinas virtuales enlazadas al grupo de CPU. Por lo tanto, a medida que las máquinas virtuales son máquinas virtuales enlazadas o desenlazadas de un grupo de CPU, se debe reajustar el límite general del grupo de CPU y configurarlo para mantener el límite por máquina virtual que se desea. El nivel de software de administrador de host de VM o de administración de virtualización es responsable de administrar las Cap de grupo según sea necesario para lograr la asignación de recursos de CPU por máquina virtual deseada.

## <a name="example-classes-of-service"></a>Clases de ejemplo de servicio

Echemos un vistazo a algunos ejemplos sencillos. Para empezar, supongamos que el administrador del host de Hyper-V desea admitir dos niveles de servicio para las máquinas virtuales invitadas:

1. Nivel "C" de low-end. Proporcionaremos este nivel el 10% de los recursos de proceso del host completo.

1. Nivel "B" de intervalo medio. A este nivel se le asigna el 50% de los recursos de proceso del host completo.

En este punto del ejemplo, se afirmará que no hay ningún otro control de recursos de CPU en uso, como Cap, pesos y reservas de máquinas virtuales individuales.
Sin embargo, las Cap de máquinas virtuales individuales son importantes, ya que veremos un poco más adelante.

Por motivos de simplicidad, supongamos que cada máquina virtual tiene 1 VP y que nuestro host tiene 8 LPs. Comenzaremos con un host vacío.

Para crear el nivel "B", el host adminstartor establece el límite de grupo en 50%:

- G = n * C
- G = 8 * 50%
- G = 4 veces el tiempo de CPU de LP para todo el grupo

El administrador del host agrega una única máquina virtual de nivel "B".
En este punto, la máquina virtual de nivel "B" puede usar como máximo el 50% de la CPU del host o el equivalente de 4 LPs en nuestro sistema de ejemplo.

Ahora, el administrador agrega una segunda máquina virtual de "nivel B". La asignación del grupo de CPU, se divide uniformemente entre todas las máquinas virtuales. Tenemos un total de 2 máquinas virtuales en el grupo B, por lo que cada máquina virtual ahora obtiene la mitad del total del grupo B del 50%, el 25%, o el equivalente a 2 LPs de tiempo de proceso.

## <a name="setting-cpu-caps-on-individual-vms"></a>Establecer límites de CPU en máquinas virtuales individuales

Además del límite de grupo, cada máquina virtual también puede tener una "Cap de VM" individual. Los controles de recursos de CPU por máquina virtual, incluido un límite de CPU, peso y reserva, han sido parte de Hyper-V desde su introducción.
Cuando se combina con un límite de grupo, una Cap de VM especifica la cantidad máxima de CPU que puede obtener cada VP, incluso si el grupo tiene recursos de CPU disponibles.

Por ejemplo, es posible que el administrador del host desee colocar un límite de VM del 10% en las máquinas virtuales de "C".
De este modo, incluso si la mayoría de los VPs "C" están inactivos, cada VP podría no llegar a más de un 10%.
Sin una Cap de máquina virtual, las máquinas virtuales "C" podían lograr el rendimiento de forma oportunista más allá de los niveles permitidos por su nivel.

## <a name="isolating-vm-groups-to-specific-host-processors"></a>Aislamiento de grupos de máquinas virtuales a procesadores de host específicos

Los administradores del host de Hyper-V también pueden querer dedicar recursos de proceso a una máquina virtual.
Por ejemplo, Imagine que el administrador desea ofrecer una máquina virtual "A" Premium con un límite de clase del 100%.
Estas máquinas virtuales Premium también requieren la latencia y la vibración de programación más bajas posibles. es decir, es posible que no sean desprogramados por ninguna otra máquina virtual.
Para lograr esta separación, un grupo de CPU también puede configurarse con una asignación de afinidad de LP específica.

Por ejemplo, para ajustar una máquina virtual "A" en el host en nuestro ejemplo, el administrador crearía un nuevo grupo de CPU y establecería la afinidad del procesador del grupo en un subconjunto de LPs del host.
Los grupos B y C se afinidad con a los LPs restantes.
El administrador puede crear una única máquina virtual en el grupo A, que tendría acceso exclusivo a todos los LPs del grupo A, mientras que los grupos de nivel presumiblemente inferiores B y C compartirían el LP restante.

## <a name="segregating-root-vps-from-guest-vps"></a>Segregación de VPs raíz de VPs de invitado

De forma predeterminada, Hyper-V creará una VP raíz en cada LP físico subyacente.
Estos VPs raíz se asignan estrictamente 1:1 con los LPs del sistema y no se migran; es decir, cada VP raíz siempre se ejecutará en el mismo LP físico.
Los VPs invitados se pueden ejecutar en cualquier LP disponible y compartirán la ejecución con VPs raíz.

Sin embargo, puede ser deseable separar por completo la actividad de Vicepresidente raíz de VPs de invitado.
Considere el ejemplo anterior donde se implementa una máquina virtual de nivel "A" Premium.
Para asegurarse de que el VPs de la máquina virtual "A" tiene la latencia y la "vibración más bajas posible" o la variación de programación, nos gustaría ejecutarlas en un conjunto dedicado de LPs y asegurarnos de que la raíz no se ejecute en estos LPs.

Esto puede realizarse mediante una combinación de la configuración de "minroot", que limita la partición del sistema operativo del host a la ejecución en un subconjunto del total de procesadores lógicos del sistema, junto con uno o varios grupos de CPU de afinidad con.

El host de virtualización puede configurarse para restringir la partición de host a un LPs específico, con uno o varios grupos de CPU afinidad con a los LPs restantes.
De esta manera, las particiones raíz e invitadas se pueden ejecutar en recursos de CPU dedicados y completamente aislados, sin uso compartido de la CPU.

Para obtener más información acerca de la configuración de "minroot", consulte [Administración de recursos de CPU del host de Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-minroot-2016).

## <a name="using-the-cpugroups-tool"></a>Uso de la herramienta CpuGroups

Echemos un vistazo a algunos ejemplos de cómo usar la herramienta CpuGroups.

>[!NOTE]
>Los parámetros de línea de comandos para la herramienta CpuGroups se pasan usando solo espacios como delimitadores. Ningún carácter '/' o '-' debe continuar el modificador de línea de comandos deseado.

### <a name="discovering-the-cpu-topology"></a>Detección de la topología de CPU

Al ejecutar CpuGroups con GetCpuTopology, se devuelve información sobre el sistema actual, como se muestra a continuación, incluido el índice de LP, el nodo NUMA al que pertenece el LP, los identificadores de paquete y núcleo y el índice de VP raíz.

En el ejemplo siguiente se muestra un sistema con dos sockets de CPU y nodos NUMA, un total de 32 LPs y varios subprocesos habilitados, y configurados para habilitar Minroot con 8 VPs raíz, 4 desde cada nodo NUMA.
Los LPs que tienen VPs raíz tienen un RootVpIndex >= 0; LPs con un valor de RootVpIndex de-1 no está disponible para la partición raíz, pero el hipervisor sigue siendo administrado por el hipervisor y ejecutará VPs de invitado tal y como lo permitan otras opciones de configuración.

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

### <a name="example-2--print-all-cpu-groups-on-the-host"></a>Ejemplo 2: imprimir todos los grupos de CPU en el host

Aquí, se enumerarán todos los grupos de CPU en el host actual, su GroupId, el límite de CPU del grupo y las lenguas indias de los LPs asignados a ese grupo.

Tenga en cuenta que los valores de límite de CPU válidos están en el intervalo [0, 65536] y estos valores expresan el límite de grupo en porcentaje (por ejemplo, 32768 = 50%).

```console
C:\vm\tools>CpuGroups.exe GetGroups

CpuGroupId                          CpuCap  LpIndexes
------------------------------------ ------ --------
36AB08CB-3A76-4B38-992E-000000000002 32768  4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536  24,25,26,27,28,29,30,31
```

### <a name="example-3--print-a-single-cpu-group"></a>Ejemplo 3: imprimir un solo grupo de CPU

En este ejemplo, se consultará un solo grupo de CPU con el GroupId como filtro.

```console
C:\vm\tools>CpuGroups.exe GetGroups /GroupId:36AB08CB-3A76-4B38-992E-000000000003
CpuGroupId                          CpuCap   LpIndexes
------------------------------------ ------ ----------
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
```

### <a name="example-4--create-a-new-cpu-group"></a>Ejemplo 4: crear un nuevo grupo de CPU

Aquí vamos a crear un nuevo grupo de CPU, especificando el identificador de grupo y el conjunto de LPs que se asignará al grupo.

```console
C:\vm\tools>CpuGroups.exe CreateGroup /GroupId:36AB08CB-3A76-4B38-992E-000000000001 /GroupAffinity:0,1,16,17
```

Ahora, muestre el grupo recién agregado.

```console
C:\vm\tools>CpuGroups.exe GetGroups
CpuGroupId                          CpuCap LpIndexes
------------------------------------ ------ ---------
36AB08CB-3A76-4B38-992E-000000000001 65536 0,1,16,17
36AB08CB-3A76-4B38-992E-000000000002 32768 4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536 12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536 24,25,26,27,28,29,30,31
```

### <a name="example-5--set-the-cpu-group-cap-to-50"></a>Ejemplo 5: establecer el límite de grupo de CPU en 50%

Aquí estableceremos el límite del grupo de CPU en 50%.

```console
C:\vm\tools>CpuGroups.exe SetGroupProperty /GroupId:36AB08CB-3A76-4B38-992E-000000000001 /CpuCap:32768
```

Ahora vamos a confirmar nuestra configuración mostrando el grupo que acabamos de actualizar.

```console
C:\vm\tools>CpuGroups.exe GetGroups /GroupId:36AB08CB-3A76-4B38-992E-000000000001

CpuGroupId                          CpuCap LpIndexes
------------------------------------ ------ ---------
36AB08CB-3A76-4B38-992E-000000000001 32768 0,1,16,17
```

### <a name="example-6--print-cpu-group-ids-for-all-vms-on-the-host"></a>Ejemplo 6: imprimir los ID. de grupo de CPU para todas las máquinas virtuales del host

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

### <a name="example-7--unbind-a-vm-from-the-cpu-group"></a>Ejemplo 7: desenlazar una máquina virtual del grupo de CPU

Para quitar una máquina virtual de un grupo de CPU, establezca en CpuGroupId de VM en el GUID NULL. Esto desenlaza la máquina virtual del grupo de CPU.

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

### <a name="example-8--bind-a-vm-to-an-existing-cpu-group"></a>Ejemplo 8: enlazar una máquina virtual a un grupo de CPU existente

Aquí vamos a agregar una máquina virtual a un grupo de CPU existente.
Tenga en cuenta que la máquina virtual no debe estar enlazada a ningún grupo de CPU existente, o bien se producirá un error al establecer el ID. de grupo de CPU.

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:36AB08CB-3A76-4B38-992E-000000000001
```

Ahora, confirme que la máquina virtual G1 está en el grupo de CPU deseado.

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

### <a name="example-9--print-all-vms-grouped-by-cpu-group-id"></a>Ejemplo 9: imprimir todas las máquinas virtuales agrupadas por identificador de grupo de CPU

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

### <a name="example-10--print-all-vms-for-a-single-cpu-group"></a>Ejemplo 10: imprimir todas las máquinas virtuales de un solo grupo de CPU

```console
C:\vm\tools>CpuGroups.exe GetGroupVms /GroupId:36ab08cb-3a76-4b38-992e-000000000002

CpuGroupId                           VmName                                VmId
------------------------------------ ------ ------------------------------------
36ab08cb-3a76-4b38-992e-000000000002     G2 4ABCFC2F-6C22-498C-BB38-7151CE678758
36ab08cb-3a76-4b38-992e-000000000002     G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200
```

### <a name="example-11--attempting-to-delete-a-non-empty-cpu-group"></a>Ejemplo 11: intento de eliminar un grupo de CPU que no está vacío

Solo se pueden eliminar los grupos de CPU vacíos (es decir, los grupos de CPU sin máquinas virtuales enlazadas).
Se producirá un error al intentar eliminar un grupo de CPU que no esté vacío.

```console
C:\vm\tools>CpuGroups.exe DeleteGroup /GroupId:36ab08cb-3a76-4b38-992e-000000000001
(null)
Failed with error 0xc0350070
```

### <a name="example-12--unbind-the-only-vm-from-a-cpu-group-and-delete-the-group"></a>Ejemplo 12: desenlazar la única máquina virtual de un grupo de CPU y eliminar el grupo

En este ejemplo, usaremos varios comandos para examinar un grupo de CPU, quitar la única máquina virtual que pertenece a ese grupo y, después, eliminar el grupo.

En primer lugar, vamos a enumerar las máquinas virtuales de nuestro grupo.

```console
C:\vm\tools>CpuGroups.exe GetGroupVms /GroupId:36AB08CB-3A76-4B38-992E-000000000001
CpuGroupId                           VmName                                VmId
------------------------------------ ------ ------------------------------------
36AB08CB-3A76-4B38-992E-000000000001     G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC
```

Vemos que solo una única máquina virtual, denominada G1, pertenece a este grupo.
Vamos a eliminar la máquina virtual G1 de nuestro grupo estableciendo el identificador de grupo de la máquina virtual en NULL.

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:00000000-0000-0000-0000-000000000000
```

Y compruebe nuestro cambio...

```console
C:\vm\tools>CpuGroups.exe GetVmGroup /VmName:g1
VmName                                 VmId                           CpuGroupId
------ ------------------------------------ ------------------------------------
    G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC 00000000-0000-0000-0000-000000000000
```

Ahora que el grupo está vacío, podemos eliminarlo de forma segura.

```console
C:\vm\tools>CpuGroups.exe DeleteGroup /GroupId:36ab08cb-3a76-4b38-992e-000000000001
```

Y confirme que nuestro grupo ha desaparecido.

```console
C:\vm\tools>CpuGroups.exe GetGroups
CpuGroupId                          CpuCap                     LpIndexes
------------------------------------ ------ -----------------------------
36AB08CB-3A76-4B38-992E-000000000002 32768  4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536 24,25,26,27,28,29,30,31
```

### <a name="example-13--bind-a-vm-back-to-its-original-cpu-group"></a>Ejemplo 13: enlazar una máquina virtual a su grupo de CPU original

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
