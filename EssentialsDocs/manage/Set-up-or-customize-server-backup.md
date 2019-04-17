---
title: Configurar o personalizar copias de seguridad del servidor
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 441c2d6c-435a-42cb-90f2-6d680d279d34
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 727dd74e4bddc52f735969f216914b9d76d1f413
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="set-up-or-customize-server-backup"></a>Configurar o personalizar copias de seguridad del servidor

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 Copia de seguridad del servidor no se configura automáticamente durante la instalación. Debes proteger el servidor y sus datos automáticamente programando copias de seguridad diarias. Se recomienda mantener un plan de copia de seguridad diario porque la mayoría de las organizaciones no puede permitirse perder los datos que se ha creado durante varios días.  
  
 Consulta las siguientes secciones para configurar o personalizar copias de seguridad del servidor:  
  
-   [Establecer o cambiar la configuración de copia de seguridad del servidor](Set-up-or-customize-server-backup.md#BKMK_1)  
  
-   [Programación de copia de seguridad del servidor](Set-up-or-customize-server-backup.md#BKMK_2)  
  
-   [Unidad de destino de copia de seguridad](Set-up-or-customize-server-backup.md#BKMK_Target)  
  
-   [Elementos de una copia de seguridad](Set-up-or-customize-server-backup.md#BKMK_4)  
  
##  <a name="BKMK_1"></a>Establecer o cambiar la configuración de copia de seguridad del servidor  
  
#### <a name="to-set-up-or-change-server-backup-settings"></a>Para establecer o cambiar la configuración de copia de seguridad del servidor  
  
1.  Abre el **panel**y, a continuación, haz clic en el **dispositivos** pestaña.  
  
2.  En la vista de lista, haz clic en el servidor para seleccionarlo.  
  
3.  En el panel de tareas, haz clic en **Configurar copia de seguridad del servidor**.  
  
    > [!NOTE]
    >  Si quieres cambiar la configuración de copia de seguridad existente, haz clic en **personalizar copias de seguridad del servidor**.  
  
4.  Sigue las instrucciones del asistente.  
  
    > [!NOTE]
    >  Si se inicia el asistente antes de adjuntarlos al disco duro externo al servidor, haz clic en **lista actualización** en la **seleccionar el destino de copia de seguridad** página después de asociar el disco duro.  
  
> [!NOTE]
>  En la instalación predeterminada de Windows Server Essentials, el servidor está configurado para realizar automáticamente una vez a la semana de desfragmentación. Si utiliza el software de creación de imágenes no son de Microsoft, esto puede hacer en más de copias de seguridad normales. Si no es necesario desfragmentar el servidor de forma regular, puedes seguir estos pasos para desactivar la programación de desfragmentación:  
>   
>  1.  Presiona la tecla Windows + W para abrir **búsqueda**.  
> 2.  En el cuadro de texto de búsqueda, escribe **desfragmentar**.  
> 3.  En la sección de resultados, haz clic en **desfragmentar y optimizar unidades su**.  
> 4.  En la **optimizar unidades** página, selecciona una unidad y, a continuación, haz clic en **cambiar la configuración de**.  
> 5.  En la **programación de optimización** ventana, desactiva la **ejecutar según una programación (recomendada)** y, a continuación, haz clic en **Aceptar** para guardar el cambio.  
  
##  <a name="BKMK_2"></a>Programación de copia de seguridad del servidor  
 Cuando usas el asistente Configurar copia de seguridad de servidor o el Asistente para personalizar la copia de seguridad de servidor, puedes hacer la copia de seguridad de datos del servidor en varias veces durante el día. Debido a los asistentes programación incremental copias de seguridad, las copias de seguridad se ejecuten rápidamente y no se ha afectada significativamente el rendimiento del servidor. De manera predeterminada, los asistentes para programación una copia de seguridad para ejecutar todos los días en 12:00 PM y 11:00 PM. Sin embargo, puedes ajustar la programación de copia de seguridad según las necesidades de la organización. En ocasiones, debe evaluar la eficacia de tu plan de copia de seguridad y cambiar el plan según sea necesario.  
  
##  <a name="BKMK_Target"></a>Unidad de destino de copia de seguridad  
 Puedes usar varias unidades de almacenamiento externo para las copias de seguridad, y puedes girar las unidades entre las ubicaciones de almacenamiento en el sitio y fuera del sitio. Esto puede mejorar la preparación ante desastres planeación al ayudar a recuperar los datos si se producen daños físicos para el hardware de las instalaciones.  
  
 Al elegir una unidad de almacenamiento para la copia de seguridad del servidor, ten en cuenta lo siguiente:  
  
-   Elegir una unidad que contiene espacio suficiente para almacenar los datos. Las unidades de almacenamiento deben contener al menos 2,5 veces la capacidad de almacenamiento de los datos que quieres hacer copia de seguridad. Las unidades de disco también deben ser lo suficientemente grandes como para acomodar el crecimiento en el futuro de los datos del servidor.  
  
-   Al usar una unidad de almacenamiento externo, asegúrate de que la unidad está vacía o si solo contiene datos que no es necesario.  
  
    > [!CAUTION]
    >  El servidor de copia de seguridad Asistente Configurar da formato a las unidades de almacenamiento cuando se configura para la copia de seguridad.  
  
-   Si la unidad de copia de seguridad de destino contiene unidades sin conexión, la configuración de copia de seguridad no funcionarán correctamente. Para completar la configuración, al seleccionar el destino de copia de seguridad, desactiva la casilla de verificación para excluir las unidades que están sin conexión.  
  
-   Si eliges una unidad que contiene las copias de seguridad anteriores como el destino de copia de seguridad, el asistente permite que elegir si quieres mantener las copias de seguridad anteriores. Si guardas copias de seguridad, el asistente no formatees la unidad.  
  
-   Conviene que visites el sitio Web para el fabricante de la unidad de almacenamiento externo para asegurarse de que la unidad de copia de seguridad se admite en equipos que ejecutan Windows Server Essentials.  
  
-   La unidad no puede contener una partición del sistema de Extensible Firmware Interface (EFI). Si hay una partición EFI en una unidad USB, se da por hecho que el disco es un disco de inicio. Si estás seguro de que no necesitas t los datos en el disco, puedes volver a formatear el disco y Úsalo para las copias de seguridad.  
  
    > [!CAUTION]
    >  Al formatear el disco, se eliminarán todos los datos.  
  
    #### <a name="to-remove-an-efi-system-partition-from-a-usb-disk"></a>Para quitar una partición del sistema EFI desde un disco USB  
  
    1.  En el Panel de Control, abre **seguridad y sistemas**.  
  
    2.  En **herramientas administrativas**, haz clic en **crear y formatear particiones de disco duro**.  
  
    3.  Eliminar todos los volúmenes en el disco USB, o simplemente eliminar la partición EFI. El asistente Configurar copia de seguridad de servidor, se eliminarán todos los volúmenes.  
  
-   La unidad no puede contener las carpetas de servidor compartido. Antes de poder usar el disco como una unidad de destino de copia de seguridad, debe dejar de compartir en las carpetas de servidor compartido. Puedes dejar de compartir desde el panel o en el Explorador de archivos.  
  
    #### <a name="to-stop-sharing-on-a-server-folder-from-the-dashboard"></a>Para dejar de compartir en una carpeta del servidor desde el panel  
  
    1.  En el panel, haz clic en **almacenamiento**y, a continuación, haz clic en **carpetas del servidor**.  
  
    2.  Selecciona la carpeta que desea dejar de compartir y, a continuación, en el panel de tareas, haz clic en **detener**.  
  
> [!NOTE]
>  Si una copia de seguridad se realiza correctamente debido a la unidad de copia de seguridad tenido espacio suficiente, se quita la letra de unidad de la unidad de destino de copia de seguridad de la base de datos de Windows Server Essentials y el panel no muestra la unidad. Si quieres usar la unidad en futuras copias de seguridad, deberá reasignar la letra de unidad mediante una herramienta nativa.  
>   
>  **Volver a asignar una letra de unidad para un volumen existente**  
>   
>  1.  En el Panel de Control, abre **seguridad y sistemas**.  
> 2.  En **herramientas administrativas**, haz clic en **crear y formatear particiones de disco duro**.  
> 3.  Haz clic en la unidad y haz clic en **cambiar la letra de unidad y rutas de acceso**.  
> 4.  Haz clic en **agregar**.  
> 5.  En el cuadro de diálogo Agregar letra de unidad o ruta de acceso, selecciona para asignar una letra de unidad. (Puede reasignar la misma letra de unidad.) A continuación, haz clic en **Aceptar**.  
>   
>      La unidad aparecerá en el panel inmediatamente.  
  
##  <a name="BKMK_4"></a>Elementos de una copia de seguridad  
 Puedes elegir copia todas las unidades, archivos y carpetas en el servidor o seleccionar solo unidades individuales, archivos o carpetas a la copia de seguridad.  
  
 Cuando, agregan o quitar una unidad, o agregar o quitarán archivos y carpetas compartidos, debe revisar la configuración de copia de seguridad del servidor para asegurarse de que estos elementos se agregan o se quita de la configuración de copia de seguridad. Para agregar o quitar elementos de la copia de seguridad, realiza una de las siguientes acciones:  
  
-   Para incluir una unidad de datos en la copia de seguridad del servidor, selecciona la casilla de verificación adyacentes  
  
-   Para excluir una unidad de datos de la copia de seguridad del servidor, desactive la casilla de verificación adyacentes  
  
    > [!NOTE]
    >  Si quieres excluir la **sistema operativo** elemento de la copia de seguridad, primero debe borrar el **sistema copia de seguridad (recomendado)** casilla de verificación.  
  
 Para minimizar la cantidad de almacenamiento del servidor que usan las copias de seguridad del servidor, querer excluir todas las carpetas que contienen los archivos que se considere valioso o especialmente importante.  
  
 Por ejemplo, puede tener una carpeta que contiene los programas de televisión grabados que usa una gran cantidad de espacio en disco duro. Puede elegir no hacer una copia de estos archivos porque normalmente se eliminan después de visualización de todos modos. O bien, puede que una carpeta que contiene los archivos temporales que no vayas a mantener.  
  
## <a name="see-also"></a>Consulta también  
  
-   [Administrar la copia de seguridad del servidor](Manage-Server-Backup-in-Windows-Server-Essentials.md)  
  
-   [Administrar la copia de seguridad y restauración](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Usar Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
