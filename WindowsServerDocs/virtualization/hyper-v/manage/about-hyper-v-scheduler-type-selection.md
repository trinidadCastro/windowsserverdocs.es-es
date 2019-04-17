---
title: Acerca de la selección del tipo de programador de hipervisor de Hyper-V
description: Proporciona información para los administradores del host de Hyper-V en el uso de programador de Hyper-V modos
author: allenma
ms.author: allenma
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows-server-hyper-v
ms.technology: virtualization
ms.localizationpriority: low
ms.assetid: 5fe163d4-2595-43b0-ba2f-7fad6e4ae069
ms.openlocfilehash: c5360d8e2fdc23f8b05c6be0f665407eebedeba2
ms.sourcegitcommit: 546229d6b5fa7e16f725c6c35f4dcc272711b811
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2018
ms.locfileid: "4905132"
---
# Acerca de la selección del tipo de programador de hipervisor de Hyper-V

Se aplica a:

* WindowsServer2016
* Windows Server, versión 1709
* Windows Server, versión 1803
* Windows Server 2019

Este documento describe los cambios importantes en predeterminado de Hyper-V y tipos del programador de uso de hipervisor de recomendado. Estos cambios afectan a ambos rendimiento del sistema de seguridad y la virtualización. Los administradores del host de virtualización deben revisar y comprender los cambios y las implicaciones que se describen en este documento y evaluar detenidamente el impacto, Guía de implementación sugerida y factores de riesgo implicadas a comprender mejor cómo implementar y administrar Hosts de Hyper-V en la parte frontal del cambiante panorama de seguridad.

>[!IMPORTANT]
>Conocidas actualmente Invitado VM malintencionado mediante el comportamiento de la programación del tipo de programador clásico de hipervisor heredadas cuando se ejecuta en los hosts con simultáneo podrían ser víctimas observa en varias arquitecturas de procesador de vulnerabilidades de seguridad de canal lateral Multithreading (SMT) habilitado.  Si se aprovecha con éxito, una carga de trabajo malintencionado podría observar datos fuera de su límite de partición. Esta clase de ataques se subsanan configurando el hipervisor de Hyper-V para utilizar el tipo de programador de hipervisor core y el invitado volver a configurar las máquinas virtuales. Con el programador de core, el hipervisor restringe VP de una máquina virtual de invitado que se ejecute en el mismo núcleo de procesador físico, por lo tanto, encarecidamente aislar la capacidad de la máquina virtual de acceder a los datos a los límites del núcleo físico en el que se ejecuta.  Esto es una mitigación altamente eficaz contra estos ataques de canal lateral, lo que impide que la máquina virtual observar los artefactos de otras particiones, si la raíz o en otra partición de invitado.  Por lo tanto, Microsoft está cambiando el valor predeterminado y recomienda opciones de configuración para hosts de virtualización y máquinas virtuales invitadas.

## Segundo plano

A partir de Windows Server 2016, Hyper-V admite varios métodos de programación y administración de procesadores virtuales, que se conoce como tipos de programador de hipervisor.  Una descripción detallada de todos los tipos de programador de hipervisor puede encontrarse en la [Descripción y el uso de tipos de programador de hipervisor de Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types).

>[!NOTE]
>Nuevos tipos de programador de hipervisor se presentó por primera vez con Windows Server 2016 y no están disponibles en versiones anteriores. Todas las versiones de Hyper-V, antes de Windows Server 2016 admiten a solo el programador clásico. Soporte técnico para el programador de core publicó recientemente.

## Acerca de los tipos de programador de hipervisor

En este artículo se centra específicamente en el uso del tipo de programador de hipervisor core en comparación con el programador de "clásico" heredado nuevo y cómo se relacionan estos tipos de programador con el uso de los subprocesos múltiples simétricas o SMT.  Es importante entender las diferencias entre los programadores core y clásica y cómo cada coloca el trabajo de máquinas virtuales invitadas en los procesadores de sistema subyacente.

### El programador de clásico

El programador clásico es el método de turnos de recurso compartido de equitativa, de la programación de trabajo en procesadores virtuales (VP) en todo el sistema - como raíz VP como VP que pertenecen a máquinas virtuales invitadas. El programador de clásico ha sido el tipo de programador predeterminado usado en todas las versiones de Hyper-V (hasta Windows Server 2019, tal como se describe en este documento).  También se conocen las características de rendimiento del programador clásica y se muestra el programador de clásico para admitir la suscripción en exceso de cargas de trabajo: es decir, una suscripción de relación de VP:LP del host por un margen razonable vienen (dependiendo de la tipos de cargas de trabajo virtualizadas, general utilización de recursos, etcetera.).

Cuando se ejecuta en un host de virtualización con SMT habilitado, el programador de clásico programará VP de invitado de cualquier máquina virtual en cada subproceso SMT pertenecientes a un núcleo de forma independiente. Por lo tanto, pueden ejecutar máquinas virtuales diferentes en el mismo núcleo al mismo tiempo (una VM que se ejecutan en un subproceso de un núcleo mientras se ejecuta otro VM en el otro subproceso).

### El programador de core

El programador de core aprovecha las propiedades de SMT para proporcionar aislamiento de cargas de trabajo de invitado, que afecta a seguridad y rendimiento del sistema. El programador de core garantiza que se programa VP desde una máquina virtual en subprocesos SMT de elementos del mismo nivel. Esto se realiza de forma simétrica para que si lógicos (LP) se encuentran en grupos de dos, VP se programan en grupos de dos y un núcleo de CPU del sistema nunca se comparte entre las máquinas virtuales.

Al programar VP de invitado en pares SMT subyacentes, el programador de core ofrece un límite de seguridad sólida para el aislamiento de carga de trabajo y también puede usarse para reducir la variabilidad de rendimiento para las cargas de trabajo con información confidencial de latencia.

Ten en cuenta que cuando la VP se ha programado para una máquina virtual sin SMT habilitada, VP consumirá el núcleo todo cuando se ejecuta y relacionado del núcleo subprocesos SMT quede inactivo.  Esto es necesario para proporcionar el aislamiento de carga de trabajo correcto, pero afecta al rendimiento general del sistema, especialmente a medida que el lógicos (LP) del sistema exceso suscrito - es decir, cuando supera el total VP:LP relación 1:1. Por lo tanto, la ejecución de máquinas virtuales de invitado configuradas sin varios subprocesos por núcleo es una configuración óptima.

### Ventajas de usar al programador de core

El programador de core ofrece las siguientes ventajas:

* Un límite de seguridad sólida para el aislamiento de carga de trabajo de invitado - VP de invitado están restringidos a ejecutarse en pares de núcleo físico subyacente, reduciendo vulnerabilidad frente a ataques de cualquier canal lateral.

* Reducción de la carga de trabajo variabilidad - variabilidad de rendimiento de carga de trabajo de invitado se reduce considerablemente, que ofrece una mayor coherencia de carga de trabajo.

* Uso de SMT en Invitado VM - el sistema operativo y las aplicaciones que se ejecutan en la máquina virtual invitada puede utilizar el comportamiento SMT y (API) para controlar y distribuir el trabajo a través de subprocesos SMT, igual que cuando se ejecutarían de interfaces de programación no virtualizados.

El programador de core actualmente se usa en los hosts de virtualización de Azure, específicamente para aprovechar el límite de seguridad sólida y la carga de trabajo bajo variabilty. Microsoft cree que el tipo de programador de principal debe ser y seguirá siendo el hipervisor de forma predeterminada la programación de tipo para la mayoría de escenarios de virtualización.  Por lo tanto, para garantizar que nuestros clientes son seguras de manera predeterminada, Microsoft realiza este cambio para Windows Server 2019 ahora.

### Impactos de rendimiento de programador de Core en las cargas de trabajo de invitado

Aunque es necesario para mitigar de forma eficaz ciertas clases de vulnerabilidades, el programador de core potencialmente también puede reducir el rendimiento. Los clientes pueden ver una diferencia en las características de rendimiento con sus máquinas virtuales y afecta a la capacidad de carga de trabajo general de los hosts de virtualización. En casos donde el programador de core debe ejecutar una que no sea - SMT VP, solo una de las secuencias de instrucción en el núcleo lógico subyacente ejecuta mientras que la otra debe dejarse inactivo. Esto limitará la capacidad total de host para cargas de trabajo de invitado.

Estos efectos sobre el rendimiento se pueden minimizar siguiendo las instrucciones de implementación de este documento. Los administradores del host cuidadosamente deben tener en cuenta su virtualización específica de escenarios de implementación y equilibrar su tolerancia de riesgos de seguridad con la necesidad de densidad de carga de trabajo máxima, exceso consolidación de hosts de virtualización, etcetera.

## Cambios en la predeterminada y las configuraciones recomendadas para Windows Server 2016 y Windows Server 2019

Implementación de hosts de Hyper-V con la posición de máxima seguridad requiere el uso del tipo de programador de hipervisor core. Para garantizar que nuestros clientes son seguras de manera predeterminada, Microsoft está cambiando el valor predeterminado siguiente y la configuración recomendada.

>[!NOTE]
>Mientras interno de soporte del hipervisor para los tipos de programador se incluyó en la versión inicial de Windows Server 2016, Windows Server 1709 y Windows Server 1803, las actualizaciones se necesitan para tener acceso al control de configuración que permite seleccionar el tipo de programador de hipervisor.  Consulte [comprender y utilizar tipos de programador de hipervisor de Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types) para obtener más información sobre estas actualizaciones.

### Cambios de host de virtualización

* El hipervisor usará al programador de core al principio de forma predeterminada con Windows Server 2019.

* Microsoft reccommends de configurar al programador de core en Windows Server 2016. El tipo de programador de núcleo de hipervisor se admite en Windows Server 2016, sin embargo, el valor predeterminado es el programador clásico. El programador de core es opcional y el administrador del host de Hyper-V debe habilitar explícitamente.

### Cambios de configuración de máquina virtual

* En Windows Server 2019, máquinas virtuales nuevas creadas con la versión de máquina virtual predeterminada 9.0 heredarán automáticamente las propiedades SMT (habilitado o deshabilitado) del host de virtualización. Es decir, si SMT está habilitado en el host físico, recién creado las máquinas virtuales también tendrá SMT habilitado y heredará la topología SMT del host de manera predeterminada, con la máquina virtual con el mismo número de subprocesos de hardware por núcleo que el sistema subyacente. Esto se reflejarán en la configuración de la máquina virtual con HwThreadCountPerCore = 0, donde 0 indica que la máquina virtual debe heredan la configuración de SMT del host.

* Las máquinas virtuales existentes con una versión de la máquina virtual de 8.2 o versiones anterior se conservan su configuración original de procesador de máquina virtual para HwThreadCountPerCore y el valor predeterminado de 8.2 invitados de versión de máquina virtual es HwThreadCountPerCore = 1. Cuando estos invitados se ejecutan en un host de Windows Server 2019, se tratará como sigue:

    1. Si la máquina virtual tiene un recuento de VP que es menor o igual al número de núcleos LP, la máquina virtual se tratará como una máquina virtual no SMT por el programador de core. Cuando se ejecuta la VP de invitado en un solo subproceso SMT, elementos del mismo nivel de las principales subproceso SMT se se inactivos. Esto es que no es óptimo y resultará en general pérdida de rendimiento.

    2. Si la máquina virtual tiene más VP que núcleos LP, la máquina virtual se tratará como una VM de SMT por el programador de core. Sin embargo, la máquina virtual no observará otras indicaciones que es una VM de SMT. Por ejemplo, el uso de la instrucción CPUID o la API de Windows para consultar la topología de CPU por el sistema operativo o aplicaciones no indicará que SMT está habilitado.

* Cuando una máquina virtual existente explícitamente se actualiza desde versiones de la máquina virtual de eariler a la versión 9.0 a través de la operación de actualización de la VM, la máquina virtual conservará su valor actual de HwThreadCountPerCore.  La máquina virtual no tendrá SMT habilitado en vigor.

* En Windows Server 2016, Microsoft recomienda habilitar SMT para máquinas virtuales invitadas.  De manera predeterminada, las máquinas virtuales que se creen en Windows Server 2016 habría SMT está deshabilitado, que es HwThreadCountPerCore se establece en 1, a menos que se modifique explícitamente.

>[!NOTE]
>Windows Server 2016 no admite la configuración HwThreadCountPerCore en 0.

#### Administrar la configuración de máquina virtual SMT

La configuración de SMT de máquina virtual de invitado se establece de forma por máquina virtual. El administrador del host puede inspeccionar y la configuración de la VM SMT para seleccionar entre las siguientes opciones:

    1. Configurar máquinas virtuales para ejecutar como SMT habilitado, opcionalmente, heredar de automáticamente la topología SMT de host

    2. Configurar máquinas virtuales para ejecutarse como no SMT

En los paneles de resumen de la consola del Administrador de Hyper-V, se muestra el configuaration SMT para una máquina virtual.  Configurar opciones de configuración de la VM SMT puede hacerse mediante el uso de la configuración de máquina virtual o PowerShell.

#### Configuración de la VM SMT mediante PowerShell

Para configurar la configuración de SMT para una máquina virtual invitada, abre una ventana de PowerShell con permisos suficientes y escribe:

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <0, 1, 2>
```

Donde:

    0 = Inherit SMT topology from the host (this setting of HwThreadCountPerCore=0 is not supported on Windows Server 2016)

    1 = Non-SMT

    Values > 1 = the desired number of SMT threads per core. May not exceed the number of physical SMT threads per core.

Para leer la configuración de SMT para una máquina virtual invitada, abre una ventana de PowerShell con permisos suficientes y escribe:

``` powershell
(Get-VMProcessor -VMName <VMName>).HwThreadCountPerCore
```

Ten en cuenta que las máquinas virtuales que se configuran con HwThreadCountPerCore invitado = 0 indica que SMT se habilitarán para el invitado y expondrá el mismo número de subprocesos SMT para el invitado, que estén en el host de virtualización subyacente, por lo general, 2.

### Invitada puede observar cambios a la topología de CPU a través de los escenarios de movilidad VM

El sistema operativo y las aplicaciones en una máquina virtual pueden ver los cambios en el host y la configuración de la máquina virtual antes y después de los eventos de ciclo de vida de máquina virtual, como la migración en vivo o guardar y restauración las operaciones. Durante una operación en qué VM se guarda y se restauran estado, se migran configuración de HwThreadCountPerCore de la máquina virtual y el valor ejecutado (es decir, la combinación calculado de configuración de la máquina virtual y configuración del host de origen). La máquina virtual seguirá ejecutándose con estas opciones de configuración en el host de destino. En el punto de la máquina virtual está apagado y volver a iniciar, es posible que el valor ejecutado observado por la máquina virtual cambiará. Este debe ser peligroso, como el sistema operativo y aplicación de software de capa debe buscar información de la topología de CPU como parte de sus flujos de código de inicialización y de inicio normales. Sin embargo, dado que podría observar estos inicialización en tiempo de arranque secuencias se omiten durante dinámicos migración o guardar y restaurar las operaciones, las máquinas virtuales que se producen en estas transiciones de estado originalmente calculado realizado valor hasta que se cierra y vuelve a inicia.  

### Alertas de configuraciones de máquina virtual no óptima

Máquinas virtuales configuradas con VP más que el número de núcleos físicos en el resultado de host en una configuración no óptima. El programador de hipervisor tratará estas máquinas virtuales como si son compatibles con la SMT. Sin embargo, sistema operativo y software de la aplicación en la máquina virtual, aparecerá una topología de CPU que muestra SMT está deshabilitado. Cuando se detecta esta condición, el proceso de trabajo de Hyper-V registrará un evento en el administrador del host de advertencia que la configuración de la máquina virtual está no óptima y recomendar SMT será el host de virtualización habilitada para la máquina virtual.

#### Cómo identificar de forma no óptima configurado las máquinas virtuales

Puedes identificar no - SMT máquinas virtuales examinando el registro del sistema en el Visor de eventos para el evento de proceso de trabajo de Hyper-V 3498 de Id., que se activará para una máquina virtual siempre que el número de VP en la máquina virtual es mayor que el recuento de núcleo físico. Los eventos de proceso de trabajo pueden obtenerse desde el Visor de eventos, o a través de PowerShell.

#### Consulta el evento de VM de proceso de trabajo de Hyper-V con PowerShell

Para consultar para el evento de proceso de trabajo de Hyper-V 3498 de Id. de uso de PowerShell, escribe los siguientes comandos desde un símbolo del sistema de PowerShell.

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Worker"; ID=3498}
```

### Impactos de invitado SMT configuaration sobre el uso de las mejoras de hipervisor para sistemas operativos invitados

El hipervisor de Microsoft ofrece varias mejoras de código o de las sugerencias, que se puede consultar y se usa para desencadenar optimizaciones, como aquellos que podrían beneficiarse de rendimiento o mejorar lo contrario, el control de distintas condiciones de cuando se ejecuta el sistema operativo que se ejecuta en un máquina virtual de invitado virtualizada. El control de programación de procesador virtual y el uso de las mitigaciones de sistema operativo de los ataques de canal lateral que SMT contra vulnerabilidades de seguridad se refiere a una optimización presentado recientemente.

>[!NOTE]
>Microsoft recomienda que los administradores del host habilitan SMT para máquinas virtuales invitadas optimizar el rendimiento de la carga de trabajo.

Los detalles de esta optimización de invitado aparecen a continuación, sin embargo el punto clave para los administradores del host de virtualización es que las máquinas virtuales deben tener HwThreadCountPerCore configurado para que coincidan con la configuración del host física SMT. Esto permite que el hipervisor informar de que no hay ningún core no arquitectura de uso compartido. Por lo tanto, cualquier optimizaciones auxiliares del sistema operativo invitado que requieren la optimización pueden estar habilitadas. En Windows Server 2019, crear máquinas virtuales nuevas y dejar el valor predeterminado de HwThreadCountPerCore (0). Migran de máquinas virtuales anteriores de Windows Server 2016 hosts pueden actualizarse a la versión de configuración de Windows Server 2019. Después de hacerlo, Microsoft recomienda establecer HwThreadCountPerCore = 0.  En Windows Server 2016, Microsoft recomienda establecer HwThreadCountPerCore para que coincida con la configuración de host (por lo general, 2).

### Detalles de optimización de NoNonArchitecturalCoreSharing

A partir de Windows Server 2016, el hipervisor define una nueva optimización para describir su control de programación VP y la colocación en el sistema operativo invitado. Esta optimización se define en la [especificación funcional de nivel de parte superior de hipervisor v5.0c](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/tlfs).

Hoja CPUID sintética de hipervisor CPUID.0x40000004.EAX:18[NoNonArchitecturalCoreSharing = 1] indica que un procesador virtual nunca van a compartir un núcleo físico con otro procesador virtual, excepto los procesadores virtuales que se notifican como elementos del mismo nivel SMT subprocesos. Por ejemplo, un Vicepresidente de invitado nunca se ejecutará en un subproceso SMT junto con una VP raíz que se ejecutan al mismo tiempo en un subproceso SMT en el mismo núcleo de procesador elementos del mismo nivel. Esta condición es posible solamente cuando se ejecuta virtualizado y, por lo tanto, representa un comportamiento SMT no arquitectura que también tiene graves implicaciones de seguridad. El sistema operativo invitado puede utilizar NoNonArchitecturalCoreSharing = 1 como una indicación de que es seguro habilitar las optimizaciones en el que se pueden ayudar a evitar la sobrecarga de rendimiento de la configuración de STIBP.

En determinadas configuraciones, el hipervisor no indicará que NoNonArchitecturalCoreSharing = 1. Por ejemplo, si un host tiene SMT habilitado y está configurado para usar al programador de clásico de hipervisor, NoNonArchitecturalCoreSharing será 0. Esto puede impedir que los invitados habilitados habilitar ciertas optimizaciones. Por lo tanto, Microsoft recomienda que los administradores de host con SMT se basan en el programador de núcleo de hipervisor y asegurarse de que las máquinas virtuales están configuradas para su configuración SMT se heredan desde el host para garantizar un rendimiento óptimo de la carga de trabajo.

## Resumen

El panorama de amenazas de seguridad continúa evolucionando. Para garantizar que nuestros clientes son seguras de manera predeterminada, Microsoft está cambiando la configuración predeterminada para el hipervisor y máquinas virtuales a partir de Windows Server 2019 Hyper-V y proporcionar actualizado instrucciones y recomendaciones para clientes que ejecutan Windows Server 2016 Hyper-V. Los administradores del host de virtualización deben:

* Leer y comprender las instrucciones proporcionadas en este documento

* Evaluar y ajustar sus implementaciones de virtualización para asegurarse de que cumplan la seguridad, rendimiento, densidad de virtualización y los objetivos de capacidad de respuesta de carga de trabajo de sus requisitos únicos de cuidadosamente

* Considera la posibilidad de volver a configurar los hosts de Windows Server 2016 Hyper-V existente para aprovechar las ventajas de seguridad sólida que ofrece el programador de núcleo de hipervisor

* Actualizar no - SMT las máquinas virtuales existentes para reducir el impacto de rendimiento de programación restricciones impuestas mediante el aislamiento de VP que soluciona vulnerabilidades de seguridad de hardware
