---
title: Administrar copias de seguridad del servidor en Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 0302d070-c58a-40f2-b56d-7e7842813d02
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d792a03771ad406400877d73c0aa7ef1124adeb3
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85470590"
---
# <a name="manage-server-backup-in-windows-server-essentials"></a>Administrar copias de seguridad del servidor en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

 En los siguientes temas se incluye información sobre las tareas de copia de seguridad comunes que se pueden llevar a cabo con el panel de Windows Server Essentials:

-   [¿Qué copia de seguridad debo elegir?](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_WhichBackup)

-   [Configurar o personalizar la copia de seguridad del servidor](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_1)

-   [Detener la copia de seguridad del servidor en curso](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_2)

-   [Administrar las copias de seguridad de manera remota](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_3)

-   [Deshabilitar la copia de seguridad del servidor](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_4)

-   [Más información acerca de cómo configurar la copia de seguridad del servidor](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_5)

-   [Volver a particionar un disco duro en el servidor](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_6)

-   [Restaurar archivos y carpetas desde una copia de seguridad del servidor](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_7)

##  <a name="which-backup-should-i-choose"></a><a name="BKMK_WhichBackup"></a>¿Qué copia de seguridad debo elegir?
 La elección de una copia de seguridad del servidor puede ser sencilla si dispone de una copia de seguridad correcta reciente y está seguro de que contiene todos sus datos importantes. Si intenta restaurar al servidor o a un equipo desde una copia de seguridad antigua, el hecho de elegir una buena copia de seguridad para la restauración puede requerir cierta investigación y, posiblemente, transigir en algunas exigencias.

#### <a name="to-choose-a-backup"></a>Para seleccionar una copia de seguridad

1.  Póngase en contacto con el propietario de los archivos o las carpetas y anote las fechas y las horas en las que se han agregado o editado. Use las fechas y horas como punto de inicio.

2.  En la página **Elija una opción de restauración** del asistente de restauración de archivos y carpetas, haga clic en **Restaurar desde una copia de seguridad seleccionada (avanzado)**.

3.  En función de si quiere restaurar una versión anterior o posterior de los archivos o carpetas, seleccione la copia de seguridad que mejor se adapte a las fechas y horas que ha anotado en el paso 1.

4.  Como práctica recomendada, puede restaurar archivos y carpetas en una ubicación alternativa y, a continuación, permitir que el propietario de los archivos y carpetas pueda mover a la ubicación original los que necesite. Al finalizar se pueden eliminar los archivos y carpetas que permanezcan en la ubicación alternativa.

##  <a name="set-up-or-customize-server-backup"></a><a name="BKMK_1"></a>Configurar o personalizar la copia de seguridad del servidor
 La copia de seguridad del servidor no se configura automáticamente durante la instalación. Debe proteger el servidor y sus datos de forma automática programando copias de seguridad diarias. Se recomienda mantener un plan de copias de seguridad diario porque la mayoría de las empresas no puede permitirse perder los datos creados durante varios días. Para obtener más información, vea cómo [configurar o personalizar una copia de seguridad del servidor](Set-up-or-customize-server-backup.md).

##  <a name="stop-server-backup-in-progress"></a><a name="BKMK_2"></a>Detener la copia de seguridad del servidor en curso
 Tanto si se inicia una copia de seguridad del servidor en una hora programada de manera regular como si inicia una copia de seguridad manualmente, puede detener la copia de seguridad en curso.

#### <a name="to-stop-a-backup-in-progress"></a>Para detener una copia de seguridad en curso

1.  Abra el Panel.

2.  En la barra de navegación, haga clic en **Dispositivos**.

3.  En la lista de equipos, haga clic en el servidor y, a continuación, haga clic en **Detener copia de seguridad del servidor** en el panel **Tareas**.

4.  Haga clic en **Sí** para confirmar la acción.

##  <a name="remotely-manage-your-backups"></a><a name="BKMK_3"></a>Administrar las copias de seguridad de forma remota
 Cuando esté fuera de la oficina podrá usar Acceso Web remoto de Windows Server Essentials para tener acceso al panel de Windows Server Essentials y administrar el servidor.

#### <a name="to-use-remote-web-access-to-manage-your-server"></a>Para usar Acceso Web remoto y administrar el servidor

1. Abra un explorador web.

2. En el cuadro de dirección, escriba el nombre del dominio de Windows Server Essentials.

3. Cuando se le solicite, escriba el nombre de usuario y la contraseña.

4. Al hacer clic en el nombre del servidor en acceso Web remoto, se muestra la página de inicio de sesión del panel.

5. Inicie sesión en el panel como administrador y, a continuación, haga clic en **Dispositivos**.

   Para obtener más información sobre el acceso Web remoto, vea [información general sobre el acceso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Overview).

##  <a name="disable-server-backup"></a><a name="BKMK_4"></a>Deshabilitar la copia de seguridad del servidor
 Debe proteger el servidor y sus datos de forma automática programando copias de seguridad diarias. Se recomienda mantener un plan de copias de seguridad diario porque la mayoría de las empresas no puede permitirse perder los datos creados durante varios días.

 Si ya tiene configurada la copia de seguridad del servidor y posteriormente quiere usar una aplicación de terceros para hacer una copia de seguridad del servidor, puede deshabilitar la copia de seguridad del servidor de Windows Server Essentials.

#### <a name="to-disable-server-backup"></a>Para deshabilitar la copia de seguridad del servidor

1.  Inicie sesión en el panel de Windows Server Essentials mediante una cuenta de administrador y una contraseña.

2.  Haga clic en la pestaña **Dispositivos** y, a continuación, haga clic en el nombre del servidor.

3.  En el panel de tareas, haga clic en **Personalizar la copia de seguridad para el servidor**.

    > [!NOTE]
    >  Se muestra la tarea **Personalizar la copia de seguridad para el servidor** tras configurar la copia de seguridad del servidor con el asistente Configurar copias de seguridad del servidor. Para obtener más información acerca de cómo configurar las copias de seguridad del servidor, vea [Configurar o personalizar las copias de seguridad del servidor](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_1).

4.  Aparece el asistente Personalizar la copia de seguridad del servidor.

5.  En la página **Opciones de configuración**, haga clic en **Deshabilitar copia de seguridad del servidor**. Siga las instrucciones del asistente.

##  <a name="learn-more-about-setting-up-server-backup"></a><a name="BKMK_5"></a>Más información sobre la configuración de copias de seguridad del servidor
 La copia de seguridad del servidor no está habilitada durante la configuración del servidor.

> [!NOTE]
>  Al configurar la copia de seguridad del servidor debe conectar como mínimo una unidad de disco duro externo al servidor para usarla como unidad de disco duro de destino de la copia de seguridad.

###  <a name="backup-destination-drive"></a><a name="BKMK_Target"></a>Unidad de destino de copia de seguridad
 Puede usar varias unidades de almacenamiento externo para efectuar las copias de seguridad y puede rotar las unidades entre ubicaciones de almacenamiento en el sitio y fuera del sitio. Esto puede mejorar la planificación de preparación ante desastres ayudando a recuperar los datos si se producen daños físicos en el hardware de las instalaciones.

 Al elegir una unidad de almacenamiento para la copia de seguridad del servidor, tenga en cuenta los siguientes aspectos:

-   Elija una unidad que contenga espacio suficiente para almacenar los datos. Las unidades de almacenamiento deben contener al menos 2,5 veces la capacidad de almacenamiento de los datos de los que quiere hacer una copia de seguridad. Las unidades también deben ser suficientemente grandes para alojar el crecimiento futuro de los datos del servidor.

-   Si la unidad de destino de la copia de seguridad contiene unidades sin conexión, la configuración de la copia de seguridad no se llevará a cabo correctamente. Para completar la configuración, al seleccionar el destino de la copia de seguridad, desactive la casilla para excluir las unidades sin conexión.

-   Si elige una unidad que contiene copias de seguridad anteriores como destino de copia de seguridad, el asistente le permite elegir si quiere mantener las copias de seguridad anteriores. Si mantiene las copias de seguridad, el asistente no formateará la unidad.

-   Cuando se reutiliza una unidad de almacenamiento externo, asegúrese de que la unidad esté vacía o que solo contenga los datos que no necesita.

-   Visite el sitio web del fabricante de la unidad de almacenamiento externo para asegurarse de que la unidad de copia de seguridad sea compatible con los equipos que ejecutan Windows Server Essentials.

> [!CAUTION]
>  El asistente Configurar copias de seguridad del servidor da formato a las unidades de almacenamiento cuando las configura para la copia de seguridad.

### <a name="server-backup-schedule"></a>Programación de copia de seguridad del servidor
 Debe proteger el servidor y sus datos de forma automática programando copias de seguridad diarias. Se recomienda mantener un plan de copias de seguridad diario porque la mayoría de las empresas no puede permitirse perder los datos creados durante varios días.

 Al usar el asistente Configurar copias de seguridad del servidor de Windows Server puede optar por hacer una copia de seguridad de los datos del servidor varias veces durante el día. Dado que el asistente programa copias de seguridad diferenciales, la copia de seguridad se ejecuta rápidamente y el rendimiento del servidor apenas se ve afectado. De manera predeterminada, **Configurar copia de seguridad del servidor** programa una copia de seguridad para que se ejecute cada día a las 12:00 y a las 23:00. Sin embargo, puede ajustar la programación de copias de seguridad según las necesidades de su organización. De vez en cuando debe evaluar la eficacia de su plan de copia de seguridad y cambiar el plan si es necesario.

> [!NOTE]
>  En la instalación predeterminada de Windows Server Essentials, el servidor está configurado para llevar a cabo una desfragmentación automáticamente una vez a la semana. Esto podría comportar que se hicieran más copias de seguridad de las habituales si usa software de creación de imágenes que no sea de Microsoft. Si no es necesario desfragmentar el servidor con regularidad puede seguir estos pasos para desactivar el programa de desfragmentación:
>
> 1. Presione la tecla de Windows + W para abrir **Buscar**.
>    2. En el cuadro de texto Buscar, escriba **Defragment**.
>    3. En la sección de resultados, haga clic en **Desfragmentar y optimizar unidades**.
>    4. En la página **Optimizar unidades**, seleccione una unidad y haga clic en **Cambiar la configuración**.
>    5. En la ventana **Programación de la optimización**, desactive la casilla **Ejecución programada (recomendado)** y, a continuación, haga clic en **Aceptar** para guardar el cambio.

### <a name="items-to-be-backed-up"></a>Elementos para la copia de seguridad
 De manera predeterminada se seleccionan todos los archivos y carpetas del sistema operativo para efectuar la copia de seguridad. Puede elegir hacer una copia de seguridad de todos los discos duros, archivos y carpetas del servidor, o seleccionar únicamente determinados discos duros, archivos o carpetas para la copia de seguridad. Para agregar o quitar elementos de la copia de seguridad, realice una de las acciones siguientes:

-   Para incluir una unidad de datos en la copia de seguridad del servidor, active la casilla adyacente.

-   Para excluir una unidad de datos de la copia de seguridad del servidor, desactive la casilla adyacente.

> [!NOTE]
>  Si quiere excluir el elemento **Sistema operativo** de la copia de seguridad, primero debe desactivar la casilla **Copia de seguridad del sistema (recomendado)**.

 Para reducir la cantidad de espacio en disco duro de copia de seguridad que usan las copias de seguridad del servidor, es posible que quiera excluir las carpetas que contienen archivos que no considere valiosos o muy importantes.

 Por ejemplo, es posible que tenga una carpeta que contiene programas de televisión grabados que emplea una gran cantidad de espacio de disco duro. Puede elegir no hacer una copia de estos archivos porque normalmente los elimina después de verlos. Asimismo, también es posible que tenga una carpeta que contiene archivos temporales que no quiere conservar.

##  <a name="repartition-a-hard-drive-on-the-server"></a><a name="BKMK_6"></a>Volver a particionar una unidad de disco duro en el servidor
 Cuando se detecta una unidad de disco duro interna sin formato en el servidor de Windows Server Essentials se genera una alerta de estado que contiene un vínculo al asistente Agregar un nuevo disco duro. El asistente Agregar un nuevo disco duro le guiará por las distintas opciones que permiten formatear la unidad de disco duro. Cuando el asistente haya finalizado, se crearán una o varias unidades lógicas de disco duro (en función del tamaño de la unidad) en la unidad de disco duro y con el formato NTFS.

 Si es necesario volver a particionar una unidad de disco duro, siga estas instrucciones:

#### <a name="to-repartition-a-hard-disk-drive"></a>Para volver a particionar una unidad de disco duro

1.  En la pantalla **Inicio**, haga clic en **Herramientas administrativas** y, a continuación, haga doble clic en **Administración de equipos**.

2.  En Administración de equipos, haga clic en **Almacenamiento** y, a continuación, haga doble clic en **Administración de discos**.

3.  Haga clic con el botón derecho en la unidad que quiere volver a particionar, haga clic en **Eliminar volumen** y, a continuación, haga clic en **Sí**.

    > [!NOTE]
    >  Repita este paso para todas las particiones de la unidad de disco duro.

4.  Haga clic con el botón derecho en la unidad de disco duro **Sin asignar** y, a continuación, haga clic en **Nuevo volumen simple**.

5.  En el asistente Nuevo volumen simple, cree y formatee un volumen de 16 TB (16.000.000 MB) o menos.

    > [!NOTE]
    >  Repita este paso hasta que se use todo el espacio sin asignar de la unidad de disco duro.

##  <a name="restore-files-and-folders-from-a-server-backup"></a><a name="BKMK_7"></a>Restaurar archivos y carpetas desde una copia de seguridad del servidor
 Puede examinar y restaurar archivos y carpetas individuales desde una copia de seguridad del servidor.

#### <a name="to-restore-files-and-folders-from-a-server-backup"></a>Para restaurar archivos y carpetas desde una copia de seguridad del servidor

1.  Abra el panel y, a continuación, haga clic en la pestaña **Dispositivos**.

2.  Haga clic en el nombre del servidor y, a continuación, haga clic en **Restaurar los archivos o las carpetas del servidor** en el panel **Tareas**.

3.  Se abre el Asistente de restauración de archivos o carpetas. Siga las instrucciones del asistente para restaurar los archivos o carpetas.

## <a name="additional-references"></a>Referencias adicionales

-   [Administrar copias de seguridad y restauración](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)

-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)

-   [Usar Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
