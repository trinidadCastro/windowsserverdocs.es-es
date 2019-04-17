---
title: Administrar el almacenamiento del servidor en Windows Server Essentials
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1836682e-c7bb-4dd5-a2b5-6ff032693574
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e0f65dfd25afbd584764d33904ba82e4da4c5443
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="manage-server-storage-in-windows-server-essentials"></a>Administrar el almacenamiento del servidor en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
   
 Windows Server Essentials te permite administrar todo el almacenamiento del servidor (incluidas las unidades de disco duro y espacios de almacenamiento) desde el **unidades de disco duro** páginas en la **almacenamiento** ficha del panel.  
  
 Las secciones siguientes proporcionan información que te ayudarán a aumentar el almacenamiento del servidor, comprender y usar espacios de almacenamiento y administrar las unidades de disco duras:  
  
-   [Administrar unidades de disco duro usando el panel](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Aumentar la capacidad de almacenamiento en el servidor](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Realizar comprobaciones y reparaciones en unidades de disco duro](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_Check)  
  
-   [Unidades de disco duro de formato](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Agregar una nueva unidad de disco dura](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Información general de espacios de almacenamiento](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Crear un espacio de almacenamiento](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_6)  
  
##  <a name="BKMK_1"></a>Administrar unidades de disco duro usando el panel  
 Windows Server Essentials le permite administrar todas las unidades de disco duras que están conectadas al servidor a través del panel. En el panel **almacenamiento** ficha, **unidades de disco duro** muestra todas las unidades de disco duras que están disponibles en el servidor para almacenar las copias de seguridad de datos y el servidor. El servidor supervisa el espacio disponible en cada unidad de disco duro y muestra una alerta si queda poco espacio de disco duro. La **unidades de disco duro** pestaña muestra la siguiente información:  
  
-   El nombre de cada unidad de disco duro  
  
-   La capacidad de cada unidad de disco duro  
  
-   La cantidad de espacio utilizado en cada unidad de disco duro  
  
-   La cantidad de espacio libre en cada unidad de disco duro  
  
-   El estado de cada unidad de disco duro; un estado en blanco significa que el disco duro está funcionando correctamente  
  
-   El panel de detalles, que muestra toda la información de la pila de almacenamiento (por grupo de almacenamiento, el espacio de almacenamiento y la unidad de disco duro) si la unidad de disco dura seleccionada se encuentra en un espacio de almacenamiento (en lugar de un disco físico)  
  
 La siguiente tabla enumera las tareas de administración de la unidad de disco duro que están disponibles en el panel y sus descripciones. Algunas de las tareas se muestran solamente cuando se selecciona una unidad de disco duro.  
  
### <a name="available-hard-drive-management-tasks"></a>Tareas de administración de la unidad de disco duro  
  
|Nombre de la tarea|Descripción|  
|---------------|-----------------|  
|**Ver las propiedades de la unidad de disco duro**|Abre el *HardDriveName***propiedades** página. Esta tarea se muestra cuando se selecciona la unidad de disco duro. La **General** ficha de la *HardDriveName* página Propiedades incluye las siguientes tareas adicionales:<br /><br /> -   **Liberador de espacio en disco**: permite limpiar los archivos en el disco duro (esta tarea solo está disponible en Windows Server Essentials).<br />-   **Comprobar y reparar**: comprueba el disco duro para errores del sistema de archivos e intenta reparar errores detectados automáticamente.<br /><br /> La **instantáneas** ficha de la *HardDriveName***propiedades** página te permite habilitar instantáneas de volumen. Esta pestaña también muestra la próxima vez que instantáneas está programada para ejecutarse.|  
|**Administrar espacios de almacenamiento**|**Nota:** para Windows Server Essentials, esta tarea solo se muestra cuando hay un espacio de almacenamiento existente.<br /><br /> Abre el **espacios de almacenamiento** panel de control, desde el que puedes crear y administrar grupos de almacenamiento y espacios de almacenamiento.|  
|**Crear un espacio de almacenamiento**|Abre la crear un asistente de espacio de almacenamiento, que permite usar uno o más unidades de disco duro para aumentar la capacidad de un grupo de almacenamiento.|  
|**Aumentar la capacidad del grupo de almacenamiento**|**Nota:** esta tarea solo es visible, si la unidad de disco dura seleccionada se encuentra en un espacio de almacenamiento.<br /><br /> Abre la capacidad de aumentar de un asistente de grupo de almacenamiento, que te permite usar uno o más unidades de disco duro para aumentar la capacidad de un grupo de almacenamiento.|  
  
##  <a name="BKMK_2"></a>Aumentar la capacidad de almacenamiento en el servidor  
 Para aumentar el almacenamiento en el servidor, puedes agregar un disco duro interno adicional al servidor. Para agregar el disco duro interno adicional, debes apagar el servidor, agrega el disco duro interno y, a continuación, reiniciar el servidor. No es necesario apagar el servidor, si el disco duro se adjunta el controlador SCSI. En ese caso, se puede conectar el disco duro mientras se ejecuta el servidor.  
  
 Dependiendo de si se formatea el disco duro para agregarse, realiza una de las siguientes acciones:  
  
-   **El formato** si el disco duro interno esté formateado con NTFS o ReFS, el servidor le asigna una letra de unidad y el disco duro aparece en la **unidades de disco duro** pestaña. Ahora puedes crear o mover carpetas de servidor en el disco duro nuevo.  
  
-   **No tiene el formato** si el disco duro interno es sin formato, aparecerá el siguiente mensaje: uno o más discos duros están conectados al servidor. Usa el siguiente procedimiento para formatear el disco duro.  
  
#### <a name="to-format-the-hard-disk"></a>Para formatear el disco duro  
  
1.  Abra el panel.  
  
2.  En el panel de navegación, haz clic en el icono de alerta para iniciar la **Visor de alertas** en Windows Server Essentials, o la **supervisión de estado** ficha en Windows Server Essentials.  
  
3.  En la **Visor de alertas** o **supervisión de estado** pestaña, haz clic en la alerta y, a continuación, en el panel de tareas, haz clic en **solucionar este problema**.  
  
4.  Sigue las instrucciones para completar el asistente Agregar un nuevo disco duro de unidad.  
  
###  <a name="BKMK_Clean"></a>Para limpiar el disco duro  
  
1.  Abra el panel.  
  
2.  En el panel de navegación, haz clic en **almacenamiento**y, a continuación, haz clic en **unidades de disco duro**.  
  
3.  En la **unidades de disco duro** sección, selecciona la letra de unidad que se asignó al disco duro recién agregado y, en el panel de tareas, haz clic en **ver las propiedades de la unidad de disco duro**.  
  
4.  En **< Driveletter\ > propiedades**, en la **General** , haga clic **Liberador de espacio en disco**.  
  
##  <a name="BKMK_Check"></a>Realizar comprobaciones y reparaciones en unidades de disco duro  
 Los discos duros comprobar y reparar proceso comprueba el estado del sistema de archivos almacenado en unidades de disco duro. Se ejecuta un **chkdsk** procesos en el volumen que están almacenados los archivos de copia de seguridad en. El siguiente problema alerta pueden resolver mediante la ejecución de una comprobación y reparar en unidades de disco duro:  
  
-   Uno o más discos duros en copia de seguridad del servidor deben comprobarse.  
  
#### <a name="to-check-and-repair-hard-drives"></a>Para comprobar y reparar las unidades de disco duro  
  
1.  Abra el panel.  
  
2.  Haz clic en **discos duros y carpetas de servidor**y, a continuación, haz clic en **unidades de disco duro**.  
  
3.  Selecciona el disco duro que muestra el error y, a continuación, selecciona **ver las propiedades de la unidad de disco duro**.  
  
4.  En la **comprobar y reparar** , haga clic **comprobar y reparar**.  
  
##  <a name="BKMK_3"></a>Unidades de disco duro de formato  
 Cuando se detecta una unidad de disco dura interna sin formato en el servidor, una alerta de salud guía al usuario a través del proceso de formato. Agregar un nuevo Asistente de unidad de disco duro te guía a través de formatear el disco duro y te permite configurar el disco duro en una de las siguientes maneras:  
  
1.  Formatea el disco duro y crear automáticamente una unidad en él. Si eliges esta opción, cuando se completa el asistente, se crea una unidad de disco duro lógica formateada con el sistema de archivos NTFS.  
  
2.  Formatea el disco duro y configurado para la copia de seguridad del servidor. Si elige esta opción, se inicia el servidor de copia de seguridad Asistente para configurar y te guía a través de la configuración de copia de seguridad del servidor.  
  
3.  Si existe un t de espacio de almacenamiento, usa el nuevo disco duro para crear un espacio de almacenamiento. Debes tener al menos dos unidades de disco duro para crear un espacio de almacenamiento.  
  
4.  Si ya existe un espacio de almacenamiento, usar el nuevo disco duro para aumentar la capacidad de un grupo de almacenamiento. Esta opción solo aparece si hay un espacio de almacenamiento existente creado en el servidor. Si eliges esta opción, el asistente agregará esta unidad de disco duro al grupo de almacenamiento.  
  
##  <a name="BKMK_4"></a>Agregar una nueva unidad de disco dura  
 Cuando conectes una nueva unidad de disco duro en un servidor que ejecuta Windows Server Essentials, puedes:  
  
-   [Usar el nuevo disco duro para almacenar las carpetas del servidor](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4a)  
  
-   [Usar el nuevo disco duro para almacenar copias de seguridad del servidor](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4b)  
  
-   [Usar el nuevo disco duro para aumentar la capacidad del grupo de almacenamiento](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4c)  
  
###  <a name="BKMK_4a"></a>Usar el nuevo disco duro para almacenar las carpetas del servidor  
 Para usar el nuevo disco duro para carpetas del servidor de la tienda, puedes agregar una nueva carpeta de servidor en el disco duro o mover una carpeta existente de servidor en el disco duro.  
  
##### <a name="to-store-server-folders"></a>Para almacenar las carpetas del servidor  
  
1.  Abra el panel.  
  
2.  Haz clic en el **almacenamiento** pestaña y, a continuación, haz clic en **carpetas del servidor**.  
  
3.  En la **tareas de carpetas de servidor** panel, realiza una de las siguientes acciones:  
  
    1.  Para agregar una carpeta del servidor, haz clic en **agregar una carpeta**.  
  
    2.  Para mover una carpeta del servidor, selecciona la carpeta que quieras mover a la nueva unidad de disco dura y, a continuación, haz clic en **mover una carpeta**.  
  
    > [!NOTE]
    >  Si busca en el disco duro y selecciona como ubicación de carpeta del servidor sin tener que crear una carpeta, aparecerá el siguiente mensaje de error: **un directorio raíz (por ejemplo, C:\\, D:\\) no se pueden agregar como una carpeta del servidor. Crea una nueva carpeta o selecciona una existente en el directorio raíz y, a continuación, vuelve a intentarlo**. Para resolver este error, crea una nueva carpeta en el disco duro recién agregado y, a continuación, selecciona la nueva carpeta como la ubicación para almacenar las carpetas del servidor.  
  
4.  Sigue las instrucciones para finalizar al asistente.  
  
 Para obtener más información sobre cómo mover carpetas de servidor, consulta [agregar o mover una carpeta del servidor](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5).  
  
###  <a name="BKMK_4b"></a>Usar el nuevo disco duro para almacenar copias de seguridad del servidor  
 Puedes usar la unidad de disco dura recién agregada para almacenar copias de seguridad del servidor.  
  
##### <a name="to-store-server-backups"></a>Para almacenar las copias de seguridad del servidor  
  
1.  Abra el panel.  
  
2.  Haz clic en el **dispositivos** pestaña, selecciona el servidor desde el panel de lista y, a continuación, en el panel de tareas, realiza una de las siguientes acciones:  
  
    1.  Si la copia de seguridad del servidor no está configurado en el servidor, haz clic en **Configurar copia de seguridad del servidor**.  
  
    2.  Si la copia de seguridad del servidor está configurado en el servidor, haz clic en **personalizar copia de seguridad del servidor**.  
  
     Aparece el asistente Configurar copia de seguridad de servidor.  
  
3.  En la **seleccionar el destino de copia de seguridad** página, selecciona el nuevo disco duro como el destino de copia de seguridad.  
  
4.  Sigue las instrucciones para finalizar al asistente.  
  
###  <a name="BKMK_4c"></a>Usar el nuevo disco duro para aumentar la capacidad del grupo de almacenamiento  
 Cuando la capacidad del grupo de almacenamiento es baja, recibe una alerta que indica que puede aumentar la capacidad del grupo de almacenamiento agregando una nueva unidad de disco dura al grupo de almacenamiento con la capacidad de aumentar de un asistente de grupo de almacenamiento.  
  
> [!NOTE]
>  Puedes realizar este procedimiento solo si has creado un grupo de almacenamiento en el servidor.  
  
##### <a name="to-increase-storage-pool-capacity"></a>Para aumentar la capacidad del grupo de almacenamiento  
  
1.  Abra el panel.  
  
2.  Haz clic en el **almacenamiento** pestaña y, a continuación, haz clic en **unidades de disco duro**.  
  
3.  Selecciona la unidad que se muestra una baja capacidad.  
  
4.  En el panel de tareas, selecciona **aumentar la capacidad del grupo de almacenamiento**. Aparece la capacidad de aumentar de un asistente de grupo de almacenamiento.  
  
5.  Sigue las instrucciones para finalizar al asistente.  
  
##  <a name="BKMK_5"></a>Información general de espacios de almacenamiento  
 Espacios de almacenamiento permite agrupar juntos discos en un grupo de almacenamiento. A continuación, puedes usar capacidad del grupo para crear espacios de almacenamiento. Espacios de almacenamiento son unidades virtuales que aparecen en la **unidades de disco duro** ficha del panel. Puedes usar espacios de almacenamiento como cualquier otra unidad, por lo tanto, se s fácil trabajar con archivos en ellos. Cuando te queda poca capacidad del grupo, puedes crear grandes espacios de almacenamiento y agregar más unidades al grupo de almacenamiento. Si tienes dos o más discos dentro del grupo de almacenamiento, puedes crear espacios de almacenamiento con un reflejo doble que no se verán afectado por un error de disco? o incluso el error de dos unidades? si creas un espacio de almacenamiento de reflejo triple.  
  
 Para crear un espacio de almacenamiento, todo lo que necesitas es una o varias unidades adicionales además la unidad donde está instalado Windows. Estas unidades pueden ser las unidades de disco duro internas o externas o unidades de estado sólido. Puedes usar una variedad de tipos de unidades con espacios de almacenamiento, incluidas las unidades USB, SATA y SAS.  
  
> [!NOTE]
>  Si configuras espacios de almacenamiento en un servidor que ejecuta Windows Server Essentials, no puedes realizar un restablecimiento de fábrica con el **datos limpia** opción. La solución alternativa para este problema es quitar espacios de almacenamiento y, a continuación, realizar un restablecimiento de fábrica con el **datos limpia** opción.  
  
 Para obtener más información sobre los espacios de almacenamiento, consulte [almacenamiento espacios más preguntas frecuentes (P+F)](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx).  
  
##  <a name="BKMK_6"></a>Crear un espacio de almacenamiento  
 Para empezar a trabajar con espacios de almacenamiento en servidor, deben cumplirse los siguientes requisitos mínimos:  
  
-   El servidor que ejecuta Windows Server Essentials debe adjuntarse a otras unidades físicas (no solo la unidad de arranque), las unidades de disco no deben hospedar los volúmenes y deben tener una capacidad mínima de 10 GB. Se requiere una unidad física para crear un grupo de almacenamiento; se necesita un mínimo de dos unidades físicas para crear un espacio de almacenamiento de reflejo resistente.  
  
-   Un mínimo de dos unidades físicas son necesarios para crear un espacio de almacenamiento con la resistencia a través de paridad o la creación de reflejos bidireccional.  
  
> [!NOTE]
>  Si configuras espacios de almacenamiento en un servidor que ejecuta Windows Server Essentials, no puedes realizar un restablecimiento de fábrica con el **datos limpia** opción. La solución alternativa para este problema es quitar espacios de almacenamiento y, a continuación, realizar un restablecimiento de fábrica con el **datos limpia** opción.  
  
#### <a name="to-create-a-storage-space-in-windows-server-essentials"></a>Para crear un espacio de almacenamiento en Windows Server Essentials  
  
1.  Agrega o conecta todas las unidades que desees agrupar con espacios de almacenamiento al servidor que ejecuta Windows Server Essentials.  
  
2.  En el panel, haz clic en **opciones avanzadas: administrar espacios de almacenamiento**.  
  
3.  Haz clic en **crear un nuevo espacio de almacenamiento y la agrupación**.  
  
4.  Selecciona las unidades que quieras agregar al nuevo espacio de almacenamiento y, a continuación, haz clic en **crear grupo**.  
  
5.  Asigna un nombre y la letra a la unidad y, a continuación, elige una distribución. **Reflejo doble**, **reflejo triple**, y **paridad** puede ayudar a proteger los archivos en el espacio de almacenamiento ante errores de la unidad.  
  
6.  Escribe el tamaño máximo que puede alcanzar el espacio de almacenamiento y, a continuación, haz clic en **crear espacio de almacenamiento**.  
  
 En Windows Server Essentials, puedes crear un espacio de almacenamiento reflejado bidireccional utilizando la crear un asistente de espacio de almacenamiento desde el panel.  
  
#### <a name="to-create-a-storage-space-in-windows-server-essentials"></a>Para crear un espacio de almacenamiento en Windows Server Essentials  
  
1.  Agrega o conecta todas las unidades que desees agrupar con espacios de almacenamiento al servidor que ejecuta Windows Server Essentials.  
  
2.  En el panel, haz clic en **administrar espacios de almacenamiento**. Aparecerá el crear un asistente de espacio de almacenamiento.  
  
3.  Sigue las instrucciones para completar al asistente.  
  
 Para obtener información sobre cómo aumentar la capacidad del grupo de almacenamiento, consulte [usar el nuevo disco duro para aumentar la capacidad del grupo de almacenamiento](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4c).  
  
## <a name="see-also"></a>Consulta también  
  
-   [Administrar carpetas del servidor](Manage-Server-Folders-in-Windows-Server-Essentials.md)  
  
-   [Usar Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)  
  
-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)
