---
title: Administrar carpetas de servidor en Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 090cf1b8-7b9b-48b9-ae85-b98477b8d7cc
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6ba5dd5e5978687c9d80a6d34e5e622aaa37c4b9
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311125"
---
# <a name="manage-server-folders-in-windows-server-essentials"></a>Administrar carpetas de servidor en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 Como administrador del servidor, puede administrar el acceso a las carpetas del servidor (conocidas como carpetas compartidas cuando se accede desde Launchpad, acceso Web remoto, mi aplicación de servidor para Windows Phone o mi aplicación de servidor para Windows 8) en el servidor mediante las tareas de la pestaña **carpetas del servidor** del panel, lo que permite a los usuarios variar los niveles de acceso a diversos archivos.  
  
 Los siguientes temas proporcionan información que le ayudará a comprender, crear y administrar las carpetas del servidor:  
  
-   [Administrar carpetas de servidor mediante el panel](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Administrar el acceso a las carpetas del servidor](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Agregar o quitar una carpeta de servidor](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Agregar una carpeta de servidor que falta](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Comprender las carpetas compartidas](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [Descripción de las instantáneas](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_Shadow)  
  
##  <a name="manage-server-folders-using-the-dashboard"></a><a name="BKMK_2"></a>Administrar carpetas de servidor mediante el panel  
 Windows Server Essentials permite realizar tareas administrativas comunes desde el panel. La página **Carpetas del servidor** del panel contiene los elementos siguientes:  
  
- Una lista de las carpetas del servidor, que muestra:  
  
  -   El nombre de la carpeta.  
  
  -   Una descripción de la carpeta.  
  
  -   La ubicación de la carpeta.  
  
  -   La cantidad de espacio libre disponible en la ubicación de la carpeta.  
  
  -   Breve información de estado sobre las tareas que se realizan en la carpeta. El campo **Estado** está en blanco si el estado de la carpeta es correcto y no se está ejecutando ninguna tarea.  
  
- Un panel de detalles que puede proporcionar información adicional sobre una carpeta seleccionada.  
  
- Un panel de tareas que incluye un conjunto de tareas administrativas relacionadas con la carpeta.  
  
  La tabla siguiente describe las distintas tareas de carpeta del servidor que están disponibles en el panel de Windows Server Essentials. La mayoría de las tareas son específicas de una carpeta y solo pueden verse cuando se selecciona una carpeta en la lista.  
  
### <a name="server-folder-tasks-on-the-dashboard"></a>Tareas de carpeta del servidor en el panel  
  
|Nombre de tarea|Descripción|  
|---------------|-----------------|  
|Abrir la carpeta|Muestra el contenido de la carpeta seleccionada en el Explorador de archivos (denominado Explorador de Windows en las versiones anteriores de Windows).|  
|Elimina la carpeta|Permite eliminar una carpeta creada por el usuario. Esta tarea no está disponible para las carpetas predeterminadas que crea la instalación del servidor.|  
|Mover carpeta|Abre a un asistente que le ayudará a mover una carpeta del servidor a una nueva ubicación.|  
|Dejar de compartir la carpeta|Deja de compartir la carpeta seleccionada, pero no la elimina. Las carpetas que ya no se comparten no aparecen en el panel. Esta tarea no está disponible para las carpetas predeterminadas que crea la instalación del servidor.|  
|Ver las propiedades de la carpeta|Muestra las propiedades de la carpeta seleccionada y permite:<br /><br /> -Cambiar el nombre de las carpetas creadas por el usuario.<br /><br /> : Cambiar la descripción de una carpeta seleccionada.<br /><br /> -Ver el tamaño de la carpeta.<br /><br /> -Abra la carpeta seleccionada en el explorador de archivos.<br /><br /> -Especifique los permisos de acceso de la cuenta de usuario para una carpeta seleccionada.<br /><br /> -Ocultar una carpeta seleccionada de las aplicaciones de acceso Web remoto y servicio Web.<br /><br /> -Especifique la cuota de la carpeta.|  
|Agregar una carpeta|Ayuda a crear una nueva carpeta del servidor y asignar el nivel de acceso permitido a cada cuenta de usuario.|  
|Información acerca de las carpetas del servidor|Abre un tema de Ayuda en Internet que describe el uso y la funcionalidad de las carpetas del servidor.|  
  
##  <a name="manage-access-to-server-folders"></a><a name="BKMK_1"></a>Administrar el acceso a las carpetas del servidor  
 Windows Server Essentials permite almacenar los archivos de los equipos cliente en una ubicación central mediante las carpetas del servidor. Al almacenar los archivos en carpetas del servidor, se garantiza que se mantendrán en un lugar al que se podrá acceder de forma segura en todo momento desde los distintos clientes.  
  
 Si almacena los archivos en las carpetas del servidor, podrá:  
  
- Hacer una copia de seguridad de la carpeta del servidor con las características de copia de seguridad y restauración del servidor, como medida de protección frente a un error generalizado de este.  
  
- Acceder a archivos que están almacenados en la carpeta del servidor desde cualquier ubicación con un explorador de Internet, a través del acceso Web remoto o las aplicaciones Mi servidor para Windows Phone y Windows 8.  
  
- Acceder a la nueva carpeta del servidor desde cualquier equipo cliente.  
  
  Puede administrar el acceso a cualquier carpeta del servidor desde el propio servidor, con las tareas de la pestaña **Carpetas del servidor** del panel. La tabla siguiente enumera las carpetas del servidor que se crean de forma predeterminada al instalar Windows Server Essentials o activar el streaming en el servidor.  
  
|Nombre de la carpeta del servidor|Descripción|  
|------------------------|-----------------|  
|Copias de seguridad de equipos cliente|De forma predeterminada, Windows Server Essentials crea copias de seguridad de equipos cliente que se almacenan en esta carpeta. El administrador de red puede modificar la configuración de Copias de seguridad de equipos cliente.|  
|Compañía|Se usa para que los usuarios de red almacenen y accedan a documentos relacionados con la organización.|  
|Copias de seguridad del Historial de archivos|De forma predeterminada, Windows Server Essentials usa el Historial de archivos para crear copias de seguridad de los archivos que se almacenan en esta carpeta. Los administradores de red pueden modificar esta configuración del Historial de archivos.|  
|Redirección de carpetas|Se usa para que los usuarios de red almacenen y accedan a carpetas configuradas para la redirección de carpetas.|  
|Usuarios|Se usa para que los usuarios de red almacenen y accedan a archivos. Se genera automáticamente una carpeta específica del usuario en la carpeta de servidor **Usuarios** para cada cuenta de usuario de red que se cree.|  
|Música|Se usa para que los usuarios de red almacenen y accedan a archivos de música. Esta carpeta está disponible cuando se activa el uso compartido de multimedia.|  
|Imágenes|Se utiliza para que los usuarios de red puedan almacenar archivos de imagen y acceder a ellos. Esta carpeta está disponible cuando se activa el uso compartido de multimedia.|  
|TV grabada|Se usa para que los usuarios de red almacenen y accedan a programas de TV grabados. Esta carpeta está disponible cuando se activa el uso compartido de multimedia.|  
|Vídeos|Se utiliza para que los usuarios de red puedan almacenar archivos de vídeo y acceder a ellos. Esta carpeta está disponible cuando se activa el uso compartido de multimedia.|  
  
 Si desea ocultar o establecer permisos para las carpetas del servidor, o modificar las propiedades de estas, consulte los procedimientos siguientes:  
  
-   [Ocultar carpetas del servidor](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_Hide)  
  
-   [Establecer permisos para las carpetas del servidor](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_Perms)  
  
-   [Ver o modificar las propiedades de la carpeta del servidor](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_10)  
  
###  <a name="hide-server-folders"></a><a name="BKMK_Hide"></a>Ocultar carpetas del servidor  
 Como administrador de red, puede ocultar cualquiera de estas carpetas del servidor e impedir que aparezcan en el sitio web de acceso Web remoto o en aplicaciones de servicio Web.  
  
> [!NOTE]
>  Para realizar este procedimiento debe ser un administrador de red.  
  
##### <a name="to-hide-server-folders-from-being-displayed-in-remote-web-access"></a>Para ocultar las carpetas del servidor en el acceso Web remoto:  
  
1.  Abra el panel de Windows Server Essentials.  
  
2.  Haga clic en **ALMACENAMIENTO** y después, en **Carpetas del servidor**.  
  
3.  En la vista de lista, seleccione la carpeta del servidor cuyas propiedades desea ver o modificar.  
  
4.  En el panel **tareas de < carpetadelservidor\>** , haga clic en **Ver propiedades de carpeta**.  
  
5.  En **< nombrecarpeta\> propiedades**, haga clic en **compartir**, seleccione **ocultar esta carpeta en aplicaciones de acceso Web remoto y servicio Web**y, a continuación, haga clic en **aplicar**.  
  
###  <a name="set-permissions-to-server-folders"></a><a name="BKMK_Perms"></a>Establecer permisos para las carpetas del servidor  
 Para cualquier carpeta del servidor adicional que desee agregar a este desde el panel, puede elegir tres configuraciones de acceso diferentes:  
  
-   **Lectura/escritura**  
  
     Elija este valor para permitir a la persona crear, cambiar y eliminar archivos en la carpeta del servidor.  
  
-   **Solo lectura**  
  
     Elija este valor para solo permitir a la persona leer archivos en la carpeta del servidor. Los usuarios con acceso de solo lectura no pueden crear, cambiar o eliminar archivos de la carpeta del servidor.  
  
-   **Sin acceso**  
  
     Elija este valor para no permitir a la persona el acceso a ningún archivo de la carpeta del servidor.  
  
> [!IMPORTANT]
>  Los permisos que se muestran en las propiedades de la carpeta representan solo a los usuarios que se administran desde el panel. O bien no incluyen permisos de usuario (como cuentas de servicio o grupos), o incluyen permisos que se pueden establecer en la carpeta con otras herramientas nativas, o incluyen a los usuarios que no se han agregado a través del panel.  
  
> [!NOTE]
>  Para realizar este procedimiento debe ser un administrador de red.  
  
##### <a name="to-set-permissions-to-server-folders-on-the-server"></a>Para establecer permisos para las carpetas del servidor en este:  
  
1.  Abra el panel de Windows Server Essentials.  
  
2.  Haga clic en **ALMACENAMIENTO** y después, en **Carpetas del servidor**.  
  
3.  En la vista de lista, seleccione la carpeta del servidor cuyas propiedades desea ver o modificar.  
  
4.  En el panel **tareas de < carpetadelservidor\>** , haga clic en **Ver propiedades de carpeta**.  
  
5.  En **< nombrecarpeta\> propiedades**, haga clic en **compartir**y seleccione el nivel de acceso de usuario adecuado para las cuentas de usuario de la lista y, a continuación, haga clic en **aplicar**.  
  
> [!NOTE]
>  De forma predeterminada, al agregar una cuenta de usuario a la red, se crea una subcarpeta para el usuario bajo la carpeta **Usuarios** en el servidor. Únicamente el usuario o el administrador pueden acceder a la subcarpeta desde un equipo de red. Los permisos se establecen para cada subcarpeta en **Usuarios**, por lo que no hay ningún permiso de acceso general para la carpeta **Usuarios** de primer nivel.  
  
> [!NOTE]
>  No puede modificar los permisos de uso compartido de las carpetas del servidor **Copias de seguridad del Historial de archivos**, **Redirección de carpetas**y **Usuarios** . Por tanto, las propiedades de carpeta de estas carpetas del servidor no incluyen la pestaña **Uso compartido**.  
  
###  <a name="view-or-modify-server-folder-properties"></a><a name="BKMK_10"></a>Ver o modificar las propiedades de la carpeta del servidor  
 Puede modificar el nombre de la carpeta del servidor y su descripción, y definir las cuentas de usuario que tienen acceso a una carpeta del servidor, con la tarea **Ver las propiedades de la carpeta** de la pestaña **Carpetas del servidor** del panel.  
  
> [!NOTE]
>  En Windows Server Essentials y Windows Server 2012 R2 con el rol de experiencia con Windows Server Essentials instalado, también puede modificar la cuota de carpeta.  
  
##### <a name="to-view-or-modify-folder-properties"></a>Para ver o modificar las propiedades de carpeta:  
  
1.  Abra el panel de Windows Server Essentials.  
  
2.  Haga clic en **ALMACENAMIENTO** y después, en **Carpetas del servidor**.  
  
3.  En la vista de lista, seleccione la carpeta del servidor cuyas propiedades desea ver o modificar.  
  
4.  En el panel **tareas de < carpetadelservidor\>** , haga clic en **Ver propiedades de carpeta**.  
  
5.  En **< nombrecarpeta\> propiedades**, en la pestaña **General** , vea o modifique el nombre y la descripción de la carpeta del servidor.  
  
    > [!NOTE]
    >  En Windows Server Essentials y Windows Server 2012 R2 con el rol de experiencia con Windows Server Essentials instalado, también puede modificar la cuota de carpeta que emite un mensaje de advertencia cuando una carpeta del servidor alcanza el tamaño especificado.  
  
##  <a name="add-or-move-a-server-folder"></a><a name="BKMK_5"></a>Agregar o quitar una carpeta de servidor  
 Puede **agregar más carpetas del servidor** para almacenar los archivos en el servidor, además de las carpetas del servidor predeterminadas que se crean durante la instalación. Con Windows Server Essentials, puede agregar carpetas del servidor en el servidor principal o en un servidor miembro.  
  
 Con el asistente para mover carpetas, si es preciso puede **mover una carpeta del servidor** que se encuentre en el servidor principal, ejecute Windows Server Essentials y se muestre en la pestaña **Carpetas del servidor** del panel, a otra unidad de disco duro. Puede mover una carpeta del servidor a otra dirección de ubicación de la unidad de disco duro si:  
  
- La unidad de disco duro de datos ya no tiene suficiente espacio para almacenar los datos.  
  
- Desea cambiar la ubicación de almacenamiento predeterminada. Para aumentar la velocidad, considere la posibilidad de mover la carpeta del servidor, aunque no incluya datos.  
  
- Desea quitar la unidad de disco duro existente sin perder las carpetas del servidor que contiene.  
  
  Antes de mover la carpeta, asegúrese de lo siguiente:  
  
- Ha creado una copia de seguridad del servidor.  
  
- Todas las copias de seguridad de equipos clientes están detenidas y no en curso, si va a mover la carpeta de copias de seguridad de equipos cliente. Al mover la carpeta de copia de seguridad de equipos cliente, el servidor no podrá crear ninguna de estas copias hasta que la carpeta se transfiera por completo.  
  
- Asegúrese de que el servidor no está realizando ninguna operación crítica del sistema. Se recomienda completar todas las actualizaciones o copias de seguridad en curso antes de mover una carpeta. De lo contrario, el proceso tardará más.  
  
- Ninguno de los archivos de la carpeta que desea mover está en uso. No podrá acceder a la carpeta del servidor mientras se esté moviendo.  
  
  No se podrá mover una carpeta de NTFS a ReFS si los archivos de las carpetas del servidor implementan las siguientes tecnologías:  
  
- Secuencias de datos alternativas  
  
- Id. de objeto  
  
- Nombres cortos (nombres 8.3)  
  
- Compresión  
  
- Cifrado EFS  
  
- TxF, NTFS transaccional (implementado en Windows Vista)  
  
- Archivos dispersos  
  
- Vínculos físicos  
  
- Atributos extendidos  
  
- Cuotas  
  
###  <a name="where-to-add-or-move-a-server-folder"></a><a name="BKMK_6"></a>Dónde agregar o quitar una carpeta de servidor  
 Por lo general, debe agregar o mover las carpetas del servidor a unidades de disco duro con la máxima cantidad de espacio libre. Si es posible, evite agregar o mover una carpeta compartida a la unidad de sistema (por ejemplo, C:). Puede consumir el espacio de unidad necesario para el sistema operativo y sus actualizaciones. Además, evite agregar o mover las carpetas del servidor a una unidad de disco duro externa, porque pueden desconectarse fácilmente y, en ese caso, no podría acceder a los archivos. En su lugar, se recomienda crear la carpeta en una unidad interna.  
  
 Las carpetas del servidor no se pueden agregar o mover a las siguientes ubicaciones (se produciría un error si se seleccionara cualquiera de ellas para agregar o mover elementos):  
  
-   Una unidad de disco duro que no esté formateada con el sistema de archivos NTFS o ReFS.  
  
-   La carpeta %windir%.  
  
-   Una unidad de red asignada.  
  
-   Una carpeta que contenga una carpeta compartida.  
  
-   Una unidad de disco duro de un **dispositivo con almacenamiento extraíble**.  
  
-   Un directorio raíz de una unidad de disco duro (por ejemplo, C:\\, D:\\, E:\\)  
  
-   Una subcarpeta de una carpeta compartida existente.  
  
-   Un servidor miembro que ejecuta Windows Server Essentials o Windows Server 2012 R2 con el rol de experiencia con Windows Server Essentials instalado  
  
### <a name="steps-to-add-or-move-a-server-folder"></a>Pasos para agregar o mover una carpeta del servidor  
  
> [!NOTE]
>  Debe ser un administrador del servidor para completar estos procedimientos.  
  
##### <a name="to-add-a-server-folder"></a>Para agregar una carpeta del servidor:  
  
1. Abra el Panel.  
  
2. Haga clic en **ALMACENAMIENTO** y después, en **Carpetas del servidor**.  
  
3. En **Tareas de carpeta del servidor**, haga clic en **Agregar una carpeta**. Así se inicia el asistente para agregar una carpeta.  
  
4. Siga las instrucciones para completar el asistente.  
  
   > [!NOTE]
   > - Si busca una carpeta concreta con el botón Examinar para especificar la ubicación de la carpeta del servidor, la carpeta que ha visitado se agregará como carpeta del servidor.  
   >   -   Puede definir a qué carpetas del servidor se podrá acceder a través del acceso Web remoto. Para más información, consulte [Administrar el acceso a las carpetas del servidor](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_1).  
  
##### <a name="to-move-a-server-folder"></a>Para mover una carpeta del servidor:  
  
1.  Abra el Panel.  
  
2.  Haga clic en **ALMACENAMIENTO** y después, en **Carpetas del servidor**.  
  
3.  En la lista de carpetas del servidor, seleccione la carpeta que desea mover.  
  
4.  En el panel de tareas, haga clic en **Mover carpeta**.  
  
5.  Siga las instrucciones para completar el asistente.  
  
##  <a name="add-a-missing-server-folder"></a><a name="BKMK_9"></a>Agregar una carpeta de servidor que falta  
 ¿Cuándo detecta el servidor una carpeta de servidor predefinida? La compañía, los usuarios, las copias de seguridad de los equipos cliente, la copia de seguridad del historial de archivos o la redirección de carpetas ya no se comparten (por alguna razón u otra), se genera una alerta para guiar al usuario para resolver este problema. Se recomienda intentar restaurar la carpeta de la copia de seguridad del servidor. Sin embargo, si no ha creado la copia de seguridad en el servidor, seleccione la carpeta que falta y haga clic en **Vuelva a crear la carpeta que falta** para configurar de nuevo la ubicación de la carpeta del servidor.  
  
> [!NOTE]
>  ¿Solo las carpetas predefinidas? Se pueden volver a crear la compañía, los usuarios, las copias de seguridad del equipo cliente, la copia de seguridad del historial de archivos o la redirección de carpetas. No pueden crearse de nuevo las carpetas del servidor multimedia ni las carpetas del servidor creadas por el usuario.  
  
 Después de restaurar o volver a crear la carpeta que falta, esta ya no debe aparecer como **Falta**.  
  
 Para obtener información acerca de cómo restaurar archivos de copias de seguridad del servidor, consulte la sección más información sobre la restauración de archivos y carpetas en el tema [administrar copias de seguridad y restauración](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md).  
  
##  <a name="understand-shared-folders"></a><a name="BKMK_11"></a>Comprender las carpetas compartidas  
 Desde un dispositivo conectado al servidor, puede acceder a las carpetas compartidas en Windows Server Essentials de varias maneras. Para obtener más información, vea el tema [uso de carpetas compartidas](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md).  
  
##  <a name="understand-shadow-copies"></a><a name="BKMK_Shadow"></a>Descripción de las instantáneas  
 Con las instantáneas de servidor, los usuarios pueden ver carpetas y archivos compartidos tal como eran en determinados momentos del pasado. El acceso a versiones anteriores de los archivos, o instantáneas, es útil porque permite a los usuarios:  
  
1. **Recuperar archivos eliminados de forma accidental**. Si elimina accidentalmente un archivo, puede abrir una versión anterior y copiarla en una ubicación segura.  
  
2. **Recuperar un archivo tras sobrescribirlo accidentalmente**. Si sobrescribe accidentalmente un archivo, puede recuperar una versión anterior del archivo. (El número de versiones dependerá de la cantidad de instantáneas que haya creado).  
  
3. **Comparar versiones de un archivo mientras trabaja**. Puede usar las versiones anteriores si desea comprobar qué ha cambiado entre las versiones de un archivo.  
  
   Para usar las instantáneas, desde un equipo cliente, haga clic en una carpeta compartida del servidor y seleccione **Restaurar la versión anterior**.  
  
## <a name="see-also"></a>Vea también  
  
-   [Administrar el almacenamiento del servidor](Manage-Server-Storage-in-Windows-Server-Essentials.md)  
  
-   [Usar carpetas compartidas](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md)  
  
-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)
