---
title: Administrar carpetas de servidor en Windows Server Essentials
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 090cf1b8-7b9b-48b9-ae85-b98477b8d7cc
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e1f3640f21b95acafa850b2204cd52f9c0f324e5
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="manage-server-folders-in-windows-server-essentials"></a>Administrar carpetas de servidor en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 Como administrador del servidor, puede administrar el acceso a las carpetas de servidor (conocido como carpetas compartidas al acceder a él desde el Launchpad, acceso Web remoto, servidor de mi aplicación para Windows Phone o servidor mi aplicación para Windows 8) en el servidor mediante el uso de las tareas en la **carpetas del servidor** ficha del panel, permitir a los usuarios distintos niveles de acceso a una variedad de archivos.  
  
 Los siguientes temas proporcionan información que te ayudarán a comprender, crear y administrar carpetas del servidor:  
  
-   [Administrar carpetas del servidor con el panel](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Administrar el acceso a carpetas del servidor](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Agregar o mover una carpeta del servidor](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Agregar una carpeta del servidor que faltan](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Comprender las carpetas compartidas](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [Comprender las instantáneas de volumen](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_Shadow)  
  
##  <a name="BKMK_2"></a>Administrar carpetas del servidor con el panel  
 Windows Server Essentials hace posible realizar tareas de administración comunes mediante el panel. La **carpetas del servidor** página del panel proporciona las siguientes acciones:  
  
-   Una lista de carpetas del servidor, que muestra:  
  
    -   El nombre de la carpeta  
  
    -   Una descripción de la carpeta  
  
    -   La ubicación de la carpeta  
  
    -   La cantidad de espacio libre que está disponible en la ubicación de carpeta  
  
    -   Información de estado breve sobre las tareas que se realizan en la carpeta; la **estado** campo está en blanco si la carpeta está en buen estada y si no se ejecutan tareas  
  
-   Un panel de detalles que puede proporcionar información adicional sobre una carpeta seleccionada  
  
-   Un panel de tareas que incluye un conjunto de tareas administrativas relacionadas con la carpeta  
  
 La siguiente tabla describe las diferentes tareas de carpetas de servidor que están disponibles en el escritorio de Windows Server Essentials. La mayoría de las tareas son específicos de la carpeta, y solo están visibles cuando se selecciona una carpeta en la lista.  
  
### <a name="server-folder-tasks-on-the-dashboard"></a>Tareas de carpetas de servidor en el panel  
  
|Nombre de la tarea|Descripción|  
|---------------|-----------------|  
|Abre la carpeta|Muestra el contenido de la carpeta seleccionada en el Explorador de archivos (llamado Explorador de Windows en versiones anteriores de Windows).|  
|Eliminar la carpeta|Le permite eliminar una carpeta creados por el usuario. Esta tarea no está disponible para las carpetas predeterminadas que crea la instalación del servidor.|  
|Mover la carpeta|Abre a un asistente que te ayudará a mover una carpeta del servidor a una nueva ubicación.|  
|Dejar de compartir la carpeta|Deja de compartir la carpeta seleccionada, pero no la elimina. Cuando ya no se comparte la carpeta, no aparece en el panel. Esta tarea no está disponible para las carpetas predeterminadas que crea la instalación del servidor.|  
|Ver las propiedades de carpeta|Muestra las propiedades de una carpeta seleccionada y te permite:<br /><br /> -Cambiar el nombre de carpetas creados por el usuario.<br /><br /> -Cambiar la descripción de una carpeta seleccionada.<br /><br /> -Ver el tamaño de la carpeta.<br /><br /> : Abre la carpeta seleccionada en el Explorador de archivos.<br /><br /> -Especificar permisos de acceso de la cuenta de usuario para una carpeta seleccionada.<br /><br /> -Ocultar una carpeta seleccionada desde aplicaciones de acceso Web remoto y el servicio Web.<br /><br /> -Especificar la cuota de la carpeta.|  
|Agregar una carpeta|Te ayuda a crear una nueva carpeta de servidor y asigna el nivel de acceso permitido para cada cuenta de usuario.|  
|Descripción de las carpetas de servidor|Abre un tema de Ayuda de Internet que describe el uso y la funcionalidad de carpetas del servidor.|  
  
##  <a name="BKMK_1"></a>Administrar el acceso a carpetas del servidor  
 Windows Server Essentials te permite almacenar archivos que se encuentran en los equipos cliente a una ubicación central mediante el uso de carpetas del servidor. Almacenar tus archivos en carpetas del servidor, se garantiza que los archivos están en un lugar que siempre está accesible de forma segura de cada cliente.  
  
 Te permite usar carpetas de servidor para almacenar tus archivos:  
  
-   Copia la carpeta del servidor mediante el uso de servidor de copia de seguridad y restauración para ayudar a proteger contra error total del servidor.  
  
-   Acceder a los archivos que se almacenan en la carpeta del servidor desde cualquier ubicación mediante el uso de un explorador de Internet a través de acceso Web remoto o a través de las aplicaciones mi servidor para Windows Phone y Windows 8.  
  
-   Obtener acceso a la nueva carpeta del servidor desde cualquier equipo cliente.  
  
 Puedes administrar el acceso a las carpetas de servidor en el servidor mediante el uso de las tareas en la **carpetas del servidor** ficha del panel. La siguiente tabla enumera las carpetas del servidor que se crean de forma predeterminada cuando se instalación Windows Server Essentials o activar el streaming multimedia en el servidor.  
  
|Nombre de carpeta del servidor|Descripción|  
|------------------------|-----------------|  
|Copias de seguridad del equipo de cliente|De manera predeterminada, Windows Server Essentials crea copias de seguridad del equipo que se almacenan en esta carpeta de cliente. El Administrador de red puede modificar la configuración de copias de seguridad del equipo cliente.|  
|Empresa|Se usa para almacenar y acceder a los documentos relacionados a la organización los usuarios de red.|  
|Copias de seguridad de historial de archivos|De forma predeterminada, Windows Server Essentials usa el historial de archivos para crear copias de seguridad de archivos que se almacenan en esta carpeta. Los administradores de red pueden modificar estas opciones de configuración del historial de archivos.|  
|Redirección de carpetas|Se usa para almacenar y acceder a carpetas configuradas para la redirección de carpetas de los usuarios de red.|  
|Usuarios|Usada para almacenar y acceder a los archivos que los usuarios de red. Una carpeta específica del usuario se genera automáticamente en el **usuarios** carpeta del servidor para cada cuenta de usuario de la red que crees.|  
|Música|Se usa para almacenar y acceder a archivos de música con usuarios de la red. Esta carpeta está disponible cuando activa de uso compartido de multimedia.|  
|Imágenes|Se usa para almacenar y acceder a archivos de imagen que los usuarios de red. Esta carpeta está disponible cuando activa de uso compartido de multimedia.|  
|TV grabada.|Se usa para almacenar y acceder a los programas de TV grabados los usuarios de red. Esta carpeta está disponible cuando activa de uso compartido de multimedia.|  
|Vídeos|Se usa para almacenar y acceder a archivos de vídeo por los usuarios de red. Esta carpeta está disponible cuando activa de uso compartido de multimedia.|  
  
 Para ocultar o establecer permisos de carpetas de servidor, o para modificar las propiedades de carpeta del servidor, consulte los siguientes procedimientos:  
  
-   [Ocultar carpetas del servidor](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_Hide)  
  
-   [Establecer permisos de carpetas del servidor](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_Perms)  
  
-   [Ver o modificar las propiedades de la carpeta del servidor](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_10)  
  
###  <a name="BKMK_Hide"></a>Ocultar carpetas del servidor  
 Como administrador de red, puedes ocultar cualquiera de estas carpetas de servidor e impedir que aparezca en el sitio Web de acceso Web remoto o aplicaciones de servicios Web (por ejemplo, mi servidor).  
  
> [!NOTE]
>  Debe ser un administrador de red para realizar este procedimiento.  
  
##### <a name="to-hide-server-folders-from-being-displayed-in-remote-web-access"></a>Para ocultar las carpetas de servidor no se muestren en acceso Web remoto  
  
1.  Abre el panel de Windows Server Essentials.  
  
2.  Haz clic en **almacenamiento**y, a continuación, haz clic en **carpetas del servidor**.  
  
3.  En la vista de lista, selecciona la carpeta del servidor cuyas propiedades quieres ver o modificar.  
  
4.  En la **< ServerFolder\ > tareas** panel, haz clic en **ver propiedades de carpeta**.  
  
5.  En **< FolderName\ > propiedades**, haz clic en **compartir**, selecciona **ocultar esta carpeta desde aplicaciones de acceso Web remoto y el servicio Web**y, a continuación, haz clic en **aplicar**.  
  
###  <a name="BKMK_Perms"></a>Establecer permisos de carpetas del servidor  
 Para cualquier carpeta de servidor adicionales que agregas en el servidor mediante el panel, puedes elegir tres configuración acceso diferentes:  
  
-   **Lectura y escritura**  
  
     Elige esta opción si desea permitir que esta persona para crear, modificar y eliminar los archivos en la carpeta del servidor.  
  
-   **Solo lectura**  
  
     Elige esta opción si desea permitir que esta persona solo leer los archivos de la carpeta del servidor. Los usuarios con acceso de solo lectura no se pueden crear, cambiar o eliminar los archivos en la carpeta del servidor.  
  
-   **Sin acceso**  
  
     Elige esta opción si no quieres que esta persona para acceder a los archivos en la carpeta del servidor.  
  
> [!IMPORTANT]
>  Los permisos que se muestran en las propiedades de carpeta representan solo los usuarios que administran por el panel. No incluyen los permisos de usuario como grupos o cuentas de servicio, o bien incluir ningún permiso que puede establecerse en la carpeta con otras herramientas nativas, o incluir a los usuarios que no se han agregado a través del panel.  
  
> [!NOTE]
>  Debe ser un administrador de red para realizar este procedimiento.  
  
##### <a name="to-set-permissions-to-server-folders-on-the-server"></a>Para establecer permisos en carpetas del servidor en el servidor  
  
1.  Abre el panel de Windows Server Essentials.  
  
2.  Haz clic en **almacenamiento**y, a continuación, haz clic en **carpetas del servidor**.  
  
3.  En la vista de lista, selecciona la carpeta del servidor cuyas propiedades quieres ver o modificar.  
  
4.  En la **< ServerFolder\ > tareas** panel, haz clic en **ver propiedades de carpeta**.  
  
5.  En **< FolderName\ > propiedades**, haz clic en **compartir**, selecciona el nivel de acceso de usuario adecuados para las cuentas de usuario de la lista y, a continuación, haz clic en **aplicar**.  
  
> [!NOTE]
>  De manera predeterminada, cuando agregas una cuenta de usuario a la red, se crea una subcarpeta para el usuario en el **usuarios** carpeta del servidor. Puede tener acceso a la subcarpeta desde un equipo de red solo el usuario o el administrador. Se establecen los permisos para cada subcarpeta **usuarios**, de modo que no hay ningún permiso de acceso general para el nivel superior **usuarios** carpeta.  
  
> [!NOTE]
>  No se pueden modificar los permisos de uso compartidos de la **copias de seguridad de historial de archivos**, **redirección de carpetas**, y **usuarios** carpetas del servidor. Por lo tanto, las propiedades de carpeta de estas carpetas del servidor no incluyen un **compartir** pestaña.  
  
###  <a name="BKMK_10"></a>Ver o modificar las propiedades de la carpeta del servidor  
 Puedes modificar el nombre de carpeta del servidor, su descripción y definir qué cuentas de usuario tienen acceso a una carpeta del servidor a través de la **ver las propiedades de carpeta** de tareas en la **carpetas del servidor** ficha del panel.  
  
> [!NOTE]
>  En Windows Server Essentials y Windows Server 2012 R2 con el rol de Windows Server Essentials Experience instalado, también puedes modificar la cuota de la carpeta.  
  
##### <a name="to-view-or-modify-folder-properties"></a>Para ver o modificar las propiedades de carpeta  
  
1.  Abre el panel de Windows Server Essentials.  
  
2.  Haz clic en **almacenamiento**y, a continuación, haz clic en **carpetas del servidor**.  
  
3.  En la vista de lista, selecciona la carpeta del servidor cuyas propiedades quieres ver o modificar.  
  
4.  En la **< ServerFolder\ > tareas** panel, haz clic en **ver propiedades de carpeta**.  
  
5.  En **< Foldername\ > propiedades**, en la **General** pestaña, ver o modificar el nombre y la descripción de la carpeta del servidor.  
  
    > [!NOTE]
    >  En Windows Server Essentials y Windows Server 2012 R2 con el rol de Windows Server Essentials Experience instalado, también puedes modificar la cuota de la carpeta que proporciona un mensaje de advertencia cuando una carpeta del servidor alcanza su tamaño especificado.  
  
##  <a name="BKMK_5"></a>Agregar o mover una carpeta del servidor  
 Puedes **agregar carpetas de servidor más** para almacenar tus archivos en el servidor, además de las carpetas de servidor predeterminadas que se crean durante la instalación. Puedes agregar carpetas de servidor en el servidor principal o en un servidor miembro ejecuta Windows Server Essentials.  
  
 Puedes **mover una carpeta del servidor** que se encuentra en el servidor principal que se ejecuta Windows Server Essentials y se muestra en la **carpetas del servidor** ficha del panel en otra unidad de disco duro cuando sea necesario utilizando el Asistente para carpetas de movimiento. Puedes mover una carpeta del servidor a otra dirección de la ubicación de unidad de disco duro si:  
  
-   La unidad de disco duro de datos ya no tiene espacio suficiente para almacenar datos.  
  
-   Quieres cambiar la ubicación de almacenamiento predeterminada. Para mover un más rápido, considera la posibilidad de mover la carpeta del servidor mientras esta información no incluye ningún dato.  
  
-   Quieres quitar la unidad de disco dura existente sin perder las carpetas del servidor que se encuentran en él.  
  
 Antes de mover la carpeta, asegúrate de lo siguiente:  
  
-   Asegúrate de que dispone de una copia de seguridad del servidor.  
  
-   Asegúrate de que se han detenido todas las copias de seguridad de cliente y no en curso si tienes previsto mover la carpeta de copia de seguridad de equipo cliente. Al mover la carpeta de copia de seguridad de equipo cliente, el servidor podrá hacer una copia de los equipos cliente hasta que se complete el movimiento de la carpeta.  
  
-   Asegúrate de que el servidor no está realizando operaciones críticos del sistema. Se recomienda que completar las actualizaciones o mover las copias de seguridad que están en curso, antes de empezar a una carpeta o el proceso puede tardar más en completarse.  
  
-   Ninguno de los archivos en la carpeta que se puede mover están en uso. Podrá acceder a la carpeta del servidor mientras se está moviendo.  
  
 No se admite mover una carpeta de NTFS a ReFS si los archivos en las carpetas del servidor implementan las siguientes tecnologías:  
  
-   Secuencias de datos alternativas  
  
-   Id. de objeto  
  
-   Nombres cortos (formato 8.3 nombres)  
  
-   Compresión  
  
-   Cifrado de EFS  
  
-   Transaccionales NTFS TxF (que se introdujeron con Windows Vista)  
  
-   Archivos dispersos  
  
-   Vínculos físicos  
  
-   Atributos extendidos  
  
-   Cuotas  
  
###  <a name="BKMK_6"></a>Dónde agregar o mover una carpeta del servidor  
 Por lo general, puedes agregar o mover las carpetas de servidor en unidades de disco duro que tengan la cantidad máxima de espacio libre. Si es posible, evita agregar o mover una carpeta compartida en la unidad del sistema (por ejemplo, C:) puede tardar inmediatamente el espacio en disco es necesario que se necesita para el sistema operativo y sus actualizaciones. Evita también agregar o mover carpetas del servidor en un disco duro externo, porque se puede desconectar fácilmente y, como resultado, no podrás acceder a sus archivos. En su lugar, te recomendamos que crees la carpeta en una unidad interna.  
  
 No se puede agregar una carpeta del servidor ni se ha movido a las siguientes ubicaciones y se producirá un error si ninguna de estas ubicaciones se selecciona para adiciones o movimientos:  
  
-   Un disco duro que no esté formateado con el sistema de archivos NTFS o ReFS  
  
-   La carpeta % windir %  
  
-   Una unidad de red asignadas  
  
-   Una carpeta que contiene una carpeta compartida  
  
-   Un disco duro que se encuentra en **dispositivo de almacenamiento extraíble**  
  
-   Un directorio raíz de un disco duro (por ejemplo, E:\\ C:\\, D:\\)  
  
-   Una subcarpeta de una carpeta compartida existente  
  
-   Un servidor miembro que ejecutan Windows Server Essentials o Windows Server 2012 R2 con el rol de Windows Server Essentials Experience instalado  
  
### <a name="steps-to-add-or-move-a-server-folder"></a>Pasos para agregar o mover una carpeta del servidor  
  
> [!NOTE]
>  Debe ser un administrador del servidor para completar estos procedimientos.  
  
##### <a name="to-add-a-server-folder"></a>Para agregar una carpeta del servidor  
  
1.  Abra el panel.  
  
2.  Haz clic en **almacenamiento**y, a continuación, haz clic en **carpetas del servidor**.  
  
3.  En **tareas de carpetas de servidor**, haz clic en **agregar una carpeta**. Inicia el asistente Agregar un carpeta.  
  
4.  Sigue las instrucciones para completar al asistente.  
  
    > [!NOTE]
    >  -   Si busca una carpeta específica con el botón Examinar para especificar la ubicación de carpeta del servidor, la carpeta que se ha desplazado a se agrega como una carpeta del servidor.  
    > -   Puedes definir qué carpetas del servidor se puede acceder mediante el acceso Web remoto. Para obtener más información, consulta [administrar el acceso a carpetas del servidor](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_1).  
  
##### <a name="to-move-a-server-folder"></a>Para mover una carpeta del servidor  
  
1.  Abra el panel.  
  
2.  Haz clic en **almacenamiento**y, a continuación, haz clic en **carpetas del servidor**.  
  
3.  En la lista de carpetas del servidor, seleccione la carpeta que desea mover.  
  
4.  En el panel de tareas, haz clic en **mover la carpeta**.  
  
5.  Sigue las instrucciones para completar al asistente.  
  
##  <a name="BKMK_9"></a>Agregar una carpeta del servidor que faltan  
 ¿Cuando el servidor detecta que una carpeta del servidor predefinidas? Empresa, los usuarios, las copias de seguridad del equipo de cliente, el historial de copia de seguridad o redirección de carpetas? ya no se comparte (por algún motivo u otro), se genera una alerta para guiar al usuario para resolver este problema. Se recomienda que pruebe y restaurar la carpeta de copia de seguridad del servidor. Sin embargo, si el servidor no se copia, selecciona la carpeta que falta y, a continuación, haz clic en **volver a crear la carpeta falta** volver a configurar la ubicación de la carpeta del servidor.  
  
> [!NOTE]
>  ¿Solo las carpetas predefinidas? Empresa, los usuarios, las copias de seguridad del equipo de cliente, el historial de copia de seguridad o redirección de carpetas? puede volver a crearse. No se volverán a carpetas del servidor creados por el usuario y las carpetas de servidor de multimedia.  
  
 Después de restaurar o volver a crear la carpeta que faltan, ya no se debe aparecer como **falta**.  
  
 Para obtener información sobre cómo restaurar archivos desde las copias de seguridad del servidor, consulta la sección aprender más sobre cómo restaurar archivos y carpetas en el tema [administrar copias de seguridad y restauración](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md).  
  
##  <a name="BKMK_11"></a>Comprender las carpetas compartidas  
 Hay varias formas distintas que puedes acceder desde un dispositivo que está conectado al servidor las carpetas compartidas en Windows Server Essentials. Para obtener más información, consulta el tema [carpetas compartidas de uso](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md).  
  
##  <a name="BKMK_Shadow"></a>Comprender las instantáneas de volumen  
 Con el servidor de instantáneas de volumen, los usuarios pueden ver archivos y carpetas compartidos que existían en puntos de tiempo en el pasado. Acceso a las versiones anteriores de archivos, o instantáneas, es útil porque los usuarios pueden:  
  
1.  **Recuperar archivos que se han eliminado accidentalmente**. Si eliminas accidentalmente un archivo, puedes abrir una versión anterior y copiarlo en una ubicación segura.  
  
2.  **Recuperar sobrescriba un archivo**. Si sobrescribe accidentalmente un archivo, puedes recuperar una versión anterior del archivo. (El número de versiones depende de la cantidad de instantáneas que creaste.)  
  
3.  **Comparar las versiones de un archivo mientras trabaja**. Puedes usar versiones anteriores cuando desees comprobar qué ha cambiado entre versiones de un archivo.  
  
 Para usar instantáneas de volumen, desde un equipo cliente, haz clic en una carpeta compartida del servidor y seleccione **restaurar una versión anterior**.  
  
## <a name="see-also"></a>Consulta también  
  
-   [Administrar el almacenamiento del servidor](Manage-Server-Storage-in-Windows-Server-Essentials.md)  
  
-   [Usa las carpetas compartidas](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md)  
  
-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)
