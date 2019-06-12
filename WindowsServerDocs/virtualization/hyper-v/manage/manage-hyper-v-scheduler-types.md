---
title: Entender y usar tipos de programador de hipervisor de Hyper-V
description: Proporciona información para administradores del host de Hyper-V en el uso del programador de Hyper-V de modos
author: allenma
ms.author: allenma
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows-server-hyper-v
ms.technology: virtualization
ms.localizationpriority: low
ms.assetid: 6cb13f84-cb50-4e60-a685-54f67c9146be
ms.openlocfilehash: c0c2f85fbbeca9e8ac5d40bbcb71f286fabfb65c
ms.sourcegitcommit: cd12ace92e7251daaa4e9fabf1d8418632879d38
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66501671"
---
# <a name="managing-hyper-v-hypervisor-scheduler-types"></a>Administrar tipos de programador de hipervisor de Hyper-V

>Se aplica a: Windows 10, Windows Server 2016, Windows Server, versión 1709, Windows Server, versión 1803, Windows Server 2019

En este artículo se describe nuevos modos de procesador virtual por primera vez en Windows Server 2016 de lógica de programación. Estos modos, o tipos de programador, determinan cómo el hipervisor de Hyper-V asigna y administra el trabajo entre los procesadores de invitado virtual. Un administrador del host de Hyper-V puede seleccionar tipos de hipervisor del programador que son más adecuados para las máquinas virtuales invitadas (VM) y configuración las máquinas virtuales para aprovechar las ventajas de la lógica de programación.

>[!NOTE]
>Actualizaciones son necesarias para usar las características de programador de hipervisor descritas en este documento. Para obtener más información, consulte [las actualizaciones necesarias](#required-updates).

## <a name="background"></a>Background

Antes de explicar la lógica y los controles de detrás de la programación del procesador virtual Hyper-V, es útil revisar los conceptos básicos que se tratan en este artículo.

### <a name="understanding-smt"></a>Descripción de SMT

Multithreading simultáneo o SMT, es una técnica que se utilizan en diseños de procesador moderno que permite que los recursos del procesador ser compartidos por los subprocesos de ejecución independientes. SMT generalmente ofrece una mejora de rendimiento modestas para la mayoría de las cargas en paralelo los cálculos cuando sea posible, aumenta el rendimiento de la instrucción, aunque ningún rendimiento obtener o incluso una ligera pérdida de rendimiento puede producirse cuando la contención entre subprocesos para se produce de recursos del procesador compartido.
Procesadores compatibles con SMT están disponibles de Intel y AMD. Intel hace referencia a sus ofertas de SMT como tecnología de Intel Hyper Threading o HT de Intel.

Para los fines de este artículo, las descripciones de SMT y cómo se utilizan Hyper-V se aplican igualmente a los sistemas de Intel y AMD.

* Para obtener más información sobre la tecnología Intel HT, consulte [tecnología Hyper-Threading de Intel](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html)

* Para obtener más información sobre AMD SMT, consulte [la arquitectura básica de "Zen"](https://www.amd.com/en/technologies/zen-core)

## <a name="understanding-how-hyper-v-virtualizes-processors"></a>Descripción de cómo Hyper-V virtualiza procesadores

Antes de considerar tipos del programador de hipervisor, también es útil comprender la arquitectura de Hyper-V. Puede encontrar un resumen general de [Introducción a la tecnología Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-technology-overview). Estos son conceptos importantes de este artículo:

* Hyper-V crea y administra las particiones de máquina virtual, en qué proceso recursos asignados y compartidos, bajo el control del hipervisor. Las particiones proporcionan límites de aislamiento sólido entre todas las máquinas virtuales de invitado y entre las máquinas virtuales invitadas y la partición raíz.

* La partición raíz es una partición de la máquina virtual, aunque tiene propiedades únicas y mucho más privilegios que las máquinas virtuales invitadas. La partición raíz proporciona los servicios de administración que controlan todas las máquinas virtuales de invitado, proporciona compatibilidad con dispositivos virtuales para los invitados y administra todas las E/S del dispositivo para las máquinas virtuales invitadas. Microsoft recomienda encarecidamente no ejecuta las cargas de trabajo de la aplicación en la partición raíz.

* Cada procesador virtual (VP) de la partición raíz es 1:1 asignada a un procesador lógico subyacente (LP). Un host VP siempre se ejecutará en el mismo LP subyacente: no hay ninguna migración de VPs la partición raíz.

* De forma predeterminada, el LPs en el que se ejecutan VPs host también pueden ejecutar VPs de invitado.

* Puede programar un Vicepresidente de invitado el hipervisor para ejecutarse en cualquier procesador lógico disponible. Mientras que el programador del hipervisor se encarga de considerar la localidad de memoria caché temporal, topología NUMA y muchos otros factores al programar un Vicepresidente de invitado, en última instancia, el Vicepresidente podría programarse en cualquier host LP.

## <a name="hypervisor-scheduler-types"></a>Tipos de hipervisor del programador

A partir de Windows Server 2016, el hipervisor de Hyper-V admite varios modos de lógica de programador, que determinan cómo el hipervisor programa procesadores virtuales en los procesadores lógicos subyacentes. Estos tipos de programador son:

- [El programador clásico, fair-share](#the-classic-scheduler)
- [El programador de core](#the-core-scheduler)
- [El programador de raíz](#the-root-scheduler)

### <a name="the-classic-scheduler"></a>El programador clásico

El programador clásico ha sido el valor predeterminado para todas las versiones del hipervisor de Hyper-V de Windows desde sus inicios, incluido Windows Server 2016 Hyper-V. El programador clásico proporciona equitativamente, modelo de programación preferente round-robin para procesadores virtuales invitados.

El tipo de programador clásico es la más adecuada para la mayoría de los usos de Hyper-V tradicionales: para las nubes privadas, los proveedores de hospedaje y así sucesivamente. Las características de rendimiento se entienden bien y son más adecuadas para admitir una amplia gama de escenarios de virtualización, como la sobresuscripción de VPs a LPs ejecuta muchas máquinas virtuales heterogéneos y cargas de trabajo al mismo tiempo, ejecutar mayor escala alta conjunto de rendimiento de las máquinas virtuales, que admiten la característica completa de Hyper-V sin restricciones y mucho más.

### <a name="the-core-scheduler"></a>El programador de core

El programador de hipervisor core es una alternativa nueva a la lógica de programador clásico, introducida en Windows Server 2016 y Windows 10 versión 1607. El programador de core ofrece un límite de seguridad sólidos para el aislamiento de la carga de trabajo de invitado y la variabilidad de disminución del rendimiento para cargas de trabajo dentro de máquinas virtuales que se ejecutan en un host de virtualización habilitada SMT. El programador de core permite SMT y que no son de SMT máquinas virtuales en ejecución simultáneamente en el mismo host de virtualización habilitada SMT.

El programador de core usa la topología SMT del host de virtualización y, opcionalmente, expone los pares SMT para las máquinas virtuales invitadas y las programaciones de grupos de procesadores virtuales invitados desde la misma máquina virtual en grupos de procesadores lógicos de SMT. Esto se hace simétricamente por lo que si LPs están en grupos de dos, VPs se programan en grupos de dos, y un núcleo nunca se comparte entre las máquinas virtuales.
Cuando está programado el Vicepresidente de una máquina virtual sin SMT habilitada, VP consumirá el núcleo completo cuando se ejecuta.

El resultado general del programador core es:

* VPs invitados están limitados a ejecutar en subyacente pares de núcleo físico, aislar una máquina virtual a los límites de núcleos de procesador, lo que reduce la vulnerabilidad a ataques de snooping del lado de canal desde máquinas virtuales malintencionadas.

* Se reduce significativamente la variabilidad del rendimiento.

* Potencialmente se reduce el rendimiento, ya que si solo se puede ejecutar uno de un grupo de VPs, solo uno de los flujos de instrucción en el núcleo ejecuta mientras el otro se queda inactivo.

* El sistema operativo y aplicaciones que se ejecutan en la máquina virtual invitada pueden utilizar el comportamiento SMT y las interfaces de programación (API) para controlar y distribuir el trabajo entre subprocesos SMT, igual que si se ejecutaran cuando no virtualizados.

* Un límite de seguridad sólida para el aislamiento de cargas de trabajo de invitado - VPs invitados están limitados a ejecutar en pares de núcleo físico subyacente, lo que reduce la vulnerabilidad a ataques de snooping del lado de canal.

El programador de núcleo se usará de forma predeterminada a partir de Windows Server 2019. En Windows Server 2016, el programador de núcleo es opcional y debe habilitarse explícitamente por el administrador del host de Hyper-V y el programador clásico es el valor predeterminado.

#### <a name="core-scheduler-behavior-with-host-smt-disabled"></a>Deshabilita el comportamiento del programador de Core con host SMT

Si el hipervisor está configurado para usar el tipo de planificador de core, pero la capacidad SMT está deshabilitado o no está presente en el host de virtualización, el hipervisor utilizará el comportamiento clásico de programador, independientemente de la configuración del tipo de hipervisor programador.

### <a name="the-root-scheduler"></a>El programador de raíz

El programador de raíz se introdujo con Windows 10, versión 1803. Cuando el tipo de planificador raíz está habilitado, el hipervisor cede el control de la programación de trabajo a la partición raíz. El programador de NT de la instancia de la partición raíz del sistema operativo administra todos los aspectos de la programación de trabajo al sistema de LPs.

El programador de raíz aborda los requisitos únicos inherentes con una partición de la utilidad de compatibilidad para proporcionar aislamiento de la carga de trabajo seguro, como se utiliza con Windows Defender Application Guard (WDAG). En este escenario, dejando las responsabilidades en la raíz del sistema operativo de programación ofrece varias ventajas. Por ejemplo, se pueden usar los controles de recursos de CPU aplicable a escenarios de contenedor con la partición de la utilidad, lo que simplifica la administración e implementación. Además, el programador del sistema operativo raíz puede recopilar métricas sobre el uso de CPU dentro del contenedor de la carga de trabajo fácilmente y usar estos datos como entrada para la misma directiva de programación aplicable a todas las otras cargas de trabajo en el sistema. Estas mismas métricas también ayudan a claramente atributo el trabajo realizado en un contenedor de aplicación en el sistema host. El seguimiento de estas métricas es más difícil con cargas de trabajo de máquinas virtuales tradicionales, donde algún trabajo en nombre de todos los máquina virtual en ejecución realiza en la partición raíz.

#### <a name="root-scheduler-use-on-client-systems"></a>Uso del programador de raíz en los sistemas cliente

A partir de Windows 10, versión 1803, el programador de raíz se usa de forma predeterminada en los sistemas cliente, donde el hipervisor puede habilitarse en apoyo de la seguridad basada en virtualización y aislamiento de la carga de trabajo WDAG y para el correcto funcionamiento de los sistemas con futuras arquitecturas de núcleo heterogéneos. Se trata de la configuración del programador de hipervisor compatible solo para los sistemas cliente. Los administradores no deben intentar reemplazar el tipo de planificador de hipervisor predeterminado en los sistemas de cliente de Windows 10.

#### <a name="virtual-machine-cpu-resource-controls-and-the-root-scheduler"></a>Los controles de recursos de CPU de la máquina virtual y el programador de raíz

Los controles de recursos del procesador de máquina virtual proporcionados por Hyper-V no se admiten cuando se habilita el programador de la raíz de hipervisor como lógica de programador del sistema operativo raíz administra recursos de host de forma global y no tiene conocimiento de la máquina virtual Opciones de configuración específicas. Los controles de recursos del procesador de por máquina virtual de Hyper-V, como CAP, ponderaciones y reservas, solo son aplicables cuando el hipervisor controla directamente el Vicepresidente de programación, como ocurre con los tipos de programador básico y estándar.

#### <a name="root-scheduler-use-on-server-systems"></a>Uso del programador de raíz en los sistemas de servidor

El programador de raíz no se recomienda para su uso con Hyper-V en los servidores en este momento, según sus características de rendimiento aún no se han totalmente caracterizadas y optimizar para dar cabida a la amplia gama de cargas de trabajo típicos de muchas de las implementaciones de virtualización de servidor.

## <a name="enabling-smt-in-guest-virtual-machines"></a>Habilitación de SMT en las máquinas virtuales invitadas

Una vez que el hipervisor del host de virtualización está configurado para usar el tipo de planificador de core, las máquinas virtuales invitadas puede configurarse para utilizar SMT si lo desea. Exponer el hecho de que VPs son hiperproceso a una máquina virtual invitada permite que el programador en el sistema operativo invitado y las cargas de trabajo que se ejecuta en la máquina virtual para detectar y usar la topología SMT en su propia programación de trabajo. En Windows Server 2016, SMT de invitado no está configurado de forma predeterminada y debe habilitarse explícitamente por el Administrador de Hyper-V host. A partir de Windows Server 2019, nuevas máquinas virtuales creadas en el host hereda la topología del host SMT de forma predeterminada.  Es decir, una versión que 9.0 de máquina virtual creada en un host con 2 subprocesos SMT por núcleo también verán 2 subprocesos SMT por núcleo.

Se debe usar PowerShell para habilitar SMT en una máquina virtual de invitado; No hay ninguna interfaz de usuario proporcionada en el Administrador de Hyper-V.
Para habilitar SMT en una máquina virtual invitada, abra una ventana de PowerShell con permisos suficientes y escriba:

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <n>
```

Donde <n> es el número de subprocesos SMT por núcleo del Invitado VM verá.  
Tenga en cuenta que <n> = 0 se establecerá el valor de HwThreadCountPerCore para que coincida con el número de subprocesos SMT del host por cada valor de núcleo.

>[!NOTE] 
>Establecer HwThreadCountPerCore = 0 se admite a partir de Windows Server 2019.

A continuación es un ejemplo de la información de sistema procedente del sistema operativo invitado que se ejecuta en una máquina virtual con 2 procesadores virtuales y SMT habilitado. El sistema operativo invitado es detectar 2 procesadores lógicos que pertenecen al mismo principal.

![Captura de pantalla que muestra msinfo32 en un Invitado VM con SMT habilitado](media/Hyper-V-CoreScheduler-VM-Msinfo32.png)

## <a name="configuring-the-hypervisor-scheduler-type-on-windows-server-2016-hyper-v"></a>Configuración del tipo de programador de hipervisor de Windows Server 2016 Hyper-V

Windows Server 2016 Hyper-V usa el modelo de programador de hipervisor clásico de forma predeterminada. El hipervisor puede configurarse opcionalmente para usar al programador de core, para aumentar la seguridad mediante la restricción de invitado VPs para ejecutarse en pares SMT físicos correspondientes y para admitir el uso de máquinas virtuales con la programación de SMT para sus VPs invitado.

>[!NOTE]
>Microsoft recomienda que todos los clientes que ejecutan Windows Server 2016 Hyper-v. seleccionan el programador de núcleo para asegurarse de que sus hosts de virtualización óptimamente están protegidos frente a las máquinas virtuales invitadas potencialmente malintencionado.

## <a name="windows-server-2019-hyper-v-defaults-to-using-the-core-scheduler"></a>Valores predeterminados de Windows Server 2019 Hyper-V para usar al programador de core

Para ayudar a garantizar que los hosts de Hyper-V se implementan en la configuración de seguridad óptima, Windows Server 2019 Hyper-V ahora usará el modelo de programador de hipervisor principal de forma predeterminada. El administrador del host, opcionalmente, puede configurar el host para usar al programador clásico heredado. Los administradores cuidadosamente deben leer, comprender y considerar los efectos de que cada tipo de programador tiene en la seguridad y el rendimiento de hosts de virtualización antes de reemplazar la configuración predeterminada del tipo de programador.  Consulte [selección del tipo de descripción de Hyper-V programador](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/understanding-hyper-v-scheduler-type-selection) para obtener más información.

### <a name="required-updates"></a>Actualizaciones necesarias

>[!NOTE]
>Las siguientes actualizaciones son necesarias para usar las características de programador de hipervisor se describe en este documento. Estas actualizaciones incluyen cambios para admitir la nueva opción de BCD 'hypervisorschedulertype', que es necesaria para la configuración del host.

| `Version` | Publicación  | Se requiere la actualización | Artículo de Knowledge Base |
|--------------------|------|---------|-------------:|
|Windows Server 2016 | 1607 | 2018.07 C | [KB4338822](https://support.microsoft.com/help/4338822/windows-10-update-kb4338822) |
|Windows Server 2016 | 1703 | 2018.07 C | [KB4338827](https://support.microsoft.com/help/4338827/windows-10-update-kb4338827) |
|Windows Server 2016 | 1709 | 2018.07 C | [KB4338817](https://support.microsoft.com/help/4338817/windows-10-update-kb4338817) |
|Windows Server 2019 | 1804 | Ninguno | Ninguno |

## <a name="selecting-the-hypervisor-scheduler-type-on-windows-server"></a>Seleccionar el tipo de hipervisor programador en Windows Server

La configuración del programador de hipervisor se controla a través de la entrada de BCD hypervisorschedulertype.

Para seleccionar un tipo de programador, abra un símbolo del sistema con privilegios de administrador:

``` command
     bcdedit /set hypervisorschedulertype type
```

Donde `type` es uno de:

* Clásico
* Core
* Raíz

El sistema debe reiniciarse para que los cambios en el tipo de hipervisor programador surta efecto.

>[!NOTE]
>El programador de hipervisor raíz no se admite en Windows Server Hyper-V en este momento. Los administradores de Hyper-V no deben intentar configurar al programador de raíz para su uso con escenarios de virtualización de servidor.

## <a name="determining-the-current-scheduler-type"></a>Determinar el tipo de programador actual

Puede determinar el tipo de programador de hipervisor actual en uso examinando el registro del sistema en el Visor de eventos para el evento de lanzamiento más reciente de hipervisor identificador 2, que indica el tipo de planificador de hipervisor configurado en el lanzamiento de hipervisor. Eventos de lanzamiento de hipervisor pueden obtenerse desde el Visor de eventos de Windows, o a través de PowerShell.

Evento de lanzamiento de hipervisor identificador 2 denota el tipo de programador de hipervisor, donde:

    1 = Classic scheduler, SMT disabled

    2 = Classic scheduler

    3 = Core scheduler

    4 = Root scheduler

![Mostrar detalles de evento 2 de Id. de inicio de hipervisor de captura de pantalla](media/Hyper-V-CoreScheduler-EventID2-Details.png)

![Captura de pantalla que muestra el Visor de eventos muestra 2 Id. de evento de lanzamiento de hipervisor](media/Hyper-V-CoreScheduler-EventViewer.png)

### <a name="querying-the-hyper-v-hypervisor-scheduler-type-launch-event-using-powershell"></a>Consultar el evento de lanzamiento de tipo de programador para Hyper-V hipervisor con PowerShell

Consulta de evento de hipervisor identificador 2 mediante PowerShell, escriba los siguientes comandos desde un símbolo del sistema de PowerShell.

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Hypervisor"; ID=2} -MaxEvents 1
```

![Captura de pantalla que muestra la consulta de PowerShell y los resultados de evento de lanzamiento de hipervisor identificador 2](media/Hyper-V-CoreScheduler-PowerShell.png)
