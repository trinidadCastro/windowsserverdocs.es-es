---
title: Administrar la copia de seguridad de servidor en Windows Server Essentials
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0302d070-c58a-40f2-b56d-7e7842813d02
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 2b0cd926b15d65e5cd4c784681c40df29b18a48f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="manage-server-backup-in-windows-server-essentials"></a>Administrar la copia de seguridad de servidor en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 Los siguientes temas incluyen información sobre las tareas de copia de seguridad comunes que se pueden realizar mediante el panel de Windows Server Essentials:  
  
-   [¿Qué copia de seguridad debo elegir?](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_WhichBackup)  
  
-   [Configurar o personalizar copias de seguridad del servidor](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Detener la copia de seguridad de servidor en curso](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Administrar las copias de seguridad de forma remota](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Deshabilitar la copia de seguridad del servidor](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Más información sobre la configuración de copia de seguridad del servidor](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Volver a particionar una unidad de disco duro en el servidor](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Restaurar archivos y carpetas desde una copia de seguridad del servidor](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_7)  
  
##  <a name="BKMK_WhichBackup"></a>¿Qué copia de seguridad debo elegir?  
 Elección de una copia de seguridad puede ser un proceso sencillo si tienes una copia de seguridad correcta muy reciente y se sabe que la copia de seguridad contiene todos los datos críticos. Si intentas restaurar en el servidor o un equipo desde una copia de seguridad anterior, puede requerir la elección de una buena copia de seguridad para restaurar a algunos investigación y, posiblemente, algunos poner en peligro.  
  
#### <a name="to-choose-a-backup"></a>Para elegir una copia de seguridad  
  
1.  Ponte en contacto con el propietario de los archivos o carpetas y ten en cuenta las fechas y horas cuando agregado o modificado ellos. Usa estas fechas y horas como punto de partida.  
  
2.  En la **elige una opción de restauración** página en el Asistente de carpetas y restaurar archivos, haz clic en **restaurar a partir de una copia de seguridad que seleccione (avanzado)**.  
  
3.  Según si quieres restaurar una versión anterior o posterior de los archivos o carpetas, selecciona la copia de seguridad que mejor se adapte a las fechas y horas que anotó en el paso 1.  
  
4.  Como procedimiento recomendado, puedes restaurar archivos y carpetas en una ubicación alternativa y, a continuación, permitir que el propietario de los archivos y carpetas mover las que necesitan a la ubicación original. Cuando acaban, se pueden eliminar los archivos y carpetas que permanecen en la ubicación alternativa.  
  
##  <a name="BKMK_1"></a>Configurar o personalizar copias de seguridad del servidor  
 Copia de seguridad del servidor no se configura automáticamente durante la instalación. Debes proteger el servidor y sus datos automáticamente programando copias de seguridad diarias. Se recomienda mantener un plan de copia de seguridad diario porque la mayoría de las organizaciones no puede permitirse perder los datos que se ha creado durante varios días. Para obtener más información, consulta [establecer hacia arriba o personalizar copias de seguridad del servidor](Set-up-or-customize-server-backup.md).  
  
##  <a name="BKMK_2"></a>Detener la copia de seguridad de servidor en curso  
 Si se inicia una copia de seguridad del servidor a una hora programada periódicamente o iniciar una copia de seguridad de servidor manualmente, puedes detener la copia de seguridad en curso.  
  
#### <a name="to-stop-a-backup-in-progress"></a>Para detener una copia de seguridad en curso  
  
1.  Abra el panel.  
  
2.  En la barra de navegación, haz clic en **dispositivos**.  
  
3.  En la lista de equipos, haz clic en el servidor y, a continuación, haz clic en **Detener copia de seguridad del servidor** en la **tareas** panel.  
  
4.  Haz clic en **Sí** para confirmar la acción.  
  
##  <a name="BKMK_3"></a>Administrar las copias de seguridad de forma remota  
 Cuando estés fuera de la oficina, puedes usar el acceso remoto de Web de Windows Server Essentials para acceder al panel de Windows Server Essentials para administrar el servidor.  
  
#### <a name="to-use-remote-web-access-to-manage-your-server"></a>Para usar el acceso Web remoto para administrar el servidor  
  
1.  Abre un explorador web.  
  
2.  En el cuadro de dirección, escribe el nombre del dominio de Windows Server Essentials.  
  
3.  Cuando se te pida, escribe tu nombre de usuario y contraseña.  
  
4.  Cuando haces clic en el nombre del servidor de acceso Web remoto, se muestra la página de inicio de sesión para el escritorio.  
  
5.  Iniciar sesión en el panel como administrador y, a continuación, haz clic en **dispositivos**.  
  
 Para obtener más información acerca del acceso Web remoto, vea [información general de acceso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Overview).  
  
##  <a name="BKMK_4"></a>Deshabilitar la copia de seguridad del servidor  
 Debes proteger el servidor y sus datos automáticamente programando copias de seguridad diarias. Se recomienda mantener un plan de copia de seguridad diario porque la mayoría de las organizaciones no puede permitirse perder los datos que se ha creado durante varios días.  
  
 Si ya tienes configurada de copia de seguridad de servidor y, más adelante, quieres usar una aplicación de terceros para hacer copia de seguridad del servidor, puedes deshabilitar la copia de seguridad de servidor de Windows Server Essentials.  
  
#### <a name="to-disable-server-backup"></a>Para deshabilitar la copia de seguridad del servidor  
  
1.  Iniciar sesión en el escritorio de Windows Server Essentials con una cuenta de administrador y una contraseña.  
  
2.  Haz clic en el **dispositivos** pestaña y, a continuación, haz clic en el nombre del servidor.  
  
3.  En el panel de tareas, haz clic en **personalizar copia de seguridad del servidor**.  
  
    > [!NOTE]
    >  La **personalizar copias de seguridad del servidor** se muestra después de haber configurado la copia de seguridad de servidor mediante el servidor de copia de seguridad Asistente para configurar la tarea. Para obtener más información sobre la configuración de copia de seguridad del servidor, consulta [establecer hacia arriba o personalizar copias de seguridad del servidor](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_1).  
  
4.  Aparece el Asistente para personalizar la copia de seguridad de servidor.  
  
5.  En la **opciones de configuración** página, haz clic en **deshabilitar la copia de seguridad del servidor**. Sigue las instrucciones del asistente.  
  
##  <a name="BKMK_5"></a>Más información sobre la configuración de copia de seguridad del servidor  
 Copia de seguridad del servidor no está habilitado durante la instalación del servidor.  
  
> [!NOTE]
>  Cuando se configura la copia de seguridad del servidor, debe conectarse al menos una unidad de disco dura externa para el servidor para su uso como la unidad de disco duro de copia de seguridad de destino.  
  
###  <a name="BKMK_Target"></a>Unidad de destino de copia de seguridad  
 Puedes usar varias unidades de almacenamiento externo para las copias de seguridad, y puedes girar las unidades entre las ubicaciones de almacenamiento en el sitio y fuera del sitio. Esto puede mejorar la preparación ante desastres planeación al ayudar a recuperar los datos si se producen daños físicos para el hardware de las instalaciones.  
  
 Al elegir una unidad de almacenamiento para la copia de seguridad del servidor, ten en cuenta lo siguiente:  
  
-   Elegir una unidad que contiene espacio suficiente para almacenar los datos. Las unidades de almacenamiento deben contener al menos 2,5 veces la capacidad de almacenamiento de los datos que quieres hacer copia de seguridad. Las unidades de disco también deben ser lo suficientemente grandes como para acomodar el crecimiento en el futuro de los datos del servidor.  
  
-   Si la unidad de copia de seguridad de destino contiene unidades sin conexión, la configuración de copia de seguridad no funcionarán correctamente. Para completar la configuración, al seleccionar el destino de copia de seguridad, desactiva la casilla de verificación para excluir las unidades que están sin conexión.  
  
-   Si eliges una unidad que contiene las copias de seguridad anteriores como el destino de copia de seguridad, el asistente permite que elegir si quieres mantener las copias de seguridad anteriores. Si guardas copias de seguridad, el asistente no formatees la unidad.  
  
-   Al volver a usar una unidad de almacenamiento externo, asegúrese de que la unidad esté vacía o si solo contiene datos que no es necesario.  
  
-   Conviene que visites el sitio Web para el fabricante de la unidad de almacenamiento externo para asegurarse de que la unidad de copia de seguridad se admite en equipos que ejecutan Windows Server Essentials.  
  
> [!CAUTION]
>  El servidor de copia de seguridad Asistente Configurar da formato a las unidades de almacenamiento cuando se configura para la copia de seguridad.  
  
### <a name="server-backup-schedule"></a>Programación de copia de seguridad del servidor  
 Debes proteger el servidor y sus datos automáticamente programando copias de seguridad diarias. Se recomienda mantener un plan de copia de seguridad diario porque la mayoría de las organizaciones no puede permitirse perder los datos que se ha creado durante varios días.  
  
 Cuando usas el Windows Server Essentials servidor copia de seguridad Asistente Configurar, puedes hacer la copia de seguridad de datos del servidor en varias veces durante el día. Dado que el Asistente del programa copias de seguridad basadas en diferencial, copia de seguridad se ejecuta rápidamente, y no se ha afectada significativamente el rendimiento del servidor. De manera predeterminada, **establecer una copia de seguridad Server** programa una copia de seguridad para ejecutar todos los días en 12:00 PM y 11:00 PM. Sin embargo, puedes ajustar la programación de copia de seguridad según las necesidades de la organización. En ocasiones, debe evaluar la eficacia de tu plan de copia de seguridad y cambiar el plan según sea necesario.  
  
> [!NOTE]
>  En la instalación predeterminada de Windows Server Essentials, el servidor está configurado para realizar automáticamente una vez a la semana de desfragmentación. Si utiliza el software de creación de imágenes no son de Microsoft, esto puede hacer en más de copias de seguridad normales. Si no es necesario desfragmentar el servidor de forma regular, puedes seguir estos pasos para desactivar la programación de desfragmentación:  
>   
>  1.  Presiona la tecla Windows + W para abrir **búsqueda**.  
> 2.  En el cuadro de texto de búsqueda, escribe **desfragmentar**.  
> 3.  En la sección de resultados, haz clic en **desfragmentar y optimizar unidades**.  
> 4.  En la **optimizar unidades** página, selecciona una unidad y, a continuación, haz clic en **cambiar la configuración de**.  
> 5.  En la **programación de optimización** ventana, desactiva la **ejecutar según una programación (recomendada)** y, a continuación, haz clic en **Aceptar** para guardar el cambio.  
  
### <a name="items-to-be-backed-up"></a>Elementos de una copia de seguridad  
 De manera predeterminada, se seleccionan todos los archivos del sistema operativo y las carpetas de copia de seguridad. Puedes elegir copia todos los discos duros, archivos y carpetas en el servidor o seleccionar solo los discos duros individuales, archivos o carpetas a la copia de seguridad. Para agregar o quitar elementos de la copia de seguridad, realiza una de las siguientes acciones:  
  
-   Para incluir una unidad de datos en la copia de seguridad del servidor, selecciona la casilla de verificación adyacente.  
  
-   Para excluir una unidad de datos de la copia de seguridad del servidor, desactive la casilla de verificación adyacente.  
  
> [!NOTE]
>  Si quieres excluir la **sistema operativo** elemento de la copia de seguridad, primero debe borrar el **sistema copia de seguridad (recomendado)** casilla de verificación.  
  
 Para minimizar la cantidad de espacio en disco de copia de seguridad que usan las copias de seguridad del servidor, querer excluir todas las carpetas que contienen los archivos que no considere valioso o especialmente importante.  
  
 Por ejemplo, puede tener una carpeta que contiene los programas de televisión grabados que usa una gran cantidad de espacio en disco duro. Puede elegir no hacer una copia de estos archivos porque normalmente se eliminan después de visualización de todos modos. Como alternativa, puede que una carpeta que contiene los archivos temporales que no vayas a mantener.  
  
##  <a name="BKMK_6"></a>Volver a particionar una unidad de disco duro en el servidor  
 Cuando se detecta una unidad de disco duro interna sin formato en el servidor de Windows Server Essentials, se genera una alerta de salud que contiene un vínculo a agregar un nuevo Asistente de unidad de disco duro. Agregar un nuevo Asistente de unidad de disco duro recorre las distintas opciones para formatear el disco duro. Cuando termine el asistente, se crearán en el disco duro uno o más, según el tamaño de la unidad, unidades de disco duro lógicas con formato y el formato NTFS.  
  
 Si es necesario volver a particionar una unidad de disco duro, sigue estas instrucciones:  
  
#### <a name="to-repartition-a-hard-disk-drive"></a>Para volver a particionar una unidad de disco duro  
  
1.  En la **inicio** de pantalla, haz clic en **herramientas administrativas**y, a continuación, haz doble clic en **administración de equipos**.  
  
2.  En la administración de equipos, haz clic en **almacenamiento**y, a continuación, haz doble clic en **administración de discos**.  
  
3.  Haz clic en la unidad que quieres volver a particionar, haz clic en **Eliminar volumen**y, a continuación, haz clic en **Sí**.  
  
    > [!NOTE]
    >  Repite este paso para cada partición en la unidad de disco duro.  
  
4.  Haz clic en el **Unallocated** unidad de disco duro y, a continuación, haz clic en **nuevo volumen Simple**.  
  
5.  En el nuevo Asistente de volumen Simple, crear y formatear un volumen que es de 16 TB (16,000,000 MB) o menos.  
  
    > [!NOTE]
    >  Repite este paso hasta que se usa todo el espacio sin asignar en el disco duro.  
  
##  <a name="BKMK_7"></a>Restaurar archivos y carpetas desde una copia de seguridad del servidor  
 Puedes examinar y restaurar archivos y carpetas individuales de una copia de seguridad del servidor.  
  
#### <a name="to-restore-files-and-folders-from-a-server-backup"></a>Para restaurar archivos y carpetas desde una copia de seguridad del servidor  
  
1.  Abra el panel y, a continuación, haz clic en el **dispositivos** pestaña.  
  
2.  Haz clic en el nombre del servidor y, a continuación, haz clic en **restaurar archivos o carpetas del servidor** en la **tareas** panel.  
  
3.  Abre el restaurar archivos o carpetas del asistente. Sigue las instrucciones del Asistente para restaurar los archivos o carpetas.  
  
## <a name="see-also"></a>Consulta también  
  
-   [Administrar la copia de seguridad y restauración](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Usar Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
