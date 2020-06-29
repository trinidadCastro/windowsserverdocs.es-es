---
title: Configurar o personalizar la copia de seguridad del servidor
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 441c2d6c-435a-42cb-90f2-6d680d279d34
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 1f53922f8ab90b75cd11785f0ad108e2a1c812ab
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85470320"
---
# <a name="set-up-or-customize-server-backup"></a>Configurar o personalizar la copia de seguridad del servidor

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

 La copia de seguridad del servidor no se configura automáticamente durante la instalación. Debe proteger el servidor y sus datos de forma automática programando copias de seguridad diarias. Se recomienda mantener un plan de copias de seguridad diario porque la mayoría de las empresas no puede permitirse perder los datos creados durante varios días.

 Vea las secciones siguientes para configurar o personalizar la copia de seguridad del servidor:

-   [Establecer o cambiar la configuración de las copias de seguridad del servidor](Set-up-or-customize-server-backup.md#BKMK_1)  &reg;

-   [Programación de copia de seguridad del servidor](Set-up-or-customize-server-backup.md#BKMK_2)

-   [Unidad de destino de copia de seguridad](Set-up-or-customize-server-backup.md#BKMK_Target)

-   [Elementos para la copia de seguridad](Set-up-or-customize-server-backup.md#BKMK_4)

##  <a name="set-up-or-change-server-backup-settings"></a><a name="BKMK_1"></a>Configurar o cambiar la configuración de copia de seguridad del servidor

#### <a name="to-set-up-or-change-server-backup-settings"></a>Para configurar o cambiar la configuración de copia de seguridad del servidor

1.  Abra el **Panel** y, a continuación, haga clic en la pestaña **Dispositivos**.

2.  En la vista de lista, haga clic en el servidor para seleccionarlo.

3.  En el panel de tareas, haga clic en **Configurar copias de seguridad de este servidor**.

    > [!NOTE]
    >  Si quiere cambiar la configuración de copia de seguridad existente, haga clic en **Personalizar la copia de seguridad para el servidor**.

4.  Siga las instrucciones del asistente.

    > [!NOTE]
    >  Si inicia el asistente antes de adjuntar el disco duro externo al servidor, haga clic en **Actualizar lista** en la página **Seleccione el destino de la copia de seguridad** después de adjuntar el disco duro.

> [!NOTE]
>  En la instalación predeterminada de Windows Server Essentials, el servidor está configurado para realizar una desfragmentación automáticamente una vez a la semana. Esto podría comportar que se hicieran más copias de seguridad de las habituales si usa software de creación de imágenes que no sea de Microsoft. Si no es necesario desfragmentar el servidor con regularidad puede seguir estos pasos para desactivar el programa de desfragmentación:
>
> 1. Presione la tecla de Windows + W para abrir **Buscar**.
>    2. En el cuadro de texto Buscar, escriba **Defragment**.
>    3. En la sección de resultados, haga clic en **Desfragmentar y optimizar las unidades**.
>    4. En la página **Optimizar unidades**, seleccione una unidad y haga clic en **Cambiar la configuración**.
>    5. En la ventana **Programación de la optimización**, desactive la casilla **Ejecución programada (recomendado)** y, a continuación, haga clic en **Aceptar** para guardar el cambio.

##  <a name="server-backup-schedule"></a><a name="BKMK_2"></a>Programación de copias de seguridad del servidor
 Al usar el asistente Configurar copias de seguridad del servidor o el Asistente Personalizar copias de seguridad del servidor, puede optar por hacer una copia de seguridad de los datos del servidor varias veces durante el día. Dado que los asistentes programan copias de seguridad incrementales, las copias de seguridad se ejecutan rápidamente y el rendimiento del servidor apenas se ve afectado. De manera predeterminada, los asistentes programan una copia de seguridad para que se ejecute cada día a las 12:00 y las 23:00. Sin embargo, puede ajustar la programación de copias de seguridad según las necesidades de su organización. De vez en cuando debe evaluar la eficacia de su plan de copia de seguridad y cambiar el plan si es necesario.

##  <a name="backup-target-drive"></a><a name="BKMK_Target"></a>Unidad de destino de copia de seguridad
 Puede usar varias unidades de almacenamiento externo para efectuar las copias de seguridad y puede rotar las unidades entre ubicaciones de almacenamiento en el sitio y fuera del sitio. Esto puede mejorar la planificación de preparación ante desastres ayudando a recuperar los datos si se producen daños físicos en el hardware de las instalaciones.

 Al elegir una unidad de almacenamiento para la copia de seguridad del servidor, tenga en cuenta los siguientes aspectos:

-   Elija una unidad que contenga espacio suficiente para almacenar los datos. Las unidades de almacenamiento deben contener al menos 2,5 veces la capacidad de almacenamiento de los datos de los que quiere hacer una copia de seguridad. Las unidades también deben ser suficientemente grandes para alojar el crecimiento futuro de los datos del servidor.

-   Cuando se use una unidad de almacenamiento externo, asegúrese de que la unidad esté vacía o que solo contenga los datos que no necesita.

    > [!CAUTION]
    >  El asistente Configurar copias de seguridad del servidor da formato a las unidades de almacenamiento cuando las configura para la copia de seguridad.

-   Si la unidad de destino de copia de seguridad contiene unidades sin conexión, la configuración de copia de seguridad no se llevará a cabo correctamente. Para completar la configuración, al seleccionar el destino de copia de seguridad, desactive la casilla para excluir las unidades sin conexión.

-   Si elige una unidad que contiene copias de seguridad anteriores como destino de copia de seguridad, el asistente le permite elegir si quiere mantener las copias de seguridad anteriores. Si mantiene las copias de seguridad, el asistente no formateará la unidad.

-   Debe visitar el sitio web del fabricante de la unidad de almacenamiento externo para asegurarse de que la unidad de copia de seguridad es compatible con los equipos que ejecutan Windows Server Essentials.

-   La unidad no puede contener una partición del sistema Extensible Firmware Interface (EFI). Si hay una partición EFI en una unidad USB se supone que el disco es un disco de inicio. Si está seguro de que no necesita los datos en el disco, puede volver a formatear el disco y usarlo para las copias de seguridad.

    > [!CAUTION]
    >  Al formatear el disco se eliminarán todos los datos.

    #### <a name="to-remove-an-efi-system-partition-from-a-usb-disk"></a>Para quitar una partición del sistema EFI de un disco USB

    1.  En el Panel de Control, abra **Sistemas y seguridad**.

    2.  En **Herramientas administrativas**, haga clic en **Crear y formatear particiones del disco duro**.

    3.  Elimine todos los volúmenes del disco USB o elimine la partición EFI. El asistente Configurar copias de seguridad del servidor eliminará todos los volúmenes.

-   La unidad no puede contener ninguna carpeta compartida del servidor. Para poder usar el disco como unidad de destino de copia de seguridad debe detener el uso compartido de todas las carpetas compartidas del servidor. Puede detener el uso compartido desde el panel o el Explorador de archivos.

    #### <a name="to-stop-sharing-on-a-server-folder-from-the-dashboard"></a>Para detener el uso compartido de una carpeta de servidor desde el panel

    1.  En el panel, haga clic en **Almacenamiento** y, a continuación, haga clic en **Carpetas de servidor**.

    2.  Seleccione la carpeta que quiera dejar de compartir y, a continuación, en el panel de tareas, haga clic en **Detener**.

> [!NOTE]
>  Si una copia de seguridad no se realiza correctamente porque la unidad de copia de seguridad no tiene suficiente espacio, la letra de unidad de la unidad de destino de copia de seguridad se quita de la base de datos de Windows Server Essentials y el panel no muestra la unidad. Si quiere usar la unidad en futuras copias de seguridad deberá reasignar la letra de unidad con una herramienta nativa.
>
>  **Para reasignar una letra de unidad de un volumen existente**
>
> 1. En el Panel de Control, abra **Sistemas y seguridad**.
>    2. En **Herramientas administrativas**, haga clic en **Crear y formatear particiones del disco duro**.
>    3. Haga clic con el botón derecho en la unidad y, a continuación, haga clic en **Cambiar la letra y rutas de acceso de unidad**.
>    4. Haga clic en **Agregar**.
>    5. En el cuadro de diálogo Agregar letra o ruta de acceso de unidad, seleccione una letra de unidad para asignarla (Puede reasignar la misma letra de unidad). A continuación, haga clic en **Aceptar**.
>
>    La unidad aparecerá inmediatamente en el panel.

##  <a name="items-to-be-backed-up"></a><a name="BKMK_4"></a>Elementos de los que se va a hacer una copia de seguridad
 Puede elegir hacer una copia de seguridad de todas las unidades, archivos y carpetas del servidor, o seleccionar únicamente determinadas unidades, archivos o carpetas para la copia de seguridad.

 Al agregar o quitar una unidad, o agregar o quitar carpetas y archivos compartidos, debe revisar la configuración de copia de seguridad del servidor para asegurarse de que se han agregado o quitado estos elementos de la configuración de copia de seguridad. Para agregar o quitar elementos de la copia de seguridad, realice una de las acciones siguientes:

- Para incluir una unidad de datos en la copia de seguridad del servidor, active la casilla adyacente.

- Para excluir una unidad de datos de la copia de seguridad del servidor, desactive la casilla adyacente.

  > [!NOTE]
  >  Si quiere excluir el elemento **Sistema operativo** de la copia de seguridad, primero debe desactivar la casilla **Copia de seguridad del sistema (recomendado)**.

  Para reducir la cantidad de almacenamiento del servidor que usan las copias de seguridad del servidor, es posible que quiera excluir las carpetas que contienen archivos que no considere valiosos o muy importantes.

  Por ejemplo, es posible que tenga una carpeta que contiene programas de televisión grabados que emplea una gran cantidad de espacio de disco duro. Puede elegir no hacer una copia de estos archivos porque normalmente los elimina después de verlos. También es posible que tenga una carpeta que contiene archivos temporales que no quiere conservar.

## <a name="additional-references"></a>Referencias adicionales

-   [Administrar copias de seguridad del servidor](Manage-Server-Backup-in-Windows-Server-Essentials.md)

-   [Administrar copias de seguridad y restauración](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)

-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)

-   [Usar Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
