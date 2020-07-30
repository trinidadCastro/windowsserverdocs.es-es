---
title: Solución de problemas de clúster con el id. de evento 1135
description: Describe cómo solucionar el problema de inicio del servicio de Cluster Server con el identificador de evento 1135.
ms.date: 05/28/2020
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: 2836fc9385d57ff076828ab5cf6a1e341a7d88a8
ms.sourcegitcommit: 145cf75f89f4e7460e737861b7407b5cee7c6645
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2020
ms.locfileid: "87409836"
---
# <a name="troubleshooting-cluster-issue-with-event-id-1135"></a>Solución de problemas de clúster con el id. de evento 1135

Este artículo le ayuda a diagnosticar y resolver el ID. de evento 1135, que se puede registrar durante el inicio del Servicio de clúster en el entorno de clústeres de conmutación por error.

## <a name="start-page"></a>Página de inicio

El ID. de evento 1135 indica que se han quitado uno o varios nodos del clúster de la pertenencia al clúster de conmutación por error activa. Puede ir acompañado de los síntomas siguientes:

- Failover\nodes de clúster que se quita de la pertenencia al clúster de conmutación por error activa: [problema con los nodos que se quitan de la pertenencia al clúster de conmutación por error](/problem-nodes-failover-cluster.md)
- ID. de evento 1069 [ID. de evento 1069: disponibilidad de servicio o aplicación en clúster](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc756225(v=ws.10))
- ID. de evento 1177 para pérdida de Cuórum [ID. de evento 1177: cuórum y conectividad necesarios para el cuórum](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc773498(v=ws.10))
- ID. de evento 1006 para Servicio de clúster detenido: [ID. de evento 1006: Inicio del servicio de clúster](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc773418(v=ws.10))

Se recomienda realizar una validación y las pruebas de red como uno de los pasos de solución de problemas iniciales para asegurarse de que no hay ningún problema de configuración que pueda ser una causa de problemas.

### <a name="check-if-installed-the-recommended-hot-fixes"></a>Comprobar si se han instalado las correcciones recomendadas

El Servicio de clúster es el componente de software esencial que controla todos los aspectos del funcionamiento del clúster de conmutación por error y administra la base de datos de configuración del clúster. Si ve el ID. de evento 1135, Microsoft le recomienda que instale las correcciones que se mencionan en los artículos de Knowledge Base siguientes y que reinicie todos los nodos del clúster y observe si se vuelve a producir el problema.

- [Revisión para Windows Server 2012 R2](https://support.microsoft.com/help/2920151)
- [Revisión para Windows Server 2012](https://support.microsoft.com/help/2784261)
- [Revisión para Windows Server 2008 R2](https://support.microsoft.com/help/2545685)

### <a name="check-if-the-cluster-service-running-on-all-the-nodes"></a>Compruebe si el servicio de clúster se está ejecutando en todos los nodos.

Siga el siguiente comando según el sistema operativo Windows para validar que el servicio de clúster se está ejecutando y está disponible continuamente.

#### <a name="for-windows-server-2008-r2-cluster"></a>Para el clúster de Windows Server 2008 R2

desde el símbolo del sistema con privilegios elevados, ejecute: **cluster.exe nodo/STAT**

#### <a name="for-windows-server-2012-and-windows-server-2012-r2-cluster"></a>Para el clúster de Windows Server 2012 \ y Windows Server 2012 R2

Ejecutar comando PS: **nodo de clúster/status**

¿El servicio de clúster se ejecuta continuamente y está disponible en todos los nodos?

## <a name="solution-for-cluster-service-is-failing"></a>Error en la solución para el servicio de Cluster Server

Si se produce un error en el servicio de clúster, solucione este artículo: [Windows Server 2008 y los modificadores de inicio del clúster de conmutación por error de 2008R2](/archive/blogs/askcore/windows-server-2008-and-2008r2-failover-cluster-startup-switches).


## <a name="several-scenarios-of-event-id-1135"></a>Varios escenarios del ID. de evento 1135

Queremos echar un vistazo más de cerca a los registros de eventos del sistema en todos los nodos del clúster. Revise el ID. de evento 1135 que está viendo en los nodos y copie todas las instancias de este evento. Esto hará que sea conveniente verlos y revisarlos.

```console
Event ID 1135
Cluster node ' **NODE A** ' was removed from the active failover cluster membership. The Cluster service on this node may have stopped. This could also be due to the node having lost communication with other active nodes in the failover cluster. Run the Validate a Configuration wizard to check your network configuration. If the condition persists, check for hardware or software errors related to the network adapters on this node. Also check for failures in any other network components to which the node is connected such as hubs, switches, or bridges.
```

Hay tres escenarios típicos:

### <a name="scenario-a"></a>Escenario A

Está examinando todos los eventos y todos los nodos del clúster indican que el nodo A ha perdido la comunicación.

![Escenario a ](media/troubleshooting-cluster-event-id-1135/18647.png)
 ![](media/troubleshooting-cluster-event-id-1135/18648.png)

Podría ser posible que, al ver los registros del sistema en el nodo A, tenga eventos para todos los nodos restantes del clúster.

#### <a name="solution"></a>Solución

Esto sugiere que en el momento del problema, debido a la congestión de la red o, de lo contrario, se perdió la comunicación con el nodo a.

Debe revisar y validar los problemas de comunicación y configuración de la red. Recuerde buscar problemas relacionados con el nodo a.

### <a name="scenario-b"></a>Escenario B

Está viendo los eventos en los nodos y Supongamos que el clúster está disperso en dos sitios. El nodo A, el nodo B y el nodo C en el sitio 1 y el nodo D & nodo E en el sitio 2.

![Escenario B](media/troubleshooting-cluster-event-id-1135/18649.png)

En los nodos A, B y C, verá que los eventos que se registran son para la conectividad a los nodos D & E. Del mismo modo, cuando vea los eventos en los nodos D & E, los eventos sugieren que se perdió la comunicación con A, B y C.

![Escenario B](media/troubleshooting-cluster-event-id-1135/18650.png)

#### <a name="solution"></a>Solución

Si ve actividad similar, indica que se produjo un error de comunicación, en el vínculo que conecta estos sitios. Recomendamos que revise la conexión entre los sitios, si se trata de una conexión WAN, sugerimos que compruebe con su ISP la conectividad.

### <a name="scenario-c"></a>Escenario C

Está examinando los eventos de los nodos y verá que los nombres de los nodos no se incluyen en un patrón determinado. Supongamos que el clúster está disperso en dos sitios. El nodo A, el nodo B y el nodo C en el sitio 1 y el nodo D & nodo E en el sitio 2.

- En el nodo A: verá eventos de los nodos B, D, E.
- En el nodo B: verá eventos de los nodos C, D, E.
- En el nodo C: verá eventos para los nodos A, B y E.
- En el nodo D: verá eventos de los nodos A, C, E.
- En el nodo E: se ven los eventos de los nodos B, C, D.
- O cualquier otra combinación.

![Escenario C](media/troubleshooting-cluster-event-id-1135/18651.png)

#### <a name="solution"></a>Solución

Estos eventos son posibles cuando los canales de red entre los nodos se retraen y los mensajes de comunicación del clúster no se están llegando a tiempo, lo que hace que el clúster parezca que la comunicación entre los nodos se pierde, lo que da lugar a la eliminación de nodos de la pertenencia al clúster.

## <a name="review-cluster-networks"></a>Revisar redes de clústeres

Recomendamos que revise las redes de clústeres mediante la comprobación de las tres opciones siguientes: una por una para continuar con esta guía de solución de problemas.

### <a name="check-for-antivirus-exclusion"></a>Comprobar la exclusión de antivirus

Excluya las siguientes ubicaciones del sistema de archivos de la detección de virus en un servidor que ejecuta servicios de clúster:

- Ruta de acceso del testigo de FileShare

- La *carpeta%SystemRoot%\Cluster*

Configure el componente de detección en tiempo real en el software antivirus para excluir los siguientes directorios y archivos:

- Directorio de configuración de máquina virtual predeterminado (C:\ProgramData\Microsoft\Windows\Hyper-V)

- Directorios de configuración de máquina virtual personalizada

- Directorio de unidad de disco duro virtual predeterminado (discos duros C:\Users\Public\Documents\Hyper-V\Virtual)

- Directorios de unidad de disco duro virtual personalizada

- Directorios de datos de replicación personalizados, si usa la réplica de Hyper-V

- Directorios de instantáneas

- mms.exe

    > [!NOTE]
    > Es posible que este archivo tenga que configurarse como una exclusión de procesos dentro del software antivirus).

- Vmwp.exe

    > [!NOTE]
    > Es posible que este archivo tenga que configurarse como una exclusión de procesos en el software antivirus.

Además, cuando use Migración en vivo junto con volúmenes compartidos de clúster, excluya la ruta de acceso de CSV *C:\Clusterstorage* y todos sus subdirectorios. Si está solucionando problemas de conmutación por error o problemas generales de los servicios de clúster y el software antivirus está instalado, desinstale temporalmente el software antivirus o consulte al fabricante del software para determinar si el software antivirus funciona con los servicios de clúster. Simplemente deshabilitar el software antivirus no es suficiente en la mayoría de los casos. Aunque deshabilite el software antivirus, el controlador de filtro se seguirá cargando al reiniciar el equipo.

### <a name="check-for-network-port-configuration-in-firewall"></a>Comprobar la configuración del puerto de red en el Firewall

El Servicio de clúster controla las operaciones de clúster de servidor y administra la base de datos de clúster. Un clúster es una colección de equipos independientes que actúan como un solo equipo. Los administradores, programadores y usuarios ven el clúster como un solo sistema. El software distribuye los datos entre los nodos del clúster. Si se produce un error en un nodo, otros nodos proporcionan los servicios y los datos que se proporcionaron anteriormente en el nodo que faltaba. Cuando se agrega o repara un nodo, el software del clúster migra algunos datos a ese nodo.

Nombre del servicio de sistema: **ClusSvc**

|Application|Protocolo|Puertos|
|---|---|---|
|Servicio de Cluster Server|UDP|3343|
|Servicio de Cluster Server|TCP|3343 (este puerto es necesario durante una operación de unión de nodo).|
|RPC|TCP|135|
|Administrador de clústeres|UDP|137|
|Kerberos|UDP\TCP|464 *|
|SMB|TCP|445|
|Puertos UDP altos asignados aleatoriamente * *|UDP|Número de Puerto aleatorio entre 1024 y 65535<br/>Número de Puerto aleatorio entre 49152 y 65535 * * *|
||||

> [!NOTE]
> Además, para realizar una validación correcta de los clústeres de conmutación por error de Windows en Windows Server 2008 y versiones posteriores, permita el tráfico entrante y saliente para ICMP4, ICMP6.

- Para obtener más información, vea el apartado sobre [creación de un clúster de conmutación por error de Windows Server 2012.0xc000005e](https://support.microsoft.com/help/2830510).

- Para obtener más información sobre cómo personalizar estos puertos, consulte [información general del servicio y requisitos del puerto de red para Windows](https://support.microsoft.com/help/832017/) en la sección "referencias".

Este es el intervalo en Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7, Windows Server 2008 y Windows Vista.

Además, ejecute el siguiente comando para comprobar la configuración del puerto de red en el firewall. Por ejemplo: este comando ayuda a determinar el puerto 3343 available\open usado para el clúster de conmutación por error:

```console
netsh advfirewall firewall show rule name="Failover Clusters (UDP-In)" verbose
```

### <a name="run-the-cluster-validation-report-for-any-errors-or-warnings"></a>Ejecutar el informe de validación de clústeres para cualquier error o ADVERTENCIA

La herramienta de validación de clústeres ejecuta un conjunto de pruebas para comprobar que el hardware y la configuración son compatibles con los clústeres de conmutación por error.

Siga estas instrucciones:

1. Ejecute el informe de validación de clústeres para cualquier error o advertencia. Para obtener más información, consulte Descripción de las [pruebas de validación de clúster: red](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771323(v=ws.11)?redirectedfrom=MSDN)

    ![subhatt1](media/troubleshooting-cluster-event-id-1135/18653.png)

2. Compruebe si hay advertencias y errores en las redes. Para obtener más información, vea Descripción de las [pruebas de validación de clúster: red](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771323(v=ws.11)?redirectedfrom=MSDN).

    ![Resultados por categoría ](media/troubleshooting-cluster-event-id-1135/18654.png) ![ red](media/troubleshooting-cluster-event-id-1135/18655.png)

#### <a name="check-the-list-network-binding-order"></a>Comprobar el orden de enlace de red de la lista

Esta prueba muestra el orden en el que las redes se enlazan a los adaptadores en cada nodo.

En la pestaña adaptadores y enlaces se enumeran las conexiones en el orden en el que los servicios de red tienen acceso a las conexiones. El orden de estas conexiones refleja el orden en el que se envían a la conexión los paquetes/llamadas genéricos de TCP/IP.

Siga los pasos que se indican a continuación para cambiar el orden de enlace de los adaptadores de red:

1. Haga clic en **Inicio**, haga clic en **ejecutar**, escriba ncpa.cpl y, a continuación, haga clic en **Aceptar**. Puede ver las conexiones disponibles en la sección **LAN y Internet de alta velocidad** de la ventana **conexiones de red** .

2. En el menú **Opciones avanzadas** , haga clic en **Configuración avanzada**y, a continuación, haga clic en la pestaña **adaptadores y enlaces** .

3. En el área **conexiones** , seleccione la conexión que desea que se mueva hacia arriba en la lista. Use los botones de flecha para trasladar la conexión. Como norma general, la tarjeta que se comunica con la red (conectividad de dominio, enrutamiento a otras redes, etc.) debe ser la primera tarjeta enlazada (parte superior de la lista).

Los nodos de clúster son sistemas de host múltiple. La prioridad de red afecta a cliente DNS para la conectividad de red de salida. Los adaptadores de red que se usan para la comunicación de cliente deben estar en la parte superior del orden de enlace. Las redes no enrutadas se pueden poner en prioridad más baja. En Windows Server 2012 y Windows Server 2012 R2, el adaptador de controlador de red de clúster (NETFT.SYS) se coloca automáticamente en la parte inferior de la lista de orden de enlace.

#### <a name="check-the-validate-network-communication"></a>Comprobar la comunicación de red de validación

La latencia de la red también podría hacer que esto suceda. Es posible que los paquetes no se pierdan entre los nodos, pero es posible que no lleguen a los nodos lo suficientemente rápido antes de que expire el período de tiempo de espera.

esta prueba se encarga de validar que los servidores probados se puedan comunicar con una latencia aceptable en todas las redes.

Por ejemplo: en validar la comunicación de red, es posible que vea los siguientes mensajes de problemas de latencia de red.

```console
Succeeded in pinging network interface node003.contoso.com IP Address 192.168.0.2 from network interface node004.contoso.com IP Address 192.168.0.3 with maximum delay 500 after 1 attempt(s).
Either address 10.0.0.96 is not reachable from 192.168.0.2 or **the ping latency is greater than the maximum allowed 2000 ms** This may be expected, since network interfaces node003.contoso.com - Heartbeat Network and node004.contoso.com - Production Network are on different cluster networks
Either address 192.168.0.2 is not reachable from 10.0.0.96 or **the ping latency is greater than the maximum allowed 2000 ms** This may be expected, since network interfaces node004.contoso.com - Production Network and node003.contoso.com - Heartbeat Network for MSCS are on different cluster networks
```

En el caso de clústeres de varios sitios, puede aumentar los valores de tiempo de espera. Para obtener más información, consulte [configuración de latidos y valores de DNS en un clúster de conmutación por error de varios sitios](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd197562(v=ws.10)?redirectedfrom=MSDN).

Consulte con el ISP los problemas de conectividad de la WAN.

Compruebe si encuentra alguno de los siguientes problemas.

##### <a name="network-packets-lost-between-nodes"></a>Paquetes de red perdidos entre nodos

1. Comprobación de la pérdida de paquetes con rendimiento

    Si el paquete se pierde en la conexión en algún lugar de los nodos, se producirá un error en los latidos. Podemos averiguar fácilmente si se trata de un problema mediante el monitor de rendimiento para ver el contador "red Interface\Packets recibida descartada". Una vez que haya agregado este contador, examine los números de medio, mínimo y máximo y, si son valores mayores que cero, el búfer de recepción debe ajustarse para el adaptador.

    ![Agregar contadores](media/troubleshooting-cluster-event-id-1135/18652.png)

    Si tiene un paquete de red perdido en la plataforma de virtualización de VmWare, consulte la sección "clúster instalado en la plataforma de virtualización de VmWare".

2. Actualización de los controladores NIC

    Este problema puede deberse a que los componentes de NIC drivers\Integration (IC) \VmTools o los adaptadores de NIC defectuosos no están actualizados.
    Si se pierden paquetes de red entre los nodos de las máquinas físicas, tenga las actualizaciones del controlador del adaptador de red. Controladores o firmware de tarjetas de red antiguos o no actualizados.
    En ocasiones, un problema de configuración simple de la tarjeta de red o del conmutador también puede provocar la pérdida de latidos.

##### <a name="cluster-installed-in-the-vmware-virtualization-platform"></a>Clúster instalado en la plataforma de virtualización de VmWare

Compruebe los problemas del adaptador de VMware en caso de entorno de VMware.

Este problema puede producirse si los paquetes se quitan durante ráfagas elevadas de tráfico. Asegúrese de que no se produce ningún filtrado de tráfico (por ejemplo, con un filtro de correo). Después de eliminar esta posibilidad, aumente gradualmente el número de búferes en el sistema operativo invitado y compruébelo.

Para reducir las caídas del tráfico de ráfagas, siga estos pasos:

1. Abra el cuadro ejecutar con la tecla Windows + R.
2. Escriba devmgmt. msc y presione **entrar**.
3. Expandir **adaptadores de red**
4. Haga clic con el botón secundario en **vmxnet3 y haga clic en propiedades.**
5. Haga clic en la pestaña **Opciones avanzadas**.
6. Haga clic en **búferes de recepción pequeños** y aumente el valor. El valor predeterminado es 512 y el máximo es 8192.
7. Haga clic en el **anillo Rx #1** tamaño y aumente el valor. El valor predeterminado es 1024 y el máximo es 4096.

Compruebe las siguientes direcciones URL para comprobar los problemas del adaptador de VMware en el caso del entorno de VMware:

- [¿Los nodos se quitan de la pertenencia al clúster de conmutación por error en VMware ESX?](/archive/blogs/askcore/nodes-being-removed-from-failover-cluster-membership-on-vmware-esx)

- [Pérdida de paquetes grande en el nivel del sistema operativo invitado en el VMXNET3 vNIC en ESXi](https://kb.vmware.com/s/article/2039495)

##### <a name="noticed-any-network-congestion"></a>Se ha observado cualquier congestión de red

La congestión de la red también puede producir problemas de conectividad de red.

Comprobar que la red está configurada según las recomendaciones de MS y de proveedor, consulte [configuración de redes de clústeres de conmutación por error de Windows](/archive/blogs/askcore/configuring-windows-failover-cluster-networks).

##### <a name="check-the-network-configuration"></a>comprobar la configuración de la red

Si todavía no funciona, compruebe si ha aparecido red con particiones en la GUI del clúster o si tiene habilitada la formación de equipos NIC en la NIC de latido.

Si ve red con particiones en la GUI del clúster, consulte el tema [sobre las redes de clústeres con particiones](/archive/blogs/askcore/partitioned-cluster-networks) para solucionar el problema.

Si tiene habilitada la formación de equipos NIC en la NIC de latido, Compruebe la funcionalidad del software de formación de equipos según la recomendación del proveedor de formación de equipos.

##### <a name="upgrade-the-nic-drivers"></a>Actualización de los controladores NIC

Este problema puede producirse debido a los controladores de NIC no actualizados o a los adaptadores de NIC defectuosos.

Si se pierden paquetes de red entre los nodos de las máquinas físicas, tenga las actualizaciones del controlador del adaptador de red. Controladores o firmware de tarjetas de red antiguos o no actualizados.

En ocasiones, un problema de configuración simple de la tarjeta de red o del conmutador también puede provocar la pérdida de latidos.

##### <a name="check-the-network-configuration"></a>comprobar la configuración de la red

Si sigue sin funcionar, compruebe si ha aparecido red con particiones en la GUI del clúster o si tiene habilitada la formación de equipos NIC en la NIC de latido.
