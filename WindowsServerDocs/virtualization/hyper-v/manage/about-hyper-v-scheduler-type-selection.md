---
title: Acerca de la selección del tipo de programador de hipervisor de Hyper-V
description: Obtenga información acerca de los cambios importantes en el uso predeterminado y recomendado de Hyper-V de los tipos de programador de hipervisor.
ms.author: benarm
author: BenjaminArmstrong
ms.date: 08/14/2018
ms.topic: article
ms.localizationpriority: low
ms.assetid: 5fe163d4-2595-43b0-ba2f-7fad6e4ae069
ms.openlocfilehash: 0e3dfd0d6a2ce4f7996efa42f74bcae3bc07833d
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/01/2021
ms.locfileid: "97845955"
---
# <a name="about-hyper-v-hypervisor-scheduler-type-selection"></a>Acerca de la selección del tipo de programador de hipervisor de Hyper-V

Se aplica a:

* Windows Server 2016
* Windows Server, versión 1709
* Windows Server, versión 1803
* Windows Server 2019

En este documento se describen los cambios importantes en el uso predeterminado y recomendado de Hyper-V de los tipos de programador de hipervisor. Estos cambios afectan tanto al rendimiento de virtualización como a la seguridad del sistema. Los administradores del host de virtualización deben revisar y comprender los cambios y las implicaciones que se describen en este documento y evaluar cuidadosamente los impactos, la guía de implementación sugerida y los factores de riesgo implicados para comprender mejor cómo implementar y administrar los hosts de Hyper-V en el panorama de la seguridad que cambia rápidamente.

>[!IMPORTANT]
>En la actualidad, las vulnerabilidades de seguridad de canal lateral conocidas en varias arquitecturas de procesador podrían ser aprovechadas por una máquina virtual invitada malintencionada a través del comportamiento de programación del tipo de programador clásico de hipervisor heredado cuando se ejecutan en hosts con multithreading (SMT) simultáneo habilitado.  Si se aprovecha correctamente, una carga de trabajo malintencionada podría observar los datos fuera del límite de la partición. Esta clase de ataques se puede mitigar configurando el hipervisor de Hyper-V para usar el tipo de programador Core del hipervisor y volver a configurar las máquinas virtuales invitadas. Con el programador principal, el hipervisor restringe la VPs de una máquina virtual invitada para que se ejecute en el mismo núcleo del procesador físico, con lo que se aísla fuertemente la capacidad de la máquina virtual para acceder a los datos a los límites del núcleo físico en el que se ejecuta.  Se trata de una mitigación muy eficaz contra estos ataques de canal secundario, lo que impide que la máquina virtual Observe los artefactos de otras particiones, ya sea la raíz u otra partición invitada.  Por lo tanto, Microsoft está cambiando la configuración predeterminada y recomendada para los hosts de virtualización y las máquinas virtuales invitadas.

## <a name="background"></a>Información previa

A partir de Windows Server 2016, Hyper-V admite varios métodos de programación y administración de procesadores virtuales, denominados tipos de programador de hipervisor.  Puede encontrar una descripción detallada de todos los tipos de programador de hipervisor en [comprender y usar los tipos de programador de hipervisor de Hyper-V](./manage-hyper-v-scheduler-types.md).

>[!NOTE]
>Los nuevos tipos de programador de hipervisor se introdujeron por primera vez con Windows Server 2016 y no están disponibles en versiones anteriores. Todas las versiones de Hyper-V anteriores a Windows Server 2016 solo admiten el programador clásico. La compatibilidad con el programador básico solo se publicó recientemente.

## <a name="about-hypervisor-scheduler-types"></a>Acerca de los tipos de programador de hipervisor

Este artículo se centra específicamente en el uso del nuevo tipo de programador de núcleos de hipervisor frente al programador "clásico" heredado, y cómo estos tipos de Scheduler intersecan con el uso de multithreading simétrico o SMT.  Es importante comprender las diferencias entre los programadores principal y clásico, y cómo funciona cada uno de ellos desde las máquinas virtuales invitadas en los procesadores del sistema subyacentes.

### <a name="the-classic-scheduler"></a>Programador clásico

El programador clásico hace referencia al recurso compartido equitativo round robin método de programación del trabajo en los procesadores virtuales (VPs) en el sistema, incluidos los VPs raíz y VPs que pertenecen a las máquinas virtuales invitadas. El programador clásico ha sido el tipo de programador predeterminado que se usa en todas las versiones de Hyper-V (hasta Windows Server 2019, tal y como se describe en este documento).  Las características de rendimiento del programador clásico se entienden bien y se muestra el programador clásico para ably compatibilidad con suscripciones de cargas de trabajo, es decir, por exceso de suscripción de la proporción VP: LP del host en un margen razonable (en función de los tipos de cargas de trabajo que se virtualizan, uso general de recursos, etc.).

Cuando se ejecuta en un host de virtualización con SMT habilitado, el programador clásico programará VPs de invitado desde cualquier máquina virtual de cada subproceso de SMT que pertenezca a un núcleo de forma independiente. Por lo tanto, se pueden ejecutar máquinas virtuales diferentes en el mismo núcleo al mismo tiempo (una máquina virtual que se ejecuta en un subproceso de un núcleo mientras otra máquina virtual se está ejecutando en otro subproceso).

### <a name="the-core-scheduler"></a>Programador principal

El programador principal aprovecha las propiedades de SMT para proporcionar aislamiento de las cargas de trabajo de invitado, lo que afecta al rendimiento del sistema y de la seguridad. El programador principal garantiza que VPs de una máquina virtual se programan en subprocesos SMT relacionados. Esto se hace de forma simétrica, de modo que si LPs se encuentran en grupos de dos, los VPs se programan en grupos de dos, y un núcleo de CPU del sistema nunca se comparte entre las máquinas virtuales.

Mediante la programación de VPs de invitado en pares SMT subyacentes, el programador principal ofrece un límite de seguridad fuerte para el aislamiento de la carga de trabajo y también se puede usar para reducir la variabilidad del rendimiento para cargas de trabajo sensibles a la latencia.

Tenga en cuenta que cuando el VP está programado para una máquina virtual sin SMT habilitado, esa VP consumirá todo el núcleo cuando se ejecute, y el subproceso relacionado de SMT del núcleo quedará inactivo.  Esto es necesario para proporcionar el aislamiento de la carga de trabajo correcto, pero afecta al rendimiento general del sistema, sobre todo cuando el sistema LPs se Suscríbase a la suscripción, es decir, cuando la proporción de VP: LP sea superior a 1:1. Por lo tanto, la ejecución de máquinas virtuales de invitado configuradas sin varios subprocesos por núcleo es una configuración poco óptima.

### <a name="benefits-of-the-using-the-core-scheduler"></a>Ventajas del uso del programador principal

El programador principal ofrece las siguientes ventajas:

* Un límite de seguridad fuerte para el aislamiento de la carga de trabajo invitado: los VPs de invitado están restringidos para ejecutarse en pares de núcleos físicos subyacentes, lo que reduce la vulnerabilidad a los ataques de supervisión de canal lateral.

* Reducción de la carga de trabajo: la variabilidad del rendimiento de la carga de trabajo invitado se reduce significativamente, lo que ofrece mayor coherencia en la carga de trabajo.

* Uso de SMT en máquinas virtuales invitadas: el sistema operativo y las aplicaciones que se ejecutan en la máquina virtual invitada pueden usar las interfaces de programación y el comportamiento de SMT (API) para controlar y distribuir el trabajo a través de subprocesos SMT, tal y como lo haría cuando se ejecutara de forma no virtualizada.

El programador principal se usa actualmente en los hosts de virtualización de Azure, específicamente para aprovechar las ventajas de los límites de seguridad fuertes y de baja carga de trabajo variabilty. Microsoft considera que el tipo de programador principal debe ser y seguirá siendo el tipo de programación de hipervisor predeterminado para la mayoría de los escenarios de virtualización.  Por lo tanto, para asegurarse de que nuestros clientes son seguros de forma predeterminada, Microsoft está realizando este cambio ahora en Windows Server 2019.

### <a name="core-scheduler-performance-impacts-on-guest-workloads"></a>Impactos principales en el rendimiento del programador en las cargas de trabajo de invitado

Aunque es necesario para mitigar eficazmente ciertas clases de vulnerabilidades, el programador principal también puede reducir el rendimiento. Los clientes pueden ver una diferencia en las características de rendimiento con sus máquinas virtuales e impactos en la capacidad general de carga de trabajo de sus hosts de virtualización. En los casos en que el programador principal debe ejecutar un vicepresidente que no sea de SMT, solo se ejecutará una de las secuencias de instrucciones en el núcleo lógico subyacente, mientras que el otro debe dejarse inactivo. Esto limitará la capacidad total del host para las cargas de trabajo de invitado.

Estos impactos de rendimiento se pueden minimizar siguiendo las instrucciones de implementación de este documento. Los administradores de host deben considerar detenidamente sus escenarios de implementación de virtualización específicos y equilibrar su tolerancia por riesgo de seguridad frente a la necesidad de densidad máxima de carga de trabajo, consolidación de hosts de virtualización, etc.

## <a name="changes-to-the-default-and-recommended-configurations-for-windows-server-2016-and-windows-server-2019"></a>Cambios en las configuraciones predeterminadas y recomendadas para Windows Server 2016 y Windows Server 2019

La implementación de hosts de Hyper-V con la postura de seguridad máxima requiere el uso del tipo de programador Core de hipervisor. Para asegurarse de que nuestros clientes son seguros de forma predeterminada, Microsoft está cambiando la siguiente configuración predeterminada y recomendada.

>[!NOTE]
>Aunque la compatibilidad interna del hipervisor con los tipos de programador se incluyó en la versión inicial de Windows Server 2016, Windows Server 1709 y Windows Server 1803, se necesitan actualizaciones para tener acceso al control de configuración, lo que permite seleccionar el tipo de programador de hipervisor.  Consulte [y uso de los tipos de programador de hipervisor de Hyper-V](./manage-hyper-v-scheduler-types.md) para obtener más información sobre estas actualizaciones.

### <a name="virtualization-host-changes"></a>Cambios en el host de virtualización

* El hipervisor usará el programador principal de forma predeterminada a partir de Windows Server 2019.

* Microsoft reccommends configurar el programador básico en Windows Server 2016. El tipo de programador principal del hipervisor es compatible con Windows Server 2016, pero el valor predeterminado es el programador clásico. El programador principal es opcional y debe habilitarse explícitamente mediante el administrador del host de Hyper-V.

### <a name="virtual-machine-configuration-changes"></a>Cambios de configuración de la máquina virtual

* En Windows Server 2019, las nuevas máquinas virtuales creadas con la versión de VM predeterminada 9,0 heredarán automáticamente las propiedades de SMT (habilitadas o deshabilitadas) del host de virtualización. Es decir, si SMT está habilitado en el host físico, las máquinas virtuales recién creadas también tendrán SMT habilitado y heredarán la topología SMT del host de forma predeterminada, con la máquina virtual que tiene el mismo número de subprocesos de hardware por núcleo que el sistema subyacente. Esto se reflejará en la configuración de la máquina virtual con HwThreadCountPerCore = 0, donde 0 indica que la máquina virtual debe heredar la configuración de SMT del host.

* Las máquinas virtuales existentes con una versión de máquina virtual de 8,2 o anterior conservarán su configuración de procesador de máquina virtual original para HwThreadCountPerCore, y el valor predeterminado para los invitados de versión de máquina virtual 8,2 es HwThreadCountPerCore = 1. Cuando estos invitados se ejecuten en un host de Windows Server 2019, se tratarán de la siguiente manera:

    1. Si la máquina virtual tiene un número de VP inferior o igual al recuento de núcleos LP, el programador principal tratará la máquina virtual como una máquina virtual que no sea de SMT. Cuando el VP de invitados se ejecuta en un único subproceso de SMT, se inactivará el subproceso de SMT relacionado del núcleo. Esto no es óptimo y dará lugar a una pérdida general de rendimiento.

    2. Si la máquina virtual tiene más VPs que los núcleos de LP, el programador principal tratará la máquina virtual como una máquina virtual de SMT. Sin embargo, la máquina virtual no observará otras indicaciones de que es una máquina virtual de SMT. Por ejemplo, el uso de la instrucción CPUID o las API de Windows para consultar la CPU topología por el sistema operativo o las aplicaciones no indicarán que SMT está habilitado.

* Cuando una máquina virtual existente se actualiza explícitamente de las versiones de máquina virtual anteriores a la versión 9,0 a través de la operación Update-VM, la máquina virtual conservará su valor actual para HwThreadCountPerCore.  La máquina virtual no tendrá habilitada la fuerza de SMT.

* En Windows Server 2016, Microsoft recomienda habilitar SMT para las máquinas virtuales invitadas.  De forma predeterminada, las máquinas virtuales creadas en Windows Server 2016 tendrían SMT deshabilitado, es decir, HwThreadCountPerCore se establece en 1, a menos que se cambie explícitamente.

>[!NOTE]
>Windows Server 2016 no admite la configuración de HwThreadCountPerCore en 0.

#### <a name="managing-virtual-machine-smt-configuration"></a>Administración de la configuración SMT de máquinas virtuales

La configuración de SMT de la máquina virtual invitada se establece en cada máquina virtual. El administrador del host puede inspeccionar y configurar la configuración de SMT de una máquina virtual para seleccionar entre las siguientes opciones:

1. Configurar las máquinas virtuales para que se ejecuten como habilitadas para SMT y, opcionalmente, heredar la topología de SMT de host automáticamente

2. Configuración de máquinas virtuales para que se ejecuten como no SMT

La configuración de SMT para una máquina virtual se muestra en los paneles de Resumen de la consola del administrador de Hyper-V.  La configuración de la SMT de una máquina virtual se puede realizar con la configuración de la máquina virtual o con PowerShell.

#### <a name="configuring-vm-smt-settings-using-powershell"></a>Configuración de las opciones de SMT de máquinas virtuales con PowerShell

Para configurar las opciones de SMT para una máquina virtual invitada, abra una ventana de PowerShell con permisos suficientes y escriba:

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <0, 1, 2>
```

Donde:

- 0 = heredar la topología de SMT del host (este valor de HwThreadCountPerCore = 0 no se admite en Windows Server 2016)

- 1 = no SMT

- Valores > 1 = el número deseado de subprocesos de SMT por núcleo. No puede superar el número de subprocesos de SMT físicos por núcleo.

Para leer la configuración de SMT para una máquina virtual invitada, abra una ventana de PowerShell con permisos suficientes y escriba:

``` powershell
(Get-VMProcessor -VMName <VMName>).HwThreadCountPerCore
```

Tenga en cuenta que las máquinas virtuales invitadas configuradas con HwThreadCountPerCore = 0 indican que SMT se habilitará para el invitado y expondrán el mismo número de subprocesos SMT al invitado que en el host de virtualización subyacente, normalmente 2.

### <a name="guest-vms-may-observe-changes-to-cpu-topology-across-vm-mobility-scenarios"></a>Las máquinas virtuales invitadas pueden observar cambios en la topología de CPU entre escenarios de movilidad de máquinas virtuales

El sistema operativo y las aplicaciones en una máquina virtual pueden ver los cambios en la configuración del host y de la máquina virtual antes y después de los eventos de ciclo de vida de la máquina virtual, como la migración en vivo o las operaciones de guardado Durante una operación en la que se guarda y restaura el estado de la máquina virtual, se migran la configuración HwThreadCountPerCore de la máquina virtual y el valor realizado (es decir, la combinación calculada de la configuración del host de origen y la configuración de la máquina virtual). La máquina virtual seguirá ejecutándose con esta configuración en el host de destino. En el punto en que se apaga y se vuelve a iniciar la máquina virtual, es posible que cambie el valor realizado observado por la máquina virtual. Esto debería ser benigno, ya que el software del nivel de aplicación y del sistema operativo debería buscar información de topología de CPU como parte de sus flujos de código de inicio y de inicialización normales. Sin embargo, dado que estas secuencias de inicialización de tiempo de arranque se omiten durante las operaciones de migración en vivo o de guardado y restauración, las máquinas virtuales que se someten a estas transiciones de estado podrían observar el valor realizado originalmente calculado hasta que se cierren y se vuelvan a iniciar.

### <a name="alerts-regarding-non-optimal-vm-configurations"></a>Alertas con respecto a configuraciones de máquinas virtuales no óptimas

Las máquinas virtuales configuradas con más VPs que los núcleos físicos en el host dan como resultado una configuración no óptima. El programador del hipervisor tratará estas máquinas virtuales como si fueran compatibles con SMT. Sin embargo, el software del sistema operativo y de aplicaciones de la máquina virtual se mostrará como una topología de CPU en la que se deshabilitará SMT. Cuando se detecta esta condición, el proceso de trabajo de Hyper-V registra un evento en la advertencia del host de virtualización del administrador del host de que la configuración de la máquina virtual no es óptima y la recomendación de SMT está habilitada para la máquina virtual.

#### <a name="how-to-identify-non-optimally-configured-vms"></a>Identificación de máquinas virtuales configuradas de forma no óptima

Puede identificar las máquinas virtuales que no son SMT examinando el registro del sistema en Visor de eventos para el identificador de evento 3498 del proceso de trabajo de Hyper-V, que se desencadenará para una máquina virtual cada vez que el número de VPs de la máquina virtual sea mayor que el número de núcleos físicos. Los eventos del proceso de trabajo se pueden obtener de Visor de eventos o a través de PowerShell.

#### <a name="querying-the-hyper-v-worker-process-vm-event-using-powershell"></a>Consultar el evento de máquina virtual del proceso de trabajo de Hyper-V mediante PowerShell

Para consultar el ID. de evento 3498 del proceso de trabajo de Hyper-V mediante PowerShell, escriba los siguientes comandos desde un símbolo del sistema de PowerShell.

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Worker"; ID=3498}
```

### <a name="impacts-of-guest-smt-configuaration-on-the-use-of-hypervisor-enlightenments-for-guest-operating-systems"></a>Impactos de configuración de SMT de invitados sobre el uso de las habilitaciones de hipervisor para los sistemas operativos invitados

El hipervisor de Microsoft ofrece varias habilitaciones, o sugerencias, que el sistema operativo que se ejecuta en una máquina virtual invitada puede consultar y usar para desencadenar optimizaciones, como las que pueden beneficiar el rendimiento o mejorar el control de diversas condiciones cuando se ejecutan virtualizadas. Una habilitación introducida recientemente se refiere al control de la programación del procesador virtual y al uso de mitigaciones del sistema operativo para los ataques de canal secundario que aprovechan SMT.

>[!NOTE]
>Microsoft recomienda que los administradores de host habiliten SMT para que las máquinas virtuales invitadas optimicen el rendimiento de la carga de trabajo.

A continuación se proporcionan los detalles de la habilitación de este invitado, pero la ventaja clave para los administradores de host de virtualización es que las máquinas virtuales deben tener HwThreadCountPerCore configurado para coincidir con la configuración de SMT física del host. Esto permite que el hipervisor informe de que no hay ningún uso compartido de núcleos no arquitectónicos. Por lo tanto, es posible que se habilite cualquier sistema operativo invitado que admita optimizaciones que requieran la habilitación. En Windows Server 2019, cree nuevas máquinas virtuales y deje el valor predeterminado de HwThreadCountPerCore (0). Las máquinas virtuales anteriores migradas desde hosts de Windows Server 2016 se pueden actualizar a la versión de configuración de Windows Server 2019. Después de hacerlo, Microsoft recomienda establecer HwThreadCountPerCore = 0.  En Windows Server 2016, Microsoft recomienda establecer HwThreadCountPerCore para que coincida con la configuración del host (normalmente 2).

### <a name="nononarchitecturalcoresharing-enlightenment-details"></a>Detalles de la habilitación de NoNonArchitecturalCoreSharing

A partir de Windows Server 2016, el hipervisor define una nueva habilitación para describir su control de VP programación y colocación en el SO invitado. Esta habilitación se define en la [especificación funcional de nivel superior del hipervisor v 5.0 c](/virtualization/hyper-v-on-windows/reference/tlfs).

El hipervisor sintético de la hoja CPUID. 0x40000004. EAX: 18 [NoNonArchitecturalCoreSharing = 1] indica que un procesador virtual nunca compartirá un núcleo físico con otro procesador virtual, excepto para los procesadores virtuales que se informan como subprocesos SMT relacionados. Por ejemplo, un Vicepresidente invitado nunca se ejecutará en un subproceso SMT junto con un Vicepresidente raíz que se ejecute simultáneamente en un subproceso de SMT relacionado en el mismo núcleo del procesador. Esta condición solo es posible cuando se ejecuta virtualizado y, por tanto, representa un comportamiento SMT no arquitectónico que también tiene implicaciones de seguridad graves. El SO invitado puede usar NoNonArchitecturalCoreSharing = 1 como indicación de que es seguro habilitar las optimizaciones, lo que puede ayudar a evitar la sobrecarga de rendimiento que supone la configuración de STIBP.

En algunas configuraciones, el hipervisor no indicará que NoNonArchitecturalCoreSharing = 1. Por ejemplo, si un host tiene SMT habilitado y está configurado para usar el programador clásico de hipervisor, NoNonArchitecturalCoreSharing será 0. Esto puede impedir que los invitados habilitados habiliten ciertas optimizaciones. Por lo tanto, Microsoft recomienda que los administradores de host que usen SMT dependan del programador de núcleos del hipervisor y asegúrese de que las máquinas virtuales estén configuradas para heredar la configuración de SMT del host para garantizar un rendimiento óptimo de la carga de trabajo.

## <a name="summary"></a>Resumen

El panorama de amenazas de seguridad continúa evolucionando. Para garantizar la seguridad de nuestros clientes de forma predeterminada, Microsoft cambia la configuración predeterminada del hipervisor y las máquinas virtuales a partir de Windows Server 2019 Hyper-V, y proporciona instrucciones y recomendaciones actualizadas para los clientes que ejecutan Windows Server 2016 Hyper-V. Los administradores del host de virtualización deben:

* Lea y comprenda las instrucciones proporcionadas en este documento.

* Evalúe y ajuste cuidadosamente sus implementaciones de virtualización para asegurarse de que cumplen los objetivos de seguridad, rendimiento, densidad de virtualización y capacidad de respuesta de la carga de trabajo para sus requisitos únicos.

* Considere la posibilidad de volver a configurar hosts de Hyper-V de Windows Server 2016 existentes para aprovechar las ventajas de seguridad fuertes que ofrece el programador principal del hipervisor.

* Actualice las máquinas virtuales que no sean SMT para reducir el impacto en el rendimiento de las restricciones de programación impuestas por el aislamiento de VP que aborda las vulnerabilidades de seguridad del hardware