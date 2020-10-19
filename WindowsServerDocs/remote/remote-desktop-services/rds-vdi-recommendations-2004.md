---
title: Optimización de Windows 10, compilación 2004, para un rol de escritorio virtual
description: Configuración y ajustes recomendados para minimizar la sobrecarga de los escritorios Windows 10, versión 2004, que se usan como imágenes de VDI.
ms.prod: windows-server
ms.reviewer: robsmi, timuessi
ms.technology: remote-desktop-services
author: Heidilohr
ms.author: helohr
ms.topic: article
manager: ''
ms.date: 09/24/2020
ms.openlocfilehash: 129bbc8387be655cca563935cb7ff891664da357
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92155896"
---
# <a name="optimizing-windows-10-version-2004-for-a-virtual-desktop-infrastructure-vdi-role"></a>Optimización de Windows 10, versión 2004, para un rol de Infraestructura de escritorio virtual (VDI)

El objetivo de este artículo es proporcionar sugerencias para las configuraciones de Windows 10, compilación 2004, con el fin de lograr un rendimiento óptimo en entornos de escritorio virtualizados, como Infraestructura de escritorio virtual (VDI) y Windows Virtual Desktop. Toda la configuración de esta guía son meras recomendaciones y no se consideran requisitos en absoluto.

La información de esta guía corresponde a la compilación del sistema operativo (SO) 19041 de Windows 10, versión 2004.

Los principios de referencia para optimizar el rendimiento de Windows 10 en un entorno de escritorio virtual son minimizar la repetición de gráficos y sus efectos, las actividades en segundo plano que no tienen una ventaja importante para el entorno de escritorio virtual y, en general, reducir los procesos en ejecución al mínimo. Un objetivo secundario es reducir el uso del espacio en disco de la imagen base al mínimo. Con las implementaciones de escritorio virtual, el tamaño de imagen base o de tipo "gold" más pequeño posible puede reducir ligeramente el uso de la memoria en el sistema host, así como provocar una pequeña reducción en las operaciones de red generales necesarias para entregar el entorno de escritorio al consumidor.

Ninguna optimización debe reducir la experiencia del usuario. Cada configuración de optimización se ha revisado detenidamente para garantizar que no existe ninguna degradación apreciable de la experiencia del usuario.

> [!NOTE]
> La configuración de este artículo se puede aplicar a otras instalaciones de Windows 10, como la versión 1909, dispositivos físicos u otras máquinas virtuales. En este artículo no hay ninguna recomendación que afecte a la compatibilidad de Windows 10 en un entorno de escritorio virtual.

## <a name="vdi-optimization-principles"></a>Principios de optimización de VDI

Un entorno de escritorio virtual "completo" puede presentar una sesión de escritorio completa (incluidas las aplicaciones) a un usuario de PC a través de una red. El vehículo de entrega de red puede ser una red local o Internet, o bien ambas opciones. Algunas implementaciones de entornos de escritorio virtual usan una imagen del sistema operativo base, que luego se convierte en la base de los escritorios que posteriormente se presentan a los usuarios para que puedan trabajar. Existen variaciones de las implementaciones de escritorio virtual, como "persistente", "no persistente" y "sesión de escritorio". El tipo persistente conserva los cambios en el sistema operativo del escritorio virtual de una sesión a la siguiente. El tipo no persistente no conserva los cambios en el sistema operativo del escritorio virtual de una sesión a la siguiente. Para el usuario este escritorio es un poco diferente a otro dispositivo virtual o físico, aparte de que se accede a él a través de una red.

La configuración de optimización se lleva a cabo en una máquina de referencia. Una máquina virtual (VM) es un lugar ideal para compilar la VM, ya que se puede guardar el estado, crear puntos de control, realizar copias de seguridad, etc. Una instalación predeterminada del sistema operativo se lleva a cabo en la VM base. Después, a fin de optimizar la VM base, se quitan las aplicaciones innecesarias, instalan las actualizaciones de Windows, instalan otras actualizaciones, eliminan los archivos temporales, aplica la configuración, etc.

Existen otros tipos de tecnología de escritorio virtual, como Sesión de Escritorio remoto (RDS) y [Windows Virtual Desktop](https://azure.microsoft.com/services/virtual-desktop/) de Microsoft Azure, que se ha lanzado recientemente. La explicación detallada de estas tecnologías está fuera del ámbito de este artículo. Este artículo se centra en la configuración de la imagen base de Windows, sin hacer referencia a otros factores del entorno, como la optimización de hardware del host.

La seguridad y la estabilidad son algunas de las principales prioridades de Microsoft cuando se trata de productos y servicios. En el dominio de escritorio virtual, la seguridad no se administra de forma muy diferente que en los dispositivos físicos. Los clientes empresariales pueden optar por usar los servicios integrados en Windows de Seguridad de Windows, que incluyen un conjunto de servicios que funcionan bien independientemente se si están conectados a Internet. En el caso de los entornos de escritorio virtual que no están conectados a Internet, las firmas de seguridad se pueden descargar varias veces al día, ya que Microsoft puede publicar más de una actualización de firma al día. Después, esas firmas se pueden proporcionar a los dispositivos de escritorio virtual y programarse para que se instalen durante la producción, independientemente de que sean persistentes o no persistentes. De este modo, la protección de la máquina virtual está lo más actualizada posible.

Hay algunas configuraciones de seguridad que no son aplicables a los entornos de escritorio virtual que no están conectados a Internet y, por lo tanto, no pueden participar en la seguridad habilitada para la nube. Hay otras configuraciones que los dispositivos Windows "normales" pueden usar, como la experiencia en la nube, Microsoft Store, etc. Quitar el acceso a las características no usadas reduce la superficie, el ancho de banda de red y la superficie expuesta a ataques.

En lo que respecta a las actualizaciones, Windows 10 emplea un ritmo de actualización mensual. En algunos casos, los administradores de escritorios virtuales controlan el proceso de actualización a través de un proceso de apagado de máquinas virtuales basado en una imagen "maestra" o "gold", quitan el sello de la imagen, que es de solo lectura, aplican revisiones a la imagen y, a continuación, la vuelven a sellar y la devuelven a producción. Por lo tanto, no es necesario que los dispositivos de escritorio virtual comprueben Windows Update. Sin embargo, hay casos en que se llevan a cabo procedimientos de revisión normales, como sucede con los dispositivos de escritorio virtual persistentes "personales". En algunos casos, se puede utilizar Windows Update. En algunos casos, se podría usar Intune. En determinados casos, se emplea Microsoft Endpoint Configuration Manager (anteriormente SCCM) para administrar la actualización y otras entregas de paquetes. Es responsabilidad de cada organización determinar el mejor enfoque para actualizar los dispositivos de escritorio virtual y, al mismo tiempo, reducir los ciclos de sobrecarga.

La configuración de la directiva local, así como muchas otras opciones de esta guía, se pueden invalidar con una directiva basada en dominio. Se recomienda repasar la configuración de la directiva minuciosamente, y quitar o no usar ninguna opción que no desee o no sea aplicable a su entorno. La configuración que se muestra en este documento intenta lograr el mejor equilibrio de optimización del rendimiento en entornos de escritorio virtual, al mismo tiempo que mantener una experiencia de usuario de calidad.

> [!NOTE]
> Hay un conjunto de scripts disponible en GitHub.com, que realizará todos los elementos de trabajo documentados en este documento. La dirección URL de Internet para los scripts de optimización se puede encontrar en [https://github.com/The-Virtual-Desktop-Team/Virtual-Desktop-Optimization-Tool](https://github.com/The-Virtual-Desktop-Team/Virtual-Desktop-Optimization-Tool). Este script se diseñó para que pudiera personalizarse fácilmente para su entorno y sus requisitos. El código principal es PowerShell y el trabajo se realiza mediante la llamada a archivos de entrada, que son de texto sin formato (ahora .json) y también con archivos de exportación de la herramienta Objeto de directiva de grupo local (LGPO). Estos archivos de texto contienen listas de aplicaciones que se deben quitar, los servicios que se deben deshabilitar, etc. Si no quiere quitar una aplicación determinada o deshabilitar un servicio en concreto, puede editar el archivo de texto correspondiente y quitar el elemento con el que no quiere interactuar. Por último, existe una exportación de la configuraciones de directiva local que se puede importar en las máquinas del entorno. Es mejor tener alguna configuración dentro de la imagen base, que aplicar la configuración a través de la directiva de grupo, ya que algunas de las opciones de configuración son efectivas en el siguiente reinicio o cuando se usa por primera vez un componente.

### <a name="persistent-virtual-desktop-environments"></a>Entornos de escritorio virtual persistentes

El escritorio virtual persistente se encuentra en el nivel básico, un dispositivo que guarda el estado del sistema operativo entre reinicios. Otros niveles de software de la solución de escritorio virtual proporcionan a los usuarios acceso fácil y sin problemas a sus VM asignadas, a menudo con una solución de inicio de sesión único.

Existen varias implementaciones diferentes de escritorio virtual persistente:

- Las VM tradicionales, donde la VM tiene su propio archivo de disco virtual, se inicia normalmente y guarda los cambios de una sesión a la siguiente. La diferencia radica en cómo accede el usuario a esa VM. Puede haber un portal web en el que el usuario inicie sesión y que dirija automáticamente al usuario a uno o varios dispositivos de escritorio virtual (VM) que tiene asignados.
- VM persistentes basadas en imágenes, opcionalmente con discos virtuales personales (PVD). En este tipo de implementación hay una imagen base o de tipo "gold" en uno o más servidores host. Se crea una VM y, a continuación, uno o más discos virtuales se crean y asignan a este disco para obtener un almacenamiento persistente.
  - Cuando se inicia la VM, se lee una copia de la imagen base en el espacio de memoria de esa VM. Al mismo tiempo, un disco virtual persistente asignado a esa VM, con las diferencias del sistema operativo anterior fusionadas mediante un proceso complejo.
  - Los cambios, tales como escrituras del registro de eventos, escrituras de registro, etc., se redirigen al disco virtual de lectura/escritura asignado a esa VM.
  - En esta circunstancia, el sistema operativo y el mantenimiento de la aplicación pueden funcionar normalmente si se usa software de mantenimiento tradicional, como Windows Server Update Services u otras tecnologías de administración.
- La diferencia entre un dispositivo de escritorio virtual persistente y un dispositivo de escritorio virtual "normal" es la relación con la imagen maestra/gold. En algún momento, se deben aplicar actualizaciones a la imagen maestra. Es en este punto donde las organizaciones deciden cómo se administran los cambios persistentes del usuario. En algunos casos, el disco con los cambios del usuario se descarta o se restablece. También podría ser que los cambios que realiza el usuario en la máquina se mantengan a través de actualizaciones de calidad mensuales y que la base se restablezca después de una actualización de características.

### <a name="non-persistent-virtual-desktop-environments"></a>Entornos de escritorio virtual no persistentes

Cuando una implementación de escritorio virtual no persistente se basa en una imagen base o de tipo "gold", las optimizaciones se realizan principalmente en la imagen base y luego a través de la configuración local y las directivas locales.

Con los entornos de escritorio virtual no persistentes (NP) basados en imágenes, la imagen base es de solo lectura. Cuando se inicia un dispositivo de escritorio virtual NP (VM), se transmite una copia de la imagen base a la VM. La actividad que se produce durante el inicio del proceso hasta que el siguiente reinicio, se redirige a una ubicación temporal. Por lo general, los usuarios reciben ubicaciones de red para almacenar sus datos. En algunos casos, el perfil del usuario se combina con la VM estándar para proporcionar al usuario su configuración.

Un aspecto importante del escritorio virtual NP que se basa en una sola imagen es el mantenimiento. Las actualizaciones del sistema operativo (SO) y sus componentes se entregan generalmente una vez al mes. Con el entorno de escritorio virtual basado en imágenes, se deben realizar un conjunto de procesos para obtener las actualizaciones en la imagen:

- En un host determinado, todas las VM en ese host que se basan en la imagen base deben apagarse o desactivarse. Esto significa que los usuarios se redirigen a otras VM.

- En algunas implementaciones, esto se conoce como "purga". El host de la máquina virtual o de la sesión, cuando se establece en el modo de purga, deja de aceptar nuevas solicitudes, pero continúa atendiendo a los usuarios conectados actualmente al dispositivo.

- En el modo de purga, cuando el último usuario cierra la sesión del dispositivo, ese dispositivo está listo para las operaciones de mantenimiento.
- A continuación, la imagen base se abre y se inicia. Posteriormente, se realizan todas las actividades de mantenimiento, como actualizaciones del sistema operativo, actualizaciones de .NET, actualizaciones de aplicaciones, etc.
- Cualquier configuración nueva que deba aplicarse se realiza en ese momento.
- Cualquier otro mantenimiento se realiza en ese momento.
- La imagen base se cierra.
- La imagen base se sella y se establece para volver al entorno de producción.
- Los usuarios pueden volver a iniciar sesión.

> [!NOTE]
> Windows 10 realiza un conjunto de tareas de mantenimiento automáticamente y de forma periódica. Existe una tarea programada que está configurada para ejecutarse a las 3:00 A.M. todos los días de forma predeterminada. Esta tarea programada realiza una lista de tareas, incluida la limpieza de Windows Update. Puedes ver todas las categorías de mantenimiento que tienen lugar automáticamente con este comando de PowerShell:
>
> ```powershell
> Get-ScheduledTask | Where-Object {$_.Settings.MaintenanceSettings}
> ```

Uno de los desafíos del escritorio virtual no persistente es que cuando un usuario cierra la sesión, se descarta casi toda la actividad del sistema operativo. El perfil y el estado del usuario pueden guardarse en una ubicación centralizada, pero la propia máquina virtual descarta casi todos los cambios realizados desde el último arranque. Por lo tanto, las optimizaciones destinadas a un equipo con Windows que guarda el estado de una sesión a la siguiente no son aplicables.

Dependiendo de la arquitectura del dispositivo de escritorio virtual, cosas como PreFetch y SuperFetch no van a ayudar en el proceso de una sesión a la siguiente, ya que todas las optimizaciones se descartan al reiniciar la VM. La indexación puede ser un desperdicio parcial de recursos, igual que lo sería cualquier optimización de disco, como una desfragmentación tradicional.

> [!NOTE]
> Si está preparando una imagen con virtualización y está conectado a Internet durante el proceso de creación de la imagen, en el primer inicio de sesión debe posponer las actualizaciones de características en **Configuración** > **Windows Update**.

### <a name="to-sysprep-or-not-sysprep"></a>Sysprep o no Sysprep

Windows 10 tiene una funcionalidad integrada llamada [Herramienta de preparación del sistema](/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview) (a menudo abreviada como "Sysprep"). La herramienta Sysprep se usa para preparar una imagen de Windows 10 personalizada para la duplicación. El proceso de Sysprep garantiza que el sistema operativo resultante sea único y se pueda ejecutar en la producción.

Existen varias razones a favor y en contra de ejecutar Sysprep. En el caso de los entornos de escritorio virtual, es posible que desee poder personalizar el perfil de usuario predeterminado que se usará como plantilla de perfil para los usuarios subsiguientes que inicien sesión con esta imagen. También es posible que tenga aplicaciones que quiera instalar, además de poder controlar la configuración de cada aplicación.

La alternativa es usar un .ISO estándar para realizar la instalación, mediante un archivo de respuesta de instalación desatendida, y una secuencia de tareas para instalar aplicaciones o eliminarlas. También puedes usar una secuencia de tareas para establecer la configuración de la directiva local en la imagen; por ejemplo, puedes usar la herramienta [Utilidad de objetos de la directiva de grupo local (LGPO)](/archive/blogs/secguide/lgpo-exe-local-group-policy-object-utility-v1-0).

Para más información sobre la preparación de la imagen para Azure, consulte [Preparación de un VHD o un VHDX de Windows antes de cargarlo en Azure](/azure/virtual-machines/windows/prepare-for-upload-vhd-image).

### <a name="supportability"></a>Compatibilidad

Cada vez que se cambian los valores predeterminados de Windows, surgen preguntas sobre la compatibilidad. Una vez personalizada una imagen de escritorio virtual (VM o sesión), es necesario realizar un seguimiento de todos los cambios realizados en la imagen en un registro de cambios. En el caso de tener que solucionar algún problema, a menudo se puede aislar una imagen en un grupo y configurarla para el análisis del problema. Una vez realizado el seguimiento de un problema hasta la causa raíz, ese cambio se puede implementar primero en el entorno de prueba y, por último, en la carga de trabajo de producción.

En este documento se evita intencionadamente tocar los servicios del sistema, las directivas o las tareas que afectan a la seguridad. De ellos se encarga el mantenimiento de Windows. Se quita la capacidad de mantenimiento de las imágenes de escritorio virtual de las ventanas de mantenimiento, ya que estas ventanas se dan cuando se producen la mayoría de los eventos de mantenimiento en entornos de escritorio virtual, excepto para las actualizaciones de software de seguridad. Microsoft ha publicado instrucciones sobre Seguridad de Windows en entornos de escritorio virtual en:

**Microsoft**: [Guía de implementación del Antivirus de Windows Defender en un entorno de infraestructura de escritorio virtual (VDI)](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/deployment-vdi-windows-defender-antivirus)

Tenga en cuenta la compatibilidad al modificar la configuración predeterminada de Windows. Pueden surgir problemas difíciles de resolver al modificar servicios del sistema, directivas o tareas programadas, en nombre de la protección, "iluminación", etc. Consulta en Microsoft Knowledge Base los problemas conocidos actuales relacionados con la modificación de la configuración predeterminada. Las instrucciones de este documento y el script asociado en GitHub se conservarán con respecto a los problemas conocidos, en caso de que surjan. Además, los problemas se pueden notificar de varias maneras a Microsoft.

Puede usar su motor de búsqueda favorito con los términos `"start value" site:support.microsoft.com` para mostrar los problemas conocidos relacionados con los valores de inicio predeterminados de los servicios.

Ten en cuenta que este documento y los scripts asociados de GitHub no modifican los permisos predeterminados. Si estás interesado en aumentar la configuración de seguridad, comienza con el proyecto conocido como **AaronLocker**. Para obtener más información, consulte la [información general sobre "AaronLocker"](https://github.com/microsoft/AaronLocker).

### <a name="virtual-desktop-optimization-categories"></a>Categorías de optimización de escritorio virtual

- Limpieza de la aplicación Plataforma universal de Windows (UWP)
- Limpieza de las características opcionales
- Configuración de directivas locales
- Servicios del sistema
- Tareas programadas
- Aplicación de actualizaciones de Windows (y otras)
- Seguimientos de Windows automáticos
- Optimización de Windows Defender con VDI
- Ajuste del rendimiento de red del cliente mediante la configuración del registro
- Configuración adicional de las instrucciones de "Línea de base de funcionalidades limitadas de tráfico restringido de Windows".
- Limpieza de disco

### <a name="universal-windows-platform-uwp-application-cleanup"></a>Limpieza de aplicaciones para la Plataforma universal de Windows (UWP)

Uno de los objetivos de una imagen de escritorio virtual es ser lo más ligera posible con respecto al almacenamiento persistente. Una forma de reducir el tamaño de la imagen es quitar las aplicaciones para UWP que no se usarán en el entorno. Además de las aplicaciones para UWP, están los archivos principales de la aplicación, también conocidos como la carga útil. Existe una pequeña cantidad de datos almacenados en el perfil de cada usuario para la configuración específica de la aplicación. También hay una pequeña cantidad de datos en el perfil "Todos los usuarios".

Además, todas las aplicaciones para UWP se registran en el nivel del usuario o de la máquina en algún momento después del inicio del dispositivo e inician sesión para el usuario. Las aplicaciones para UWP, que incluyen el menú Inicio y el shell de Windows, realizan varias tareas durante la instalación o después de esta, de nuevo cuando un usuario inicia sesión por primera vez y en menor medida en inicios de sesión posteriores. Para todas las aplicaciones para UWP, tienen lugar evaluaciones ocasionales, tales como:

- ¿Necesita actualizar la aplicación a la versión más reciente?
- La aplicación, si está anclada al menú Inicio, puede tener datos de iconos dinámicos para descargar.
- ¿La aplicación tiene una memoria caché de datos que deben actualizarse, como los mapas o el tiempo?
- ¿La aplicación tiene datos persistentes del perfil del usuario que deben presentarse al iniciar sesión (por ejemplo, Notas rápidas)?

Con una instalación predeterminada de Windows 10, es posible que una organización no utilice todas las aplicaciones para UWP. Por lo tanto, si se quitan esas aplicaciones, se reducirán las evaluaciones necesarias, la cantidad de almacenamiento en caché, etc. El segundo método consiste en hacer que Windows deshabilite las "experiencias del consumidor". Esto reduce la actividad de Store al tener que buscar todos los usuarios que tienen instaladas las aplicaciones y las aplicaciones que están disponibles y, a continuación, empezar a descargar algunas aplicaciones para UWP. El ahorro de rendimiento puede ser considerable cuando hay cientos o miles de usuarios y todos empiezan a trabajar aproximadamente a la misma hora, o incluso si empiezan a trabajar de manera consecutiva en las distintas zonas horarias.

La conectividad y el tiempo son factores importantes cuando se trata de la limpieza de la aplicación para UWP. Si implementa la imagen base en un dispositivo sin conectividad de red, Windows 10 no podrá conectarse a Microsoft Store, ni descargar aplicaciones o intentar instalarlas al mismo tiempo que usted intenta desinstalarlas. Puede ser una buena estrategia para tener tiempo de personalizar la imagen y luego actualizar lo que queda en una fase posterior del proceso de creación de la imagen.

Si modifica el archivo .WIM base que usa para instalar Windows 10 y elimina las aplicaciones para UWP innecesarias de dicho archivo antes de realizar la instalación, esas aplicaciones no se instalarán desde el principio y los tiempos de creación de perfiles posteriores serán más cortos. Más adelante en esta sección, encontrará un vínculo con información sobre cómo quitar aplicaciones para UWP del archivo .WIM de instalación.

Una buena estrategia para el entorno de escritorio virtual es aprovisionar las aplicaciones que desea en la imagen base y, a continuación, limitar o bloquear el acceso a Microsoft Store. Las aplicaciones de Store se actualizan periódicamente en segundo plano en equipos normales. Las aplicaciones para UWP pueden actualizarse durante la ventana de mantenimiento cuando se aplican otras actualizaciones.

#### <a name="delete-the-payload-of-uwp-apps"></a>Eliminar la carga útil de las aplicaciones para UWP

Las aplicaciones para UWP que no son necesarias todavía están en el sistema de archivos y consumen una pequeña cantidad de espacio en disco. En cuanto a las aplicaciones que nunca serán necesarias, la carga útil de aplicaciones para UWP no deseadas se puede eliminar de la imagen base mediante los comandos de PowerShell. Si elimina las cargas útiles de las aplicaciones para UWP del archivo .WIM de instalación mediante los vínculos que se proporcionan más adelante en esta sección, podrá comenzar desde el principio con una lista muy reducida de aplicaciones para UWP.

Ejecute el siguiente comando para enumerar las aplicaciones para UWP aprovisionadas desde un sistema operativo en ejecución, como en este ejemplo de salida truncado de PowerShell:

```powershell
Get-AppxProvisionedPackage -Online

DisplayName  : Microsoft.3DBuilder
Version      : 13.0.10349.0  
Architecture : neutral
ResourceId   : \~
PackageName  : Microsoft.3DBuilder_13.0.10349.0_neutral_\~_8wekyb3d8bbwe
Regions      :

DisplayName  : Microsoft.Appconnector
Version      : 2015.707.550  
Architecture : neutral
ResourceId   : \~
PackageName  : Microsoft.Appconnector_2015.707.550.0_neutral_\~_8wekyb3d8bbwe
Regions      :
...
```

Las aplicaciones para UWP que se aprovisionan en un sistema se pueden quitar durante la instalación del sistema operativo como parte de una secuencia de tareas, o más tarde después de instalarlo. Este podría ser el mejor método, porque hace que el proceso general de creación o mantenimiento de una imagen sea modular. Una vez que desarrolles los scripts, si algo cambia en una compilación posterior, solo tienes que editar un script existente en lugar de repetir el proceso desde cero.

Si desea obtener más información, aquí tiene algunos recursos que pueden resultarle útiles:

- [Removing Windows 10 in-box apps during a task sequence](/archive/blogs/mniehaus/removing-windows-10-in-box-apps-during-a-task-sequence) (Quitar aplicaciones de Windows 10 en el equipo durante una secuencia de tareas)
- [Removing Built-in apps from Windows 10 WIM-File with PowerShell - Version 1.3](https://gallery.technet.microsoft.com/Removing-Built-in-apps-65dc387b) (Quitar aplicaciones integradas del archivo .WIM de Windows 10 con PowerShell - Versión 1.3)
- [Windows 10 1607: Keeping apps from coming back when deploying the feature update](https://blogs.technet.microsoft.com/mniehaus/2016/08/23/windows-10-1607-keeping-apps-from-coming-back-when-deploying-the-feature-update/) (Evitar que las aplicaciones vuelvan a aparecer al implementar la actualización de características)
- [Removing Windows 10 in-box apps during a task sequence](https://blogs.technet.microsoft.com/mniehaus/2015/11/11/removing-windows-10-in-box-apps-during-a-task-sequence/) (Quitar aplicaciones de Windows 10 en el equipo durante una secuencia de tareas)

A continuación, ejecute el siguiente comando de PowerShell para quitar las cargas útiles de las aplicaciones para UWP:

```powershell
Remove-AppxProvisionedPackage -Online - PackageName MyAppxPackage
```

Como nota final sobre el tema, cada aplicación para UWP debe evaluarse para determinar su aplicabilidad en cada entorno único. Puede instalar una instalación predeterminada de Windows 10, versión 2004, y luego observar qué aplicaciones se están ejecutando y cuáles están consumiendo la memoria. Por ejemplo, puede considerar la posibilidad de quitar las aplicaciones que se inician automáticamente, o las aplicaciones que muestran automáticamente información en el menú Inicio (como el Tiempo y las Noticias), y que podrían no ser de utilidad en el entorno.

> [!NOTE]
> Si usa los scripts de GitHub, puede controlar fácilmente qué aplicaciones se van a quitar antes de ejecutar el script. Después de descargar los archivos de script, busque el archivo AppxPackages.json, edítelo y quite las entradas de las aplicaciones que quiera conservar, como Calculadora, Notas rápidas, etc.

## <a name="windows-optional-features-cleanup"></a>Limpieza de características opcionales de Windows

### <a name="managing-optional-features-with-powershell"></a>Administración de características opcionales con PowerShell

Microsoft: [Windows 10: Administración de características opcionales con PowerShell](https://social.technet.microsoft.com/wiki/contents/articles/39386.windows-10-managing-optional-features-with-powershell.aspx)

Puedes administrar las características opcionales de Windows mediante PowerShell. Para enumerar las características de Windows instaladas actualmente, ejecuta el siguiente comando de PowerShell:

```powershell
Get-WindowsOptionalFeature -Online
```

Con PowerShell, una característica opcional de Windows enumerada se puede configurar como habilitada o deshabilitada, como en el ejemplo siguiente:

```powershell
Enabled-WindowsOptionalFeature -Online -FeatureName "DirectPlay" -All
```

Este es un comando de ejemplo que deshabilita la característica Reproductor de Windows Media en la imagen de escritorio virtual:

```powershell
Disable-WindowsOptionalFeature -Online -FeatureName "WindowsMediaPlayer"
```

A continuación, puede quitar el paquete del Reproductor de Windows Media. Este comando de ejemplo le mostrará cómo hacerlo:

```powershell

PS C:\> Get-WindowsPackage -Online -PackageName *media*

PackageName              : Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.19041.153
Applicable               : True
Copyright                : Copyright (c) Microsoft Corporation. All Rights Reserved
Company                  :
CreationTime             :
Description              : Play audio and video files on your local machine and on the Internet.
InstallClient            : DISM Package Manager Provider
InstallPackageName       : Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.19041.153.mum
InstallTime              : 5/11/2020 5:43:37 AM
LastUpdateTime           :
DisplayName              : Windows Media Player
ProductName              : Microsoft-Windows-MediaPlayer-Package
ProductVersion           :
ReleaseType              : OnDemandPack
RestartRequired          : Possible
SupportInformation       : http://support.microsoft.com/?kbid=777777
PackageState             : Installed
CompletelyOfflineCapable : Undetermined
CapabilityId             : Media.WindowsMediaPlayer~~~~0.0.12.0
Custom Properties        :

Features                 : {}
```

Si quiere quitar el paquete del Reproductor de Windows Media (para liberar aproximadamente 60 MB de espacio en disco), puede ejecutar el comando siguiente:

```powershell
PS C:\Windows\system32> Remove-WindowsPackage -PackageName Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.19041.153 -Online

Path          :
Online        : True
RestartNeeded : False
```

### <a name="enable-of-disabling-windows-features-using-dism"></a>Habilitación de las características de deshabilitación de Windows mediante DISM

Puedes usar la herramienta integrada Dism.exe para enumerar y controlar las características opcionales de Windows. Se puede desarrollar un script Dism.exe y ejecutarlo durante una secuencia de tareas de instalación del sistema operativo. La tecnología de Windows implicada se denomina Funciones bajo demanda. Consulte el artículo siguiente para obtener más información sobre la tecnología Características a petición de Windows:

Microsoft: [Características a petición](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities)

### <a name="default-user-settings"></a>Configuración de usuario predeterminado

Puede personalizar el archivo de registro de Windows en `C:\Users\Default\NTUSER.DAT`. Cualquier cambio de configuración que realice en este archivo se aplicará a los perfiles de usuario posteriores creados desde una máquina que ejecute esta imagen. Puede controlar la configuración que se va a aplicar al perfil de usuario predeterminado; para ello, edite el archivo **DefaultUserSettings.txt**.

Para reducir la transmisión de datos gráficos a través de la infraestructura de escritorio virtual, puede establecer el fondo predeterminado en un color sólido, en lugar de usar la imagen predeterminada de Windows 10. También puede establecer que la pantalla de inicio de sesión sea de color sólido, así como desactivar el efecto de desenfoque opaco en el inicio de sesión.

La siguiente configuración se aplica al subárbol del registro del perfil de usuario predeterminado, principalmente para reducir las animaciones. Si no quiere algunas o ninguna de estas configuraciones, elimine las que no desee aplicar a los nuevos perfiles de usuario basados en esta imagen. El objetivo de esta configuración es habilitar la siguiente configuración equivalente:

- Mostrar sombras bajo el puntero del mouse.
- Mostrar sombras bajo las ventanas.
- Suavizar los bordes suaves en las fuentes de la pantalla.

![Captura de pantalla del menú de opciones de rendimiento con los elementos pertinentes seleccionados.](media/performance-options.png)

La novedad de esta versión de configuración es un método para deshabilitar los dos valores de privacidad siguientes para cualquier perfil de usuario creado después de ejecutar la optimización:

- Dejar que los sitios web ofrezcan contenido relevante a nivel local mediante el acceso a mi lista de idiomas
- Mostrarme contenido sugerido en la aplicación Configuración

![Captura de pantalla de la ventana de configuración de privacidad. Los dos valores deshabilitados están resaltados en rojo.](media/privacy-settings.png)

A continuación, se muestra la configuración de optimización que se aplica al subárbol del registro del perfil de usuario predeterminado para optimizar el rendimiento: Tenga en cuenta que para esta operación, primero se carga el subárbol del registro del perfil de usuario predeterminado **NTUser.dat**, como nombre de clave efímera **Temp** y, a continuación, se realizan las modificaciones de la lista siguiente:

```regedit
Load HKLM\Temp C:\Users\Default\NTUSER.DAT
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer" /v ShellState /t REG_BINARY /d 240000003C2800000000000000000000 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v IconsOnly /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v ListviewAlphaSelect /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v ListviewShadow /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v ShowCompColor /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v ShowInfoTip /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v TaskbarAnimations /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects" /v VisualFXSetting /t REG_DWORD /d 3 /f
add "HKLM\Temp\Software\Microsoft\Windows\DWM" /v EnableAeroPeek /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\DWM" /v AlwaysHiberNateThumbnails /t REG_DWORD /d 0 /f
add "HKLM\Temp\Control Panel\Desktop" /v DragFullWindows /t REG_SZ /d 0 /f
add "HKLM\Temp\Control Panel\Desktop" /v FontSmoothing /t REG_SZ /d 2 /f
add "HKLM\Temp\Control Panel\Desktop" /v UserPreferencesMask /t REG_BINARY /d 9032078010000000 /f
add "HKLM\Temp\Control Panel\Desktop\WindowMetrics" /v MinAnimate /t REG_SZ /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\StorageSense\Parameters\StoragePolicy" /v 01 /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager" /v SubscribedContent-338393Enabled /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager" /v SubscribedContent-353694Enabled /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager" /v SubscribedContent-353696Enabled /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager" /v SubscribedContent-338388Enabled /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager" /v SubscribedContent-338389Enabled /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager" /v SystemPaneSuggestionsEnabled /t REG_DWORD /d 0 /f
add "HKLM\Temp\Control Panel\International\User Profile" /v HttpAcceptLanguageOptOut /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications\Microsoft.Windows.Photos_8wekyb3d8bbwe" /v Disabled /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications\Microsoft.Windows.Photos_8wekyb3d8bbwe" /v DisabledByUser /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications\Microsoft.SkypeApp_kzf8qxf38zg5c" /v Disabled /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications\Microsoft.SkypeApp_kzf8qxf38zg5c" /v DisabledByUser /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications\Microsoft.YourPhone_8wekyb3d8bbwe" /v Disabled /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications\Microsoft.YourPhone_8wekyb3d8bbwe" /v DisabledByUser /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications\Microsoft.MicrosoftEdge_8wekyb3d8bbwe" /v Disabled /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications\Microsoft.MicrosoftEdge_8wekyb3d8bbwe" /v DisabledByUser /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\InputPersonalization" /v RestrictImplicitInkCollection /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\InputPersonalization" /v RestrictImplicitTextCollection /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Personalization\Settings" /v AcceptedPrivacyPolicy /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\InputPersonalization\TrainedDataStore" /v HarvestContacts /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\UserProfileEngagement" /v ScoobeSystemSettingEnabled /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\UserProfileEngagement" /v ScoobeSystemSettingEnabled /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\UserProfileEngagement" /v ScoobeSystemSettingEnabled /t REG_DWORD /d 0 /f
Unload HKLM\Temp
```

Otra serie de opciones de configuración de usuario predeterminadas que se agregaron recientemente son para deshabilitar el inicio y la ejecución de varias aplicaciones de Windows en segundo plano. Aunque no es importante en un único dispositivo, Windows 10 inicia varios procesos para cada sesión de usuario en un dispositivo determinado (host). La aplicación Skype es un ejemplo y Microsoft Edge es otro. La configuración incluye desactivar la posibilidad de que varias aplicaciones puedan ejecutarse en segundo plano. Si se desea esta funcionalidad tal cual, simplemente elimine las líneas del archivo "DefaultUserSettings.txt" que incluyen los nombres de aplicación "**Windows.Photos**", "**SkypeApp**", "**YourPhone**" o "**MicrosoftEdge**".

### <a name="local-policy-settings"></a>Configuración de directivas locales

Se pueden crear muchas optimizaciones para Windows 10 en un entorno de escritorio virtual mediante la directiva de Windows. Las configuraciones que se indican en la tabla de esta sección se pueden aplicar localmente a la imagen base o gold. Luego, si la configuración equivalente no se especifica de ninguna otra manera como, por ejemplo, una directiva de grupo, la configuración seguirá aplicándose.

Tenga en cuenta que algunas decisiones pueden basarse en los aspectos específicos del entorno.

- ¿El entorno de escritorio virtual puede acceder a Internet?
- ¿La solución de escritorio virtual es persistente o no persistente?

Las siguientes configuraciones se eligieron para no contrarrestar ni entrar en conflicto con ninguna configuración que tenga algo que ver con la seguridad. Estas configuraciones se eligieron para quitar las configuraciones o deshabilitar funcionalidades que podrían no ser aplicables en los entornos de escritorio virtual.

| Configuración de directiva | Elemento | Elemento secundario | Comentarios y configuración posibles |
|--------------|----|--------|----------------------------|
| Directiva de equipo local\\Configuración del equipo\\Configuración de Windows\\Configuración de seguridad | N/D | N/D | N/D |
| Directivas de Administrador de listas de redes | Propiedades de todas las redes | Ubicación de red | **El usuario no puede cambiar la ubicación** (se establece para evitar el elemento emergente del lado derecho cuando se detecta una nueva red). |
| Directiva de equipo local\\Configuración del equipo\\Plantillas administrativas\\Panel de control | N/D | N/D |
| Panel de control | Permitir sugerencias en línea | N/D  | **Deshabilitado** (la configuración no se pondrá en contacto con los servicios de contenido de Microsoft para recuperar sugerencias y contenido de ayuda). |
| Panel de control\Personalización | Aplicar una imagen predeterminada específica en las pantallas de bloqueo e inicio de sesión | N/D | Habilitado (esta configuración le permite aplicar una imagen predeterminada para las pantallas de bloqueo e inicio de sesión mediante la especificación de la ruta de acceso (ubicación) del archivo de imagen. Se usará la misma imagen para las pantallas de bloqueo e inicio de sesión. <p>La razón de esta recomendación es reducir los bytes que se transmiten a través de la red para los entornos de escritorio virtual. Esta opción de configuración se puede quitar o personalizar para cada entorno).|
|Panel de control\Configuración regional y de idioma\Personalización de escritura a mano|Desactivar el aprendizaje automático| N/D |**Habilitado** (si habilita esta configuración de directiva, el aprendizaje automático se detiene y todos los datos almacenados se eliminan. Los usuarios no pueden configurar esta configuración en el Panel de control).|
|Directiva de equipo local\\Configuración del equipo\\Plantillas administrativas\\Red|N/D|N/D|N/D|
|Servicio de transferencia inteligente en segundo plano (BITS)|Permitir el almacenamiento en caché de sistemas de mismo nivel de BITS| N/D |**Deshabilitado** (esta configuración de directiva determina si la característica de almacenamiento en caché del mismo nivel Servicio de transferencia inteligente en segundo plano (BITS) está habilitada en un equipo específico).|
|Servicio de transferencia inteligente en segundo plano (BITS)|No permitir que el cliente de BITS use Windows BranchCache|N/D|**Habilitado** (con esta configuración de directiva habilitada, el cliente de BITS no usa Windows BranchCache).<p>La razón de esta recomendación es que, a fin de que los dispositivos de escritorio virtual no se usen para el almacenamiento en caché de contenido, los dispositivos no tengan permiso para usar el ancho de banda de red para hacerlo.|
|Servicio de transferencia inteligente en segundo plano (BITS)|No permitir que el equipo actúe como un cliente de almacenamiento en caché de sistemas de mismo nivel de BITS|N/D|**Habilitado** (con esta configuración de directiva habilitada, el equipo ya no usará la característica de almacenamiento en caché del mismo nivel de BITS para descargar archivos; los archivos se descargarán solo desde el servidor de origen).|
Servicio de transferencia inteligente en segundo plano (BITS)|No permitir que el equipo actúe como un servidor de almacenamiento en caché de sistemas de mismo nivel de BITS|N/D|**Habilitado** (con esta configuración de directiva habilitada, el equipo ya no almacenará en caché los archivos descargados ni los ofrecerá a los equipos del mismo nivel).|
|BranchCache|Activar BranchCache|N/D|**Deshabilitado** (con esta selección deshabilitada, BranchCache se desactiva para todos los equipos cliente en los que se aplica la directiva).|
|*Fuentes|Habilitar Proveedores de fuentes|N/D|**Deshabilitado** (con esta opción deshabilitada, Windows no se conecta a un proveedor de fuente en línea y solo enumera las fuentes instaladas localmente).|
|Autenticación de zona con cobertura inalámbrica|Habilitar la autenticación de zona con cobertura inalámbrica| N/D |**Deshabilitado** (esta configuración de directiva define si se sondean las zonas activas WLAN para la compatibilidad con el protocolo WISPr, Wireless Internet Service Provider roaming). Con esta configuración de directiva deshabilitada, no se sondea la compatibilidad con el protocolo WISPr en las zonas activas de WLAN y los usuarios solo pueden autenticarse con zonas activas WLAN usando un explorador web).|
|Servicios de redes de igual a igual de Microsoft|Desactivar los Servicios de redes de igual a igual de Microsoft|N/D|**Habilitado** (esta opción desactiva los servicios de redes de igual a igual de Microsoft en su totalidad y hará que todas las aplicaciones dependientes dejen de funcionar. Si habilita esta opción de configuración, se desactivarán los protocolos de igual a igual).|
|Indicador de estado de conectividad de red<p>(hay otras configuraciones en esta sección que se pueden usar en redes aisladas).|Especificar sondeo pasivo|Deshabilitar el sondeo pasivo (**casilla**)|**Habilitado** (esta configuración de directiva permite especificar el comportamiento de sondeo pasivo. NCSI sondea varias medidas en toda la pila de red en un intervalo frecuente para determinar si se ha perdido la conectividad de red. Use las opciones para controlar el comportamiento de sondeo pasivo).<P>Deshabilitar el sondeo pasivo de NCI puede mejorar la carga de trabajo de la CPU en servidores u otras máquinas cuya conectividad de red es estática.|
|Archivos sin conexión|Permitir o denegar el uso de la característica Archivos sin conexión|N/D|**Deshabilitado** (esta configuración de directiva determina si la característica Archivos sin conexión está habilitada. Archivos sin conexión guarda una copia de los archivos de red en el equipo del usuario para su uso cuando el equipo no está conectado a la red. Con esta configuración de directiva deshabilitada, la característica Archivos sin conexión está deshabilitada y los usuarios no la pueden habilitar).|
|*Configuración de TCPIP\Tecnologías de transición IPv6| Establecer el estado de Teredo|Estado deshabilitado|**Habilitado** (con esta opción de configuración habilitada y establecida en "Estado deshabilitado", no hay interfaces de Teredo presentes en el host).|
*Servicio WLAN\Configuración de WLAN|Permitir que Windows se conecte automáticamente a zonas con cobertura inalámbrica sugeridas, a redes que los contactos comparten y a zonas con cobertura inalámbrica que ofrecen servicios de pago|N/D|**Deshabilitado** (esta configuración de directiva determina si los usuarios pueden habilitar la siguiente configuración de WLAN: "Conectarse a zonas con cobertura inalámbrica abiertas sugeridas", "Conectarse a las redes que mis contactos comparten" y "Habilitar servicios de pago". Con esta configuración de directiva deshabilitada, "Conectarse a zonas con cobertura inalámbrica abiertas sugeridas", "Conectarse a las redes que mis contactos comparten" y "Habilitar servicios de pago" se desactivarán y se evitará que los usuarios de este dispositivo puedan habilitarlas).|
|Servicio WWAN\Acceso a datos móviles|Permitir que las aplicaciones accedan a datos móviles|Predeterminado para todas las aplicaciones: **Forzar denegación**|**Habilitado** (si elige la opción "Forzar denegación", las aplicaciones de Windows no podrán acceder a los datos móviles y los usuarios no podrán cambiarlos).|
|Directiva de equipo local\Configuración del equipo\Plantillas administrativas\Menú Inicio y barra de tareas|N/D|N/D|
|*Notificaciones|Desactivar el uso de la red para notificaciones| N/D |**Habilitado** (con esta configuración de directiva habilitada, las aplicaciones y características del sistema no podrán recibir notificaciones de la red de WNS o mediante las API de sondeo de notificaciones).|
|Directiva de equipo local\Configuración del equipo\Plantillas administrativas\Sistema| N/D | N/D |N/D|
|Instalación de dispositivos|No enviar un informe de error de Windows cuando se instale un controlador genérico en un dispositivo| N/D |**Habilitado** (con esta configuración de directiva habilitada, no se envía un informe de errores cuando se instala un controlador genérico).|
|Instalación de dispositivos|Impedir la creación de un punto de restauración del sistema durante la actividad de los dispositivos que normalmente solicitaría la creación de un punto de restauración.| N/D |**Habilitado** (con esta configuración de directiva habilitada, Windows no crea ningún punto de restauración del sistema cuando se crearía uno normalmente).|
|Instalación de dispositivos|Impedir la recuperación de metadatos de dispositivo desde Internet| N/D |**Habilitado** (esta configuración de directiva permite impedir que Windows recupere de Internet los metadatos del dispositivo. Con esta configuración de directiva habilitada, Windows no recupera de Internet los metadatos de dispositivo de los dispositivos instalados. Esta configuración de directiva invalida la configuración del cuadro de diálogo Configuración de la instalación de dispositivos (Panel de control > Sistema y seguridad > Sistema > Configuración avanzada del sistema > pestaña Hardware).|
|Instalación de dispositivos|Desactivar los globos de "Nuevo hardware encontrado" durante la instalación de los dispositivos| N/D |**Habilitado** (esta configuración de directiva permite desactivar los globos de "Nuevo hardware encontrado" durante la instalación de los dispositivos). Con esta configuración de directiva habilitada, los globos de "Nuevo hardware encontrado" no aparecen mientras se instala un dispositivo).|
|Sistema de archivos\NTFS|Opciones de creación de nombre corto|Opciones de creación de nombre corto: Deshabilitada en todos los volúmenes|**Habilitado** (esta configuración proporciona control sobre si se deben generar o no nombres cortos durante la creación del archivo. Algunas aplicaciones requieren nombres cortos para ser compatibles, pero los nombres cortos tienen un impacto negativo en el rendimiento del sistema. Si los nombres cortos se deshabilitan en todos los volúmenes, nunca se generarán).|
|*Directiva de grupo|Continuar las experiencias en este dispositivo| N/D |**Deshabilitado** (esta configuración de directiva determina si el dispositivo Windows puede participar en experiencias entre dispositivos, es decir, experiencias continuas. La deshabilitación de esta directiva impide que este dispositivo sea detectable para otros dispositivos y, por tanto, no puede participar en experiencias entre dispositivos).|
|Administración de comunicaciones de Internet\Configuración de comunicaciones de Internet|Desactivar los vínculos "Events.asp" del Visor de eventos| N/D |**Habilitado** (esta configuración de directiva especifica si los hipervínculos "Events.asp" están disponibles para los eventos dentro de la aplicación Visor de eventos).|
|Administración de comunicaciones de Internet\Configuración de comunicaciones de Internet|Desactivar uso compartido de datos de personalización de escritura a mano| N/D |**Habilitado** (desactiva el uso compartido de datos de la herramienta de personalización del reconocimiento de escritura a mano).|
|Administración de comunicaciones de Internet\Configuración de comunicaciones de Internet|Desactivar informe de errores de reconocimiento de escritura a mano| N/D |**Habilitado** (desactiva la herramienta de informes de errores de reconocimiento de escritura a mano).|
|Administración de comunicaciones de Internet\Configuración de comunicaciones de Internet|Desactivar la búsqueda en Microsoft Knowledge Base del Centro de ayuda y soporte técnico| N/D |**Habilitado** (esta configuración de directiva especifica si los usuarios pueden realizar una búsqueda en Microsoft Knowledge Base desde el Centro de ayuda y soporte técnico).|
|Administración de comunicaciones de Internet\Configuración de comunicaciones de Internet|Desactivar al Asistente para la conexión a Internet si la conexión de direcciones URL hace referencia a Microsoft.com| N/D |**Habilitado** (esta configuración de directiva especifica si el Asistente para la conexión a Internet puede conectarse a Microsoft para descargar una lista de proveedores de servicios de Internet o ISP).|
|Administración de comunicaciones de Internet\Configuración de comunicaciones de Internet|Desactivar descargas de Internet de los asistentes para la publicación en Web y pedidos en línea| N/D |**Habilitado** (esta configuración de directiva especifica si Windows debe descargar una lista de proveedores para el Asistente para pedir copias fotográficas y la publicación web).|
|Administración de comunicaciones de Internet\Configuración de comunicaciones de Internet|Desactivar el servicio de asociaciones de archivo de Internet| N/D |**Habilitado** (esta configuración de directiva especifica si se debe usar el servicio web de Microsoft para buscar una aplicación con el fin de abrir un archivo con una asociación de archivo no controlada).|
|Administración de comunicaciones de Internet\Configuración de comunicaciones de Internet|Desactivar el registro si la conexión de direcciones URL hace referencia a Microsoft.com| N/D |**Habilitado** (esta configuración de directiva especifica si el Asistente para registro de Windows se conecta a Microsoft.com para el registro en línea).|
|Administración de comunicaciones de Internet\Configuración de comunicaciones de Internet|Desactivar la actualización de archivos de contenido del Asistente para búsqueda| N/D |**Habilitado** (esta configuración de directiva especifica si el Asistente para búsqueda debe descargar automáticamente las actualizaciones de contenido durante las búsquedas locales y en Internet).|
|Administración de comunicaciones de Internet\Configuración de comunicaciones de Internet|Desactivar la tarea de imágenes "Pedir copias fotográficas"| N/D |**Habilitado** (si habilita esta configuración de directiva, la tarea "Solicitar impresiones en línea" se quitará de las tareas de imagen de las carpetas del Explorador de archivos).|
Administración de comunicaciones de Internet\Configuración de comunicaciones de Internet|Desactivar la tarea "Publicar en Web" para archivos y carpetas| N/D |**Habilitado* (esta configuración de directiva especifica si las tareas "Publicar este archivo en Web", "Publicar esta carpeta en Web" y "Publicar los elementos seleccionados en Web" están disponibles en Tareas de archivos y carpetas en las carpetas de Windows).|
|Administración de comunicaciones de Internet\Configuración de comunicaciones de Internet|Desactivar el Programa para la mejora de la experiencia del usuario de Windows| N/D |**Habilitado** (el Programa para la mejora de la experiencia del usuario, CEIP, recopila información acerca de la configuración de hardware y el uso que se realiza del software y los servicios para identificar tendencias y patrones de uso. Si habilita esta configuración de directiva, todos los usuarios dejarán de participar en el programa CEIP de Windows).|
|Administración de comunicaciones de Internet\Configuración de comunicaciones de Internet|Desactivar el informe de errores de Windows| N/D |**Habilitado** (esta configuración de directiva controla si los errores se notifican a Microsoft. Si habilita esta configuración de directiva, los usuarios no tendrán la opción de notificar errores).|
|Administración de comunicaciones de Internet\Configuración de comunicaciones de Internet|Desactivar la búsqueda de controladores de dispositivo en Windows Update| N/D |**Habilitado** (esta configuración de directiva especifica si Windows tiene que buscar controladores de dispositivo en Windows Update cuando no existen controladores locales para un dispositivo. Si habilita esta configuración de directiva, no se realizará ninguna búsqueda en Windows Update cuando se instale un dispositivo nuevo).|
|Iniciar sesión|No mostrar la pantalla de inicio de sesión Introducción al iniciar sesión| N/D |**Habilitado** (con esta opción habilitada, la pantalla de bienvenida se oculta al usuario que inicia sesión en un dispositivo Windows).|
|Iniciar sesión|No enumerar usuarios conectados en equipos unidos al dominio| N/D |**Habilitado** (con esta opción habilitada, la interfaz de usuario de inicio de sesión no enumerará ningún usuario conectado en equipos unidos a un dominio).|
|Iniciar sesión|Enumerar usuarios locales en equipos unidos al dominio| N/D |**Deshabilitado** (con esta opción deshabilitada, la interfaz de usuario de inicio de sesión no enumerará los usuarios locales en equipos unidos a un dominio).|
|Iniciar sesión|Mostrar fondo transparente de inicio de sesión| N/D |**Habilitado** (esta configuración de directiva deshabilita el efecto de desenfoque acrílico en la imagen de fondo de inicio de sesión. Con esta opción habilitada, la imagen de fondo de inicio de sesión se muestra sin desenfoque).|
|Iniciar sesión|Mostrar animación de primer inicio de sesión| N/D |**Deshabilitado** (esta configuración de directiva le permite controlar si los usuarios verán la primera animación de inicio de sesión al iniciar sesión en el equipo por primera vez. Esto se aplica tanto al primer usuario del equipo que completa la configuración inicial como a los usuarios que se agregan al equipo posteriormente. También controla si se mostrará a los usuarios de la cuenta de Microsoft el aviso de participación de los servicios durante su primer inicio de sesión.<p>Con esta opción deshabilitada, los usuarios no verán la primera animación de inicio de sesión y los usuarios de la cuenta de Microsoft no verán el aviso de participación de los servicios).|
|Iniciar sesión|Desactivar las notificaciones de aplicaciones en la pantalla de bloqueo| N/D |**Habilitado** (esta configuración de directiva permite impedir que aparezcan notificaciones de aplicaciones en la pantalla de bloqueo. Con esta opción habilitada, no se muestran notificaciones de aplicaciones en la pantalla de bloqueo).|
|Administración de energía|Seleccionar un plan de energía activo|Plan de energía activo: Alto rendimiento|**Habilitado** (si habilita esta configuración de directiva, especifique un plan de energía de la lista Plan de energía activo). <p>Con el servicio "Energía" deshabilitado, la interfaz de usuario de Powercfg.cpl no puede mostrar estas opciones de energía y, en su lugar, devuelve un error de RPC.|
|Administración de energía\Configuración de vídeo y pantalla|Activar presentación de fondo de escritorio (conectado)| N/D |**Deshabilitado** (esta configuración de directiva le permite especificar si Windows debe habilitar la presentación de fondo del escritorio). Con esta opción deshabilitada, la presentación de fondo del escritorio está deshabilitada. Probablemente esta configuración no tendrá ningún efecto en una VM.|
|Recuperación|Permitir la restauración del sistema al estado predeterminado| N/D |**Deshabilitado** [con esta opción deshabilitada, los elementos "Usar una imagen del sistema creada previamente para recuperar el equipo" y "Reinstalar Windows" (o "Devolver el equipo al estado de fábrica") en Recuperación (en el Panel de control) no estarán disponibles].|
|*Estado de almacenamiento|Permitir la descarga de actualizaciones en el modelo de predicción de error de disco| N/D |**Deshabilitado** (las actualizaciones no se descargarán para el modelo de error de predicción de errores de disco).|
|Restaurar sistema|Desactivar Restaurar sistema| N/D |**Habilitado** (con esta opción habilitada, la opción Restaurar sistema se desactiva y no se puede acceder al Asistente para restaurar sistema. También se deshabilita la opción para configurar Restaurar sistema o crear un punto de restauración a través de Protección del sistema).|
|Solución de problemas y diagnósticos\Mantenimiento programado|Configurar el comportamiento del mantenimiento programado| N/D |**Deshabilitada** (determina si los diagnósticos programados se ejecutarán para detectar y resolver proactivamente los problemas del sistema. Con esta configuración de directiva deshabilitada, Windows no podrá detectar, solucionar ni resolver problemas de forma programada).|
|Solución de problemas y diagnósticos\Diagnósticos generados por script|Solución de problemas: permitir que los usuarios tengan acceso a asistentes de solución de problemas y que puedan ejecutarlos| N/D |**Deshabilitado** (con esta opción deshabilitada, los usuarios no pueden acceder a las herramientas de solución de problemas ni ejecutarlas desde el Panel de control).|
|Solución de problemas y diagnósticos\Diagnósticos generados por script|Solución de problemas: permitir que los usuarios tengan acceso a contenido de solución de problemas de los servidores de Microsoft desde el Panel de control de solución de problemas (a través del Solucionador de problemas en línea de Windows - Windows Online Troubleshooting Service, WOTS)| N/D |**Deshabilitado** (con esta opción deshabilitada, los usuarios solo pueden acceder al contenido de solución de problemas que esté disponible localmente en sus equipos, incluso si están conectados a Internet, y solo pueden realizar búsquedas en este. No se pueden conectar a los servidores de Microsoft que hospedan el servicio Solucionador de problemas en línea de Windows.|
|Solución de problemas y diagnósticos\Diagnósticos de rendimiento del arranque de Windows|Configurar el nivel de ejecución del escenario| N/D |**Deshabilitado** (determina el nivel de ejecución de Diagnósticos de rendimiento del arranque de Windows. Si deshabilita esta configuración de directiva, Windows no podrá detectar, solucionar ni resolver los problemas de rendimiento de arranque de Windows que controla el DPS).<p>Esta configuración puede ser muy útil durante las fases de diseño, prueba, desarrollo o mantenimiento. Esta configuración se puede habilitar en una VM o un host de sesión aislados, en medidas tomadas y resultados anotados en los registros de eventos en el origen "Microsoft-Windows-Diagnostics-Performance/Operational": Diagnostics-Performance, Categoría de la tarea "Supervisión de rendimiento del arranque".<p>**TAMBIÉN**: con el servicio DPS deshabilitado, este valor no tiene ningún efecto, ya que Windows no registrará los datos de rendimiento.|
|Solución de problemas y diagnósticos\Diagnóstico de pérdida de memoria de Windows|Configurar el nivel de ejecución del escenario| N/D |**Deshabilitado** (esta configuración de directiva determina si el servicio de directivas de diagnóstico, DPS, diagnostica problemas de pérdida de memoria. Con esta opción deshabilitada, el DPS no puede diagnosticar problemas de fuga de memoria). <p>Se pueden habilitar muchos modos de diagnóstico y usar muchas herramientas como WPT, aunque normalmente estas tareas se realizan en escenarios de desarrollo, pruebas y mantenimiento, y no se habilitan ni usan en VM o sesiones de producción.|
|Solución de problemas y diagnósticos\PerfTrack de rendimiento de Windows|Habilitar o deshabilitar PerfTrack| N/D |**Deshabilitado** (esta configuración de directiva especifica si se debe habilitar o deshabilitar el seguimiento de los eventos de capacidad de respuesta. Con esta opción deshabilitada, no se procesan los eventos de capacidad de respuesta).|
|Solución de problemas y diagnósticos\Detección y resolución de agotamiento de recursos de Windows|Configurar el nivel de ejecución del escenario| N/D |**Deshabilitado** (determina el nivel de ejecución de Detección y resolución de agotamiento de recursos de Windows. Con esta opción deshabilitada, Windows no podrá detectar, solucionar ni resolver los problemas de agotamiento de recursos de Windows administrados por el DPS).|
|Solución de problemas y diagnósticos\Diagnóstico de rendimiento de apagado de Windows|Configurar el nivel de ejecución del escenario| N/D |**Deshabilitado** (determina el nivel de ejecución de Diagnóstico de rendimiento de apagado de Windows. Con esta opción deshabilitada, Windows no podrá detectar, solucionar ni resolver los problemas de rendimiento de apagado de Windows administrados por el DPS).|
|Solución de problemas y diagnósticos\Diagnóstico de rendimiento de espera/reanudación de Windows|Configurar el nivel de ejecución del escenario| N/D |**Deshabilitado** (determina el nivel de ejecución Diagnóstico de rendimiento de espera/reanudación de Windows. Con esta opción deshabilitada, Windows no podrá detectar, solucionar problemas ni resolver ningún problema de rendimiento de espera o reanudación de Windows controlado por el DPS).|
|Solución de problemas y diagnósticos\Diagnóstico de rendimiento de respuesta del sistema de Windows|Configurar el nivel de ejecución del escenario| N/D |**Deshabilitado** (determina el nivel de ejecución de Diagnóstico de rendimiento de respuesta del sistema de Windows. Con esta opción deshabilitada, Windows no podrá detectar, solucionar ni resolver los problemas de capacidad de respuesta del sistema de Windows controlados por el DPS).|
*Perfiles de usuario|Desactivar el identificador de publicidad| N/D |**Habilitado** (con esta opción habilitada, el id. de publicidad se desactiva. Las aplicaciones no pueden usar el id. para experiencias entre aplicaciones).|
|Directiva de equipo local\Configuración del equipo\Plantillas administrativas\Componentes de Windows| N/D | N/D | N/D |
|*Privacidad de la aplicación|Permitir que las aplicaciones de Windows obtengan acceso a la información de otras aplicaciones|Predeterminado para todas las aplicaciones: Forzar denegación|**Habilitado** (con esta opción habilitada y usando la opción "Forzar denegación", las aplicaciones de Windows no pueden obtener información de diagnóstico sobre otras aplicaciones y los empleados de la organización no pueden cambiarla).|
|*Privacidad de la aplicación|Permitir que las aplicaciones de Windows accedan a la ubicación|Predeterminado para todas las aplicaciones: Forzar denegación|**Habilitado** (con esta opción habilitada y usando la opción "Forzar denegación", las aplicaciones de Windows no tienen permiso para acceder a la ubicación y los usuarios no pueden cambiar la configuración.|
|*Privacidad de la aplicación|Permitir que las aplicaciones Windows accedan al movimiento|Predeterminado para todas las aplicaciones: Forzar denegación|**Habilitado** (si esta opción de configuración está habilitada y usa la opción "Forzar denegación", las aplicaciones de Windows no podrán acceder a los datos de movimiento y los usuarios no podrán cambiar la configuración).|
|*Privacidad de la aplicación|Permitir que las aplicaciones Windows accedan a las notificaciones|Predeterminado para todas las aplicaciones: Forzar denegación|**Habilitado** (si esta opción de configuración está habilitada y usa la opción "Forzar denegación", las aplicaciones de Windows no podrán acceder a las notificaciones y los usuarios no podrán cambiar la configuración).|
|*Privacidad de la aplicación|Permitir que las aplicaciones de Windows se activen por voz|Predeterminado para todas las aplicaciones: Forzar denegación|**Habilitado** (esta configuración de directiva especifica si las aplicaciones de Windows pueden activarse por voz).|
|*Privacidad de la aplicación|Permitir que las aplicaciones de Windows se activen por voz con el sistema bloqueado|Predeterminado para todas las aplicaciones: Forzar denegación|**Habilitado** (esta configuración de directiva especifica si las aplicaciones Windows pueden activarse por voz mientras el sistema está bloqueado).|
|*Privacidad de la aplicación|Permitir que las aplicaciones Windows accedan a los controles de radio|Predeterminado para todas las aplicaciones: Forzar denegación|**Habilitado** (si elige la opción "Forzar denegación", las aplicaciones de Windows no podrán acceder al control de radios y los empleados de su organización no podrán cambiarlo).|
|Compatibilidad de aplicaciones|Desactivar el recopilador de inventario| N/D |**Habilitado** (esta configuración de directiva controla el estado del recopilador de inventario. El recopilador de inventario elabora inventarios de las aplicaciones, los archivos, los dispositivos y los controladores del sistema y los envía a Microsoft. Con esta configuración de directiva habilitada, el recopilador de inventario se desactivará y los datos no se enviarán a Microsoft. La recopilación de datos de instalación a través del Asistente para la compatibilidad de programas también se deshabilita).|
|Directivas de Reproducción automática|Establece el comportamiento predeterminado para ejecución automática.|No ejecutes ningún comando de ejecución automática|**Habilitado** (esta configuración de directiva establece el comportamiento predeterminado de los comandos de ejecución automática).|
|*Directivas de reproducción automática|Desactivar la reproducción automática|Todas las unidades|**Habilitado** (si habilita esta configuración de directiva, la opción Reproducción automática se deshabilita en todas las unidades).|
|*Contenido de la nube|No mostrar sugerencias de Windows| N/D |**Habilitado** (esta configuración de directiva evita que las sugerencias de Windows se muestren a los usuarios).|
|*Contenido de la nube|Desactivar las experiencias del consumidor de Microsoft| N/D |**Habilitado** (con esta configuración de directiva habilitada, los usuarios ya no verán las recomendaciones personalizadas de Microsoft ni las notificaciones sobre su cuenta de Microsoft).|
|*Recopilación de datos y versiones preliminares|Permitir telemetría|0: seguridad [solo Enterprise]|**Habilitado** (la configuración de un valor de 0 se aplica solo a los dispositivos que ejecutan las versiones Enterprise, Education, IoT o Windows Server, y reduce la telemetría que se envía al nivel más básico admitido).|
|Recopilación de datos y versiones preliminares|Configurar la recopilación de datos de exploración para Análisis de escritorio|Configurar la recopilación de telemetría: No permitir el envío del historial de la intranet o de Internet|**Habilitado** (puede configurar Microsoft Edge para enviar solo el historial de la intranet, solo el historial de Internet o ambos a Análisis de escritorio para dispositivos empresariales con un id. comercial configurado. Si se deshabilita o no se configura, Microsoft Edge no envía datos del historial de exploración a Análisis de escritorio).|
|*Recopilación de datos y versiones preliminares|No mostrar notificaciones de comentarios| N/D |**Habilitado** (esta configuración de directiva permite a una organización impedir que sus dispositivos muestren preguntas de comentarios de Microsoft).|
|Optimización de distribución|Modo de descarga|Modo de descarga: Simple (99)|**Habilitado** (99 = Modo de descarga simple sin emparejamiento. La Optimización de distribución se descarga solo mediante HTTP y no intenta ponerse en contacto con los servicios en la nube de la Optimización de distribución).|
|Administrador de ventanas de escritorio|No permitir animaciones de ventanas| N/D |**Habilitado** (esta configuración de directiva controla la apariencia de las animaciones de ventanas, como las que se encuentran al restaurar, minimizar y maximizar las ventanas. Con esta configuración de directiva habilitada, las animaciones de ventana se desactivan).|
|Administrador de ventanas de escritorio|Usar un color sólido para el fondo de Inicio| N/D |**Habilitado** (esta configuración de directiva controla los elementos visuales del fondo de Inicio. Con esta configuración de directiva habilitada, el fondo de Inicio usará un color sólido).|
|Interfaz del usuario perimetral|Permitir deslizamiento rápido desde borde| N/D |**Deshabilitado** (si deshabilita esta configuración de directiva, los usuarios no podrán invocar ninguna interfaz de usuario del sistema mediante el deslizamiento desde cualquier borde de la pantalla).|
|Interfaz del usuario perimetral|Deshabilitar las sugerencias de ayuda| N/D |**Habilitado** (si esta opción está habilitada, Windows no mostrará ninguna sugerencia de ayuda al usuario).|
|Explorador de archivos|No mostrar la notificación 'nueva aplicación instalada'| N/D |**Habilitado** (esta directiva quita la notificación del usuario final para las asociaciones de aplicaciones nuevas. Estas asociaciones se basan en tipos de archivo, como TXT, o en protocolos, como HTTP. Si esta directiva está habilitada, no se mostrará ninguna notificación al usuario final).|
|Historial de archivos|Desactivar historial de archivos| N/D |**Habilitado** (con esta configuración de directiva habilitada, no se puede activar el historial de archivos para crear copias de seguridad periódicas y automáticas).|
|*Encontrar mi dispositivo|Activar o desactivar Encontrar mi dispositivo| N/D |**Deshabilitado** (cuando la opción Encontrar mi dispositivo está desactivada, el dispositivo y su ubicación no están registrados y la característica no funcionará). El usuario tampoco podrá ver la ubicación del último uso de su digitalizador activo en el dispositivo).|
|Grupo en el hogar|Impedir que el equipo se una a un grupo en el hogar| N/D |**Habilitado** (si habilita esta configuración de directiva, los usuarios no pueden agregar equipos a un grupo en el hogar. Esta configuración de directiva no afecta a otras características de uso compartido de red).|
|*Internet Explorer|Permitir que los servicios Microsoft proporcionen sugerencias mejoradas mientras el usuario escribe en la barra de direcciones| N/D |**Deshabilitado** (los usuarios reciben sugerencias mejoradas mientras escriben en la barra de direcciones. Además, los usuarios no podrán cambiar la configuración de Sugerencias).|
|Internet Explorer|Deshabilitar la comprobación periódica de actualizaciones de software de Internet Explorer| N/D |**Habilitado** (impide que Internet Explorer compruebe si existe una nueva versión del explorador disponible).|
|Internet Explorer|Deshabilitar pantalla de presentación| N/D |**Habilitado** (impide que aparezca la pantalla de presentación de Internet Explorer cuando los usuarios inician el explorador).|
|Internet Explorer|Instalar nuevas versiones de Internet Explorer automáticamente| N/D |**Deshabilitado** (esta configuración de directiva configura Internet Explorer para que instale automáticamente nuevas versiones de Internet Explorer cuando estén disponibles).|
|Internet Explorer|Impedir la participación en el Programa para la mejora de la experiencia del usuario| N/D |**Habilitado** (esta configuración de directiva impide que el usuario participe en el Programa para la mejora de la experiencia del usuario o CEIP).|
|Internet Explorer|Evitar la ejecución del Asistente de primera ejecución|Ve directamente a la página principal|**Habilitado** (esta configuración de directiva evita que Internet Explorer ejecute el primer Asistente de ejecución la primera vez que un usuario inicia el explorador después de instalar Internet Explorer o Windows).|
|Internet Explorer|Establecer el crecimiento del proceso de pestañas|Baja|**Habilitado** (esta configuración de directiva le permite establecer la velocidad a la que Internet Explorer crea nuevos procesos de pestañas).|
|Internet Explorer|Desactivar las notificaciones de rendimiento de complementos| N/D |**Habilitado** (esta configuración de directiva impide que Internet Explorer muestre una notificación cuando el tiempo medio para cargar todos los complementos habilitados del usuario supera el umbral).|
|Internet Explorer|Desactivar la recuperación automática tras bloqueo| N/D |**Habilitado** (esta configuración de directiva desactiva la recuperación automática tras bloqueo. Con esta configuración de directiva habilitada, la recuperación automática tras bloqueo no solicita al usuario que recupere sus datos cuando un programa deja de responder).|
|*Internet Explorer|Desactivar geoubicación del explorador| N/D |**Habilitado** (con esta configuración de directiva habilitada, la compatibilidad de la geolocalización del navegador se desactiva).|
|*Internet Explorer|Activar los sitios sugeridos| N/D |**Deshabilitado** (con esta configuración de directiva deshabilitada, los puntos de entrada y la funcionalidad asociada con esta característica se desactivarán).|
|Internet Explorer\Panel de control de Internet\Página de opciones avanzadas|Reproducir animaciones en páginas web| N/D |**Deshabilitado** (esta configuración de directiva permite administrar si Internet Explorer mostrará las imágenes animadas que encuentre en el contenido web. Por lo general, solo los archivos GIF animados se ven afectados por esta configuración. No afecta al contenido web activo, como los applets Java).|
|Internet Explorer\Panel de control de Internet\Página de opciones avanzadas|Reproducir sonidos en páginas web| N/D |**Deshabilitado** (con esta configuración de directiva deshabilitada, Internet Explorer no reproducirá ni descargará sonidos del contenido web, lo que ayudará a las páginas a mostrarse con más rapidez).|
|Internet Explorer\Panel de control de Internet\Página de opciones avanzadas|Reproducir vídeos en páginas web| N/D |**Deshabilitado** (si deshabilita esta configuración de directiva, Internet Explorer no reproducirá ni descargará vídeos, lo que ayudará a las páginas a mostrarse con más rapidez).|
|Internet Explorer\Panel de control de Internet\Página de opciones avanzadas|Desactivar la carga de sitios web y contenido en segundo plano para optimizar el rendimiento| N/D |**Habilitado** (con esta configuración de directiva habilitada, Internet Explorer no carga los sitios web ni el contenido en segundo plano).|
|*Internet Explorer\Panel de control de Internet\Página de opciones avanzadas|Desactivar las características correspondientes a Pasar de página con predicción de página| N/D |**Habilitado** (Microsoft recopila su historial de exploración para mejorar el funcionamiento de Pasar de página con predicción de página. Si habilitas esta configuración de directiva, se desactiva Pasar de página con predicción de página y la siguiente página web no se carga en segundo plano).|
|Internet Explorer\Configuración de Internet\Configuración avanzada\Exploración|Desactivar detección de número de teléfono| N/D |**Habilitado** (esta configuración de directiva determina si los números de teléfono se reconocen y se convierten en hipervínculos, lo que se puede usar para invocar la aplicación telefónica predeterminada en el sistema. Si deshabilitas esta configuración de directiva, se activa la detección de número de teléfono. Los usuarios no podrán modificar esta configuración).|
|Internet Information Services|Evitar instalación de IIS| N/D |**Habilitado** (con esta configuración de directiva habilitada, IIS no se puede instalar. No podrá instalar componentes ni aplicaciones de Windows que requieran IIS).|
|*Ubicación y sensores|Desactivar la ubicación| N/D |**Habilitado** (con esta opción de configuración habilitada, la característica de ubicación se desactiva y se impide a todos los programas del equipo usar la información de ubicación de la característica de ubicación).|
|Ubicación y sensores|Desactivar sensores| N/D |**Habilitado** (esta configuración de directiva desactiva la característica de sensor para este dispositivo. Con esta configuración de directiva habilitada, la característica de sensor está desactivada y ninguno de los programas de este equipo puede usar la característica de sensor).|
|Ubicaciones y sensores / Servicio de localización de Windows|Desactivar el Servicio de localización de Windows| N/D |**Habilitado** (esta configuración de directiva desactiva la característica de proveedor de ubicación de Windows para este dispositivo).|
|*Mapas|Desactivar la descarga y actualización automáticas de los datos de mapa| N/D |**Habilitado** (con esta opción de configuración habilitada, la descarga y actualización automáticas de los datos del mapa se desactivan).|
|*Mapas|Desactivar el tráfico de red no solicitado en la página Configuración de mapas sin conexión| N/D |**Habilitado** (con esta opción de configuración habilitada, las características que generan tráfico de red en la página de configuración de Mapas sin conexión se desactivan. Nota: Esto podría desactivar toda la página de configuración).|
|*Mensajes|Permitir la sincronización en la nube del servicio de mensajes| N/D |**Deshabilitado** (esta configuración de directiva permite realizar copias de seguridad y restaurar mensajes de texto de móvil en los servicios en la nube de Microsoft).|
|*Microsoft Edge|Permitir actualizaciones de configuración para la biblioteca de libros| N/D |**Deshabilitado** (con esta opción de configuración deshabilitada, Microsoft Edge no descargará automáticamente los datos de configuración actualizados de la biblioteca de libros).|
|*Microsoft Edge|Permitir telemetría extendida para la pestaña Libros| N/D |**Deshabilitado** (con esta opción de configuración deshabilitada, Microsoft Edge solo envía datos de telemetría básicos, en función de la configuración del dispositivo).|
|Microsoft Edge|Permita que Microsoft Edge se inicie previamente al inicio de Windows, cuando el sistema está inactivo y cada vez que se cierra Microsoft Edge.|Configurar el inicio previo: impedir el inicio previo|**Habilitado** (con esta opción habilitada y configurada para evitar el inicio previo, Microsoft Edge no se iniciará previamente durante el inicio de sesión en Windows, cuando el sistema esté inactivo o cada vez que se cierre Microsoft Edge).|
|Microsoft Edge|Permita que Microsoft Edge se inicie y cargue las páginas de inicio y Nueva pestaña al iniciar Windows y cada vez que se cierra Microsoft Edge.|Configurar la precarga de pestañas: Impedir la precarga de pestañas|**Habilitado** (esta configuración de directiva permite decidir si Microsoft Edge puede cargar las páginas de inicio y Nueva pestaña durante el inicio de sesión en Windows y cada vez que se cierra Microsoft Edge. De manera predeterminada, esta configuración permite la precarga. Con la precarga deshabilitada, Microsoft Edge no cargará las páginas de inicio y Nueva pestaña durante el inicio de sesión de Windows y cada vez que se cierre Microsoft Edge).|
|Microsoft Edge|Permitir contenido web en la página Nueva pestaña| N/D |**Deshabilitado** (con esta opción de configuración deshabilitada, Microsoft Edge abre una nueva pestaña con una página en blanco. Si esta opción está configurada, los usuarios no pueden cambiar la configuración).|
|*Microsoft Edge|Evitar que la página web de primera ejecución se abra en Microsoft Edge| N/D |**Habilitado** (los usuarios no verán la página de primera ejecución al abrir Microsoft Edge por primera vez).|
|OneDrive|Impedir que OneDrive genere tráfico de red hasta que el usuario inicie sesión en OneDrive| N/D |**Habilitado** (habilite esta configuración para evitar que el cliente de sincronización de OneDrive [OneDrive.exe] genere tráfico en la red [comprobación de actualizaciones, etc.] hasta que el usuario inicie sesión en OneDrive o comience a sincronizar archivos en el equipo local).|
|Asistencia en línea|Desactivar la ayuda activa| N/D |**Habilitado** (con esta configuración de directiva habilitada, los vínculos de contenido activo no se representan. Se muestra el texto, pero no hay ningún vínculo en el que se pueda hacer clic para estos elementos).|
|OOBE|No iniciar la experiencia de configuración de privacidad en el inicio de sesión de usuario| N/D |**Habilitado** (al iniciar sesión en una nueva cuenta de usuario por primera vez o después de una actualización en algunos escenarios, es posible que se muestre una pantalla o una serie de pantallas que pida al usuario que elija la configuración de privacidad para su cuenta. Habilite esta directiva para impedir que se inicie esta experiencia).|
|Fuentes RSS|Impedir detección automática de fuentes y Web Slices| N/D |**Habilitado** (esta configuración de directiva impide que los usuarios configuren Internet Explorer para que detecte automáticamente si hay fuentes o Web Slices disponibles para una página web asociada).|
|*Fuentes RSS|Desactivar la sincronización en segundo plano de fuentes y Web Slices| N/D |**Habilitado** (con esta configuración de directiva habilitada, la capacidad de sincronizar fuentes y Web Slices en segundo plano se desactiva).|
|*Buscar|Permitir Cortana| N/D |**Deshabilitado** (esta configuración de directiva especifica si se permite Cortana en el dispositivo. Si Cortana está desactivada, los usuarios aún pueden usar la opción de búsqueda para encontrar cosas en el dispositivo).|
|Buscar|Permitir usar Cortana sobre la pantalla de bloqueo| N/D |**Deshabilitado** (esta configuración de directiva determina si el usuario puede interactuar con Cortana mediante voz cuando el sistema está bloqueado).|
|*Buscar|Permitir el uso de la ubicación para las búsquedas y Cortana| N/D |**Deshabilitado** (esta configuración de directiva especifica si la búsqueda y Cortana pueden proporcionar resultados de búsqueda y de Cortana con reconocimiento de ubicación).|
|Search|Controlar vistas preliminares de texto enriquecido para los datos adjuntos|Controlar vistas preliminares de texto enriquecido para los datos adjuntos: **.docx; .xlsx; .txt; .xls**|**Habilitado** (al habilitar esta directiva, se define una lista delimitada por puntos y coma de extensiones de archivo, a las que se les permite obtener vistas previas de datos adjuntos).<p>**NOTA**: Esta configuración se puede usar para limitar los tipos de datos adjuntos de los que se va a obtener una vista previa, lo que también puede ayudar a evitar la previsualización automática de algunos tipos de contenido potencialmente peligrosos.|
|Buscar|No permitir búsquedas en la Web| N/D |**Habilitado** (al habilitar esta directiva, se quita la opción de buscar en la web desde Windows Desktop Search).|
|*Buscar|No buscar en Internet o mostrar resultados de Internet en Search| N/D |**Habilitado** (con esta configuración de directiva habilitada, las consultas no se realizarán en la web y los resultados web no se mostrarán cuando un usuario realice una consulta con la opción Buscar).|
|Search|Habilitar la indización de carpetas de Exchange no almacenadas en caché| N/D |**Deshabilitado** (la habilitación de esta directiva permite la indexación de elementos de correo en un servidor de Microsoft Exchange cuando Microsoft Outlook no se ejecuta en modo de almacenamiento en caché. El comportamiento predeterminado de la búsqueda es no indexar las carpetas de Exchange no almacenadas en caché. Si deshabilita esta directiva, se bloqueará cualquier indexación de carpetas de Exchange no almacenadas en caché).|
|Buscar|Impedir la indización de archivos en la memoria caché de archivos sin conexión| N/D |**Habilitado** (si esta opción está habilitada, los archivos en recursos compartidos de red que estén disponibles sin conexión no se indexan. En caso contrario, se indexan. Está deshabilitada de forma predeterminada).|
|*Buscar|Definir qué información se comparte en las búsquedas|Información anónima|**Habilitado** (Información anónima: comparta la información de uso, pero no el historial de búsqueda, la información de la cuenta de Microsoft ni la ubicación específica).|
|Search|Detener la indización en el espacio limitado del disco duro|Límite de MB: **5000**|**Habilitado** (la habilitación de esta directiva impide que la indexación continúe cuando queda menos espacio del especificado en el disco duro en la misma unidad que la ubicación del índice. Seleccione entre 0 y 2147483647 MB).|
|Plataforma de protección de software|Desactivar validación AVS en línea del cliente KMS| N/D |**Habilitado** (con esta opción habilitada, el dispositivo no envía datos a Microsoft en relación con su estado de activación).|
|*Voz|Permitir la actualización automática de los datos de voz| N/D |**Deshabilitado** (especifica si el dispositivo recibirá actualizaciones en los modelos de reconocimiento de voz y síntesis de voz).|
|Tienda|Desactivar la oferta para actualizar a la versión de Windows más reciente| N/D |**Habilitado** (habilita o deshabilita la oferta de Store para actualizar a la versión más reciente de Windows. Si habilita esta opción, la aplicación Store no proporcionará actualizaciones a la versión más reciente de Windows).|
|Entrada de texto|Mejorar el reconocimiento de la entrada manuscrita y escritura por teclado| N/D |**Deshabilitado** (esta configuración de directiva controla la capacidad de enviar datos de entrada manuscrita y escritura a Microsoft para mejorar las funcionalidades de reconocimiento y sugerencia de idioma de las aplicaciones y los servicios que se ejecutan en Windows).|
|Informe de errores de Windows|Deshabilitar Informe de errores de Windows| N/D |**Habilitado** (con esta configuración de directiva habilitada, Informe de errores de Windows no envía información de problemas a Microsoft. Además, la información de la solución no está disponible en Seguridad y mantenimiento en el Panel de control).|
|Grabación y difusión de juegos de Windows|Habilita o deshabilita la opción de grabación y difusión de juegos de Windows.| N/D |**Deshabilitado** (con esta opción deshabilitada, no se permitirá la grabación de juegos de Windows).|
|Área de trabajo de Windows Ink|Permitir el Área de trabajo de Windows Ink|Elija una de las siguientes acciones: Deshabilitada|**Habilitado** (con esta opción habilitada y la opción secundaria deshabilitada, la funcionalidad Área de trabajo de Windows Ink no está disponible).|
|Windows Installer|Controlar el tamaño máximo de la memoria caché de los archivos de línea base|5|**Habilitado** (esta directiva controla el porcentaje de espacio en disco disponible para la memoria caché de archivos de línea de base de Windows Installer. Con esta configuración de directiva habilitada, puede modificar el tamaño máximo de la memoria caché de archivos de línea de base de Windows Installer).|
|Windows Installer|Desactivar la creación de puntos de control de Restaurar sistema| N/D |**Habilitad** (con esta configuración de directiva habilitada, Windows Installer no genera puntos de control de restauración del sistema al instalar aplicaciones).|
|Centro de movilidad de Windows|Desactivar Centro de movilidad de Windows| N/D |**Habilitado** (con esta configuración de directiva habilitada, el usuario no puede invocar el Centro de movilidad de Windows. La interfaz de usuario del Centro de movilidad de Windows se quita de todos los puntos de entrada del shell y el archivo .exe no la inicia).|
|Análisis de confiabilidad de Windows|Configurar proveedores de WMI de confiabilidad| N/D |**Deshabilitado** (con esta configuración de directiva deshabilitada, el Monitor de confiabilidad no mostrará la información de confiabilidad del sistema y las aplicaciones compatibles con WMI no podrán acceder a la información de confiabilidad de los proveedores de la lista).|
|Seguridad de Windows\Notificaciones|Ocultar las notificaciones no críticas| N/D |**Habilitado** (con esta opción habilitada, los usuarios locales solo verán notificaciones críticas de Seguridad de Windows. No verán otros tipos de notificaciones, como información normal sobre el estado de los equipos o dispositivos).|
|Windows Update|Activar notificaciones de software| N/D |**Deshabilitado** (esta configuración de directiva permite controlar si los usuarios verán mensajes de notificación mejorados y detallados del servicio Microsoft Update acerca del software destacado. Los mensajes de notificación mejorados destacan el valor y promueven la instalación y el uso de software opcional. Esta configuración de directiva está destinada a usarse en entornos poco administrados en los que se permite que el usuario final acceda al servicio Microsoft Update).|
|*Windows Update\Windows Update para empresas|Administrar las versiones preliminares|Establecer el comportamiento para recibir versiones preliminares: **Deshabilitar las versiones preliminares**|**Habilitado** (al seleccionar "Deshabilitar las versiones preliminares" evitará que las versiones preliminares se instalen en el dispositivo. Esto evitará que los usuarios se inscriban en el Programa Windows Insider, a través de la opción Configuración > Actualización y seguridad).|
|*Windows Update\Windows Update para empresas|Seleccionar cuándo se reciben las versiones preliminares y las actualizaciones de características|Seleccione el nivel de preparación de Windows para las actualizaciones que quiere recibir:<p>**Canal semianual**<p>Después del lanzamiento de una versión preliminar o una actualización de características, aplazar su recepción la cantidad de días siguiente: **365**<p>Pausar las versiones preliminares o las actualizaciones de características a partir de: **aaaa-mm-dd**|**Habilitado** (habilite esta directiva para especificar el nivel de versión preliminar o actualizaciones de características que quiere recibir y cuándo). Canal semianual: reciba actualizaciones de características cuando se lancen para el público general.<p> Si selecciona el canal semianual:<p>- Puede aplazar la recepción de actualizaciones de características hasta un máximo de 365 días.<p>- Para impedir la recepción de actualizaciones de características a la hora programada, puede pausarlas temporalmente. La pausa permanecerá en vigor durante 35 días a partir de la hora de inicio proporcionada.<p> - Para reanudar la recepción de actualizaciones de características que están en pausa, desactive el campo fecha de inicio).|
|Windows Update\Windows Update para empresas|Selecciona cuándo quieres recibir actualizaciones de calidad|Después del lanzamiento de una actualización de calidad, aplazar su recepción durante la siguiente cantidad de días: **30**<p>Pausar las actualizaciones de calidad a partir de: aaaa-mm-dd|**Habilitado** (habilite esta directiva para especificar cuándo se deben recibir las actualizaciones de calidad.<p>Puede aplazar la recepción de actualizaciones de calidad hasta 30 días.<p>Para evitar recibir actualizaciones de calidad a la hora programada, puede pausarlas temporalmente. La pausa permanecerá en vigor durante 35 días o hasta que desactive el campo fecha de inicio.<p>Para reanudar la recepción de actualizaciones de calidad que están en pausa, desactive el campo fecha de inicio).<p>Esta recomendación tiene el objetivo de ayudarle a controlar cuándo se aplican las actualizaciones y garantizar que estas no se ofrezcan ni instalen de forma inesperada.|
|Directiva de equipo local\Configuración de usuario\Plantillas administrativas| N/D | N/D | N/D |
|Panel de control\Configuración regional y de idioma|Desactivar la oferta de predicciones de texto al escribir| N/D |**Habilitado** (esta directiva desactiva la opción de ofrecer predicciones de texto mientras se escribe. Sin embargo, esto no impide que el usuario o una aplicación cambien la configuración mediante programación. Con esta configuración de directiva habilitada, la opción se bloqueará para no ofrecer predicciones de texto).|
|Escritorio|No agregar recursos compartidos de documentos abiertos recientemente a Ubicaciones de red| N/D |**Habilitado** (con esta opción habilitada, las carpetas compartidas no se agregan automáticamente a las ubicaciones de red al abrir un documento en la carpeta compartida).|
|Escritorio|Desactivar el gesto del mouse de minimización de ventanas de Aero Shake| N/D |**Habilitado** (impide que las ventanas se minimice o se restauren cuando la ventana activa se mueve hacia atrás y hacia delante con el mouse. Con esta directiva habilitada, las ventanas de la aplicación no se minimizarán ni restaurarán cuando la ventana activa se mueva hacia atrás y hacia delante con el mouse).|
|Escritorio / Active Directory|Tamaño máximo de las búsquedas en Active Directory|Número de objetos devueltos: **1500**|**Habilitado** (especifica el número máximo de objetos que el sistema muestra en respuesta a un comando para la exploración o la búsqueda en Active Directory. Esta configuración afecta a todas las pantallas de exploración asociadas a Active Directory, como las de Usuarios y grupos locales, Usuarios y equipos de Active Directory, y los cuadros de diálogo que se usan para establecer permisos para objetos de usuario o grupo en Active Directory).|
|Menú Inicio y barra de tareas|No mostrar elementos, ni realizar seguimiento de los mismos, en las Jump Lists desde ubicaciones remotas| N/D |**Habilitado** (esta configuración de directiva permite controlar la visualización o el seguimiento de elementos en las listas de accesos directos de ubicaciones remotas).|
|Menú Inicio y barra de tareas|No buscar en Internet| N/D |**Habilitado** (con esta configuración de directiva habilitada, el cuadro de búsqueda del menú Inicio no buscará en el historial de Internet ni en los favoritos).|
|Menú Inicio y barra de tareas|No uses el método basado en la búsqueda al resolver accesos directos de shell| N/D |**Habilitado** (esta configuración de directiva impide que el sistema realice una búsqueda completa de la unidad de destino para resolver un acceso directo).|
|Menú Inicio y barra de tareas|Desactivar todos los globos de notificaciones| N/D |**Habilitado** (con esta configuración de directiva habilitada, no se muestra al usuario ningún globo de notificación).
|Menú Inicio y barra de tareas|Desactivar globos de anuncios de características| N/D |**Habilitado** (con esta configuración de directiva habilitada, no se muestran algunos globos de notificaciones marcados como anuncios de características).|
|Menú Inicio y barra de tareas|Desactivar seguimiento del usuario| N/D |**Habilitado** (con esta configuración de directiva habilitada, el sistema no realiza un seguimiento de los programas que ejecuta el usuario y no muestra los programas que se usan con frecuencia en el menú Inicio).|
|Menú Inicio y la barra de tareas / Notificaciones|Desactivar las notificaciones del sistema| N/D |**Habilitado** (con esta configuración de directiva habilitada, las aplicaciones no podrán generar notificaciones del sistema).|
|*Menú Inicio y barra de tareas\Notificaciones|Desactivar las notificaciones del sistema en la pantalla de bloqueo| N/D |**Habilitado** (con esta configuración de directiva habilitada, las aplicaciones no podrán generar notificaciones del sistema en la pantalla de bloqueo).|
|Directiva de equipo local\Configuración de usuario| N/D | N/D | N/D |
|Componentes de Windows / Contenido en la nube|Configurar Contenido destacado de Windows en la pantalla de bloqueo| N/D |**Deshabilitado** (con esta directiva deshabilitada, se desactivará el contenido destacado de Windows y los usuarios ya no podrán seleccionarlo como pantalla de bloqueo. Los usuarios verán la imagen de la pantalla de bloqueo predeterminada y podrán seleccionar otra imagen, a menos que haya habilitado la directiva "Evitar el cambio de la imagen de la pantalla de bloqueo".|
|*Componentes de Windows/Contenido de la nube|No sugerir contenido de terceros en el contenido destacado de Windows| N/D |**Habilitado** (con esta directiva habilitada, las características destacadas de Windows, como los elementos destacados de la pantalla de bloqueo, las aplicaciones sugeridas en el menú Inicio o las sugerencias de Windows ya no sugerirán aplicaciones y contenido de editores de software de terceros. Los usuarios seguirán viendo sugerencias y consejos para mejorar su productividad con las características y aplicaciones de Microsoft).|
|Componentes de Windows / Contenido en la nube|No usar los datos de diagnóstico para las experiencias personalizadas| N/D |**Habilitado** (con esta configuración de directiva habilitada, Windows no usará los datos de diagnóstico de este dispositivo (estos datos pueden incluir el uso del explorador, las aplicaciones y las características, según el valor de configuración de "datos de diagnóstico") para personalizar el contenido que se muestra en la pantalla de bloqueo, las sugerencias de Windows, las características del consumidor de Microsoft y otras características relacionadas).|
|Componentes de Windows / Contenido en la nube|Desactivar todas las características de Contenido destacado de Windows| N/D |**Habilitado** (contenido destacado de Windows en la pantalla de bloqueo, sugerencias de Windows, características de consumidor de Microsoft y otras características relacionadas). Debe habilitar esta configuración de directiva si su objetivo es minimizar el tráfico de red de los dispositivos de destino).|
|Interfaz del usuario perimetral|Desactivar el seguimiento del uso de la aplicación| N/D |**Habilitado** (esta configuración de directiva impide que Windows realice un seguimiento de las aplicaciones que se usan y se buscan con más frecuencia. Si habilita esta configuración de directiva, las aplicaciones se ordenarán alfabéticamente en:<p> - los resultados de la búsqueda<p> - los paneles Buscar y Compartir<p> - la lista desplegable de aplicaciones del selector)|
|Explorador de archivos|Desactivar el almacenamiento en caché de imágenes en miniatura| N/D |**Habilitado** (con esta configuración de directiva habilitada, las vistas en miniatura no se almacenan en la caché).|
|Explorador de archivos|Desactivar animaciones de ventana y control comunes| N/D |**Habilitado** (la deshabilitación de las animaciones puede mejorar la facilidad de uso para usuarios con algunas discapacidades visuales, así como mejorar el rendimiento y la duración de la batería en algunos escenarios).|
|Explorador de archivos|Desactivar la visualización de las entradas de búsqueda recientes en el cuadro de búsqueda del Explorador de archivos| N/D |**Habilitado** (deshabilita la sugerencia de consultas recientes para el cuadro de búsqueda y evita que las entradas del cuadro de búsqueda se almacenen en el registro para futuras referencias).|
|Explorador de archivos|Desactivar almacenamiento en caché de vistas en miniatura en archivos thumbs.db ocultos| N/D |**Habilitado** (con esta configuración de directiva habilitada, el Explorador de archivos no crea, lee ni escribe archivos thumbs.db).|

\* procede de la [Línea base de funcionalidad limitada de tráfico restringido de Windows](https://go.microsoft.com/fwlink/?linkid=828887).

### <a name="system-services"></a>Servicios del sistema

Si está pensando en deshabilitar los servicios del sistema para conservar recursos, asegúrese de que el servicio no es un componente de otro servicio. En este documento y con los scripts de GitHub disponibles, algunos servicios no están en la lista porque no se pueden deshabilitar de una manera compatible.

La mayoría de estas recomendaciones copian las recomendaciones para Windows Server 2016 instalado con Experiencia de escritorio, según las instrucciones de la [Guía para deshabilitar los servicios del sistema en Windows Server 2016 con Experiencia de escritorio](../../security/windows-services/security-guidelines-for-disabling-system-services-in-windows-server.md).

Muchos de los servicios que pueden parecer buenos candidatos para la deshabilitación están establecidos en el tipo de inicio de servicio manual. Esto significa que el servicio no se iniciará automáticamente. No se iniciará a menos que un proceso o un evento desencadene una solicitud del servicio que se prevé deshabilitar. Los servicios que ya están configurados para iniciar el manual de tipo, no se enumeran aquí por lo general.

> [!NOTE]
> Puedes enumerar los servicios en ejecución con este código de ejemplo de PowerShell y que se muestre solo el nombre corto del servicio:
>
> ```powershell
> Get-Service | Where-Object {$_.Status -eq 'Running'} | Select-Object -ExpandProperty Name
> ```

La tabla siguiente contiene algunos servicios que se pueden considerar para su deshabilitación en entornos de escritorio virtual:

| Servicio de Windows | Nombre del servicio | Elemento | Comentario|
| --------------- | ------------ | ---- | ------ |
|Cellular Time|autotimesvc|Este servicio establece la hora en función de los mensajes NITZ de una red móvil.|Es posible que los entornos de escritorio virtual no tengan dispositivos de este tipo disponibles. <p>Para obtener más información, consulte [este artículo](/windows-hardware/drivers/network/mb-nitz-support). |
|Servicio de usuario de GameDVR y difusión|BcastDVRUserService|Este servicio (por usuario) se usa en grabaciones de juegos y difusiones en directo|NOTA: Este es un "servicio por usuario" y, como tal, el servicio de plantillas debe estar deshabilitado. Este servicio de usuario se usa en grabaciones de juegos y difusiones en directo.<p>Para obtener más información, consulte [este artículo](/windows-hardware/drivers/network/mb-nitz-support). |
|CaptureService|CaptureService|Habilita la funcionalidad de captura de pantalla opcional para las aplicaciones que llaman a la API Windows.Graphics.Capture.|Servicio de captura OneCore: habilita la funcionalidad de captura de pantalla opcional para las aplicaciones que llaman a la API Windows.Graphics.Capture.<p> Para obtener más información, consulta [este artículo](/uwp/api/windows.graphics.capture?view=winrt-19041&preserve-view=true).|
|Servicio de plataforma de dispositivos conectados|CDPSvc|Este servicio se usa en los escenarios de la plataforma de dispositivos conectados.|Servicio de plataforma de dispositivos conectados <p> Para obtener más información, consulte [este artículo](/openspecs/windows_protocols/ms-cdp/929c2238-6d49-4ba4-a36a-37e732c4f736).|
|Servicio de usuario de CDP|CDPUserSvc| N/D |Servicio de usuario de plataforma de dispositivos conectados. Para obtener más información, consulte [este artículo](/openspecs/windows_protocols/ms-cdp/f5a15c56-ac3a-48f9-8c51-07b2eadbe9b4).<p> Este servicio de usuario se usa en los escenarios de la plataforma de dispositivos conectados. <br><br>Este es un "servicio por usuario" y, como tal, el servicio de plantillas debe estar deshabilitado (CDPUserSvc).|
|Optimizar unidades|defragsvc|Permite que el equipo funcione de forma más eficiente al optimizar los archivos en las unidades de almacenamiento.|Las soluciones de escritorio virtual normalmente no se benefician de la optimización del disco. Estas "unidades" no suelen ser unidades tradicionales y, a menudo, son solo una asignación de almacenamiento temporal.|
|Servicio de ejecución de diagnósticos|DiagSvc|Ejecuta acciones de diagnóstico para el soporte técnico de solución de problemas.|Al deshabilitar este servicio, se deshabilita la capacidad de ejecutar el servicio de ejecución de diagnósticos de Diagnósticos de Windows.|
|Telemetría y experiencias del usuario conectado|DiagTrack|Este servicio habilita características que admiten experiencias de usuario conectadas y en la aplicación. Asimismo, este servicio administra la recopilación y transmisión de la información de diagnóstico y uso basada en los eventos (se usa para mejorar la experiencia y la calidad de la plataforma de Windows) cuando la configuración de opciones de diagnóstico y privacidad de uso está habilitada en Comentarios y diagnósticos.|Puedes deshabilitar esta opción si la red está desconectada. Para obtener más información, consulte [este artículo](/windows/privacy/configure-windows-diagnostic-data-in-your-organization).|
|Servicio de directivas de diagnóstico|DPS|El servicio de directivas de diagnóstico permite la detección, solución y resolución de problemas para componentes de Windows. Si este servicio se detiene, los diagnósticos ya no funcionarán.|Al deshabilitar este servicio, se deshabilita la capacidad de ejecutar diagnósticos de Windows. Para obtener más información, consulta [este artículo](/uwp/api/Windows.System.Diagnostics?view=winrt-19041&preserve-view=true).|
|Administrador de configuración del dispositivo|DsmSvc|Habilita la detección, descarga e instalación de software relacionado con el dispositivo. |Si se deshabilita este servicio, es posible que los dispositivos se configuren con software obsoleto y no funcionen correctamente. <p>Los entornos de escritorio virtual controlan muy estrechamente el software que se instala y mantienen esa coherencia en todo el entorno.|
|Servicio de uso de datos|DusmSvc|Uso de datos de red, límite de datos, restricción de datos en segundo plano, redes de uso medido.| Para obtener más información, consulta [este artículo](/uwp/schemas/mobilebroadbandschema/dusm/schema-root). |
|Servicio de zona con cobertura inalámbrica móvil de Windows|icssvc|Proporciona la capacidad de compartir una conexión de datos móviles con otro dispositivo.|Para obtener más información, consulte [este artículo](/uwp/api/Windows.Networking.NetworkOperators.NetworkOperatorTetheringAccessPointConfiguration?view=winrt-19041&preserve-view=true).|
|Servicio de instalación de la Tienda de Microsoft|InstallService|Proporciona compatibilidad de infraestructura con Microsoft Store. |Este servicio se inicia a petición y, si se deshabilita, las instalaciones no funcionarán correctamente.<p>Considere la posibilidad de deshabilitar este servicio en un escritorio virtual no persistente y dejarlo tal cual para las soluciones de escritorio virtual persistentes.|
|Servicio de geolocalización|Lfsvc|Supervisa la ubicación actual del sistema y administra las geovallas (una ubicación geográfica con eventos asociados). |Si desactivas este servicio, las aplicaciones no podrán usar ni recibir notificaciones de geolocalización o geovallas. Para obtener más información, consulte [este artículo](/uwp/api/Windows.Devices.Geolocation?view=winrt-19041&preserve-view=true).|
|Administrador de mapas descargados|MapsBroker|Servicio de Windows para que las aplicaciones obtengan acceso a mapas descargados. Este servicio lo pide la aplicación que accederá a los mapas descargados.|Si deshabilitas este servicio las aplicaciones no obtendrán acceso a los mapas. Para obtener más información, consulte [este artículo](/uwp/api/Windows.Services.Maps?view=winrt-19041&preserve-view=true).|
|MessagingService|MessagingService|Servicio de compatibilidad con mensajes de texto y funcionalidades relacionadas.|Este es un "servicio por usuario" y, como tal, el servicio de plantillas debe estar deshabilitado.|
|Host de sincronización|OneSyncSvc|Este servicio sincroniza el correo, los contactos, el calendario y varios otros datos del usuario. |(UWP) El correo y otras aplicaciones que dependen de esta funcionalidad no funcionarán correctamente si este servicio no se está ejecutando. <p>Este es un "servicio por usuario" y, como tal, el servicio de plantillas debe estar deshabilitado.|
|Datos de contacto|PimIndexMaintenanceSvc|Indexa los datos de contacto para poder buscar los contactos rápidamente. Si detienes o deshabilitas este servicio, es posible que falten contactos en tus resultados de búsqueda.|Este es un "servicio por usuario" y, como tal, el servicio de plantillas debe estar deshabilitado.|
|Power|Power|Administra la directiva de energía y la entrega de notificaciones de dicha directiva.|Las máquinas virtuales prácticamente no influyen en las propiedades de energía. Si este servicio está deshabilitado, la administración de energía y los informes no están disponibles. Para obtener más información, consulte [este artículo](/windows-hardware/drivers/powermeter/user-mode-power-service).|
|Administrador de pagos y NFC/SE|SEMgrSvc|Administra los pagos y los elementos seguros basados en la transmisión de campos en proximidad (NFC).|Es posible que no necesite este servicio para los pagos en el entorno empresarial.|
|Servicio enrutador de SMS de Microsoft Windows|SmsRouter|Enruta los mensajes en función de las reglas a los clientes adecuados.|Es posible que no necesite este servicio si se usan otras herramientas para la mensajería, como Teams o Skype. Para obtener más información, consulte [este artículo](/dotnet/framework/wcf/feature-details/routing-service).|.
|SuperFetch (SysMain)|SysMain|Mantiene y mejora el rendimiento del sistema a lo largo del tiempo.|En general, SuperFetch no mejora el rendimiento en entornos de escritorio virtual por varias razones. El almacenamiento subyacente suele estar virtualizado y posiblemente seccionado en varias unidades. En algunas soluciones de escritorio virtual, el estado de usuario acumulado se descarta cuando el usuario cierra la sesión. La característica SysMain debe evaluarse en cada entorno.|
|Servicio de teclado táctil y panel de escritura a mano|TabletInputService|Habilita las funciones de lápiz y entradas de lápiz del teclado táctil y del panel de escritura a mano.|No es necesario a menos que se use una pantalla táctil activa o un dispositivo de entrada manuscrita.|
|Actualizar el servicio Orchestrator|UsoSvc|Administra las actualizaciones de Windows. Si se detiene, los dispositivos no podrán descargar ni instalar las actualizaciones más recientes.|A menudo, los dispositivos de escritorio virtual se administran cuidadosamente con respecto a las actualizaciones. El mantenimiento se realiza normalmente durante las ventanas de mantenimiento. En algunos casos, se puede utilizar un cliente de actualización, como SCCM. La excepción serían las actualizaciones de firmas de seguridad, que se aplicarían en cualquier momento en cualquier dispositivo de escritorio virtual, a fin de mantener las firmas actualizadas. Si deshabilita este servicio, realice una prueba para asegurarse de que las firmas de seguridad todavía se pueden instalar.|
|Instantánea de volumen|VSS|Administra e implementa instantáneas de volumen usadas para copias de seguridad y otros propósitos. |Si este servicio se detiene, las instantáneas se deshabilitarán para la copia de seguridad y esta podría generar un error. Si el servicio se deshabilita, los servicios que dependen explícitamente de él no se podrán iniciar. Para obtener más información, consulte [este artículo](../../../WindowsServerDocs/storage/file-server/volume-shadow-copy-service.md).|
|Host de sistema de diagnóstico|WdiSystemHost|El Servicio de directivas de diagnóstico usa el Host de sistema de diagnóstico para hospedar los diagnósticos que deben ejecutarse en un contexto de sistema local. Si se detiene este servicio, los diagnósticos que dependan de él dejarán de funcionar.|Al deshabilitar este servicio, se deshabilita la capacidad de ejecutar diagnósticos de Windows.
|Informe de errores de Windows|WerSvc|Permite que se informe acerca de los errores cuando los programas dejan de funcionar o responder y permite que se entreguen las soluciones existentes. También permite generar registros para los servicios de diagnóstico y reparaciones. Si se detiene este servicio, es posible que el informe de errores no funcione correctamente y que no se muestren los resultados de los servicios de diagnóstico y reparaciones.|Con los entornos de escritorio virtual, los diagnósticos se realizan a menudo en un escenario "sin conexión" y no en la producción convencional. Asimismo, algunos clientes deshabilitan WER de todos modos. WER usa una pequeña cantidad de recursos para hacer muchas cosas diferentes, incluyendo mostrar un error al instalar un dispositivo o un error al instalar una actualización. Para obtener más información, consulte [este artículo](/windows/win32/wer/windows-error-reporting).|
|Windows Search|WSearch|Proporciona la indexación del contenido, el almacenamiento en caché de propiedades y los resultados de la búsqueda de archivos, correo electrónico y otro contenido.|La deshabilitación de este servicio impide la indexación del correo electrónico y otras cosas. Realice una prueba antes de deshabilitar este servicio. Para obtener más información, consulte [este artículo](/windows/win32/search/-search-3x-wds-overview#windows-search-service). |
|Administración de autenticación de Xbox Live|XblAuthManager|Proporciona servicios de autenticación y de autorización para interactuar con Xbox Live. |Si se detiene este servicio, es posible que algunas aplicaciones no funcionen correctamente.
|Partida guardada en Xbox Live|XblGameSave|Este servicio sincroniza los datos guardados para los juegos que pueden guardarse en Xbox Live. |Si se detiene el servicio, la partida guardada no se cargará a Xbox Live ni se descargará desde ahí.
|Servicio de administración de accesorios de Xbox|XboxGipSvc|Este servicio administra los accesorios de Xbox conectados.| N/D |
|Servicio de red de Xbox Live|XboxNetApiSvc|Este servicio es compatible con la interfaz de programación de aplicaciones Windows.Networking.XboxLive.| N/D |

#### <a name="per-user-services-in-windows"></a>Servicios por usuario en Windows

Los servicios por usuario son servicios que se crean cuando un usuario inicia sesión en Windows o Windows Server y se detienen y eliminan cuando ese usuario cierra la sesión. Estos servicios se ejecutan en un contexto de seguridad de la cuenta de usuario; esto proporciona una administración de recursos mejor que la del enfoque anterior para ejecutar este tipo de servicios en el explorador, el cual asociado a una cuenta preconfigurada o como tareas. Para obtener más información, consulte [Servicios por usuario en Windows](/windows/application-management/per-user-services-in-windows).

Si piensa cambiar el valor de inicio de un servicio, el método preferido es abrir un símbolo del sistema con privilegios elevados .cmd y ejecutar la herramienta Administrador de control de servicios `SC.EXE`. Para obtener más información, consulte [SC](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc754599(v=ws.11)).

### <a name="scheduled-tasks"></a>Tareas programadas

Al igual que otros elementos en Windows, asegúrese de que un elemento no es necesario antes de deshabilitar una tarea programada. Es posible que no desee que algunas tareas en entornos de escritorio virtual, como **StartComponentCleanup**, se ejecuten en producción, pero que resulten adecuadas para su ejecución durante una ventana de mantenimiento en la "imagen gold" (imagen de referencia).

En la siguiente lista se incluyen tareas que realizan optimizaciones o recopilaciones de datos en equipos que mantienen su estado durante los reinicios. Cuando un dispositivo de escritorio virtual se reinicia y descarta todos los cambios desde el último arranque, las optimizaciones destinadas a los equipos físicos no son útiles.

Puedes obtener todas las tareas programadas actuales, incluidas las descripciones, con el siguiente código de PowerShell:

```powershell
  Get-ScheduledTask | Select-Object -Property TaskPath,TaskName,State,Description
```

> [!NOTE]
> Hay varias tareas que no se pueden deshabilitar con un script, incluso si se ejecutan en un símbolo del sistema con privilegios elevados. Las recomendaciones que se indican aquí y en los scripts de GitHub no intentan deshabilitar las tareas que no se pueden deshabilitar con un script.

|Nombre de tarea programada|Description|
|-------------------|-----------|
|MNO|Analizador de metadatos de experiencia de cuenta de banda ancha móvil.|
|AnalyzeSystem|Esta tarea analiza el sistema en busca de condiciones que puedan causar un uso elevado de energía.|
|Móvil|Relacionada con dispositivos móviles.|
|Compatibilidad|Recopila información de telemetría de programas si participa en el Programa para la mejora de la experiencia del usuario de Microsoft.|
|Consolidator|Si el usuario ha dado su consentimiento para participar en el Programa para la mejora de la experiencia del usuario de Windows, este trabajo recopila y envía datos de uso a Microsoft.|
|Diagnóstico|(DiskFootprint en la ruta de acceso de la tarea) "DiskFootprint" es la contribución combinada de todos los procesos que emiten E/S de almacenamiento en forma de lecturas, escrituras y vaciados de almacenamiento.|
|FamilySafetyMonitor|Inicializa la supervisión y la aplicación de Protección infantil.|
|FamilySafetyRefreshTask|Sincroniza la configuración más reciente con el servicio de características de familia de Microsoft.|
|MapsToastTask|Esta tarea muestra varias notificaciones del sistema relacionadas con el mapa.|
|Microsoft-Windows-DiskDiagnosticDataCollector|El diagnóstico de discos de Windows genera informes de datos generales sobre el disco y el sistema para Microsoft para los usuarios que participan en el Programa de la experiencia del usuario.|
|NotificationTask|Tarea en segundo plano para realizar interacciones por usuario y web.|
|ProcessMemoryDiagnosticEvents|Programa un diagnóstico de memoria en respuesta a eventos del sistema.|
|Proxy|Esta tarea recopila y carga datos SQM de comprobación automática si participa en el Programa para la mejora de la experiencia del usuario de Microsoft.|
|QueueReporting|Tarea Informe de errores de Windows para procesar los informes en cola.|
|RecommendedTroubleshootingScanner|Búsqueda de la solución de problemas recomendada de Microsoft.|
|RegIdleBackup|Tarea de copia de seguridad de inactividad de Registro|
|RunFullMemoryDiagnostic|Detecta y mitiga problemas en la memoria física (RAM).|
|Programado|La tarea de mantenimiento programada de Windows realiza un mantenimiento periódico del sistema del equipo; para ello, corrige los problemas automáticamente o los notifica a través de Seguridad y el mantenimiento.|
|ScheduledDefrag|Esta tarea optimiza las unidades de almacenamiento local.|
|SilentCleanup|Tarea de mantenimiento usada por el sistema para iniciar una limpieza silenciosa automática del disco cuando el espacio en disco es insuficiente.|
|SpeechModelDownloadTask|
|Sqm-Tasks|Esta tarea recopila información sobre el Módulo de plataforma segura (TPM), el arranque seguro y el arranque medido.|
|SR|Esta tarea crea puntos de protección del sistema habituales.|
|StartComponentCleanup|Tarea de mantenimiento que puede realizarse mejor durante las ventanas de mantenimiento.|
|StartupAppTask|Examina las entradas de inicio y genera una notificación para el usuario si hay demasiadas entradas de inicio.|
|SyspartRepair|
|WindowsActionDialog|Notificación de ubicación|
|WinSAT|Mide el rendimiento y las funcionalidades de un sistema.|
|XblGameSaveTask|Tarea en espera GameSave de Xbox Live|

### <a name="apply-windows-and-other-updates"></a>Aplicación de actualizaciones de Windows (y otras)

Desde Microsoft Update, o desde los recursos internos, se aplican las actualizaciones disponibles, incluidas las firmas de Windows Defender. Este es un buen momento para aplicar otras actualizaciones disponibles, incluidas las de Microsoft Office si están instaladas, y otras actualizaciones de software. Si PowerShell va a permanecer en la imagen, puedes descargar la ayuda más reciente disponible para PowerShell ejecutando el comando `Update-Help`.

#### <a name="servicing-os-and-apps"></a>Mantenimiento del sistema operativo y las aplicaciones

En algún momento durante el proceso de optimización de la imagen, se deben aplicar las actualizaciones de Windows. Hay una opción en la configuración de actualización de Windows 10 que puede proporcionar actualizaciones adicionales. Puede encontrarla en **Configuración** > **Opciones avanzadas**. Una vez allí, establezca **Ofrecer actualizaciones para otros productos de Microsoft cuando actualice Windows** en **Activada**.

![Captura de pantalla del menú Opciones avanzadas. La opción "Ofrecer actualizaciones para otros productos de Microsoft cuando actualice Windows" está activada.](media/rds-vdi-recommendations-1909/servicing.png)

Esta es una buena configuración en caso de que vayas a instalar aplicaciones de Microsoft como Microsoft Office en la imagen base. De esta forma, Office se actualiza cuando la imagen se agrega al servicio. Asimismo, también hay actualizaciones de .NET y ciertos componentes de terceros, como Adobe, que tienen actualizaciones disponibles a través de Windows Update.

Es muy importante que tenga en cuenta las actualizaciones de seguridad de los dispositivos de escritorio virtual no persistentes, incluyendo los archivos de definición de software de seguridad. Estas actualizaciones se pueden publicar una vez o más de una vez al día.

En cuanto a Windows Defender, es posible que sea mejor permitir que se realicen las actualizaciones, incluso en entornos de escritorio virtual no persistentes. Las actualizaciones se aplicarán casi todas las veces que inicie sesión, pero estas son pequeñas y no deberían suponer un problema. Además, el dispositivo no se retrasará en las actualizaciones porque solo se aplicará la última disponible. Lo mismo podría aplicarse a los archivos de definición de terceros.

>[!NOTE]
> Las aplicaciones de Store (aplicaciones UWP) se actualizan a través de Microsoft Store. Las versiones modernas de Office, como Office 365, se actualizan a través de sus propios mecanismos cuando están conectadas directamente a Internet o a través de tecnologías de administración cuando no lo están.

#### <a name="windows-system-startup-event-traces-autologgers"></a>Seguimientos de eventos de inicio del sistema Windows (registradores automáticos)

Windows está configurado, de forma predeterminada, para recopilar y guardar datos de diagnóstico. El propósito es habilitar los diagnósticos o registrar datos si es necesario solucionar problemas adicionales. Los seguimientos del sistema automáticos se pueden encontrar en la ubicación que se muestra en la siguiente ilustración:

![Captura de pantalla de la carpeta de administración del equipo. La carpeta de seguimientos del sistema está abierta y se ha seleccionado el archivo WiFiSession.](media/rds-vdi-recommendations-1909/system-traces.png)

Algunas los seguimientos mostrados en **Sesiones de seguimiento de eventos** y **Sesiones de seguimiento de eventos de inicio** no pueden y no deben detenerse. Otras opciones, como el seguimiento WiFiSession, se pueden detener. Para detener un seguimiento en ejecución en **Sesiones de seguimiento de eventos**, haga clic con el botón derecho en el seguimiento y seleccione **Detener**. Usa el siguiente procedimiento para evitar que los seguimientos se inicien automáticamente en el inicio:

1. Seleccione la carpeta **Sesiones de seguimiento de eventos de inicio**.

2. Busque y seleccione el archivo de seguimiento que quiere ver para abrirlo.

3. Selecciona la pestaña **Realizar seguimiento de sesión**.

4. Desactive la casilla **Habilitado**.

5. Seleccione **Aceptar**.

En la tabla siguiente se enumeran algunos seguimientos del sistema que debería considerar deshabilitar en los entornos de escritorio virtual:

|Nombre|Comentario|
|---|--------|
|CellCore|[https://docs.microsoft.com/windows-hardware/drivers/network/cellular-architecture-and-driver-model](/windows-hardware/drivers/network/cellular-architecture-and-driver-model)|
|CloudExperienceHostOOBE|Se documenta [aquí](/windows/security/identity-protection/hello-for-business/hello-how-it-works-technology#cloud-experience-host).|
|DiagLog|Un registro generado por el Servicio de directivas de diagnóstico, que se documenta [aquí](/windows-server/security/windows-services/security-guidelines-for-disabling-system-services-in-windows-server)|
|RadioMgr|Se documenta [aquí](/windows-hardware/drivers/nfc/what-s-new-in-nfc-device-drivers).|
|ReadyBoot|Se documenta [aquí](/previous-versions/windows/desktop/xperf/readyboot-analysis).|
|WDIContextLog|Interfaz del controlador de dispositivo de red de área local inalámbrica (WLAN), que se documenta aquí. |
|WiFiDriverIHVSession|Se documenta [aquí](/windows-hardware/drivers/network/user-initiated-feedback-normal-mode).|
|WiFiSession|Registro de diagnóstico de la tecnología WLAN. Si no se implementa la conexión Wi-Fi, no se necesita este registrador.|
|WinPhoneCritical|Registro de diagnóstico del teléfono (¿Windows?). Si no se usan teléfonos, no se necesita este registrador.|

#### <a name="windows-defender-optimization-in-the-virtual-desktop-environment"></a>Optimización de Windows Defender en el entorno de escritorio virtual

Para obtener más información sobre cómo optimizar Windows Defender en un entorno de escritorio virtual, consulte la [Guía de implementación del Antivirus de Windows Defender en un entorno de infraestructura de escritorio virtual (VDI)](/windows/security/threat-protection/windows-defender-antivirus/deployment-vdi-windows-defender-antivirus).

El artículo anterior contiene procedimientos para el mantenimiento de la imagen de escritorio virtual de tipo "gold" y sobre cómo mantener los clientes de escritorio virtual mientras se ejecutan. Para reducir el uso de ancho de banda de la red cuando los dispositivos de escritorio virtual necesiten actualizar sus firmas de Windows Defender, es recomendable escalonar los reinicios y programarlos durante sus horas libres, siempre que sea posible. Las actualizaciones de firmas de Windows Defender pueden estar en los recursos compartidos de archivos de forma interna y, si es necesario, esos archivos compartidos se encuentran en los mismos segmentos de red o en segmentos de red cercanos a los dispositivos de escritorio virtual.

#### <a name="client-network-performance-tuning-by-registry-settings"></a>Ajuste del rendimiento de red del cliente mediante la configuración del registro

Hay algunas configuraciones del registro que pueden aumentar el rendimiento de la red. Esto es especialmente importante en entornos donde el dispositivo de escritorio virtual o el equipo físico tiene una carga de trabajo que se basa principalmente en la red. Se recomienda la configuración de esta sección para ajustar el rendimiento para el perfil de la carga de trabajo de redes. Para ello, configure un búfer adicional y almacene en caché cosas como las entradas de directorio, entre otras.

> [!NOTE]
> Algunas configuraciones de esta sección se basan únicamente en el registro y deben incorporarse en la imagen base antes de implementarla para su uso en la producción.

Las siguientes opciones de configuración se documentan en [Directrices de optimización del rendimiento para Windows Server 2016](../../administration/performance-tuning/index.md).

#### <a name="disablebandwidththrottling"></a>DisableBandwidthThrottling

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DisableBandwidthThrottling`

Válido para Windows 10. El valor predeterminado es **0**. De manera predeterminada, el redirector de SMB limita el rendimiento en las conexiones de red de alta latencia, en algunos casos para evitar tiempos de espera relacionados con la red. Al establecer este valor del Registro en **1**, se deshabilita esta limitación, lo que permite un mayor rendimiento de transferencia de archivos a través de conexiones de red de alta latencia. Intenta establecer este valor en **1**.

#### <a name="fileinfocacheentriesmax"></a>FileInfoCacheEntriesMax

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileInfoCacheEntriesMax`

Válido para Windows 10. El valor predeterminado es **64**, con un intervalo válido entre 1 y 65536. Este valor se usa para determinar la cantidad de metadatos de archivo que el cliente puede almacenar en caché. Aumentar el valor puede disminuir el tráfico de red y aumentar el rendimiento cuando se accede a varios archivos. Intenta aumentar este valor a **1024**.

#### <a name="directorycacheentriesmax"></a>DirectoryCacheEntriesMax

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DirectoryCacheEntriesMax`

Válido para Windows 10. El valor predeterminado es **16**, con un intervalo válido entre 1 y 4096. Este valor se usa para determinar la cantidad de información de directorio que el cliente puede almacenar en caché. Aumentar el valor puede disminuir el tráfico y aumentar el rendimiento cuando se accede a directorios de gran tamaño. Intenta aumentar este valor a **1024**.

#### <a name="filenotfoundcacheentriesmax"></a>FileNotFoundCacheEntriesMax

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileNotFoundCacheEntriesMax`

Válido para Windows 10. El valor predeterminado es **128**, con un intervalo válido entre 1 y 65536. Este valor se usa para determinar la cantidad de información de nombres de archivos que el cliente puede almacenar en caché. Aumentar el valor puede disminuir el tráfico de red y aumentar el rendimiento cuando se accede a varios archivos de nombres. Intenta aumentar este valor a **2048**.

#### <a name="dormantfilelimit"></a>DormantFileLimit

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DormantFileLimit`

Válido para Windows 10. El valor predeterminado es **1023**. Este parámetro especifica el número máximo de archivos que se deben dejar abiertos en un recurso compartido una vez que la aplicación cerró el archivo. Si varios miles de clientes se conectan a servidores SMB, considera la posibilidad de reducir este valor a **256**.

Puedes configurar varias de estas opciones de SMB con los cmdlets [Set-SmbClientConfiguration](/powershell/module/smbshare/set-smbclientconfiguration?view=win10-ps&preserve-view=true) y [Set-SmbServerConfiguration](/powershell/module/smbshare/set-smbserverconfiguration?view=win10-ps&preserve-view=true) de Windows PowerShell. Las opciones de solo registro se pueden configurar con Windows PowerShell, tal como se muestra en el siguiente ejemplo:

```powershell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters" RequireSecuritySignature -Value 0 -Force
```

#### <a name="additional-settings-from-the-windows-restricted-traffic-limited-functionality-baseline-guidance"></a>Configuración adicional de las instrucciones de línea de base de funcionalidades limitadas de tráfico restringido de Windows

Microsoft ha lanzado una línea de base que se creó con los mismos procedimientos que las [Líneas de base de seguridad de Windows](/windows/device-security/windows-security-baselines), para entornos que no están conectados directamente a Internet o que deben reducir los datos enviados a Microsoft y otros servicios.

La configuración [Línea de base de funcionalidades limitadas de tráfico restringido de Windows](/windows/privacy/manage-connections-from-windows-operating-system-components-to-microsoft-services) se indica en la tabla de directivas de grupo con un asterisco.

#### <a name="disk-cleanup-including-using-the-disk-cleanup-wizard"></a>Limpieza del disco (incluido el uso del Asistente para liberar espacio en disco)

La limpieza del disco puede ser especialmente útil con implementaciones de escritorio virtual de imagen gold o maestra. Una vez que la imagen gold o maestra está preparada, actualizada y configurada, una de las últimas tareas a realizar es la de limpieza del disco. Los scripts de optimización de Github.com tienen código de PowerShell para realizar tareas comunes de limpieza del disco.

> [!NOTE]
> La configuración de limpieza del disco se encuentra en Configuración, en la categoría de "Sistema" denominada "Almacenamiento". De forma predeterminada, el Sensor de almacenamiento se ejecuta cuando se alcanza un umbral de espacio libre en disco bajo.
>
> Para más información sobre cómo usar el Sensor de almacenamiento con imágenes VHD personalizadas de Azure, consulte [Preparación y personalización de una imagen de disco duro virtual maestro](/azure/virtual-desktop/set-up-customize-master-image).
>
> Se recomienda deshabilitar el sensor de almacenamiento en el host de sesión de Windows Virtual Desktop que usa Windows 10 Enterprise o la sesión múltiple de Windows 10 Enterprise. Puede deshabilitar el Sensor de almacenamiento en el menú Configuración, debajo de **Almacenamiento**.

Igualmente, aquí tienes sugerencias para realizar varias tareas para liberar espacio en disco. Todas deben probarse antes de la implementación:

1. El Sensor de almacenamiento se puede usar de forma manual o automática. Para obtener más información sobre el Sensor de almacenamiento, consulte este artículo: Uso de OneDrive y el Sensor de almacenamiento en Windows 10 para administrar el espacio en disco.
2. Limpieza manual de los archivos temporales y los registros. Desde un símbolo del sistema con privilegios elevados, ejecuta estos comandos:

   1. `Del C:\*.tmp /s`
   2. `C:\*.etl /s`
   3. `C:\*.evtx /s`

   ```powershell
   Get-ChildItem -Path c:\ -Include *.tmp, *.dmp, *.etl, *.evtx, thumbcache*.db, *.log -File -Recurse -Force -ErrorAction SilentlyContinue | Remove-Item -ErrorAction SilentlyContinue

   Remove-Item -Path $env:ProgramData\Microsoft\Windows\WER\Temp\* -Recurse -Force -ErrorAction SilentlyContinue

   Remove-Item -Path $env:ProgramData\Microsoft\Windows\WER\ReportArchive\* -Recurse -Force -ErrorAction SilentlyContinue

   Remove-Item -Path $env:ProgramData\Microsoft\Windows\WER\ReportQueue\* -Recurse -Force -ErrorAction SilentlyContinue

   Clear-RecycleBin -Force -ErrorAction SilentlyContinue

   Clear-BCCache -Force -ErrorAction SilentlyContinue
   ```

3. Elimine los perfiles que no use del sistema mediante la ejecución del comando siguiente:

    `wmic path win32_UserProfile where LocalPath="C:\\users\\<users>" Delete`

Si tiene cualquier pregunta o duda sobre la información de este documento, póngase en contacto con el equipo de su cuenta de Microsoft, lea el [blog para profesionales de TI de Microsoft Virtual Desktop](https://community.windows.com/stories/virtual-desktop-windows-10), publique un mensaje en los [foros de Microsoft Virtual Desktop](https://techcommunity.microsoft.com/t5/windows-virtual-desktop/bd-p/WindowsVirtualDesktop) o póngase en contacto con [Microsoft](https://support.microsoft.com/contactus/) directamente.

### <a name="re-enable-windows-update"></a>Volver a habilitar Windows Update

Si desea habilitar el uso de Windows Update después de deshabilitarlo, como en el caso del escritorio virtual persistente, realice los pasos siguientes:

1. Vuelva a habilitar la configuración de directiva de grupo:

   - Diríjase a **Directiva de equipo local** > **Configuración del equipo** > **Plantillas administrativas** > **Sistema** > **Administración de comunicaciones de Internet** > **Configuración de comunicaciones de Internet**.
     - Para desactivar el acceso a todas las características de Windows Update, cambie la opción de **habilitado** a **no configurado**.
   - Diríjase a **Directiva de equipo local** > **Configuración del equipo** > **Plantillas administrativas** > **Componentes de Windows** > **Windows Update**.
     - Para quitar el acceso a todas las características de Windows Update, cambie la opción de **habilitado** a **no configurado**.
     - No se conecte a ninguna ubicación de Internet de Windows Update mediante la modificación de la opción de **habilitado** a **no configurado**.
   - Vaya a **Directiva de equipo local** > **Configuración del equipo** > **Plantillas administrativas** > **Componentes de Windows** > **Windows Update** > **Windows Update para empresas**.
     - Seleccione cuándo quiere recibir las actualizaciones de calidad (cambie la opción de **habilitado** a **no configurado**).
   - Vaya a **Directiva de equipo local** > **Configuración del equipo** > **Plantillas administrativas** > **Componentes de Windows** > **Windows Update** > **Windows Update para empresas**.
     - Selecciona cuándo quieres recibir las actualizaciones de versiones preliminares y características (cámbialas de **habilitado** a **no configurado**).

2. Vuelva a habilitar los servicios:

   - Cambie la opción de **Actualizar el servicio Orchestrator** de **deshabilitado** a **Automático (inicio retrasado)** .

3. Edite el registro de Windows (advertencia: tenga cuidado al editar el Registro).

   - Ir a `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsUpdate\UpdatePolicy\PolicyState`.
     - Cambie el valor de **DeferQualityUpdates** de "1" a "0".
   - `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsUpdate\UpdatePolicy\Settings`
     - Elimine cualquier valor existente de **PausedQualityDate**.
   - Vaya a `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\Explorer\WAU`.
     - Establezca esta opción en **Deshabilitado**.

4. Vuelva a habilitar las tareas programadas:

   - Vaya a **Biblioteca del Programador de tareas** > **Microsoft** > **Windows** > **InstallService** > **ScanForUpdates**.
   - Vaya a **Biblioteca del Programador de tareas** > **Microsoft** > **Windows** > **InstallService** > **ScanForUpdatesAsUser**.

5. Para que todas estas configuraciones surtan efecto, reinicie el dispositivo.

6. Si no quiere que este dispositivo le ofrezca actualizaciones de características, diríjase a **Configuración** > **Windows Update** > **Opciones avanzadas** > **Elegir cuándo se instalarán las actualizaciones** y, a continuación, establezca manualmente la opción **Una actualización de características incluye mejoras y nuevas capacidades. Se puede posponer todos estos días** en un valor distinto de cero, como 180, 365, etc.

## <a name="additional-information"></a>Información adicional

Obtenga más información sobre la arquitectura de VDI de Microsoft en nuestra [documentación de Windows Virtual Desktop](https://azure.microsoft.com/services/virtual-desktop/).

Si necesita ayuda adicional para solucionar problemas de Sysprep, consulte [Se produce un error en Sysprep después de quitar o actualizar aplicaciones de Microsoft Store que incluyen imágenes integradas de Windows](https://support.microsoft.com/help/2769827/sysprep-fails-after-you-remove-or-update-windows-store-apps-that-inclu).
