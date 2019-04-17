---
title: Instalar y configurar Windows Server Essentials
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 06/17/2013
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e95cf219-46a4-4041-bd81-0c4c2a0622cf
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: cdad118c30fbf303b55ec7ea25bbe3e209c016db
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="install-and-configure-windows-server-essentials"></a>Instalar y configurar Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_InstallConfigure"></a>   

 Este documento proporciona instrucciones paso a paso para instalar y configurar Windows Server Essentials. Antes de comenzar la instalación, revisar y realizar las tareas que se describen en [antes de que instalar Windows Server Essentials](Before-You-Install-Windows-Server-Essentials.md).  

 Este documento proporciona instrucciones paso a paso para instalar y configurar Windows Server Essentials. Antes de comenzar la instalación, revisar y realizar las tareas que se describen en [antes de que instalar Windows Server Essentials](../install/Before-You-Install-Windows-Server-Essentials.md).  
  
 Instalar y configurar Windows Server Essentials en dos pasos:  
  

1.  [Paso 1: Instalar el sistema operativo de Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials.md#BKMK_ManualInstallation) en este paso, instala el sistema operativo en el servidor.  
  
2.  [Paso 2: Configurar el sistema operativo de Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials.md#BKMK_Step2Configure) en este paso, completar la instalación proporcionando información sobre la empresa, la configuración de dominio y el Administrador de red. Esta información se usa para preparar el servidor para su uso.  

1.  [Paso 1: Instalar el sistema operativo de Windows Server Essentials](../install/Install-and-Configure-Windows-Server-Essentials.md#BKMK_ManualInstallation) en este paso, instala el sistema operativo en el servidor.  
  
2.  [Paso 2: Configurar el sistema operativo de Windows Server Essentials](../install/Install-and-Configure-Windows-Server-Essentials.md#BKMK_Step2Configure) en este paso, completar la instalación proporcionando información sobre la empresa, la configuración de dominio y el Administrador de red. Esta información se usa para preparar el servidor para su uso.  

  
###  <a name="BKMK_ManualInstallation"></a>Paso 1: Instalar el sistema operativo de Windows Server Essentials  
  
> [!IMPORTANT]
>  Después de instalar el sistema operativo, no se personaliza tu servidor hasta que finalice [paso 2: configurar el sistema operativo de Windows Server Essentials](#BKMK_Step2Configure).  
  
 **El tiempo de finalización estimado:** aproximadamente 30 minutos.  
  
> [!NOTE]
>  Los tiempos de finalización estimada a lo largo de este procedimiento se basan en los requisitos mínimos de hardware.  
  
##### <a name="to-install-the-operating-system"></a>Para instalar el sistema operativo  
  
1.  Conectar el equipo a la red con un cable de red.  
  
    > [!IMPORTANT]
    >  Desconecta el equipo desde la red durante la instalación. Si lo haces, pueden provocar un error en la instalación.  
  
2.  Encienda el equipo y, a continuación, inserta el DVD de Windows Server Essentials en la unidad de DVD.  
  
     Si estás realizando una instalación desatendida, conecta el medio extraíble (como un disco o una unidad flash USB) que contiene los archivos de respuesta. Según el contenido de los archivos de respuesta, no verás algunas o ninguna de las siguientes pantallas de instalación.  
  
3.  Reinicia el equipo. Cuando el mensaje **presione cualquier tecla para arrancar desde un CD o DVD** aparece, presiona cualquier tecla.  
  
    > [!NOTE]
    >  Si el equipo no se inicia desde el DVD, asegúrate de que la unidad de CD-ROM aparece en primer lugar en la secuencia de arranque de BIOS. Para obtener más información acerca de la secuencia de arranque de BIOS, consulta la documentación del fabricante del equipo.  
  
4.  Selecciona el **idioma** que quieras instalar, **formato de hora y moneda**, y **teclado o método de entrada**y, a continuación, haz clic en **siguiente**.  
  
5.  Haz clic en **instalar ahora**.  
  
6.  En **escribir la clave de producto**, escriba la clave de producto.  
  
7.  Leer la **términos de licencia**. Si aceptas, selecciona el **acepto los términos de licencia** y, a continuación, haz clic en **siguiente**.  
  
    > [!NOTE]
    >  Si no eliges Aceptar los términos de licencia, la instalación no continuará.  
  
8.  ¿En **qué tipo de instalación desea? **, haz clic en **personalizada: instalar Windows solo (avanzado)**  
  
9. En **¿dónde desea instalar Windows? **, seleccionar el disco duro en la que desee instalar el sistema operativo Windows. Comprueba que todas las unidades de disco duras internas están disponibles para la instalación.  
  
    > [!IMPORTANT]
    >   Windows Server Essentials debe estar instalado como el volumen C: y el tamaño del volumen debe tener al menos 60 GB. Se recomienda que crear dos particiones del disco del sistema operativo y no uses la unidad C: (partición del sistema) para almacenar datos empresariales.  
  
    > [!NOTE]
    >  Si no aparece una unidad de disco duro (por ejemplo, un disco duro de serie Advanced Technology Attachment (SATA)), debe cargar los controladores de dispositivo de disco duro. Obtener el controlador de dispositivo del fabricante y guardarla en un medio extraíble (como un disco o una unidad flash USB). Conecta el medio extraíble en el equipo y, a continuación, haz clic en **cargar controlador**.  
  
     Si necesitas eliminar o crear particiones, consulte los siguientes pasos:  
  
    1.  Para eliminar una partición, selecciona la partición, haz clic en **opciones de unidad (avanzadas)**y, a continuación, haz clic en **eliminar**. Después de eliminar la partición del sistema, crear una nueva partición mediante las instrucciones en cualquier paso **b** o paso **c**.  
  
        > [!NOTE]
        >  Después de hacer clic **opciones de unidad (avanzadas)**, que opción no volverá a aparecer. En ese caso, se omite la parte del paso hace referencia a las opciones de unidad.  
  
    2.  Para crear una partición de espacio en disco sin, haz clic en el disco duro que quieres crear particiones, haz clic en **opciones de unidad (avanzadas)**, haz clic en **nueva**y, a continuación, en la **tamaño** texto, escriba el tamaño de partición que quieras crear. Por ejemplo, si usas el tamaño de partición recomendada de 120 gigabytes (GB), escribe **122880**y, a continuación, haz clic en **aplicar**. Después de crea la partición, haz clic en **siguiente**. La partición es el formato antes de la instalación continúa.  
  
    3.  Para crear una partición que use todo el espacio sin particiones, haz clic en el disco duro que quieres crear particiones, haz clic en **opciones de unidad (avanzadas)**, haz clic en **nueva**y, a continuación, haz clic en **aplicar** para aceptar el tamaño de partición predeterminado. Después de crea la partición, haz clic en **siguiente**. La partición es el formato antes de la instalación continúa.  
  
        > [!IMPORTANT]
        >  No puedes mover el sistema operativo en una partición diferente después de finalizar este paso.  
  
 Durante la instalación, se copian los archivos temporales en una carpeta de instalación en el equipo, que toma alrededor de 30 minutos. Después de instalar el sistema operativo de Windows Server Essentials, reinicia el equipo. Ahora ya estás listo para configurar el sistema operativo de Windows Server Essentials.  
  
###  <a name="BKMK_Step2Configure"></a>Paso 2: Configurar el sistema operativo de Windows Server Essentials  
  
> [!IMPORTANT]
>  Si vas a migrar desde una versión anterior de Windows Small Business Server para Windows Server Essentials, debes seguir un proceso distinto. Para obtener información acerca de las instalaciones de migración, consulta los siguientes temas:  
>   
>  -   [Migrar de Windows SBS 2003](../migrate/Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
> -   [Migrar de Windows SBS 2008](../migrate/Migrate-Windows-Small-Business-Server-2008-to-Windows-Server-Essentials.md)  
  
 Durante esta fase de la instalación, se le pedirá a algunas preguntas acerca de la organización. Esta información se usa para configurar el sistema operativo.  
  
> [!IMPORTANT]
>  Antes de comenzar este paso, asegúrate de que el adaptador de red local está conectado a un enrutador o un conmutador que está activado y funciona correctamente.  
  
 **El tiempo de finalización estimado:** aproximadamente 30 minutos  
  
##### <a name="to-configure-the-operating-system"></a>Para configurar el sistema operativo  
  
1.  En la **comprobar la fecha y la configuración de tiempo** página, haz clic en **cambiar la fecha del sistema y la configuración de tiempo** para seleccionar la configuración de fecha, hora y zona horaria para que el servidor. Cuando hayas terminado, haz clic en **siguiente**.  
  
    > [!IMPORTANT]
    >  Si vas a instalar Windows Server Essentials en una máquina virtual, asegúrate de que eliges la misma configuración de zona horaria que usa el sistema operativo host. Si la configuración de zona horaria varía, la instalación del servidor no puede realizarse correctamente.  
  
2.  En la **elegir el modo de instalación de servidor** página, realiza una de las siguientes acciones:  
  
    1.  Elige **instalación limpia** para configurar una nueva instalación del software de servidor de Windows Server Essentials.  
  
    2.  Elige **migración de servidor** para instalar Windows Server Essentials y unirte a este servidor a un dominio de Windows existente.  
  
3.  En la **personalizar tu servidor**, escriba el nombre de la organización, un nombre de dominio interno y el nombre del servidor.  
  
    > [!IMPORTANT]
    >  El nombre del servidor debe ser un nombre único de la red. No puedes cambiar el nombre del servidor o el nombre de dominio interno después de finalizar este paso.  
  
4.  Haz clic en **siguiente**.  
  
5.  En la **proporcionar información de tu cuenta de administrador**, escriba la información para una nueva cuenta de administrador.  
  
    > [!CAUTION]
    >  No nombre de la cuenta de administrador de red de administrador o administrador de red. Estos nombres de cuenta están reservados para su uso por el sistema.  
  
6.  En la **proporcionar información de tu cuenta de usuario estándar** página, escribe la información para una nueva cuenta de usuario estándar y, a continuación, haz clic en **siguiente**.  
  
7.  En la **mantener tu servidor actualizados automáticamente** página, selecciona cómo deseas recibir las actualizaciones de Windows de tu servidor y, a continuación, haz clic en **siguiente**.  
  
8.  La **actualizar y preparar el servidor** página muestra el progreso del proceso de instalación final. Esto tarda en completarse, y el equipo reiniciará un par de veces.  
  
9. El último servidor después de reiniciar, el **el servidor está listo para usarse** aparecerá la página. Haz clic en **cerrar **.  
  
10. Haz clic en el icono de escritorio en la **inicio** de pantalla y, a continuación, en el panel, completa la **establecer mi servidor** tareas en la **Home** página. Debes completar estas tareas inmediatamente una vez finalizada la instalación de Windows Server Essentials.  
  
> [!NOTE]
>  Una vez finalizada la instalación, se registran automáticamente el servidor con la nueva cuenta de administrador que agregaste durante la instalación. La contraseña de cuenta predefinida de administrador se establece en la misma contraseña que la nueva cuenta de administrador y, a continuación, se deshabilita la cuenta predefinida de administrador.  
  
 Puedes usar tu servidor recién instalado para una cantidad limitada de tiempo (conocido como el período de evaluación) sin necesidad de escribir una clave de producto. Después del período de evaluación, tienes que escribir una clave de producto para activar el servidor o ampliar el período de evaluación. Puedes extender el período de evaluación un máximo de dos veces. Cuando se alcance el número máximo de días permitidos para el período de evaluación, debe activar el servidor con una clave de producto.  
  
### <a name="customize-windows-server-essentials"></a>Personalizar Windows Server Essentials  
 La **Home** página del panel Windows Server Essentials vínculos a **instalación** tareas que se deben realizar inmediatamente después de instalación el servidor. Al realizar estas tareas, puedes ayudar a proteger la información que se almacena en el servidor y habilitar las características que están disponibles en Windows Server Essentials.  
  
 Si eliges no realizar las tareas, los usuarios no tendrán acceso a algunas características de red. Para volver a estas tareas más adelante, vuelve al escritorio de Windows Server Essentials **Home** página.  
  
 La siguiente tabla define los elementos que pueden aparecer en la lista de tareas de configuración.  
  
|Tarea|Descripción
|----------|-----------------|  
|Obtener actualizaciones para otros productos de Microsoft|Haz clic en esta tarea para tener acceso a un vínculo que se ejecuta una herramienta que te permite especificar si quieres usar Microsoft Update para obtener automáticamente actualizaciones para Windows Server Essentials y otros productos de Microsoft como Office.  
|Agregar cuentas de usuario|Haz clic en esta tarea para ver información resumida acerca de cómo agregar cuentas de usuario. Un vínculo a ejecutar el **agrega un asistente de cuenta de usuario** se proporciona. Para obtener más información, consulta [agregar una cuenta de usuario](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1).  
|Agregar carpetas de servidor|Haz clic en esta tarea para ver información resumida acerca de cómo agregar carpetas del servidor. Un vínculo a ejecutar el **carpeta Asistente para agregar un** se proporciona. También proporciona es un vínculo a un tema de ayuda en línea sobre el uso de carpetas del servidor. Para obtener más información, consulta [agregar o mover una carpeta del servidor](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5). 
|Configurar copia de seguridad del servidor|Haz clic en esta tarea para ver información resumida acerca del uso de copia de seguridad del servidor para proteger los datos. Un vínculo a ejecutar el **servidor copia de seguridad Asistente para configurar el** se proporciona. Para obtener más información, consulta [establecer hacia arriba o personalizar copias de seguridad del servidor](../manage/Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_1). 
|Configurar acceso desde cualquier lugar|Haz clic en esta tarea para ver información resumida acerca de la característica de acceso desde cualquier lugar en Windows Server Essentials. Un vínculo a la **configuración de acceso desde cualquier lugar** se proporciona la página. Para obtener más información, consulta [administrar acceso desde cualquier lugar](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md). 
|Configurar la notificación de alertas por correo electrónico|Haz clic en esta tarea para ver información resumida acerca de la notificación de alertas por correo electrónico. Un vínculo a ejecutar el **configurar correo electrónico notificación de alertas** se proporciona la herramienta. Para obtener más información, consulta [configurar notificaciones de correo electrónico de alertas](../manage/Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Email).  
|Configurar el servidor multimedia|Haz clic en esta tarea para ver información resumida sobre el uso de servidor multimedia para compartir la música, vídeos y archivos de imagen. Un vínculo a la **configuración multimedia** se proporciona la página. También proporciona es un vínculo a un tema de ayuda en línea para obtener más información sobre el servidor multimedia. Para obtener más información, consulta [administrar archivos multimedia digitales](../manage/Manage-Digital-Media-in-Windows-Server-Essentials.md). 
|Conectar equipos|Haz clic en esta tarea para ver información resumida sobre cómo conectar un equipo de red al servidor. Para obtener más información, consulta [conectar equipos con el servidor](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).
  
## <a name="see-also"></a>Consulta también  
  
-   [Instalar Windows Server Essentials](Install-Windows-Server-Essentials.md)

