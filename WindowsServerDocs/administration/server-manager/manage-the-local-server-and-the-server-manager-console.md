---
title: Manage the Local Server and the Server Manager Console
description: Obtenga información acerca de cómo administrar el servidor local y los servidores remotos que ejecutan Windows Server 2008 y las versiones más recientes del sistema operativo Windows Server.
ms.topic: article
ms.assetid: eeb32f65-d588-4ed5-82ba-1ca37f517139
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 92434d59abf1373ac87f869b99552409cfeb22b9
ms.sourcegitcommit: 605a9b46b74b2c7a9116e631e902467ea02a6e70
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/07/2021
ms.locfileid: "97965697"
---
# <a name="manage-the-local-server-and-the-server-manager-console"></a>Manage the Local Server and the Server Manager Console

>Se aplica a: Windows Server (Canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En Windows Server, Administrador del servidor le permite administrar el servidor local (si ejecuta Administrador del servidor en Windows Server y no en un sistema operativo de cliente basado en Windows) y los servidores remotos que ejecutan Windows Server 2008 y versiones más recientes del sistema operativo Windows Server.

En la página **servidor local** de administrador del servidor se muestran las propiedades del servidor, los eventos, los datos del contador de rendimiento y de servicio, y los resultados del analizador de procedimientos recomendados (BPA) para el servidor local. Los iconos de eventos, servicios, BPA y rendimiento funcionan igual que en las páginas de grupos de servidores y roles. Para obtener más información sobre cómo configurar los datos que se muestran en estos iconos, vea [Visualización y configuración de datos de servicios, eventos y rendimiento](view-and-configure-performance-event-and-service-data.md) y [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](run-best-practices-analyzer-scans-and-manage-scan-results.md).

Los comandos de menú y la configuración de las barras de título de la consola de Administrador del servidor se aplican globalmente a todos los servidores del grupo de servidores y permiten usar Administrador del servidor para administrar todo el grupo de servidores.

En este tema se incluyen las siguientes secciones.

-   [Cerrar el servidor local](#BKMK_shutdown)

-   [Configurar las propiedades del Administrador del servidor](#BKMK_props)

-   [Administrar la consola del Administrador del servidor](#BKMK_managesm)

-   [Personalizar las herramientas mostradas en el menú Herramientas](#BKMK_tools)

-   [Administrar roles en páginas principales de rol](#BKMK_roles)

## <a name="shut-down-the-local-server"></a><a name=BKMK_shutdown></a>Cerrar el servidor local
El menú **tareas** del icono **propiedades** del servidor local permite iniciar una sesión de Windows PowerShell en el servidor local, abrir el complemento MMC **Administración de equipos** o abrir complementos MMC para roles o características instalados en el servidor local. También puede cerrar el servidor local mediante el comando **Cerrar servidor local** de este menú **Tareas**. El comando **Cerrar servidor local** también está disponible para el servidor local en el icono **Servidores** de la página **Todos los servidores** o en cualquier página de rol o grupo en la que esté representado el servidor local.

Cerrar el servidor local mediante este método, a diferencia de cerrar Windows Server 2016 desde la pantalla **Inicio** , abre el cuadro de diálogo **cerrar Windows** , que permite especificar los motivos para el apagado en el área **rastreador de eventos de apagado** .

> [!NOTE]
> Solo los miembros del grupo Administradores pueden cerrar o reiniciar un servidor. Los usuarios estándar no pueden cerrar ni reiniciar un servidor. Al hacer clic en el comando **Cerrar servidor local**, se cierran las sesiones de los usuarios estándar en el servidor. Esto es igual que si un usuario estándar ejecuta el comando de cierre **Alt+F4** desde el escritorio del servidor.

## <a name="configure-server-manager-properties"></a><a name=BKMK_props></a>Configurar las propiedades del Administrador del servidor
Puede ver o cambiar las siguientes opciones de configuración en el icono **Propiedades** de la página **Servidor local**. Para cambiar el valor de una configuración, haga clic en el valor de hipertexto de la configuración.

> [!NOTE]
> Por lo general, las propiedades mostradas en el icono **Propiedades** del servidor local solo se pueden cambiar en el servidor local. No se pueden cambiar las propiedades del servidor local desde un equipo remoto mediante Administrador del servidor porque el icono **propiedades** solo puede obtener información acerca del equipo local, no de los equipos remotos.
>
> Dado que muchas de las propiedades mostradas en el icono **propiedades** se controlan mediante herramientas que no forman parte de administrador del servidor (el panel de control, por ejemplo), los cambios en la configuración de **las propiedades** no siempre se muestran en el icono **propiedades** inmediatamente. De forma predeterminada, los datos del icono **Propiedades** se actualizan cada dos minutos. Para actualizar los datos del icono **propiedades** inmediatamente, haga clic en **Actualizar** en la barra de direcciones administrador del servidor.

|Configuración|Descripción|
|------|--------|
|nombre de equipo|Muestra el nombre descriptivo del equipo y abre el cuadro de diálogo **propiedades del sistema** , que permite cambiar el nombre del servidor, la pertenencia a dominio y otras opciones del sistema, como los perfiles de usuario.|
|Dominio (o Grupo de trabajo, si el servidor no está unido a un dominio)|Muestra el dominio o grupo de trabajo al que pertenece el servidor. Abre el cuadro de diálogo **propiedades del sistema** , que permite cambiar el nombre del servidor, la pertenencia a dominio y otras configuraciones del sistema, como los perfiles de usuario.|
|Firewall de Windows|Muestra el estado de Firewall de Windows del servidor local. Abre **Panel de control\Sistema y seguridad\Firewall de Windows**. Para obtener más información sobre la configuración de Firewall de Windows, consulta [Firewall de Windows con seguridad avanzada e IPsec](https://go.microsoft.com/fwlink/?LinkId=253465).|
|administración remota|Muestra Administrador del servidor y el estado de administración remota de Windows PowerShell. Abre el cuadro de diálogo **Configurar administración remota** . Para obtener más información sobre la administración remota, vea [configuración de la administración remota en Administrador del servidor](configure-remote-management-in-server-manager.md).|
|Escritorio remoto|Muestra si los usuarios se pueden conectar el servidor de forma remota mediante sesiones de Escritorio remoto. Abre la pestaña **remoto** del cuadro de diálogo **propiedades del sistema** .|
|Formación de equipos NIC|Muestra si el servidor local participa o no en la Formación de equipos NIC. Abre el cuadro de diálogo **NIC Teaming** y le permite unir el servidor local a un equipo NIC si lo desea. Para obtener más información sobre la Formación de equipos NIC, consulta las [Notas del producto de Formación de equipos NIC](https://go.microsoft.com/fwlink/?LinkID=253449).|
|Ethernet|Muestra el estado de red del servidor. Abre **Panel de control\Red e Internet\Conexiones de red**.|
|Versión del sistema operativo|Este campo de solo lectura muestra el número de versión del sistema operativo Windows que ejecuta el servidor local.|
|Información de hardware|Este campo de solo lectura muestra el fabricante y el número y nombre de modelo del hardware del servidor.|
|Últimas actualizaciones instaladas|Muestra el día y la hora en que se instalaron por última vez actualizaciones de Windows. Abre **Panel de control\Sistema y seguridad\Windows Update**.|
|Windows Update|Muestra la configuración de Windows Update del servidor local. Abre **Panel de control\Sistema y seguridad\Windows Update**.|
|Últimas actualizaciones buscadas|Muestra el día y la hora en que el servidor comprobó por última vez si había actualizaciones de Windows disponibles. Abre **Panel de control\Sistema y seguridad\Windows Update**.|
|Informe de errores de Windows|Muestra el estado de participación en el Informe de errores de Windows. Abre el cuadro de diálogo **Configuración de Informe de errores de Windows**. Para obtener más información sobre el Informe de errores de Windows, sus ventajas, declaraciones de privacidad y configuración de participación, consulta [Informe de errores de Windows](https://go.microsoft.com/fwlink/?LinkID=245991).|
|Programa para la mejora de la experiencia del usuario|Muestra el estado de participación en el Programa para la mejora de la experiencia del usuario de Windows. Abre el cuadro de diálogo **Configuración del Programa para la mejora de la experiencia del usuario**. Para obtener más información sobre el Programa para la mejora de la experiencia del usuario de Windows, sus ventajas y configuración de participación, consulta [Programa para la mejora de la experiencia del usuario de Windows](https://go.microsoft.com/fwlink/?LinkID=245992).|
|Configuración de seguridad mejorada de Internet Explorer|Muestra si la Configuración de seguridad mejorada de Internet Explorer (también conocida como IE Hardening o IE ESC) está activada o desactivada. Abre el cuadro de diálogo **Configuración de seguridad mejorada de Internet Explorer**. La Configuración de seguridad mejorada de Internet Explorer es una medida de seguridad para servidores que impide la apertura de páginas web en Internet Explorer. Para obtener más información acerca de la Configuración de seguridad mejorada de Internet Explorer, sus ventajas y configuración, vea [Configuración de seguridad mejorada de Internet Explorer](https://go.microsoft.com/fwlink/?LinkId=253461).|
|zona horaria|Muestra la zona horaria del servidor local. Abre el cuadro de diálogo **fecha y hora** .|
|Product ID|Muestra el estado de activación de Windows y el número de ID. de producto (si se ha activado Windows) del sistema operativo Windows Server 2016. No es el mismo número que la clave del producto de Windows. Abre el cuadro de diálogo **Activar Windows**.|
|Procesadores|Este campo de solo lectura muestra la información de fabricante, nombre de modelo y velocidad de los procesadores del servidor local.|
|Memoria instalada (RAM)|Este campo de solo lectura muestra la cantidad de RAM disponible, en gigabytes.|
|Espacio total en disco|Este campo de solo lectura muestra la cantidad de espacio en disco disponible, en gigabytes.|

## <a name="manage-the-server-manager-console"></a><a name=BKMK_managesm></a>Administrar la consola del Administrador del servidor
La configuración global que se aplica a toda la consola de Administrador del servidor y a todos los servidores remotos que se han agregado al grupo de servidores de Administrador del servidor se encuentra en las barras de título de la parte superior de la ventana de la consola de Administrador del servidor.

### <a name="add-servers-to-server-manager"></a>agregar servidores a Administrador del servidor
El comando que abre el cuadro de diálogo **agregar servidores** y permite agregar servidores físicos o virtuales remotos al grupo de servidores de administrador del servidor, se encuentra en el menú **administrar** de la consola de administrador del servidor. Para obtener información detallada sobre cómo agregar servidores, consulte [adición de servidores a administrador del servidor](add-servers-to-server-manager.md).

### <a name="refresh-data-that-is-displayed-in-server-manager"></a>Actualización de los datos mostrados en el Administrador del servidor
Puede configurar el intervalo de actualización para los datos que se muestran en Administrador del servidor en el cuadro de diálogo **propiedades de administrador del servidor** , que se abre desde el menú **administrar** .

##### <a name="to-configure-the-refresh-interval-in-server-manager"></a>Para configurar el intervalo de actualización en el Administrador del servidor

1.  En el menú **administrar** de la consola de administrador del servidor, haga clic en **Administrador del servidor propiedades**.

2.  En el cuadro de diálogo **propiedades de administrador del servidor** , especifique un período de tiempo, en minutos, para la cantidad de tiempo que desea que transcurra entre las actualizaciones de los datos que se muestran en Administrador del servidor. El valor predeterminado es 10 minutos. Haga clic en Aceptar cuando haya acabado.

#### <a name="refresh-limitations"></a>Limitaciones de la actualización
La actualización se aplica globalmente a los datos de todos los servidores que ha agregado al grupo de servidores de Administrador del servidor. No puede actualizar los datos ni configurar intervalos de actualización distintos para servidores, roles o grupos individuales.

Cuando los servidores que se encuentran en un clúster se agregan a Administrador del servidor, tanto si son equipos físicos como si son máquinas virtuales, la primera actualización de los datos puede producir un error o mostrar solo los datos del servidor host para los objetos en clúster. Las actualizaciones siguientes ya muestran datos precisos de los servidores físicos o virtuales de un clúster de servidores.

Los datos que se muestran en las páginas principales de roles en Administrador del servidor para Servicios de Escritorio remoto, administración de direcciones IP y servicios de archivos y almacenamiento no se actualizan automáticamente. Actualice los datos que se muestran en estas páginas manualmente; para ello, presione **F5** o haga clic en **Actualizar** en el encabezado de la consola de administrador del servidor mientras está en esas páginas.

### <a name="add-or-remove-roles-or-features"></a>Agregar o quitar roles o características
Los comandos que abren el Asistente para agregar roles y características y el Asistente para quitar roles y características, y permiten agregar o quitar roles, servicios de rol y características de los servidores del grupo de servidores, se encuentran en el menú **administrar** de la consola de administrador del servidor, y en el menú **tareas** del icono **roles y características** en páginas de roles o grupos. Para obtener información detallada sobre cómo agregar o quitar roles o características, vea [Instalación o desinstalación de roles, servicios de rol o características](install-or-uninstall-roles-role-services-or-features.md).

En Administrador del servidor, los datos de roles y características se muestran en el idioma base del sistema, también denominado idioma predeterminado de la GUI del sistema, o el idioma seleccionado durante la instalación del sistema operativo.

### <a name="create-server-groups"></a>crear grupos de servidores
El comando que abre el cuadro de diálogo **Crear grupo** de servidores y que permite crear grupos de servidores personalizados se encuentra en el menú **administrar** de la consola de administrador del servidor. Para obtener información detallada sobre cómo crear grupos de servidores, vea [crear y administrar grupos de servidores](create-and-manage-server-groups.md).

### <a name="prevent-server-manager-from-opening-automatically-at-logon"></a>Impedir que el Administrador del servidor se abra automáticamente al iniciar sesión
La casilla no **iniciar administrador del servidor automáticamente al iniciar sesión** del cuadro de diálogo **propiedades de Administrador del servidor** controla si administrador del servidor se abre automáticamente al iniciar sesión para los miembros del grupo administradores en un servidor local. Esta configuración no afecta al comportamiento de Administrador del servidor cuando se ejecuta en Windows 10 como parte de Herramientas de administración remota del servidor. Para obtener más información acerca de cómo configurar esta opción, consulte [Administrador del servidor](server-manager.md).

### <a name="zoom-in-or-out"></a>Acercar o alejar
Para acercar o alejar la vista de la consola de Administrador del servidor, puede usar los comandos de **zoom** del menú **Ver** o presionar **Ctrl + más (+)** para acercar y **Ctrl + menos (-)** para alejar.

## <a name="customize-tools-that-are-displayed-in-the-tools-menu"></a><a name=BKMK_tools></a>Personalizar las herramientas mostradas en el menú Herramientas
El menú **herramientas** de administrador del servidor incluye vínculos a los accesos directos de la carpeta **herramientas administrativas** del **Panel de control/sistema y seguridad**. La carpeta **herramientas administrativas** contiene una lista de accesos directos o archivos lnk para las herramientas de administración disponibles, como los complementos MMC. Administrador del servidor rellena el menú **herramientas** con vínculos a dichos accesos directos y copia la estructura de carpetas de la carpeta **herramientas administrativas** en el menú **herramientas** . De forma predeterminada, las herramientas de la carpeta Herramientas administrativas están organizadas en una lista plana, ordenadas por tipo y nombre. En el menú **herramientas** de administrador del servidor, los elementos se ordenan solo por nombre, no por tipo.

Para personalizar el menú **Herramientas**, copie los accesos directos a herramientas o scripts que desee usar en la carpeta **Herramientas administrativas**. También puede organizar los accesos directos en carpetas, con lo que se crean menús en cascada en el menú **Herramientas**. Además, si desea restringir el acceso a las herramientas personalizadas del menú **herramientas** , puede establecer derechos de acceso de usuario en las carpetas de herramientas personalizadas de herramientas administrativas o directamente en los archivos de herramientas o scripts originales.

Se recomienda no reorganizar las herramientas administrativas y del sistema y las herramientas de administración asociadas con roles y características instalados en el servidor local. Si se mueven las herramientas de administración de roles y características, se puede impedir la correcta desinstalación de estas herramientas de administración, cuando sea necesario. Después de desinstalar un rol o característica, en el menú **Herramientas** podría quedar un vínculo no funcional a una herramienta cuyo acceso directo se haya movido. Si reinstala el rol, se crea un vínculo duplicado a la misma herramienta en el menú **Herramientas**, pero uno de los vínculos no funcionará.

No obstante, las herramientas de roles y características que se instalan como parte de las Herramientas de administración remota del servidor en un equipo de Windows basado en el cliente, se pueden organizar en carpetas personalizadas. La desinstalación del rol o la característica primarios no tiene ningún efecto en los métodos abreviados de herramientas que están disponibles en un equipo remoto que ejecuta Windows 10.

En el procedimiento siguiente se describe cómo crear una carpeta de ejemplo denominada mis *herramientas* y cómo trasladar los accesos directos de dos scripts de Windows PowerShell a la carpeta a la que se puede tener acceso desde el menú herramientas administrador del servidor.

#### <a name="to-customize-the-tools-menu-by-adding-shortcuts-in-administrative-tools"></a>Para personalizar el menú Herramientas mediante la adición de accesos directos en Herramientas administrativas

1.  Cree una nueva carpeta denominada mis *herramientas* en una ubicación adecuada.

    > [!NOTE]
    > A causa de los restrictivos derechos de acceso en la carpeta **Herramientas administrativas**, no puede crear una carpeta directamente en la carpeta **Herramientas administrativas**; debe crear una carpeta en otra ubicación (como el escritorio) y luego copiar la nueva carpeta en la carpeta **Herramientas administrativas**.

2.  mueva o copie mis *herramientas* en **Panel de control/sistema y seguridad/herramientas administrativas**. De forma predeterminada, debe pertenecer al grupo Administradores del equipo para realizar cambios en la carpeta **Herramientas administrativas**.

3.  Si no necesita restringir los derechos de acceso de usuario a los métodos abreviados de herramientas personalizados, vaya al paso 6. De lo contrario, haga clic con el botón secundario en el archivo de la herramienta (o en la carpeta *MisHerramientas*) y, a continuación, haga clic en **Propiedades**.

4.  En la pestaña **seguridad** del cuadro de diálogo **propiedades** del archivo, haga clic en **Editar**.

5.  para los usuarios para los que desea restringir el acceso a las herramientas, desactive las casillas de los permisos de **lectura & ejecutar**, **leer** y **escribir** . El acceso directo a la herramienta hereda estos permisos en la carpeta **Herramientas administrativas**.

    Si edita los derechos de acceso de un usuario mientras el usuario usa Administrador del servidor (o mientras el Administrador del servidor está abierto), los cambios no se muestran en el menú **herramientas** hasta que el usuario reinicia administrador del servidor.

    > [!NOTE]
    > Si restringe el acceso a una carpeta completa que ha copiado en herramientas administrativas, los usuarios restringidos no podrán ver ni la carpeta ni su contenido en el menú **herramientas** de administrador del servidor.
    >
    > Edite los permisos de la carpeta en la carpeta **herramientas administrativas** . Dado que los archivos y carpetas ocultos de las herramientas administrativas siempre se muestran en el menú **herramientas** de administrador del servidor, no use el valor **oculto** en el cuadro de diálogo **propiedades** de un archivo o carpeta para restringir el acceso de los usuarios a los accesos directos a herramientas personalizadas.
    >
    > Los permisos **Denegar** siempre sobrescriben los permisos **Permitir**.

6.  Haga clic con el botón secundario en la herramienta, el script o el archivo ejecutable original para el que desea agregar entradas en el menú **herramientas** y, a continuación, haga clic en **crear acceso directo**.

7.  Mueva el acceso directo a *la carpeta de mis herramientas en* herramientas administrativas.

8.  Actualice o reinicie Administrador del servidor, si es necesario, para ver el acceso directo a la herramienta personalizada en el menú **herramientas** .

## <a name="manage-roles-on-role-home-pages"></a><a name=BKMK_roles></a>Administrar roles en páginas principales de rol
Después de agregar servidores al grupo de servidores de Administrador del servidor y Administrador del servidor recopila datos de inventario sobre los servidores del grupo, Administrador del servidor agrega páginas al panel de navegación para los roles detectados en los servidores administrados. El icono **Servidores** de las páginas de rol muestra los servidores administrados que ejecutan el rol. De forma predeterminada, los iconos **Eventos**, **Analizador de procedimientos recomendados**, **Servicios** y **Rendimiento** muestran datos de todos los servidores que ejecutan el rol y si seleccionas servidores específicos en el icono **Servidores**, se limita el ámbito de los eventos, servicios, contadores de rendimiento y resultados de BPA solo al servidor seleccionado. Las herramientas de administración suelen estar disponibles en el menú **herramientas** de la consola de administrador del servidor, después de instalar o detectar un rol o una característica en un servidor administrado. También puede hacer clic con el botón secundario en las entradas de servidor del icono **Servidores** para un rol o grupo y, a continuación, iniciar la herramienta de administración que desee usar.

En Windows Server 2016, los siguientes roles y características tienen herramientas de administración que se integran en Administrador del servidor consola como páginas.

-   **Servicios de archivos y almacenamiento.** Las páginas de Servicios de archivos y almacenamiento incluyen iconos y comandos personalizados para administrar volúmenes, recursos compartidos, discos virtuales iSCSI y grupos de almacenamiento. Al abrir la Página principal del rol servicios de archivos y almacenamiento en Administrador del servidor, se abre un panel de retracción que muestra las páginas de administración personalizadas de los servicios de archivos y almacenamiento. Para obtener más información sobre la implementación y administración de los Servicios de archivos y almacenamiento, consulta [Servicios de archivos y almacenamiento](https://go.microsoft.com/fwlink/p/?LinkId=241530).

-   **Servicios de Escritorio remoto.** Las páginas de Servicios de Escritorio remoto incluyen iconos y comandos personalizados para administrar sesiones, licencias, puertas de enlace y escritorios virtuales. Para obtener más información sobre la implementación y administración de Servicios de Escritorio remoto, vea [servicios de escritorio remoto (rdS)](https://go.microsoft.com/fwlink/p/?LinkId=241532).

-   **Administración de direcciones IP (IPAM).** La página del rol IPAM incluye un icono de **Página principal** personalizado que contiene vínculos a tareas comunes de configuración y administración de IPAM, incluido un asistente para aprovisionar un servidor IPAM. La página principal de IPAM también incluye iconos para ver la red administrada, el resumen de configuración y las tareas programadas.

    Existen algunas limitaciones en la administración de IPAM en Administrador del servidor. A diferencia de las páginas de rol y grupo habituales, IPAM no tiene los iconos **Servidores**, **Eventos**, **Rendimiento**, **Analizador de procedimientos recomendados** o **Servicios**. No hay ningún modelo de Analizador de procedimientos recomendados disponible para IPAM; No se admiten los exámenes de Analizador de procedimientos recomendados en IPAM. Para obtener acceso a los servidores del grupo de servidores que ejecutan IPAM, cree un grupo personalizado de esos servidores y obtenga acceso a la lista de servidores desde el icono **Servidores** de la página del grupo personalizado. Como alternativa, puede obtener acceso a los servidores IPAM desde el icono **Servidores** de la página del grupo **Todos los servidores**.

    Las miniaturas del panel también muestran filas limitadas para IPAM, en comparación de las miniaturas de otros roles y grupos. Si hace clic en las filas de miniaturas de IPAM, puede ver los eventos, datos de rendimiento y alertas de estado de administración de los servidores que ejecutan IPAM. Los servicios relacionados con IPAM se pueden administrar desde páginas de grupos de servidores que contiene servidores IPAM, como la página del grupo **Todos los servidores**.

    para obtener más información sobre la implementación y administración de IPAM, consulte [Administración de direcciones IP (IPAM)](https://go.microsoft.com/fwlink/p/?LinkId=241533).

## <a name="see-also"></a>Consulte también
[Administrador del servidor](server-manager.md) 
 [agregar servidores a administrador del servidor](add-servers-to-server-manager.md) 
 [crear y administrar grupos](create-and-manage-server-groups.md) 
 de servidores [Ver y configurar los datos de rendimiento, evento y servicio](view-and-configure-performance-event-and-service-data.md) 
 Servicios de archivos [y almacenamiento](https://go.microsoft.com/fwlink/p/?LinkId=241530) 
 [Servicios de escritorio remoto (rdS)](https://go.microsoft.com/fwlink/p/?LinkId=241532) 
 [Administración de direcciones IP (IPAM)](https://go.microsoft.com/fwlink/p/?LinkId=241533)



