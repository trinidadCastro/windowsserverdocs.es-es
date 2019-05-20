---
title: Manage the Local Server and the Server Manager Console
description: Administrador de servidores
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eeb32f65-d588-4ed5-82ba-1ca37f517139
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1f22578cc54a22464fe5d9208731fe681be30481
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832986"
---
# <a name="manage-the-local-server-and-the-server-manager-console"></a>Manage the Local Server and the Server Manager Console

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En Windows Server, el administrador del servidor le permite administrar tanto el servidor local (si está ejecutando el administrador del servidor en Windows Server y no en un sistema operativo cliente basado en Windows) y los servidores remotos que ejecutan Windows Server 2008 y versiones más recientes de la Windows Sistema operativo de servidor.

El **servidor Local** página en el administrador del servidor muestra los datos de contador de propiedades, eventos, servicios y rendimiento de servidor y resultados de Best Practices Analyzer (BPA) para el servidor local. Los iconos de eventos, servicios, BPA y rendimiento funcionan igual que en las páginas de grupos de servidores y roles. Para obtener más información sobre cómo configurar los datos que se muestran en estos iconos, vea [Visualización y configuración de datos de servicios, eventos y rendimiento](view-and-configure-performance-event-and-service-data.md) y [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](run-best-practices-analyzer-scans-and-manage-scan-results.md).

Comandos de menú y la configuración de las barras de título de la consola de administrador del servidor se aplica globalmente a todos los servidores en el grupo de servidores y le permite usar el administrador del servidor para administrar el grupo de todo el servidor.

En este tema se incluyen las siguientes secciones.

-   [Apague el servidor local](#BKMK_shutdown)

-   [Configurar las propiedades del administrador del servidor](#BKMK_props)

-   [Administración de la consola de administrador del servidor](#BKMK_managesm)

-   [Personalizar las herramientas que se muestran en el menú Herramientas](#BKMK_tools)

-   [Administrar roles en páginas principales de roles](#BKMK_roles)

## <a name="BKMK_shutdown"></a>Apague el servidor local
El **tareas** menú en el servidor local **propiedades** icono le permite iniciar una sesión de Windows PowerShell en el servidor local, abra el **equipo administración** mmc complemento, o abierto complementos MMC para roles o características instalados en el servidor local. También puede cerrar el servidor local mediante el comando **Cerrar servidor local** de este menú **Tareas** . El comando **Cerrar servidor local** también está disponible para el servidor local en el icono **Servidores** de la página **Todos los servidores** o en cualquier página de rol o grupo en la que esté representado el servidor local.

Cerrando el servidor local mediante este método, a diferencia de cerrar Windows Server 2016 desde el **iniciar** pantalla, se abre el **apagar abajo Windows** cuadro de diálogo que le permite especificar los motivos para cerrar en el **Rastreador de eventos de apagado** área.

> [!NOTE]
> Solo los miembros del grupo Administradores pueden cerrar o reiniciar un servidor. Los usuarios estándar no pueden cerrar ni reiniciar un servidor. Al hacer clic en el comando **Cerrar servidor local**, se cierran las sesiones de los usuarios estándar en el servidor. Esto es igual que si un usuario estándar ejecuta el comando de cierre **Alt+F4** desde el escritorio del servidor.

## <a name="BKMK_props"></a>Configurar las propiedades del administrador del servidor
Puede ver o cambiar las siguientes opciones de configuración en el icono **Propiedades** de la página **Servidor local** . Para cambiar un valor de configuración, haga clic en el valor de hipertexto de la configuración.

> [!NOTE]
> Por lo general, las propiedades mostradas en el icono **Propiedades** del servidor local solo se pueden cambiar en el servidor local. No se puede cambiar las propiedades del servidor local desde un equipo remoto mediante el administrador del servidor porque la **propiedades** mosaico solo puede obtener información sobre el equipo local, los equipos remotos no.
> 
> Como muchas de las propiedades se muestran en el **propiedades** mosaico se controlan mediante herramientas que no forman parte del administrador del servidor (Panel de Control, por ejemplo), cambia a **propiedades** valores no son siempre muestra en el **propiedades** icono inmediatamente. De forma predeterminada, los datos del icono **Propiedades** se actualizan cada dos minutos. Para actualizar **propiedades** datos del icono inmediatamente, haga clic en **actualizar** en la barra de direcciones del administrador del servidor.

|Parámetro|Descripción|
|------|--------|
|Nombre del equipo|Muestra el nombre descriptivo del equipo y abre el **las propiedades del sistema** cuadro de diálogo que le permite cambiar el nombre del servidor, pertenencia al dominio y otras configuraciones del sistema como los perfiles de usuario.|
|Dominio (o Grupo de trabajo, si el servidor no está unido a un dominio)|Muestra el dominio o grupo de trabajo al que pertenece el servidor. Se abre el **las propiedades del sistema** cuadro de diálogo que le permite cambiar el nombre del servidor, pertenencia al dominio y otras configuraciones del sistema como los perfiles de usuario.|
|Firewall de Windows|Muestra el estado de Firewall de Windows del servidor local. Abre **Panel de control\Sistema y seguridad\Firewall de Windows**. Para obtener más información sobre la configuración de Firewall de Windows, consulta [Firewall de Windows con seguridad avanzada e IPsec](https://go.microsoft.com/fwlink/?LinkId=253465).|
|Administración remota|Muestra el estado de administración remota del administrador del servidor y Windows PowerShell. Se abre el **configurar la administración remota** cuadro de diálogo. Para obtener más información sobre la administración remota, consulte [configurar la administración remota en el administrador del servidor](configure-remote-management-in-server-manager.md).|
|Escritorio remoto|Muestra si los usuarios se pueden conectar el servidor de forma remota mediante sesiones de Escritorio remoto. Se abre el **remoto** pestaña de la **las propiedades del sistema** cuadro de diálogo.|
|Formación de equipos NIC|Muestra si el servidor local participa o no en la Formación de equipos NIC. Abre el cuadro de diálogo **Formación de equipos NIC** y le permite unir el servidor local a un equipo NIC si lo desea. Para obtener más información sobre la Formación de equipos NIC, consulta las [Notas del producto de Formación de equipos NIC](https://go.microsoft.com/fwlink/?LinkID=253449).|
|Ethernet|Muestra el estado de red del servidor. Abre **Panel de control\Red e Internet\Conexiones de red**.|
|Versión del sistema operativo|Este campo de solo lectura muestra el número de versión del sistema operativo Windows que ejecuta el servidor local.|
|Información de hardware|Este campo de solo lectura muestra el fabricante y el número y nombre de modelo del hardware del servidor.|
|Últimas actualizaciones instaladas|Muestra el día y la hora en que se instalaron por última vez actualizaciones de Windows. Abre **Panel de control\Sistema y seguridad\Windows Update**.|
|Windows Update|Muestra la configuración de Windows Update del servidor local. Abre **Panel de control\Sistema y seguridad\Windows Update**.|
|Últimas actualizaciones buscadas|Muestra el día y la hora en que el servidor comprobó por última vez si había actualizaciones de Windows disponibles. Abre **Panel de control\Sistema y seguridad\Windows Update**.|
|Informe de errores de Windows|Muestra el estado de participación en el Informe de errores de Windows. Abre el cuadro de diálogo **Configuración de Informe de errores de Windows** . Para obtener más información sobre el Informe de errores de Windows, sus ventajas, declaraciones de privacidad y configuración de participación, consulta [Informe de errores de Windows](https://go.microsoft.com/fwlink/?LinkID=245991).|
|Programa para la mejora de la experiencia del usuario|Muestra el estado de participación en el Programa para la mejora de la experiencia del usuario de Windows. Abre el cuadro de diálogo **Configuración del Programa para la mejora de la experiencia del usuario** . Para obtener más información sobre el Programa para la mejora de la experiencia del usuario de Windows, sus ventajas y configuración de participación, consulta [Programa para la mejora de la experiencia del usuario de Windows](https://go.microsoft.com/fwlink/?LinkID=245992).|
|Configuración de seguridad mejorada de Internet Explorer|Muestra si la Configuración de seguridad mejorada de Internet Explorer (también conocida como IE Hardening o IE ESC) está activada o desactivada. Abre el cuadro de diálogo **Configuración de seguridad mejorada de Internet Explorer**. La Configuración de seguridad mejorada de Internet Explorer es una medida de seguridad para servidores que impide la apertura de páginas web en Internet Explorer. Para obtener más información acerca de la configuración de seguridad mejorada de Internet Explorer, sus ventajas y configuración, consulte [Internet Explorer: Configuración de seguridad mejorada](https://go.microsoft.com/fwlink/?LinkId=253461).|
|Zona horaria|Muestra la zona horaria del servidor local. Se abre el **fecha y hora** cuadro de diálogo.|
|Id. del producto|Muestra el número de identificación de producto y el estado de activación de Windows (si se ha activado Windows) del sistema operativo Windows Server 2016. No es el mismo número que la clave del producto de Windows. Abre el cuadro de diálogo **Activación de Windows** .|
|Procesadores|Este campo de solo lectura muestra información acerca de los procesadores del servidor local de la velocidad, nombre del modelo y fabricante.|
|Memoria instalada (RAM)|Este campo de solo lectura muestra la cantidad de RAM disponible, en gigabytes.|
|Espacio total en disco|Este campo de solo lectura muestra la cantidad de espacio en disco disponible, en gigabytes.|

## <a name="BKMK_managesm"></a>Administración de la consola de administrador del servidor
Configuración global que se aplica a toda la consola de administrador del servidor y a todos los servidores remotos que se han agregado al grupo de servidores de administrador del servidor, se encuentra en las barras de título en la parte superior de la ventana de consola de administrador del servidor.

### <a name="add-servers-to-server-manager"></a>Agregar servidores al administrador del servidor
El comando que abre el **agregar servidores** cuadro de diálogo y le permite agregar servidores físicos o virtuales remotos al grupo de servidores de administrador del servidor, se encuentra en la **administrar** menú de la consola de administrador del servidor. Para obtener información detallada acerca de cómo agregar servidores, consulte [agregar servidores al administrador del servidor](add-servers-to-server-manager.md).

### <a name="refresh-data-that-is-displayed-in-server-manager"></a>Actualización de los datos mostrados en el Administrador del servidor
Puede configurar el intervalo de actualización de datos que se muestra en el administrador del servidor en el **propiedades del administrador del servidor** cuadro de diálogo, que puede abrir desde el **administrar** menú.

##### <a name="to-configure-the-refresh-interval-in-server-manager"></a>Para configurar el intervalo de actualización en el Administrador del servidor

1.  En el **administrar** menú en la consola de administrador del servidor, haga clic en **propiedades del administrador del servidor**.

2.  En el **propiedades del administrador del servidor** diálogo cuadro, especifique un período de tiempo en minutos, durante el tiempo transcurrido que desee entre las actualizaciones de los datos que se muestran en el administrador del servidor. El valor predeterminado es 10 minutos. Haga clic en Aceptar cuando haya acabado.

#### <a name="refresh-limitations"></a>Limitaciones de la actualización
La actualización se aplica globalmente a los datos de todos los servidores que haya agregado al grupo de servidores de administrador del servidor. No puede actualizar los datos ni configurar intervalos de actualización distintos para servidores, roles o grupos individuales.

Cuando los servidores que están en un clúster se agregan al administrador del servidor, ya sean equipos físicos o máquinas virtuales, la primera actualización de datos puede producir errores o mostrar datos solo para el servidor host para objetos en clúster. Las actualizaciones siguientes ya muestran datos precisos de los servidores físicos o virtuales de un clúster de servidores.

Datos que se muestran en las páginas principales de rol en el administrador del servidor para servicios de escritorio remoto, la dirección IP de administración y servicios de almacenamiento y archivos no se actualizan automáticamente. Actualizar los datos que se muestran en estas páginas manualmente, presionando **F5** o haciendo clic en **actualizar** en el encabezado de consola de administrador del servidor mientras estás en esas páginas.

### <a name="add-or-remove-roles-or-features"></a>Agregar o quitar roles o características
Los comandos que abren el agregar Roles y características Asistente y quitar Roles y características Asistente y permiten agregar o quitar roles, servicios de rol y características a servidores en los servidores del grupo están en el **administrar** menú del administrador del servidor la consola y el **tareas** menú de la **Roles y características** icono en las páginas de rol o grupo. Para obtener información detallada acerca de cómo agregar o quitar roles o características, consulte [instalación o desinstalación de Roles, servicios de rol o características](install-or-uninstall-roles-role-services-or-features.md).

En el administrador del servidor, roles y características de datos se muestran en el idioma base del sistema, también denominado el idioma predeterminado de la interfaz gráfica de usuario o el idioma seleccionado durante la instalación del sistema operativo.

### <a name="create-server-groups"></a>crear grupos de servidores
El comando que abre el **crear grupo de servidores** cuadro de diálogo y le permite crear grupos de servidores personalizados, que se encuentra en la **administrar** menú de la consola de administrador del servidor. Para obtener información detallada acerca de cómo crear grupos de servidores, consulte [crear y administrar grupos de servidores](create-and-manage-server-groups.md).

### <a name="prevent-server-manager-from-opening-automatically-at-logon"></a>Impedir que el Administrador del servidor se abra automáticamente al iniciar sesión
El **no iniciar el administrador del servidor automáticamente al inicio de sesión** casilla de verificación en la **propiedades del administrador del servidor** cuadro de diálogo controla si el administrador del servidor se abre automáticamente al inicio de sesión para los miembros de la Grupo de administradores en un servidor local. Esta configuración no afecta el comportamiento de administrador del servidor cuando se ejecuta en Windows 10 como parte de herramientas de administración remota del servidor. Para obtener más información acerca de cómo configurar esta opción, consulte [administrador del servidor](server-manager.md).

### <a name="zoom-in-or-out"></a>Acercar o alejar
Para acercar o alejar la vista de la consola de administrador del servidor, puede usar el **Zoom** comandos en el **vista** menús o presione **CTRL+más (+)** para acercar y **Ctrl + signo menos (-)** para alejar.

## <a name="BKMK_tools"></a>Personalizar las herramientas que se muestran en el menú Herramientas
El **herramientas** menú Administrador del servidor incluye vínculos a los accesos directos en el **herramientas administrativas** carpeta **Panel de Control/sistema y seguridad**. El **herramientas administrativas** carpeta contiene una lista de accesos directos o archivos LNK para las herramientas de administración disponibles, como complementos mmc. Rellena el administrador del servidor el **herramientas** menú con vínculos a dichos accesos directos y copia la estructura de carpetas de la **herramientas administrativas** carpeta a la **herramientas** menú. De forma predeterminada, las herramientas de la carpeta Herramientas administrativas están organizadas en una lista plana, ordenadas por tipo y nombre. En el administrador del servidor**herramientas** menú, los elementos se ordenan solo por nombre, no por tipo.

Para personalizar el menú **Herramientas** , copie los accesos directos a herramientas o scripts que desee usar en la carpeta **Herramientas administrativas** . También puede organizar los accesos directos en carpetas, con lo que se crean menús en cascada en el menú **Herramientas** . Además, si desea restringir el acceso a las herramientas personalizadas en el **herramientas** menú, puede establecer los derechos de acceso de usuario en las carpetas de herramientas personalizados en Herramientas administrativas o directamente en los archivos de herramientas o scripts originales.

Se recomienda no reorganizar las herramientas administrativas y del sistema y las herramientas de administración asociadas con roles y características instalados en el servidor local. Si se mueven las herramientas de administración de roles y características, se puede impedir la correcta desinstalación de estas herramientas de administración, cuando sea necesario. Después de desinstalar un rol o característica, en el menú **Herramientas** podría quedar un vínculo no funcional a una herramienta cuyo acceso directo se haya movido. Si reinstala el rol, se crea un vínculo duplicado a la misma herramienta en el menú **Herramientas**, pero uno de los vínculos no funcionará.

No obstante, las herramientas de roles y características que se instalan como parte de las Herramientas de administración remota del servidor en un equipo de Windows basado en el cliente, se pueden organizar en carpetas personalizadas. Desinstalar el rol o característica primario no tiene ningún efecto en los accesos directos a herramientas están disponibles en un equipo remoto que ejecuta Windows 10.

El siguiente procedimiento describe cómo crear una carpeta de ejemplo denominada *MisHerramientas*, y mover accesos directos de dos secuencias de comandos de Windows PowerShell en la carpeta que después estarán accesibles desde el menú Herramientas del administrador del servidor.

#### <a name="to-customize-the-tools-menu-by-adding-shortcuts-in-administrative-tools"></a>Para personalizar el menú Herramientas mediante la adición de accesos directos en Herramientas administrativas

1.  Cree una carpeta nueva denominada *MisHerramientas* en una ubicación adecuada.

    > [!NOTE]
    > A causa de los restrictivos derechos de acceso en la carpeta **Herramientas administrativas** , no puede crear una carpeta directamente en la carpeta **Herramientas administrativas** ; debe crear una carpeta en otra ubicación (como el escritorio) y luego copiar la nueva carpeta en la carpeta **Herramientas administrativas** .

2.  mover o copiar *MisHerramientas* a **Panel de Control/sistema y seguridad/herramientas administrativas**. De forma predeterminada, debe pertenecer al grupo Administradores del equipo para realizar cambios en la carpeta **Herramientas administrativas** .

3.  Si no es necesario restringir los derechos de acceso de usuario a los accesos directos de la herramienta personalizada, vaya al paso 6. De lo contrario, haga clic con el botón secundario en el archivo de la herramienta (o en la carpeta *MisHerramientas*) y, a continuación, haga clic en **Propiedades**.

4.  En el **seguridad** ficha del archivo **propiedades** cuadro de diálogo, haga clic en **editar**.

5.  para los usuarios para los que desea restringir el acceso a la herramienta, desactive las casillas de verificación para **lectura & ejecutar**, **lectura**, y **escribir** permisos. El acceso directo a la herramienta hereda estos permisos en la carpeta **Herramientas administrativas**.

    Si edita los derechos de acceso para un usuario mientras el usuario está utilizando el administrador del servidor (o abrir mientras el administrador del servidor es), entonces no se muestran los cambios en el **herramientas** menú hasta que el usuario reinicie el administrador del servidor.

    > [!NOTE]
    > Si restringe el acceso a una carpeta completa que copiaste en Herramientas administrativas, los usuarios restringidos pueden ver ni la carpeta ni su contenido en el administrador del servidor**herramientas** menú.
    > 
    > Editar permisos para la carpeta en la **herramientas administrativas** carpeta. Como los archivos ocultos y carpetas en Herramientas administrativas siempre se muestran en el administrador del servidor**herramientas** menú, no use la **Hidden** en un archivo o carpeta **propiedades** cuadro de diálogo para restringir el acceso de usuario a los accesos directos de la herramienta personalizada.
    > 
    > Los permisos **Denegar** siempre sobrescriben los permisos **Permitir**.

6.  Haga clic en la herramienta original, script o archivo ejecutable para el que desea agregar entradas en el **herramientas** menú y, a continuación, haga clic en **crear acceso directo**.

7.  Mueva el acceso directo a la *MisHerramientas* carpeta en Herramientas administrativas.

8.  Actualice o reinicie el administrador del servidor, si es necesario, para ver el acceso directo de herramienta personalizada en el **herramientas** menú.

## <a name="BKMK_roles"></a>Administrar roles en páginas principales de roles
Después de agregar servidores al grupo de servidores de administrador del servidor y el administrador del servidor recopila los datos de inventario sobre los servidores del grupo, el administrador del servidor agrega páginas al panel de navegación para los roles detectados en servidores administrados. El icono **Servidores** de las páginas de rol muestra los servidores administrados que ejecutan el rol. De forma predeterminada, los iconos **Eventos**, **Analizador de procedimientos recomendados**, **Servicios**y **Rendimiento** muestran datos de todos los servidores que ejecutan el rol y si seleccionas servidores específicos en el icono **Servidores** , se limita el ámbito de los eventos, servicios, contadores de rendimiento y resultados de BPA solo al servidor seleccionado. Las herramientas de administración están disponibles normalmente en la consola de administrador del servidor **herramientas** menú después de haberse instalado o detectado en un servidor administrado un rol o característica. También puede hacer clic con el botón secundario en las entradas de servidor del icono **Servidores** para un rol o grupo y, a continuación, iniciar la herramienta de administración que desee usar.

En Windows Server 2016, los siguientes roles y características tienen herramientas de administración que se integran en la consola de administrador del servidor como páginas.

-   **Servicios de archivos y almacenamiento.** Las páginas de Servicios de archivos y almacenamiento incluyen iconos y comandos personalizados para administrar volúmenes, recursos compartidos, discos virtuales iSCSI y grupos de almacenamiento. Cuando abra el archivo y abre la página principal del rol Servicios de almacenamiento en Administrador del servidor, un panel retráctil muestra las páginas de administración personalizadas de servicios de archivos y almacenamiento. Para obtener más información sobre la implementación y administración de los Servicios de archivos y almacenamiento, consulta [Servicios de archivos y almacenamiento](https://go.microsoft.com/fwlink/p/?LinkId=241530).

-   **Servicios de Escritorio remoto.** Las páginas de Servicios de Escritorio remoto incluyen iconos y comandos personalizados para administrar sesiones, licencias, puertas de enlace y escritorios virtuales. Para obtener más información sobre la implementación y administración de servicios de escritorio remoto, consulte [servicios de escritorio remoto (rdS)](https://go.microsoft.com/fwlink/p/?LinkId=241532).

-   **Dirección IP (IPAM) de administración.** La página del rol IPAM incluye un icono de **Página principal** personalizado que contiene vínculos a tareas comunes de configuración y administración de IPAM, incluido un asistente para aprovisionar un servidor IPAM. La página principal de IPAM también incluye iconos para ver la red administrada, el resumen de configuración y las tareas programadas.

    Existen algunas limitaciones en el administrador del servidor de administración de IPAM. A diferencia de las páginas de rol y grupo habituales, IPAM no tiene los iconos **Servidores**, **Eventos**, **Rendimiento**, **Analizador de procedimientos recomendados** o **Servicios**. No hay ningún modelo del analizador de procedimientos recomendados disponible para IPAM; No se admiten los exámenes del analizador de procedimientos recomendados en IPAM. Para obtener acceso a los servidores del grupo de servidores que ejecutan IPAM, cree un grupo personalizado de esos servidores y obtenga acceso a la lista de servidores desde el icono **Servidores** de la página del grupo personalizado. Como alternativa, puede obtener acceso a los servidores IPAM desde el icono **Servidores** de la página del grupo **Todos los servidores**.

    Las miniaturas del panel también muestran filas limitadas para IPAM, en comparación de las miniaturas de otros roles y grupos. Si hace clic en las filas de miniaturas de IPAM, puede ver los eventos, datos de rendimiento y alertas de estado de administración de los servidores que ejecutan IPAM. Los servicios relacionados con IPAM se pueden administrar desde páginas de grupos de servidores que contiene servidores IPAM, como la página del grupo **Todos los servidores** .

    Para obtener más información sobre la implementación y administración de IPAM, consulte [dirección IP (IPAM) de administración](https://go.microsoft.com/fwlink/p/?LinkId=241533).

## <a name="see-also"></a>Vea también
[El administrador del servidor](server-manager.md)
[agregar servidores al administrador del servidor](add-servers-to-server-manager.md)
[crear y administrar grupos de servidores](create-and-manage-server-groups.md)
[ver y configurar Rendimiento, eventos y datos del servicio](view-and-configure-performance-event-and-service-data.md)
[File and Storage Services](https://go.microsoft.com/fwlink/p/?LinkId=241530)
[servicios de escritorio remoto (rdS)](https://go.microsoft.com/fwlink/p/?LinkId=241532) 
 [ Dirección IP (IPAM) de administración](https://go.microsoft.com/fwlink/p/?LinkId=241533)



