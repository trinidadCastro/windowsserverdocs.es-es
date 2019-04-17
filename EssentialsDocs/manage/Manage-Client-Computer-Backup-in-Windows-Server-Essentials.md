---
title: Administrar la copia de seguridad de cliente equipo en Windows Server Essentials
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b4776e8-9504-4b98-ae80-11da797d9819
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d184c97e47f04b9d7434aaeb0d328761bcfac1c0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="manage-client-computer-backup-in-windows-server-essentials"></a>Administrar la copia de seguridad de cliente equipo en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Este tema incluye información sobre las tareas de copia de seguridad comunes para los equipos cliente que puedes realizar con el escritorio de Windows Server Essentials e incluye las siguientes secciones:  
  
-   [Cómo funciona la reparación el Asistente para la base de datos de copia de seguridad](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Comprender las opciones de copia de seguridad del equipo](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Configurar copia de seguridad de un equipo cliente](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Cambiar la hora de copia de seguridad que está programada para ejecutarse](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Cambiar la directiva de retención de copia de seguridad del equipo](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Restablecer la copia de seguridad a la configuración predeterminada](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Usar herramientas de reparación y recuperación](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [Reparar la copia de seguridad](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [Deshabilitar la copia de seguridad para un equipo](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Ejecutar la tarea de limpieza de copia de seguridad](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [Ver los avisos en la barra de tareas en un equipo cliente](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [Ver el estado de copia de seguridad desde el Launchpad](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_12)  
  
-   [Detener una copia de seguridad en curso del Launchpad](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_13)  
  
-   [Iniciar una copia de seguridad desde el Launchpad](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_14)  
  
-   [Cómo funciona la copia de seguridad del equipo](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_15)  
  
-   [Sugerencias para ayudar a evitar la pérdida de datos a causa daños de la base de datos de copia de seguridad de cliente](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_16)  
  
-   [Restaurar archivos o carpetas desde una copia de seguridad del equipo cliente](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_17)  
  
-   [Historial de archivos de descripción](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_FileHistory)  
  
##  <a name="BKMK_1"></a>Cómo funciona la reparación el Asistente para la base de datos de copia de seguridad  
 Si Windows Server Essentials detecte errores en la base de datos de copia de seguridad, envía una notificación de estado y el estado de alerta cambia a rojo, que indica una condición crítica.  
  
 En el panel de Windows Server Essentials, haz clic en el icono de estado de alerta para ver la notificación de errores de copia de seguridad de la base de datos. La notificación incluye un **reparación** botón, que inicia la reparación el Asistente para la base de datos de copia de seguridad. El asistente puede tardar varias horas a fin.  
  
 El asistente examina la base de datos de copia de seguridad para el equipo seleccionado y se intenta reparar la base de datos si se detectan errores. En algunos casos, el asistente no puede reparar la base de datos de copia de seguridad completa. Antes de iniciar al asistente, comprueba que todas las unidades de disco duras externas que usas en el servidor estén conectadas al servidor, activado y funcionando correctamente. El error de copia de seguridad de la base de datos puede corregirse automáticamente volviendo a conectar una unidad de disco dura que falta.  
  
> [!CAUTION]
>  Debe hacer una copia de la base de datos antes de intentar repararlo. Según el tipo de errores encontrados en la base de datos de copia de seguridad, el asistente no podrá recuperar algunas copias de seguridad. Algunas o todas las copias de seguridad puede perdidos definitivamente.  
  
##  <a name="BKMK_2"></a>Comprender las opciones de copia de seguridad del equipo  
 Después de configura copia de seguridad para los equipos cliente, puedes especificar un distinto período de tiempo para la copia de seguridad que se produzcan. Del mismo modo, puedes especificar un tiempo de retención de copia de seguridad de mayor o menor que el valor predeterminado.  
  
 La **Restaurar valores predeterminados de** opción te permite restablecer la directiva de ventana y la retención de copia de seguridad a los valores predeterminados proporcionados durante la configuración inicial de copia de seguridad.  
  
 Los valores predeterminados son:  
  
|Configuración de copia de seguridad|Predeterminado|Descripción|  
|--------------------|-------------|-----------------|  
|Hora de inicio|6:00 P.M.|Especifica el tiempo que se inicie la copia de seguridad diaria. Se recomienda establecer esto en una hora cuando los usuarios no usan sus equipos.|  
|Hora de finalización|9:00A.M.|Especifica el tiempo que se debe finalizar la copia de seguridad. Si la copia de seguridad no requiere la duración especificada, usa solo el tiempo necesario para correctamente una copia de seguridad del equipo.|  
|Conservar copias de seguridad diarias|5 días|Especifica el número de días que se conservan las copias de seguridad diarias. Por ejemplo, usando la configuración predeterminada, se conservan las copias de seguridad diarias 5. En el 6 y cada día a partir de entonces, el archivo de copia de seguridad diario más antiguo se elimina.|  
|Conservar las copias de seguridad semanales|4 semanas|Especifica el número de semanas que se conserva la última copia de seguridad de la semana. Por ejemplo, con la configuración predeterminada, se conservan las copias de seguridad semanales 4. En la semana 5ª y cada semana posteriormente, se elimina la copia de seguridad semanal más antiguo.|  
|Conservar las copias de seguridad mensuales|6meses|Especifica el número de meses que se conserva la última copia de seguridad del mes. Por ejemplo, usando la configuración predeterminada, se conservan 6meses a partir de las copias de seguridad. En el 7 y posteriores cada mes, la copia de seguridad mensual más antigua se elimina.|  
  
##  <a name="BKMK_3"></a>Configurar copia de seguridad de un equipo cliente  
 Si se deshabilita la copia de seguridad, puedes establecer una copia de seguridad para el equipo desde el panel. Cuando configuras una copia de seguridad para un equipo, puedes elegir hacer una copia de seguridad de todo en el equipo o seleccionar los volúmenes y las carpetas que desees hacer una copia.  
  
> [!NOTE]
>  El equipo debe estar conectado para que puedas Configurar copia de seguridad.  
  
> [!IMPORTANT]
>   Windows Server Essentials no admite copias de seguridad y restauración de los discos dinámicos en los equipos cliente.  
  
#### <a name="to-set-up-backup-for-a-client-computer"></a>Configurar copia de seguridad de un equipo cliente  
  
1.  Abre el **panel**y, a continuación, haz clic en el **dispositivos** pestaña.  
  
2.  Haz clic en el nombre del equipo cliente que desea configurar copia de seguridad y, a continuación, en la **tareas** panel, haz clic en **Configurar copia de seguridad para este equipo**.  
  
    > [!NOTE]
    >  Si la copia de seguridad ya está configurado para el equipo cliente, **personalizar copias de seguridad para este equipo** aparece en la **tareas** panel en lugar de **Configurar copia de seguridad para este equipo**.  
  
3.  En la copia de seguridad Asistente para configurar el, puedes hacer copias de seguridad de las carpetas o selecciona ciertas carpetas que desees hacer una copia. Sigue las instrucciones del asistente.  
  
4.  Haz clic en **cerrar** al configurar copia de seguridad para el equipo.  
  
### <a name="critical-system-files"></a>Archivos críticos del sistema  
 Cuando se instala el sistema operativo Windows, el programa de instalación crea carpetas en la unidad del sistema que se coloca archivos que el sistema necesita para iniciar y ejecutar. Archivos críticos del sistema incluyen archivos con extensiones de archivo .sys, .ocx, .exe y .dll. Algunos de estos archivos son TrueType. Además, los archivos de estado del sistema, como el registro de s de sistema son necesarios para el sistema operativo se ejecute correctamente.  
  
### <a name="find-the-file-you-are-looking-for"></a>Busca el archivo que estás buscando  
 Puedes restaurar todas las carpetas de un equipo, varios archivos y carpetas, o un solo archivo o carpeta desde una copia de seguridad existente.  
  
 Después de seleccionar la copia de seguridad que quieras usar para restaurar desde, se lee el archivo de copia de seguridad y se muestran todos los archivos y carpetas. Puede desglosar el archivo o carpeta específicos para restaurar doble clic en la carpeta de nivel superior y, a continuación, desplazándose por la jerarquía de carpetas hasta encontrar el archivo que estás buscando.  
  
### <a name="why-am-i-unable-to-select-some-items"></a>¿Por qué no puedo seleccionar algunos elementos?  
 La casilla de verificación en el menú de selección de la **seleccionar los elementos para hacer copia de seguridad de la página** puede indicar un estado diferente para cada carpeta. Cuando la casilla de verificación es:  
  
-   **Seleccionado**, se selecciona la carpeta asociada y el contenido de la carpeta para la copia de seguridad.  
  
-   **No seleccionado**, se excluye la carpeta asociada y el contenido de la carpeta de copia de seguridad.  
  
-   **Sólido**, se selecciona la carpeta asociada para copia de seguridad, pero uno o más elementos de esa carpeta están excluidos de la copia de seguridad.  
  
 Si no puedes seleccionar una carpeta específica:  
  
-   El volumen puede estar configurado para la copia de seguridad, pero es posible sin conexión. Esto es habitual para las unidades USB extraíbles. Volúmenes que se encuentran sin conexión se muestran en texto gris.  
  
-   Solo puede hacer la copia de seguridad de datos desde una unidad local que es el formato de un sistema de archivos NTFS. Unidades con formato FAT (incluidos FAT32) o los sistemas de archivos de referencias (Refs) no aparecen en la lista de unidades para realizar una copia.  
  
> [!IMPORTANT]
>  Servicio de copia sombra de volumen (VSS) no admite la creación de una instantánea de un volumen virtual y el volumen de host en el mismo conjunto de instantánea. VSS admite la creación de instantáneas de volúmenes en un disco duro virtual (VHD), si se necesita la copia de seguridad del volumen virtual. Para obtener más información, consulta [mantenimiento y realizar copias de seguridad de los discos duros virtuales](https://go.microsoft.com/fwlink/p/?LinkId=256577).  
  
##  <a name="BKMK_4"></a>Cambiar la hora de copia de seguridad que está programada para ejecutarse  
 El proceso de copia de seguridad debe programarse durante un tiempo cuando usas el número de personas como sea posible a sus equipos en red. Esto es normalmente durante la noche u horas temprano. El tiempo predeterminado para la copia de seguridad es 6:00 p. M. hasta las 9:00A.M.. El servidor intenta hacer la copia de seguridad de los equipos cliente solo durante el periodo de tiempo programado.  
  
 Sin embargo, si tu empresa está activa durante estos tradicionalmente horas, puedes cambiar esta configuración predeterminada.  
  
#### <a name="to-change-the-time-that-backup-is-scheduled-to-run"></a>Para cambiar la hora esa copia de seguridad está programada para ejecutarse  
  
1.  Abre el servidor **panel**y, a continuación, haz clic en **dispositivos**.  
  
2.  En **dispositivos tareas**, haz clic en **personalizar copia de seguridad del equipo y el historial de archivos de configuración**.  
  
    > [!NOTE]
    >  En Windows Server Essentials, esta tarea se ha cambiado de nombre **tareas de copia de seguridad de equipos cliente**.  
  
3.  En **configuración copia de seguridad del equipo cliente y herramientas**, en la **copia de seguridad de equipo** ficha, puedes cambiar las horas de inicio y fin para satisfacer sus necesidades.  
  
4.  Haz clic en **aplicar**y, a continuación, haz clic en **Aceptar**.  
  
##  <a name="BKMK_5"></a>Cambiar la directiva de retención de copia de seguridad del equipo  
 Copias de seguridad diarias de todos los equipos se acumulen en el servidor con el tiempo. Para ayudarte a administrar estas copias de seguridad, Windows Server Essentials puede ayudarte a administrar la base de datos de las copias de seguridad del equipo. Puedes configurar cómo muchas copias de seguridad que mantenga para todos tus equipos.  
  
 La directiva de retención de copia de seguridad determina cuánto tiempo se conserva una copia de seguridad antes de eliminarse durante el proceso de limpieza de copia de seguridad. Liberador de espacio en copia de seguridad se ejecuta en todos los sábados 11:59 P.M.. Elimina todas las copias de seguridad que se encuentran fuera de la directiva de retención de copia de seguridad. Los valores predeterminados de la directiva de retención de copia de seguridad son:  
  
-   **Copias de seguridad diarias se conservan durante 5 días.** La copia de seguridad de primer del día se conserva como la copia de seguridad diaria. Después de 5 días, se elimina la copia de seguridad diaria más antiguos en el proceso de limpieza.  
  
-   **Conservar la copia de seguridad semanal para 4 semanas.** El primer copia de seguridad de la semana se conserva como la copia de seguridad semanal. Después de 4 semanas, se elimina la copia de seguridad semanal más antiguos en el proceso de limpieza.  
  
-   **Conservar las copias de seguridad mensuales durante seis meses.** El primer copia de seguridad del mes se conserva como la copia de seguridad mensual. Después de 6meses, se elimina la copia de seguridad mensual más antiguos en el proceso de limpieza  
  
#### <a name="to-change-the-computer-backup-retention-policy"></a>Para cambiar la directiva de retención de copia de seguridad del equipo  
  
1.  Abre el **panel**.  
  
2.  Haz clic en **dispositivos**.  
  
3.  En la **dispositivos tareas** panel, haz clic en **personalizar copia de seguridad del equipo y el historial de archivos de configuración**.  
  
    > [!NOTE]
    >  En Windows Server Essentials, esta tarea se ha cambiado de nombre **tareas de copia de seguridad del equipo cliente**.  
  
4.  En **configuración copia de seguridad del equipo cliente y herramientas**, en la **directiva de retención de copia de seguridad de equipos cliente** sección, realiza los cambios a la directiva de retención que satisfaga sus necesidades.  
  
5.  Cuando termines, haz clic en **Aceptar** para aplicar los cambios y cerrar el cuadro de diálogo. La directiva de retención actualizados se aplicará la próxima vez que se ejecuta la limpieza de copia de seguridad.  
  
    > [!NOTE]
    >  La directiva actualizada de retención se aplica a todos los equipos cliente de la red que están configurados para la copia de seguridad.  
  
##  <a name="BKMK_6"></a>Restablecer la copia de seguridad a la configuración predeterminada  
 Después de configura copia de seguridad para los equipos cliente, el Administrador de red que haya especificado un período de tiempo diferente. Del mismo modo, es posible que el administrador puede especificar un tiempo de retención de copia de seguridad de mayor o menor que el valor predeterminado. La **Restaurar valores predeterminados** botón te permite restablecer la directiva de ventana y la retención de copia de seguridad a los valores predeterminados proporcionados durante la configuración inicial de copia de seguridad.  
  
 Los valores predeterminados son:  
  
-   Hora de inicio de la copia de seguridad: 6:00 P.M.  
  
-   Hora de finalización: 9:00A.M.  
  
-   Número de días que se conservan las copias de seguridad diarias: 5 días  
  
-   Número de semanas que se conserva la última copia de seguridad de la semana: 4 semanas  
  
-   Número de meses que se conserva la última copia de seguridad del mes: 6meses  
  
#### <a name="to-reset-computer-backup-to-the-default-settings"></a>Para restablecer la copia de seguridad del equipo a la configuración predeterminada  
  
1.  Abre el **panel**y, a continuación, abre el **dispositivos** página.  
  
2.  En **dispositivos tareas**, haz clic en **personalizar copia de seguridad del equipo y el historial de archivos de configuración**.  
  
    > [!NOTE]
    >  En Windows Server Essentials, haz clic en **tareas de copia de seguridad de equipos cliente**.  
  
3.  En la **copia de seguridad de equipo** ficha de la **herramientas y configuración de copia de seguridad y del equipo cliente** página, haz clic en **Restaurar valores predeterminados**.  
  
    > [!NOTE]
    >  Puedes escribir hacia abajo de la programación personalizada y configuración de directiva de retención, ya que se perderán cuando se restablece la configuración predeterminada.  
  
4.  Haz clic en **aplicar**y, a continuación, haz clic en **Aceptar**.  
  
##  <a name="BKMK_7"></a>Usar herramientas de reparación y recuperación  
 **Reparar las copias de seguridad:** si la base de datos de las copias de seguridad del equipo resulta dañado o inutilizable por algún motivo, puede intentar reparar la base de datos mediante la reparación el Asistente para la base de datos de copia de seguridad. El Asistente para analiza los archivos de copia de seguridad para determinar si hay problemas que se pueden reparar. El Asistente para, a continuación, intenta corrige los problemas que se encuentran.  
  
> [!WARNING]
>  Si no se puede resolver un problema, el asistente puede eliminar permanentemente un archivo de copia de seguridad del equipo.  
  
 **Recuperación del equipo:** puede crear una unidad flash USB de arranque usar para restaurar un equipo desde una copia de seguridad existente. Debes usar una unidad flash USB que es 1 GB o más grande. Después de crea la unidad flash USB de arranque, se insertan en el equipo que desea restaurar y, a continuación, arrancar en la unidad flash USB para iniciar el proceso de restauración del sistema completo.  
  
##  <a name="BKMK_8"></a>Reparar la copia de seguridad  
 Si recibes una alerta que indica que la copia de seguridad del equipo tiene problemas, puede intentar repararlo.  
  
#### <a name="to-repair-the-backup-database"></a>Para reparar la copia de seguridad  
  
1.  Abre el **panel**.  
  
2.  Haz clic en **dispositivos**.  
  
3.  En la **dispositivos tareas** panel, haz clic en **personalizar copia de seguridad del equipo y el historial de archivos de configuración.**  
  
    > [!NOTE]
    >  En Windows Server Essentials, haz clic en **tareas de copia de seguridad de equipos cliente**.  
  
4.  En **configuración copia de seguridad del equipo cliente y herramientas**, haz clic en el **herramientas** pestaña.  
  
5.  En la **reparar las copias de seguridad** sección, haz clic en **reparar ahora**. Abre la reparación el Asistente para la base de datos de copia de seguridad.  
  
6.  Sigue las instrucciones de la reparación el Asistente para la base de datos de copia de seguridad.  
  
7.  Dependiendo de cuánto es la copia de seguridad, la reparación de la base de datos puede tardar varias horas. Haz clic en **cerrar**y, a continuación, haz clic en **Aceptar** para cerrar la **personalizar copia de seguridad del equipo y el historial de archivos de configuración** página.  
  
    > [!NOTE]
    >  En Windows Server Essentials, haz clic en **configuración copia de seguridad del equipo cliente y herramientas**.  
  
#### <a name="to-review-the-results-of-the-backup-database-repair"></a>Para revisar los resultados de la reparación de copia de seguridad de la base de datos  
  
1.  Abre el **panel**.  
  
2.  Haz clic en **dispositivos**.  
  
3.  En la **dispositivos tareas** panel, haz clic en **personalizar copia de seguridad del equipo y el historial de archivos de configuración.**  
  
    > [!NOTE]
    >  En Windows Server Essentials, haz clic en **tareas de copia de seguridad de equipos cliente**.  
  
4.  En **configuración copia de seguridad del equipo cliente y herramientas**, haz clic en el **herramientas** pestaña.  
  
5.  Los resultados se muestran en la **reparar las copias de seguridad** sección.  
  
##  <a name="BKMK_9"></a>Deshabilitar la copia de seguridad para un equipo  
 Usa el panel para rápidamente deshabilitar las copias de seguridad para los equipos de la red.  
  
#### <a name="to-disable-backup-for-a-computer"></a>Para deshabilitar la copia de seguridad para un equipo  
  
1.  Abre el **panel**.  
  
2.  Haz clic en el **dispositivos** pestaña.  
  
3.  Haz clic en el nombre del equipo para el que desea deshabilitar las copias de seguridad.  
  
4.  En el panel de tareas, haz clic en **personalizar copia de seguridad para el equipo**. Aparece el Asistente para personalizar la copia de seguridad.  
  
5.  Haz clic en **deshabilitar copia de seguridad para este equipo**y, a continuación, selecciona si quieres conservar o eliminar los archivos de copia de seguridad existentes.  
  
6.  Haz clic en **guardar cambios**y, a continuación, haz clic en **cerrar**.  
  
 Para obtener información acerca de cómo habilitar la copia de seguridad para un equipo tras haber deshabilitado la copia de seguridad, consulta [Configurar copia de seguridad de un equipo cliente](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_3).  
  
##  <a name="BKMK_10"></a>Ejecutar la tarea de limpieza de copia de seguridad  
 La tarea de limpieza de copia de seguridad del equipo cliente está programada para ejecutarse a 11:59 P.M. todos los sábados cuando se completen todas las copias de seguridad. La tarea de limpieza elimina las copias de seguridad de la base de datos de copia de seguridad de cliente equipo según la directiva de retención de copia de seguridad. La configuración predeterminada para la directiva de retención de copia de seguridad es:  
  
-   Número de días que se conservan las copias de seguridad diarias: 5 días  
  
-   Número de semanas que se conserva la última copia de seguridad de la semana: 4 semanas  
  
-   Número de meses que se conserva la última copia de seguridad del mes: 6meses  
  
 Para obtener información sobre cómo cambiar la directiva de retención de copia de seguridad, consulta [cambiar la directiva de retención de copia de seguridad del equipo](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_5).  
  
#### <a name="to-run-the-client-backup-database-cleanup-task"></a>Para ejecutar al cliente de la tarea de limpieza de copia de seguridad de la base de datos  
  
1.  Iniciar sesión en el servidor con tu cuenta de usuario de administrador y la contraseña.  
  
2.  En el servidor, abra **programador de tareas**. Puedes acceder al programa de programador de tareas desde la **herramientas administrativas** consola.  
  
3.  En el programador de tareas, expanda **biblioteca del programador de tareas**, expanda **Microsoft**, expanda **Windows**y, a continuación, haz clic en **Windows Server Essentials**.  
  
4.  Haz clic en el **Liberador de espacio de copia de seguridad** de tareas y, a continuación, haz clic en **ejecutar** en la **acciones** panel. El estado cambia a **ejecutando** hasta que se complete la tarea.  
  
##  <a name="BKMK_11"></a>Ver los avisos en la barra de tareas en un equipo cliente  
 Puede recibir una notificación de copia de seguridad en la barra de tareas en el equipo por los siguientes motivos:  
  
-   Inicia una copia de seguridad.  
  
-   Finalizó una copia de seguridad.  
  
-   No hay una copia de seguridad correcta reciente. Se incluye el número de días desde la última copia de seguridad correcta en el anuncio.  
  
-    Windows Server Essentials: se agrega un nuevo volumen al equipo. La persona que administra la red debe ejecutar el Asistente para personalizar la copia de seguridad para incluir o excluir el volumen de copias de seguridad futuras.  
  
##  <a name="BKMK_12"></a>Ver el estado de copia de seguridad desde el Launchpad  
 Usar el Launchpad para ver el estado rápido de copia de seguridad para el equipo.  
  
> [!TIP]
>  Para un estado rápido, mover el cursor sobre el icono de Launchpad en el área de notificación del escritorio. El estado de copia de seguridad aparece en la información sobre herramientas.  
  
#### <a name="to-view-status-of-a-backup-for-your-computer"></a>Para ver el estado de una copia de seguridad para tu equipo  
  
1.  Inicia sesión en el **Launchpad** con tu nombre de usuario y contraseña.  
  
2.  Haz clic en **copia de seguridad**.  
  
3.  En la **propiedades de copia de seguridad** cuadro de diálogo, en la **una copia de seguridad estado** se muestra la sección, el estado de la copia de seguridad.  
  
    -   **Ninguna copia de seguridad anterior**: Backup ya sea aún no se ha ejecutado, o se ha eliminado el historial de copia de seguridad.  
  
    -   **Copia de seguridad pendientes**: una copia de seguridad está esperando otro proceso en el servidor para completar.  
  
    -   **Conexión**: copia de seguridad se conecta al servidor.  
  
    -   **No se puede conectar**: copia de seguridad no se puede conectar con el servicio copia de seguridad proveedor del equipo de cliente de Windows Server.  
  
        > [!NOTE]
        >  En Windows Server Essentials, este servicio ha sido cambiar el nombre de servicio de administración de equipo de cliente de Windows Server Essentials.  
  
    -   **Copia de seguridad en curso**: muestra el porcentaje de copia de seguridad realizada.  
  
    -   **Se canceló la copia de seguridad**: muestra la fecha y hora de inicio de la copia de seguridad si le o el Administrador de red, cancela la copia de seguridad.  
  
    -   **Copia de seguridad se realizó correctamente**: muestra la fecha y hora de inicio de la copia de seguridad si se completó correctamente la copia de seguridad.  
  
    -   **Copia de seguridad estaba incompleta**: muestra la fecha y hora de inicio de la copia de seguridad si no se completó correctamente la copia de seguridad.  
  
    -   **Copia de seguridad no fue correcto**: muestra la fecha y hora de inicio de la copia de seguridad si no se completó correctamente la copia de seguridad.  
  
4.  Haz clic en **Aceptar** para cerrar la **propiedades de copia de seguridad** cuadro de diálogo.  
  
##  <a name="BKMK_13"></a>Detener una copia de seguridad en curso del Launchpad  
 Puedes detener fácilmente una copia de seguridad que está en curso.  
  
#### <a name="to-stop-a-backup-in-progress"></a>Para detener una copia de seguridad en curso  
  
1.  Abre el **Launchpad**.  
  
    > [!NOTE]
    >  Si se te pide, inicia sesión en el **Launchpad** con tu nombre de usuario y contraseña.  
  
2.  Haz clic en **copia de seguridad**.  
  
3.  En la **propiedades de copia de seguridad** cuadro de diálogo la **una copia de seguridad estado** sección, el **iniciar copia de seguridad** botón cambia a **copia de seguridad de detención** cuando la copia de seguridad está en curso. Haz clic en **copia de seguridad de detención**y, a continuación, haz clic en **Sí** para confirmar. La copia de seguridad continuará ejecutándose hasta que se hace clic en **Sí**.  
  
4.  Cuando se detiene una copia de seguridad en curso, el estado cambia a **se canceló la copia de seguridad** con la fecha y hora en que inició la copia de seguridad. Si en su lugar, ver un estado de **correcto**, **incompleto**, o **Failed**, ya ha terminado la última copia de seguridad.  
  
5.  Haz clic en **Aceptar** para cerrar la **propiedades de copia de seguridad** cuadro de diálogo.  
  
##  <a name="BKMK_14"></a>Iniciar una copia de seguridad desde el Launchpad  
 Puede que a veces cuando quieres hacer copia de seguridad de los archivos y carpetas antes de la hora de copia de seguridad programada configuración en el servidor. Al Launchpad te permite iniciar manualmente la copia de seguridad del equipo.  
  
#### <a name="to-start-a-backup"></a>Para iniciar una copia de seguridad  
  
1.  Abre el **Launchpad**.  
  
    > [!NOTE]
    >  Si se te pide, inicia sesión en el **Launchpad** con tu nombre de usuario y contraseña.  
  
2.  Haz clic en **copia de seguridad**.  
  
3.  En la **propiedades de copia de seguridad** cuadro de diálogo, en la **una copia de seguridad estado** sección, haz clic en **iniciar copia de seguridad**y, a continuación, haz clic en **Aceptar**.  
  
4.  Escribe una descripción para la copia de seguridad y, a continuación, haz clic en **Aceptar**. El estado cambia a **a partir de copia de seguridad**y, a continuación, **copia de seguridad en proceso** con un porcentaje completado.  
  
5.  Después de iniciar copia de seguridad, el botón cambia a **copia de seguridad de detención**. Si la copia de seguridad está en curso y desea dejar de compartirla, haz clic en **copia de seguridad de detención**y, a continuación, haz clic en **Sí**. Cuando se detiene una copia de seguridad en curso, el estado cambia a **se canceló la copia de seguridad** con la fecha y hora en que se inició la copia de seguridad.  
  
6.  Cuando la copia de seguridad finaliza correctamente, el estado cambia a **copia de seguridad se realizó correctamente** con la fecha y hora en que inició la copia de seguridad terminado.  
  
7.  Haz clic en **Aceptar** para cerrar la **propiedades de copia de seguridad** cuadro de diálogo.  
  
##  <a name="BKMK_15"></a>Cómo funciona la copia de seguridad del equipo  
 Se produce lo siguiente durante la copia de seguridad cada día:  
  
-   Equipos de la red se crea una copia de seguridad de uno tras otro.  
  
-   Finaliza una copia de seguridad que está en curso, incluso si cierras la ventana de la copia de seguridad.  
  
-   Se instalan las actualizaciones de Windows y Windows Server Essentials reinicia (si es necesario).  
  
-   Domingo, se ejecuta la limpieza de copia de seguridad.  
  
> [!IMPORTANT]
>  Servicio de copia sombra de volumen (VSS) no admite la creación de una instantánea de un volumen virtual y el volumen de host en el mismo conjunto de instantánea. VSS admite la creación de instantáneas de volúmenes en un disco duro virtual (VHD), si se necesita la copia de seguridad del volumen virtual. Para obtener más información, consulta [mantenimiento y realizar copias de seguridad de los discos duros virtuales](https://go.microsoft.com/fwlink/p/?LinkId=256577).  
  
##  <a name="BKMK_16"></a>Sugerencias para ayudar a evitar la pérdida de datos a causa daños de la base de datos de copia de seguridad de cliente  
 Si se daña la base de datos de copia de seguridad de cliente, puede perder datos fundamentales.  
  
 Para ayudar a evitar la pérdida de datos debido a los daños en la base de datos de copia de seguridad de cliente, ten en cuenta lo siguiente:  
  
-   Habilitar un método adicional de copia de seguridad de la base de datos de copia de seguridad de cliente. Por ejemplo, según la versión de Windows Server Essentials cuando se está ejecutando, usar copia de seguridad del servidor, copia de seguridad en línea o copia de seguridad de Microsoft Azure para hacer copia de seguridad de la base de datos.  
  
    > [!IMPORTANT]
    >  Asegúrate de que seleccionas copia todos los archivos de la **copia de seguridad de los equipos cliente** carpeta.  
  
-   En caso de que debe restaurar la base de datos, asegúrate de restaurar todos los archivos en la **copia de seguridad de los equipos cliente** carpeta. Si no se restaura todos los archivos, la base de datos puede ser incoherente.  
  
-   Antes de limpiar o restaurar la copia de seguridad del cliente, asegúrate de detener todas las copias de seguridad de cliente que están actualmente en curso.  
  
     Si estás ejecutando Windows Server Essentials, también debe detener el servicio de copia de seguridad del equipo cliente de Windows Server y el servicio copia de seguridad proveedor del equipo de cliente de Windows Server.  
  
     Si estás ejecutando Windows Server Essentials, también debe detener el servicio de copia de seguridad del equipo de Windows Server.  
  
     Después de completar la operación de restauración, reiniciar el servicio.  
  
##  <a name="BKMK_17"></a>Restaurar archivos o carpetas desde una copia de seguridad del equipo cliente  
 Puedes examinar y restaurar archivos y carpetas individuales de una copia de seguridad.  
  
> [!NOTE]
>  No puedes restaurar archivos y carpetas en un equipo cliente mediante el panel en el servidor. Debes abrir el panel en un equipo cliente para completar la tarea.  
  
> [!IMPORTANT]
>  No puedes restaurar archivos y carpetas en un disco duro que se ha configurado para que sea un disco dinámico.  
  
#### <a name="to-restore-files-and-folders"></a>Para restaurar archivos y carpetas  
  
1.  Abre el **panel** en un equipo cliente.  
  
2.  Haz clic en el **dispositivos** pestaña.  
  
3.  Haz clic en el equipo que desea restaurar archivos y carpetas y, a continuación, haz clic en **restaurar archivos o carpetas para el equipo** en la **tareas** panel. Abre el restaurar archivos o carpetas del asistente.  
  
4.  Sigue las instrucciones del asistente.  
  
##  <a name="BKMK_FileHistory"></a>Historial de archivos de descripción  
 La característica de historial de archivos de Windows Server Essentials realiza automáticamente una copia de seguridad de los archivos que están en las carpetas de bibliotecas, contactos, escritorio y favoritos de equipos de red que tienen funcionalidad de historial de archivos. Si los archivos originales se perdido, dañan o eliminan, puede restaurarlos. También puede encontrar diferentes versiones de los archivos desde un punto específico en el tiempo. Con el tiempo, tendrás un historial completo de los archivos.  
  
 En Windows Server Essentials, puede personalizar la configuración del historial de archivos desde el **historial de archivos** página de **configuración copia de seguridad del equipo cliente y herramientas**.  
  
 En Windows Server Essentials, puedes personalizar la configuración del historial de archivos, ve a la **usuarios** pestaña y, a continuación, selecciona el **cambiar la configuración del historial de archivos** tarea.  
  
 La página del historial de archivos proporciona las siguientes opciones:  
  
|Configuración de copia de seguridad|Predeterminado|Descripción|  
|--------------------|-------------|-----------------|  
|Activar / desactivar|En|Historial de archivos está activado de manera predeterminada cuando se instalación Windows Server Essentials.|  
|Datos de copia de seguridad|Documentos y escritorio|Hay tres las opciones preconfiguradas, que te permiten especificar una variedad de soluciones de copia de seguridad. Puedes elegir una de las siguientes opciones:<br /><br /> -Documentos y escritorio<br /><br /> -Todas las bibliotecas, escritorio, contactos y favoritos<br /><br /> -Todos los datos en bibliotecas, contactos, de escritorio y de exclusión de datos en las bibliotecas de música, vídeo y fotos de favoritos|  
|Frecuencia de copia de seguridad|Cada hora|Especifica la frecuencia con el historial de archivos crea una copia de seguridad de los datos seleccionados. Puedes elegir entre varias opciones de ese intervalo de cada 10 minutos a diario.|  
|Mantener copias de|1 año|Especifica la cantidad de tiempo que el historial de archivos conserva una copia de una copia de seguridad.|  
  
 Para obtener información acerca de problemas de historial de archivos, consulta [solucionar problemas de historial de archivos](../support/Troubleshoot-File-History-in-Windows-Server-Essentials.md).  
  
## <a name="see-also"></a>Consulta también  
  
-   [Administrar la copia de seguridad y restauración](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Usar Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
