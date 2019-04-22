---
title: Acerca de la selección del tipo de programador de hipervisor de Hyper-V
description: Proporciona información para administradores del host de Hyper-V en el uso del programador de Hyper-V de modos
author: allenma
ms.author: allenma
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows-server-hyper-v
ms.technology: virtualization
ms.localizationpriority: low
ms.assetid: 5fe163d4-2595-43b0-ba2f-7fad6e4ae069
ms.openlocfilehash: c5360d8e2fdc23f8b05c6be0f665407eebedeba2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823886"
---
# <a name="about-hyper-v-hypervisor-scheduler-type-selection"></a>Acerca de la selección del tipo de programador de hipervisor de Hyper-V

Se aplica a:

* Windows Server 2016
* Windows Server, versión 1709
* Windows Server, versión 1803
* Windows Server 2019

Este documento describe cambios importantes en el valor predeterminado de Hyper-V y recomienda el uso de hipervisor tipos del programador. Estos cambios afectan tanto rendimiento del sistema de seguridad y la virtualización. Deben revisar y comprender los cambios y las implicaciones que se describe en este documento y evaluar cuidadosamente los impactos, Guía de implementación recomendada y factores de riesgo implicados para comprender mejor cómo implementar y administrar administradores del host de virtualización Hosts de Hyper-V ante el cambiante panorama de seguridad.

>[!IMPORTANT]
>Conocido trabajar en varias arquitecturas de procesador de vulnerabilidades podrían ser aprovechadas por una máquina virtual invitada malintencionada mediante el comportamiento del tipo clásico programador hipervisor heredado cuando se ejecuta en hosts con simultáneo de programación de seguridad de canal lateral Multithreading (SMT) habilitado.  Si ha explotado correctamente, una carga de trabajo malintencionado podría observar datos fuera de su límite de partición. Esta clase de ataques puede mitigarse configurando el hipervisor de Hyper-V para utilizar el tipo de hipervisor core programador y el invitado reconfigurar las máquinas virtuales. Con el programador de core, el hipervisor restringe VPs un Invitado VM se ejecute en el mismo núcleo de procesador físico, por lo tanto, fuertemente aislar la capacidad de la máquina virtual para acceder a los datos a los límites del núcleo físico en el que se ejecuta.  Se trata de una mitigación muy eficaz contra estos ataques del lado de canal, lo que impide que la máquina virtual se observa los artefactos de otras particiones, si la raíz o en otra partición de invitado.  Por lo tanto, Microsoft está cambiando el valor predeterminado y recomendado de valores de configuración para los hosts de virtualización y las máquinas virtuales invitadas.

## <a name="background"></a>Background

A partir de Windows Server 2016, Hyper-V admite varios métodos de programación y administración de procesadores virtuales, conocidos como tipos de programador de hipervisor.  Encontrará una descripción detallada de todos los tipos de hipervisor del programador en [descripción y el uso de tipos del programador de hipervisor de Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types).

>[!NOTE]
>Nuevos tipos de programador de hipervisor se introdujeron con Windows Server 2016 y no están disponibles en versiones anteriores. Todas las versiones de Hyper-V antes de Windows Server 2016 admiten a solo el programador clásico. Compatibilidad con el programador de core solo recientemente se ha publicado.

## <a name="about-hypervisor-scheduler-types"></a>Acerca de los tipos del programador de hipervisor

En este artículo se centra específicamente en el uso del nuevo tipo de hipervisor core programador en comparación con el programador "clásico" heredado, y cómo estos tipos de programador forma una intersección con el uso de subprocesamiento múltiple simétrico o SMT.  Es importante comprender las diferencias entre los programadores de núcleo y el clásico y cómo cada uno coloca el trabajo de las máquinas virtuales invitadas en los procesadores del sistema subyacente.

### <a name="the-classic-scheduler"></a>El programador clásico

El programador clásico hace referencia al método fair-share, operación por turnos de programar el trabajo en procesadores virtuales (VPs) a través del sistema - como raíz VPs VPs que pertenecen a las máquinas virtuales invitadas. El programador clásico ha sido el tipo de programador predeterminado utilizado en todas las versiones de Hyper-V (hasta Windows Server 2019, tal como se describe en este documento).  Las características de rendimiento del programador clásico se entienden bien y se demuestra el programador clásico para admitir la sobresuscripción de cargas de trabajo: es decir, sobresuscripción de proporción de VP:LP del host por un margen razonable vienen (función de la tipos de cargas de trabajo que se estaba virtualizadas, utilización de recursos global, etcetera.).

Cuando se ejecuta en un host de virtualización con SMT habilitado, el programador clásico programará VPs invitado desde cualquier máquina virtual en cada subproceso SMT que pertenecen a un núcleo por separado. Por lo tanto, pueden ejecutar diferentes máquinas virtuales en el mismo núcleo al mismo tiempo (una máquina virtual que se ejecutan en un subproceso de un núcleo, mientras que otra máquina virtual se está ejecutando en el otro subproceso).

### <a name="the-core-scheduler"></a>El programador de core

El programador de core aprovecha las propiedades de SMT para proporcionar aislamiento de cargas de trabajo de invitado, lo que afecta a la seguridad y rendimiento del sistema. El programador de core garantiza que se programan VPs desde una máquina virtual en subprocesos SMT relacionado. Esto se hace simétricamente por lo que si LPs están en grupos de dos, VPs se programan en grupos de dos, y un núcleo de CPU del sistema nunca se comparte entre las máquinas virtuales.

Mediante la programación VPs invitado en pares SMT subyacentes, el programador de core ofrece un límite de seguridad sólidos para el aislamiento de la carga de trabajo y también puede usarse para reducir la variabilidad del rendimiento para cargas de trabajo confidenciales de latencia.

Tenga en cuenta que cuando se programa el Vicepresidente de una máquina virtual sin SMT habilitado, VP consumirá el núcleo completo cuando se ejecuta y relacionado del núcleo subproceso SMT quedará inactivo.  Esto es necesario proporcionar el aislamiento de la carga de trabajo correcta, pero afecta al rendimiento general del sistema, especialmente a medida que el sistema de LPs sobresuscrito: es decir, cuando la proporción de VP:LP total supera 1:1. Por lo tanto, ejecutar máquinas virtuales invitadas configuradas sin varios subprocesos por núcleo es una configuración poco óptima.

### <a name="benefits-of-the-using-the-core-scheduler"></a>Ventajas del empleo del programador de core

El programador de core ofrece las siguientes ventajas:

* Un límite de seguridad sólida para el aislamiento de cargas de trabajo de invitado - VPs invitados están limitados a ejecutar en pares de núcleo físico subyacente, lo que reduce la vulnerabilidad a ataques de snooping del lado de canal.

* Carga de trabajo reducida variabilidad - variabilidad del rendimiento de carga de trabajo de invitado se ha reducido significativamente, ofrece mayor coherencia de la carga de trabajo.

* Uso de SMT en Invitados VM: el sistema operativo y aplicaciones que se ejecutan en la máquina virtual invitada puede utilizar el comportamiento SMT y interfaces de programación (API) para controlar y distribuir el trabajo entre subprocesos SMT, igual que si se ejecutaran cuando no virtualización.

El programador de core actualmente se usa en un host de virtualización de Azure, específicamente para aprovechar las ventajas de los límites de seguridad sólida y variabilty bajo carga de trabajo. Microsoft cree que el tipo de planificador de core debe ser y seguirá siendo el hipervisor de forma predeterminada la programación de tipo para la mayoría de escenarios de virtualización.  Por lo tanto, para asegurarse de que nuestros clientes son seguros de forma predeterminada, Microsoft ha realizado este cambio para Windows Server 2019 ahora.

### <a name="core-scheduler-performance-impacts-on-guest-workloads"></a>Core impactos de rendimiento de programador en cargas de trabajo de invitado

Aunque es necesario para mitigar eficazmente ciertas clases de vulnerabilidades, el programador core potencialmente puede reducir el rendimiento. Los clientes pueden ver una diferencia en las características de rendimiento con sus máquinas virtuales y efectos a la capacidad de carga de trabajo global de sus hosts de virtualización. En casos donde el programador core debe ejecutar un no - SMT VP, se ejecuta solo una de las secuencias de instrucción en el núcleo lógico subyacente mientras el otro debe quedar inactivo. Esto limita la capacidad total del host para cargas de trabajo de invitado.

Siguiendo las instrucciones de implementación en este documento se pueden minimizar estos efectos sobre el rendimiento. Los administradores de host cuidadosamente deben considerar su virtualización específica en escenarios de implementación y equilibrar su tolerancia de riesgos de seguridad con la necesidad de densidad de carga de trabajo máxima, la consolidación de exceso de hosts de virtualización, etcetera.

## <a name="changes-to-the-default-and-recommended-configurations-for-windows-server-2016-and-windows-server-2019"></a>Cambios de forma predeterminada y configuraciones recomendadas para Windows Server 2016 y Windows Server 2019

Implementar hosts de Hyper-V con la posición de seguridad máximo requiere el uso del tipo de programador de núcleo de hipervisor. Para asegurarse de que nuestros clientes son seguros de forma predeterminada, Microsoft está cambiando el valor predeterminado siguiente y la configuración recomendada.

>[!NOTE]
>Aunque la compatibilidad interna del hipervisor para los tipos de scheduler se incluyó en la versión inicial de Windows Server 2016, Windows Server 1709 y Windows Server 1803, las actualizaciones son necesarias para tener acceso al control de configuración que permite seleccionar el tipo de planificador de hipervisor.  Consulte [descripción y el uso de tipos del programador de hipervisor de Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types) para obtener más información sobre estas actualizaciones.

### <a name="virtualization-host-changes"></a>Cambios de host de virtualización

* El hipervisor usará al programador core al principio de forma predeterminada con Windows Server 2019.

* Microsoft reccommends configurar al programador de core en Windows Server 2016. El tipo de planificador de hipervisor core es compatible en Windows Server 2016, pero el valor predeterminado es el programador clásico. El programador de núcleo es opcional y debe habilitarse explícitamente por el Administrador de Hyper-V host.

### <a name="virtual-machine-configuration-changes"></a>Cambios de configuración de máquina virtual

* En Windows Server 2019, nuevas máquinas virtuales creadas con la versión de máquina virtual predeterminada 9.0 heredarán automáticamente las propiedades SMT (habilitado o deshabilitado) del host de virtualización. Es decir, si SMT está habilitado en el host físico, recién creado las máquinas virtuales también tendrá SMT habilitado y heredará la topología SMT del host de forma predeterminada, con la máquina virtual con el mismo número de subprocesos de hardware por núcleo, como el sistema subyacente. Esto se reflejará en la configuración de la máquina virtual con HwThreadCountPerCore = 0, donde 0 indica que la máquina virtual debe heredar la configuración de SMT del host.

* Las máquinas virtuales existentes con una versión de la máquina virtual de 8.2 o versiones anterior se conservan su configuración original de procesador de máquina virtual para HwThreadCountPerCore y el valor predeterminado para invitados de versión 8.2 de máquina virtual es HwThreadCountPerCore = 1. Cuando estos invitados se ejecutan en un host de Windows Server 2019, se tratará como sigue:

    1. Si la máquina virtual tiene un recuento de VP es menor o igual que el recuento de núcleos LP, la máquina virtual se tratará como una máquina virtual no SMT por el programador de núcleo. Cuando se ejecuta el Vicepresidente de invitado en un único subproceso SMT, se le inactivo relacionado del núcleo subproceso SMT. Esto es poco óptimo y dará como resultado una pérdida total de rendimiento.

    2. Si la máquina virtual tiene más VPs de núcleos LP, la máquina virtual se tratará como una VM de SMT por el programador de núcleo. Sin embargo, la máquina virtual no advertirán otras indicaciones que es una VM de SMT. Por ejemplo, uso de las API de Windows o la instrucción CPUID para consultar la topología de la CPU por el sistema operativo o las aplicaciones no indicará que SMT está habilitada.

* Cuando una máquina virtual existente se actualiza de forma explícita de anteriores versiones de máquina virtual a la versión 9.0 a través de la operación de actualización de la VM, la máquina virtual conservará su valor actual para HwThreadCountPerCore.  La máquina virtual no tendrá SMT forzar habilita.

* En Windows Server 2016, Microsoft recomienda habilitar SMT para las máquinas virtuales invitadas.  De forma predeterminada, las máquinas virtuales creadas en Windows Server 2016 habría SMT deshabilitado, que es HwThreadCountPerCore está establecido en 1, a menos que cambie explícitamente.

>[!NOTE]
>Windows Server 2016 no admite el parámetro HwThreadCountPerCore en 0.

#### <a name="managing-virtual-machine-smt-configuration"></a>Administrar la configuración SMT de máquina virtual

La configuración de SMT de máquina virtual de invitado está establecida en una base por máquina virtual. El administrador del host puede inspeccionar y configuración de SMT de la máquina virtual para seleccionar entre las siguientes opciones:

    1. Configurar máquinas virtuales para ejecutar como SMT habilitado, si lo desea heredar de automáticamente la topología SMT de host

    2. Configurar máquinas virtuales para ejecutarse como no SMT

En los paneles de resumen en la consola de administrador de Hyper-V, se muestra la configuración SMT para una máquina virtual.  Configuración de las opciones de SMT de la máquina virtual puede realizarse mediante la configuración de máquina virtual o PowerShell.

#### <a name="configuring-vm-smt-settings-using-powershell"></a>Configuración de VM SMT con PowerShell

Para configurar la configuración SMT para una máquina virtual invitada, abra una ventana de PowerShell con permisos suficientes y escriba:

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <0, 1, 2>
```

Donde:

    0 = Inherit SMT topology from the host (this setting of HwThreadCountPerCore=0 is not supported on Windows Server 2016)

    1 = Non-SMT

    Values > 1 = the desired number of SMT threads per core. May not exceed the number of physical SMT threads per core.

Para leer la configuración SMT para una máquina virtual invitada, abra una ventana de PowerShell con permisos suficientes y escriba:

``` powershell
(Get-VMProcessor -VMName <VMName>).HwThreadCountPerCore
```

Tenga en cuenta que las máquinas virtuales configuradas con HwThreadCountPerCore invitado = 0 indica que se habilitará para el invitado SMT y expondrá el mismo número de subprocesos SMT al invitado tal como están en el host de virtualización subyacente, normalmente 2.

### <a name="guest-vms-may-observe-changes-to-cpu-topology-across-vm-mobility-scenarios"></a>Las máquinas virtuales invitadas puede observar los cambios en la topología de la CPU a través de escenarios de movilidad de máquinas virtuales

El sistema operativo y aplicaciones en una máquina virtual pueden ver cambios al host y configuración de máquina virtual antes y después de eventos de ciclo de vida de la máquina virtual, como la migración en vivo o guardar y restauración las operaciones. Durante una operación en qué máquina virtual se guardó y restauró, se migran la configuración de la máquina virtual HwThreadCountPerCore y el valor realizado (es decir, la combinación calculada de la configuración de la máquina virtual y configuración del host de origen). La máquina virtual seguirá ejecutándose con esta configuración en el host de destino. En el punto de la máquina virtual está apagada y vuelve a iniciarse, es posible que observar el valor producido por la máquina virtual cambiará. Esto debe ser benigno, como sistema operativo y aplicación de software con capas debe buscar información de topología de CPU como parte de sus flujos de código de inicialización y de inicio normales. Sin embargo, como pudo observar estos inicialización en tiempo de arranque se omiten las secuencias durante live migración o guardar ni restaurar las operaciones, las máquinas virtuales que se someten a estas transiciones de estado calculado originalmente producidas valor hasta que se cierra y vuelve a iniciarse.  

### <a name="alerts-regarding-non-optimal-vm-configurations"></a>Alertas relacionadas con las configuraciones de máquina virtual no óptimo

Máquinas de virtuales configuradas con VPs más que el número de núcleos físicos en el resultado de host en una configuración que no sea óptimo. El programador de hipervisor tratará estas máquinas virtuales como si fueran compatibles con SMT. Sin embargo, sistema operativo y software de la aplicación en la máquina virtual se presentará una topología de CPU que muestra SMT está deshabilitado. Cuando se detecta esta condición, el proceso de trabajo de Hyper-V registrará un evento en el host de virtualización, el administrador del host de advertencia que configuración de la máquina virtual es que no sea óptima y recomendar SMT estar habilitado para la máquina virtual.

#### <a name="how-to-identify-non-optimally-configured-vms"></a>Cómo identificar de forma que no sea óptima configurado las máquinas virtuales

Puede identificar no - SMT máquinas virtuales examinando el registro del sistema en el Visor de eventos para eventos del proceso de trabajo de Hyper-V 3498 ID, que se desencadenará para una máquina virtual cada vez que el número de VPs en la máquina virtual es mayor que el recuento de núcleos físicos. Eventos de proceso de trabajo pueden obtenerse desde el Visor de eventos, o a través de PowerShell.

#### <a name="querying-the-hyper-v-worker-process-vm-event-using-powershell"></a>Consultar el evento de máquina virtual de proceso de trabajo de Hyper-V mediante PowerShell

Consulta de evento 3498 Id. del proceso de trabajo de Hyper-V con PowerShell, escriba los siguientes comandos desde un símbolo del sistema de PowerShell.

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Worker"; ID=3498}
```

### <a name="impacts-of-guest-smt-configuaration-on-the-use-of-hypervisor-enlightenments-for-guest-operating-systems"></a>Efectos de la configuración SMT de invitado en el uso de hipervisor giraría para sistemas operativos invitados

El hipervisor de Microsoft ofrece varias mejoras de código o sugerencias, que puede consultar y usar para desencadenar las optimizaciones, como las que podría beneficiarse de rendimiento o en caso contrario, mejorar el tratamiento de varias condiciones cuando se ejecuta el sistema operativo que se ejecuta en una máquina virtual invitada virtualizar. El control de programación del procesador virtual y el uso de las mitigaciones de sistema operativo para los ataques de canal lateral que SMT contra vulnerabilidades de seguridad se refiere a una sensación presentado recientemente.

>[!NOTE]
>Microsoft recomienda que los administradores de host habilitar SMT para máquinas virtuales invitadas a optimizar el rendimiento de carga de trabajo.

Los detalles de esta sensación de invitado son proporcionados a continuación, pero la conclusión principal para los administradores del host de virtualización es que las máquinas virtuales deben tener HwThreadCountPerCore configurado para que coincida con la configuración del host física SMT. Esto permite que el hipervisor informar de que no hay ningún core que no es arquitectura de uso compartido. Por lo tanto, puede habilitarse ninguna optimización de apoyo del sistema operativo invitado que requieren la iluminación. En Windows Server 2019, crear nuevas máquinas virtuales y deje el valor predeterminado de HwThreadCountPerCore (0). Migran máquinas virtuales anteriores de Windows Server 2016 hosts se pueden actualizar a la versión de configuración de Windows Server 2019. Una vez hecho esto, Microsoft recomienda establecer HwThreadCountPerCore = 0.  En Windows Server 2016, Microsoft recomienda establecer HwThreadCountPerCore para que coincida con la configuración del host (normalmente 2).

### <a name="nononarchitecturalcoresharing-enlightenment-details"></a>Detalles de iluminación NoNonArchitecturalCoreSharing

A partir de Windows Server 2016, el hipervisor define una sensación nuevo para describir su propio control de selección de ubicación para el sistema operativo invitado y Vicepresidente de programación. Esta sensación se define en el [v5.0c especificación funcional de nivel de parte superior de hipervisor](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/tlfs).

Hoja CPUID sintético de hipervisor CPUID.0x40000004.EAX:18[NoNonArchitecturalCoreSharing = 1] indica que un procesador virtual nunca comparten un núcleo físico con otro procesador virtual, excepto para los procesadores virtuales que se notifican como elemento relacionado SMT subprocesos. Por ejemplo, un Vicepresidente de invitado nunca se ejecutará en un subproceso SMT junto con una VP raíz que se ejecutan simultáneamente en un subproceso SMT en el mismo núcleo de procesador relacionado. Esta condición solo es posible cuando se ejecutan virtualizados y, por lo que representa un comportamiento que no es arquitectura de SMT que también tiene implicaciones de seguridad grave. El sistema operativo invitado puede usar NoNonArchitecturalCoreSharing = 1 como una indicación de que es seguro habilitar las optimizaciones, que pueden ayudarle a evitar la sobrecarga de rendimiento de establecer STIBP.

En determinadas configuraciones, el hipervisor no indicará que NoNonArchitecturalCoreSharing = 1. Por ejemplo, si un host tiene SMT habilitado y está configurado para usar al programador clásico de hipervisor, NoNonArchitecturalCoreSharing será 0. Esto puede impedir que los invitados habilitados de la habilitación de ciertas optimizaciones. Por lo tanto, Microsoft recomienda que los administradores de host mediante SMT se basan en el programador de núcleo de hipervisor y asegúrese de que las máquinas virtuales están configuradas para heredar la configuración SMT desde el host para garantizar un rendimiento óptimo de la carga de trabajo.

## <a name="summary"></a>Resumen

El panorama de amenazas continúa evolucionando. Para asegurarse de que nuestros clientes son seguros de forma predeterminada, Microsoft está cambiando la configuración predeterminada para el hipervisor y las máquinas virtuales a partir de Windows Server 2019 Hyper-V y proporcionar actualizado instrucciones y recomendaciones para los clientes que ejecutan Windows Server 2016 Hyper-V. Los administradores del host de virtualización deben:

* Leer y comprender las instrucciones proporcionadas en este documento

* Evalúe cuidadosamente y ajustar sus implementaciones de virtualización para garantizar que cumplen con la seguridad, rendimiento, densidad de virtualización y los objetivos de la capacidad de respuesta de carga de trabajo para sus requisitos únicos

* Considere la posibilidad de volver a configurar los hosts de Windows Server 2016 Hyper-V existentes para aprovechar las ventajas de seguridad sólida proporcionadas por el programador del núcleo de hipervisor

* Actualizar no - SMT las máquinas virtuales existentes para reducir el impacto de rendimiento de la programación de las restricciones impuestas por aislamiento VP que corrige vulnerabilidades de seguridad de hardware
