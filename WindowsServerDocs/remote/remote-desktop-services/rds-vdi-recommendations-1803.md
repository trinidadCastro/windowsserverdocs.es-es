---
title: Optimización de Windows 10, versión 1803, para un rol de infraestructura de escritorio virtual (VDI)
description: Configuración y opciones de configuración recomendadas para minimizar la sobrecarga de los escritorios Windows 10, versión 1803 que se usan como imágenes VDI
ms.author: robsmi
ms.topic: article
author: jaimeo
ms.date: 12/08/2020
ms.openlocfilehash: 5db6ef50667a7280b20b49fce408ac903c1323b7
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947311"
---
# <a name="optimizing-windows-10-version-1803-for-a-virtual-desktop-infrastructure-vdi-role"></a>Optimización de Windows 10, versión 1803, para un rol de infraestructura de escritorio virtual (VDI)

En este artículo podrás elegir la configuración para Windows 10, versión 1803 (compilación 17134) que debería tener el mejor rendimiento en un entorno de infraestructura de escritorio virtualizado (VDI). Todas las configuraciones de esta guía son *recomendaciones a tener en cuenta* y de ninguna manera son obligatorias.

En un entorno de VDI, los métodos clave para optimizar el rendimiento de Windows 10 son minimizar los gráficos de la aplicación que han vuelto a dibujarse, las actividades en segundo plano que no tienen un beneficio importante para el entorno de VDI y, en general, reducir los procesos en ejecución al mínimo. Un objetivo secundario es reducir el uso del espacio en disco de la imagen base al mínimo. Con las implementaciones de VDI, el tamaño de imagen base o de tipo "gold" más pequeño posible, puede reducir ligeramente el uso de la memoria en el hipervisor, así como provocar una pequeña reducción en las operaciones de red generales necesarias para entregar la imagen de escritorio al consumidor.

> [!NOTE]
> La configuración que aquí se recomienda se puede aplicar a otra instalación de Windows 10, versión 1803, incluyendo los dispositivos físicos u otros dispositivos virtuales. Ninguna recomendación de este tema debería afectar a la compatibilidad de Windows 10, versión 1803.

> [!TIP]
> Un script que implementa las optimizaciones discutidas en este tema, así como un archivo de exportación GPO que puedes importar con **LGPO.exe**, está disponible en [TheVDIGuys](https://github.com//TheVDIGuys) en GitHub.

## <a name="vdi-optimization-principles"></a>Principios de optimización de VDI

Un entorno de VDI presenta una sesión de escritorio completa (incluyendo las aplicaciones) de un usuario de PC a través de una red. Los entornos de VDI normalmente usan una imagen del sistema operativo base, que luego se convierte en la base de los escritorios que posteriormente se presentan a los usuarios para que puedan trabajar. Existen variaciones de las implementaciones de VDI, como "persistente", "no persistente" y "sesión de escritorio". El tipo "persistente" conserva los cambios en el sistema operativo del escritorio de VDI de una sesión a la siguiente. El tipo "no persistente" no conserva los cambios en el sistema operativo del escritorio de VDI de una sesión a la siguiente. Para el usuario este escritorio es un poco diferente a otro dispositivo virtual o físico, aparte de que se accede a él a través de una red.

La configuración de optimización se lleva a cabo en un dispositivo de referencia. Una VM es un lugar ideal para compilar la imagen, ya que puedes guardar el estado, crear puntos de control, realizar copias de seguridad y otras tareas útiles. Primero, instala el sistema operativo predeterminado en la VM básica y optimízala para el uso de VDI; para ello, elimina aplicaciones innecesarias instalando actualizaciones de Windows, instalando otras actualizaciones, eliminando archivos temporales, aplicando configuraciones, etc.

Existen otros tipos de VDI, como los Servicios de Escritorio remoto (RDS) y los tipos persistentes. En este tema no se proporciona una explicación más detallada sobre estas tecnologías, ya que se centra en la configuración de la imagen base de Windows y hace referencia a otros factores del entorno, como la optimización del host.

### <a name="persistent-vdi"></a>VDI persistente

En el nivel básico, la VDI persistente es una VM que guarda el estado del sistema operativo entre reinicios. Otros niveles de software de la solución VDI proporcionan a los usuarios acceso fácil y sin problemas a sus VM asignadas, a menudo con una solución de inicio de sesión único.

Existen varias implementaciones diferentes de VDI persistente:

- La máquina virtual tradicional, donde la VM tiene su propio archivo de disco virtual, se inicia normalmente, guarda los cambios de una sesión a la siguiente y es esencialmente una VM normal. La diferencia radica en cómo accede el usuario a esa VM. Es posible que haya un portal web en el que el usuario inicia sesión y que lo dirige automáticamente a sus una o más VM asignadas de VDI.

- Una máquina virtual persistente basada en imágenes, con discos virtuales personales. En este tipo de implementación hay una imagen base o de tipo "gold" en uno o más servidores host. Se crea una VM y, a continuación, uno o más discos virtuales se crean y asignan a este disco para obtener un almacenamiento persistente.

    - Cuando se inicia la VM, se lee una copia de la imagen base en la memoria de la VM. Al mismo tiempo, un disco virtual persistente asignado a esa VM, con cualquier cambio previo en el sistema operativo se fusiona mediante un proceso complejo.

    - Cambios tales como las escrituras del registro de eventos o las escrituras del registro, se redirigen al disco virtual de lectura y escritura que está asignado a esa VM.

    - En esta circunstancia, el sistema operativo y el mantenimiento de la aplicación pueden funcionar normalmente si se usa el software de mantenimiento tradicional, como Windows Server Update Services u otras tecnologías de administración.

### <a name="non-persistent-vdi"></a>VDI no persistente

Cuando una implementación de VDI no persistente se basa en una imagen base o de tipo "gold", las optimizaciones se realizan principalmente en la imagen base y luego a través de la configuración local y las directivas locales.

Con la VDI no persistente basada en imágenes, la imagen base es de solo lectura. Cuando se inicia una VM de VDI no persistente, se transmite una copia de la imagen base a la VM. La actividad que se produce durante el inicio del proceso hasta que el siguiente reinicio, se redirige a una ubicación temporal. Por lo general, los usuarios reciben ubicaciones de red para almacenar sus datos. En algunos casos, el perfil del usuario se combina con la VM estándar para proporcionar al usuario su configuración.

Un aspecto importante de la VDI no persistente que se basa en una sola imagen es el mantenimiento. Las actualizaciones del sistema operativo se entregan generalmente una vez al mes.
Con la VDI basada en imágenes, hay un conjunto de procesos que se deben realizar con el fin de obtener las actualizaciones de la imagen:

- En un host determinado, todas las VM en ese host que se derivan de la imagen base deben apagarse o desactivarse. Esto significa que los usuarios se redirigen a otras VM.

- A continuación, la imagen base se abre y se inicia. Posteriormente, se realizan todas las actividades de mantenimiento, como actualizaciones del sistema operativo, actualizaciones de .NET, actualizaciones de la aplicación, etc.

- Cualquier configuración nueva que deba aplicarse se realiza en ese momento.

- Cualquier otro mantenimiento se realiza en ese momento.

- La imagen base se cierra.

- La imagen base se sella y se establece para volver al entorno de producción.

Los usuarios pueden volver a iniciar sesión.

> [!NOTE]
> Windows 10 realiza un conjunto de tareas de mantenimiento automáticamente y de forma periódica. Existe una tarea programada que está configurada para ejecutarse a las 3:00 A.M. (hora local) todos los días y de forma predeterminada. Esta tarea programada realiza una lista de tareas, incluida la limpieza de Windows Update. Puedes ver todas las categorías de mantenimiento que tienen lugar automáticamente con este comando de PowerShell:
>
>```powershell
>Get-ScheduledTask | ? {$_.Settings.MaintenanceSettings}
>```
>

Uno de los desafíos de las VDI no persistentes, es que cuando un usuario cierra la sesión, se descarta casi toda la actividad del sistema operativo. El perfil y el estado del usuario pueden guardarse, pero la propia máquina virtual descarta casi todos los cambios realizados desde el último arranque. Por lo tanto, las optimizaciones destinadas a un equipo con Windows que guarda el estado de una sesión a la siguiente no son aplicables.

Dependiendo de la arquitectura de la VM de VDI, cosas como PreFetch y SuperFetch no van a ayudar en el proceso de una sesión a la siguiente, ya que todas las optimizaciones se descartan al reiniciar la VM. La indexación puede ser un desperdicio parcial de recursos, como lo sería cualquier optimización de disco, como una desfragmentación tradicional.

### <a name="to-sysprep-or-not-sysprep"></a>Sysprep o no Sysprep

Windows 10 tiene una funcionalidad integrada llamada [Herramienta de preparación del sistema](/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview) (a menudo abreviada como "Sysprep"). La herramienta Sysprep se usa para preparar para la duplicación una imagen de Windows 10 personalizada. El proceso de Sysprep garantiza que el sistema operativo resultante sea único y se pueda ejecutar en la producción.
Existen varias razones a favor y en contra de ejecutar Sysprep. En el caso de VDI, es posible que quieras personalizar el perfil de usuario predeterminado que se usará como plantilla de perfil para los usuarios subsiguientes que inicien sesión con esta imagen. También es posible que tengas aplicaciones que quieras instalar y que también quieras tener la posibilidad de controlar la configuración de cada aplicación.

La alternativa es usar un .ISO estándar para realizar la instalación, mediante un archivo de respuesta de instalación desatendida, y una secuencia de tareas para instalar aplicaciones o eliminarlas. También puedes usar una secuencia de tareas para establecer la configuración de la directiva local en la imagen; por ejemplo, puedes usar la herramienta [Utilidad de objetos de la directiva de grupo local (LGPO)](/archive/blogs/secguide/lgpo-exe-local-group-policy-object-utility-v1-0).

#### <a name="vdi-optimization-categories"></a>Categorías de optimización de VDI

- Configuración del sistema operativo global

    - Limpieza de la aplicación para UWP

    - Limpieza de las características opcionales

    - Configuración de directivas locales

    - Servicios del sistema

    -   Tareas programadas

    -   Aplicar actualizaciones de Windows

    -   Seguimientos de Windows automáticos

    -   Liberar espacio en disco antes de finalizar (sellado) la imagen

-   Configuración de usuario

-   Configuración del host y del hipervisor

- [Ajuste del rendimiento de red de Windows 10 mediante la configuración del Registro](#tuning-windows-10-network-performance-by-using-registry-settings)

- Configuración adicional de la [línea base de funcionalidad limitada del tráfico restringido de Windows](https://go.microsoft.com/fwlink/?linkid=828887).

- [Liberador de espacio en disco](#disk-cleanup-including-using-the-disk-cleanup-wizard)

### <a name="universal-windows-platform-app-cleanup"></a>Limpieza de la aplicación Plataforma universal de Windows

Uno de los objetivos de una imagen de VDI es ser tan pequeña como sea posible. Una forma de reducir el tamaño de la imagen es eliminar las aplicaciones UWP que no se usarán en el entorno. Además de las aplicaciones para UWP, están los archivos principales de la aplicación, también conocidos como la carga útil. Existe una pequeña cantidad de datos almacenados en el perfil de cada usuario para la configuración específica de la aplicación. También hay una pequeña cantidad de datos en el perfil Todos los usuarios.

La conectividad y el tiempo lo son todo cuando se trata de la limpieza de la aplicación para UWP. Si implementas la imagen base en un dispositivo sin conectividad de red, Windows 10 no podrá conectarse a Microsoft Store, ni descargar aplicaciones o intentar instalarlas mientras, a su vez, intentas desinstalarlas.

Si modificas el archivo .WIM base que usas para instalar Windows 10 y eliminas las aplicaciones para UWP innecesarias de .WIM antes de realizar la instalación, esas aplicaciones no se instalarán; gracias a ello, los tiempos de creación de tu perfil serán más cortos. Más adelante en esta sección, encontrarás información sobre cómo eliminar aplicaciones para UWP del archivo .WIM de instalación.

Una buena estrategia para VDI es aprovisionar las aplicaciones que quieres en la imagen base y, a continuación, limitar o bloquear el acceso a Microsoft Store. Las aplicaciones de Store se actualizan periódicamente en segundo plano en equipos normales. Las aplicaciones para UWP pueden actualizarse durante la ventana de mantenimiento cuando se aplican otras actualizaciones.

#### <a name="delete-the-payload-of-uwp-apps"></a>Eliminar la carga útil de las aplicaciones para UWP

Las aplicaciones para UWP que no son necesarias todavía están en el sistema de archivos y consumen una pequeña cantidad de espacio en disco. En cuanto a las aplicaciones que nunca serán necesarias, la carga útil de aplicaciones para UWP no deseadas se puede eliminar de la imagen base mediante los comandos de PowerShell.

De hecho, si los eliminas del archivo .WIM de instalación mediante los vínculos que se proporcionan más adelante en esta sección, podrás comenzar desde el principio con una lista muy reducida de aplicaciones para UWP.

Ejecuta el siguiente comando para enumerar las aplicaciones para UWP aprovisionadas de un sistema operativo Windows 10 en ejecución, como en este ejemplo de salida truncado de PowerShell:

```powershell

    Get-AppxProvisionedPackage -Online

    DisplayName  : Microsoft.3DBuilder
    Version      : 13.0.10349.0
    Architecture : neutral
    ResourceId   : \~
    PackageName  : Microsoft.3DBuilder_13.0.10349.0_neutral_\~_8wekyb3d8bbwe
    Regions      :
```

Las aplicaciones para UWP que se aprovisionan en un sistema pueden eliminarse durante la instalación del sistema operativo como parte de una secuencia de tareas, o más tarde después de que se instale ese sistema operativo. Este podría ser el mejor método, porque hace que el proceso general de creación o mantenimiento de una imagen sea modular. Una vez que desarrolles los scripts, si algo cambia en una compilación posterior, solo tienes que editar un script existente en lugar de repetir el proceso desde cero. A continuación te mostramos algunos vínculos de información sobre este tema:

[Removing Windows 10 in-box apps during a task sequence](/archive/blogs/mniehaus/removing-windows-10-in-box-apps-during-a-task-sequence) (Quitar aplicaciones de Windows 10 en el equipo durante una secuencia de tareas)

[Removing Built-in apps from Windows 10 WIM-File with Powershell - Version 1.3](https://gallery.technet.microsoft.com/Removing-Built-in-apps-65dc387b) (Quitar aplicaciones integradas del archivo .WIM de Windows 10 con Powershell - Versión 1.3)

[Windows 10 1607: Keeping apps from coming back when deploying the feature update](/archive/blogs/mniehaus/windows-10-1607-keeping-apps-from-coming-back-when-deploying-the-feature-update) (Evitar que las aplicaciones vuelvan a aparecer al implementar la actualización de características)

A continuación, ejecute el comando [Remove-AppxProvisionedPackage](/powershell/module/dism/remove-appxprovisionedpackage) de PowerShell para eliminar las cargas útiles de la aplicación UWP:

```powershell
Remove-AppxProvisionedPackage -Online -PackageName
```

Cada aplicación para UWP debe evaluarse para determinar su aplicabilidad en cada entorno único. Posiblemente quieras instalar una instalación predeterminada de Windows 10, versión 1803, y luego observar qué aplicaciones se están ejecutando y cuáles están consumiendo la memoria. Por ejemplo, es posible que quieras eliminar las aplicaciones que se inician automáticamente, o las aplicaciones que muestran automáticamente la información en el menú Inicio (como el Tiempo y las Noticias), y que podrían no ser de utilidad en el entorno.

Una de las aplicaciones para UWP de la "bandeja de entrada" denominada Fotos, tiene una configuración predeterminada que se denomina **Mostrar una notificación cuando haya nuevos álbumes disponibles**.  La aplicación Fotos puede usar aproximadamente 145 MB de memoria; específicamente, la memoria de conjuntos de trabajo privada, incluso si no se utiliza.  Cambiar la opción **Mostrar una notificación cuando haya nuevos álbumes disponibles** para todos los usuarios no es práctico en este momento, así que puedes eliminar la aplicación Fotos si no es necesaria o si ya no la quieres.

### <a name="clean-up-optional-features"></a>Limpieza de características opcionales

#### <a name="managing-optional-features-with-powershell"></a>Gestión de características opcionales con PowerShell

 Para enumerar las características de Windows instaladas actualmente, ejecuta este comando de PowerShell:

```powershell
Get-WindowsOptionalFeature -Online
```

Puedes habilitar o deshabilitar una característica opcional de Windows específica, tal como se muestra en este ejemplo:

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName "DirectPlay"
```

Para obtener más información al respecto, consulte el [foro de Windows PowerShell](/answers/topics/windows-server-powershell.ht).

#### <a name="enable-or-disable-windows-features-by-using-dism"></a>Habilitar o deshabilitar características de Windows mediante DISM

Puedes usar la herramienta integrada **Dism.exe** para enumerar y controlar las características opcionales de Windows. Asimismo, puedes configurar un script de Dism.exe para que se ejecute durante una secuencia de tareas que instala el sistema operativo.

### <a name="local-policy-settings"></a>Configuración de directivas locales

Se pueden crear muchas optimizaciones para Windows 10 en un entorno VDI mediante la directiva de Windows. Las configuraciones enumeradas aquí pueden aplicarse a la imagen base de forma local. A continuación, si la configuración equivalente no se especifica de ninguna otra manera, como por ejemplo, mediante una directiva de grupo, la configuración seguirá aplicándose.

Algunas decisiones pueden basarse en los aspectos específicos del entorno, por ejemplo:

-   ¿El entorno de VDI puede obtener acceso a Internet?

-   ¿La solución VDI es persistente o no persistente?

Específicamente, las siguientes configuraciones no contrarrestan ni entran en conflicto con ninguna configuración que tenga algo que ver con la seguridad. Estas configuraciones se eligieron para eliminar configuraciones que podrían no ser aplicables a los entornos de VDI.

> [!NOTE]
> En esta tabla de configuraciones de directivas de grupo, los elementos marcados con un asterisco son de la [Línea de base de funcionalidades limitadas de tráfico restringido de Windows](https://go.microsoft.com/fwlink/?linkid=828887).

| Configuración de directiva | Elemento | Elemento secundario | Comentarios y configuración posibles |
|--|--|--|--|
| **Directiva del equipo local \\ Configuración de equipo \\ Configuración de Windows \\ Configuración de seguridad** |  |  |  |
| **Directivas del administrador de listas de redes** | Todas las propiedades de redes | Ubicación de red | El usuario no puede cambiar la ubicación |
| **Directiva del equipo local \\ Configuración del equipo \\ Plantillas administrativas \\ Panel de control** |  |  |  |
| \**_Panel de control_* | Permitir sugerencias en línea |  | Deshabilitado (la configuración no se pondrá en contacto con los servicios de contenido de Microsoft para recuperar sugerencias y contenido de ayuda) |
| **\*Panel de control**\\ Personalización | No mostrar la pantalla de bloqueo |  | Habilitado (esta configuración de directiva controla si aparece la pantalla de bloqueo para los usuarios. Si habilitas esta configuración de directiva, los usuarios que no deben presionar CTRL + ALT + SUPR antes de iniciar sesión verán el icono seleccionado después de bloquear su PC). |
| **\*Panel de control**\\ Personalización | Aplicar una pantalla de bloqueo predeterminada específica y una imagen de inicio de sesión | [![Imagen de la interfaz de usuario para establecer la ruta de acceso a la imagen de la pantalla bloqueo](media/lock-screen-image-settings.png)](media/lock-screen-image-settings.png) | Habilitado (esta configuración te permite especificar la pantalla de bloqueo predeterminada y la imagen de inicio de sesión que se muestra cuando ningún usuario ha iniciado sesión; también establece la imagen especificada como predeterminada para todos los usuarios y reemplaza la imagen predeterminada). Una imagen de baja resolución y no compleja transmite menos datos a través de la red cada vez que se representa la imagen. |
| **\*Panel de control**\\Opciones regionales y de idioma\\Personalización de la escritura a mano | Desactivar el aprendizaje automático |  | Habilitado (si habilitas esta configuración de directiva, el aprendizaje automático se detiene y todos los datos almacenados se eliminan. Los usuarios no pueden configurar esta configuración en el Panel de control). |
| **Directiva de equipo local\\Configuración del equipo\\Plantillas administrativas\\Red** |  |  |  |
| **Servicio de transferencia inteligente en segundo plano (BITS)** | No permitas que el cliente BITS use Windows BranchCache |  | Habilitada |
| **Servicio de transferencia inteligente en segundo plano (BITS)** | No permitas que el equipo actúe como un cliente del caché del mismo nivel de BITS |  | Habilitada |
| **Servicio de transferencia inteligente en segundo plano (BITS)** | No permitas que el equipo actúe como un servidor del caché del mismo nivel de BITS |  | Habilitada |
| **Servicio de transferencia inteligente en segundo plano (BITS)** | Permite el caché del mismo nivel de BITS |  | Deshabilitada |
| **BranchCache** | Activar BranchCache |  | Deshabilitada |
| \**_Fuentes_* | Habilitar los proveedores de fuentes |  | Deshabilitado (Windows no se conecta a un proveedor de fuente en línea y solo enumera las fuentes instaladas localmente). |
| **Autenticación del punto de acceso** | Habilitar la autenticación del punto de acceso |  | Deshabilitada |
| **Servicios de redes punto a punto de Microsoft** | Desactivar los servicios de redes punto a punto de Microsoft |  | Habilitada |
| **Indicador de estado de conectividad de red** (ten en cuenta que hay otras configuraciones en esta sección que se pueden usar en redes aisladas). | Especificar sondeo pasivo | Deshabilitar el sondeo pasivo (casilla) | Habilitado (usa esta configuración si estás en una red aislada o si usas direcciones IP estáticas). |
| **Archivos sin conexión** | Permitir o rechazar el uso de la característica Archivos sin conexión |  | Deshabilitada |
| **\*Configuración de TCPIP**\\Tecnologías de transición IPv6 | Establecer el estado de Teredo | Estado deshabilitado | Habilitado (en el estado deshabilitado no hay interfaces de Teredo presentes en el host). |
| **\*Servicio WLAN**\\Configuración de WLAN | Permitir que Windows se conecte automáticamente a puntos de acceso abiertos sugeridos, a redes que compartan los contactos y a puntos de acceso que ofrezcan servicios de pago |  | Deshabilitado (las opciones **Conectarse a puntos de acceso abiertos sugeridos** , **Conectarse a las redes que compartan mis contactos**  y **Habilitar servicios de pago** se desactivarán y se evitará que los usuarios de este dispositivo puedan habilitarlas.) |
| **Directiva de equipo local \\ Configuración del equipo \\ Plantillas administrativas \\ Menú Inicio y Barra de tareas** |  |  |  |
| \**_Notificaciones_* | Desactivar el uso de la red para notificaciones |  | Habilitado (si se habilita a esta configuración de directiva, las aplicaciones y características del sistema no podrá recibir notificaciones de la red de WNS o mediante las API de sondeo de notificaciones). |
| **Directiva de equipo local \\ Configuración del equipo \\ Plantillas administrativas \\ Sistema** |  |  |  |
| **Instalación de dispositivos** | No enviar un informe de error de Windows cuando se instale un controlador genérico en un dispositivo. |  | Habilitada |
| **Instalación de dispositivos** | Impedir la creación de un punto de restauración del sistema durante la actividad de los dispositivos que normalmente solicitaría la creación de un punto de restauración. |  | Habilitada |
| **Instalación de dispositivos** | Evitar la recuperación de metadatos del dispositivo desde Internet. |  | Habilitada |
| **Instalación de dispositivos** | Impedir que Windows envíe un informe de error cuando un controlador de dispositivo solicite software adicional durante su instalación. |  | Habilitada |
| **Instalación de dispositivos** | Desactivar las notificaciones **Nuevo hardware encontrado** durante la instalación del dispositivo. |  | Habilitada |
| **Filesystem**\\NTFS | Opciones de creación de nombre corto | Deshabilitada en todos los volúmenes | Habilitada |
| \**_Directiva de grupo_* | Configurar la vinculación de un sitio web con la aplicación mediante los controladores de URI de aplicación. |  | Deshabilitado (se deshabilita la vinculación de un sitio web con la aplicación y las URI de HTTP[S] se abrirán en el navegador predeterminado en lugar de iniciar la aplicación asociada). |
| \**_Directiva de grupo_* | Continuar las experiencias en este dispositivo |  | Deshabilitado (otros dispositivos no pueden detectar el dispositivo de Windows y no puede participar en experiencias entre dispositivos). |
| **Administración de comunicación de Internet**\\ Configuración de comunicación de Internet | Desactivar el acceso a todas las características de Windows Update |  | Habilitado (si habilitas esta configuración de directiva, se eliminarán todas las características de Windows Update. Esto incluye bloquear el acceso al sitio web de Windows Update en https://windowsupdate.microsoft.com, desde el hipervínculo de Windows Update en el menú Inicio y también en el menú Herramientas de Internet Explorer. La actualización automática de Windows también está deshabilitada; no recibirás actualizaciones críticas de Windows Update. Igualmente, esta configuración de directiva también evita que el Administrador de dispositivos instale automáticamente las actualizaciones de controladores desde el sitio web de Windows Update). |
| **Administración de comunicación de Internet**\\ Configuración de comunicación de Internet | Desactivar la actualización automática de certificados raíz |  | Habilitado (si habilitas esta configuración de directiva, cuando se recibas un certificado que haya emitido una entidad de certificación raíz no confiable, tu equipo no se comunicará con el sitio web de Windows Update para ver si Microsoft ha agregado la CA a su lista de autoridades de confianza).    **NOTA:** Usa esta directiva solo si tienes un medio alternativo a la última lista de revocación de certificados. |
| **Administración de comunicación de Internet**\\ Configuración de comunicación de Internet | Desactivar los vínculos del visor de eventos "Events.asp" |  | Habilitada |
| **Administración de comunicación de Internet**\\ Configuración de comunicación de Internet | Desactivar el uso compartido de datos de personalización de escritura a mano |  | Habilitada |
| **Administración de comunicación de Internet**\\ Configuración de comunicación de Internet | Desactivar el informe de errores de reconocimiento de escritura a mano |  | Habilitada |
| **Administración de comunicación de Internet**\\ Configuración de comunicación de Internet | Desactivar el contenido de "¿Sabías que...?" del Centro de ayuda y soporte técnico contenido |  | Habilitada |
| **Administración de comunicación de Internet**\\ Configuración de comunicación de Internet | Desactivar la búsqueda en Microsoft Knowledge Base del Centro de ayuda y soporte técnico |  | Habilitada |
| **Administración de comunicación de Internet**\\ Configuración de comunicación de Internet | Desactivar al Asistente para la conexión a Internet si la conexión de direcciones URL hace referencia a Microsoft.com |  | Habilitada |
| **Administración de comunicación de Internet**\\ Configuración de comunicación de Internet | Desactivar la descarga de Internet para los asistentes para publicación y pedidos en línea |  | Habilitada |
| **Administración de comunicación de Internet**\\ Configuración de comunicación de Internet | Desactivar el servicio de asociaciones de archivo de Internet |  | Habilitada |
| **Administración de comunicación de Internet**\\ Configuración de comunicación de Internet | Desactivar el registro si la conexión de direcciones URL hace referencia a Microsoft.com |  | Habilitada |
| **Administración de comunicación de Internet**\\ Configuración de comunicación de Internet | Desactivar la tarea de imágenes "Pedir copias fotográficas" |  | Habilitada |
| **Administración de comunicación de Internet**\\ Configuración de comunicación de Internet | Desactivar la tarea "Publicar en web" para archivos y carpetas |  | Habilitada |
| **Administración de comunicación de Internet**\\ Configuración de comunicación de Internet | Desactivar el Programa para la mejora de la experiencia del usuario de Windows Messenger |  | Habilitada |
| **Administración de comunicación de Internet**\\ Configuración de comunicación de Internet | Desactivar el Programa para la mejora de la experiencia del usuario de Windows |  | Habilitada |
| **\*Administración de comunicación de Internet**\\ Configuración de comunicación de Internet | Desactivar pruebas activas del indicador de estado de conectividad de la red de Windows |  | Habilitado (esta configuración de directiva desactiva las pruebas activas que realiza el indicador de estado de conectividad de red de Windows (NCSI) para determinar si tu equipo está conectado a Internet o a una red más limitada. Como parte del proceso para determinar el nivel de conectividad, NCSI realiza una de dos pruebas activas: descargar una página desde un servidor web dedicado o realizar una solicitud de DNS para una dirección dedicada. Si habilitas esta configuración de directiva, NCSI no ejecuta ninguna de las dos pruebas activas. Esto podría reducir la capacidad de NCSI y de otros componentes que utilizan NCSI para determinar el acceso a Internet) NOTA: Existen otras directivas que le permiten redirigir las pruebas NCSI a los recursos internos, si quieres usar esta funcionalidad. |
| **Administración de comunicación de Internet**\\ Configuración de comunicación de Internet | Desactivar Informe de errores de Windows |  | Habilitada |
| **Administración de comunicación de Internet**\\ Configuración de comunicación de Internet | Desactivar la búsqueda de controladores de dispositivo de Windows Update |  | Habilitada |
| **Iniciar sesión** | Mostrar animación de primer inicio de sesión |  | Deshabilitada |
| **Iniciar sesión** | Desactivar las notificaciones de aplicaciones en la pantalla de bloqueo |  | Habilitada |
| **Iniciar sesión** | Desactivar el sonido inicio de Windows |  | Habilitada |
| **Administración de energía** | Seleccionar un plan de energía activo | Alto rendimiento | Habilitada |
| **Recuperación** | Permitir la restauración del sistema al estado predeterminado |  | Deshabilitada |
| \**_Estado de almacenamiento_* | Permitir la descarga de actualizaciones en el modelo de predicción de error de disco |  | Deshabilitado (las actualizaciones no se descargarán para el modelo de error de predicción de errores de disco). |
| \**_Servicios de hora de Windows_*\\Proveedores de hora | Habilitar el cliente de Windows NTP |  | Deshabilitado (si deshabilitas o no configuras esta configuración de directiva, el reloj del equipo local no se sincronizará con la hora con los servidores NTP) **NOTA**: *Sopesa esta configuración con cuidado*. Los dispositivos de Windows que están unidos a un dominio deben usar **NT5DS**. El controlador de dominio correspondiente al controlador de dominio principal debe usar NTP. El rol de PDCe puede usar NTP. Las máquinas virtuales a veces usan "mejoras" o "servicios de integración". |
| **Solución de problemas y diagnósticos**\\ Mantenimiento programado | Configurar el comportamiento del mantenimiento programado |  | Deshabilitada |
| **Solución de problemas y diagnósticos**\\ Diagnósticos de rendimiento del arranque de Windows | Configurar el nivel de ejecución del escenario |  | Deshabilitada |
| **Solución de problemas y diagnósticos**\\ Diagnósticos de fuga de memoria de Windows | Configurar el nivel de ejecución del escenario |  | Deshabilitada |
| **Solución de problemas y diagnósticos**\\ Detección y resolución del agotamiento de recursos de Windows | Configurar el nivel de ejecución del escenario |  | Deshabilitada |
| **Solución de problemas y diagnósticos**\\ Diagnósticos de rendimiento del apagado de Windows | Configurar el nivel de ejecución del escenario |  | Deshabilitada |
| **Solución de problemas y diagnósticos**\\ Diagnósticos de rendimiento del modo de espera y del modo de reanudación de Windows | Configurar el nivel de ejecución del escenario |  | Deshabilitada |
| **Solución de problemas y diagnósticos**\\ Diagnósticos de rendimiento de la capacidad de respuesta de Windows | Configurar el nivel de ejecución del escenario |  | Deshabilitada |
| \**_Perfiles de usuario_* | Desactivar el identificador de publicidad |  | Habilitado (si habilitas esta configuración de directiva, el id. de publicidad se desactivará. Las aplicaciones no pueden usar el id. para experiencias entre aplicaciones). |
| **Directiva de equipo local \\ Configuración del equipo \\ Plantillas administrativas \\ Componentes de Windows** |  |  |  |
| **Agregar características a Windows 10** | Evitar que el asistente se ejecute |  | Habilitada |
| \**_Privacidad de la aplicación_* | Permitir que las aplicaciones de Windows accedan a la información de la cuenta | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado (si eliges la opción **Forzar denegación**, las aplicaciones de Windows no podrán obtener acceso a la información de la cuenta y los empleados de tu organización no podrán cambiarla). |
| \**_Privacidad de la aplicación_* | Permitir que las aplicaciones de Windows accedan al historial de llamadas | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado (si eliges la opción **Forzar denegación**, las aplicaciones de Windows no podrán obtener acceso al historial de llamadas y los empleados de tu organización no podrán cambiarlo). |
| \**_Privacidad de la aplicación_* | Permitir que las aplicaciones de Windows accedan a los contactos | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado (si eliges la opción **Forzar denegación**, las aplicaciones de Windows no podrán obtener acceso a los contactos y los empleados de tu organización no podrán cambiarlos). |
| \**_Privacidad de la aplicación_* | Permitir que las aplicaciones de Windows obtengan acceso a la información de otras aplicaciones | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado (si deshabilitas o no configuras esta configuración de directiva, los empleados de tu organización podrán decidir si las aplicaciones de Windows pueden obtener información de diagnóstico sobre otras aplicaciones mediante la configuración \>Privacidad en el dispositivo). |
| \**_Privacidad de la aplicación_* | Permitir que las aplicaciones Windows accedan al correo electrónico | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado (si eliges la opción "Forzar permiso", las aplicaciones de Windows podrán obtener acceso al correo electrónico y los empleados de tu organización no podrán cambiarlo). |
| \**_Privacidad de la aplicación_* | Permitir que las aplicaciones de Windows accedan a la ubicación | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado (si eliges la opción **Forzar denegación**, las aplicaciones de Windows no podrán obtener acceso a la ubicación y los empleados de tu organización no podrán cambiarlos). |
| \**_Privacidad de la aplicación_* | Permitir que las aplicaciones de Windows accedan a la mensajería | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado (si eliges la opción **Forzar denegación**, las aplicaciones de Windows no podrán obtener acceso a la ubicación y los empleados de tu organización no podrán cambiarlos). |
| \**_Privacidad de la aplicación_* | Permitir que las aplicaciones Windows accedan al movimiento | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado (si eliges la opción **Forzar denegación**, las aplicaciones de Windows no podrán obtener acceso a los datos de movimiento y los empleados de tu organización no podrán cambiarlos). |
| \**_Privacidad de la aplicación_* | Permitir que las aplicaciones Windows accedan a las notificaciones | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado (si eliges la opción **Forzar denegación**, las aplicaciones de Windows no podrán obtener acceso a las notificaciones y los empleados de tu organización no podrán cambiarlas). |
| \**_Privacidad de la aplicación_* | Permitir que las aplicaciones Windows accedan a las tareas | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado (si eliges la opción **Forzar denegación**, las aplicaciones de Windows no podrán obtener acceso a las tareas y los empleados de tu organización no podrán cambiarlas). |
| \**_Privacidad de la aplicación_* | Permitir que las aplicaciones de Windows accedan al calendario | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado (si eliges la opción **Forzar denegación**, las aplicaciones de Windows no podrán obtener acceso al calendario y los empleados de tu organización no podrán cambiarlo). |
| \**_Privacidad de la aplicación_* | Permitir que las aplicaciones de Windows accedan a la cámara | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado (si eliges la opción **Forzar denegación**, las aplicaciones de Windows no podrán obtener acceso a la cámara y los empleados de tu organización no podrán cambiarla). |
| \**_Privacidad de la aplicación_* | Permitir que las aplicaciones de Windows accedan al micrófono | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado (si eliges la opción **Forzar denegación**, las aplicaciones de Windows no podrán obtener acceso al micrófono y los empleados de tu organización no podrán cambiarlo). |
| \**_Privacidad de la aplicación_* | Permitir que las aplicaciones de Windows accedan a los dispositivos de confianza | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado (si eliges la opción **Forzar denegación**, las aplicaciones de Windows no podrán obtener acceso a los dispositivos de confianza cámara y los empleados de tu organización no podrán cambiarlos). |
| \**_Privacidad de la aplicación_* | Permitir que las aplicaciones de Windows se comuniquen con dispositivos no emparejados | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado (si eliges la opción **Forzar denegación**, las aplicaciones de Windows no podrán comunicarse con dispositivos inalámbricos no emparejados y los empleados de tu organización no podrán cambiarlos). |
| \**_Privacidad de la aplicación_* | Permitir que las aplicaciones Windows accedan a las radios | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado (si eliges la opción **Forzar denegación**, las aplicaciones de Windows no podrán obtener acceso al control de radios y los empleados de tu organización no podrán cambiarlo). |
| **Privacidad de la aplicación** | Permitir que las aplicaciones de Windows hagan llamadas telefónicas | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado (las aplicaciones de Windows no pueden hacer llamadas telefónicas y los empleados de tu organización no pueden cambiar esta opción). |
| \**_Privacidad de la aplicación_* | Permitir que las aplicaciones de Windows se ejecuten en segundo plano | Predeterminado para todas las aplicaciones: Forzar denegación | Habilitado (si eliges la opción **Forzar denegación**, las aplicaciones de Windows no podrán ejecutarse en segundo plano y los empleados de tu organización no podrán cambiar esta opción). |
| **Directivas de reproducción automática** | Configurar el comportamiento predeterminado de la ejecución automática | No ejecutes ningún comando de ejecución automática | Habilitada |
| \**_Directivas de Reproducción automática_* | Desactivar la reproducción automática |  | Habilitado (si habilitas esta configuración de directiva, la reproducción automática se deshabilita en las unidades de CD-ROM y en los medios extraíbles, o se deshabilita en todas las unidades). |
| \**_Contenido de la nube_* | No mostrar sugerencias de Windows |  | Habilitado (esta configuración de directiva evita que las sugerencias de Windows se muestren a los usuarios). |
| \**_Contenido de la nube_* | Desactivar las experiencias del consumidor de Microsoft |  | Habilitado (si habilitas esta configuración de directiva, los usuarios ya no verán las recomendaciones personalizadas de Microsoft ni las notificaciones sobre su cuenta de Microsoft). |
| \**_Recopilación de datos y versiones preliminares_* | Permitir telemetría | 0: seguridad [solo Enterprise] | Habilitado (la configuración de un valor de 0 se aplica solo a los dispositivos que ejecutan las versiones Enterprise, Education, IoT o Windows Server). |
| \**_Recopilación de datos y versiones preliminares_* | No mostrar notificaciones de comentarios |  | Habilitada |
| \**_Recopilación de datos y versiones preliminares_* | Alternar control de usuario para compilaciones de Insider |  | Deshabilitada |
| **Optimización de distribución** | Modo de descarga | Modo de descarga: Simple (99) | 99 = Modo de descarga simple sin emparejamiento. La Optimización de distribución se descarga solo mediante HTTP y no intenta ponerse en contacto con los servicios en la nube de la Optimización de distribución. |
| **Administrador de ventanas de escritorio** | No permitir la invocación de Flip3D |  | Habilitada |
| **Administrador de ventanas de escritorio** | No permitir animaciones de ventanas |  | Habilitada |
| **Administrador de ventanas de escritorio** | Usar un or sólido para el fondo de Inicio |  | Habilitada |
| **Interfaz de usuario perimetral** | Deslizar rápidamente desde el borde |  | Deshabilitar |
| **Interfaz de usuario perimetral** | Deshabilitar las sugerencias de ayuda |  | Habilitada |
| \**_Explorador de archivos_* | Configurar SmartScreen de Microsoft Defender |  | Deshabilitado (SmartScreen se desactivará para todos los usuarios. No se avisará a los usuarios si intentan ejecutar aplicaciones sospechosas desde Internet). |
|  |  |  | **NOTA**: Si no estás conectado a Internet, evitarás que los equipos intenten comunicarse con Microsoft para obtener información de SmartScreen. |
| **Explorador de archivos** | No mostrar la notificación de la **nueva aplicación instalada** |  | Habilitada |
| \**_Encontrar mi dispositivo_* | Activar o desactivar Encontrar mi dispositivo |  | Deshabilitado (cuando la opción Encontrar mi dispositivo está desactivada, el dispositivo y su ubicación no están registrados y la característica no funcionará). El usuario tampoco podrá ver la ubicación del último uso de su digitalizador activo en el dispositivo). |
| **Explorador de juegos** | Desactivar la descarga de información del juego |  | Habilitada |
| **Explorador de juegos** | Desactivar las actualizaciones de juegos |  | Habilitada |
| **Explorador de juegos** | Desactivar el seguimiento de las últimas partidas en la carpeta Juegos |  | Habilitada |
| **Grupo Hogar** | Impedir que el equipo se una a un grupo en el hogar |  | Habilitada |
| \**_Internet Explorer_* | Permitir que los servicios Microsoft proporcionen sugerencias mejoradas mientras el usuario escribe en la barra de direcciones |  | Deshabilitado (los usuarios reciben sugerencias mejoradas mientras escriben en la barra de direcciones. Además, los usuarios no podrán cambiar la configuración de Sugerencias). |
| **Internet Explorer** | Deshabilitar la comprobación periódica de actualizaciones de software de Internet Explorer |  | Habilitada |
| **Internet Explorer** | Deshabilitar la visualización de la pantalla de presentación |  | Habilitada |
| **Internet Explorer** | Instalar nuevas versiones de Internet Explorer automáticamente |  | Deshabilitada |
| **Internet Explorer** | Evitar la participación en el Programa para la mejora de la experiencia del usuario |  | Habilitada |
| **Internet Explorer** | Evitar la ejecución del Asistente de primera ejecución | Ve directamente a la página principal | Habilitada |
| **Internet Explorer** | Establecer el crecimiento del proceso de pestañas | Baja | Habilitada |
| **Internet Explorer** | Especificar el comportamiento predeterminado de una nueva pestaña | Página de la nueva pestaña | Habilitada |
| **Internet Explorer** | Desactivar las notificaciones de rendimiento de complementos |  | Habilitada |
| \**_Internet Explorer_* | Desactivar la característica Autocompletar para direcciones web |  | Habilitado (si habilitas esta configuración de directiva, no se sugerirán coincidencias al usuario cuando escriba direcciones web). El usuario no podrá cambiar el autocompletado para configurar las direcciones web). |
| \**_Internet Explorer_* | Desactivar geoubicación del explorador |  | Habilitado (si habilitas esta configuración de directiva, la compatibilidad de geolocalización del navegador estará desactivada). |
| **Internet Explorer** | Desactivar la opción para volver a abrir la última sesión de navegación |  | Habilitada |
| \**_Internet Explorer_* | Activar los sitios sugeridos |  | Deshabilitado (si deshabilitas esta configuración de directiva, los puntos de entrada y la funcionalidad asociada con esta característica se desactivarán). |
| **\*Internet Explorer**\\ Vista de compatibilidad | Desactivar la vista de compatibilidad |  | Habilitado (si habilitas esta configuración de directiva, el usuario no puede usar el botón Vista de compatibilidad ni administrar la lista de sitios de la Vista de compatibilidad). |
| **Internet Explorer**\\ Panel de control de Internet <strong>\\</strong> Página de opciones avanzadas | Reproducir animaciones en páginas web |  | Deshabilitada |
| **Internet Explorer**\\ Panel de control de Internet\\ Página de opciones avanzadas | Reproducir vídeos en páginas web |  | Deshabilitada |
| **\*Internet Explorer**\\ Panel de control de Internet\\ Página de opciones avanzadas | Desactivar las características correspondientes a Pasar de página con predicción de página |  | Habilitado (Microsoft recopila tu historial de exploración para mejorar el funcionamiento de Pasar de página con predicción de página. Esta característica no está disponible para Internet Explorer para el escritorio. Si habilitas esta configuración de directiva, se desactiva Pasar de página con predicción de página y la siguiente página web no se carga en segundo plano). |
| **Internet Explorer**\\ Configuración de Internet\\ Configuración avanzada\\ Exploración | Desactivar detección de número de teléfono |  | Habilitada |
| **Internet Explorer**\\ Configuración de Internet\\ Configuración avanzada\\ Multimedia | Permitir que Internet Explorer reproduzca archivos multimedia que usan códecs alternativos |  | Deshabilitada |
| \**_Ubicación y sensores_* | Desactivar la ubicación |  | Habilitado (si habilitas esta configuración de directiva, la característica de ubicación se desactiva y se impide que todos los programas del equipo usen la información de ubicación de la característica de ubicación). |
| **Ubicación y sensores** | Desactivar sensores |  | Habilitada |
| **Ubicaciones y sensores /** Servicio de localización de Windows | Desactivar el Servicio de localización de Windows |  | Habilitada |
| \**_Mapas_* | Desactivar la descarga y actualización automáticas de los datos de mapa |  | Habilitado (si habilitas esta configuración, la descarga y actualización automáticas de los datos del mapa se desactivarán). |
| \**_Mapas_* | Desactivar el tráfico de red no solicitado en la página Configuración de mapas sin conexión |  | Habilitado (si habilitas esta configuración de directiva, las características que generan tráfico de red en la página de configuración de Mapas sin conexión se desactivarán. Nota: Ten en cuenta que esto podría desactivar toda la página de configuración). |
| \**_Mensajes_* | Permitir la sincronización en la nube del servicio de mensajes |  | Deshabilitado (esta configuración de directiva permite realizar copias de seguridad y restaurar mensajes de texto de móvil en los servicios en la nube de Microsoft). |
| \**_Microsoft Edge_* | Permitir sugerencias en la lista desplegable de la barra de direcciones |  | Deshabilitada |
| \**_Microsoft Edge_* | Permitir actualizaciones de configuración para la biblioteca de libros |  | Deshabilitado (desactiva las listas de compatibilidad en Microsoft Edge). |
| \**_Microsoft Edge_* | Permitir la lista de compatibilidad de Microsoft |  | Deshabilitado (si deshabilitas esta configuración, la lista de compatibilidad de Microsoft no se usa durante la navegación del explorador). |
| \**_Microsoft Edge_* | Permitir contenido web en la página Nueva pestaña |  | Deshabilitado (indica que Edge se abra con contenido en blanco cuando se abre una nueva pestaña). |
| \**_Microsoft Edge_* | Configurar Autorrellenar |  | Deshabilitado (desactiva la función de autocompletar en la barra de direcciones). |
| \**_Microsoft Edge_* | Configurar No realizar seguimiento |  | Habilitado (si habilitas esta configuración, siempre se enviarán solicitudes de No realizar seguimiento a los sitios web que piden información de seguimiento). |
| \**_Microsoft Edge_* | Configurar el Administrador de contraseñas |  | Deshabilitado (si deshabilitas esta configuración, los empleados no podrán usar el Administrador de contraseñas para guardar sus contraseñas localmente). |
| \**_Microsoft Edge_* | Configurar sugerencias de búsqueda en la barra de direcciones |  | Deshabilitado (los usuarios no pueden ver las sugerencias de búsqueda en la barra de direcciones de Microsoft Edge). |
| \**_Microsoft Edge_* | Configurar las páginas de inicio |  | Habilitado (si habilitas esta configuración, puedes configurar una o varias páginas de inicio). Si esta configuración está habilitada, también debes incluir direcciones URL a las páginas y separar varias páginas con corchetes angulares con este formato: \<support.contoso.com\>\<support.microsoft.com\> Windows 10, versión 1703 o posterior: Si no quieres enviar tráfico a Microsoft, puedes usar el valor \<about:blank\>, cuando es la única URL configurada, porque se admite en los dispositivos sin importar si están unidos a un dominio o no. |
| \**_Microsoft Edge_* | Configurar SmartScreen de Microsoft Defender |  | Deshabilitado (SmartScreen de Windows Defender está desactivado y los empleados no pueden activarlo). |
|  |  |  | **NOTA**: Ten a en cuenta esta configuración dentro del entorno. Si no estás conectado a Internet, evitarás que los equipos intenten comunicarse con Microsoft para obtener información de SmartScreen. |
| \**_Microsoft Edge_* | Evitar que la página web de primera ejecución se abra en Microsoft Edge |  | **Habilitado** (los usuarios no verán la página de primera ejecución al abrir Microsoft Edge por primera vez). |
| **OneDrive** | Impedir que OneDrive genere tráfico de red hasta que el usuario inicie sesión en OneDrive |  | **Habilitado** (habilita esta configuración para evitar que el cliente de sincronización de OneDrive [OneDrive.exe] genere tráfico en la red [comprobación de actualizaciones, etc.] hasta que el usuario inicie sesión en OneDrive o comience a sincronizar archivos en el equipo local). |
| \**_OneDrive_* | Impedir el uso de OneDrive para almacenar archivos |  | **Habilitado** (a menos que OneDrive se use dentro o fuera de las instalaciones). |
| **OneDrive** | Guardar los documentos en OneDrive de forma predeterminada |  | **Deshabilitado** (a menos que OneDrive se use dentro o fuera de las instalaciones). |
| **Fuentes RSS** | Evitar el descubrimiento automático de las fuentes e instancias de Web Slice |  | **Habilitado** |
| \**_Fuentes RSS_* | Desactivar la sincronización en segundo plano de fuentes y Web Slices |  | **Habilitado** (si habilitas esta configuración de directiva, la capacidad de sincronizar fuentes e instancias de Web Slice en segundo plano se desactiva). |
| \**_Buscar_* | Permitir Cortana |  | **Deshabilitado** (cuando Cortana está desactivada, los usuarios aún podrán usar la opción de búsqueda para encontrar cosas en el dispositivo). |
| **Búsqueda** | Permitir que Cortana funcione sobre la pantalla de bloqueo |  | **Deshabilitado** |
| \**_Buscar_* | Permitir el uso de la ubicación para las búsquedas y Cortana |  | **Deshabilitado** |
| **Búsqueda** | No permitir búsquedas en la Web |  | **Habilitado** |
| \**_Buscar_* | No buscar en Internet ni mostrar resultados de Internet en la búsqueda |  | **Habilitado** (si habilitas esta configuración de directiva, las consultas no se realizarán en la web y los resultados web no se mostrarán cuando un usuario realice una consulta con la opción Buscar). |
| **Búsqueda** | Evitar agregar ubicaciones UNC al índice desde el Panel de control |  | **Habilitado** |
| **Búsqueda** | Evitar la indexación de archivos en la caché de archivos sin conexión |  | **Habilitado** |
| \**_Buscar_* | Definir qué información se comparte en las búsquedas | Información anónima | **Habilitado** (comparte la información de uso, pero no el historial de búsqueda, la información de la cuenta de Microsoft o la ubicación específica). |
| \**_Plataforma de protección de software_* | Desactivar la validación de AVC en línea del cliente KMS |  | **Habilitado** (habilitar esta configuración evita que este equipo envíe datos a Microsoft con respecto a su estado de activación). |
| \**_Voz_* | Permitir la actualización automática de los datos de voz |  | **Deshabilitado** (no verificará periódicamente los modelos de voz actualizados). |
| \**_Store_* | Desactivar la descarga e instalación automáticas de las actualizaciones |  | **Habilitado** (si habilitas esta configuración, la descarga e instalación automáticas de las actualizaciones de aplicaciones se desactivarán). |
| \**_Store_* | Desactivar la descarga automáticas de las actualizaciones en dispositivos de Win8 |  | **Habilitado** (si habilitas esta configuración, la descarga automática de las actualizaciones de aplicaciones se desactivará). |
| **Store** | Desactivar la oferta para actualizar a la última versión de Windows |  | **Habilitado** |
| \**_Sincronizar la configuración_* | No sincronizar | Permitir a los usuarios activar la sincronización (no seleccionado) | **Habilitado** (si habilitas esta configuración de directiva, la opción "sincronizar la configuración" se desactivará, y ninguno de los grupos de la "sincronización de la configuración" se sincronizarán en este dispositivo. |
| **Entrada de texto** | Mejorar el reconocimiento de la entrada manuscrita y escritura por teclado |  | **Deshabilitado** |
| **Antivirus de Windows Defender**\\ MAPS | Unirse a Microsoft MAPS |  | **Deshabilitado** (si deshabilitas o no configuras esta opción, no te unirás a Microsoft MAPS). |
| **Antivirus de Windows Defender**\\ MAPS | Enviar muestras de archivos cuando se requieran más análisis | No enviar nunca | **Habilitado** (solo si no participas en la opción de envío de los datos de diagnóstico de MAPS). |
| **Antivirus de Windows Defender**\\ Generación de informes | Desactivar notificaciones mejoradas |  | **Habilitado** (si habilitas esta configuración, las notificaciones mejoradas del Antivirus de Windows Defender no se mostrarán en los clientes). |
| **Antivirus de Windows Defender**\\ Actualizaciones de firmas | Definir el orden de los orígenes para descargar actualizaciones de definiciones | FileShares | **Habilitado** (si habilitas esta configuración, se contactará con los orígenes de actualización de las definiciones en el orden especificado). Una vez que las actualizaciones de definiciones se hayan descargado correctamente de una fuente específica, no se contactará con las fuentes restantes de la lista). |
| **Informe de errores de Windows** | Enviar automáticamente volcados de memoria para informes de errores que haya generado el sistema operativo |  | **Deshabilitado** |
| **Informe de errores de Windows** | Deshabilitar Informe de errores de Windows |  | **Habilitado** |
| **Grabación y difusión de juegos de Windows** | Habilita o deshabilita la opción de grabación y difusión de juegos de Windows. |  | **Deshabilitado** |
| **Windows Installer** | Tamaño máximo del control de caché de archivos de la línea base | 5 | **Habilitado** |
| **Windows Installer** | Desactivar la creación de puntos de control de Restaurar sistema |  | **Habilitado** |
| **Windows Mail** | Desactivar la característica de comunidades |  | **Habilitado** |
| **Reproductor de Windows Media** | No mostrar los cuadros de diálogo de primer uso |  | **Habilitado** |
| **Windows Media Player** | Evitar el uso compartido de elementos multimedia |  | **Habilitado** |
| **Centro de movilidad de Windows** | Desactivar el Centro de movilidad de Windows |  | **Habilitado** |
| **Análisis de confiabilidad de Windows** | Configurar proveedores WMI de confiabilidad |  | **Deshabilitado** |
| **Windows Update** | Permitir la instalación inmediata de actualizaciones automáticas |  | **Habilitado** |
| **Windows Update** | No conectarse a ninguna ubicación de Internet de Windows Update |  | **Habilitado** (si habilitas esta directiva se deshabilitará esa funcionalidad y podría causar que la conexión a los servicios públicos como Microsoft Store dejen de funcionar. **Nota:** Esta directiva solo se aplica cuando el dispositivo está configurado para conectarse a un servicio de actualización de intranet mediante la directiva "Especificar la ubicación del servicio Microsoft Update en la intranet". |
| **Windows Update** | Desactivar el acceso a todas las características de Windows Update |  | **Habilitado** |
| **\*Windows Update**\\ Windows Update para empresas | Administrar las versiones preliminares | Establecer el comportamiento para recibir versiones preliminares: | **Habilitado** (al seleccionar **Deshabilitar versiones preliminares** evitarás que las versiones preliminares se instalen en el dispositivo. Esto evitará que los usuarios se inscriban en el Programa Windows Insider, a través de la opción de configuración -\> Actualización y seguridad). |
|  |  | Deshabilitar las versiones preliminares |  |
| **\*Windows Update**\\ Windows Update para empresas | Seleccionar cuándo se reciben las versiones preliminares y las actualizaciones de características | Canal semianual | **Habilitado** (habilita esta directiva para especificar el nivel de versión preliminar o actualizaciones de características para recibirlas y cuándo). |
|  |  | Aplazamiento: 365 días, |  |
|  |  | Inicio de la pausa: aaaa-mm-dd |  |
| **Windows Update**\\ Windows Update para empresas | Selecciona cuándo se reciben las actualizaciones de calidad | 1. 30 días 2. Pausar las actualizaciones de calidad a partir del aaaa-mm-dd | **Habilitado** |
| **Configuración de directivas personalizadas de tráfico de restricciones de Windows** | Impedir que OneDrive genere tráfico de red hasta que el usuario inicie sesión en OneDrive |  | **Habilitado** (habilita esta configuración si quieres evitar que el cliente de sincronización de OneDrive [OneDrive.exe] genere tráfico en la red [comprobación de actualizaciones, etc.] hasta que el usuario inicie sesión en OneDrive o comience a sincronizar archivos en el equipo local). |
| **Configuración de directivas personalizadas de tráfico de restricciones de Windows** | Desactivar las notificaciones de Windows Defender |  | **Habilitado** (si habilitas esta configuración de directiva, Windows Defender no enviará notificaciones con información crítica sobre el estado y la seguridad de tu dispositivo). |
| **Directiva de equipo local \\ Configuración de usuario \\ Plantillas administrativas** |  |  |  |
| **Panel de control**\\ Opciones regionales y de idioma | Desactivar la oferta de predicciones de texto al escribir |  | **Habilitado** |
| **Dispositivo de escritorio** | No agregar recursos compartidos de documentos abiertos recientemente a las ubicaciones de red |  | **Habilitado** |
| **Dispositivo de escritorio** | Desactivar el gesto del mouse de minimización de ventanas de Aero Shake |  | **Habilitado** |
| **Escritorio** / Active Directory | Tamaño máximo de las búsquedas de Active Directory | 2500 | **Habilitado** |
| **Menú Inicio y barra de tareas** | No permitir el anclado de la aplicación Store a la barra de tareas |  | **Habilitado** |
| **Menú Inicio y barra de tareas** | No mostrar elementos, ni realizar seguimiento de los mismos, en las Jump Lists desde ubicaciones remotas |  | **Habilitado** |
| **Menú Inicio y barra de tareas** | No uses el método basado en la búsqueda al resolver accesos directos de shell |  | **Habilitado** (el sistema no realiza la búsqueda final de la unidad de disco. Simplemente muestra un mensaje que explica que el archivo no se encuentra). |
| **Menú Inicio y barra de tareas** | Eliminar la barra de Contactos de la barra de tareas |  | **Habilitado** (el icono de contactos se eliminará de la barra de tareas, la configuración de alternancia correspondiente se eliminará de la página de configuración de la barra de tareas y los usuarios no podrán anclar contactos a la barra de tareas). |
| **Menú Inicio y barra de tareas** | Desactivar los globos de notificaciones de anuncios de características |  | **Habilitado** (los usuarios no pueden anclar la aplicación Store en la barra de tareas. Si la aplicación Store ya está anclada en la barra de tareas, se eliminará de la misma en el próximo inicio de sesión.) |
| **Menú Inicio y barra de tareas** | Desactivar el seguimiento de usuarios |  | **Habilitado** |
| **Menú Inicio y la barra de tareas** / Notificaciones | Desactivar notificaciones del sistema |  | **Habilitado** |
| **Componentes de Windows** / Contenido en la nube | Desactivar todas las características de Contenido destacado de Windows |  | **Habilitado** |
| **Interfaz de usuario perimetral** | Desactivar el seguimiento del uso de la aplicación |  | **Habilitado** |
| **Explorador de archivos** | Desactivar el almacenamiento en caché de imágenes en miniatura |  | **Habilitado** |
| **Explorador de archivos** | Desactivar la visualización de las entradas de búsqueda recientes en el cuadro de búsqueda del Explorador de archivos |  | **Habilitado** |
| **Explorador de archivos** | Desactivar el almacenamiento en caché de las miniaturas del archivo oculto thumbs.db |  | **Habilitado** ||

### <a name="notes-about-network-connectivity-status-indicator"></a>Notas sobre el indicador de estado de conectividad de red

La configuración de la directiva de grupo anterior incluye opciones de configuración para desactivar la comprobación para ver si el sistema está conectado a Internet. Si tu entorno no se conecta a Internet de ninguna manera o se conecta de forma indirecta, puedes establecer una configuración de directiva de grupo para eliminar el icono de red de la barra de tareas. Si quieres eliminar el ícono de red de la barra de tareas, probablemente se deba a que has desactivado las comprobaciones de conectividad a Internet; recuerda que habrá una marca amarilla en el ícono de red, aunque la red esté funcionando normalmente. Si quieres eliminar el icono de red como una configuración de directiva de grupo, puedes encontrarlo en esta ubicación:

| Windows Update o Windows Update para empresas | Selecciona cuándo quieres recibir actualizaciones de calidad | 1. 30 días 2. Pausar las actualizaciones de calidad a partir del aaaa-mm-dd | Habilitada |
|--|--|--|--|
| **Directiva de equipo local \\ Configuración de usuario \\ Plantillas administrativas** |  |  |  |
| **Menú Inicio y barra de tareas** | Quitar el icono de redes |  | **Habilitado** (el icono de redes no se muestra en el área de notificaciones del sistema). |

Para obtener más información sobre el Indicador de estado de la conexión de red (NCSI), consulta: [Icono del estado de conexión de red](https://techcommunity.microsoft.com/t5/networking-blog/bg-p/NetworkingBlog)

### <a name="system-services"></a>Servicios del sistema

Si quieres deshabilitar los servicios del sistema para conservar los recursos, asegúrate de que el servicio que quieres deshabilitar no sea un componente de otro servicio.

Asimismo, la mayoría de estas recomendaciones también pueden aplicarse para Windows Server 2016 con Experiencia de escritorio; para obtener más información, consulta la [ Guía para deshabilitar los servicios del sistema en Windows Server 2016 con Experiencia de escritorio](../../security/windows-services/security-guidelines-for-disabling-system-services-in-windows-server.md).

Ten en cuenta que muchos de los servicios quieras deshabilitar también están configurados para el tipo de inicio de servicio manual. Esto significa que el servicio no se iniciará automáticamente y no se iniciará a menos que una aplicación o servicio específico desencadene una solicitud a ese servicio que quieres deshabilitar. Los servicios que ya están configurados para iniciar el manual de tipo, no se enumeran aquí por lo general.

| Servicio de Windows | Elemento | Comentario |
|--|--|--|
| CDPUserService | Este servicio de usuario se usa en los escenarios de la plataforma de dispositivos conectados. | NOTA: Este es un servicio por usuario y, como tal, el *servicio de plantillas* debe estar deshabilitado. |
| Telemetría y experiencias del usuario conectado | Habilita características que admiten experiencias de usuario conectadas y en la aplicación. Asimismo, este servicio administra la recopilación y transmisión de la información de diagnóstico y uso basada en los eventos (se usa para mejorar la experiencia y la calidad de la plataforma de Windows) cuando la configuración de opciones de diagnóstico y privacidad de uso está habilitada en la opción Comentarios y diagnósticos. | Puedes deshabilitar esta opción si la red está desconectada. |
| Datos de contacto | Indexa los datos de contacto para poder buscar los contactos rápidamente. Si detienes o deshabilitas este servicio, es posible que falten contactos en tus resultados de búsqueda. | (PimIndexMaintenanceSvc) NOTA: Este es un servicio por usuario y, como tal, el *servicio de plantillas* debe estar deshabilitado. |
| Servicio de directivas de diagnóstico | Permite la detección, solución y resolución de problemas para componentes de Windows. Si este servicio se detiene, los diagnósticos ya no funcionarán. |  |
| Administrador de mapas descargados | Servicio de Windows para que las aplicaciones obtengan acceso a mapas descargados. Este servicio lo pide la aplicación que accederá a los mapas descargados. Si deshabilitas este servicio las aplicaciones no obtendrán acceso a los mapas. |  |
| Servicio de geolocalización | Supervisa la ubicación actual del sistema y gestiona las geovallas. |  |
| Servicio de usuario de GameDVR y difusión | Este servicio de usuario se usa en grabaciones de juegos y difusiones en directo. | NOTA: Este es un servicio por usuario y, como tal, el servicio de plantillas debe estar deshabilitado. |
| MessagingService | Servicio de compatibilidad con mensajes de texto y funcionalidades relacionadas. | NOTA: Este es un servicio por usuario y, como tal, el *servicio de plantillas* debe estar deshabilitado. |
| Optimizar unidades | Permite que el equipo funcione de forma más eficiente al optimizar los archivos en las unidades de almacenamiento. | Las soluciones VDI normalmente no se benefician de la optimización del disco. Estas "unidades" no son unidades tradicionales y, a menudo, son solo una asignación de almacenamiento temporal. |
| SuperFetch | Mantiene y mejora el rendimiento del sistema a lo largo del tiempo. | En general, no mejora el rendimiento en VDI (especialmente cuando es no persistente) dado que el estado del sistema operativo se descarta en cada reinicio. |
| Servicio de teclado táctil y panel de escritura a mano | Habilita las funciones de lápiz y entradas de lápiz del teclado táctil y del panel de escritura a mano. |  |
| Informe de errores de Windows | Permite que se informe acerca de los errores cuando los programas dejan de funcionar o responder y permite que se entreguen las soluciones existentes. También permite generar registros para los servicios de diagnóstico y reparaciones. Si se detiene este servicio, es posible que el informe de errores no funcione correctamente y que no se muestren los resultados de los servicios de diagnóstico y reparaciones. | Con VDI, los diagnósticos se realizan a menudo en un escenario sin conexión y no en la producción convencional. Asimismo, algunos clientes deshabilitan WER de todos modos. WER usa una pequeña cantidad de recursos para hacer muchas cosas diferentes, incluyendo mostrar un error al instalar un dispositivo o un error al instalar una actualización. |
| Servicio de uso compartido de red del Reproductor de Windows Media | Comparte las bibliotecas de Windows Media Player con otros reproductores de red y dispositivos multimedia con Universal Plug and Play. | No es necesario a menos que los clientes compartan bibliotecas WMP en la red. |
| Servicio de zona con cobertura inalámbrica móvil de Windows | Proporciona la capacidad de compartir una conexión de datos móviles con otro dispositivo. |  |
| Windows Search | Proporciona la indexación del contenido, el almacenamiento en caché de propiedades y los resultados de la búsqueda de archivos, correo electrónico y otro contenido. | Probablemente no sea necesario, especialmente con VDI no persistentes. |

#### <a name="per-user-services-in-windows"></a>Servicios por usuario en Windows

[Los servicios por usuario](/windows/application-management/per-user-services-in-windows) son servicios que se crean cuando un usuario inicia sesión en Windows o Windows Server y se detienen y eliminan cuando ese usuario cierra la sesión. Estos servicios se ejecutan en un contexto de seguridad de la cuenta de usuario; esto proporciona una administración de recursos mejor que la del enfoque anterior para ejecutar este tipo de servicios en el explorador, el cual asociado a una cuenta preconfigurada o como tareas.

### <a name="scheduled-tasks"></a>Tareas programadas

Al igual que otros elementos en Windows, asegúrate de que un elemento no sea necesario antes de desactivarlo.

En la siguiente lista, las tareas que se muestran son aquellas que realizan optimizaciones o recopilaciones de datos en equipos que mantienen su estado durante los reinicios. Cuando una tarea de VM de VDI se reinicia y descarta todos los cambios desde el último arranque, las optimizaciones destinadas a los equipos físicos no son útiles.

Puedes obtener todas las tareas programadas actuales, incluyendo las descripciones, con el siguiente código de PowerShell:

`Get-ScheduledTask | Select-Object -Property TaskPath,TaskName,State,Description |Export-CSV -Path C:\Temp\W10_1803_SchTasks.csv -NoTypeInformation`

Los valores válidos del **nombre de la tarea programada**  incluyen:

- Tarea de actualización de OneDrive independiente (v2)
- Compatibilidad con Microsoft Appraiser
- ProgramDataUpdater
- StartupAppTask
- CleanupTemporaryState
- Proxy
- UninstallDeviceTask
- ProactiveScan
- Consolidator
- UsbCeip
- Examen de integridad de datos
- Examen de integridad de datos para la recuperación tras bloqueo
- ScheduledDefrag
- SilentCleanup
- Microsoft-Windows-DiskDiagnosticDataCollector
- Diagnóstico
- StorageSense
- DmClient
- DmClientOnScenarioDownload
- Historial de archivos (modo de mantenimiento)
- ScanForUpdates
- ScanForUpdatesAsUser
- SmartRetry
- Notificaciones
- WindowsActionDialog
- Red de telefonía móvil WinSAT
- MapsToastTask
- ProcessMemoryDiagnosticEvents
- RunFullMemoryDiagnostic
- Analizador de metadatos MNO
- LPRemove
- GatherNetworkInfo
- WiFiTask
- Sqm-Tasks
- AnalyzeSystem
- MobilityManager
- VerifyWinRE
- RegIdleBackup
- FamilySafetyMonitor
- FamilySafetyRefreshTask
- IndexerAutomaticMaintenance
- SpaceAgentTask
- SpaceManagerTask
- HeadsetButtonPress
- SpeechModelDownloadTask
- ResPriStaticDbSync
- WsSwapAssessmentTask
- SR
- SynchronizeTimeZone
- Usb-Notifications
- QueueReporting
- UpdateLibrary
- Scheduled Start
- sih
- XblGameSaveTask

### <a name="apply-windows-and-other-updates"></a>Aplicar Windows y otras actualizaciones

Desde Microsoft Update, o desde los recursos internos, se aplican las actualizaciones disponibles, incluidas las firmas de Windows Defender. Este es un buen momento para aplicar otras actualizaciones disponibles, incluidas las de Microsoft Office, si están instaladas.

### <a name="automatic-windows-traces"></a>Seguimientos de Windows automáticos

Windows está configurado, de forma predeterminada, para recopilar y guardar datos de diagnóstico limitados. El propósito es habilitar los diagnósticos o registrar datos en caso de que sea necesario solucionar problemas adicionales. Puedes encontrar opciones automáticas de seguimiento del sistema iniciando la aplicación Administración de equipos y expandiendo **Herramientas del sistema**, **Rendimiento**, **Conjuntos de recopiladores de datos** y, a continuación, seleccionando **Sesiones de seguimiento de eventos**.

Algunas de las opciones de seguimiento mostradas en **Sesiones de seguimiento de eventos** y **Sesiones de seguimiento de eventos de inicio** no pueden y no deben detenerse. Otras opciones, como el seguimiento de tipo **WiFiSession** se pueden detener. Para detener un seguimiento en ejecución en **Sesiones de seguimiento de eventos**, haz clic con el botón derecho en el seguimiento y selecciona **Detener**. Para evitar que los seguimientos se inicien automáticamente en el inicio, sigue estos pasos:

1.  Selecciona la carpeta **Sesiones de seguimiento de eventos de inicio**

2.  Localiza el seguimiento que te interesa y haz doble clic en el mismo.

3.  Selecciona la pestaña **Realizar seguimiento de sesión**.

4.  Desactiva la casilla **Habilitada**.

5.  Selecciona **Aceptar**.

Estos son algunos seguimientos del sistema que puedes deshabilitar en VDI:

| Nombre | Comentario |
|--|--|
| AppModel | Una colección de seguimientos, uno de los cuales es el teléfono |
| CloudExperienceHostOOBE |  |
| DiagLog |  |
| NtfsLog |  |
| TileStore |  |
| UBPM |  |
| WiFiDriverIHVSession | Si no estás usando un dispositivo WiFi |
| WiFiSession |  |

#### <a name="servicing-the-operating-system-and-apps"></a>Mantenimiento del sistema operativo y las aplicaciones

En algún momento durante el proceso de optimización de la imagen, se deben aplicar las actualizaciones de Windows. Puedes configurar Windows Update para instalar actualizaciones para otros productos de Microsoft, así como para Windows. Para configurar esto, abre **Configuración de Windows**, selecciona **Actualización y seguridad** y **Opciones avanzadas**. Selecciona **Ofrecer actualizaciones para otros productos de Microsoft cuando actualice Windows** y establece esta opción como **Activada**.

Esta es una buena configuración en caso de que vayas a instalar aplicaciones de Microsoft como Microsoft Office en la imagen base. De esta forma, Office se actualiza cuando la imagen se agrega al servicio. Asimismo, también hay actualizaciones de .NET y ciertos componentes que no son de Microsoft, como Adobe, que tienen actualizaciones disponibles a través de Windows Update.

Es muy importante que tengas en cuenta las actualizaciones de seguridad para las máquinas virtuales de VDI no persistente, incluyendo los archivos de definición de software de seguridad. Estas actualizaciones se pueden publicar una vez o incluso más de una vez por día. Además, existe una forma de conservar estas actualizaciones, incluidos los componentes de Windows Defender y los que no son de Microsoft.

En cuanto a Windows Defender, es mejor permitir que se realicen las actualizaciones, incluso en VDI no persistente. Las actualizaciones se aplicarán a casi todas las sesiones de inicio de sesión, pero recuerda que estas son pequeñas y no deberían suponer un problema. Además, la VM no se retrasará en las actualizaciones porque solo se aplicará la última disponible. Lo mismo podría aplicarse en los archivos de definición que no sean de Microsoft.

> [!NOTE]
> Las aplicaciones de Store (aplicaciones UWP) se actualizan a través de Microsoft Store. Las versiones modernas de Office, como Microsoft 365, se actualizan a través de sus propios mecanismos cuando se conectan directamente a Internet o a través de tecnologías de administración cuando no lo hacen.

### <a name="windows-defender-optimization-with-vdi"></a>Optimización de Windows Defender con VDI

Microsoft ha publicado recientemente documentación sobre Windows Defender en un entorno de VDI. Consulta la [Guía de implementación del Antivirus de Windows Defender en un entorno de infraestructura de escritorio virtual (VDI)](/windows/security/threat-protection/windows-defender-antivirus/deployment-vdi-windows-defender-antivirus) para obtener más detalles.

En el artículo anterior verás procedimientos para dar servicio la imagen de VDI de tipo "gold" y sabrás cómo mantener los clientes de VDI mientras se ejecutan. Para reducir el uso de ancho de banda de la red cuando los equipos de VDI necesiten actualizar sus firmas de Windows Defender, es recomendable escalonar los reinicios y programarlos durante tus horas libres, siempre que sea posible. Las actualizaciones de firmas de Windows Defender pueden estar en los recursos compartidos de archivos de forma interna y, cuando sean necesarios, esos archivos compartidos se encuentran en los mismos segmentos de red o en segmentos de red cercanos a las máquinas virtuales de VDI.

Consulta el documento que se encuentra al principio de esta sección para obtener más información sobre cómo optimizar Windows Defender con VDI.

### <a name="tuning-windows-10-network-performance-by-using-registry-settings"></a>Ajustar el rendimiento de red de Windows 10 mediante la configuración del Registro

Esto es especialmente importante en entornos donde VDI o el equipo físico tiene una carga de trabajo que se basa principalmente en la red. La configuración de esta sección sesga el rendimiento para favorecer las redes; para ello, se configura un búfer adicional y se almacenan en caché cosas como entradas de directorio, etc.

Ten en cuenta que algunas configuraciones de esta sección se basan *únicamente en el registro* y deben incorporarse en la imagen base antes de implementarla para su uso en la producción.

La siguiente configuración se documenta en la información que proporciona las [instrucciones de ajustes de rendimiento de Windows Server 2016](../../administration/performance-tuning/index.md), que publicó en Microsoft.com el grupo de productos de Windows.

#### <a name="disablebandwidththrottling"></a>DisableBandwidthThrottling

HKLM\\System\\CurrentControlSet\\Services\\LanmanWorkstation\\Parameters\\DisableBandwidthThrottling

Válido para Windows 10. El valor predeterminado es **0**. De manera predeterminada, el redirector de SMB limita el rendimiento en las conexiones de red de alta latencia, en algunos casos para evitar tiempos de espera relacionados con la red. Establecer este valor del Registro en **1** deshabilita esta limitación y permite un mayor rendimiento de transferencia de archivos a través de conexiones de red de alta frecuencia, así que es recomendable usar esta configuración.

#### <a name="fileinfocacheentriesmax"></a>FileInfoCacheEntriesMax

HKLM\\System\\CurrentControlSet\\Services\\LanmanWorkstation\\Parameters\\FileInfoCacheEntriesMax

Válido para Windows 10. El valor predeterminado es **64**, con un intervalo válido entre 1 y 65536. Este valor se usa para determinar la cantidad de metadatos de archivo que el cliente puede almacenar en caché. Aumentar el valor puede disminuir el tráfico de red y aumentar el rendimiento cuando se accede a varios archivos. Intenta aumentar este valor a **1024**.

#### <a name="directorycacheentriesmax"></a>DirectoryCacheEntriesMax

HKLM\\System\\CurrentControlSet\\Services\\LanmanWorkstation\\Parameters\\DirectoryCacheEntriesMax

Válido para Windows 10. El valor predeterminado es **16**, con un intervalo válido entre 1 y 4096. Este valor se usa para determinar la cantidad de información de directorio que el cliente puede almacenar en caché. Aumentar el valor puede disminuir el tráfico y aumentar el rendimiento cuando se accede a directorios de gran tamaño. Intenta aumentar este valor a **1024**.

#### <a name="filenotfoundcacheentriesmax"></a>FileNotFoundCacheEntriesMax

HKLM\\System\\CurrentControlSet\\Services\\LanmanWorkstation\\Parameters\\FileNotFoundCacheEntriesMax

Válido para Windows 10. El valor predeterminado es **128**, con un intervalo válido entre 1 y 65536. Este valor se usa para determinar la cantidad de información de nombres de archivos que el cliente puede almacenar en caché. Aumentar el valor puede disminuir el tráfico de red y aumentar el rendimiento cuando se accede a varios archivos de nombres. Intenta aumentar este valor a **2048**.

#### <a name="dormantfilelimit"></a>DormantFileLimit

HKLM\\System\\CurrentControlSet\\Services\\LanmanWorkstation\\Parameters\\DormantFileLimit

Válido para Windows 10. El valor predeterminado es **1023**. Este parámetro especifica el número máximo de archivos que se deben dejar abiertos en un recurso compartido una vez que la aplicación cerró el archivo. Si varios miles de clientes se conectan a servidores SMB, considera la posibilidad de reducir este valor a **256**.

Puedes configurar varias de estas opciones de SMB con los cmdlets [Set-SmbClientConfiguration](/powershell/module/smbshare/set-smbclientconfiguration) y [Set-SmbServerConfiguration](/powershell/module/smbshare/set-smbserverconfiguration) de Windows PowerShell. También puedes configurar las opciones de solo registro con Windows PowerShell, tal como se muestra en el siguiente ejemplo:

`Set-ItemProperty -Path "HKLM:\\SYSTEM\\CurrentControlSet\\Services\\LanmanWorkstation\\Parameters" RequireSecuritySignature -Value 0 -Force`

### <a name="additional-settings-from-the-windows-restricted-traffic-limited-functionality-baseline-guidance"></a>Configuración adicional de las instrucciones de línea de base de funcionalidades limitadas de tráfico restringido de Windows

Microsoft ha lanzado una línea de base que se creó con los mismos procedimientos que las [Líneas de base de seguridad de Windows](/windows/device-security/windows-security-baselines), para entornos que no están conectados directamente a Internet o que deben reducir los datos enviados a Microsoft y otros servicios.

La configuración de la [línea de base de funcionalidades limitadas de tráfico restringido de Windows](/windows/privacy/manage-connections-from-windows-operating-system-components-to-microsoft-services) está marcada en la tabla de directivas de grupo con un asterisco.

### <a name="disk-cleanup-including-using-the-disk-cleanup-wizard"></a>Liberar espacio en disco (incluidos con el Asistente para liberar espacio en disco)

Liberar espacio en disco puede ser especialmente útil con implementaciones de VDI de imagen maestra. Una vez que la imagen maestra está preparada, actualizada y configurada, una de las últimas tareas a realizar es la de liberar espacio en disco. El asistente integrado en Windows para liberar espacio en disco puede ayudarte a limpiar la mayoría de las áreas susceptibles a ahorrar espacio en disco.

> [!NOTE]
> Ten en cuenta que el asistente para liberar espacio en disco ya no se sigue desarrollando. Windows usará otros métodos para proporcionar funciones para liberar espacio en disco.



Igualmente, aquí tienes sugerencias para realizar varias tareas para liberar espacio en disco. Recuerda que debes realizar estos pasos antes de implementar cualquiera de ellas:

1. Ejecuta el asistente para liberar espacio en disco (con privilegios elevados) después de aplicar todas las actualizaciones. Incluye las categorías **Optimización de distribución**  y **Limpieza de Windows Update**. Puedes automatizar este proceso con **Cleanmgr.exe** mediante la opción **/SAGESET:11**. Esta opción establece los valores del Registro que se pueden usar más adelante para automatizar la función para liberar espacio en disco; para ello, se usan todas las opciones disponibles en el asistente para liberar espacio en disco.

   1.  En una VM de prueba, desde una instalación limpia, la ejecución de **Cleanmgr.exe/SAGESET:11** revela que solo hay dos opciones para liberar espacio en disco de forma automática y que están habilitadas de forma predeterminada:

   - Archivos de programa descargados

   - Archivos temporales de Internet

   2. Si configuras más opciones o todas ellas, esas opciones se guardan en el Registro, de acuerdo con el valor del índice proporcionado en el comando anterior (**Cleanmgr.exe/SAGESET:11**). En este ejemplo, usaremos el valor *11* como índice, para liberar posteriormente espacio en disco.

   3. Después de ejecutar **Cleanmgr.exe/SAGESET:11** verás una serie de categorías de opciones para liberar espacio en disco. Puedes seleccionar cada opción y **Aceptar**. Notarás que el asistente para liberar espacio en disco simplemente desaparece. Sin embargo, las configuraciones que seleccionaste se guardarán en el Registro y pueden invocarse ejecutando **Cleanmgr.exe/SAGERUN:11**.

2. Limpia el almacenamiento de instantáneas de volumen, si hay alguno en uso. Para hacer esto, ejecuta los siguientes comandos en una solicitud elevada:

   - **vssadmin list shadows**

   - **vssadmin list shadowstorage**

       Si el resultado de estos comandos es *No se encontraron elementos que satisfagan la consulta*, entonces no se está usando ningún almacenamiento de VSS.

3. Limpieza de los archivos temporales y los registros. Desde un símbolo del sistema con privilegios elevados, ejecuta estos comandos:

   - **Del C:\\\*.tmp /s**

   - **Del C:\\Windows\\Temp\\.**

   - **Del %temp%\\.**

4. Elimina los perfiles que no uses en el sistema con este comando:

   **wmic path win32_UserProfile where LocalPath="c:\\\\users\\\\\<user\>" Delete**

### <a name="remove-onedrive"></a>Quitar OneDrive

Quitar OneDrive implica quitar del paquete, la desinstalación y la eliminación de los archivos \*.lnk. Puedes usar el siguiente código de ejemplo de PowerShell para quitar OneDrive de la imagen:

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
