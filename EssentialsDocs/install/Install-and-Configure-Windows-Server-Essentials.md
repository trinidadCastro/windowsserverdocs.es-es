---
title: Instalación y configuración de Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.date: 06/17/2013
ms.prod: windows-server
ms.topic: article
ms.assetid: e95cf219-46a4-4041-bd81-0c4c2a0622cf
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 504ce971691d85c6d3727bacd6419f548673c88a
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85470900"
---
# <a name="install-and-configure-windows-server-essentials"></a>Instalación y configuración de Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_InstallConfigure"></a>

 En este documento se proporcionan instrucciones paso a paso para instalar y configurar Windows Server Essentials. Antes de comenzar la instalación, revise y complete las tareas que se describen en [antes de instalar Windows Server Essentials](Before-You-Install-Windows-Server-Essentials.md).

 En este documento se proporcionan instrucciones paso a paso para instalar y configurar Windows Server Essentials. Antes de comenzar la instalación, revise y complete las tareas que se describen en [antes de instalar Windows Server Essentials](../install/Before-You-Install-Windows-Server-Essentials.md).

 Instale y configure Windows Server Essentials en dos pasos:


###  <a name="step-1-install-the-windows-server-essentials-operating-system"></a><a name="BKMK_ManualInstallation"></a>Paso 1: instalar el sistema operativo Windows Server Essentials

> [!IMPORTANT]
>  Una vez instalado el sistema operativo, no Personalice el servidor hasta que finalice el [paso 2: configurar el sistema operativo Windows Server Essentials](#BKMK_Step2Configure).

 **Tiempo de finalización estimado:** 30 minutos aproximadamente.

> [!NOTE]
>  Los tiempos de finalización estimados para este procedimiento se basan en los requisitos mínimos de hardware.

##### <a name="to-install-the-operating-system"></a>Para instalar el sistema operativo

1. Conecte el equipo a la red con un cable de red.

   > [!IMPORTANT]
   >  Mantenga el equipo conectado a la red durante la instalación. Si no lo hace así, podría producirse un error en la instalación.

2. Encienda el equipo y, a continuación, inserte el DVD de Windows Server Essentials en la unidad de DVD.

    Si va a realizar una instalación desatendida, conecte el medio extraíble (como un disquete o una unidad flash USB) que contenga los archivos de respuesta. Según el contenido de los archivos de respuesta, puede que no vea alguna o ninguna de las siguientes pantallas de instalación.

3. Reinicie el equipo. Cuando aparezca el mensaje **Presione cualquier tecla para iniciar desde el CD o el DVD**, presione una tecla.

   > [!NOTE]
   >  Si el equipo no se inicia desde el DVD, asegúrese de que la unidad de CD-ROM aparezca en primer lugar en la secuencia de arranque del BIOS. Para obtener más información acerca de la secuencia de arranque del BIOS, consulte la documentación del fabricante del equipo.

4. Seleccione el **Idioma** de la instalación, el **Formato de hora y moneda** y el **Teclado o método de entrada** y, a continuación, haga clic en **Siguiente**.

5. Haga clic en **Install now** (Instalar ahora).

6. En **Escriba la clave del producto**, escriba la clave del producto.

7. Lea los **Términos de licencia**. Si los acepta, active la casilla **Acepto los términos de licencia** y, a continuación, haga clic en **Siguiente**.

   > [!NOTE]
   >  Si decide no aceptar los términos de licencia, la instalación no continuará.

8. En **¿Qué tipo de instalación desea?**, haga clic en **Personalizada: instalar solo Windows (avanzado)**

9. En **¿Dónde desea instalar Windows?**, seleccione la unidad de disco duro donde desea instalar el sistema operativo Windows. Compruebe que todas las unidades de disco duro internas estén disponibles para la instalación.

    > [!IMPORTANT]
    >   Windows Server Essentials debe instalarse como volumen C:, y el tamaño del volumen debe ser de al menos 60 GB. Se recomienda crear dos particiones en el disco del sistema operativo, en lugar de utilizar la C: (partición del sistema) para almacenar los datos de la empresa.

    > [!NOTE]
    >  Si alguna de las unidades de disco duro no se muestra (por ejemplo, un disco duro Serial Advanced Technology Attachment (SATA)), deberá cargar los controladores de dispositivo de ese disco duro. Obtenga el controlador de dispositivo del fabricante y guárdelo en un medio extraíble (como un disquete o una unidad flash USB). Conecte el medio extraíble al equipo y, a continuación, haga clic en **Cargar controlador**.

     Si necesita eliminar o crear particiones, consulte los siguientes pasos:

    1.  Para eliminar una partición, selecciónela, haga clic en **Opciones de unidad (avanzadas)** y, a continuación, haga clic en **Eliminar**. Después de eliminar la partición del sistema, cree una nueva partición siguiendo las instrucciones del paso **b** o del paso **c**.

        > [!NOTE]
        >  Una vez que ha hecho clic en **Opciones de unidad (avanzadas)**, esa opción no volverá a aparecer. En ese caso, omita la parte del paso que hace referencia a las opciones de unidad.

    2.  Para crear una partición a partir de un espacio sin particionar, haga clic en el disco duro que desea particionar, haga clic en **Opciones de unidad (avanzadas)**, haga clic en **Nuevo** y, a continuación, en el cuadro de texto **Tamaño**, escriba la partición que desea crear. Por ejemplo, si utiliza el tamaño de partición recomendado de 120 gigabytes (GB), escriba **122880**y, a continuación, haga clic en **Aplicar**. Una vez creada la partición, haga clic en **Siguiente**. La partición se formatea antes de continuar la instalación.

    3.  Para crear una partición que utilice todo el espacio sin particionar, haga clic en el disco duro que desea particionar, haga clic en **Opciones de unidad (avanzadas)**, haga clic en **Nuevo** y, a continuación, haga clic en **Aplicar** para aceptar el tamaño de partición predeterminado. Una vez creada la partición, haga clic en **Siguiente**. La partición se formatea antes de continuar la instalación.

        > [!IMPORTANT]
        >  Tras finalizar este paso, no podrá mover el sistema operativo a otra partición.

   Durante la instalación, los archivos temporales se copian en una carpeta de instalación en el equipo. Esta operación dura aproximadamente 30 minutos. Una vez instalado el sistema operativo Windows Server Essentials, el equipo se reinicia. Ahora está listo para configurar el sistema operativo Windows Server Essentials.

###  <a name="step-2-configure-the-windows-server-essentials-operating-system"></a><a name="BKMK_Step2Configure"></a>Paso 2: configurar el sistema operativo Windows Server Essentials

> [!IMPORTANT]
>  Si va a migrar desde una versión anterior de Windows Small Business Server a Windows Server Essentials, debe seguir un proceso diferente. Para obtener información acerca de la migración de instalaciones, consulte:
>
> - [Migrar desde Windows SBS 2003](../migrate/Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)
>   -   [Migrar desde Windows SBS 2008](../migrate/Migrate-Windows-Small-Business-Server-2008-to-Windows-Server-Essentials.md)

 Durante esta fase de la instalación, se le pedirá que responda a unas preguntas acerca de su organización. Esta información se utiliza para configurar el sistema operativo.

> [!IMPORTANT]
>  Antes de comenzar este paso, asegúrese de que el adaptador de red local esté conectado a un enrutador o a un conmutador que esté encendido y funcione correctamente.

 **Tiempo de finalización estimado:** 30 minutos aproximadamente

##### <a name="to-configure-the-operating-system"></a>Para configurar el sistema operativo

1.  En la página **Compruebe la configuración de fecha y hora**, haga clic en **Cambiar la configuración de fecha y hora del sistema** para seleccionar la configuración de fecha, hora y zona horaria de su servidor. Cuando haya terminado, haga clic en **Siguiente**.

    > [!IMPORTANT]
    >  Si va a instalar Windows Server Essentials en una máquina virtual, asegúrese de elegir la misma configuración de zona horaria usada por el sistema operativo host. Si la configuración de zona horaria es diferente, puede que la instalación del servidor no se realice correctamente.

2.  En la página **Elegir el modo de instalación del servidor**, realice uno de los siguientes pasos:

    1.  Elija **instalación limpia** para configurar una nueva instalación completa del software de servidor de Windows Server Essentials.

    2.  Elija **migración de servidor** para instalar Windows Server Essentials y unir este servidor a un dominio de Windows existente.

3.  En la página **Personalice el servidor**, escriba el nombre de la organización, un nombre del dominio interno y el nombre del servidor.

    > [!IMPORTANT]
    >  El nombre del servidor debe ser un nombre único en la red. Una vez finalizado este paso, no podrá cambiar el nombre del servidor ni el nombre del dominio interno.

4.  Haga clic en **Next**.

5.  En la página **Especifique la información de la cuenta de administrador**, escriba la información de una nueva cuenta de administrador.

    > [!CAUTION]
    >  No asigne un nombre al administrador de la cuenta de administrador de red o al administrador de red. Estos nombres de cuenta están reservados para el sistema.

6.  En la página **Especifique la información de su cuenta de usuario estándar**, escriba la información de una nueva cuenta de usuario estándar y, a continuación, haga clic en **Siguiente**.

7.  En la página **Mantenga el servidor actualizado automáticamente**, seleccione cómo desea recibir actualizaciones de Windows para el servidor y, a continuación, haga clic en **Siguiente**.

8.  La página **Actualizar y preparar el servidor** muestra el progreso del proceso final de instalación. Esta operación tarda tiempo en realizarse, y el equipo se reiniciará un par de veces.

9. Al finalizar el último reinicio del servidor, aparece la página **El servidor está listo para utilizarse**. Haga clic en **Cerrar**.

10. Haga clic en el icono Panel de la pantalla **Inicio** y, a continuación, en el Panel, realice las tareas de **Configurar el servidor** en la página **Inicio**. Debe completar estas tareas inmediatamente después de que finalice la instalación de Windows Server Essentials.

> [!NOTE]
>  Una vez finalizada la instalación, iniciará automáticamente la sesión en el servidor con la nueva cuenta de administrador que agregó durante este proceso. La contraseña de la cuenta de administrador integrada se configura con la misma contraseña que la nueva cuenta de administrador y, a continuación, se deshabilita la cuenta de administrador integrada.

 Puede usar el servidor recién instalado durante una cantidad de tiempo limitada (conocido como período de evaluación) sin especificar una clave de producto. Después del período de evaluación, debe escribir una clave de producto para activar el servidor o ampliar dicho período. El período de evaluación se puede ampliar dos veces como máximo. Al llegar al número máximo de días permitidos para el período de evaluación, debe activar el servidor con una clave de producto.

### <a name="customize-windows-server-essentials"></a>Personalizar Windows Server Essentials
 La página **principal** del panel de Windows Server Essentials incluye vínculos a las tareas de **configuración** que debe completar inmediatamente después de instalar el servidor. Al realizar estas tareas, puede ayudar a proteger la información que está almacenada en el servidor y habilitar las características que están disponibles en Windows Server Essentials.

 Si opta por no realizar las tareas, puede que los usuarios no tengan acceso a algunas de las características de red. Para volver a estas tareas más adelante, vuelva a la página **principal** del panel de Windows Server Essentials.

 En la siguiente tabla se definen los elementos que pueden aparecer en la lista de tareas de configuración.

|Tarea|Descripción
|----------|-----------------|
|Obtener actualizaciones para otros productos de Microsoft|Haga clic en esta tarea para tener acceso a un vínculo que ejecuta una herramienta que le permite especificar si desea usar Microsoft Update para obtener automáticamente actualizaciones para Windows Server Essentials y otros productos de Microsoft como Office.
|Agregar cuentas de usuario|Haga clic en esta tarea para ver información breve acerca de cómo agregar cuentas de usuario. Se proporciona un vínculo al **Asistente para agregar cuentas de usuario**. Para obtener más información, vea cómo [agregar una cuenta de usuario](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1).
|Agregar carpetas de servidor|Haga clic en esta tarea para ver información breve acerca de cómo agregar carpetas de servidor. Se proporciona un vínculo al **Asistente para agregar carpetas**. También se proporciona un vínculo a un tema de ayuda en pantalla acerca del uso de carpetas de servidor. Para obtener más información, vea cómo [agregar o mover una carpeta del servidor](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5).
|Configurar copia de seguridad del servidor|Haga clic en esta tarea para ver información breve acerca del uso de Copia de seguridad del servidor para proteger sus datos. Se proporciona un vínculo al **Asistente para configuración de copia de seguridad del servidor**. Para obtener más información, vea cómo [configurar o personalizar una copia de seguridad del servidor](../manage/Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_1).
|Configurar Acceso desde cualquier lugar|Haga clic en esta tarea para ver información breve acerca de la característica acceso desde cualquier lugar en Windows Server Essentials. Se proporciona un vínculo a la página **Configuración de Acceso desde cualquier lugar**. Para obtener más información, consulte [administrar el acceso desde cualquier lugar](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md).
|Configurar notificación de alertas por correo electrónico|Haga clic en esta tarea para ver información breve acerca de la notificación de alertas por correo electrónico. Se proporciona un vínculo a la herramienta **Configurar notificación de alertas por correo electrónico**. Para obtener más información, vea cómo [configurar las notificaciones de correo electrónico para recibir alertas](../manage/Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Email).
|Configurar el servidor multimedia|Haga clic en esta tarea para ver información breve acerca del uso del servidor multimedia para compartir archivos de música, de vídeo y de imagen. Se proporciona un vínculo a la página  **Configuración de medios** . También se proporciona un vínculo a un tema de ayuda en pantalla para obtener más información acerca del servidor multimedia. Para obtener más información, vea [administrar medios digitales](../manage/Manage-Digital-Media-in-Windows-Server-Essentials.md).
|Conectar los equipos|Haga clic en esta tarea para ver información breve acerca de cómo conectar un equipo de red al servidor. Para obtener más información, consulte [Conectar los equipos al servidor](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)

## <a name="additional-references"></a>Referencias adicionales

-   [Instalación de Windows Server 2012 Essentials](Install-Windows-Server-Essentials.md)

