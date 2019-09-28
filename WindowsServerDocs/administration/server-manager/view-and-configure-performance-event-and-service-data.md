---
title: Ver y configurar los datos de servicio y de eventos de rendimiento
description: Administrador de servidores
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ccd59c35-4dbf-48e7-88a4-c519c00184d1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 55ff19988cf502c2fdc968f08f207120956217df
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383042"
---
# <a name="view-and-configure-performance-event-and-service-data"></a>Visualización y configuración de rendimiento, evento y datos de servicio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se describe cómo ver y configurar las entradas del registro de eventos, los contadores de rendimiento y las alertas de servicio que se muestran para los servidores locales y remotos en Administrador del servidor.  

Los datos del registro de eventos, servicios y rendimiento se muestran en dos lugares de la consola de Administrador del servidor en Windows Server.  

-   En el panel, puede hacer clic en las filas de miniaturas **eventos**, **rendimiento**y **servicios** para configurar los datos del registro de eventos, rendimiento y servicios que desea ver para los roles, todo el grupo de servidores de administrador del servidor, grupos creados por el usuario de servidores y el servidor local. Al hacer clic en las filas de hipertexto, se abren los cuadros de diálogo **vista de detalle** que permiten especificar los datos sobre los que desea recibir alertas en el panel. Después de configurar los datos de registro de eventos, servicios y rendimiento que desea que se resalten en las miniaturas del panel, las entradas de registro que coincidan con los criterios especificados se enumeran en la parte inferior de los cuadros de diálogo **vista de detalle** .  

-   Los iconos**Eventos**, **Servicios**y **Rendimiento** forman parte de las páginas principales de los roles y los grupos. Los comandos del menú **Tareas** de estos iconos permiten especificar los datos que desea recopilar de los servidores administrados. Los iconos incluyen filtros y consultas para limitar aún más las entradas del registro que se muestran en el icono, si lo desea.  

En este tema se incluyen las siguientes secciones.  

-   [¿Qué son las miniaturas?](#BKMK_thumb)  

-   [Ver y configurar eventos](#BKMK_events)  

-   [Ver y configurar contadores de rendimiento](#BKMK_perf)  

-   [Administrar servicios y configurar alertas de servicio](#BKMK_services)  

-   [Ver y copiar entradas de eventos o de rendimiento](#BKMK_copy)  

## <a name="BKMK_thumb"></a>¿Qué son las miniaturas?  
Las *miniaturas* se muestran en el panel de administrador del servidor de cada rol (la miniatura de un rol refleja los datos recopilados sobre todos los servidores del grupo de administrador del servidor que ejecutan el rol), para cada grupo de servidores, para el grupo **todos los servidores** (todos de los servidores del grupo de Administrador del servidor) y para el servidor local. Una vez que Administrador del servidor obtiene los datos de los servidores administrados, se crean automáticamente miniaturas para los roles que se ejecutan en los servidores del grupo de servidores.  

Si la consola de Administrador del servidor se está ejecutando en un equipo cliente como parte de Herramientas de administración remota del servidor, no hay ninguna miniatura del **servidor local** .  

La miniatura muestra una vista rápida del estado y la capacidad de administración de los roles, servidores y grupos de servidores. La fila de encabezado de la miniatura cambia de color (y los números resaltados se muestran en el margen izquierdo) cuando los eventos, los contadores de rendimiento, los resultados de Analizador de procedimientos recomendados, los servicios o los problemas de capacidad de administración generales cumplen los criterios que se configuran en el **detalle Ver** cuadros de diálogo abiertos haciendo clic en filas en miniatura. En la tabla siguiente se describen los datos que se muestran en las miniaturas.  

|Fila de la miniatura|Descripción|  
|---------|--------|  
|Manageability|La capacidad de administración de un servidor de incluye varias medidas: Si el servidor está en línea o sin conexión, si es accesible e informa de los datos de Administrador del servidor, si el usuario que ha iniciado sesión en el equipo local tiene los derechos de usuario adecuados para obtener acceso o administrar el servidor remoto, si el servidor remoto ejecuta todo el software necesario para administrarlo de forma remota o si el servidor está configurado de forma que permita consultarlo y administrarlo mediante Administrador del servidor. Los únicos datos de capacidad de administración que Administrador del servidor puede recopilar de un servidor que ejecuta Windows Server 2003 es si el servidor está en línea o sin conexión. Para obtener información detallada sobre los errores de estado de capacidad de administración y cómo resolverlos, consulta la [Guía de solución de problemas del administrador del servidor](https://social.technet.microsoft.com/wiki/contents/articles/13443.windows-server-2012-server-manager-troubleshooting-guide-part-i-overview.aspx).|  
|Events|Puede configurar la fila **Eventos** de una miniatura para mostrar las alertas cuando se registren eventos que coincidan con los niveles de gravedad, las fuentes, los períodos de tiempo o los identificadores de eventos especificados. Para ver detalles acerca de los eventos y cambiar las alertas que desea ver, haga clic en la fila **eventos** y abra el cuadro de diálogo **vista de detalles de eventos** del rol o grupo de servidores.|  
|Servicios|Puede configurar la fila **servicios** para mostrar alertas cuando se encuentren servicios en un rol o grupo de servidores que coincidan con los tipos de inicio, el estado del servicio, los nombres de servicio y los servidores que especifique en el cuadro de diálogo **vista de detalles de servicios** .<br /><br />Después de agregar un servidor al grupo de servidores de Administrador del servidor, se pueden mostrar alertas de servicio sobre el servicio detección de hardware Shell si no hay usuarios con sesión iniciada en el servidor administrado. Esto sucede porque el servicio Detección de hardware shell se ejecuta solo cuando los usuarios han iniciado sesión en el servidor administrado o cuando están conectados a una sesión de Escritorio remoto en el servidor administrado. Para no ver las alertas del servicio Detección de hardware shell en este caso, haga clic en **Servicios** en las miniaturas para grupos de servidores, incluido el grupo **Todos los servidores**. En el cuadro de diálogo **vista de detalles de servicios** , en la lista desplegable **servicios** , desactive la casilla de **detección de hardware Shell**y, a continuación, haga clic en **Aceptar**.|  
|Rendimiento|Puede configurar la fila **rendimiento** para mostrar las alertas de un rol o grupo de servidores cuando se produzcan alertas de rendimiento que coincidan con los tipos de recursos, los servidores o los períodos de tiempo que especifique en el cuadro de diálogo **vista de detalles de rendimiento** .<br /><br />De manera predeterminada, los contadores de rendimiento están desactivados. Los servidores administrados que ejecutan sistemas operativos posteriores a Windows Server 2003 y cuyos contadores de rendimiento no se han iniciado, normalmente muestran errores de estado de capacidad de administración de **contadores de rendimiento en línea no iniciados** en los **servidores** icono de páginas de roles o grupos. Para activar los contadores de rendimiento en los servidores administrados, en la página **todos los servidores** , haga clic con el botón secundario en entradas en el icono **rendimiento** que muestran un valor de **Estado de contador** de **desactivado**y, a continuación, haga clic en **iniciar contadores de rendimiento**. También puede iniciar los contadores de rendimiento haciendo clic con el botón secundario en entradas para servidores en el icono **servidores** de las páginas de roles o grupos y, a continuación, haciendo clic en **iniciar contadores de rendimiento**.|  
|Resultados BPA|Puede configurar la fila de **resultados de BPA** para mostrar las alertas de un rol o grupo de servidores cuando se encuentren resultados de análisis BPA que coincidan con los niveles de gravedad, los servidores o las categorías de BPA que especifique en el cuadro de diálogo vista de detalles de resultados de **BPA** .|  

## <a name="BKMK_events"></a>Ver y configurar eventos  
En esta sección, aprenderá a configurar qué datos del registro de eventos se recopilan de los servidores del grupo de servidores de Administrador del servidor y qué eventos desea que se resalten en las miniaturas.  

> [!NOTE]  
> Los eventos sobre los que se notifican en las miniaturas son un subconjunto del total de eventos que se indican Administrador del servidor que se van a recopilar de los servidores administrados. Aunque el cambio de criterios de eventos en el cuadro de diálogo **configurar datos de eventos** en los iconos de **eventos** puede cambiar el número de alertas que se ven en el panel de administrador del servidor, el cambio de los criterios de alertas de eventos en las miniaturas no tiene ningún efecto en los datos de registro de eventos que se recopila de los servidores administrados.  

#### <a name="to-configure-the-events-collected-from-managed-servers"></a>Para configurar los eventos recopilados desde los servidores administrados  

1.  En la consola de Administrador del servidor, abra cualquier página excepto el panel. Puede configurar los eventos que desea recopilar de los servidores administrados en el icono **Eventos** en las páginas de roles, grupos de servidores o servidores locales.  

2.  En el menú **Tareas** del icono **Eventos**, haga clic en **Configurar datos de eventos**.  

3.  Seleccione los niveles de gravedad de los eventos que desea recopilar de los servidores del grupo seleccionado. De manera predeterminada, están seleccionados los niveles de gravedad **Crítico**, **Error** y **Advertencia**.  

4.  Especifique un período de tiempo durante el cuál tienen lugar los eventos. El tiempo predeterminado de los eventos es de 24 horas.  

5.  Seleccione los archivos de registro de eventos desde los que desea que se recopilen los eventos. Los valores predeterminados son **Aplicación**, **Instalación**y **Sistema**.  

6.  Para guardar los cambios, haga clic en **Aceptar** para cerrar el cuadro de diálogo **Configurar datos de eventos** . Los datos de los eventos se actualizan automáticamente al guardar los cambios.  

#### <a name="to-configure-the-events-highlighted-in-thumbnails"></a>Para configurar los eventos destacados en las miniaturas  

1.  Si Administrador del servidor ya está abierto, vaya al paso siguiente. Si todavía no se ha abierto el Administrador del servidor, ábralo mediante una de las siguientes acciones.  

    -   En el escritorio de Windows, haga clic en **Administrador del servidor** en la barra de tareas de Windows para iniciar el Administrador del servidor.  

    -   En la pantalla **Inicio** de Windows, haz clic en la ventana Administrador de servidores.  

2.  En la página del panel, en una miniatura del icono **Grupos de servidores y roles**, haga clic en la fila **Eventos**.  

3.  En el cuadro de diálogo **vista de detalles de eventos** , agregue un nivel de gravedad a los eventos que desea mostrar. De manera predeterminada, las miniaturas solo muestran las alertas que resaltan eventos críticos. Tenga en cuenta que el número de eventos mostrados en el cuadro de diálogo **vista de detalle** aumenta al agregar un nivel de gravedad sobre el que desea recibir alertas.  

4.  En el campo **Orígenes del evento** , seleccione los orígenes del evento sobre los cuales desea recibir alertas. El valor predeterminado es **All**.  

5.  Si esta miniatura es para un rol que está instalado en varios servidores o en un grupo de varios servidores, puede seleccionar los servidores en los que desea que se muestren alertas de eventos en la lista desplegable **servidores** .  

6.  En el campo **período de tiempo** , especifique un período de tiempo de hasta 1440 minutos, 24 horas o 1 día.  

7.  En el campo **Id. de evento**, escriba los números de identificación de los eventos específicos sobre los cuales desea recibir alertas. Puedes escribir un intervalo de identificadores de eventos separados por guión ( **-** ) y excluir identificadores del intervalo anteponiendo el guión al identificador del evento o al intervalo de identificadores del evento que quieras excluir. Por ejemplo, el valor **1,3,5-99,-76** significa que se generan alertas para los identificadores del evento del 1 y 3 y para todos los eventos con identificadores entre 5 y 99, excepto para el identificador del evento 76.  

8.  A medida que modifica los criterios de visualización de alertas, también podría modificarse la cantidad de alertas de eventos que se muestran en el panel de resultados en la parte inferior del cuadro de diálogo. Seleccione entradas en la lista y haga clic en **ocultar alertas** para impedir que afecten al recuento de alertas que se muestra en la miniatura de origen. Mantenga presionado **Ctrl** mientras selecciona las alertas para seleccionar varias al mismo tiempo. Puede realizar esto con las alertas que coinciden con los criterios de las alertas de eventos, pero que no necesita ver.  

9. Haga clic en **Mostrar todo** para que las alertas ocultas vuelvan a mostrarse en la lista.  

10. Haga clic en **Aceptar** para guardar los cambios, cierre el cuadro de diálogo **vista de detalle** y vea los cambios en las alertas de eventos en la miniatura de origen.  

## <a name="BKMK_perf"></a>Ver y configurar los datos del registro de rendimiento  
En esta sección, aprenderá a configurar qué datos del registro de rendimiento se recopilan de los servidores del grupo de servidores de Administrador del servidor y qué alertas del contador de rendimiento desea que se destaquen en las miniaturas.  

De manera predeterminada, los contadores de rendimiento están desactivados. Los servidores administrados que ejecutan sistemas operativos posteriores a Windows Server 2003 y cuyos contadores de rendimiento no se han iniciado, normalmente muestran errores de estado de capacidad de administración de **contadores de rendimiento en línea no iniciados** en los **servidores** icono de páginas de roles o grupos. Para activar los contadores de rendimiento en los servidores administrados, en la página **todos los servidores** , haga clic con el botón secundario en entradas en el icono **rendimiento** que muestran un valor de **Estado de contador** de **desactivado**y, a continuación, haga clic en **iniciar contadores de rendimiento**. También puede iniciar los contadores de rendimiento haciendo clic con el botón secundario en entradas para servidores en el icono **servidores** de las páginas de roles o grupos y, a continuación, haciendo clic en **iniciar contadores de rendimiento**.  

> [!NOTE]  
> Las alertas de rendimiento que se ven en las miniaturas son un subconjunto del total de datos del contador de rendimiento que se indican Administrador del servidor que se van a recopilar de los servidores administrados. Aunque el cambio de los criterios de alerta de rendimiento en el cuadro de diálogo **configurar alertas** de rendimiento en los iconos de **rendimiento** puede cambiar el número de alertas que se ven en el panel de administrador del servidor, cambiando los criterios de alerta de rendimiento en miniaturas no tiene ningún efecto en los datos del registro de rendimiento que se recopilan de los servidores administrados.  
>   
> Por este motivo, la antigüedad máxima de los datos de rendimiento que puedes mostrar en las miniaturas no puede ser mayor que el período de presentación de gráfico máximo configurado en el cuadro de diálogo **Configurar alertas de rendimiento** . Por ejemplo, si el valor del **período de visualización del gráfico** en **configurar alertas de rendimiento** es **1 día**, el valor máximo para el campo **período de tiempo** en el cuadro de diálogo vista de **detalles de rendimiento** que ha abierto en el administrador del servidor el panel puede ser **1 día**, **24 horas**o **1.440 minutos**.  

#### <a name="to-configure-the-performance-log-data-collected-from-managed-servers"></a>Para configurar los datos de registro de rendimiento recopilados de los servidores administrados  

1.  En la consola de Administrador del servidor, abra cualquier página excepto el panel. Puede configurar los datos de rendimiento que desea recopilar de los servidores administrados en el icono **Rendimiento** en las páginas de roles, grupos de servidores o servidores locales.  

2.  Para recopilar datos del registro de rendimiento desde los servidores administrados, deben estar activados los contadores de rendimiento. Si los contadores de rendimiento están desactivados, haga clic con el botón secundario en una entrada de la lista icono **rendimiento** y, a continuación, haga clic en **iniciar contadores de rendimiento**. La recopilación de datos de los contadores de rendimiento puede llevar un tiempo, según el número de servidores desde los que se recopilan datos y el ancho de banda de red disponible. Observe el estado en la columna **Estado del contador** .  

3.  En el menú **Tareas** del icono **Rendimiento** , haga clic en **Configurar alertas de rendimiento**.  

4.  en el caso de los servidores del grupo seleccionado o que ejecutan el rol seleccionado, especifique en qué porcentaje del uso de CPU desea que se recopilen las alertas del contador de rendimiento por Administrador del servidor. El valor predeterminado es 85 %.  

5.  Especifique la memoria disponible restante, en megabytes, que deben tener los servidores antes de que se recopilen las alertas del contador de rendimiento. El valor predeterminado es 2 MB.  

6.  Especifique un período de tiempo que mostrarán los gráficos de los recursos **Uso de CPU** y **Memoria disponible** en el icono **Rendimiento** de la página seleccionada. El período predeterminado es un día. Haga clic en **Guardar**.  

    Tenga en cuenta que el número de alertas de rendimiento en el icono **Rendimiento** y la asignación de las alertas con el transcurso del tiempo que muestra el gráfico pueden cambiar al hacer clic en **Guardar**.  

    > [!NOTE]  
    > en el caso de las máquinas virtuales que tienen [memoria dinámica](https://technet.microsoft.com/library/ff817651.aspx) activado, el aumento del umbral de alertas de rendimiento puede generar alertas de falsos positivos.  

7.  Para actualizar la lista de alertas de rendimiento que se recopilan de los servidores, en el menú **Tareas**, haga clic en **Actualizar**.  

#### <a name="to-configure-the-performance-alerts-highlighted-in-thumbnails"></a>Para configurar las alertas de rendimiento destacadas en las miniaturas  

1.  En la página del panel, en una miniatura del icono **Grupos de servidores y roles** , haga clic en la fila **Rendimiento** .  

2.  En el cuadro de diálogo **vista de detalles de rendimiento** , Active o desactive las casillas de los umbrales de rendimiento de recursos sobre los que desea recibir alertas en el campo tipo de **recurso** . Tenga en cuenta que el número de alertas de rendimiento que se muestran en el cuadro de diálogo **vista de detalle** puede aumentar al agregar un umbral de rendimiento de recurso sobre el que desea recibir alertas.  

3.  Si esta miniatura es para un rol que está instalado en varios servidores o en un grupo de varios servidores, puede seleccionar los servidores en los que desea que se muestren las alertas de rendimiento en la lista desplegable **servidores** .  

4.  En el campo **período de tiempo** , especifique un período de tiempo de hasta 1440 minutos, 24 horas o 1 día.  

5.  A medida que modifica los criterios de visualización de alertas, podría modificarse la cantidad de alertas que se muestran en el panel de resultados en la parte inferior del cuadro de diálogo. Haga clic en **Ocultar alertas** para ocultar todas las alertas anteriores a la hora actual e impedir que afecten al recuento de alertas que se muestra en la miniatura de origen.  

6.  Haga clic en **Mostrar todo** para que las alertas ocultas vuelvan a mostrarse en la lista.  

7.  Haga clic en **Aceptar** para guardar los cambios, cierre el cuadro de diálogo **vista de detalle** y vea los cambios en la alerta de rendimiento en la miniatura de origen.  

#### <a name="to-view-the-properties-of-performance-alerts"></a>Para visualizar las propiedades de las alertas de rendimiento  

1.  Realice una de las siguientes acciones:  

    -   En la página del panel, en una miniatura del icono **Grupos de servidores y roles** , haga clic en la fila **Rendimiento** .  

    -   Abra la página principal de un rol o grupo y busque el icono **Rendimiento** del rol o grupo.  

2.  Haga doble clic en una alerta de rendimiento de la lista para ver sus propiedades. Como alternativa, puede hacer clic con el botón secundario en una alerta de rendimiento y, a continuación, hacer clic en **Ver propiedades**.  

3.  En el cuadro de diálogo **Propiedades de alerta de rendimiento**, seleccione entradas del registro para ver información sobre los procesos asociados a la entrada en el área **Procesos**.  

4.  Cuando haya terminado de visualizar las propiedades de alerta de rendimiento, cierre el cuadro de diálogo.  

### <a name="analyze-performance-data-and-solve-problems"></a>Analizar los datos de rendimiento y resolver problemas  
para obtener más información acerca de cómo analizar los datos del contador de rendimiento que se ven en Administrador del servidor y cómo solucionar problemas de rendimiento en los servidores administrados, vea los siguientes recursos.  

-   [Analizar datos de rendimiento](https://go.microsoft.com/fwlink/?LinkId=239829)  

-   [Solucionar problemas de rendimiento](https://go.microsoft.com/fwlink/?LinkId=239831)  

para obtener más información acerca de las herramientas avanzadas de supervisión y análisis de rendimiento disponibles para Windows Server 2012 y versiones posteriores de Windows Server, incluido Server Performance Advisor 3,0, vea [rendimiento](https://msdn.microsoft.com/windows/hardware/gg463374.aspx) en MSDN.  

## <a name="BKMK_services"></a>Administrar servicios y configurar alertas de servicio  
En esta sección, aprenderá a iniciar, detener, reiniciar, pausar o reanudar los servicios que se muestran en el icono **servicios** en las páginas de roles y grupos de servidores en Administrador del servidor. También puede configurar los servicios sobre los que recibirá alertas en miniaturas en el panel de Administrador del servidor.  

> [!NOTE]  
> No se puede cambiar el tipo de inicio de los servicios, las dependencias de servicio, las opciones de recuperación ni otras propiedades del servicio en el icono servicios de Administrador del servidor. Para cambiar otras propiedades de los servicios, aparte del estado del servicio, abra el complemento **Servicios**. En el menú **herramientas** de administrador del servidor hay disponible un acceso directo para abrir el complemento **servicios** .  

#### <a name="to-start-stop-restart-pause-or-resume-a-service"></a>Para iniciar, detener, reiniciar, pausar o reanudar un servicio  

1.  En la consola de Administrador del servidor, abra cualquier página excepto el panel (en otras palabras, cualquier página principal de rol o grupo).  

2.  En el icono **Servicios** del rol o grupo, haga clic con el botón secundario en un servicio.  

3.  En el menú contextual, haga clic sobre la acción que desea realizar sobre el servicio. Si el servicio se detiene, la única acción que puede realizar es iniciar el servicio. Del mismo modo, si el servicio se pausa, la única acción que puede realizar es reanudar el servicio.  

4.  Observe que al iniciar, detener, reiniciar, pausar o reanudar un servicio, el valor de la columna **Estado** cambia para el servicio en el icono **Servicios**.  

#### <a name="to-configure-service-alerts-highlighted-in-thumbnails"></a>Para configurar las alertas de servicio destacadas en las miniaturas  

1.  En la página del panel, en una miniatura del icono **Grupos de servidores y roles**, haga clic en la fila **Servicios**.  

2.  En el cuadro de diálogo **vista de detalles de servicios** , seleccione los tipos de inicio para los servicios sobre los que desea recibir alertas. De forma predeterminada, se selecciona **automático (Inicio retrasado)** y **automático** .  

3.  Seleccione los Estados de servicio sobre los que desea recibir alertas. De manera predeterminada, está seleccionado **Todos** .  

4.  Seleccione los servicios sobre los que desea recibir alertas. De manera predeterminada, está seleccionado **Todos** .  

5.  Seleccione los servidores asociados con el rol o grupo para el que desea recibir alertas sobre los servicios. De manera predeterminada, está seleccionado **Todos** .  

6.  A medida que modifica los criterios de visualización de alertas, podría modificarse la cantidad de alertas que se muestran en el panel de resultados en la parte inferior del cuadro de diálogo. Haga clic en **Ocultar alertas** para ocultar todas las alertas anteriores a la hora actual e impedir que afecten al recuento de alertas que se muestra en la miniatura de origen.  

7.  Haga clic en **Mostrar todo** para que las alertas ocultas vuelvan a mostrarse en la lista.  

8.  Haga clic en **Aceptar** para guardar los cambios, cierre el cuadro de diálogo **vista de detalle** y vea los cambios en las alertas de servicio en la miniatura de origen.  

## <a name="BKMK_copy"></a>Ver y copiar entradas de eventos, servicios o rendimiento  
Puede copiar las propiedades de las entradas de eventos, servicios o rendimiento tanto en los cuadros de diálogo **vista de detalle** como en los iconos **eventos** y **rendimiento** de un rol o grupo. Haga clic con el botón secundario en una entrada de evento o rendimiento y, a continuación, haga clic en **copiar**.  

El icono **Eventos** también permite obtener una vista previa de las propiedades del evento en la mitad inferior del icono al seleccionar un evento en la lista. Para copiar las propiedades que se muestran en la vista previa, haga clic con el botón secundario en el panel de vista previa y, a continuación, haga clic en **copiar**.  

## <a name="see-also"></a>Vea también  
[Administrador del servidor](server-manager.md)  
[Filtrar, ordenar y consultar datos en los iconos del Administrador del servidor](filter-sort-and-query-data-in-server-manager-tiles.md)  
