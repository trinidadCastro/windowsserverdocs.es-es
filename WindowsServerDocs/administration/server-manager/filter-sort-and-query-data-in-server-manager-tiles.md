---
title: Filtrar, ordenar y consultar sobre datos en las ventanas del Administrador del servidor
description: Administrador de servidores
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8786f791-73e5-4c75-8d12-46e88a196976
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 51c0e1f3af727c4e7ddf18024fab9a95808d2338
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868916"
---
# <a name="filter-sort-and-query-data-in-server-manager-tiles"></a>Filtrar, ordenar y consultar sobre datos en las ventanas del Administrador del servidor

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En Windows Server, los iconos en el administrador del servidor le permitirán filtrar y ordenar los datos y crear y guardar las consultas personalizadas. Puede ordenar, utilice filtros de palabras clave y ejecutar consultas en las entradas de la lista de los mosaicos eventos, rendimiento, Best Practices Analyzer, servicios y Roles y características en las páginas de rol o grupo de servidor en el administrador del servidor.  
  
En este tema se incluyen las siguientes secciones.  
  
-   [Filtrar entradas de lista en mosaicos](#BKMK_tiles)  
  
-   [ordenar las entradas de lista de iconos](#BKMK_sort)  
  
-   [crear y ejecutar consultas personalizadas en los datos de mosaico](#BKMK_query)  
  
## <a name="BKMK_tiles"></a>Filtrar entradas de lista en mosaicos  
El cuadro de texto **Filtro** es una manera rápida de reducir la lista de entradas que se muestran en un mosaico a solo aquellas entradas que contienen una cadena de texto especificada.  
  
#### <a name="to-apply-a-filter-to-the-list-of-entries-in-a-tile"></a>Para aplicar un filtro en la lista de entradas de un mosaico  
  
1.  Abra la página de un rol o el servidor de grupo en Administrador del servidor.  
  
2.  En el **filtro** cuadro de texto de un mosaico de eventos, rendimiento, Best Practices Analyzer, servicios o Roles y características, escriba una cadena en el que desea filtrar.  
  
    Por ejemplo, si desea ver aquellos eventos con un identificador de evento 1014, escriba **1014** en el **filtro** cuadro de texto. Como mínimo, se devolverá como resultado todos los eventos recopilados que contengan la cadena **1014** en un campo.  
  
3.  Tenga en cuenta que el filtro cambia la descripción del texto debajo del título del mosaico. En lugar de los resultados **Todos**, dice **Resultados filtrados**.  
  
4.  Para eliminar el filtro, elimine la cadena del cuadro de filtro o haga clic en **X**.  
  
## <a name="BKMK_sort"></a>ordenar las entradas de lista de iconos  
Haga clic en los encabezados de columna para ordenar las entradas de la lista de iconos del administrador del servidor. Al hacer clic en el mosaico de una columna por primera vez, los valores de la columna se ordenan en orden alfanumérico ascendente (flecha hacia arriba); al hacer clic de nuevo, los valores se ordenan en orden alfanumérico descendente (flecha hacia abajo).  
  
## <a name="BKMK_query"></a>crear y ejecutar consultas personalizadas en los datos de mosaico  
Puede crear consultas personalizadas en los mosaicos eventos, rendimiento, Best Practices Analyzer, servicios o Roles y características en el administrador del servidor. De forma predeterminada, el área de la barra de herramientas del mosaico donde selecciona los criterios para crear una consulta personalizada está oculta; Haga clic en **expanda** (botón de contenido en el borde derecho de la barra de herramientas del mosaico) para mostrar los criterios de consulta.  
  
#### <a name="to-create-a-custom-query-for-tile-data"></a>Para crear una consulta personalizada para los datos del mosaico  
  
1.  Abra la página de un rol o el servidor de grupo en Administrador del servidor.  
  
2.  En un mosaico de eventos, rendimiento, Best Practices Analyzer, servicios o Roles y características, expanda el área de creación de la consulta haciendo clic en **expanda**.  
  
3.  Haga clic en **agregar criterios** para abrir una lista de atributos (o campos) que se aplican a las entradas en el icono.  
  
4.  Seleccione los criterios para agregar. Cuando haya terminado, haga clic en **agregar**. Los criterios seleccionados se agregan en el área de creación de consultas.  
  
5.  Haga clic en el operador de hipertexto para seleccionar un operador. Por ejemplo, para conocer los criterios de hora y fecha numéricos, el valor predeterminado es **inferior o igual a**.  
  
6.  Especifique los valores aceptables para los criterios. Por ejemplo, si seleccionó **fecha y hora**, proporcione una fecha con el formato *m/d/aaaa*.  
  
7.  Repita estos pasos, desde el paso 3 en adelante para agregar más criterios en la consulta.  
  
    Puede agregar duplicados de criterios que ya están en la consulta, pero los duplicados se agregan a la consulta con el operador **o**.  
  
    Por ejemplo, para consultar eventos identificadores 1003 o 1014, se primero agregar el criterio de identificador a la consulta, igualar el valor de identificador a **1003**y, a continuación, agregue un segundo criterio de identificador a la consulta, lo que hace que el valor del segundo identificador sea igual a  **1014**. El resultado de la búsqueda es **y el identificador es igual a 1003 o 1014**.  
  
8.  Cuando haya terminado de agregar los criterios y de especificar los operadores y los valores, haga clic en **Guardar** para guardar la consulta.  
  
9. Escriba un nombre descriptivo para la consulta. Por ejemplo, la consulta creada en el paso anterior puede llamarse **Eventos de licencias**.  
  
10. Cuando haya terminado de ver los resultados, haga clic en **Borrar todo** para borrar todos los filtros y las consultas y mostrar todas las entradas de la lista.  
  
11. Para ejecutar una consulta guardada, haga clic en **Consultas de búsqueda guardadas**y haga clic en el nombre de la consulta guardada que desea ejecutar.  
  
12. Para eliminar una consulta guardada, haga clic en **Consultas de búsqueda guardadas**y haga clic **X** junto al nombre de la consulta guardada que desea eliminar.  
  
## <a name="see-also"></a>Vea también  
[Administrador del servidor](server-manager.md)  
[Ver y configurar el rendimiento, eventos, datos de servicio](view-and-configure-performance-event-and-service-data.md)  
  


