---
title: Administrar el almacenamiento de servidor en Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 1836682e-c7bb-4dd5-a2b5-6ff032693574
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 2cd8ac4e93027e3ab88042d0bca465f3fea67f26
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89626108"
---
# <a name="manage-server-storage-in-windows-server-essentials"></a>Administrar el almacenamiento de servidor en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

 Windows Server Essentials permite administrar todo el almacenamiento de servidor (incluidas las unidades de disco duro y los espacios de almacenamiento) desde las páginas **Unidades de disco duro** en la pestaña **Almacenamiento** del panel.

 Las secciones siguientes proporcionan información que le ayudará a aumentar el almacenamiento de servidor, comprender y usar los espacios de almacenamiento, y administrar las unidades de disco duro:

-   [Administrar unidades de disco duro mediante el panel](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_1)

-   [Aumentar el almacenamiento del servidor](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_2)

-   [Realizar comprobaciones y reparaciones en las unidades de disco duro](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_Check)

-   [Formatear unidades de disco duro](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_3)

-   [Agregar una nueva unidad de disco duro](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4)

-   [Introducción a los espacios de almacenamiento](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_5)

-   [Crear un espacio de almacenamiento](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_6)

##  <a name="manage-hard-drives-using-the-dashboard"></a><a name="BKMK_1"></a> Administrar unidades de disco duro mediante el panel
 Windows Server Essentials permite administrar todas las unidades de disco duro que están conectadas al servidor mediante el panel. En la pestaña **Almacenamiento** del panel, **Unidades de disco duro** muestra todas las unidades de disco duro que están disponibles en el servidor para almacenar datos y copias de seguridad del servidor. El servidor supervisa el espacio disponible en cada unidad de disco duro y mostrará una alerta si queda poco espacio en la unidad de disco duro. La pestaña **Unidades de disco duro** muestra la siguiente información:

- El nombre de cada unidad de disco duro.

- La capacidad de cada unidad de disco duro.

- La cantidad de espacio usado en cada unidad de disco duro.

- La cantidad de espacio libre en cada unidad de disco duro.

- El estado de cada unidad de disco duro; un estado en blanco significa que la unidad de disco duro está funcionando correctamente.

- El panel de detalles, que muestra toda la información de la pila de almacenamiento (para el grupo de almacenamiento, el espacio de almacenamiento y la unidad de disco duro) si la unidad de disco duro seleccionada se encuentra en un espacio de almacenamiento (en lugar de un disco físico).

  En la tabla siguiente se enumeran las tareas de administración de la unidad de disco duro que están disponibles en el panel y sus descripciones. Algunas de las tareas se muestran únicamente cuando se selecciona una unidad de disco duro.

### <a name="available-hard-drive-management-tasks"></a>Tareas administrativas de la unidad de disco duro disponibles

|Nombre de tarea|Descripción|
|---------------|-----------------|
|**Ver las propiedades de la unidad de disco duro**|Se abrirá la página _Propiedades_**NombreDeDiscoDuro** . Esta tarea se muestra cuando se selecciona la unidad de disco duro. La pestaña **General** de la página Propiedades de *NombreDeUnidadDeDiscoDuro* incluye las siguientes tareas adicionales:<br /><br /> -   **Limpieza de unidad**: permite limpiar los archivos no usados en el disco duro (esta tarea solo está disponible en Windows Server Essentials).<br />-   **Comprobar y reparar**: comprueba el disco duro en busca de errores del sistema de archivos e intenta reparar automáticamente los errores detectados.<br /><br /> La pestaña **Instantáneas** de la página Propiedades de _Propiedades_**NombreDeDiscoDuro** permite habilitar las instantáneas. Esta pestaña también muestra la hora en la que está programada la ejecución de la próxima instantánea.|
|**Administrar los espacios de almacenamiento**|**Nota:** En Windows Server Essentials, esta tarea solo se muestra cuando hay un espacio de almacenamiento existente.<br /><br /> Abre el panel de control de los **Espacios de almacenamiento**, desde el que puede crear y administrar grupos de almacenamiento y espacios de almacenamiento.|
|**Crear un espacio de almacenamiento**|Abre el Asistente para crear un espacio de almacenamiento, lo que permite usar una o más unidades de disco duro para aumentar la capacidad de un grupo de almacenamiento.|
|**Aumentar la capacidad del grupo de almacenamiento**|**Nota:** Esta tarea solo está visible si la unidad de disco duro seleccionada se encuentra en un espacio de almacenamiento.<br /><br /> Abre el Asistente para aumentar la capacidad de un grupo de almacenamiento, lo que permite usar una o más unidades de disco duro para aumentar la capacidad de un grupo de almacenamiento.|

##  <a name="increase-storage-on-the-server"></a><a name="BKMK_2"></a> Aumentar el almacenamiento en el servidor
 Para aumentar el almacenamiento del servidor, puede agregar un disco duro interno adicional al servidor. Para agregar el disco duro interno adicional, debe apagar el servidor, agregar el disco duro interno y reiniciar el servidor. No es necesario apagar el servidor si el disco duro está conectado a la controladora SCSI. En ese caso, el disco duro puede estar conectado mientras se está ejecutando el servidor.

 En función de si el disco duro que se va a agregar está formateado, realice una de las acciones siguientes:

-   **Con formato** Si el disco duro interno está formateado con NTFS o ReFS, el servidor le asigna una letra de unidad y el disco duro aparece en la pestaña **unidades de disco duro** . Ahora puede crear o trasladar las carpetas del servidor a la nueva unidad de disco duro.

-   **Sin formatear** Si el disco duro interno no está formateado, aparece la alerta siguiente: Una o varias unidades de disco duro sin formatear están conectadas al servidor. Use el siguiente procedimiento para formatear el disco duro.

#### <a name="to-format-the-hard-disk"></a>Para formatear el disco duro

1.  Abra el Panel.

2.  En el panel de navegación, haga clic en el icono de alerta para iniciar el **visor de alertas** en Windows Server Essentials o en la pestaña **seguimiento de estado** en Windows Server Essentials.

3.  En el **Visor de alertas** o en la pestaña **Supervisión de estado**, haga clic en la alerta y, a continuación, haga clic en **Solucionar este problema** en el panel de tareas.

4.  Siga las instrucciones que se indican a continuación para completar el Asistente para agregar nueva unidad de disco duro.

###  <a name="to-clean-up-the-hard-drive"></a><a name="BKMK_Clean"></a> Para limpiar el disco duro

1.  Abra el Panel.

2.  En el panel de navegación, haga clic en **Almacenamiento** y, a continuación, haga clic en **Unidades de disco duro**.

3.  En la sección **Unidades de disco duro**, seleccione la letra de unidad que se asignó al disco duro recién agregado y, en el panel de tareas, haga clic en **Ver las propiedades de la unidad de disco duro**.

4.  En **<\> propiedades de driveletter**, en la pestaña **General** , haga clic en **limpieza de unidad**.

##  <a name="perform-checks-and-repairs-on-hard-drives"></a><a name="BKMK_Check"></a> Realizar comprobaciones y reparaciones en las unidades de disco duro
 El proceso de comprobación y reparación de las unidades de disco duro comprueba el estado del sistema de archivos almacenado en las unidades de disco duro. Ejecuta un proceso **chkdsk** en el volumen en el que están almacenados los archivos de copia de seguridad. El siguiente problema de la alerta se puede resolver ejecutando una comprobación y reparación en las unidades de disco duro:

-   Deben comprobarse una o más unidades de disco duro de la copia de seguridad del servidor.

#### <a name="to-check-and-repair-hard-drives"></a>Para comprobar y reparar las unidades de disco duro

1.  Abra el Panel.

2.  Haga clic en **Carpetas de servidor y unidades de disco duro** y, a continuación, haga clic en **Unidades de disco duro**.

3.  Seleccione la unidad de disco duro que muestra el error y, a continuación, seleccione **Ver las propiedades de la unidad de disco duro**.

4.  En la pestaña **Comprobar y reparar**, haga clic en **Comprobar y reparar**.

##  <a name="format-hard-drives"></a><a name="BKMK_3"></a> Formatear unidades de disco duro
 Cuando se detecta una unidad de disco duro interna no formateada en el servidor, una alerta de estado guía al usuario a lo largo del proceso de formateo. El asistente para Agregar nueva unidad de disco duro le guiará durante el proceso de formatear la unidad de disco duro y le permitirá configurar la unidad de disco duro de una de las maneras siguientes:

1.  Formatear la unidad de disco duro y crear automáticamente una unidad en ella. Si elige esta opción, cuando se complete el asistente, se crea una unidad de disco duro lógico formateada con el sistema de archivos NTFS.

2.  Formatear la unidad de disco duro y configurarla como copia de seguridad de servidor. Si elige esta opción, se inicia el Asistente para configurar la copia de seguridad del servidor, que le guiará a lo largo del proceso de configurar la copia de seguridad del servidor.

3.  Si no existe un espacio de almacenamiento, use la nueva unidad de disco duro para crear un espacio de almacenamiento. Debe tener al menos dos unidades de disco duro para crear un espacio de almacenamiento.

4.  Si ya existe un espacio de almacenamiento, use la nueva unidad de disco duro para aumentar la capacidad de un grupo de almacenamiento. Esta opción solo se muestra si hay un espacio de almacenamiento creado en el servidor. Si elige esta opción, el asistente agregará esta unidad de disco duro al grupo de almacenamiento.

##  <a name="add-a-new-hard-drive"></a><a name="BKMK_4"></a> Agregar una nueva unidad de disco duro
 Al conectar una nueva unidad de disco duro en un servidor que ejecuta Windows Server Essentials, puede hacer lo siguiente:

-   [Usar la nueva unidad de disco duro para almacenar las carpetas de servidor](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4a)

-   [Usar la nueva unidad de disco duro para almacenar copias de seguridad del servidor](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4b)

-   [Usar la nueva unidad de disco duro para aumentar la capacidad del grupo de almacenamiento](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4c)

###  <a name="use-the-new-hard-drive-to-store-server-folders"></a><a name="BKMK_4a"></a> Usar la nueva unidad de disco duro para almacenar las carpetas de servidor
 Para usar la nueva unidad de disco duro para almacenar las carpetas de servidor, puede agregar una nueva carpeta de servidor a la unidad de disco duro o mover una carpeta de servidor existente a la unidad de disco duro.

##### <a name="to-store-server-folders"></a>Para almacenar las carpetas de servidor

1. Abra el Panel.

2. Haga clic en la pestaña **ALMACENAMIENTO** y, después, en **Carpetas de servidor**.

3. En el panel **Tareas de carpetas de servidor**, realice una de las siguientes acciones:

   1.  Para agregar una carpeta de servidor, haga clic en **Agregar una carpeta**.

   2.  Para mover una carpeta de servidor, seleccione la carpeta que desea mover a la nueva unidad de disco duro y, a continuación, haga clic en **Mover una carpeta**.

   > [!NOTE]
   >  Si busca la unidad de disco duro y la selecciona como ubicación de la carpeta del servidor sin crear una carpeta, aparece el siguiente mensaje de error: **no se puede Agregar un directorio raíz (por ejemplo, C: \\ , D: \\ ) como una carpeta de servidor. Cree una nueva carpeta o seleccione una existente en el directorio raíz e inténtelo de nuevo**. Para resolver este error, cree una carpeta nueva en la unidad de disco duro recién agregada y, a continuación, seleccione la nueva carpeta como ubicación para almacenar las carpetas de servidor.

4. Siga las instrucciones para finalizar el asistente.

   Para obtener más información sobre cómo mover las carpetas de servidor, consulte [Add or move a server folder](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5).

###  <a name="use-the-new-hard-drive-to-store-server-backups"></a><a name="BKMK_4b"></a> Usar la nueva unidad de disco duro para almacenar copias de seguridad del servidor
 Puede usar la unidad de disco duro recién agregada para almacenar copias de seguridad del servidor.

##### <a name="to-store-server-backups"></a>Para almacenar las copias de seguridad del servidor

1. Abra el Panel.

2. Haga clic en la pestaña **Dispositivos**, seleccione el servidor en el panel de lista y, a continuación, desde el panel de tareas, realice una de las acciones siguientes:

   1. Si la copia de seguridad del servidor no está configurada en el servidor, haga clic en **Configurar copia de seguridad del servidor**.

   2. Si la copia de seguridad del servidor está configurada en el servidor, haga clic en **Personalizar la copia de seguridad del servidor**.

      Aparece el Asistente para configurar la copia de seguridad del servidor.

3. En la página **Seleccionar el destino de la copia de seguridad**, seleccione la nueva unidad de disco duro como destino de la copia de seguridad.

4. Siga las instrucciones para finalizar el asistente.

###  <a name="use-the-new-hard-drive-to-increase-storage-pool-capacity"></a><a name="BKMK_4c"></a> Usar la nueva unidad de disco duro para aumentar la capacidad del grupo de almacenamiento
 Cuando la capacidad del grupo de almacenamiento sea baja, recibirá una alerta que indica que puede aumentar la capacidad del grupo de almacenamiento agregando una nueva unidad de disco duro al grupo de almacenamiento mediante el Asistente para aumentar la capacidad de un grupo de almacenamiento.

> [!NOTE]
>  Puede realizar este procedimiento solo si creó un grupo de almacenamiento en el servidor.

##### <a name="to-increase-storage-pool-capacity"></a>Para aumentar la capacidad del grupo de almacenamiento

1.  Abra el Panel.

2.  Haga clic en la pestaña **Almacenamiento** y, después, en **Unidades de disco duro**.

3.  Seleccione la unidad que tiene una capacidad baja.

4.  En el panel de tareas, seleccione **Aumentar la capacidad del grupo de almacenamiento**. Aparece el Asistente para aumentar la capacidad de un grupo de almacenamiento.

5.  Siga las instrucciones para finalizar el asistente.

##  <a name="storage-spaces-overview"></a><a name="BKMK_5"></a> Introducción a los espacios de almacenamiento
 Los espacios de almacenamiento permiten agrupan discos en un grupo de almacenamiento. A continuación, puede usar la capacidad del grupo para crear espacios de almacenamiento. Los espacios de almacenamiento son unidades virtuales que aparecen en la pestaña **Unidades de disco duro** del panel. Puede usar espacios de almacenamiento como cualquier otra unidad, por lo que es fácil trabajar con archivos en ellos. Cuando empieza a escasear la capacidad del grupo, puede crear espacios de almacenamiento grandes y agregar más unidades al grupo de almacenamiento. Si tiene dos o más discos en el bloque de almacenamiento, puede crear espacios de almacenamiento con un reflejo doble que no se verá afectado por un error en la unidad? o incluso por un error de dos unidades. Si crea un espacio de almacenamiento de reflejo triple.

 Para crear un espacio de almacenamiento, lo único que necesita es una o más unidades adicionales, además la unidad donde está instalado Windows. Estas unidades pueden ser unidades de disco duro internas o externas, o unidades de estado sólido. Puede usar diversos tipos de unidades con los espacios de almacenamiento, incluidas las unidades USB, SATA y SAS.

> [!NOTE]
>  Si configura los espacios de almacenamiento en un servidor que ejecuta Windows Server Essentials, no puede realizar un restablecimiento de fábrica con la opción **limpiar datos** . La solución de este problema consiste en quitar en primer lugar los espacios de almacenamiento y, a continuación, realizar un restablecimiento de fábrica con la opción **Limpiar datos**.

 Para obtener más información sobre los espacios de almacenamiento, consulte [Preguntas más frecuentes sobre los espacios de almacenamiento](/windows-server/storage/storage-spaces/storage-spaces-direct-faq).

##  <a name="create-a-storage-space"></a><a name="BKMK_6"></a> Crear un espacio de almacenamiento
 Para empezar a trabajar con los espacios de almacenamiento en el servidor, deben cumplirse los siguientes requisitos mínimos:

-   El servidor que ejecuta Windows Server Essentials debe estar adjuntado a las unidades físicas adicionales (no solo a la unidad de arranque), las unidades no deben alojar volúmenes y deben tener una capacidad mínima de 10 GB. Se necesita una unidad física para crear un grupo de almacenamiento; se necesita un mínimo de dos unidades físicas para crear un espacio de almacenamiento de reflejo resistente.

-   Se necesita un mínimo de dos unidades físicas para crear un espacio de almacenamiento con resistencia mediante paridad o reflejo doble.

> [!NOTE]
>  Si configura los espacios de almacenamiento en un servidor que ejecuta Windows Server Essentials, no puede realizar un restablecimiento de fábrica con la opción **limpiar datos** . La solución de este problema consiste en quitar en primer lugar los espacios de almacenamiento y, a continuación, realizar un restablecimiento de fábrica con la opción **Limpiar datos**.

#### <a name="to-create-a-storage-space-in-windows-server-essentials"></a>Para crear un espacio de almacenamiento en Windows Server Essentials

1. Agregue o conecte todas las unidades que desea agrupar con los espacios de almacenamiento al servidor que ejecuta Windows Server Essentials.

2. En el panel, haga clic en **Avanzado: Administrar los espacios de almacenamiento**.

3. Haga clic en **Crear un nuevo grupo y espacio de almacenamiento**.

4. Seleccione las unidades que desea agregar al nuevo espacio de almacenamiento y, a continuación, haga clic en **Crear grupo**.

5. Asigne un nombre y una letra a la unidad y, a continuación, elija un diseño. **Reflejo doble**, **Reflejo triple** y **Paridad** pueden ayudar a proteger los archivos del espacio de almacenamiento frente a errores de la unidad.

6. Especifique el tamaño máximo que puede alcanzar el espacio de almacenamiento y, a continuación, haga clic en **Crear espacio de almacenamiento**.

   En Windows Server Essentials, puede crear un espacio de almacenamiento reflejado bidireccional mediante el Asistente para crear un espacio de almacenamiento desde el panel.

#### <a name="to-create-a-storage-space-in-windows-server-essentials"></a>Para crear un espacio de almacenamiento en Windows Server Essentials

1. Agregue o conecte todas las unidades que desea agrupar con los espacios de almacenamiento al servidor que ejecuta Windows Server Essentials.

2. En el panel, haga clic en **Administrar los espacios de almacenamiento**. Aparecerá el Asistente para crear un espacio de almacenamiento.

3. Siga las instrucciones para completar el asistente.

   Para obtener información acerca de cómo aumentar la capacidad del grupo de almacenamiento, consulte [Use the new hard drive to increase storage pool capacity](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4c).

## <a name="additional-references"></a>Referencias adicionales

-   [Administrar carpetas de servidor](Manage-Server-Folders-in-Windows-Server-Essentials.md)

-   [Usar Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)

-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)