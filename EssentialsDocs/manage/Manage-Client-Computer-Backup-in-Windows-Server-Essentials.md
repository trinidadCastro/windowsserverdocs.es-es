---
title: Administrar copias de seguridad del equipo cliente en Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 1b4776e8-9504-4b98-ae80-11da797d9819
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: c743e0a30796eac374052787f7c47b0af6e656b6
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89623163"
---
# <a name="manage-client-computer-backup-in-windows-server-essentials"></a>Administrar copias de seguridad del equipo cliente en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

 En este tema se incluye información sobre las tareas de copia de seguridad comunes para los equipos cliente que puede realizar mediante el panel de Windows Server Essentials; en él se incluyen las siguientes secciones:

-   [Cómo funciona el asistente de reparación de bases de datos de copias de seguridad](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_1)

-   [Comprender la configuración de copia de seguridad del equipo](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_2)

-   [Configurar la copia de seguridad de un equipo cliente](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_3)

-   [Cambiar la hora en que la copia de seguridad está programada para ejecutarse](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_4)

-   [Cambiar la directiva de retención de copias de seguridad de equipos](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_5)

-   [Restablecer la configuración predeterminada de la copia de seguridad](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_6)

-   [Usar herramientas de reparación y recuperación](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_7)

-   [Reparar la base de datos de copias de seguridad](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_8)

-   [Deshabilitar la copia de seguridad de un equipo](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_9)

-   [Ejecutar la tarea de limpieza de copia de seguridad](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_10)

-   [Ver alertas en la barra de tareas de un equipo cliente](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_11)

-   [Ver el estado de copia de seguridad de Launchpad](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_12)

-   [Detener una copia de seguridad en curso desde Launchpad](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_13)

-   [Iniciar una copia de seguridad desde Launchpad](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_14)

-   [Cómo funciona la copia de seguridad del equipo](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_15)

-   [Sugerencias para evitar la pérdida de datos por daños en la base de datos de copias de seguridad del cliente](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_16)

-   [Restaurar archivos o carpetas desde una copia de seguridad del equipo cliente](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_17)

-   [Información sobre el Historial de archivos](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_FileHistory)

##  <a name="how-the-repair-the-backup-database-wizard-works"></a><a name="BKMK_1"></a> Cómo funciona el Asistente para reparar bases de datos de copia de seguridad
 Si Windows Server Essentials detecta errores en la base de datos de copias de seguridad, envía una notificación de mantenimiento y el estado de alerta cambia a rojo, lo que indica una condición crítica.

 En el panel de Windows Server Essentials, haga clic en el icono de estado de alerta para ver la notificación de error de la base de datos de copias de seguridad. La notificación incluye un botón **Reparar** que inicia el asistente de reparación de bases de datos de copias de seguridad. El asistente puede tardar varias horas en finalizar.

 Este examina la base de datos de copias de seguridad del equipo seleccionado y trata de repararla si se detectan errores. En algunos casos, el asistente no puede reparar toda la base de datos de copias de seguridad. Antes de iniciar al asistente, compruebe que todos los discos duros externos que usa en el servidor estén conectados al servidor, activados y que funcionen correctamente. Se puede solucionar automáticamente el error de la base de datos de copias de seguridad al reconectar un disco duro que falta.

> [!CAUTION]
>  Debe hacer una copia de seguridad de la base de datos antes de tratar de repararla. Según el tipo de errores encontrados en la base de datos de copias de seguridad, es posible que el asistente no pueda recuperar algunas copias de seguridad o que algunas o todas ellas se pierdan de forma permanente.

##  <a name="understand-the-computer-backup-settings"></a><a name="BKMK_2"></a> Descripción de la configuración de copia de seguridad del equipo
 Después de configurar la copia de seguridad de los equipos cliente, puede especificar un espacio diferente de tiempo en el que se hará la copia de seguridad. De forma similar, puede especificar un tiempo de retención de copias de seguridad mayor o menor que el valor predeterminado.

 La opción **Restaurar valores predeterminados** permite restablecer los valores predeterminados de espacio de tiempo y directiva de retención de copias de seguridad que se proporcionan durante la configuración inicial.

 Los valores predeterminados son:

|Configuración de copia de seguridad|Valor predeterminado|Descripción|
|--------------------|-------------|-----------------|
|Hora de inicio|18:00|Especifica la hora a la que se inicia la copia de seguridad diaria. Se recomienda establecer este valor en una hora en la que los usuarios no usen sus equipos.|
|Hora de finalización|9:00 a. m.|Especifica la hora a la que debe finalizar la copia de seguridad. Si no es necesario especificar la duración de la copia de seguridad, se usa solo el tiempo necesario para hacer la copia de seguridad del equipo correctamente.|
|Retener copias de seguridad diarias|5 días|Especifica el número de días que se retienen las copias de seguridad diarias. Por ejemplo, con la configuración predeterminada, las copias de seguridad se retienen 5 días. En el sexto día y en cada día posterior, se elimina el archivo de copia de seguridad diaria más antiguo.|
|Retener copias de seguridad semanales|4 semanas|Especifica el número de semanas que se retiene la última copia de seguridad de la semana. Por ejemplo, con la configuración predeterminada, las copias de seguridad se retienen 4 semanas. En la quinta semana y en cada semana posterior, se elimina la copia de seguridad semanal más antigua.|
|Retener copias de seguridad mensuales|6 meses|Especifica el número de meses que se retiene la última copia de seguridad del mes. Por ejemplo, con la configuración predeterminada, las copias de seguridad se retienen 6 meses. En el séptimo mes y en cada mes posterior, se elimina la copia de seguridad mensual más antigua.|

##  <a name="set-up-backup-for-a-client-computer"></a><a name="BKMK_3"></a> Configurar la copia de seguridad de un equipo cliente
 Si está deshabilitada, puede configurar la copia de seguridad del equipo desde el panel. Al configurar copias de seguridad de un equipo, puede optar por hacer una copia de seguridad de todo el contenido del equipo o seleccionar los volúmenes y las carpetas de los que quiere hacer una copia.

> [!NOTE]
>  El equipo debe estar conectado para configurar la copia de seguridad.

> [!IMPORTANT]
>   Windows Server Essentials no admite la copia de seguridad y la restauración de discos dinámicos en equipos cliente.

#### <a name="to-set-up-backup-for-a-client-computer"></a>Para configurar la copia de seguridad de un equipo cliente

1.  Abra el **Panel** y, a continuación, haga clic en la pestaña **Dispositivos**.

2.  Haga clic en el nombre del equipo cliente para el que quiera configurar la copia de seguridad y después, en el panel de **tareas**, haga clic en **Configurar copias de seguridad de este equipo**.

    > [!NOTE]
    >  Si la copia de seguridad del equipo cliente ya está configurada, aparece **Personalizar la copia de seguridad para el equipo** en el panel de **tareas** en vez de **Configurar copias de seguridad de este equipo**.

3.  En el asistente para configurar las copias de seguridad, puede optar por hacer copias de seguridad de todas las carpetas o seleccionar las carpetas de las que quiera hacer copias de seguridad. Siga las instrucciones del asistente.

4.  Haga clic en **Cerrar** cuando haya configurado las copias de seguridad del equipo.

### <a name="critical-system-files"></a>Archivos imprescindibles del sistema
 Al instalar el sistema operativo Windows, el programa de instalación crea carpetas en la unidad del sistema donde coloca los archivos que el sistema necesita para iniciarse y funcionar. Los archivos imprescindibles del sistema incluyen archivos con las extensiones .exe, .dll, .ocx y .sys. Algunos de estos archivos son fuentes True Type. Además, los archivos de estado del sistema, como el registro del sistema, son necesarios para que el sistema operativo se ejecute correctamente.

### <a name="find-the-file-you-are-looking-for"></a>Encuentre el archivo que está buscando
 Puede restaurar todas las carpetas de un equipo, varios archivos y carpetas, o un único archivo o carpeta desde una copia de seguridad existente.

 Después de seleccionar la copia de seguridad desde la que quiere hacer la restauración, se lee el archivo de copia de seguridad y se muestran todos los archivos y carpetas. Puede explorar en profundidad hasta el archivo o carpeta específico que quiere restaurar haciendo doble clic en la carpeta de nivel superior y después explorando la jerarquía de carpetas hasta encontrar el archivo que busca.

### <a name="why-am-i-unable-to-select-some-items"></a>¿Por qué no puedo seleccionar algunos elementos?
 La casilla de verificación del menú de selección de la página **Seleccione los elementos de los que se va a realizar copia de seguridad** puede indicar un estado diferente para cada carpeta. Cuando la casilla de verificación está:

- **seleccionada**, se selecciona la carpeta asociada y el contenido de la carpeta para hacer una copia de seguridad.

- **no seleccionada**, la carpeta asociada y el contenido de la carpeta no se incluyen en la copia de seguridad.

- **sólida**, se selecciona la carpeta asociada para hacer una copia de seguridad, pero uno o varios elementos incluidos en esa carpeta no se incluyen en la copia de seguridad.

  Si no puede seleccionar una carpeta específica:

- El volumen puede estar configurado para hacer copias de seguridad, pero es posible que esté sin conexión. Esto es común en las unidades USB extraíbles. Los volúmenes sin conexión se muestran en texto gris.

- Solo puede hacer copias de seguridad de datos desde una unidad local que tenga el formato de un sistema de archivos NTFS. Las unidades que tengan el formato FAT (incluido FAT32) o los sistemas de archivos ReFS no aparecen en la lista de unidades de disco de las que se va a hacer una copia de seguridad.

> [!IMPORTANT]
>  El Servicio de instantáneas de volumen (VSS) no admite la creación de una instantánea de un volumen virtual y del volumen del host en el mismo conjunto de instantáneas. VSS admite la creación de instantáneas de volúmenes en un disco duro virtual (VHD) si es necesario hacer una copia de seguridad del volumen virtual. Para obtener más información, consulte [Servicing and Backing Up Virtual Hard Disks (Mantenimiento y seguridad de los discos duros virtuales)](https://go.microsoft.com/fwlink/p/?LinkId=256577).

##  <a name="change-the-time-that-backup-is-scheduled-to-run"></a><a name="BKMK_4"></a> Cambiar el tiempo durante el que está programada la ejecución de la copia de seguridad
 El proceso de copia de seguridad debe programarse para una hora en la que haya el menor número de personas posible usando sus equipos conectados en red. Esto es generalmente durante la noche o a primera hora de la mañana. La hora predeterminada para hacer copias de seguridad es de las 6:00 p. m. a las 9:00 a. m. El servidor intenta hacer copias de seguridad de los equipos cliente solo durante el espacio de tiempo programado.

 Sin embargo, si su empresa tiene actividad durante estas horas que suelen quedar fuera del horario laboral, tal vez quiera cambiar la configuración predeterminada.

#### <a name="to-change-the-time-that-backup-is-scheduled-to-run"></a>Para cambiar la hora en que la copia de seguridad está programada para ejecutarse

1.  Abra el **panel** del servidor y haga clic en **Dispositivos**.

2.  En **Tareas de dispositivos**, haga clic en **Personalizar la configuración de copia de seguridad de equipos y del Historial de archivos**.

    > [!NOTE]
    >  En Windows Server Essentials, esta tarea se llama tareas de **copia de seguridad de equipos cliente**.

3.  En la pestaña **Copia de seguridad de equipos** de **Configuración y herramientas de copia de seguridad de equipos**, puede cambiar las horas de inicio y finalización según sus necesidades.

4.  Haga clic en **Aplicar** y en **Aceptar**.

##  <a name="change-the-computer-backup-retention-policy"></a><a name="BKMK_5"></a> Cambiar la Directiva de retención de copias de seguridad de equipos
 Las copias de seguridad diarias de todos los equipos se acumulan en el servidor con el tiempo. Para administrar estas copias de seguridad, Windows Server Essentials puede ayudarle a administrar la base de datos de copias de seguridad de los equipos. Puede configurar el número de copias de seguridad que se mantendrán para todos los equipos.

 La directiva de retención de copias de seguridad determina el tiempo que se conserva una copia de seguridad antes de eliminarla durante el proceso de limpieza de copias de seguridad. La limpieza de copias de seguridad se hace cada sábado a las 11:59 p. m. y elimina todas las copias de seguridad que se encuentran fuera del ámbito de la directiva de retención de copias de seguridad. Los valores predeterminados de la directiva de retención de copias de seguridad son:

-   **Retener copias de seguridad diarias durante 5 días.** La primera copia de seguridad del día se retiene como copia de seguridad diaria. Después de 5 días, la copia de seguridad diaria más antigua se elimina en el proceso de limpieza.

-   **Retener copias de seguridad semanales durante 4 semanas.** La primera copia de seguridad de la semana se retiene como copia de seguridad semanal. Después de 4 semanas, la copia de seguridad semanal más antigua se elimina en el proceso de limpieza.

-   **Retener copias de seguridad mensuales durante 6 meses.** La primera copia de seguridad del mes se retiene como copia de seguridad mensual. Después de 6 meses, la copia de seguridad mensual más antigua se elimina en el proceso de limpieza.

#### <a name="to-change-the-computer-backup-retention-policy"></a>Para cambiar la directiva de retención de copias de seguridad del equipo

1.  Abra el **Panel**.

2.  Haga clic en **Dispositivos**.

3.  En el panel **Tareas de dispositivos**, haga clic en **Personalizar la configuración de copia de seguridad de equipos y del Historial de archivos**.

    > [!NOTE]
    >  En Windows Server Essentials, esta tarea se llama tareas de **copia de seguridad de equipos cliente**.

4.  En la sección **Directiva de retención de copias de seguridad de equipos cliente** de **Configuración y herramientas de copia de seguridad de equipos**, haga los cambios que necesite a la directiva de retención.

5.  Cuando haya terminado, haga clic en **Aceptar** para aplicar los cambios y cerrar el cuadro de diálogo. La directiva de retención actualizada se aplicará la próxima vez que se haga la limpieza de copias de seguridad.

    > [!NOTE]
    >  La directiva de retención actualizada se aplica a todos los equipos cliente de la red que estén configurados para hacer copias de seguridad.

##  <a name="reset-backup-to-default-settings"></a><a name="BKMK_6"></a> Restablecer la configuración predeterminada de la copia de seguridad
 Después de configurar las copias de seguridad de los equipos cliente, es posible que el administrador de red haya especificado un espacio de tiempo diferente. De forma similar, puede que el administrador haya especificado un tiempo de retención de copias de seguridad mayor o menor que el valor predeterminado. El botón **Restaurar valores predeterminados** permite restablecer los valores predeterminados de espacio de tiempo y directiva de retención de copias de seguridad que se proporcionan durante la configuración inicial.

 Los valores predeterminados son:

-   Hora de inicio de copia de seguridad: 6:00 PM

-   Hora de finalización: 9:00 AM

-   Número de días que se conservan las copias de seguridad diarias: 5 días

-   Número de semanas que se conserva la última copia de seguridad de la semana: 4 semanas

-   Número de meses que se conserva la última copia de seguridad del mes: 6 meses

#### <a name="to-reset-computer-backup-to-the-default-settings"></a>Para restablecer la configuración predeterminada de copias de seguridad del equipo

1.  Abra el **panel** y después abra la página **Dispositivos**.

2.  En **Tareas de dispositivos**, haga clic en **Personalizar la configuración de copia de seguridad de equipos y del Historial de archivos**.

    > [!NOTE]
    >  En Windows Server Essentials, haga clic en **tareas de copia de seguridad de equipos cliente**.

3.  En la pestaña **Copia de seguridad de equipos** de la página **Configuración y herramientas de copia de seguridad de equipos**, haga clic en **Restablecer valores predeterminados**.

    > [!NOTE]
    >  Tal vez quiera anotar la configuración personalizada de programación y directiva de retención porque se perderán al restablecer la configuración predeterminada.

4.  Haga clic en **Aplicar** y en **Aceptar**.

##  <a name="use-repair-and-recovery-tools"></a><a name="BKMK_7"></a> Usar herramientas de reparación y recuperación
 **Reparar copias de seguridad:** si la base de datos de copias de seguridad de equipos resulta dañada o inutilizable por cualquier motivo, puede tratar de repararla mediante el asistente para la reparación de bases de datos de copias de seguridad. El asistente analiza los archivos de copias de seguridad para determinar si hay problemas que se puedan reparar y luego trata de corregir los problemas encontrados.

> [!WARNING]
>  Si no se puede solucionar un problema, el asistente puede eliminar permanentemente un archivo de copia de seguridad del equipo.

 **Recuperación de equipo:** puede crear una unidad flash USB de arranque que se usará para restaurar un equipo desde una copia de seguridad existente. Debe usar una unidad flash USB de 1 GB o más. Después de crear la unidad flash USB de arranque, insértela en el equipo que quiera restaurar y después inícielo desde dicha unidad flash USB para iniciar el proceso de restauración completa del sistema.

##  <a name="repair-the-backup-database"></a><a name="BKMK_8"></a> Reparar la base de datos de copia de seguridad
 Si recibe una alerta que le indica que la base de datos de copias de seguridad de equipos tiene problemas, puede tratar de repararla.

#### <a name="to-repair-the-backup-database"></a>Para reparar la base de datos de copias de seguridad

1.  Abra el **Panel**.

2.  Haga clic en **Dispositivos**.

3.  En el panel **Tareas de dispositivos**, haga clic en **Personalizar la configuración de copia de seguridad de equipos y del Historial de archivos**.

    > [!NOTE]
    >  En Windows Server Essentials, haga clic en **tareas de copia de seguridad de equipos cliente**.

4.  En **Configuración y herramientas de copia de seguridad de equipos**, haga clic en la pestaña **Herramientas**.

5.  En la sección **Reparar copias de seguridad**, haga clic en **Reparar ahora**. Se abre el asistente de reparación de bases de datos de copias de seguridad.

6.  Siga las instrucciones del asistente de reparación de bases de datos de copias de seguridad.

7.  Según el tamaño de la base de datos de copias de seguridad, la reparación puede tardar varias horas. Haga clic en **Cerrar** y después, en **Aceptar** para cerrar la página **Personalizar la configuración de copia de seguridad de equipos y del Historial de archivos**.

    > [!NOTE]
    >  En Windows Server Essentials, haga clic en **configuración y herramientas de copia de seguridad de equipos cliente**.

#### <a name="to-review-the-results-of-the-backup-database-repair"></a>Para revisar los resultados de la reparación de la base de datos de copias de seguridad

1.  Abra el **Panel**.

2.  Haga clic en **Dispositivos**.

3.  En el panel **Tareas de dispositivos**, haga clic en **Personalizar la configuración de copia de seguridad de equipos y del Historial de archivos**.

    > [!NOTE]
    >  En Windows Server Essentials, haga clic en **tareas de copia de seguridad de equipos cliente**.

4.  En **Configuración y herramientas de copia de seguridad de equipos**, haga clic en la pestaña **Herramientas**.

5.  Los resultados se muestran en la sección **Reparar copias de seguridad**.

##  <a name="disable-backup-for-a-computer"></a><a name="BKMK_9"></a> Deshabilitar la copia de seguridad de un equipo
 Use el panel para deshabilitar rápidamente las copias de seguridad de equipos de la red.

#### <a name="to-disable-backup-for-a-computer"></a>Para deshabilitar la copia de seguridad de un equipo

1. Abra el **Panel**.

2. Haga clic en la pestaña **Dispositivos**.

3. Haga clic en el nombre del equipo del que quiere deshabilitar las copias de seguridad.

4. En el panel de tareas, haga clic en **Personalizar la copia de seguridad para el equipo**. Aparece el asistente para personalizar las copias de seguridad.

5. Haga clic en **Deshabilitar la copia de seguridad para este equipo** y después seleccione si quiere conservar o eliminar los archivos de copias de seguridad existentes.

6. Haga clic en **Guardar cambios** y después, en **Cerrar**.

   Para información sobre cómo habilitar la copia de seguridad de un equipo después de haberla deshabilitado, vea [Configurar la copia de seguridad de un equipo cliente](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_3).

##  <a name="run-the-backup-cleanup-task"></a><a name="BKMK_10"></a> Ejecutar la tarea de limpieza de copia de seguridad
 La tarea de limpieza de copias de seguridad del equipo cliente está programada para ejecutarse cada sábado a las 11:59 p. m. después de haberse completado todas las copias de seguridad. La tarea de limpieza elimina las copias de seguridad de la base de datos de copias de seguridad del equipo cliente según la directiva de retención de copias de seguridad. Los valores predeterminados de la directiva de retención de copias de seguridad son:

- Número de días que se conservan las copias de seguridad diarias: 5 días

- Número de semanas que se conserva la última copia de seguridad de la semana: 4 semanas

- Número de meses que se conserva la última copia de seguridad del mes: 6 meses

  Para información sobre cómo cambiar la directiva de retención de copias de seguridad, vea [Cambiar la directiva de retención de copias de seguridad de equipos](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_5).

#### <a name="to-run-the-client-backup-database-cleanup-task"></a>Para ejecutar la tarea de limpieza de la base de datos de copias de seguridad del cliente

1.  Inicie sesión en el servidor con la cuenta de usuario y la contraseña de administrador.

2.  En el servidor, abra **Programador de tareas**. Puede acceder al programa del Programador de tareas desde la consola **Herramientas administrativas**.

3.  En el programador de tareas, expanda sucesivamente **Biblioteca del Programador de tareas**, **Microsoft** y **Windows**, y después haga clic en **Windows Server Essentials**.

4.  Haga clic en la tarea **Limpieza de copias de seguridad** y después, en **Ejecutar** en el panel **Acciones**. El estado cambia a **En ejecución** hasta que se complete la tarea.

##  <a name="view-alerts-in-the-task-bar-on-a-client-computer"></a><a name="BKMK_11"></a> Ver alertas en la barra de tareas de un equipo cliente
 Puede recibir un aviso de copia de seguridad en la barra de tareas del equipo por los siguientes motivos:

-   Se ha iniciado una copia de seguridad.

-   Ha finalizado una copia de seguridad.

-   No hay ninguna copia de seguridad correcta reciente. El número de días transcurridos desde la última copia de seguridad correcta se incluye en el aviso.

-    Solo Windows Server Essentials: se agrega un nuevo volumen al equipo. La persona que administra la red debe ejecutar el asistente para personalizar las copias de seguridad de modo que incluya o excluya el volumen para futuras copias de seguridad.

##  <a name="view-backup-status-from-the-launchpad"></a><a name="BKMK_12"></a> Ver el estado de la copia de seguridad desde Launchpad
 Use Launchpad para ver el estado rápido de copia de seguridad del equipo.

> [!TIP]
>  Para ver el estado rápido, mueva el cursor sobre el icono de Launchpad en el área de notificación del escritorio. El estado de copia de seguridad aparece en la información sobre herramientas.

#### <a name="to-view-status-of-a-backup-for-your-computer"></a>Para ver el estado de una copia de seguridad del equipo

1.  Inicie sesión en **Launchpad** con su nombre de usuario y contraseña.

2.  Haga clic en **Copia de seguridad**.

3.  En el cuadro de diálogo **Propiedades de la copia de seguridad**, el estado de la copia de seguridad se muestra en la sección **Estado de la copia de seguridad**.

    -   **No hay copias de seguridad anteriores**: la copia de seguridad aún no se ha hecho o se ha eliminado el historial de copias de seguridad.

    -   **Copia de seguridad pendiente**: la copia de seguridad está esperando a que se complete otro proceso en el servidor.

    -   **Conectando**: la copia de seguridad se está conectando al servidor.

    -   **No se puede conectar**: la copia de seguridad no puede conectar con el servicio de proveedores de copias de seguridad de equipos cliente de Windows Server.

        > [!NOTE]
        >  En Windows Server Essentials, se ha cambiado el nombre del servicio administración de equipos cliente de Windows Server Essentials.

    -   **Copia de seguridad en curso**: muestra el porcentaje de copia de seguridad completado.

    -   **Copia de seguridad cancelada**: muestra la fecha y la hora de inicio de la copia de seguridad si usted o el administrador de red la cancelaron.

    -   **La copia de seguridad se realizó correctamente**: muestra la fecha y la hora de inicio de la copia de seguridad si la copia de seguridad se completó correctamente.

    -   **La copia de seguridad no se completó**: muestra la fecha y la hora de inicio de la copia de seguridad si la copia de seguridad no se completó correctamente.

    -   **La copia de seguridad no se completó correctamente**: muestra la fecha y la hora de inicio de la copia de seguridad si la copia de seguridad no se completó correctamente.

4.  Haga clic en **Aceptar** para cerrar el cuadro de diálogo **Propiedades de la copia de seguridad**.

##  <a name="stop-a-backup-in-progress-from-the-launchpad"></a><a name="BKMK_13"></a> Detener una copia de seguridad en curso desde Launchpad
 Puede detener fácilmente una copia de seguridad que está en curso.

#### <a name="to-stop-a-backup-in-progress"></a>Para detener una copia de seguridad en curso

1.  Abra **Launchpad**.

    > [!NOTE]
    >  Si se le solicita, inicie sesión en **Launchpad** con su nombre de usuario y contraseña.

2.  Haga clic en **Copia de seguridad**.

3.  En la sección **Estado de la copia de seguridad** del cuadro de diálogo **Propiedades de la copia de seguridad**, el botón **Iniciar la copia de seguridad** cambia a **Detener copia de seguridad** cuando la copia de seguridad está en curso. Haga clic en **Detener copia de seguridad** y después, en **Sí** para confirmar. La copia de seguridad seguirá ejecutándose hasta que haga clic en **Sí**.

4.  Al detener una copia de seguridad en curso, el estado cambia a **Copia de seguridad cancelada** con la fecha y la hora en que se inició. Si, en vez de ello, ve un estado de copia de seguridad **correcta**, **incompleta** o **con errores**, significa que ya ha terminado la última copia de seguridad.

5.  Haga clic en **Aceptar** para cerrar el cuadro de diálogo **Propiedades de la copia de seguridad**.

##  <a name="start-a-backup-from-the-launchpad"></a><a name="BKMK_14"></a> Inicio de una copia de seguridad desde Launchpad
 Es posible que a veces quiera hacer una copia de seguridad de los archivos y carpetas antes de la hora programada de copia de seguridad configurada en el servidor. Launchpad le permite iniciar manualmente la copia de seguridad de su equipo.

#### <a name="to-start-a-backup"></a>Para iniciar una copia de seguridad

1.  Abra **Launchpad**.

    > [!NOTE]
    >  Si se le solicita, inicie sesión en **Launchpad** con su nombre de usuario y contraseña.

2.  Haga clic en **Copia de seguridad**.

3.  En la sección **Estado de la copia de seguridad** del cuadro de diálogo **Propiedades de la copia de seguridad**, haga clic en **Iniciar la copia de seguridad** y después, en **Aceptar**.

4.  Escriba una descripción de la copia de seguridad y después haga clic en **Aceptar**. El estado cambia a **Iniciando copia de seguridad** y después, a **Haciendo copia de seguridad** con un porcentaje completado.

5.  Tras iniciarse la copia de seguridad, el botón cambia a **Detener copia de seguridad**. Si la copia de seguridad está en curso y quiere detenerla, haga clic en **Detener copia de seguridad** y después, en **Sí**. Al detener una copia de seguridad en curso, el estado cambia a **Copia de seguridad cancelada** con la fecha y la hora en que se inició.

6.  Cuando la copia de seguridad finaliza correctamente, el estado cambia a **La copia de seguridad se realizó correctamente** con la fecha y la hora en que se inició la copia de seguridad finalizada.

7.  Haga clic en **Aceptar** para cerrar el cuadro de diálogo **Propiedades de la copia de seguridad**.

##  <a name="how-computer-backup-works"></a><a name="BKMK_15"></a> Cómo funciona la copia de seguridad del equipo
 Al hacer la copia de seguridad cada día, ocurrirá lo siguiente:

-   Se hace la copia de seguridad de los equipos de red uno tras otro.

-   Una copia de seguridad que está en curso finaliza aunque cierre la ventana de copia de seguridad.

-   Se instalan las actualizaciones de Windows y Windows Server Essentials se reinicia (si es necesario).

-   Los domingos se hace la limpieza de copias de seguridad.

> [!IMPORTANT]
>  El Servicio de instantáneas de volumen (VSS) no admite la creación de una instantánea de un volumen virtual y del volumen del host en el mismo conjunto de instantáneas. VSS admite la creación de instantáneas de volúmenes en un disco duro virtual (VHD) si es necesario hacer una copia de seguridad del volumen virtual. Para obtener más información, consulte [Servicing and Backing Up Virtual Hard Disks (Mantenimiento y seguridad de los discos duros virtuales)](https://go.microsoft.com/fwlink/p/?LinkId=256577).

##  <a name="tips-to-help-prevent-data-loss-due-to-corruption-of-the-client-backup-database"></a><a name="BKMK_16"></a> Sugerencias para ayudar a evitar la pérdida de datos debido a daños en la base de datos de copias de seguridad del cliente
 Si la base de datos de copias de seguridad del cliente se daña, puede perder datos críticos.

 Para evitar la pérdida de datos por daños en la base de datos de copias de seguridad del cliente, considere las siguientes opciones:

-   Habilite un método adicional de copia de seguridad de la base de datos de copias de seguridad del cliente. Por ejemplo, en función de la versión de Windows Server Essentials que esté ejecutando, use la copia de seguridad del servidor, la copia de seguridad en línea o Microsoft Azure Backup para hacer una copia de seguridad de la base de datos.

    > [!IMPORTANT]
    >  Asegúrese de seleccionar la opción de hacer una copia de seguridad de todos los archivos de la carpeta **Copias de seguridad de equipos cliente**.

-   Si tiene que restaurar la base de datos, asegúrese de restaurar todos los archivos de la carpeta **Copias de seguridad de equipos cliente**. Si no restaura todos los archivos, la base de datos puede volverse incoherente.

-   Antes de limpiar o restaurar la base de datos de copias de seguridad del cliente, asegúrese de detener todas las copias de seguridad del cliente que estén actualmente en curso.

     Si está ejecutando Windows Server Essentials, también debe detener el servicio de copia de seguridad de equipos cliente de Windows Server y el servicio de proveedor de copias de seguridad de equipos cliente de Windows Server.

     Si está ejecutando Windows Server Essentials, debe detener también el servicio de copias de seguridad de equipos de Windows Server.

     Después de completar la operación de restauración, reinicie el servicio.

##  <a name="restore-files-or-folders-from-a-client-computer-backup"></a><a name="BKMK_17"></a> Restaurar archivos o carpetas desde una copia de seguridad del equipo cliente
 Puede examinar y restaurar archivos y carpetas individuales desde una copia de seguridad.

> [!NOTE]
>  No puede restaurar archivos ni carpetas en un equipo cliente usando el panel en el servidor. Debe abrir el panel en un equipo cliente para completar la tarea.

> [!IMPORTANT]
>  No puede restaurar archivos ni carpetas en un disco duro que se haya configurado para ser un disco dinámico.

#### <a name="to-restore-files-and-folders"></a>Para restaurar archivos y carpetas

1.  Abra el **panel** en un equipo cliente.

2.  Haga clic en la pestaña **Dispositivos**.

3.  Haga clic en el equipo en el que quiere restaurar archivos y carpetas y después haga clic en **Restaurar archivos o carpetas para el equipo** en el panel de **tareas**. Se abre el Asistente de restauración de archivos o carpetas.

4.  Siga las instrucciones del asistente.

##  <a name="understanding-file-history"></a><a name="BKMK_FileHistory"></a> Información sobre el historial de archivos
 La característica de Historial de archivos de Windows Server Essentials hace automáticamente copias de seguridad de los archivos que se encuentran en las carpetas Bibliotecas, Contactos, Escritorio y Favoritos de los equipos de red que tienen la función de Historial de archivos. Si los originales se pierden, se dañan o se eliminan, puede restaurarlos. También puede encontrar diferentes versiones de los archivos desde un punto específico en el tiempo. Con el tiempo, tendrá un historial completo de los archivos.

 En Windows Server Essentials, puede personalizar la configuración del historial de archivos desde la página **historial de archivos** de **configuración y herramientas de copia de seguridad de equipos cliente**.

 En Windows Server Essentials, puede personalizar la configuración del historial de archivos yendo a la pestaña **usuarios** y seleccionando la tarea **cambiar la configuración del historial de archivos** .

 La página Historial de archivos ofrece las siguientes opciones:

|Configuración de copia de seguridad|Valor predeterminado|Descripción|
|--------------------|-------------|-----------------|
|Activar/Desactivar|Activado|El Historial de archivos está activado de forma predeterminada al instalar Windows Server Essentials.|
|Datos de copia de seguridad|Documentos y Escritorio|Hay tres opciones preconfiguradas que le permiten especificar diversas soluciones de copia de seguridad. Puede elegir una de las siguientes opciones:<br /><br /> -Documentos y escritorio<br /><br /> -Todas las bibliotecas, escritorio, contactos y favoritos<br /><br /> -Todos los datos de bibliotecas, escritorio, contactos y favoritos, excepto los datos de las bibliotecas de música, vídeo e imágenes|
|Frecuencia de copia de seguridad|Cada hora|Especifica la frecuencia con la que el Historial de archivos crea una copia de seguridad de los datos seleccionados. Puede elegir entre varias opciones comprendidas entre cada 10 minutos o a diario.|
|Retener copias durante|1 año|Especifica la cantidad de tiempo que el Historial de archivos conserva una copia de una copia de seguridad.|

 Para información sobre los problemas del Historial de archivos, vea [Solucionar problemas del Historial de archivos](../support/Troubleshoot-File-History-in-Windows-Server-Essentials.md).

## <a name="additional-references"></a>Referencias adicionales

-   [Administrar copias de seguridad y restauración](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)

-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)

-   [Usar Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
