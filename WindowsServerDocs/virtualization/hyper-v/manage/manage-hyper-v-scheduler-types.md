---
title: Descripción y uso de los tipos de programador de hipervisor de Hyper-V
description: Proporciona información para los administradores de hosts de Hyper-V sobre el uso de los modos de programador de Hyper-V.
author: allenma
ms.author: allenma
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows-server-hyper-v
ms.technology: virtualization
ms.localizationpriority: low
ms.assetid: 6cb13f84-cb50-4e60-a685-54f67c9146be
ms.openlocfilehash: c7c2de8354d067faf0dcf1787c3e178421e2ac03
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872026"
---
# <a name="managing-hyper-v-hypervisor-scheduler-types"></a>Administrar tipos de programador de hipervisor de Hyper-V

>Se aplica a: Windows 10, Windows Server 2016, Windows Server, versión 1709, Windows Server, versión 1803, Windows Server 2019

En este artículo se describen los nuevos modos de la lógica de programación del procesador virtual que se presentó por primera vez en Windows Server 2016. Estos modos, o tipos de programador, determinan cómo el hipervisor de Hyper-V asigna y administra el trabajo en los procesadores virtuales invitados. Un administrador de hosts de Hyper-V puede seleccionar los tipos de programador de hipervisor más adecuados para las máquinas virtuales (VM) invitadas y configurar las máquinas virtuales para aprovechar la lógica de programación.

>[!NOTE]
>Las actualizaciones son necesarias para usar las características del programador de hipervisor descritas en este documento. Para obtener más información, consulte [actualizaciones necesarias](#required-updates).

## <a name="background"></a>Background

Antes de analizar la lógica y los controles que hay detrás de la programación del procesador virtual de Hyper-V, resulta útil revisar los conceptos básicos que se tratan en este artículo.

### <a name="understanding-smt"></a>Descripción de SMT

Multithreading simultáneo, o SMT, es una técnica que se utiliza en los diseños de procesador modernos que permite compartir los recursos del procesador con subprocesos de ejecución independientes e independientes. SMT suele ofrecer un aumento modesto en el rendimiento de la mayoría de las cargas de trabajo mediante la ejecución en paralelo de los cálculos cuando sea posible, lo que aumenta el rendimiento de las instrucciones, aunque no se produzca ninguna mejora de rendimiento o incluso una ligera pérdida de rendimiento cuando se contención entre subprocesos para se producen recursos compartidos del procesador.
Los procesadores que admiten SMT están disponibles en Intel y AMD. Intel hace referencia a sus ofertas de SMT como tecnología Intel Hyper Threading o Intel HT.

Para los fines de este artículo, las descripciones de SMT y cómo las utiliza Hyper-V se aplican igualmente a los sistemas Intel y AMD.

* Para obtener más información sobre la tecnología Intel HT, consulte [tecnología Hyper-Threading de Intel](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html) .

* Para obtener más información sobre AMD SMT, consulte [la arquitectura básica de "Zen"](https://www.amd.com/en/technologies/zen-core)

## <a name="understanding-how-hyper-v-virtualizes-processors"></a>Descripción de cómo Hyper-V Virtualiza los procesadores

Antes de considerar los tipos de programador de hipervisor, también resulta útil comprender la arquitectura de Hyper-V. Puede encontrar un resumen general en [información general sobre la tecnología de Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-technology-overview). Estos son los conceptos importantes de este artículo:

* Hyper-V crea y administra particiones de máquinas virtuales, en las que los recursos de proceso están asignados y compartidos, bajo el control del hipervisor. Las particiones proporcionan límites de aislamiento fuertes entre todas las máquinas virtuales invitadas y entre las máquinas virtuales invitadas y la partición raíz.

* La partición raíz es en sí misma una partición de máquina virtual, aunque tiene propiedades únicas y privilegios mucho mayores que las máquinas virtuales invitadas. La partición raíz proporciona los servicios de administración que controlan todas las máquinas virtuales invitadas, proporcionan compatibilidad con dispositivos virtuales para los invitados y administran todas las e/s de dispositivos para las máquinas virtuales invitadas. Microsoft recomienda no ejecutar cargas de trabajo de aplicaciones en la partición raíz.

* Cada procesador virtual (VP) de la partición raíz se asigna 1:1 a un procesador lógico subyacente (LP). Un Vicepresidente de host siempre se ejecutará en el mismo LP subyacente, ya que no se realiza ninguna migración del VPs de la partición raíz.

* De forma predeterminada, el LPs en el que se ejecuta VPs de host también puede ejecutar el VPs invitado.

* El hipervisor puede programar un Vicepresidente de invitado para que se ejecute en cualquier procesador lógico disponible. Aunque el programador del hipervisor se encarga de considerar la ubicación de la caché temporal, la topología NUMA y muchos otros factores al programar un Vicepresidente invitado, en última instancia, el VP podría programarse en cualquier host LP.

## <a name="hypervisor-scheduler-types"></a>Tipos de programador de hipervisor

A partir de Windows Server 2016, el hipervisor de Hyper-V admite varios modos de lógica del programador, que determinan cómo programa el hipervisor los procesadores virtuales en los procesadores lógicos subyacentes. Estos tipos de programador son:

- [El programador clásico de recursos compartidos](#the-classic-scheduler)
- [Programador principal](#the-core-scheduler)
- [Programador raíz](#the-root-scheduler)

### <a name="the-classic-scheduler"></a>Programador clásico

El programador clásico es el valor predeterminado para todas las versiones del hipervisor de Hyper-V de Windows desde su inicio, incluido Windows Server 2016 Hyper-V. El programador clásico proporciona un modelo de programación de uso compartido equitativo y preventivo para los procesadores virtuales invitados.

El tipo de programador clásico es el más apropiado para la mayoría de los usos tradicionales de Hyper-V: para nubes privadas, proveedores de hospedaje, etc. Las características de rendimiento se entienden bien y se optimizan mejor para admitir una amplia gama de escenarios de virtualización, como la suscripción excesiva de VPs a LPs, la ejecución simultánea de muchas máquinas virtuales y cargas de trabajo heterogéneas. Máquinas virtuales de rendimiento, que admiten el conjunto completo de características de Hyper-V sin restricciones y mucho más.

### <a name="the-core-scheduler"></a>Programador principal

Hipervisor Core Scheduler es una nueva alternativa a la lógica del programador clásico, que se incorporó en Windows Server 2016 y Windows 10, versión 1607. El programador principal ofrece un límite de seguridad fuerte para el aislamiento de la carga de trabajo invitado y la variabilidad del rendimiento reducida para las cargas de trabajo dentro de las máquinas virtuales que se ejecutan en un host de virtualización con SMT. El programador principal permite ejecutar máquinas virtuales de SMT y no SMT simultáneamente en el mismo host de virtualización compatible con SMT.

El programador principal emplea la topología SMT del host de virtualización y, opcionalmente, expone los pares SMT a las máquinas virtuales invitadas y programa los grupos de procesadores virtuales invitados de la misma máquina virtual en grupos de procesadores lógicos SMT. Esto se realiza de forma simétrica, de modo que si LPs se encuentran en grupos de dos, los VPs se programan en grupos de dos, y un núcleo nunca se comparte entre máquinas virtuales.
Cuando el VP está programado para una máquina virtual sin SMT habilitado, esa VP consumirá todo el núcleo cuando se ejecute.

El resultado general del programador principal es que:

* Los VPs invitados están restringidos para ejecutarse en pares de núcleos físicos subyacentes, aislando una máquina virtual en límites de núcleos de procesador, lo que reduce la vulnerabilidad en los ataques de supervisión de canal lateral de máquinas virtuales malintencionadas.

* La variabilidad del rendimiento se reduce significativamente.

* El rendimiento puede reducirse, ya que si solo se puede ejecutar uno de los grupos de VPs, solo se ejecutará uno de los flujos de instrucciones del núcleo mientras el otro se quede inactivo.

* El sistema operativo y las aplicaciones que se ejecutan en la máquina virtual invitada pueden usar interfaces de programación y comportamiento de SMT (API) para controlar y distribuir el trabajo a través de subprocesos SMT, tal como lo haría cuando se ejecutara de forma no virtualizada.

* Un límite de seguridad fuerte para el aislamiento de la carga de trabajo invitado: los VPs de invitado están restringidos para ejecutarse en pares de núcleos físicos subyacentes, lo que reduce la vulnerabilidad a los ataques de supervisión de canal lateral.

El programador principal se usará de forma predeterminada a partir de Windows Server 2019. En Windows Server 2016, el programador principal es opcional y debe habilitarse explícitamente mediante el administrador del host de Hyper-V, y el programador clásico es el predeterminado.

#### <a name="core-scheduler-behavior-with-host-smt-disabled"></a>Comportamiento del programador principal con SMT de host deshabilitado

Si el hipervisor está configurado para usar el tipo de programador principal pero la capacidad de SMT está deshabilitada o no está presente en el host de virtualización, el hipervisor usará el comportamiento del programador clásico, independientemente de la configuración de tipo de programador del hipervisor.

### <a name="the-root-scheduler"></a>Programador raíz

El programador raíz se presentó con la versión 1803 de Windows 10. Cuando el tipo de programador raíz está habilitado, el hipervisor tiene el control de la programación del trabajo en la partición raíz. El programador de NT de la instancia de sistema operativo de la partición raíz administra todos los aspectos del trabajo de programación en LPs del sistema.

El programador raíz aborda los requisitos únicos inherentes a la compatibilidad con una partición de utilidad para proporcionar aislamiento de carga de trabajo seguro, como se usa con protección de aplicaciones de Windows Defender (WDAG). En este escenario, el abandono de responsabilidades de programación en el sistema operativo raíz ofrece varias ventajas. Por ejemplo, los controles de recursos de CPU aplicables a los escenarios de contenedor se pueden usar con la partición de la utilidad, lo que simplifica la administración y la implementación. Además, el programador del sistema operativo raíz puede recopilar fácilmente métricas sobre el uso de CPU de la carga de trabajo dentro del contenedor y usar estos datos como entrada para la misma directiva de programación aplicable a todas las demás cargas de trabajo del sistema. Estas mismas métricas también ayudan a atribuir claramente el trabajo realizado en un contenedor de la aplicación en el sistema host. Realizar el seguimiento de estas métricas es más difícil con las cargas de trabajo de máquina virtual tradicionales, donde algunos trabajos en el nombre de todas las máquinas virtuales se ejecutan en la partición raíz.

#### <a name="root-scheduler-use-on-client-systems"></a>Uso del programador raíz en los sistemas cliente

A partir de la versión 1803 de Windows 10, el programador raíz se utiliza de forma predeterminada en los sistemas cliente únicamente, donde el hipervisor puede estar habilitado para admitir la seguridad basada en la virtualización y el aislamiento de la carga de trabajo de WDAG, y para un correcto funcionamiento de sistemas futuros con arquitecturas de núcleos heterogéneas. Esta es la única configuración de programador de hipervisor admitida para los sistemas cliente. Los administradores no deben intentar invalidar el tipo de programador de hipervisor predeterminado en los sistemas cliente de Windows 10.

#### <a name="virtual-machine-cpu-resource-controls-and-the-root-scheduler"></a>Controles de recursos de CPU de la máquina virtual y el programador raíz

Los controles de recursos del procesador de la máquina virtual proporcionados por Hyper-V no se admiten cuando el programador raíz del hipervisor está habilitado, ya que la lógica del programador del sistema operativo raíz es la administración de los recursos de host de forma global y no tiene conocimiento de las máquinas virtuales. Opciones de configuración específicas. Los controles de recursos del procesador por máquina virtual de Hyper-V, como los Cap, los pesos y las reservas, solo se aplican cuando el hipervisor controla directamente la programación de VP, como con los tipos de programador clásico y principal.

#### <a name="root-scheduler-use-on-server-systems"></a>Uso del programador raíz en los sistemas de servidor

El programador raíz no se recomienda para su uso con Hyper-V en servidores en este momento, ya que sus características de rendimiento todavía no se han caracterizado y optimizado para adaptarse a la amplia gama de cargas de trabajo típicas de muchas implementaciones de virtualización de servidor.

## <a name="enabling-smt-in-guest-virtual-machines"></a>Habilitación de SMT en máquinas virtuales invitadas

Una vez que el hipervisor del host de virtualización está configurado para usar el tipo de programador principal, las máquinas virtuales invitadas se pueden configurar para utilizar SMT si se desea. Al exponer el hecho de que los VPs de subprocesos en una máquina virtual invitada, el programador del sistema operativo invitado y las cargas de trabajo que se ejecutan en la máquina virtual detectan y usan la topología de SMT en su propia programación de trabajo. En Windows Server 2016, el SMT de invitado no está configurado de forma predeterminada y debe habilitarse explícitamente mediante el administrador del host de Hyper-V. A partir de Windows Server 2019, las nuevas máquinas virtuales creadas en el host heredarán de forma predeterminada la topología SMT del host.  Es decir, una máquina virtual de la versión 9,0 creada en un host con 2 subprocesos SMT por núcleo también verá 2 subprocesos SMT por núcleo.

PowerShell debe usarse para habilitar SMT en una máquina virtual invitada; no se ha proporcionado ninguna interfaz de usuario en el administrador de Hyper-V.
Para habilitar SMT en una máquina virtual invitada, abra una ventana de PowerShell con permisos suficientes y escriba:

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <n>
```

Donde <n> es el número de subprocesos de SMT por núcleo que verá la máquina virtual invitada.  
Tenga en <n> cuenta que = 0 establecerá el valor de HwThreadCountPerCore para que coincida con el número de subprocesos SMT del host por valor principal.

>[!NOTE] 
>La configuración de HwThreadCountPerCore = 0 se admite a partir de Windows Server 2019.

A continuación se muestra un ejemplo de información del sistema tomada del sistema operativo invitado que se ejecuta en una máquina virtual con dos procesadores virtuales y SMT habilitados. El sistema operativo invitado está detectando dos procesadores lógicos que pertenecen al mismo núcleo.

![Captura de pantalla que muestra msinfo32 en una máquina virtual invitada con SMT habilitado](media/Hyper-V-CoreScheduler-VM-Msinfo32.png)

## <a name="configuring-the-hypervisor-scheduler-type-on-windows-server-2016-hyper-v"></a>Configuración del tipo de programador de hipervisor en Windows Server 2016 Hyper-V

Windows Server 2016 Hyper-V usa el modelo de programador de hipervisor clásico de forma predeterminada. El hipervisor se puede configurar opcionalmente para usar el programador principal, con el fin de aumentar la seguridad mediante la restricción de los VPs de invitado para que se ejecuten en los pares de SMT físicos correspondientes y para admitir el uso de máquinas virtuales con la programación SMT para sus VPs de invitados.

>[!NOTE]
>Microsoft recomienda que todos los clientes que ejecutan Windows Server 2016 Hyper-V seleccionen el programador principal para asegurarse de que sus hosts de virtualización estén protegidos de manera óptima frente a máquinas virtuales invitadas potencialmente malintencionadas.

## <a name="windows-server-2019-hyper-v-defaults-to-using-the-core-scheduler"></a>Windows Server 2019, de forma predeterminada, Hyper-V usa el programador principal

Para ayudar a garantizar que los hosts de Hyper-V se implementan en la configuración de seguridad óptima, Windows Server 2019 Hyper-V ahora usará el modelo de programador del hipervisor principal de forma predeterminada. Opcionalmente, el administrador del host puede configurar el host para usar el programador clásico heredado. Los administradores deben leer detenidamente, comprender y considerar los efectos que cada tipo de programador tiene en la seguridad y el rendimiento de los hosts de virtualización antes de invalidar la configuración predeterminada del tipo de programador.  Consulte [Descripción de la selección del tipo de programador de Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/understanding-hyper-v-scheduler-type-selection) para obtener más información.

### <a name="required-updates"></a>Actualizaciones necesarias

>[!NOTE]
>Las siguientes actualizaciones son necesarias para usar las características del programador de hipervisor descritas en este documento. Estas actualizaciones incluyen cambios para admitir la nueva opción de BCD ' hypervisorschedulertype ', que es necesaria para la configuración del host.

| `Version` | Release  | Actualización necesaria | Artículo de Knowledge base |
|--------------------|------|---------|-------------:|
|Windows Server 2016 | 1607 | 2018,07 C | [KB4338822](https://support.microsoft.com/help/4338822/windows-10-update-kb4338822) |
|Windows Server 2016 | 1703 | 2018,07 C | [KB4338827](https://support.microsoft.com/help/4338827/windows-10-update-kb4338827) |
|Windows Server 2016 | 1709 | 2018,07 C | [KB4338817](https://support.microsoft.com/help/4338817/windows-10-update-kb4338817) |
|Windows Server 2019 | 1804 | None | None |

## <a name="selecting-the-hypervisor-scheduler-type-on-windows-server"></a>Selección del tipo de programador de hipervisor en Windows Server

La configuración del programador del hipervisor se controla a través de la entrada BCD de hypervisorschedulertype.

Para seleccionar un tipo de programador, abra un símbolo del sistema con privilegios de administrador:

``` command
     bcdedit /set hypervisorschedulertype type
```

Donde `type` es uno de los siguientes:

* Clásico
* Core
* Raíz

El sistema debe reiniciarse para que los cambios en el tipo de programador del hipervisor surtan efecto.

>[!NOTE]
>En este momento, no se admite el programador raíz de hipervisor en Windows Server Hyper-V. Los administradores de Hyper-V no deben intentar configurar el programador raíz para su uso con escenarios de virtualización de servidores.

## <a name="determining-the-current-scheduler-type"></a>Determinar el tipo de programador actual

Puede determinar el tipo de programador de hipervisor actual en uso examinando el registro del sistema en Visor de eventos para el evento de inicio de hipervisor más reciente con el identificador 2, que notifica el tipo de programador de hipervisor configurado en el inicio del hipervisor. Los eventos de inicio de hipervisor se pueden obtener de la Visor de eventos de Windows o a través de PowerShell.

El evento de inicio de hipervisor con ID. 2 denota el tipo de programador de hipervisor, donde:

    1 = Classic scheduler, SMT disabled

    2 = Classic scheduler

    3 = Core scheduler

    4 = Root scheduler

![Captura de pantalla que muestra el ID. de evento de inicio de hipervisor 2 detalles](media/Hyper-V-CoreScheduler-EventID2-Details.png)

![Captura de pantalla que muestra Visor de eventos mostrar el ID. de evento de inicio de hipervisor 2](media/Hyper-V-CoreScheduler-EventViewer.png)

### <a name="querying-the-hyper-v-hypervisor-scheduler-type-launch-event-using-powershell"></a>Consultar el evento de inicio de tipo programador del hipervisor de Hyper-V mediante PowerShell

Para consultar el ID. de evento 2 del hipervisor mediante PowerShell, escriba los siguientes comandos desde un símbolo del sistema de PowerShell.

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Hypervisor"; ID=2} -MaxEvents 1
```

![Captura de pantalla que muestra la consulta y los resultados de PowerShell para el evento de inicio de hipervisor con ID. 2](media/Hyper-V-CoreScheduler-PowerShell.png)
