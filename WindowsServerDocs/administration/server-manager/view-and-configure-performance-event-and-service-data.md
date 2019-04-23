---
title: Ver y configurar eventos de rendimiento y servicio de datos
description: Administrador de servidores
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 727b81d1ba2cda32a7568e4e2f23065b1589e745
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861906"
---
# <a name="view-and-configure-performance-event-and-service-data"></a>Visualización y configuración de rendimiento, evento y datos de servicio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema describe cómo ver y configurar las entradas de registro de eventos, contadores de rendimiento y alertas de servicio que se muestran para los servidores locales y remotos en el administrador del servidor.  

Datos del registro de eventos, servicios y rendimiento se muestran en dos lugares en la consola de administrador del servidor en Windows Server.  

-   En el panel, puede hacer clic en el **eventos**, **rendimiento**, y **servicios** filas de miniaturas para configurar eventos, rendimiento y datos de registro de servicio que desea ver para roles, todo el grupo de servidor del Administrador de servidores, grupos de servidores creados por el usuario y el servidor local. Al hacer clic en el hipertexto filas, se abren **vista de detalle** cuadros de diálogo que le permiten especifican los datos sobre los cuales desea recibir alertas en el panel. Después de configurar eventos, servicios y datos de registro de rendimiento que desea destacar en las miniaturas del panel, las entradas de registro que coinciden con los criterios especificados se enumeran en la parte inferior de la **vista de detalle** cuadros de diálogo.  

-   Los iconos**Eventos**, **Servicios**y **Rendimiento** forman parte de las páginas principales de los roles y los grupos. Los comandos del menú **Tareas** de estos iconos permiten especificar los datos que desea recopilar de los servidores administrados. Los iconos incluyen filtros y consultas para limitar aún más las entradas del registro que se muestran en el icono, si lo desea.  

En este tema se incluyen las siguientes secciones.  

-   [¿Cuáles son las miniaturas?](#BKMK_thumb)  

-   [Ver y configurar eventos](#BKMK_events)  

-   [Ver y configurar los contadores de rendimiento](#BKMK_perf)  

-   [Administrar servicios y configurar alertas de servicio](#BKMK_services)  

-   [Visualización y copia las entradas de evento o rendimiento](#BKMK_copy)  

## <a name="BKMK_thumb"></a>¿Cuáles son las miniaturas?  
*Miniaturas* se muestran en el panel Administrador del servidor para cada rol (miniatura de un rol refleja los datos recopilados sobre todos los servidores del grupo Administrador del servidor que ejecutan el rol), para cada grupo de servidores, para el **todas Servidores** grupo (todos los servidores del grupo Administrador del servidor) y para el servidor local. Después de que el administrador del servidor obtiene datos de los servidores administrados, se crean automáticamente miniaturas para los roles que se ejecutan en servidores en el grupo de servidores.  

Si la consola de administrador del servidor se ejecuta en un equipo cliente como parte de herramientas de administración remota del servidor, no hay ningún **servidor Local** miniatura.  

La miniatura muestra una vista rápida del estado y la capacidad de administración de los roles, servidores y grupos de servidores. La fila de encabezado de la miniatura cambia de color (y los números resaltados se muestran en el margen izquierdo) cuando los eventos, contadores de rendimiento, resultados Best Practices Analyzer, servicios o problemas de facilidad de uso general cumplen los criterios que configuren en el **vista de detalle** cuadros de diálogo se abren haciendo clic en filas de miniaturas. En la tabla siguiente se describen los datos que se muestran en las miniaturas.  

|Fila de la miniatura|Descripción|  
|---------|--------|  
|Manageability|La capacidad de administración de un servidor incluye varias medidas: si el servidor está en línea o sin conexión, si es accesibles y generación de informes de datos al administrador del servidor, si el usuario que ha iniciado sesión en el equipo local tiene derechos de usuario adecuadas para tener acceso o administrar el servidor remoto, si se está ejecutando todo el software necesario para administrarlo de forma remota el servidor remoto o si el servidor está configurado de forma que le permite consultar y administrar mediante el administrador del servidor. Los datos de capacidad de administración única que puede recopilar el administrador del servidor desde un servidor que ejecuta Windows Server 2003 están si el servidor está conectado o desconectado. Para obtener información detallada sobre los errores de estado de capacidad de administración y cómo resolverlos, consulta la [Guía de solución de problemas del administrador del servidor](https://social.technet.microsoft.com/wiki/contents/articles/13443.windows-server-2012-server-manager-troubleshooting-guide-part-i-overview.aspx).|  
|Eventos|Puede configurar la fila **Eventos** de una miniatura para mostrar las alertas cuando se registren eventos que coincidan con los niveles de gravedad, las fuentes, los períodos de tiempo o los identificadores de eventos especificados. Ver detalles sobre los eventos y cambiar las alertas que desea ver, haga clic en el **eventos** de fila y abrir el **eventos de vista de detalle** cuadro de diálogo para el rol o grupo de servidores.|  
|Servicios|Puede configurar el **servicios** fila para mostrar las alertas cuando los servicios se encuentran en un rol o grupo de servidores que coincidan con los tipos de inicio, el estado del servicio, los nombres de servicio y los servidores que especifican en el **servicios de vista de detalle**  cuadro de diálogo.<br /><br />Después de agregar un servidor al grupo de servidores de administrador del servidor, se pueden mostrar las alertas de servicio sobre el servicio de detección de Hardware Shell si no hay ningún usuario inició sesión en el servidor administrado. Esto sucede porque el servicio Detección de hardware shell se ejecuta solo cuando los usuarios han iniciado sesión en el servidor administrado o cuando están conectados a una sesión de Escritorio remoto en el servidor administrado. Para no ver las alertas del servicio Detección de hardware shell en este caso, haga clic en **Servicios** en las miniaturas para grupos de servidores, incluido el grupo **Todos los servidores**. En el **servicios de vista de detalle** cuadro de diálogo el **servicios** la lista desplegable, desactive la casilla de **detección de Hardware Shell**y, a continuación, haga clic en **Aceptar**.|  
|Rendimiento|Puede configurar el **rendimiento** fila para mostrar las alertas para un rol o grupo de servidores cuando se produzcan alertas de rendimiento que coincidan con los tipos de recursos, servidores o períodos de tiempo que especifique en el **lavistadedetallederendimiento** cuadro de diálogo.<br /><br />De manera predeterminada, los contadores de rendimiento están desactivados. Los servidores administrados que ejecutan sistemas operativos posteriores a Windows Server 2003, y qué rendimiento contadores no se ha iniciado, normalmente muestran manejabilidad errores de estado de **en línea: contadores de rendimiento no ha iniciado** en el **servidores** icono de páginas de rol o grupo. Para activar los contadores de rendimiento para servidores administrados, en el **todos los servidores** página, haga clic en las entradas de la **rendimiento** mosaico que muestran un **estado del contador** valor  **Desactivar**y, a continuación, haga clic en **iniciar contadores de rendimiento**. También puede iniciar los contadores de rendimiento haciendo las entradas para servidores en el **servidores** icono de rol o las páginas de grupos y, a continuación, haga clic en **iniciar contadores de rendimiento**.|  
|Resultados BPA|Puede configurar el **los resultados del BPA** fila para mostrar las alertas para un rol o grupo de servidores cuando los resultados de examen BPA bps que coinciden con los niveles de gravedad, los servidores o las categorías que se especifican en el **devistadedetalledelosresultadosdelBPA** cuadro de diálogo.|  

## <a name="BKMK_events"></a>Ver y configurar eventos  
En esta sección, aprenda a configurar qué datos de registro de eventos se recopilan desde los servidores en el grupo de servidores del administrador del servidor y qué eventos desea que destaquen en las miniaturas.  

> [!NOTE]  
> Los eventos sobre el que se generan alertas en las miniaturas son un subconjunto del total de eventos que se indique al administrador del servidor para recopilar de los servidores administrados. Si bien la modificación de criterios de eventos en el **configurar datos de eventos** cuadro de diálogo de **eventos** iconos pueden cambiar el número de alertas en el panel Administrador del servidor, consulte Cambiar los criterios de alerta de evento en las miniaturas no tiene ningún efecto en los datos de registro de eventos que se recopilan de los servidores administrados.  

#### <a name="to-configure-the-events-collected-from-managed-servers"></a>Para configurar los eventos recopilados desde los servidores administrados  

1.  En la consola de administrador del servidor, abra cualquier página excepto el panel. Puede configurar los eventos que desea recopilar de los servidores administrados en el icono **Eventos** en las páginas de roles, grupos de servidores o servidores locales.  

2.  En el menú **Tareas** del icono **Eventos**, haga clic en **Configurar datos de eventos**.  

3.  Seleccione los niveles de gravedad del evento que desea que se recopilan desde los servidores en el grupo seleccionado. De manera predeterminada, están seleccionados los niveles de gravedad **Crítico**, **Error** y **Advertencia**.  

4.  Especifique un período de tiempo durante el cuál tienen lugar los eventos. El tiempo predeterminado de los eventos es de 24 horas.  

5.  Seleccione los archivos de registro de eventos del que desea que se recopilan los eventos. Los valores predeterminados son **Aplicación**, **Instalación**y **Sistema**.  

6.  Para guardar los cambios, haga clic en **Aceptar** para cerrar el cuadro de diálogo **Configurar datos de eventos** . Los datos de los eventos se actualizan automáticamente al guardar los cambios.  

#### <a name="to-configure-the-events-highlighted-in-thumbnails"></a>Para configurar los eventos destacados en las miniaturas  

1.  Si el administrador del servidor ya está abierto, vaya al paso siguiente. Si todavía no se ha abierto el Administrador del servidor, ábralo mediante una de las siguientes acciones.  

    -   En el escritorio de Windows, haga clic en **Administrador del servidor** en la barra de tareas de Windows para iniciar el Administrador del servidor.  

    -   En la pantalla **Inicio** de Windows, haz clic en la ventana Administrador de servidores.  

2.  En la página del panel, en una miniatura del icono **Grupos de servidores y roles**, haga clic en la fila **Eventos**.  

3.  En el **eventos de vista de detalle** diálogo cuadro, agregue un nivel de gravedad a los eventos que desea mostrar. De manera predeterminada, las miniaturas solo muestran las alertas que resaltan eventos críticos. Tenga en cuenta que el número de eventos mostrados en la **vista de detalle** cuadro de diálogo aumenta al agregar un nivel de gravedad sobre el que desea recibir alertas.  

4.  En el campo **Orígenes del evento** , seleccione los orígenes del evento sobre los cuales desea recibir alertas. El valor predeterminado es **All**.  

5.  Si esta miniatura es para un rol que está instalado en varios servidores o un grupo de varios servidores, puede seleccionar los servidores que se desean alertas de eventos en el **servidores** lista desplegable.  

6.  En el **período de tiempo** , a continuación, especifique un período de tiempo de hasta 1440 minutos, 24 horas o 1 día.  

7.  En el campo **Id. de evento**, escriba los números de identificación de los eventos específicos sobre los cuales desea recibir alertas. Puedes escribir un intervalo de identificadores de eventos separados por guión (**-**) y excluir identificadores del intervalo anteponiendo el guión al identificador del evento o al intervalo de identificadores del evento que quieras excluir. Por ejemplo, el valor **1,3,5-99,-76** significa que se generan alertas para los identificadores del evento del 1 y 3 y para todos los eventos con identificadores entre 5 y 99, excepto para el identificador del evento 76.  

8.  A medida que modifica los criterios de visualización de alertas, también podría modificarse la cantidad de alertas de eventos que se muestran en el panel de resultados en la parte inferior del cuadro de diálogo. Seleccione las entradas en la lista y haga clic en **Ocultar alertas** para impedir que afectan al recuento de alertas que se muestra en la miniatura de origen. Mantenga presionado **Ctrl** mientras selecciona las alertas para seleccionar varias al mismo tiempo. Puede realizar esto con las alertas que coinciden con los criterios de las alertas de eventos, pero que no necesita ver.  

9. Haga clic en **Mostrar todo** para que las alertas ocultas vuelvan a mostrarse en la lista.  

10. Haga clic en **Aceptar** para guardar los cambios, cierre el **vista de detalle** cuadro de diálogo y observe la alerta de evento de cambios en la miniatura de origen.  

## <a name="BKMK_perf"></a>Ver y configurar los datos de registro de rendimiento  
En esta sección, aprenda a configurar qué datos del registro de rendimiento se recopilan desde los servidores en el grupo de servidores del administrador del servidor y le avisa el contador de rendimiento desea que se destaquen en las miniaturas.  

De manera predeterminada, los contadores de rendimiento están desactivados. Los servidores administrados que ejecutan sistemas operativos posteriores a Windows Server 2003, y qué rendimiento contadores no se ha iniciado, normalmente muestran manejabilidad errores de estado de **en línea: contadores de rendimiento no ha iniciado** en el **servidores** icono de páginas de rol o grupo. Para activar los contadores de rendimiento para servidores administrados, en el **todos los servidores** página, haga clic en las entradas de la **rendimiento** mosaico que muestran un **estado del contador** valor  **Desactivar**y, a continuación, haga clic en **iniciar contadores de rendimiento**. También puede iniciar los contadores de rendimiento haciendo las entradas para servidores en el **servidores** icono de rol o las páginas de grupos y, a continuación, haga clic en **iniciar contadores de rendimiento**.  

> [!NOTE]  
> Las alertas de rendimiento que ven en las miniaturas son un subconjunto de los datos del contador de rendimiento total que se indique al administrador del servidor para recopilar de los servidores administrados. Si bien la modificación de criterios de alerta de rendimiento en el **configurar alertas de rendimiento** cuadro de diálogo de **rendimiento** iconos pueden alterar el número de alertas que se ven en el panel Administrador del servidor, cambiar los criterios de alerta de rendimiento en las miniaturas no tiene ningún efecto en los datos de registro de rendimiento que se recopilan de los servidores administrados.  
>   
> Por este motivo, la antigüedad máxima de los datos de rendimiento que puedes mostrar en las miniaturas no puede ser mayor que el período de presentación de gráfico máximo configurado en el cuadro de diálogo **Configurar alertas de rendimiento** . Por ejemplo, si la **período de presentación de gráfico** valor en **configurar alertas de rendimiento** es **1 día**, el valor máximo para el **período de tiempo**campo un **vista de detalle de rendimiento** cuadro de diálogo que se ha abierto desde el panel Administrador del servidor puede ser **1 día**, **24 horas**, o **1.440 minutos**.  

#### <a name="to-configure-the-performance-log-data-collected-from-managed-servers"></a>Para configurar los datos de registro de rendimiento recopilados de los servidores administrados  

1.  En la consola de administrador del servidor, abra cualquier página excepto el panel. Puede configurar los datos de rendimiento que desea recopilar de los servidores administrados en el icono **Rendimiento** en las páginas de roles, grupos de servidores o servidores locales.  

2.  Para recopilar datos del registro de rendimiento desde los servidores administrados, deben estar activados los contadores de rendimiento. Si los contadores de rendimiento están apagados, haga clic en una entrada en el **rendimiento** lista del icono y, a continuación, haga clic en **iniciar contadores de rendimiento**. La recopilación de datos de los contadores de rendimiento puede llevar un tiempo, según el número de servidores desde los que se recopilan datos y el ancho de banda de red disponible. Observe el estado en la columna **Estado del contador** .  

3.  En el menú **Tareas** del icono **Rendimiento** , haga clic en **Configurar alertas de rendimiento**.  

4.  los servidores en el grupo seleccionado, o que se ejecutan el rol seleccionado, especifique en qué porcentaje de uso de CPU desea que las alertas de contador de rendimiento recopiladas por el administrador del servidor. El valor predeterminado es 85 %.  

5.  Especifique la memoria disponible restante, en megabytes, que deben tener los servidores antes de que se recopilen las alertas del contador de rendimiento. El valor predeterminado es 2 MB.  

6.  Especifique un período de tiempo que mostrarán los gráficos de los recursos **Uso de CPU** y **Memoria disponible** en el icono **Rendimiento** de la página seleccionada. El período predeterminado es un día. Haga clic en **Guardar**.  

    Tenga en cuenta que el número de alertas de rendimiento en el icono **Rendimiento** y la asignación de las alertas con el transcurso del tiempo que muestra el gráfico pueden cambiar al hacer clic en **Guardar**.  

    > [!NOTE]  
    > para máquinas virtuales que tengan [memoria dinámica](https://technet.microsoft.com/library/ff817651.aspx) activado, aumento del umbral de alertas de rendimiento puede generar alertas de falsos positivos.  

7.  Para actualizar la lista de alertas de rendimiento que se recopilan de los servidores, en el menú **Tareas**, haga clic en **Actualizar**.  

#### <a name="to-configure-the-performance-alerts-highlighted-in-thumbnails"></a>Para configurar las alertas de rendimiento destacadas en las miniaturas  

1.  En la página del panel, en una miniatura del icono **Grupos de servidores y roles** , haga clic en la fila **Rendimiento** .  

2.  En el **vista de detalle de rendimiento** cuadro de diálogo, active o desactive las casillas para los umbrales de rendimiento de recurso sobre el que desea recibir alertas en el **tipo de recurso** campo. Tenga en cuenta que el número de alertas de rendimiento se muestra en el **vista de detalle** cuadro de diálogo puede aumentar al agregar un umbral de rendimiento de recurso sobre el que desea recibir alertas.  

3.  Si esta miniatura es para un rol que está instalado en varios servidores o un grupo de varios servidores, puede seleccionar los servidores que se desean alertas de rendimiento en el **servidores** lista desplegable.  

4.  En el **período de tiempo** , a continuación, especifique un período de tiempo de hasta 1440 minutos, 24 horas o 1 día.  

5.  A medida que modifica los criterios de visualización de alertas, podría modificarse la cantidad de alertas que se muestran en el panel de resultados en la parte inferior del cuadro de diálogo. Haga clic en **Ocultar alertas** para ocultar todas las alertas anteriores a la hora actual e impedir que afecten al recuento de alertas que se muestra en la miniatura de origen.  

6.  Haga clic en **Mostrar todo** para que las alertas ocultas vuelvan a mostrarse en la lista.  

7.  Haga clic en **Aceptar** para guardar los cambios, cierre el **vista de detalle** cuadro de diálogo y ver los cambios de la alerta de rendimiento en la miniatura de origen.  

#### <a name="to-view-the-properties-of-performance-alerts"></a>Para visualizar las propiedades de las alertas de rendimiento  

1.  Realice una de las siguientes acciones:  

    -   En la página del panel, en una miniatura del icono **Grupos de servidores y roles** , haga clic en la fila **Rendimiento** .  

    -   Abra la página principal de un rol o grupo y busque el icono **Rendimiento** del rol o grupo.  

2.  Haga doble clic en una alerta de rendimiento de la lista para ver sus propiedades. Como alternativa, puede hacer clic con el botón secundario en una alerta de rendimiento y, a continuación, hacer clic en **Ver propiedades**.  

3.  En el cuadro de diálogo **Propiedades de alerta de rendimiento**, seleccione entradas del registro para ver información sobre los procesos asociados a la entrada en el área **Procesos**.  

4.  Cuando haya terminado de visualizar las propiedades de alerta de rendimiento, cierre el cuadro de diálogo.  

### <a name="analyze-performance-data-and-solve-problems"></a>Analizar los datos de rendimiento y resolver problemas  
Para obtener más información acerca del análisis de datos del contador de rendimiento que se ve en el administrador del servidor y solucionar problemas de rendimiento en los servidores administrados, consulte los siguientes recursos.  

-   [Analizar los datos de rendimiento](https://go.microsoft.com/fwlink/?LinkId=239829)  

-   [Solucionar problemas de rendimiento](https://go.microsoft.com/fwlink/?LinkId=239831)  

Para obtener más información sobre herramientas avanzadas de rendimiento de supervisión y análisis que están disponibles para Windows Server 2012 y versiones posteriores de Windows Server, incluyendo Server Performance Advisor 3.0, consulte [rendimiento](https://msdn.microsoft.com/windows/hardware/gg463374.aspx) en MSDN.  

## <a name="BKMK_services"></a>Administrar servicios y configurar alertas de servicio  
En esta sección, aprenderá a iniciar, detener, reiniciar, pausar o reanudar los servicios que se muestran en el **servicios** icono en las páginas de grupo de servidores y roles en el administrador del servidor. También puede configurar los servicios sobre el que se generan alertas en las miniaturas del panel del administrador del servidor.  

> [!NOTE]  
> No se puede cambiar el tipo de inicio de servicios, las dependencias del servicio, las opciones de recuperación u otras propiedades de servicio en el icono Servicios del administrador del servidor. Para cambiar otras propiedades de los servicios, aparte del estado del servicio, abra el complemento **Servicios**. Un acceso directo para abrir el **servicios** complemento está disponible en el **herramientas** menú Administrador del servidor.  

#### <a name="to-start-stop-restart-pause-or-resume-a-service"></a>Para iniciar, detener, reiniciar, pausar o reanudar un servicio  

1.  En la consola de administrador del servidor, abra cualquier página excepto el panel (en otras palabras, cualquier página de inicio rol o grupo).  

2.  En el icono **Servicios** del rol o grupo, haga clic con el botón secundario en un servicio.  

3.  En el menú contextual, haga clic sobre la acción que desea realizar sobre el servicio. Si el servicio se detiene, la única acción que puede realizar es iniciar el servicio. Del mismo modo, si el servicio se pausa, la única acción que puede realizar es reanudar el servicio.  

4.  Observe que al iniciar, detener, reiniciar, pausar o reanudar un servicio, el valor de la columna **Estado** cambia para el servicio en el icono **Servicios**.  

#### <a name="to-configure-service-alerts-highlighted-in-thumbnails"></a>Para configurar las alertas de servicio destacadas en las miniaturas  

1.  En la página del panel, en una miniatura del icono **Grupos de servidores y roles**, haga clic en la fila **Servicios**.  

2.  En el **servicios de vista de detalle** cuadro de diálogo, seleccione los tipos de inicio para los servicios sobre los cuales desea recibir alertas. De forma predeterminada, **automático (Inicio retrasado)** y **automática** están seleccionadas.  

3.  Seleccione los Estados de servicio sobre los cuales desea recibir alertas. De manera predeterminada, está seleccionado **Todos** .  

4.  Seleccione servicios sobre los cuales desea recibir alertas. De manera predeterminada, está seleccionado **Todos** .  

5.  Seleccione los servidores asociados con el rol o grupo para el que desea recibir alertas acerca de los servicios. De manera predeterminada, está seleccionado **Todos** .  

6.  A medida que modifica los criterios de visualización de alertas, podría modificarse la cantidad de alertas que se muestran en el panel de resultados en la parte inferior del cuadro de diálogo. Haga clic en **Ocultar alertas** para ocultar todas las alertas anteriores a la hora actual e impedir que afecten al recuento de alertas que se muestra en la miniatura de origen.  

7.  Haga clic en **Mostrar todo** para que las alertas ocultas vuelvan a mostrarse en la lista.  

8.  Haga clic en **Aceptar** para guardar los cambios, cierre el **vista de detalle** cuadro de diálogo y ver los cambios de la alerta del servicio en la miniatura de origen.  

## <a name="BKMK_copy"></a>Visualización y copia las entradas de eventos, servicios o rendimiento  
Puede copiar las propiedades de entrada de eventos, servicios o rendimiento tanto en el **vista de detalle** cuadros de diálogo y el **eventos** y **rendimiento** iconos para un rol o grupo. Haga clic en una entrada de evento o rendimiento y, a continuación, haga clic en **copia**.  

El icono **Eventos** también permite obtener una vista previa de las propiedades del evento en la mitad inferior del icono al seleccionar un evento en la lista. Para copiar las propiedades mostradas en la vista previa, haga clic en el panel de vista previa y, a continuación, haga clic en **copia**.  

## <a name="see-also"></a>Vea también  
[Administrador del servidor](server-manager.md)  
[Filtrar, ordenar y consultar datos en iconos del administrador del servidor](filter-sort-and-query-data-in-server-manager-tiles.md)  
