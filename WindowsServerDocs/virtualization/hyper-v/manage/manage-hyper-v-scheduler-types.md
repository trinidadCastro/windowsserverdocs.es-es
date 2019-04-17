---
title: Descripción y uso de tipos de programador de hipervisor de Hyper-V
description: Proporciona información para los administradores del host de Hyper-V en el uso de programador de Hyper-V modos
author: allenma
ms.author: allenma
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows-server-hyper-v
ms.technology: virtualization
ms.localizationpriority: low
ms.assetid: 6cb13f84-cb50-4e60-a685-54f67c9146be
ms.openlocfilehash: 7af6d68b02367d349580eacb27405c6f37e97ff8
ms.sourcegitcommit: 3883eebbba70bfea0221e510863ee1a724a5f926
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "5783697"
---
# Administración de tipos de programador de hipervisor de Hyper-V

>Se aplica a: Windows 10, Windows Server 2016, Windows Server, versión 1709 y Windows Server, versión 1803, Windows Server 2019

En este artículo se describe nuevos modos de procesador virtual programación lógica que se presentó por primera vez en Windows Server 2016. Estos modos o tipos de programador, determinan cómo el hipervisor de Hyper-V asigna y administra de trabajo a través de procesadores virtuales invitadas. Un administrador del host de Hyper-V puede seleccionar los tipos de programador de hipervisor que son más adecuadas para las máquinas virtuales invitadas (VM) y configurar las máquinas virtuales para sacar partido de la lógica de programación.

>[!NOTE]
>Las actualizaciones deben usar las características de programador de hipervisor se describe en este documento. Para obtener más información, consulta [necesiten actualizarse](#required-updates).

## Segundo plano

Antes de tratar la lógica y los controles detrás de la programación de procesador virtual de Hyper-V, es útil revisar los conceptos básicos que se tratan en este artículo.

### Conocimiento SMT

Multithreading simultáneo o SMT, es una técnica que se usan en los diseños de procesador modernas que permite a los recursos del procesador a ser compartidos por subprocesos de ejecución independientes. SMT ofrece un aumento del rendimiento reducido para la mayoría de las cargas de trabajo en paralelo cálculos cuando sea posible, aumentar el rendimiento de la instrucción por lo general, aunque no rendimiento conseguir o incluso una pérdida ligeramente en el rendimiento puede producirse cuando contención entre subprocesos para se produce los recursos compartidos de procesador.
Procesadores admiten SMT están disponibles de Intel y AMD. Intel se refiere a sus ofertas de SMT como tecnología Hyper Threading de Intel o Intel HT.

Para los fines de este artículo, las descripciones de SMT y cómo se está utilizando Hyper-v se aplican por igual a los sistemas de Intel y AMD.

* Para obtener más información sobre la tecnología Intel HT, hacer referencia a la [Tecnología Hyper-Threading de Intel](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html)

* Para obtener más información sobre AMD SMT, consulte [La arquitectura de Core "Zen"](https://www.amd.com/en/technologies/zen-core)

## Comprender cómo Hyper-V virtualiza procesadores

Antes de considerar los tipos de programador de hipervisor, también es útil comprender la arquitectura de Hyper-V. Encontrarás un resumen general en [Introducción a la tecnología Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-technology-overview). Estos son conceptos importantes para este artículo:

* Hyper-V crea y administra las particiones de máquina virtual, a través de qué cálculo recursos se asignan y compartidos, bajo el control del hipervisor. Las particiones proporcionan los límites de aislamiento sólido entre todas las máquinas virtuales invitadas y entre máquinas virtuales invitadas y la partición raíz.

* La partición raíz está una partición de máquina virtual, aunque tiene propiedades únicas y mucho más privilegios que las máquinas virtuales invitadas. La partición raíz proporciona los servicios de administración que controlan todas las máquinas virtuales invitadas, proporciona compatibilidad con dispositivos virtuales de invitados y administra todas las E/S de los dispositivos para las máquinas virtuales invitadas. Microsoft recomienda encarecidamente no ejecuta las cargas de trabajo de la aplicación en la partición raíz.

* Cada procesador virtual (VP) de la partición raíz es 1:1 asignadas a un procesador subyacente lógico (LP). Un host VP siempre se ejecutará en el mismo LP subyacente: no hay ninguna migración de VP de la partición raíz.

* De manera predeterminada, el lógicos (LP) en el que ejecuta el host VP también puede ejecutar VP de invitado.

* Un Vicepresidente de invitado se puede programar por el hipervisor para ejecutarse en cualquier procesador lógico disponible. Mientras el programador de hipervisor se encarga a tener en cuenta ubicación de caché temporal, topología NUMA y muchos otros factores al programar una VP invitado, en última instancia el VP podría programar en cualquier host LP.

## Tipos de programador de hipervisor

A partir de Windows Server 2016, el hipervisor de Hyper-V admite varios modos de lógica del programador, que determinan cómo el hipervisor programa procesadores virtuales en los procesadores lógicos subyacentes. Estos tipos de programador son:

- [El programador de recurso compartido de equitativa clásico](#the-classic-scheduler)
- [El programador de core](#the-core-scheduler)
- [El programador de raíz](#the-root-scheduler)

### El programador clásico

El programador clásico ha sido el valor predeterminado para todas las versiones del hipervisor de Hyper-V de Windows desde su creación, incluido Windows Server 2016 Hyper-V. El programador clásico proporciona una parte equitativa, preferente modelo de programación de round robin de DNS para procesadores virtuales invitadas.

El tipo de programador clásica es el más apropiado para la mayoría de usos de Hyper-V tradicionales: para nubes privadas, los proveedores de hospedaje y así sucesivamente. Las características de rendimiento se comprenden bien y mejor están optimizadas para admitir una amplia variedad de escenarios de virtualización, como la suscripción en exceso de VP lógicos (LP), ejecuten heterogéneos muchas de las máquinas virtuales y cargas de trabajo al mismo tiempo, ejecutar escala mayor alto rendimiento de las máquinas virtuales, compatibilidad con la característica completa conjunto de Hyper-V sin restricciones y mucho más.

### El programador de core

El programador de hipervisor core es una alternativa a la lógica del programador clásica, introducida en Windows Server 2016 y Windows 10 versión 1607 nueva. El programador de core ofrece un límite de seguridad sólida para el aislamiento de carga de trabajo de invitado y la variabilidad de disminución del rendimiento para cargas de trabajo dentro de las máquinas virtuales que se ejecutan en un host de virtualización habilitada SMT. El programador de core permite SMT y que no sean SMT máquinas virtuales en ejecución al mismo tiempo en el mismo host de virtualización habilitada SMT.

El programador de core utiliza la topología SMT del host de virtualización y, opcionalmente, expone los pares de SMT en las máquinas virtuales invitadas y los grupos de programaciones de procesadores virtuales de invitado desde la misma máquina virtual en grupos de procesadores lógicos SMT. Esto se realiza de forma simétrica para que si lógicos (LP) se encuentran en grupos de dos, VP se programan en grupos de dos y nunca se comparte un núcleo entre las máquinas virtuales.
Cuándo se ha programado el VP para una máquina virtual sin SMT habilitado que VP consumirá el núcleo todo cuando se ejecuta.

El resultado general del programador de core es:

* Invitado VP están limitados que se ejecute en subyacente pares de núcleo físico, aislar una máquina virtual para los límites de núcleo de procesador, lo que disminuirá vulnerabilidad a los ataques de cualquier canal lateral de máquinas virtuales malintencionadas.

* Se reduce significativamente la variabilidad en el rendimiento.

* Potencialmente se reduce el rendimiento, porque si solo se puede ejecutar uno de un grupo de VP, solo una de las secuencias de instrucción en el núcleo se ejecutará mientras que el otro se queda inactivo.

* El sistema operativo y las aplicaciones que se ejecuta en la máquina virtual invitada pueden utilizar el comportamiento SMT y (API) para controlar y distribuir el trabajo a través de subprocesos SMT, al igual que cuando se ejecuta no virtualizado de interfaces de programación.

* Un límite de seguridad sólida para el aislamiento de carga de trabajo de invitado - VP invitado están restringidos a ejecutarse en pares de núcleo físico subyacente, reduciendo vulnerabilidad frente a ataques de cualquier canal lateral.

Se usará el programador de core de forma predeterminada a partir de Windows Server 2019. En Windows Server 2016, el programador de core es opcional y debe estar habilitado de forma explícita mediante el administrador del host de Hyper-V y el programador clásico es el valor predeterminado.

#### Deshabilita el comportamiento de programador de Core con host SMT

Si el hipervisor está configurado para usar el tipo de programador core, pero la funcionalidad SMT está deshabilitado o no está presente en el host de virtualización, el hipervisor usará el comportamiento de programador clásica, independientemente del tipo de programador de hipervisor configuración.

### El programador de raíz

El programador de raíz se incluyó en Windows 10 versión 1803. Cuando se habilita el tipo de programador de raíz, el hipervisor cedes un control de programación de trabajo a la partición raíz. El programador de NT en la instancia de la partición raíz del sistema operativo administra todos los aspectos de programación de trabajo al sistema lógicos (LP).

El programador de raíz trata los requisitos únicos inherentes con compatibilidad con una partición de utilidad para proporcionar aislamiento de carga de trabajo segura, ya usa protección de aplicaciones con Windows Defender (WDAG). En este escenario, salir de programación de responsabilidades a la raíz del sistema operativo ofrece varias ventajas. Por ejemplo, los controles de recursos de CPU aplicable para escenarios de contenedor pueden usarse con la partición de utilidad, simplificar la administración y la implementación. Además, el programador de raíz del sistema operativo puede recopilar métricas sobre la carga de trabajo de la CPU dentro del contenedor fácilmente y usar estos datos como entrada para la misma directiva de programación aplicable a todas las otras cargas de trabajo en el sistema. Estas mismas métricas también ayudan a claramente atributo trabajo realizado en un contenedor de aplicación en el sistema host. Estas métricas de seguimiento es más difícil con cargas de trabajo de máquina virtual tradicional, donde algún trabajo en nombre del todas las VM en ejecución tiene lugar en la partición raíz.

#### Uso de programador de raíz en los sistemas cliente

A partir de Windows 10 versión 1803, el programador de raíz se usa de manera predeterminada en los sistemas de cliente solo, donde el hipervisor puede estar habilitado en apoyo a la seguridad basada en virtualización y aislamiento de carga de trabajo WDAG y para el correcto funcionamiento de los sistemas futuros con arquitecturas de núcleo heterogéneos. Se trata de la configuración de programador de hipervisor compatible solo para los sistemas cliente. Los administradores de TI no deben intentar invalidar el tipo de programador de hipervisor de forma predeterminada en los sistemas de cliente de Windows 10.

#### Controles de recursos de CPU de máquina virtual y el programador de raíz

Los controles de recursos de procesador de máquina virtual proporcionados por Hyper-V no se admiten cuando está habilitado, el programador de raíz del hipervisor como lógica de programador del sistema operativo de raíz administra los recursos de host de forma global y no tiene un conocimiento de una VM Opciones de configuración específicas. Los controles de recursos del procesador de por máquina virtual de Hyper-V, como BLOQ, pesos y reservas, solo son aplicables cuando el hipervisor controla directamente VP programación, al igual que con los tipos de programador clásico y core.

#### Uso de programador de raíz en sistemas de servidor

El programador de raíz no se recomienda para su uso con Hyper-V en servidores en este momento, como sus características de rendimiento aún no se han totalmente caracterizadas y optimizado para dar cabida a la amplia gama de cargas de trabajo típicos de muchas de las implementaciones de virtualización de servidor.

## Habilitar SMT en las máquinas virtuales invitadas

Una vez que el hipervisor del host de virtualización se configura para usar el tipo de programador core, las máquinas virtuales invitadas pueden configurarse para utilizar SMT si lo deseas. Exponer el hecho de que VP es con tecnología de hyperthreading a una máquina virtual invitada permite que al programador en el sistema operativo invitado y las cargas de trabajo que se ejecuta en la máquina virtual para detectar y utilizar la topología SMT en sus propios programación del trabajo. En Windows Server 2016, invitado SMT no se configura de manera predeterminada y debe estar habilitado de forma explícita mediante el administrador del host de Hyper-V. A partir de Windows Server 2019, máquinas virtuales nuevas creadas en el host heredarán la topología SMT del host de manera predeterminada.  Es decir, una versión de que máquina virtual 9.0 creado en un host con 2 subprocesos SMT por núcleo también verán 2 subprocesos SMT por núcleo.

Se debe usar PowerShell para habilitar SMT en una máquina virtual de invitado; No hay ninguna interfaz de usuario proporcionada en el Administrador de Hyper-V.
Para habilitar SMT en una máquina virtual invitada, abre una ventana de PowerShell con permisos suficientes y el tipo:

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <n>
```

Donde <n> es el número de subprocesos SMT por núcleo el Invitado VM verán.  
Ten en cuenta que <n> = 0 se establece el valor de HwThreadCountPerCore para que coincida con el recuento de subprocesos SMT del host por valor principal.

>[!NOTE] 
>Establecer HwThreadCountPerCore = 0 se admite a partir de Windows Server 2019.

A continuación es un ejemplo de información del sistema extraído del sistema operativo invitado que se ejecuta en una máquina virtual con 2 procesadores virtuales y SMT habilitado. El sistema operativo invitado es detección de 2 procesadores lógicos que pertenecen al mismo núcleo.

![Captura de pantalla que muestra msinfo32 en una máquina virtual con SMT habilitado de invitado](media/Hyper-V-CoreScheduler-VM-Msinfo32.png)

## Configurar el tipo de programador de hipervisor de Windows Server 2016 Hyper-V

Windows Server 2016 Hyper-V usa el modelo de programador de hipervisor clásico de manera predeterminada. El hipervisor, opcionalmente, se puede configurar para usar al programador de core, para aumentar la seguridad restringiendo VP invitado que se ejecute en pares SMT físicos correspondientes y para admitir el uso de las máquinas virtuales con la programación de SMT para su VP invitado.

>[!NOTE]
>Microsoft recomienda que todos los clientes que ejecutan Windows Server 2016 Hyper-V seleccionan el programador de core para garantizar que sus hosts de virtualización de forma óptima están protegidos frente a máquinas virtuales invitadas potencialmente malintencionados.

## Valores predeterminados de Windows Server 2019 Hyper-V mediante el programador de core

Para ayudar a garantizar que los hosts de Hyper-V se implementan en el configuaration de seguridad óptimas, Windows Server 2019 Hyper-V ahora usará el modelo de core hipervisor programador de manera predeterminada. El administrador del host, opcionalmente, puede configurar el host para usar al programador de clásico heredado. Los administradores de TI cuidadosamente deben leer, comprender y tener en cuenta el impacto que tiene cada tipo de programador en la seguridad y el rendimiento de hosts de virtualización antes de reemplazar la configuración predeterminada de tipo de programador.  Para más información, consulta la [selección del tipo de programador de descripción de Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/understanding-hyper-v-scheduler-type-selection) .

### Actualizaciones necesarias

>[!NOTE]
>Las actualizaciones siguientes son necesarios para usar las características de programador de hipervisor se describe en este documento. Estas actualizaciones incluyen cambios para admitir la nueva opción de BCD 'hypervisorschedulertype', lo cual es necesaria para la configuración de host.

| Versión | Lanzamiento  | Se requiere una actualización | Artículo de Knowledge Base |
|--------------------|------|---------|-------------:|
|WindowsServer2016 | 1607 | 2018.07 C | [KB4338822](https://support.microsoft.com/help/4338822/windows-10-update-kb4338822) |
|WindowsServer2016 | 1703 | 2018.07 C | [KB4338827](https://support.microsoft.com/help/4338827/windows-10-update-kb4338827) |
|WindowsServer2016 | 1709 | 2018.07 C | [KB4338817](https://support.microsoft.com/help/4338817/windows-10-update-kb4338817) |
|Windows Server 2019 | 1804 | Ninguno | Ninguno |

## Seleccionar el tipo de programador de hipervisor de Windows Server

La configuración de programador de hipervisor se controla a través de la entrada de BCD hypervisorschedulertype.

Para seleccionar un tipo de programador, abre un símbolo del sistema con privilegios de administrador:

``` command
     bcdedit /set hypervisorschedulertype type
```

Donde `type` es uno de:

* Clásico
* Core

Debes reiniciar el sistema para que los cambios en el tipo de programador de hipervisor surta efecto.

>[!NOTE]
>El programador de raíz de hipervisor no se admite en Windows Server Hyper-V en este momento. Los administradores de Hyper-V no deben intentar configurar al programador de raíz para su uso con escenarios de virtualización de servidores.

## Determinar el tipo de programador actual

Puedes determinar el tipo de programador de hipervisor actual en uso examinando el registro del sistema en el Visor de eventos para el evento de lanzamiento más reciente de hipervisor Id. de 2, que indica el tipo de programador de hipervisor configurado en el inicio de hipervisor. Eventos de inicio de hipervisor pueden obtenerse desde el Visor de eventos de Windows, o a través de PowerShell.

2 de Id. de evento de lanzamiento de hipervisor denota el tipo de programador de hipervisor, donde:

    1 = Classic scheduler, SMT disabled

    2 = Classic scheduler

    3 = Core scheduler

    4 = Root scheduler

![Captura de pantalla que muestra hipervisor los detalles de evento 2 de Id. de inicio](media/Hyper-V-CoreScheduler-EventID2-Details.png)

![Captura de pantalla que muestra el Visor de eventos 2 de Id. de evento de lanzamiento de hipervisor](media/Hyper-V-CoreScheduler-EventViewer.png)

### Consultar el evento de inicio de tipo de programador para Hyper-V hipervisor mediante PowerShell

Para consultar para 2 de Id. de evento de hipervisor con PowerShell, escribe los siguientes comandos desde un símbolo del sistema de PowerShell.

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Hypervisor"; ID=2} -MaxEvents 1
```

![Captura de pantalla que muestra la consulta de PowerShell y los resultados para el evento de lanzamiento de hipervisor 2 de Id.](media/Hyper-V-CoreScheduler-PowerShell.png)
