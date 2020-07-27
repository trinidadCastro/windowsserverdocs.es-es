---
title: Optimización de Windows 10, versión 1909, para un rol de Infraestructura de escritorio virtual (VDI)
description: Configuración y ajustes recomendados para minimizar la sobrecarga de los escritorios Windows 10, versión 1909, que se usan como imágenes VDI.
ms.prod: windows-server
ms.reviewer: robsmi
ms.technology: remote-desktop-services
ms.author: helohr
ms.topic: article
author: heidilohr
manager: lizross
ms.date: 02/19/2020
ms.openlocfilehash: 4598c0f60fac98cd14a6f7d920b9c6f31704bd06
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86963377"
---
# <a name="optimizing-windows-10-version-1909-for-a-virtual-desktop-infrastructure-vdi-role"></a>Optimización de Windows 10, versión 1909, para un rol de Infraestructura de escritorio virtual (VDI)

En este artículo podrás elegir la configuración para Windows 10, versión 1909 (compilación 18363) que debería tener el mejor rendimiento en un entorno de infraestructura de escritorio virtualizado (VDI). Todas las configuraciones de esta guía son recomendaciones a tener en cuenta y de ninguna manera son obligatorias.

Los métodos clave para optimizar el rendimiento de Windows 10 en un entorno de VDI son minimizar el volver a dibujar los gráficos de la aplicación, las actividades en segundo plano que no tienen una ventaja importante para el entorno de VDI y, en general, reducir los procesos en ejecución al mínimo. Un objetivo secundario es reducir el uso del espacio en disco de la imagen base al mínimo. Con las implementaciones de VDI, el tamaño de imagen base o de tipo "gold" más pequeño posible, puede reducir ligeramente el uso de la memoria en el hipervisor, así como provocar una pequeña reducción en las operaciones de red generales necesarias para entregar la imagen de escritorio al consumidor.

> [!NOTE]
> Esta configuración recomendada se puede aplicar a otras instalaciones de Windows 10 1909, incluidos los dispositivos físicos u otras máquinas virtuales. Ninguna recomendación de este artículo debería afectar a la compatibilidad de Windows 10 1909.

## <a name="vdi-optimization-principles"></a>Principios de optimización de VDI

Un entorno de VDI presenta una sesión de escritorio completa (incluyendo las aplicaciones) de un usuario de PC a través de una red. El vehículo de entrega de red puede ser una red local o puede ser Internet. Los entornos de VDI son una imagen del sistema operativo "base", que luego se convierte en la base de los escritorios que posteriormente se presentan a los usuarios. Existen variaciones de las implementaciones de VDI, por ejemplo, "persistente", "no persistente" y "sesión de escritorio". El tipo persistente conserva los cambios en el sistema operativo del escritorio de VDI de una sesión a la siguiente. El tipo no persistente no conserva los cambios en el sistema operativo del escritorio de VDI de una sesión a la siguiente. Para el usuario, este escritorio no es muy diferente a otro dispositivo virtual o físico, aparte de que se accede a través de una red.

La configuración de optimización se lleva a cabo en un dispositivo de referencia. Una VM es un lugar ideal para compilar la imagen, ya que se puede guardar el estado, crear puntos de control y realizar copias de seguridad. Se realiza una instalación predeterminada del sistema operativo en la máquina virtual base. Después, la máquina virtual base se optimiza quitando las aplicaciones innecesarias, instalando actualizaciones de Windows, instalando otras actualizaciones, eliminando archivos temporales y aplicando la configuración.

Hay otros tipos de VDI, como la sesión de Escritorio remoto (RDS) y [Windows Virtual Desktop](https://azure.microsoft.com/services/virtual-desktop/) que se ha lanzado recientemente. La explicación detallada de estas tecnologías está fuera del ámbito de este artículo. Este artículo se centra en la configuración de la imagen base de Windows, sin hacer referencia a otros factores del entorno, como la optimización del host.

La seguridad y la estabilidad son las principales prioridades de Microsoft cuando se trata de productos y servicios. Los clientes empresariales pueden optar por usar la seguridad integrada de Windows, un conjunto de servicios que funcionan bien con o sin Internet. En el caso de los entornos de VDI que no están conectados a Internet, las firmas de seguridad se pueden descargar varias veces al día, ya que Microsoft puede publicar más de una actualización de firma al día. Después, esas firmas se pueden proporcionar a las máquinas virtuales de VDI y programarse para que se instalen durante la producción, independientemente de que sean persistentes o no persistentes. De este modo, la protección de la máquina virtual está lo más actualizada posible.

Hay algunas configuraciones de seguridad que no son aplicables a los entornos de VDI que no están conectados a Internet y, por lo tanto, no pueden participar en la seguridad habilitada para la nube. Hay otras configuraciones que los dispositivos Windows "normales" pueden usar, como la experiencia en la nube, Microsoft Store, etc. Quitar el acceso a las características no usadas reduce la superficie, el ancho de banda de red y la superficie expuesta a ataques.

En lo que respecta a las actualizaciones, Windows 10 emplea un algoritmo de actualización mensual, por lo que no es necesario que los clientes intenten actualizarse. En la mayoría de los casos, los administradores de VDI controlan el proceso de actualización a través de un proceso de apagado de máquinas virtuales basado en una imagen "maestra" o "gold", quitan el sello de la imagen, que es de solo lectura, aplican revisiones a la imagen y, a continuación, la vuelven a sellar y la devuelven a producción. Por lo tanto, no es necesario que las máquinas virtuales de VDI comprueben Windows Update. En ciertos casos, por ejemplo, máquinas virtuales de VDI persistentes, se realizan procedimientos de revisión normales. También se puede usar Windows Update o Microsoft Intune. Puede usarse System Center Configuration Manager para administrar la actualización y otras entregas de paquetes. Depende de cada organización determinar el mejor enfoque para actualizar VDI.

> [!TIP]  
> Un script que implementa las optimizaciones discutidas en este tema, así como un archivo de exportación GPO que puedes importar con **LGPO.exe**, está disponible en [TheVDIGuys](https://github.com/TheVDIGuys) en GitHub.

Este script se diseñó para adaptarse a tu entorno y requisitos. El código principal es PowerShell y el trabajo se realiza mediante el uso de archivos de entrada (texto sin formato), con los archivos de exportación de la herramienta Objeto de directiva de grupo local (LGPO). Estos archivos contienen listas de aplicaciones que se deben quitar y servicios que se deben deshabilitar. Si no quieres quitar una aplicación determinada o deshabilitar un servicio en concreto, edita el archivo de texto correspondiente y quita el elemento. Por último, hay configuraciones de directiva local que se pueden importar al dispositivo. Es mejor tener alguna configuración dentro de la imagen base, que aplicar la configuración a través de la directiva de grupo, ya que algunas de las opciones de configuración son efectivas en el siguiente reinicio o cuando se usa por primera vez un componente.

### <a name="persistent-vdi"></a>VDI persistente

En el nivel básico, la VDI persistente es una VM que guarda los estados del sistema operativo entre arranques. Otros niveles de software de la solución VDI proporcionan a los usuarios acceso fácil y sin problemas a sus VM asignadas, a menudo con una solución de inicio de sesión único.

Existen varias implementaciones diferentes de VDI persistente:

- La máquina virtual tradicional, donde la VM tiene su propio archivo de disco virtual, se inicia normalmente y guarda los cambios de una sesión a la siguiente. La diferencia radica en cómo accede el usuario a esa VM. Es posible que haya un portal web en el que el usuario inicia sesión y que lo dirige automáticamente a sus una o más VM asignadas de VDI.

- Una máquina virtual persistente basada en imágenes, opcionalmente con discos virtuales personales. En este tipo de implementación hay una imagen base o de tipo "gold" en uno o más servidores host. Se crea una VM y, a continuación, uno o más discos virtuales se crean y asignan a este disco para obtener un almacenamiento persistente.

    - Cuando se inicia la VM, se lee una copia de la imagen base en la memoria de esa VM. Al mismo tiempo, se asigna un disco virtual persistente a esa VM, con cualquier cambio previo en el sistema operativo combinado mediante un proceso complejo.

    - Cambios tales como las escrituras del registro de eventos o las escrituras del registro, se redirigen al disco virtual de lectura y escritura que está asignado a esa VM.

    - En esta circunstancia, el sistema operativo y el mantenimiento de la aplicación pueden funcionar normalmente si se usa el software de mantenimiento tradicional, como Windows Server Update Services u otras tecnologías de administración.

    - La diferencia entre una máquina VDI persistente y una máquina virtual "normal" es la relación con la imagen maestra/gold. En algún momento, se deben aplicar actualizaciones a la imagen maestra. Aquí es donde, en las implementaciones, se decide cómo se administran los cambios persistentes del usuario. En algunos casos, el disco con los cambios se descarta o restablece, con lo que se establece un nuevo punto de control. También podría ser que los cambios que realiza el usuario se mantengan a través de actualizaciones de calidad mensuales y que la base se restablezca después de una actualización de características.

### <a name="non-persistent-vdi"></a>VDI no persistente

Cuando una implementación de VDI no persistente se basa en una imagen base o de tipo "gold", las optimizaciones se realizan principalmente en la imagen base y luego a través de la configuración local y las directivas locales.

Con la VDI no persistente basada en imágenes, la imagen base es de solo lectura. Cuando se inicia una VM no persistente, se transmite una copia de la imagen base a la VM. La actividad que se produce durante el inicio del proceso hasta que el siguiente reinicio, se redirige a una ubicación temporal. Por lo general, se proporciona a los usuarios ubicaciones de red para almacenar sus datos. En algunos casos, el perfil del usuario se combina con la VM estándar para proporcionar al usuario su configuración.

Un aspecto importante de la VDI no persistente que se basa en una sola imagen es el mantenimiento. Las actualizaciones del sistema operativo y los componentes se entregan generalmente una vez al mes. Con la VDI basada en imágenes, se deben realizar un conjunto de procesos para obtener las actualizaciones de la imagen:

- En un host determinado, todas las VM de ese host que se derivan de la imagen base deben apagarse o desactivarse. Esto significa que los usuarios se redirigen a otras VM.

- A continuación, la imagen base se abre y se inicia. Posteriormente, se realizan todas las actividades de mantenimiento, como actualizaciones del sistema operativo, actualizaciones de .NET, actualizaciones de la aplicación, etc.

- Cualquier configuración nueva que deba aplicarse se realiza en ese momento.

- Cualquier otro mantenimiento se realiza en ese momento.

- La imagen base se cierra.

- La imagen base se sella y se establece para volver al entorno de producción.

- Los usuarios pueden volver a iniciar sesión.

> [!NOTE]
> Windows 10 realiza un conjunto de tareas de mantenimiento automáticamente y de forma periódica. Existe una tarea programada que está configurada para ejecutarse a las 3:00 A.M. todos los días de forma predeterminada. Esta tarea programada realiza una lista de tareas, incluida la limpieza de Windows Update. Puedes ver todas las categorías de mantenimiento que tienen lugar automáticamente con este comando de PowerShell:
>
>```powershell
>Get-ScheduledTask | ? {$_.Settings.MaintenanceSettings}
>```
>

Uno de los desafíos de las VDI no persistentes, es que cuando un usuario cierra la sesión, se descarta casi toda la actividad del sistema operativo. El perfil y el estado del usuario pueden guardarse en una ubicación centralizada, pero la propia máquina virtual descarta casi todos los cambios realizados desde el último arranque. Por lo tanto, las optimizaciones destinadas a un equipo con Windows que guarda el estado de una sesión a la siguiente no son aplicables.

Dependiendo de la arquitectura de la VM de VDI, cosas como PreFetch y SuperFetch no van a ayudar en el proceso de una sesión a la siguiente, ya que todas las optimizaciones se descartan al reiniciar la VM. La indexación puede ser un desperdicio parcial de recursos, como lo sería cualquier optimización de disco, como una desfragmentación tradicional.

> [!NOTE]
> Si estás preparando una imagen con virtualización y, estás conectado a Internet durante el proceso de creación de la imagen, en el primer inicio de sesión debes posponer las actualizaciones de características en **Configuración**, **Windows Update**.

### <a name="to-sysprep-or-not-sysprep"></a>Sysprep o no Sysprep

Windows 10 tiene una funcionalidad integrada llamada [Herramienta de preparación del sistema](/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview) (a menudo abreviada como "Sysprep"). La herramienta Sysprep se usa para preparar para la duplicación una imagen de Windows 10 personalizada. El proceso de Sysprep garantiza que el sistema operativo resultante sea único y se pueda ejecutar en la producción.

Existen varias razones a favor y en contra de ejecutar Sysprep. En el caso de VDI, es posible que quieras personalizar el perfil de usuario predeterminado que se usará como plantilla de perfil para los usuarios subsiguientes que inicien sesión con esta imagen. También es posible que tengas aplicaciones que quieras instalar y que también quieras tener la posibilidad de controlar la configuración de cada aplicación.

La alternativa es usar un .ISO estándar para realizar la instalación, mediante un archivo de respuesta de instalación desatendida, y una secuencia de tareas para instalar aplicaciones o eliminarlas. También puedes usar una secuencia de tareas para establecer la configuración de la directiva local en la imagen; por ejemplo, puedes usar la herramienta [Utilidad Objeto de directiva de grupo local (LGPO)](/archive/blogs/secguide/lgpo-exe-local-group-policy-object-utility-v1-0).

### <a name="supportability"></a>Compatibilidad

Cada vez que se cambian los valores predeterminados de Windows, surgen preguntas sobre la compatibilidad. Una vez personalizada una imagen de VDI (VM o sesión), es necesario realizar un seguimiento de todos los cambios realizados en la imagen en un registro de cambios. Al solucionar problemas, a menudo se puede aislar una imagen en un grupo y configurarla para el análisis de problemas. Una vez realizado el seguimiento de un problema hasta la causa raíz, ese cambio se puede implementar primero en el entorno de prueba y, por último, en la carga de trabajo de producción.

En este documento se evita intencionadamente tocar los servicios del sistema, las directivas o las tareas que afectan a la seguridad. De ellos se encarga el mantenimiento de Windows. Se quita la capacidad de mantenimiento de las imágenes de VDI fuera de las ventanas de mantenimiento, ya que estas ventanas se dan cuando se producen la mayoría de los eventos de mantenimiento en entornos de VDI, *excepto para las actualizaciones de software de seguridad*. Microsoft ha publicado instrucciones sobre la seguridad de Windows en entornos de VDI. Para más información, consulta la [Guía de implementación del Antivirus de Windows Defender en un entorno de infraestructura de escritorio virtual (VDI)](/windows/security/threat-protection/windows-defender-antivirus/deployment-vdi-windows-defender-antivirus).

Ten en cuenta la compatibilidad al modificar la configuración predeterminada de Windows. Pueden surgir problemas difíciles al modificar servicios del sistema, directivas o tareas programadas, en nombre de la protección, "iluminación", etc. Consulta en Microsoft Knowledge Base los problemas conocidos actuales relacionados con la modificación de la configuración predeterminada. Las instrucciones de este documento y el script asociado en GitHub se conservarán con respecto a los problemas conocidos, en caso de que surjan. Además, los problemas se pueden notificar de varias maneras a Microsoft.

Puedes usar tu motor de búsqueda favorito con los términos ""start value" site:support.microsoft.com" para mostrar los problemas conocidos relacionados con los valores de inicio predeterminados de los servicios.

Ten en cuenta que este documento y los scripts asociados de GitHub no modifican los permisos predeterminados. Si estás interesado en aumentar la configuración de seguridad, comienza con el proyecto conocido como **AaronLocker**. Para más información, consulta [ANUNCIO: Inclusión de aplicaciones en la lista blanca con "AaronLocker"](/archive/blogs/aaron_margosis/announcing-application-whitelisting-with-aaronlocker).

#### <a name="vdi-optimization-categories"></a>Categorías de optimización de VDI

- Categorías globales de configuración del sistema operativo:

    - Limpieza de la aplicación para UWP

    - Limpieza de las características opcionales

    - Configuración de directivas locales

    - Servicios del sistema

    - Tareas programadas

    - Aplicación de actualizaciones de Windows (y otras)

    - Seguimientos de Windows automáticos

    - Liberar espacio en disco antes de finalizar (sellado) la imagen

    - Configuración de usuario

    - Configuración del host y del hipervisor

### <a name="universal-windows-platform-uwp-application-cleanup"></a>Limpieza de aplicaciones para la Plataforma universal de Windows (UWP)

Uno de los objetivos de una imagen de VDI es ser tan ligera como sea posible. Una forma de reducir el tamaño de la imagen es eliminar las aplicaciones para UWP que no se usarán en el entorno. Además de las aplicaciones para UWP, están los archivos principales de la aplicación, también conocidos como la carga útil. Existe una pequeña cantidad de datos almacenados en el perfil de cada usuario para la configuración específica de la aplicación. También hay una pequeña cantidad de datos en el perfil "Todos los usuarios".

La conectividad y el tiempo son factores importantes cuando se trata de la limpieza de la aplicación para UWP. Si implementas la imagen base en un dispositivo sin conectividad de red, Windows 10 no podrá conectarse a Microsoft Store, ni descargar aplicaciones o intentar instalarlas mientras, a su vez, intentas desinstalarlas. Puede ser una buena estrategia para tener tiempo de personalizar la imagen y luego actualizar lo que queda en una fase posterior del proceso de creación de la imagen.

Si modificas el archivo .WIM base que usas para instalar Windows 10 y eliminas las aplicaciones para UWP innecesarias de .WIM antes de realizar la instalación, esas aplicaciones no se instalarán; gracias a ello, los tiempos de creación de tu perfil serán más cortos. Más adelante en esta sección, encontrarás información sobre cómo eliminar aplicaciones para UWP del archivo .WIM de instalación.

Una buena estrategia para VDI es aprovisionar las aplicaciones que quieres en la imagen base y, a continuación, limitar o bloquear el acceso a Microsoft Store. Las aplicaciones de Store se actualizan periódicamente en segundo plano en equipos normales. Las aplicaciones para UWP pueden actualizarse durante la ventana de mantenimiento cuando se aplican otras actualizaciones. Para más información, consulta [Aplicaciones para la Plataforma universal de Windows](https://docs.citrix.com/citrix-virtual-apps-desktops/manage-deployment/applications-manage/universal-apps.html).

#### <a name="delete-the-payload-of-uwp-apps"></a>Eliminar la carga útil de las aplicaciones para UWP

Las aplicaciones para UWP que no son necesarias todavía están en el sistema de archivos y consumen una pequeña cantidad de espacio en disco. En cuanto a las aplicaciones que nunca serán necesarias, la carga útil de aplicaciones para UWP no deseadas se puede eliminar de la imagen base mediante los comandos de PowerShell.

De hecho, si los eliminas del archivo .WIM de instalación mediante los vínculos que se proporcionan más adelante en esta sección, podrás comenzar desde el principio con una lista muy reducida de aplicaciones para UWP.

Ejecuta el siguiente comando para enumerar las aplicaciones para UWP aprovisionadas desde un sistema operativo en ejecución, como en este ejemplo de salida truncado de PowerShell:

```powershell

    Get-AppxProvisionedPackage -Online

    DisplayName  : Microsoft.3DBuilder
    Version      : 13.0.10349.0  
    Architecture : neutral
    ResourceId   : \~ 
    PackageName  : Microsoft.3DBuilder_13.0.10349.0_neutral_\~_8wekyb3d8bbwe
    Regions      :
    ...
```

Las aplicaciones para UWP que se aprovisionan en un sistema pueden eliminarse durante la instalación del sistema operativo como parte de una secuencia de tareas, o más tarde después de que se instale ese sistema operativo. Este podría ser el mejor método, porque hace que el proceso general de creación o mantenimiento de una imagen sea modular. Una vez que desarrolles los scripts, si algo cambia en una compilación posterior, solo tienes que editar un script existente en lugar de repetir el proceso desde cero. A continuación te mostramos algunos vínculos de información sobre este tema:

[Removing Windows 10 in-box apps during a task sequence](/archive/blogs/mniehaus/removing-windows-10-in-box-apps-during-a-task-sequence) (Quitar aplicaciones de Windows 10 en el equipo durante una secuencia de tareas)

[Removing Built-in apps from Windows 10 WIM-File with Powershell - Version 1.3](https://gallery.technet.microsoft.com/Removing-Built-in-apps-65dc387b) (Quitar aplicaciones integradas del archivo .WIM de Windows 10 con Powershell - Versión 1.3)

[Windows 10 1607: Keeping apps from coming back when deploying the feature update](/archive/blogs/mniehaus/windows-10-1607-keeping-apps-from-coming-back-when-deploying-the-feature-update) (Evitar que las aplicaciones vuelvan a aparecer al implementar la actualización de características)

A continuación, ejecute el comando [Remove-AppxProvisionedPackage](/powershell/module/dism/remove-appxprovisionedpackage?view=win10-ps) de PowerShell para eliminar las cargas útiles de la aplicación UWP:

```powershell
Remove-AppxProvisionedPackage -Online -PackageName
```

Cada aplicación para UWP debe evaluarse para determinar su aplicabilidad en cada entorno único. Posiblemente quieras instalar una instalación predeterminada de Windows 10 1909, y luego observar qué aplicaciones se están ejecutando y cuáles están consumiendo memoria. Por ejemplo, es posible que quieras eliminar las aplicaciones que se inician automáticamente, o las aplicaciones que muestran automáticamente información en el menú Inicio (como Tiempo y Noticias), y que podrían no ser de utilidad en el entorno.

>[!NOTE]
>Si usas los scripts de GitHub, puedes controlar fácilmente qué aplicaciones se quitan antes de ejecutar el script. Después de descargar los archivos de script, busca el archivo "Win10_1909_AppxPackages.txt", edítalo y quita las entradas de las aplicaciones que quieres conservar, como Calculadora, Notas rápidas, etc.

### <a name="manage-windows-optional-features-using-powershell"></a>Administración de características opcionales de Windows mediante PowerShell

Puedes administrar las características opcionales de Windows mediante PowerShell. Para más información, consulta [Windows 10: Administración de características opcionales con PowerShell](https://social.technet.microsoft.com/wiki/contents/articles/39386.windows-10-managing-optional-features-with-powershell.aspx). Para enumerar las características de Windows instaladas actualmente, ejecuta el siguiente comando de PowerShell:

```powershell
Get-WindowsOptionalFeature -Online
```

Puedes habilitar o deshabilitar una característica opcional de Windows específica, tal como se muestra en este ejemplo:

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName "DirectPlay" -All
```

Puedes deshabilitar características de la imagen de VDI, tal como se muestra en este ejemplo:

```powershell
Disable-WindowsOptionalFeature -Online -FeatureName “WindowsMediaPlayer”
```

A continuación, puedes quitar el paquete de Reproductor de Windows Media. Hay dos paquetes de Reproductor de Windows Media en Windows 10 1909:

```powershell
Get-WindowsPackage -Online -PackageName *media*

PackageName       : Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.18362.1
Applicable        : True
Copyright        : Copyright (c) Microsoft Corporation. All Rights Reserved
Company         :
CreationTime       :
Description       : Play audio and video files on your local device and on the Internet.
InstallClient      : DISM Package Manager Provider
InstallPackageName    : Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.18362.1.mum
InstallTime       : 3/19/2019 6:20:22 AM
...

Features         : {}

PackageName       : Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.18362.449
Applicable        : True
Copyright        : Copyright (c) Microsoft Corporation. All Rights Reserved
Company         :
CreationTime       :
Description       : Play audio and video files on your local device and on the Internet.
InstallClient      : UpdateAgentLCU
InstallPackageName    : Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.18362.449.mum
InstallTime       : 10/29/2019 5:15:17 AM
...
```

Si quieres quitar el paquete de Reproductor de Windows Media (para liberar aproximadamente 60 MB de espacio en disco):

```powershell
 Remove-WindowsPackage -PackageName Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.18362.1 -Online

 Remove-WindowsPackage -PackageName Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.18362.1 -Online
```

#### <a name="enable-or-disable-windows-features-using-dism"></a>Habilitación o deshabilitación de características de Windows mediante DISM

Puedes usar la herramienta integrada Dism.exe para enumerar y controlar las características opcionales de Windows. Se puede desarrollar un script Dism.exe y ejecutarlo durante una secuencia de tareas de instalación del sistema operativo. La tecnología de Windows implicada se denomina [Funciones bajo demanda](/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities).

#### <a name="default-user-settings"></a>Configuración de usuario predeterminado

Hay personalizaciones que se pueden realizar en un archivo de registro de Windows denominado "C:\Usuarios\Default\NTUSER.DAT". Cualquier configuración realizada en este archivo se aplicará a los perfiles de usuario posteriores creados desde un dispositivo que ejecute esta imagen. Puedes controlar la configuración que se va a aplicar al perfil de usuario predeterminado; para ello, edita el archivo "Win10_1909_DefaultUserSettings.txt". Una configuración que debes examinar detenidamente, nueva en esta iteración de recomendaciones de configuración, es la denominada **TaskbarSmallIcons**. Es posible que quieras consultar la base de usuarios antes de implementar esta configuración. **TaskbarSmallIcons** hace que la barra de tareas de Windows sea más pequeña y consuma menos espacio de la pantalla, hace que los iconos sean más compactos y minimiza la interfaz de búsqueda. En las ilustraciones siguientes se representa antes y después:

Figura 1: Barra de tareas normal de Windows 10, versión 1909

![Versión estándar de la barra de tareas de Windows 10, versión 1909](media/rds-vdi-recommendations-1909/standard-taskbar.png)

Figura 2: Barra de tareas con la configuración de iconos pequeños

![Barra de tareas con la configuración de iconos pequeños](media/rds-vdi-recommendations-1909/taskbar-sm-icons.png)

Además, para reducir la transmisión de imágenes a través de la infraestructura de VDI, puedes establecer el fondo predeterminado en un color sólido, en lugar de la imagen predeterminada de Windows 10. También puedes establecer que la pantalla de inicio de sesión sea de color sólido, así como desactivar el efecto de desenfoque opaco en el inicio de sesión.

La siguiente configuración se aplica al subárbol del registro del perfil de usuario predeterminado, principalmente para reducir las animaciones. Si no quieres algunas o ninguna de estas configuraciones, elimina las que no se deben aplicar a los nuevos perfiles de usuario basados en esta imagen. El objetivo de esta configuración es habilitar la siguiente configuración equivalente:

Figura 3: Propiedades del sistema optimizado, opciones de rendimiento

![Propiedades del sistema optimizado, opciones de rendimiento](media/rds-vdi-recommendations-1909/performance-options.png)

A continuación, se muestra la configuración de optimización que se aplica al subárbol del registro del perfil de usuario predeterminado para optimizar el rendimiento:

```
Delete HKLM\Temp\SOFTWARE\Microsoft\Windows\CurrentVersion\Run /v OneDriveSetup /f
add "HKLM\Temp\Control Panel\Desktop" /v DragFullWindows /t REG_SZ /d 0 /f
add "HKLM\Temp\Control Panel\Desktop" /v WallPaper /t REG_SZ /d "" /f
add "HKLM\Temp\Control Panel\Desktop\WindowMetrics" /v MinAnimate /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\DWM /v AccentColor /t REG_DWORD /d 4292311040 /f
add HKLM\Temp\Software\Microsoft\Windows\DWM /v ColorizationColor /t REG_DWORD /d 4292311040 /f
add HKLM\Temp\Software\Microsoft\Windows\DWM /v AlwaysHibernateThumbnails /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\DWM /v EnableAeroPeek /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\DWM /v AlwaysHibernateThumbnails /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced /v AutoCheckSelect /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced /v HideIcons /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced /v ListviewAlphaSelect /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced /v ListViewShadow /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced /v ShowInfoTip /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced /v TaskbarAnimations /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced /v TaskbarSmallIcons /t REG_DWORD /d 1 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced\People /v PeopleBand /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\AnimateMinMax /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\ComboBoxAnimation /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\ControlAnimations /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\DWMAeroPeekEnabled /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\DWMSaveThumbnailEnabled /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\MenuAnimation /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\SelectionFade /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\TaskbarAnimations /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\TooltipAnimation /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager /v SubscribedContent-338388Enabled /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager /v SubscribedContent-338389Enabled /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager /v SystemPaneSuggestionsEnabled /t REG_DWORD /d 0 /f
```

En la configuración de la directiva local, puedes deshabilitar las imágenes para los fondos en VDI.  Si quieres imágenes, podrías crear imágenes de fondo personalizadas con una profundidad de color reducida para limitar el ancho de banda de red usado para transmitir la información de la imagen. Si decides no especificar ninguna imagen de fondo en la directiva local, es aconsejable establecer el color de fondo antes de la directiva local, ya que, una vez establecida la directiva, el usuario no tiene forma de cambiar el color de fondo. Podría ser mejor especificar "(null)" como imagen de fondo. Existe otra configuración de directiva en la sección siguiente sobre no usar el fondo en sesiones de Protocolo de escritorio remoto.

### <a name="local-policy-settings"></a>Configuración de directivas locales

Se pueden crear muchas optimizaciones para Windows 10 en un entorno VDI mediante la directiva de Windows. Las configuraciones que se indican en la tabla de esta sección se pueden aplicar localmente a la imagen base o gold. Si la configuración equivalente no se especifica de ninguna otra manera como, por ejemplo, mediante una directiva de grupo, la configuración seguirá aplicándose.

Algunas decisiones pueden basarse en los aspectos específicos del entorno, por ejemplo:

- ¿El entorno de VDI puede obtener acceso a Internet?

- ¿La solución VDI es persistente o no persistente?

Las siguientes configuraciones se eligieron para no contrarrestar ni entrar en conflicto con ninguna configuración que tenga algo que ver con la seguridad. Estas configuraciones se eligieron para eliminar las configuraciones o deshabilitar la funcionalidad que podrían no ser aplicables en los entornos de VDI.

| Configuración de directiva | Elemento | Elemento secundario | Comentarios y configuración posibles|
| -------------- | ---- | -------- | ---------------------------- |
| Directiva de equipo local\\Configuración del equipo\\Configuración de Windows\\Configuración de seguridad | | | |
| Directivas de Administrador de listas de redes | Propiedades de todas las redes | Ubicación de red | El usuario no puede cambiar la ubicación. |
| Directiva de equipo local\\Configuración del equipo\\Plantillas administrativas\\Panel de control | | | |
| *Panel de control | Permitir sugerencias en línea | | Deshabilitado. La configuración no se pondrá en contacto con los servicios de contenido de Microsoft para recuperar sugerencias y contenido de ayuda. |
| *Panel de control\Personalización | No mostrar la pantalla de bloqueo | Habilitado. Esta configuración controla si aparece la pantalla de bloqueo para los usuarios. Si habilitas esta configuración de directiva, los usuarios que no tienen que presionar CTRL+ALT+SUPR antes de iniciar sesión verán el mosaico seleccionado después de bloquear su equipo. |
| *Panel de control\Personalización | Aplicar una pantalla de bloqueo predeterminada específica y una imagen de inicio de sesión | [![Interfaz de usuario para establecer la ruta de acceso a la pantalla de bloqueo](media/lock-screen-image-settings.png)](media/lock-screen-image-settings.png) | Habilitado. Esta configuración te permite especificar la pantalla de bloqueo predeterminada y la imagen de inicio de sesión que se muestra cuando ningún usuario ha iniciado sesión; también establece la imagen especificada como predeterminada para todos los usuarios y reemplaza la imagen predeterminada.<p>Recomendamos usar una imagen de baja resolución y no compleja para que se tengan que transmitir menos datos a través de la red cada vez que se representa la imagen. |
| *Panel de control\Configuración regional y de idioma\Personalización de escritura a mano | Desactivar el aprendizaje automático | | Habilitado. Si habilitas esta configuración de directiva, el aprendizaje automático se detiene y se eliminan todos los datos almacenados. Los usuarios no pueden configurar esta configuración en el Panel de control. |
| Directiva de equipo local\\Configuración del equipo\\Plantillas administrativas\\Red | | | |
| Servicio de transferencia inteligente en segundo plano (BITS) | No permitir que el cliente de BITS use Windows BranchCache |  | Habilitada |
| Servicio de transferencia inteligente en segundo plano (BITS) | No permitir que el equipo actúe como un cliente de almacenamiento en caché de sistemas de mismo nivel de BITS |  | Habilitada |
| Servicio de transferencia inteligente en segundo plano (BITS) | No permitir que el equipo actúe como un servidor de almacenamiento en caché de sistemas de mismo nivel de BITS |  | Habilitada |
| Servicio de transferencia inteligente en segundo plano (BITS) | Permitir el almacenamiento en caché de sistemas de mismo nivel de BITS |  | Deshabilitada |
| BranchCache | Activar BranchCache |  | Deshabilitada |
| *Fuentes | Habilitar Proveedores de fuentes |  | Deshabilitado. Windows no se conecta a un proveedor de fuentes en línea y solo enumera las fuentes instaladas localmente. |
| Autenticación de zona con cobertura inalámbrica | Habilitar la autenticación del punto de acceso |  | Deshabilitada |
| Servicios de redes de igual a igual de Microsoft | Desactivar los Servicios de redes de igual a igual de Microsoft |  | Habilitada |
| Indicador de estado de conectividad de red | Especificar sondeo pasivo | Deshabilitar sondeo pasivo (casilla) | Habilitado. Usa esta configuración si estás en una red aislada o si usas direcciones IP estáticas. |
| Archivos sin conexión | Permitir o denegar el uso de archivos sin conexión |  | Deshabilitada |
| Configuración de TCPIP\\Tecnologías de transición IPv6 | Establecer estado de Teredo | Estado deshabilitado | Habilitado. En el estado deshabilitado, no hay interfaces de Teredo presentes en el host. |
| Servicio WLAN\\Configuración de WLAN | Permitir que Windows se conecte automáticamente a zonas con cobertura inalámbrica sugeridas, a redes que los contactos comparten y a zonas con cobertura inalámbrica que ofrecen servicios de pago |  | Deshabilitado. Las opciones **Conectarse a zonas con cobertura inalámbrica abiertas sugeridas**, **Conectarse a las redes que compartan mis contactos** y **Enable paid services** (Habilitar servicios de pago) están desactivadas, pero los usuarios de este dispositivo pueden habilitarlas. |
| Directiva de equipo local\\Configuración del equipo\\Plantillas administrativas\\Menú Inicio y barra de tareas |  |  |  |
| *Notificaciones | Desactivar el uso de la red para notificaciones |  | Habilitado. Si habilitas esta configuración, las aplicaciones y características del sistema no podrán recibir notificaciones de la red de WNS o mediante las API de sondeo de notificaciones. |
| Directiva de equipo local\\Configuración del equipo\\Plantillas administrativas\\Sistema |  |  |  |
| Instalación de dispositivos | No enviar un informe de error de Windows cuando se instale un controlador genérico en un dispositivo |  | Habilitada |
| Instalación de dispositivos | Impedir la creación de un punto de restauración del sistema cuando se produzca una actividad de dispositivo que normalmente pediría la creación de un punto de restauración |  | Habilitada |
| Instalación de dispositivos | Impedir la recuperación de metadatos de dispositivo desde Internet |  | Habilitada |
| Instalación de dispositivos | Impedir que Windows envíe un informe de error cuando un controlador de dispositivo solicite software adicional durante su instalación |  | Habilitada |
| Instalación de dispositivos | Desactivar los globos de **Nuevo hardware encontrado** durante la instalación de los dispositivos |  | Habilitada |
| Sistema de archivos\\NTFS | Opciones de creación de nombre corto | Deshabilitada en todos los volúmenes | Habilitada |
| *Directiva de grupo | Configurar la vinculación de un sitio web con la aplicación mediante los controladores de URI de aplicación. |  | Deshabilitado. Se desactiva la vinculación de un sitio web con la aplicación, y las URI http(s) se abren en el navegador predeterminado en lugar de iniciar la aplicación asociada. |
| *Directiva de grupo | Continuar las experiencias en este dispositivo |  | Deshabilitado. Otros dispositivos no pueden detectar el dispositivo de Windows y no puede participar en experiencias entre dispositivos. |
| Administración de comunicaciones de Internet\\Configuración de comunicaciones de Internet | Desactivar el acceso a todas las características de Windows Update |  |Habilitado. Si habilitas esta configuración de directiva, se eliminan todas las características de Windows Update. Esto incluye bloquear el acceso al sitio web de Windows Update en https://windowsupdate.microsoft.com, desde el hipervínculo de Windows Update en el menú Inicio y también en el menú Herramientas de Internet Explorer. La actualización automática de Windows también está deshabilitada; no se te notificarán ni recibirás las actualizaciones críticas de Windows Update. Igualmente, esta configuración de directiva también evita que el Administrador de dispositivos instale automáticamente las actualizaciones de controladores desde el sitio web de Windows Update. |
| Administración de comunicaciones de Internet\\Configuración de comunicaciones de Internet | Desactivar la actualización automática de certificados raíz |  |Habilitado. Si habilitas esta configuración de directiva, cuando se recibas un certificado que haya emitido una entidad de certificación raíz no confiable, tu equipo no se pondrá en contacto con el sitio web de Windows Update para ver si Microsoft ha agregado la CA a su lista de entidad de confianza. NOTA: Usa esta directiva solo si tienes un medio alternativo a la última lista de revocación de certificados. |
| Administración de comunicaciones de Internet\\Configuración de comunicaciones de Internet | Desactivar los vínculos "Events.asp" del Visor de eventos |  | Habilitada |
| Administración de comunicaciones de Internet\\Configuración de comunicaciones de Internet | Desactivar uso compartido de datos de personalización de escritura a mano |  | Habilitada |
| Administración de comunicaciones de Internet\\Configuración de comunicaciones de Internet | Desactivar informe de errores de reconocimiento de escritura a mano |  | Habilitada |
| Administración de comunicaciones de Internet\\Configuración de comunicaciones de Internet | Desactivar el contenido "¿Sabía que...?" del Centro de ayuda y soporte técnico contenido |  | Habilitada |
| Administración de comunicaciones de Internet\\Configuración de comunicaciones de Internet | Desactivar la búsqueda en Microsoft Knowledge Base del Centro de ayuda y soporte técnico |  | Habilitada |
| Administración de comunicaciones de Internet\\Configuración de comunicaciones de Internet | Desactivar al Asistente para la conexión a Internet si la conexión de direcciones URL hace referencia a Microsoft.com |  | Habilitada |
| Administración de comunicaciones de Internet\\Configuración de comunicaciones de Internet | Desactivar descargas de Internet de los asistentes para la publicación en Web y pedidos en línea |  | Habilitada |
| Administración de comunicaciones de Internet\\Configuración de comunicaciones de Internet | Desactivar el servicio de asociaciones de archivo de Internet |  | Habilitada |
| Administración de comunicaciones de Internet\\Configuración de comunicaciones de Internet | Desactivar el registro si la conexión de direcciones URL hace referencia a Microsoft.com |  | Habilitada |
| Administración de comunicaciones de Internet\\Configuración de comunicaciones de Internet | Desactivar la tarea de imágenes "Pedir copias fotográficas" |  | Habilitada |
| Administración de comunicaciones de Internet\\Configuración de comunicaciones de Internet | Desactivar la tarea "Publicar en Web" para archivos y carpetas |  | Habilitada |
| Administración de comunicaciones de Internet\\Configuración de comunicaciones de Internet | Desactivar el Programa para la mejora de la experiencia del usuario de Windows Messenger |  | Habilitada |
| Administración de comunicaciones de Internet\\Configuración de comunicaciones de Internet | Desactivar el Programa para la mejora de la experiencia del usuario de Windows |  | Habilitada |
| Administración de comunicaciones de Internet\\Configuración de comunicaciones de Internet | Desactivar pruebas activas del indicador de estado de conectividad de la red de Windows |  | Habilitado. Esta configuración de directiva desactiva las pruebas activas que realiza el indicador de estado de conectividad de red de Windows (NCSI) para determinar si tu equipo está conectado a Internet o a una red más limitada. Como parte del proceso para determinar el nivel de conectividad, NCSI realiza una de dos pruebas activas: descargar una página desde un servidor web dedicado o realizar una solicitud de DNS para una dirección dedicada. Si habilitas esta configuración de directiva, NCSI no ejecuta ninguna de las dos pruebas activas. Esto podría reducir la capacidad de NCSI y de otros componentes que utilizan NCSI para determinar el acceso a Internet) NOTA: Existen otras directivas que le permiten redirigir las pruebas NCSI a los recursos internos, si quieres usar esta funcionalidad. |
| Administración de comunicaciones de Internet\\Configuración de comunicaciones de Internet | Desactivar el informe de errores de Windows |  | Habilitada |
| Administración de comunicaciones de Internet\\Configuración de comunicaciones de Internet | Desactivar la búsqueda de controladores de dispositivo en Windows Update |  | Habilitada |
| Iniciar sesión | Mostrar animación de primer inicio de sesión |  | Deshabilitada |
| Iniciar sesión | Desactivar las notificaciones de aplicaciones en la pantalla de bloqueo |  | Habilitada |
| Iniciar sesión | Desactivar el sonido de Inicio de Windows |  | Habilitada |
| Administración de energía | Seleccionar un plan de energía activo | Alto rendimiento | Habilitada |
| Recuperación | Permitir la restauración del sistema al estado predeterminado |  | Deshabilitada |
| *Estado de almacenamiento | Permitir la descarga de actualizaciones en el modelo de predicción de error de disco |  | Deshabilitado. No se descargan las actualizaciones para el modelo de error de predicción de errores de disco. |
| *Servicios de hora de Windows\\Proveedores de hora | Habilitar el cliente de Windows NTP |  | Deshabilitado. Si deshabilitas o no configuras esta configuración de directiva, el reloj del equipo local no se sincroniza con la hora de los servidores NTP. NOTA: Sopesa esta configuración con cuidado. Los dispositivos de Windows que están unidos a un dominio deben usar **NT5DS**. El controlador de dominio correspondiente al controlador de dominio principal debe usar NTP. El rol de PDCe puede usar NTP. Las máquinas virtuales a veces usan "mejoras" o "servicios de integración". |
| Solución de problemas y diagnósticos\\Mantenimiento programado | Configurar el comportamiento del mantenimiento programado |   | Deshabilitada |
| Solución de problemas y diagnósticos\\Diagnósticos de rendimiento del arranque de Windows | Configurar el nivel de ejecución del escenario |   | Deshabilitada |
| Solución de problemas y diagnósticos\\Diagnóstico de pérdida de memoria de Windows | Configurar el nivel de ejecución del escenario |   | Deshabilitada |
| Solución de problemas y diagnósticos\\Detección y resolución del agotamiento de recursos de Windows | Configurar el nivel de ejecución del escenario |   | Deshabilitada |
| Solución de problemas y diagnósticos\\Diagnóstico de rendimiento de apagado de Windows | Configurar el nivel de ejecución del escenario |   | Deshabilitada |
| Solución de problemas y diagnósticos\\Diagnóstico de rendimiento de espera/reanudación de Windows | Configurar el nivel de ejecución del escenario |   | Deshabilitada |
| Solución de problemas y diagnósticos\\Diagnóstico de rendimiento de respuesta del sistema de Windows | Configurar el nivel de ejecución del escenario |   | Deshabilitada |
| *Perfiles de usuario | Desactivar el identificador de publicidad |   | Habilitado. Si habilitas esta configuración de directiva, el id. de publicidad se desactiva. Las aplicaciones no pueden usar el id. para experiencias entre aplicaciones. |
| Directiva de equipo local\\Configuración del equipo\\Plantillas administrativas\\Componentes de Windows |  |  |  |
| Agregar características a Windows 10 | Impedir que el asistente se ejecute |  | Habilitada |
| *Privacidad de la aplicación | Impedir que el asistente se ejecute |  | Habilitada |
| *Privacidad de la aplicación | Permitir que las aplicaciones de Windows accedan a la información de la cuenta | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado. Si eliges la opción **Forzar denegación**, las aplicaciones de Windows no podrán obtener acceso a la información de la cuenta y los empleados de tu organización no podrán cambiarlo. |
| *Privacidad de la aplicación | Permitir que las aplicaciones de Windows accedan al historial de llamadas | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado. Si eliges la opción **Forzar denegación**, las aplicaciones de Windows no podrán obtener acceso al historial de llamadas y los empleados de tu organización no podrán cambiarlo. |
| *Privacidad de la aplicación | Permitir que las aplicaciones de Windows accedan a los contactos | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado. Si eliges la opción **Forzar denegación**, las aplicaciones de Windows no podrán obtener acceso a los contactos y los empleados de tu organización no podrán cambiarlo. |
| *Privacidad de la aplicación | Permitir que las aplicaciones de Windows obtengan acceso a la información de otras aplicaciones | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado. Si deshabilitas o no configuras esta configuración de directiva, los empleados de tu organización podrán decidir si las aplicaciones de Windows pueden obtener información de diagnóstico sobre otras aplicaciones mediante Configuración > Privacidad en el dispositivo. |
| *Privacidad de la aplicación | Permitir que las aplicaciones Windows accedan al correo electrónico | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado. Si eliges la opción **Forzar permiso**, las aplicaciones de Windows podrán obtener acceso al correo electrónico y los empleados de tu organización no podrán cambiarlo. |
| *Privacidad de la aplicación | Permitir que las aplicaciones de Windows accedan a la ubicación | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado. Si eliges la opción **Forzar denegación**, las aplicaciones de Windows no podrán obtener acceso a la ubicación y los empleados de tu organización no podrán cambiarlo. |
| *Privacidad de la aplicación | Permitir que las aplicaciones de Windows accedan a la mensajería | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado. Si eliges la opción **Forzar denegación**, las aplicaciones de Windows no podrán obtener acceso a los mensajes y los empleados de tu organización no podrán cambiarlo. |
| *Privacidad de la aplicación | Permitir que las aplicaciones Windows accedan al movimiento | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado. Si eliges la opción **Forzar denegación**, las aplicaciones de Windows no podrán obtener acceso a los datos de movimiento y los empleados de tu organización no podrán cambiarlo. |
| *Privacidad de la aplicación | Permitir que las aplicaciones Windows accedan a las notificaciones | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado. Si eliges la opción **Forzar denegación**, las aplicaciones de Windows no podrán obtener acceso a las notificaciones y los empleados de tu organización no podrán cambiarlo. |
| *Privacidad de la aplicación | Permitir que las aplicaciones Windows accedan a las tareas | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado. Si eliges la opción **Forzar denegación**, las aplicaciones de Windows no podrán obtener acceso a las tareas y los empleados de tu organización no podrán cambiarlo. |
| *Privacidad de la aplicación | Permitir que las aplicaciones de Windows accedan al calendario | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado. Si eliges la opción **Forzar denegación**, las aplicaciones de Windows no podrán obtener acceso al calendario y los empleados de tu organización no podrán cambiarlo. |
| *Privacidad de la aplicación | Permitir que las aplicaciones de Windows accedan a la cámara | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado. Si eliges la opción **Forzar denegación**, las aplicaciones de Windows no podrán obtener acceso a la cámara y los empleados de tu organización no podrán cambiarlo. |
| *Privacidad de la aplicación | Permitir que las aplicaciones de Windows accedan al micrófono | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado. Si eliges la opción **Forzar denegación**, las aplicaciones de Windows no podrán obtener acceso al micrófono y los empleados de tu organización no podrán cambiarlo. |
| *Privacidad de la aplicación | Permitir que las aplicaciones de Windows accedan a los dispositivos de confianza | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado. Si eliges la opción **Forzar denegación**, las aplicaciones de Windows no podrán obtener acceso a los dispositivos de confianza y los empleados de tu organización no podrán cambiarlo. |
| *Privacidad de la aplicación | Permitir que las aplicaciones de Windows se comuniquen con dispositivos no emparejados | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado. Si eliges la opción **Forzar denegación**, las aplicaciones de Windows no podrán comunicarse con dispositivos inalámbricos no emparejados y los empleados de tu organización no podrán cambiarlo. |
| *Privacidad de la aplicación | Permitir que las aplicaciones Windows accedan a las radios | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado. Si eliges la opción **Forzar denegación**, las aplicaciones de Windows no podrán obtener acceso al control de radios y los empleados de tu organización no podrán cambiarlo. |
| *Privacidad de la aplicación | Permitir que las aplicaciones de Windows hagan llamadas telefónicas | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado. Si eliges la opción **Forzar denegación**, las aplicaciones de Windows no podrán realizar llamadas telefónicas y los empleados de tu organización no podrán cambiarlo. |
| *Privacidad de la aplicación | Permitir que las aplicaciones de Windows se ejecuten en segundo plano | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado. Si eliges la opción **Forzar denegación**, las aplicaciones de Windows no podrán ejecutarse en segundo plano y los empleados de tu organización no podrán cambiarlo. |
| Directivas de reproducción automática | Establece el comportamiento predeterminado para ejecución automática. | No ejecutes ningún comando de ejecución automática | Habilitada |
| *Directivas de reproducción automática | Desactivar la reproducción automática |   | Habilitado. Si habilitas esta configuración de directiva, la reproducción automática se deshabilita en las unidades de CD-ROM y en los medios extraíbles, o se deshabilita en todas las unidades. |
| *Contenido de la nube | No mostrar sugerencias de Windows | Habilitado. Esta configuración de directiva evita que las sugerencias de Windows se muestren a los usuarios. |
| *Contenido de la nube | Desactivar las experiencias del consumidor de Microsoft | Habilitado. Si habilitas esta configuración de directiva, los usuarios ya no verán las recomendaciones personalizadas de Microsoft ni las notificaciones sobre su cuenta Microsoft. |
| *Recopilación de datos y versiones preliminares | Permitir telemetría | 0: seguridad [solo Enterprise] | Habilitado. La configuración de un valor 0 se aplica solo a los dispositivos que ejecutan las versiones Enterprise, Education, IoT o Windows Server. |
| *Recopilación de datos y versiones preliminares | No mostrar notificaciones de comentarios |  | Habilitada |
| *Recopilación de datos y versiones preliminares | Alternar control de usuario para compilaciones de Insider  |  | Deshabilitada |
| Optimización de distribución | Modo de descarga | Modo de descarga: Simple (99) | 99 = Modo de descarga simple sin emparejamiento. La Optimización de distribución se descarga solo mediante HTTP y no intenta ponerse en contacto con los servicios en la nube de la Optimización de distribución. |
| Administrador de ventanas de escritorio |  No permitir la invocación de Flip 3D |  | Habilitada |
| Administrador de ventanas de escritorio |  No permitir animaciones de ventanas |  | Habilitada |
| Administrador de ventanas de escritorio |  Usar un color sólido para el fondo de Inicio |  | Habilitada |
| Interfaz del usuario perimetral |  Permitir deslizamiento rápido desde borde |  | Deshabilitada |
| Interfaz del usuario perimetral |  Deshabilitar sugerencias de ayuda |  | Habilitada |
| Interfaz del usuario perimetral | Desactivar el seguimiento del uso de la aplicación |  | Habilitada |
| *Explorador de archivos |  Configurar SmartScreen de Microsoft Defender |  | Deshabilitado. SmartScreen se desactivará para todos los usuarios. No se advertirá a los usuarios si intentan ejecutar aplicaciones sospechosas de Internet. NOTA: Si no estás conectado a Internet, evitarás que los equipos intenten comunicarse con Microsoft para obtener información de SmartScreen. |
| Explorador de archivos |  No mostrar la notificación de la **nueva aplicación instalada** |  | Habilitada |
| *Encontrar mi dispositivo |  Activar o desactivar Encontrar mi dispositivo |  | Deshabilitado. Cuando la opción Encontrar mi dispositivo está desactivada, el dispositivo y su ubicación no se registran y la característica no funcionará. El usuario tampoco podrá ver la ubicación del último uso de su digitalizador activo en el dispositivo. |
| Explorador de archivos | Desactivar el almacenamiento en caché de imágenes en miniatura |  | Habilitada |
| Explorador de archivos | Desactivar la visualización de las entradas de búsqueda recientes en el cuadro de búsqueda del Explorador de archivos |  | Habilitada |
| Explorador de archivos | Desactivar almacenamiento en caché de vistas en miniatura en archivos thumbs.db ocultos |  | Habilitada |
| Explorador de juegos | Desactivar la descarga de información sobre juegos |  | Habilitada |
| Explorador de juegos | Desactivar la actualización de juegos |  | Habilitada |
| Explorador de juegos | Desactivar el seguimiento del último tiempo de juego en la carpeta Juegos |  | Habilitada |
| Grupo en el hogar | Impedir que el equipo se una a un grupo en el hogar |  | Habilitada |
| *Internet Explorer | Permitir que los servicios Microsoft proporcionen sugerencias mejoradas mientras el usuario escribe en la barra de direcciones |  | Deshabilitado. Los usuarios no recibirán sugerencias mejoradas mientras escriben en la barra de direcciones. Además, los usuarios no podrán cambiar la configuración de Sugerencias. |
| Internet Explorer | Deshabilitar la comprobación periódica de actualizaciones de software de Internet Explorer |  | Habilitada |
| Internet Explorer | Deshabilitar pantalla de presentación |  | Habilitada |
| Internet Explorer | Instalar nuevas versiones de Internet Explorer automáticamente |  | Deshabilitada |
| Internet Explorer | Impedir la participación en el Programa para la mejora de la experiencia del usuario |  | Habilitada |
| Internet Explorer | Evitar la ejecución del Asistente de primera ejecución | Ve directamente a la página principal | Habilitada |
| Internet Explorer | Establecer el crecimiento del proceso de pestañas | Baja | Habilitada |
| Internet Explorer | Especificar el comportamiento predeterminado para una nueva pestaña | Página de la nueva pestaña | Habilitada |
| Internet Explorer | Desactivar las notificaciones de rendimiento de complementos |  | Habilitada |
| *Internet Explorer | Desactivar la característica Autocompletar para direcciones web |  | Habilitado. Si habilitas esta configuración de directiva, no se sugerirán coincidencias al usuario cuando escriba direcciones web. El usuario no podrá cambiar el autocompletado para configurar las direcciones web. |
| *Internet Explorer | Desactivar geoubicación del explorador |  | Habilitado. Si habilitas esta configuración de directiva, la compatibilidad de geolocalización del navegador se desactivará. |
| *Internet Explorer | Desactivar Volver a abrir la última sesión de Exploración |  | Habilitada |
| Internet Explorer | Desactivar Volver a abrir la última sesión de Exploración |  | Habilitada |
| *Internet Explorer | Activar los sitios sugeridos |  | Deshabilitado. Si deshabilitas esta configuración de directiva, los puntos de entrada y la funcionalidad asociados con esta característica se desactivarán. |
| *Internet Explorer\\Vista de compatibilidad | Desactivar la vista de compatibilidad |  | Habilitado. Si habilitas esta configuración de directiva, el usuario no puede usar el botón Vista de compatibilidad ni administrar la lista de sitios de la Vista de compatibilidad. |
| Internet Explorer\\Panel de control de Internet\\Página de opciones avanzadas | Reproducir animaciones en páginas web |  | Deshabilitada |
| Internet Explorer\\Panel de control de Internet\\Página de opciones avanzadas | Reproducir vídeos en páginas web |  | Deshabilitada |
| Internet Explorer\\Panel de control de Internet\\Página de opciones avanzadas | Desactivar las características correspondientes a Pasar de página con predicción de página |  | Habilitado. Microsoft recopila tu historial de exploración para mejorar el funcionamiento de Pasar de página con predicción de página. Esta característica no está disponible para Internet Explorer para el escritorio. Si habilitas esta configuración de directiva, se desactiva Activar Pasar de página con predicción de página y la siguiente página web no se carga en segundo plano. |
| Internet Explorer\\Configuración de Internet\\Configuración avanzada\\Exploración | Desactivar detección de número de teléfono |  | Habilitada |
| *Ubicación y sensores | Desactivar la ubicación |  | Habilitado. Si habilitas esta configuración de directiva, la característica de ubicación se desactiva y se impide a todos los programas del equipo usar la información de ubicación de la característica de ubicación. |
| Ubicación y sensores | Desactivar sensores |  | Habilitada |
| Ubicación y sensores\\Servicio de localización de Windows | Desactivar el Servicio de localización de Windows |  | Habilitada |
| *Mapas | Desactivar la descarga y actualización automáticas de los datos de mapa |  | Habilitado. Si habilitas esta configuración, la descarga y actualización automáticas de los datos del mapa se desactivarán. |
| *Mapas | Desactivar el tráfico de red no solicitado en la página Configuración de mapas sin conexión |  | Habilitado. Si habilitas esta configuración de directiva, las características que generan tráfico de red en la página de configuración de Mapas sin conexión se desactivarán. Nota: Ten en cuenta que esto podría desactivar toda la página de configuración. |
| *Mensajes | Permitir la sincronización en la nube del servicio de mensajes |  | Deshabilitado. Esta configuración de directiva permite la copia de seguridad y la restauración de los mensajes de texto de móviles en los servicios en la nube de Microsoft. |
| *Microsoft Edge | Permitir sugerencias en la lista desplegable de la barra de direcciones |  | Deshabilitada |
| *Microsoft Edge | Permitir actualizaciones de configuración para la biblioteca de libros |  | Deshabilitado. Desactiva las listas de compatibilidad en Microsoft Edge. |
| *Microsoft Edge | Permitir la lista de compatibilidad de Microsoft |  | Deshabilitado. Si deshabilitas esta configuración, la lista de compatibilidad de Microsoft no se usa durante la navegación del explorador. |
| *Microsoft Edge | Permitir contenido web en la página Nueva pestaña |  | Deshabilitado. Indica que Microsoft Edge se abra con contenido en blanco cuando se abre una nueva pestaña. |
| *Microsoft Edge | Configurar Autorrellenar |  | Deshabilitado. Deshabilita la función de autorrellenar en la barra de direcciones. |
| *Microsoft Edge | Configurar No realizar seguimiento |  | Habilitado. Si habilitas esta configuración, siempre se enviarán solicitudes No realizar seguimiento a los sitios web que piden información de seguimiento. |
| *Microsoft Edge | Configurar el Administrador de contraseñas |  | Deshabilitado. Si deshabilitas esta configuración, los empleados no podrán usar el Administrador de contraseñas para guardar sus contraseñas localmente. |
| *Microsoft Edge | Configurar sugerencias de búsqueda en la barra de direcciones |  | Deshabilitado. Los usuarios no pueden ver sugerencias de búsqueda en la barra de direcciones de Microsoft Edge. |
| *Microsoft Edge | Configurar las páginas de inicio |  | Habilitado. Si habilitas esta configuración, puedes configurar una o varias páginas de inicio. Si esta configuración está habilitada, también debes incluir direcciones URL para las páginas y separar varias páginas con corchetes angulares con este formato: <support.contoso.com><support.microsoft.com> Windows 10, versión 1703 o posterior: Si no quieres enviar tráfico a Microsoft, puedes usar el valor <about:blank>, cuando es la única URL configurada, porque se admite en los dispositivos sin importar si están unidos a un dominio o no. |
| *Microsoft Edge | Configurar SmartScreen de Microsoft Defender |  | Deshabilitado. SmartScreen de Windows Defender está desactivado y los empleados no pueden activarlo. NOTA: Ten a en cuenta esta configuración dentro del entorno. Si no estás conectado a Internet, evitarás que los equipos intenten comunicarse con Microsoft para obtener información de SmartScreen. |
| *Microsoft Edge | Evitar que la página web de primera ejecución se abra en Microsoft Edge |  | Habilitado. Los usuarios no verán la página de primera ejecución al abrir Microsoft Edge por primera vez. |
| OneDrive | Impedir que OneDrive genere tráfico de red hasta que el usuario inicie sesión en OneDrive |  | Habilitado. Habilita esta configuración para evitar que el cliente de sincronización de OneDrive (OneDrive.exe) genere tráfico en la red (comprobación de actualizaciones, etc.) hasta que el usuario inicie sesión en OneDrive o comience a sincronizar archivos en el equipo local. |
| *OneDrive | Impedir el uso de OneDrive para almacenar archivos |  | Habilitado. A menos que OneDrive se use local o no localmente. |
| OneDrive | Guardar documentos en OneDrive de forma predeterminada |  | Deshabilitado. A menos que OneDrive se use local o no localmente. |
| Fuentes RSS | Impedir detección automática de fuentes y Web Slices |  | Habilitada |
|*Fuentes RSS | Desactivar la sincronización en segundo plano de fuentes y Web Slices |  | Habilitado. Si habilitas esta configuración de directiva, la capacidad de sincronizar fuentes e instancias de Web Slice en segundo plano se desactiva. |
|*Buscar | Permitir Cortana |  | Deshabilitado. Cuando Cortana está desactivada, los usuarios aún podrán usar la opción de búsqueda para encontrar cosas en el dispositivo. |
|Buscar | Permitir usar Cortana sobre la pantalla de bloqueo |   | Deshabilitada |
|*Buscar | Permitir el uso de la ubicación para las búsquedas y Cortana |  | Deshabilitada |
|Buscar | No permitir búsquedas en la Web |   | Habilitada |
|*Buscar | No buscar en Internet ni mostrar resultados de Internet en la búsqueda |  | Habilitado. Si habilitas esta configuración de directiva, las consultas no se realizarán en Internet y los resultados web no se mostrarán cuando un usuario realice una consulta con la opción Buscar. |
|Buscar | Impedir la adición de ubicaciones UNC al índice desde el Panel de control |  | Habilitada |
|Buscar | Impedir la indización de archivos en la memoria caché de archivos sin conexión |  | Habilitada |
|*Buscar | Definir qué información se comparte en la búsqueda de información anónima |  | Habilitado. Comparte la información de uso, pero no el historial de búsqueda, la información de la cuenta de Microsoft o la ubicación específica. |
|*Plataforma de protección de software | Desactivar la validación de AVC en línea del cliente KMS | Habilitado. Habilitar esta configuración evita que este equipo envíe datos a Microsoft relativos a su estado de activación. |
|*Voz | Permitir la actualización automática de los datos de voz |  | Deshabilitado. No comprobará periódicamente los modelos de voz actualizados. |
|*Store | Desactivar la descarga e instalación automáticas de las actualizaciones |  | Habilitado. Si habilitas esta configuración, la descarga e instalación automáticas de las actualizaciones de aplicaciones se desactivarán. |
|*Store | Desactivar la descarga automáticas de las actualizaciones en dispositivos de Win8 | Habilitado. Si habilitas esta configuración, la descarga automática de las actualizaciones de aplicaciones se desactivará. |
| Tienda | Desactivar la oferta para actualizar a la versión de Windows más reciente |  | Habilitada |
|*Sincronizar la configuración | No sincronizar | Permitir a los usuarios activar la sincronización (no seleccionado) | Habilitado. Si habilitas esta configuración de directiva, la opción "sincronizar la configuración" se desactivará, y ninguno de los grupos de "sincronizar la configuración" se sincronizarán en este dispositivo. |
| Entrada de texto | Mejorar el reconocimiento de la entrada manuscrita y escritura por teclado |  | Deshabilitada |
| Antivirus de Windows Defender\\MAPS | Unirse a Microsoft MAPS |  | Deshabilitado. Si deshabilitas o no configuras esta opción, no te unirás a Microsoft MAPS. |
| Antivirus de Windows Defender\\MAPS | Enviar muestras de archivos cuando se requieran más análisis | No enviar nunca | Habilitado. Solo si no participas en la opción de envío de los datos de diagnóstico de MAPS. |
| Antivirus de Windows Defender\\Generación de informes | Desactivar notificaciones mejoradas |  | Habilitado. Si habilitas esta configuración, las notificaciones mejoradas del Antivirus de Windows Defender no se mostrarán en los clientes. |
| Antivirus de Windows Defender\\Actualizaciones de firma | Definir el orden de los orígenes para descargar actualizaciones de definiciones | FileShares | Habilitado. Si habilitas esta configuración, se contactará con los orígenes de actualización de las definiciones en el orden especificado. Una vez que las actualizaciones de definiciones se hayan descargado correctamente desde una origen específico, no se contactará con los restantes orígenes de la lista. |
| Informe de errores de Windows | Enviar automáticamente volcados de memoria para informes de errores que haya generado el sistema operativo |  | Deshabilitada |
| Informe de errores de Windows | Deshabilitar Informe de errores de Windows |  | Habilitada |
| Grabación y difusión de juegos de Windows | Habilita o deshabilita la opción de grabación y difusión de juegos de Windows. | | Deshabilitada |
| Windows Installer | Controlar el tamaño máximo de la memoria caché de los archivos de línea base | 5 | Habilitada |
| Windows Installer | Desactivar la creación de puntos de control de Restaurar sistema |  | Habilitada |
| Windows Mail | Desactivar la característica de comunidades |  | Habilitada |
| Reproductor de Windows Media | No mostrar cuadros de diálogo de primer uso |  | Habilitada |
| Reproductor de Windows Media | Impedir compartir medios |  | Habilitada |
| Centro de movilidad de Windows | Desactivar Centro de movilidad de Windows |  | Habilitada |
| Análisis de confiabilidad de Windows | Configurar proveedores de WMI de confiabilidad |  | Deshabilitada |
| Windows Update | Permitir la instalación inmediata de Actualizaciones automáticas |  | Habilitada |
| Windows Update | No conectarse a ninguna ubicación de Internet de Windows Update |  | Habilitado. Si habilitas esta directiva se deshabilitará esa funcionalidad y podría causar que la conexión a los servicios públicos, como Microsoft Store, deje de funcionar. NOTA: Esta directiva solo se aplica cuando el dispositivo está configurado para conectarse a un servicio de actualización de intranet mediante la directiva "Especificar la ubicación del servicio Microsoft Update en la intranet". |
| Windows Update | Quitar el acceso a todas las características de Windows Update |   | Habilitada |
| *Windows Update\\Windows Update para empresas | Administrar las versiones preliminares | Establecer el comportamiento para recibir versiones preliminares: | Habilitado. Al seleccionar la deshabilitación de versiones preliminares evitarás que las versiones preliminares se instalen en el dispositivo. Esto evitará que los usuarios se inscriban en el Programa Windows Insider, a través de la opción Configuración > Actualización y seguridad.<br>Deshabilitado. Deshabilita las versiones preliminares. |
| *Windows Update\\Windows Update para empresas | Seleccionar cuándo se reciben las versiones preliminares y las actualizaciones de características | Canal semianual<br>Aplazamiento: 365 días<br>Inicio de la pausa: aaaa-mm-dd. | Habilitado. Habilita esta directiva para especificar el nivel de versión preliminar o actualizaciones de características para recibirlas y cuándo. |
| Windows Update\\Windows Update para empresas | Selecciona cuándo quieres recibir actualizaciones de calidad | 1. 30 días<br>2. Pausar las actualizaciones de calidad a partir del aaaa-mm-dd | Habilitada |
| Configuración de directivas personalizadas de tráfico de restricciones de Windows | Impedir que OneDrive genere tráfico de red hasta que el usuario inicie sesión en OneDrive |  | Habilitado. Habilita esta configuración si quieres evitar que el cliente de sincronización de OneDrive (OneDrive.exe) genere tráfico en la red (comprobación de actualizaciones, etc.) hasta que el usuario inicie sesión en OneDrive o comience a sincronizar archivos en el equipo local. |
| Configuración de directivas personalizadas de tráfico de restricciones de Windows | Desactivar las notificaciones de Windows Defender |  | Habilitado. Si habilitas esta configuración de directiva, Windows Defender no enviará notificaciones con información crítica sobre el estado y la seguridad de tu dispositivo. |
| Directiva de equipo local\\Configuración de usuario\\Plantillas administrativas  |  |  |
|Panel de control\\Configuración regional y de idioma | Desactivar la oferta de predicciones de texto al escribir |  | Habilitada |
| Escritorio | No agregar recursos compartidos de documentos abiertos recientemente a Ubicaciones de red |  | Habilitada |
| Escritorio | Desactivar el gesto del mouse de minimización de ventanas de Aero Shake |  | Habilitada |
| Escritorio\\Active Directory | Tamaño máximo de las búsquedas en Active Directory | 2500 | Habilitada |
| Menú Inicio y barra de tareas | No permitir el anclado de la aplicación Store a la barra de tareas |  | Habilitada |
| Menú Inicio y barra de tareas | No mostrar elementos, ni realizar seguimiento de los mismos, en las Jump Lists desde ubicaciones remotas |  | Habilitada |
| Menú Inicio y barra de tareas | No uses el método basado en la búsqueda al resolver accesos directos de shell |  | Habilitado. El sistema no realiza la búsqueda final de la unidad de disco. Simplemente muestra un mensaje que explica que el archivo no se encuentra. |
| Menú Inicio y barra de tareas | Eliminar la barra de Contactos de la barra de tareas |  | Habilitado. El icono de contactos se eliminará de la barra de tareas, la configuración de alternancia correspondiente se eliminará de la página de configuración de la barra de tareas y los usuarios no podrán anclar contactos a la barra de tareas. |
| Menú Inicio y barra de tareas | Desactivar globos de anuncios de características |  | Habilitado. Los usuarios no pueden anclar la aplicación Store en la barra de tareas. Si la aplicación Store ya está anclada en la barra de tareas, se eliminará de la misma en el próximo inicio de sesión. |
| Menú Inicio y barra de tareas | Desactivar seguimiento del usuario |  | Habilitada |
| Menú Inicio y barra de tareas\\Notificaciones | Desactivar las notificaciones del sistema |  | Habilitada |
| Componentes de Windows\\Contenido de la nube | Desactivar todas las características de Contenido destacado de Windows |  | Habilitada |

### <a name="notes-about-network-connectivity-status-indicator"></a>Notas sobre el indicador de estado de conectividad de red

La configuración de la directiva de grupo anterior incluye opciones de configuración para desactivar la comprobación para ver si el sistema está conectado a Internet. Si tu entorno no se conecta a Internet de ninguna manera o se conecta de forma indirecta, puedes establecer una configuración de directiva de grupo para eliminar el icono de red de la barra de tareas. Si quieres eliminar el ícono de red de la barra de tareas, probablemente se deba a que has desactivado las comprobaciones de conectividad a Internet; recuerda que habrá una marca amarilla en el ícono de red, aunque la red esté funcionando normalmente. Si quieres eliminar el icono de red como una configuración de directiva de grupo, puedes encontrarlo en esta ubicación:

| Configuración de directiva | Elemento | Elemento secundario | Comentarios y configuración posibles|
| -------------- | ---- | -------- | ---------------------------- |
| Windows Update o Windows Update para empresas | Selecciona cuándo quieres recibir actualizaciones de calidad | 1. 30 días<br>2. Pausar las actualizaciones de calidad a partir del aaaa-mm-dd | Habilitada |
| Directiva de equipo local\\Configuración de usuario\\Plantillas administrativas |  |  |  |
| Menú Inicio y barra de tareas | Quitar el icono de redes |  | Habilitado. El icono de redes no se muestra en el área de notificaciones del sistema. |

Para obtener más información sobre el indicador de estado de conexión de red (NCSI), consulta [Administrar puntos de conexión de conexión para Windows 10 Enterprise, versión 1903](/windows/privacy/manage-windows-1903-endpoints) y [Administrar las conexiones de los componentes del sistema operativo Windows 10 a servicios Microsoft](/windows/privacy/manage-connections-from-windows-operating-system-components-to-microsoft-services).

### <a name="system-services"></a>Servicios del sistema

Si estás pensando en deshabilitar servicios del sistema para conservar recursos, asegúrate de que el servicio que quieres deshabilitar no sea un componente de otro servicio. Tenga en cuenta que algunos servicios no están en la lista porque no se pueden deshabilitar de una manera compatible.

La mayoría de estas recomendaciones copian las recomendaciones para Windows Server 2016 instalado con Experiencia de escritorio de la [Guía para deshabilitar los servicios del sistema en Windows Server 2016 con Experiencia de escritorio](../../security/windows-services/security-guidelines-for-disabling-system-services-in-windows-server.md).

Muchos de los servicios que pueden parecer buenos candidatos para la deshabilitación están configurados para el tipo de inicio de servicio manual. Esto significa que el servicio no se iniciará automáticamente y no se inicia a menos que un proceso o un evento desencadene una solicitud del servicio que piensas deshabilitar. Los servicios que ya están configurados para iniciar el manual de tipo, no se enumeran aquí por lo general.

> [!NOTE]
> Puedes enumerar los servicios en ejecución con este código de ejemplo de PowerShell y que se muestre solo el nombre corto del servicio:

```powershell
 Get-Service | Where-Object {$_.Status -eq "Running"} | select -ExpandProperty Name
 ```

| Servicio de Windows | Elemento | Comentarios|
| -------------- | ---- | ---------------------------- |
| CDPUserService | Este servicio de usuario se usa en los escenarios de la plataforma de dispositivos conectados. | Este es un servicio por usuario y, como tal, el *servicio de plantillas* debe estar deshabilitado. |
| Telemetría y experiencias del usuario conectado | Habilita características que admiten experiencias de usuario conectadas y en la aplicación. Asimismo, este servicio administra la recopilación y transmisión de la información de diagnóstico y uso basada en los eventos (se usa para mejorar la experiencia y la calidad de la plataforma de Windows) cuando la configuración de opciones de diagnóstico y privacidad de uso está habilitada en la opción Comentarios y diagnósticos. | Puedes deshabilitar esta opción si la red está desconectada. |
| Datos de contacto | Indexa los datos de contacto para poder buscar los contactos rápidamente. Si detienes o deshabilitas este servicio, es posible que falten contactos en tus resultados de búsqueda. | Este es un servicio por usuario y, como tal, el *servicio de plantillas* debe estar deshabilitado. |
| Servicio de directivas de diagnóstico | Permite la detección, solución y resolución de problemas para componentes de Windows. Si este servicio se detiene, los diagnósticos ya no funcionarán. | |
| Administrador de mapas descargados | Servicio de Windows para que las aplicaciones obtengan acceso a mapas descargados. Este servicio lo pide la aplicación que accederá a los mapas descargados. Si deshabilitas este servicio las aplicaciones no obtendrán acceso a los mapas. | |
| Servicio de geolocalización | Supervisa la ubicación actual del sistema y gestiona las geovallas. | |
| Servicio de usuario de GameDVR y difusión | Este servicio de usuario se usa en grabaciones de juegos y difusiones en directo. | Este es un servicio por usuario y, como tal, el servicio de plantillas debe estar deshabilitado. |
| MessagingService | Servicio de compatibilidad con mensajes de texto y funcionalidades relacionadas. | Este es un servicio por usuario y, como tal, el *servicio de plantillas* debe estar deshabilitado. |
| Optimizar unidades | Permite que el equipo funcione de forma más eficiente al optimizar los archivos en las unidades de almacenamiento. | Las soluciones VDI normalmente no se benefician de la optimización del disco. Estas "unidades" no son unidades tradicionales y, a menudo, son solo una asignación de almacenamiento temporal. |
| SuperFetch | Mantiene y mejora el rendimiento del sistema a lo largo del tiempo. | En general, no mejora el rendimiento en VDI (especialmente cuando es no persistente) dado que el estado del sistema operativo se descarta en cada reinicio. |
| Servicio de teclado táctil y panel de escritura a mano | Habilita las funciones de lápiz y entradas de lápiz del teclado táctil y del panel de escritura a mano. | |
| Informe de errores de Windows | Permite que se informe acerca de los errores cuando los programas dejan de funcionar o responder y permite que se entreguen las soluciones existentes. También permite generar registros para los servicios de diagnóstico y reparaciones. Si se detiene este servicio, es posible que el informe de errores no funcione correctamente y que no se muestren los resultados de los servicios de diagnóstico y reparaciones. | Con VDI, los diagnósticos se realizan a menudo en un escenario sin conexión y no en la producción convencional. Asimismo, algunos clientes deshabilitan WER de todos modos. WER usa una pequeña cantidad de recursos para hacer muchas cosas diferentes, incluyendo mostrar un error al instalar un dispositivo o un error al instalar una actualización. |
| Servicio de uso compartido de red del Reproductor de Windows Media | Comparte las bibliotecas de Windows Media Player con otros reproductores de red y dispositivos multimedia con Universal Plug and Play. | No es necesario a menos que los clientes compartan bibliotecas WMP en la red. |
| Servicio de zona con cobertura inalámbrica móvil de Windows | Proporciona la capacidad de compartir una conexión de datos móviles con otro dispositivo. | |
| Windows Search | Proporciona la indexación del contenido, el almacenamiento en caché de propiedades y los resultados de la búsqueda de archivos, correo electrónico y otro contenido.                                                                    | Probablemente no sea necesario, especialmente con VDI no persistentes. |

#### <a name="per-user-services-in-windows"></a>Servicios por usuario en Windows

Los servicios por usuario son servicios que se crean cuando un usuario inicia sesión en Windows o Windows Server y se detienen y eliminan cuando ese usuario cierra la sesión. Estos servicios se ejecutan en un contexto de seguridad de la cuenta de usuario; esto proporciona una administración de recursos mejor que la del enfoque anterior para ejecutar este tipo de servicios en el explorador, el cual asociado a una cuenta preconfigurada o como tareas.

[Servicios por usuario en Windows 10 y Windows Server](/windows/application-management/per-user-services-in-windows)

Si piensas cambiar el valor de inicio de un servicio, el método preferido es abrir un símbolo del sistema con privilegios elevados .cmd y ejecutar la herramienta Administrador de control de servicios "Sc.exe". Para más información sobre cómo utilizar "Sc.exe", consulta [Sc](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc754599(v=ws.11)).

### <a name="scheduled-tasks"></a>Tareas programadas

Al igual que otros elementos en Windows, asegúrate de que un elemento no sea necesario antes de desactivarlo.

En la siguiente lista, las tareas que se muestran son aquellas que realizan optimizaciones o recopilaciones de datos en equipos que mantienen su estado durante los reinicios. Cuando una tarea de VM de VDI se reinicia y descarta todos los cambios desde el último arranque, las optimizaciones destinadas a los equipos físicos no son útiles.

Puedes obtener todas las tareas programadas actuales, incluidas las descripciones, con el siguiente código de PowerShell:

```powershell
 Get-ScheduledTask | Select-Object -Property TaskPath,TaskName,State,Description
```

>[!NOTE]
> Hay varias tareas que no se pueden deshabilitar mediante script, aunque estés ejecutando con privilegios elevados. Se recomienda no deshabilitar las tareas que no se pueden deshabilitar mediante script.

Nombre de tarea programada:

- Móvil
- Consolidator
- Diagnóstico
- FamilySafetyMonitor
- FamilySafetyRefreshTask
- MaintenanceTasks
- MapsToastTask
- Compatibilidad
- Microsoft-Windows-DiskDiagnosticDataCollector
- MNO
- NotificationTask
- PerformRemediation
- ProactiveScan
- ProcessMemoryDiagnosticEvents
- ProgramDataUpdater
- Proxy
- QueueReporting
- RecommendedTroubleshootingScanner
- ReconcileFeatures
- ReconcileLanguageResources
- RefreshCache
- RegIdleBackup
- ResPriStaticDbSync
- RunFullMemoryDiagnostic
- ScanForUpdates
- ScanForUpdatesAsUser
- Programado
- ScheduledDefrag
- sihpostreboot
- SilentCleanup
- SmartRetry
- SpaceAgentTask
- SpaceManagerTask
- SpeechModelDownloadTask
- Sqm-Tasks
- SR
- StartComponentCleanup
- StartupAppTask
- StorageSense
- SyspartRepair
- Sysprep
- UninstallDeviceTask
- UpdateLibrary
- UpdateModelTask
- UsbCeip
- Usb-Notifications
- USO_UxBroker
- Wi-Fi
- WIM-Hash-Management
- WindowsActionDialog
- WinSAT
- Carpetas
- WsSwapAssessmentTask
- XblGameSaveTask

### <a name="apply-windows-and-other-updates"></a>Aplicación de actualizaciones de Windows (y otras)

Aplica las actualizaciones disponibles, desde Microsoft Update o desde los recursos internos, incluidas las firmas de Windows Defender. Este es un buen momento para aplicar otras actualizaciones disponibles, incluidas las de Microsoft Office si están instaladas, y otras actualizaciones de software. Si PowerShell va a permanecer en la imagen, puedes descargar la ayuda más reciente disponible para PowerShell ejecutando el comando [Update-Help](/powershell/module/microsoft.powershell.core/update-help?view=powershell-7).

#### <a name="servicing-the-operating-system-and-apps"></a>Mantenimiento del sistema operativo y las aplicaciones

En algún momento durante el proceso de optimización de la imagen, se deben aplicar las actualizaciones de Windows. Hay una opción en la configuración de actualización de Windows 10 que puede proporcionar actualizaciones adicionales:

![Actualizaciones adicionales](media/rds-vdi-recommendations-1909/servicing.png)

Esta es una buena configuración en caso de que vayas a instalar aplicaciones de Microsoft como Microsoft Office en la imagen base. De esta forma, Office se actualiza cuando la imagen se agrega al servicio. Asimismo, también hay actualizaciones de .NET y ciertos componentes de terceros, como Adobe, que tienen actualizaciones disponibles a través de Windows Update.

Es muy importante que tengas en cuenta las actualizaciones de seguridad para las máquinas virtuales de VDI no persistente, incluyendo los archivos de definición de software de seguridad. Estas actualizaciones se pueden publicar una vez o más de una vez al día. Además, existe una forma de conservar estas actualizaciones, incluidos los componentes de Windows Defender y los de terceros.

En cuanto a Windows Defender, es mejor permitir que se realicen las actualizaciones, incluso en VDI no persistente. Las actualizaciones se aplicarán a casi todas las sesiones de inicio de sesión, pero recuerda que estas son pequeñas y no deberían suponer un problema. Además, la VM no se retrasará en las actualizaciones porque solo se aplicarán las más recientes disponibles. Lo mismo podría aplicarse en los archivos de definición de terceros.

> [!NOTE]
> Las aplicaciones de Store (aplicaciones UWP) se actualizan a través de Microsoft Store. Igualmente, las versiones modernas de Office, como Office 365, se actualizan a través de sus propios mecanismos cuando se conectan directamente a Internet o a través de tecnologías de administración cuando no lo están.

### <a name="windows-system-startup-event-traces"></a>Seguimientos de eventos de inicio del sistema Windows

Windows está configurado, de forma predeterminada, para recopilar y guardar datos de diagnóstico limitados. El propósito es habilitar los diagnósticos o registrar datos si es necesario solucionar problemas adicionales. Los seguimientos del sistema automáticos se pueden encontrar en la ubicación que se muestra en la siguiente ilustración:

![Seguimientos del sistema](media/rds-vdi-recommendations-1909/system-traces.png)

Algunas los seguimientos mostrados en **Sesiones de seguimiento de eventos** y **Sesiones de seguimiento de eventos de inicio** no pueden y no deben detenerse. Otras opciones, como el seguimiento de tipo "WiFiSession" se pueden detener. Para detener un seguimiento en ejecución en **Sesiones de seguimiento de eventos**, haz clic con el botón derecho en el seguimiento y luego en "Detener". Usa el siguiente procedimiento para evitar que los seguimientos se inicien automáticamente en el inicio:

1. Haz clic en la carpeta **Sesiones de seguimiento de eventos de inicio**.

2. Localiza el seguimiento que te interesa y haz doble clic en el mismo.

3. Haz clic en la pestaña **Realizar seguimiento de sesión**.

4. Haz clic en el cuadro con la etiqueta **Habilitado** para quitar la marca de comprobación.

5. Haga clic en **Aceptar**.

Los siguientes son algunos seguimientos del sistema que puedes deshabilitar en VDI:

| Nombre                    | Comentario                       |
| ----------------------- | ----------------------------- |
| AppModel | Una colección de seguimientos, uno de los cuales es el teléfono |
| CloudExperienceHostOOBE | |
| DiagLog | |
| NtfsLog | |
| TileStore | |
| UBPM | |
| WiFiDriverIHVSession | Si no estás usando un dispositivo WiFi |
| WiFiSession | |
| WinPhoneCritical | |

### <a name="windows-defender-optimization-with-vdi"></a>Optimización de Windows Defender con VDI

Microsoft ha publicado recientemente documentación sobre Windows Defender en un entorno de VDI. Consulta la [Guía de implementación del Antivirus de Windows Defender en un entorno de infraestructura de escritorio virtual (VDI)](/windows/security/threat-protection/windows-defender-antivirus/deployment-vdi-windows-defender-antivirus) para más información.

En el artículo anterior verás procedimientos para el mantenimiento de la imagen de VDI de tipo "gold" y sabrás cómo mantener los clientes de VDI mientras se ejecutan. Para reducir el uso de ancho de banda de la red cuando los equipos de VDI necesiten actualizar sus firmas de Windows Defender, es recomendable escalonar los reinicios y programarlos durante tus horas libres, siempre que sea posible. Las actualizaciones de firmas de Windows Defender pueden estar en los recursos compartidos de archivos de forma interna y, cuando sean necesarios, esos archivos compartidos se encuentran en los mismos segmentos de red o en segmentos de red cercanos a las máquinas virtuales de VDI.

### <a name="client-network-performance-tuning-by-registry-settings"></a>Ajuste del rendimiento de red del cliente mediante la configuración del registro

Hay algunas configuraciones del registro que pueden aumentar el rendimiento de la red. Esto es especialmente importante en entornos donde VDI o el equipo tiene una carga de trabajo que se basa principalmente en la red. Se recomienda la configuración de esta sección para sesgar el rendimiento para favorecer las redes, configurando un búfer adicional y almacenando en caché cosas como entradas de directorio.

>[!NOTE]
> Algunas configuraciones de esta sección se basan únicamente en el registro y deben incorporarse en la imagen base antes de implementarla para su uso en la producción.

La siguiente configuración se documenta en la información que proporciona las [Directrices de optimización del rendimiento para Windows Server 2016](/windows-server/administration/performance-tuning/), que publicó en Microsoft.com el grupo de productos de Windows.

#### <a name="disablebandwidththrottling"></a>DisableBandwidthThrottling

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DisableBandwidthThrottling`

Válido para Windows 10. El valor predeterminado es **0**. De manera predeterminada, el redirector de SMB limita el rendimiento en las conexiones de red de alta latencia, en algunos casos para evitar tiempos de espera relacionados con la red. Establecer este valor del Registro en 1 deshabilita esta limitación y permite un mayor rendimiento de transferencia de archivos a través de conexiones de red de alta frecuencia. Intenta establecer este valor en **1**.

#### <a name="fileinfocacheentriesmax"></a>FileInfoCacheEntriesMax

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileInfoCacheEntriesMax` se aplica en Windows 10. El valor predeterminado es **64**, con un intervalo válido entre 1 y 65536. Este valor se usa para determinar la cantidad de metadatos de archivo que el cliente puede almacenar en caché. Aumentar el valor puede disminuir el tráfico de red y aumentar el rendimiento cuando se accede a varios archivos. Intenta aumentar este valor a **1024**.

#### <a name="directorycacheentriesmax"></a>DirectoryCacheEntriesMax

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DirectoryCacheEntriesMax`

Válido para Windows 10. El valor predeterminado es **16**, con un intervalo válido entre 1 y 4096. Este valor se usa para determinar la cantidad de información de directorio que el cliente puede almacenar en caché. Aumentar el valor puede disminuir el tráfico y aumentar el rendimiento cuando se accede a directorios de gran tamaño. Intenta aumentar este valor a **1024**.

#### <a name="filenotfoundcacheentriesmax"></a>FileNotFoundCacheEntriesMax

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileNotFoundCacheEntriesMax`

Válido para Windows 10. El valor predeterminado es **128**, con un intervalo válido entre 1 y 65536. Este valor se usa para determinar la cantidad de información de nombres de archivos que el cliente puede almacenar en caché. Aumentar el valor puede disminuir el tráfico de red y aumentar el rendimiento cuando se accede a varios archivos de nombres. Intenta aumentar este valor a **2048**.

#### <a name="dormantfilelimit"></a>DormantFileLimit

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DormantFileLimit`

Válido para Windows 10. El valor predeterminado es **1023**. Este parámetro especifica el número máximo de archivos que se deben dejar abiertos en un recurso compartido una vez que la aplicación cerró el archivo. Si varios miles de clientes se conectan a servidores SMB, considera la posibilidad de reducir este valor a **256**.

Puedes configurar varias de estas opciones de SMB con los cmdlets [Set-SmbClientConfiguration](/powershell/module/smbshare/set-smbclientconfiguration?view=win10-ps) y [Set-SmbServerConfiguration](/powershell/module/smbshare/set-smbserverconfiguration?view=win10-ps) de Windows PowerShell. Las opciones de solo registro se pueden configurar con Windows PowerShell, tal como se muestra en el siguiente ejemplo:

```powershell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters" RequireSecuritySignature -Value 0 -Force
```

Configuración adicional de las instrucciones de línea de base de funcionalidades limitadas de tráfico restringido de Windows: Microsoft ha lanzado una línea de base que se creó con los mismos procedimientos que las [Líneas de base de seguridad de Windows](/powershell/module/smbshare/set-smbserverconfiguration?view=win10-ps), para entornos que no están conectados directamente a Internet o que deben reducir los datos enviados a Microsoft y otros servicios.

La configuración [Línea de base de funcionalidades limitadas de tráfico restringido de Windows](/windows/privacy/manage-connections-from-windows-operating-system-components-to-microsoft-services) se indica en la tabla de directivas de grupo con un asterisco.

#### <a name="disk-cleanup-including-using-the-disk-cleanup-wizard"></a>Limpieza del disco (incluido el uso del Asistente para liberar espacio en disco)

La limpieza del disco puede ser especialmente útil con implementaciones de VDI de imagen gold o maestra. Después de preparar, actualizar y configurar la imagen, una de las últimas tareas a realizar es la limpieza del disco. Hay una herramienta integrada denominada "Liberador de espacio en disco" que puede ayudarte a limpiar la mayoría de las áreas susceptibles a ahorrar espacio en disco. En una máquina virtual que tenga muy poco instalado, pero a la que se han aplicado todos los parches, se suelen liberar aproximadamente 4 GB de espacio en disco al ejecutar la limpieza del disco.

Igualmente, aquí tienes sugerencias para realizar varias tareas para liberar espacio en disco. Todas deben probarse antes de la implementación:

1. Ejecutar (con privilegios elevados) el liberador de espacio en disco, después de aplicar todas las actualizaciones. Incluye las categorías "Optimización de distribución" y "Limpieza de actualizaciones de Windows". Este proceso se puede automatizar mediante la línea de comandos `Cleanmgr.exe` con la opción `/SAGESET:11`. La opción `/SAGESET` establece los valores del Registro que se pueden usar más adelante para automatizar la limpieza del disco, que usa todas las opciones disponibles en el liberador de espacio en disco.

    1. En una VM de prueba, desde una instalación limpia, la ejecución de `Cleanmgr.exe /SAGESET:11` revela que solo hay dos opciones para la limpieza del disco automática y que están habilitadas de forma predeterminada:
    
        - Archivos de programa descargados

        - Archivos temporales de Internet

    2. Si configuras más opciones o todas ellas, esas opciones se guardan en el Registro, de acuerdo con el valor del **Índice** proporcionado en el comando anterior (`Cleanmgr.exe /SAGESET:11`). En este caso, usaremos el valor `11` como índice, para un posterior procedimiento de limpieza del disco automatizado.

    3. Después de ejecutar `Cleanmgr.exe /SAGESET:11`, verás varias categorías de opciones de limpieza del disco. Puedes comprobar todas las opciones y luego hacer clic en **Aceptar**. Desaparece el Liberador de espacio en disco y la configuración se guarda en el Registro.

2. Limpia el almacenamiento de instantáneas de volumen, si hay alguno en uso.

    - Abre un símbolo del sistema con permisos elevados y ejecuta el comando `vssadmin list shadows` y luego el comando `vssadmin list shadowstorage`.
    
        Si el resultado de estos comandos es **La consulta no dio ningún resultado**, es que no se está usando ningún almacenamiento de VSS.

3. Limpieza de los archivos temporales y los registros. En un símbolo del sistema con permisos elevados, ejecuta el comando `Del C:\*.tmp /s`, el comando `Del C:\Windows\Temp\.` y luego el comando `Del %temp%\.`.

4. Elimina los perfiles que no uses del sistema, ejecutando `wmic path win32_UserProfile where LocalPath="c:\users\<user>" Delete`.

### <a name="remove-onedrive-components"></a>Eliminación de componentes de OneDrive

Quitar OneDrive implica la eliminación del paquete, la desinstalación y la eliminación de los archivos *.lnk. El siguiente código de ejemplo de PowerShell se puede usar para ayudar a quitar OneDrive de la imagen y se incluye en los scripts de optimización de VDI de GitHub:

```azurecli

Taskkill.exe /F /IM "OneDrive.exe"
Taskkill.exe /F /IM "Explorer.exe"` 
    if (Test-Path "C:\\Windows\\System32\\OneDriveSetup.exe")`
     { Start-Process "C:\\Windows\\System32\\OneDriveSetup.exe"`
         -ArgumentList "/uninstall"`
         -Wait }
    if (Test-Path "C:\\Windows\\SysWOW64\\OneDriveSetup.exe")`
     { Start-Process "C:\\Windows\\SysWOW64\\OneDriveSetup.exe"`
         -ArgumentList "/uninstall"`
         -Wait }
Remove-Item -Path
"C:\\Windows\\ServiceProfiles\\LocalService\\AppData\\Roaming\\Microsoft\\Windows\\Start Menu\\Programs\\OneDrive.lnk" -Force
Remove-Item -Path "C:\\Windows\\ServiceProfiles\\NetworkService\\AppData\\Roaming\\Microsoft\\Windows\\Start Menu\\Programs\\OneDrive.lnk" -Force \# Remove the automatic start item for OneDrive from the default user profile registry hive
Start-Process C:\\Windows\\System32\\Reg.exe -ArgumentList "Load HKLM\\Temp C:\\Users\\Default\\NTUSER.DAT" -Wait
Start-Process C:\\Windows\\System32\\Reg.exe -ArgumentList "Delete HKLM\\Temp\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Run /v OneDriveSetup /f" -Wait
Start-Process C:\\Windows\\System32\\Reg.exe -ArgumentList "Unload HKLM\\Temp" -Wait Start-Process -FilePath C:\\Windows\\Explorer.exe -Wait
```

Si tienes cualquier pregunta o duda sobre la información de este documento, ponte en contacto con el equipo de tu cuenta de Microsoft, lee el blog de Microsoft VDI, publica un mensaje en los foros de Microsoft o ponte directamente en contacto con Microsoft.

## <a name="turn-windows-update-back-on"></a>Volver a activar Windows Update

Si quieres volver a activar Windows Update, como en el caso de VDI persistente, sigue estos pasos:

- Vuelve a habilitar estas configuraciones de directiva de grupo:

    - Directiva de equipo local\\Configuración del equipo\\Plantillas administrativas\\Sistema\\Administración de comunicaciones de Internet\\Configuración de comunicaciones de Internet

        - Desactiva el acceso a todas las características de Windows Update (cámbialas de **habilitado** a **no configurado**).

    - Directiva de equipo local\\Configuración del equipo\\Plantillas administrativas\\Componentes de Windows\\Windows Update

        - Quita el acceso a todas las características de Windows Update (cámbialas de **habilitado** a **no configurado**).

        - No te conectes a ninguna ubicación de Internet de Windows Update (cámbialas de **habilitado** a **no configurado**).

    - Directiva de equipo local\\Configuración del equipo\\Plantillas administrativas\\Componentes de Windows\\Windows Update\\Windows Update para empresas

        - Selecciona cuándo quieres recibir las actualizaciones de calidad (cámbialas de "habilitado" a "no configurado").

    -   Directiva de equipo local\\Configuración del equipo\\Plantillas administrativas\\Componentes de Windows\\Windows Update\\Windows Update para empresas

        - Selecciona cuándo quieres recibir las actualizaciones de versiones preliminares y características (cámbialas de **habilitado** a **no configurado**).

-  Vuelve a habilitar los servicios

    - Actualiza el servicio Orchestrator (cambia de **deshabilitado** a **Automático (inicio retrasado)** ).

    - Edita las configuraciones del Registro de Windows siguientes:

        - HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsUpdate\UpdatePolicy\PolicyState

            - DeferQualityUpdates (cámbiala de **1** a **0**)

        - HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsUpdate\UpdatePolicy\Settings

            - PausedQualityDate (elimina cualquier valor existente)

        - HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\Explorer\WAU

            - Deshabilitada

-  Vuelve a habilitar las tareas programadas

    - Biblioteca del Programador de tareas\\Microsoft\\Windows\\InstallService\\ScanForUpdates

    - Biblioteca del Programador de tareas\\Microsoft\\Windows\\InstallService\\ScanForUpdatesAsUser

Para que todas estas configuraciones surtan efecto, reinicia el dispositivo. Si no quieres que este dispositivo le ofrezca actualizaciones de características, vea a Configuración\\Windows Update\\Opciones avanzadas\\Elegir cuándo se instalarán las actualizaciones y, a continuación, establece la opción manualmente, **Una actualización de características incluye mejoras y nuevas capacidades. Se puede posponer todos estos días en un valor distinto de cero, como 180, 365, etc.**

### <a name="references"></a>Referencias

- [¿Qué es VDI (infraestructura de escritorio virtual)?](https://www.citrix.com/glossary/vdi.html)

- [Se produce un error en Sysprep después de quitar o actualizar aplicaciones de Microsoft Store que incluyen imágenes integradas de Windows](https://support.microsoft.com/help/2769827/sysprep-fails-after-you-remove-or-update-windows-store-apps-that-inclu)
