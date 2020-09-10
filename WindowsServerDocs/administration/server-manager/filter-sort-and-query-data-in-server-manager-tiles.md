---
title: Filtrar, ordenar y consultar sobre datos en las ventanas del Administrador del servidor
description: Administrador de servidores
ms.topic: article
ms.assetid: 8786f791-73e5-4c75-8d12-46e88a196976
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 0ddaaadbda28b5494ce5ab0a053b70abd5882591
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89628306"
---
# <a name="filter-sort-and-query-data-in-server-manager-tiles"></a>Filtrar, ordenar y consultar sobre datos en las ventanas del Administrador del servidor

>Se aplica a: Windows Server (Canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En Windows Server, los iconos de Administrador del servidor permiten filtrar y ordenar los datos, así como crear y guardar consultas personalizadas. Puede ordenar, usar filtros de palabras clave y ejecutar consultas en las entradas de la lista en los mosaicos eventos, rendimiento, Analizador de procedimientos recomendados, servicios y roles y características en las páginas de rol de servidor o de grupo en Administrador del servidor.

En este tema se incluyen las siguientes secciones.

-   [Filtrar entradas de la lista en mosaicos](#BKMK_tiles)

-   [ordenar entradas de la lista en mosaicos](#BKMK_sort)

-   [crear y ejecutar consultas personalizadas en los datos de los mosaicos](#BKMK_query)

## <a name="filter-list-entries-in-tiles"></a><a name=BKMK_tiles></a>Filtrar entradas de la lista en mosaicos
El cuadro de texto **Filtro** es una manera rápida de reducir la lista de entradas que se muestran en un mosaico a solo aquellas entradas que contienen una cadena de texto especificada.

#### <a name="to-apply-a-filter-to-the-list-of-entries-in-a-tile"></a>Para aplicar un filtro en la lista de entradas de un mosaico

1.  Abra una página de rol o grupo de servidores en Administrador del servidor.

2.  En el cuadro de texto **filtro** de un mosaico de eventos, rendimiento, analizador de procedimientos recomendados, servicios o roles y características, escriba una cadena en la que desee filtrar.

    por ejemplo, si desea ver solo los eventos con el identificador de evento 1014, escriba **1014** en el cuadro de texto **filtro** . Como mínimo, se devolverá como resultado todos los eventos recopilados que contengan la cadena **1014** en un campo.

3.  Tenga en cuenta que el filtro cambia la descripción del texto debajo del título del mosaico. En lugar de los resultados **Todos**, dice **Resultados filtrados**.

4.  Para eliminar el filtro, elimine la cadena del cuadro de filtro o haga clic en **X**.

## <a name="sort-list-entries-in-tiles"></a><a name=BKMK_sort></a>ordenar entradas de la lista en mosaicos
Ordene las entradas de la lista en Administrador del servidor mosaicos haciendo clic en los encabezados de columna. Al hacer clic en el mosaico de una columna por primera vez, los valores de la columna se ordenan en orden alfanumérico ascendente (flecha hacia arriba); al hacer clic de nuevo, los valores se ordenan en orden alfanumérico descendente (flecha hacia abajo).

## <a name="create-and-run-custom-queries-on-tile-data"></a><a name=BKMK_query></a>crear y ejecutar consultas personalizadas en los datos de los mosaicos
Puede crear consultas personalizadas en los iconos eventos, rendimiento, Analizador de procedimientos recomendados, servicios o roles y características en Administrador del servidor. De forma predeterminada, el área de la barra de herramientas del mosaico en la que se seleccionan los criterios para crear una consulta personalizada está oculta. Haga clic en **expandir** (botón de contenido adicional en el borde derecho de la barra de herramientas del mosaico) para mostrar los criterios de consulta.

#### <a name="to-create-a-custom-query-for-tile-data"></a>Para crear una consulta personalizada para los datos del mosaico

1.  Abra una página de rol o grupo de servidores en Administrador del servidor.

2.  En un mosaico de eventos, rendimiento, Analizador de procedimientos recomendados, servicios o roles y características, expanda el área de creación de consultas haciendo clic en **expandir**.

3.  Haga clic en **Agregar criterios** para abrir una lista de atributos (o campos) que se aplican a las entradas del icono.

4.  Seleccione los criterios que desea agregar. Cuando haya terminado, haga clic en **Agregar**. Los criterios seleccionados se agregan en el área de creación de consultas.

5.  Haga clic en el operador de hipertexto para seleccionar un operador. Por ejemplo, para conocer los criterios de hora y fecha numéricos, el valor predeterminado es **inferior o igual a**.

6.  Especifique los valores aceptables para los criterios. Por ejemplo, si seleccionó **fecha y hora**, proporcione una fecha con el formato *m/d/YYYY*.

7.  Repita estos pasos, desde el paso 3 en adelante para agregar más criterios en la consulta.

    Puede agregar duplicados de criterios que ya están en la consulta, pero los duplicados se agregan a la consulta con el operador **o**.

    por ejemplo, para consultar los identificadores de evento 1003 o 1014, primero debe agregar los criterios de identificador a la consulta, hacer que el valor de identificador sea igual a **1003**y, a continuación, agregar un segundo criterio de identificador a la consulta, lo que hace que el valor del segundo identificador sea igual a **1014**. El resultado de la búsqueda es **y el identificador es igual a 1003 o 1014**.

8.  Cuando haya terminado de agregar los criterios y de especificar los operadores y los valores, haga clic en **Guardar** para guardar la consulta.

9. Escriba un nombre descriptivo para la consulta. Por ejemplo, la consulta creada en el paso anterior puede llamarse **Eventos de licencias**.

10. Cuando haya terminado de ver los resultados, haga clic en **Borrar todo** para borrar todos los filtros y las consultas y mostrar todas las entradas de la lista.

11. Para ejecutar una consulta guardada, haga clic en **Consultas de búsqueda guardadas** y haga clic en el nombre de la consulta guardada que desea ejecutar.

12. Para eliminar una consulta guardada, haga clic en **Consultas de búsqueda guardadas** y haga clic **X** junto al nombre de la consulta guardada que desea eliminar.

## <a name="see-also"></a>Consulte también
[Administrador del servidor](server-manager.md) 
 [Ver y configurar los datos de rendimiento, evento y servicio](view-and-configure-performance-event-and-service-data.md)



