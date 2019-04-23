---
title: Visualización y administración de actualizaciones
description: Tema de Windows Server Update Service (WSUS) - cómo ver y administrar las actualizaciones en la consola de WSUS
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ac70192b-0309-4385-b697-2e8eda51911c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cd517be0de3ba6ca97ca11f4bbe8f59111a01216
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870956"
---
# <a name="viewing-and-managing-updates"></a>Visualización y administración de actualizaciones

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puede usar la consola de WSUS para ver y administrar las actualizaciones.

## <a name="viewing-updates"></a>Ver las actualizaciones
En el **actualizaciones** página, puede hacer lo siguiente:

-   Ver las actualizaciones. La información general de actualizaciones muestra las actualizaciones que se han sincronizado desde el origen de actualización en el servidor WSUS y están disponibles para su aprobación.

-   Filtrar las actualizaciones. En la vista predeterminada puede filtrar las actualizaciones por estado de aprobación y el estado de la instalación. El valor predeterminado es sin aprobar actualizaciones que son necesarios para algunos clientes o que han tenido errores de instalación en algunos clientes. Puede cambiar esta vista cambiando los filtros de estado de instalación y el estado de aprobación y, a continuación, haga clic en **actualizar**.

-   Crear nuevas vistas de actualización. En el **acciones** panel, haga clic en **nueva vista de actualización**. Puede filtrar las actualizaciones por clasificación, producto, el grupo para el que se han aprobado y fecha de sincronización. Puede ordenar la lista, haga clic en el encabezado de columna correspondiente en la barra de título.

-   Buscar actualizaciones. Puede buscar una actualización individual o un conjunto de actualizaciones por título, descripción, el artículo de Knowledge Base o el número de Microsoft Security Response Center para la actualización.

-   Ver detalles, el estado y el historial de revisiones para cada actualización.

-   Aprobar y rechazar actualizaciones.

#### <a name="to-view-updates"></a>Para ver las actualizaciones

1.  En la consola de administración de WSUS, expanda el nodo actualizaciones y, a continuación, haga clic en todas las actualizaciones.

2.  De forma predeterminada, las actualizaciones se muestran con su título, clasificación, porcentaje aplicable/no instalado y el estado de aprobación. Si desea mostrar las propiedades de la actualización más o diferentes, haga clic en la barra de título de columna y seleccione las columnas apropiadas.

3.  Para ordenar según criterios diferentes, por ejemplo, el estado de la descarga, título, clasificación, fecha de lanzamiento o estado de aprobación, haga clic en el encabezado de columna correspondiente.

#### <a name="to-filter-the-list-of-updates-displayed-on-the-updates-page"></a>Para filtrar la lista de actualizaciones que se muestra en la página actualizaciones

1.  En la consola de administración de WSUS, expanda **actualizaciones**y, a continuación, haga clic en **todas las actualizaciones**.

2.  En el panel central junto a **aprobación**, seleccione el estado de aprobación deseado y, junto a **estado** seleccione el estado de instalación deseada. Haz clic en **Actualizar**.

#### <a name="to-create-a-new-update-view-on-wsus"></a>Para crear una nueva vista de actualización en WSUS

1.  En la consola de administración de WSUS, expanda **actualizaciones**y, a continuación, haga clic en **todas las actualizaciones**.

2.  En el **acciones** panel, haga clic en **nueva vista de actualización**.

3.  En el **agregar vista actualizar** ventana, en **paso 1: seleccionar propiedades**, seleccione las propiedades que necesita para filtrar la vista de actualización:

    -   Las actualizaciones seleccionadas están en una clasificación específica para filtrar las actualizaciones que pertenecen a uno o más clasificaciones de actualizaciones.

    -   Seleccione las actualizaciones son para que un producto específico filtrar las actualizaciones para uno o varios productos o familias de productos.

    -   Seleccione las actualizaciones se aprueban para que un grupo específico filtrar según las actualizaciones aprobadas para uno o varios grupos de equipos.

    -   Seleccione las actualizaciones se sincronizan en un período de tiempo concreto para filtrar según las actualizaciones sincronizadas en un momento determinado.

    -   Seleccione las actualizaciones son actualizaciones WSUS para filtrar las actualizaciones de WSUS.

4.  En **paso 2: editar las propiedades**, haga clic en las palabras subrayadas para elegir los valores que desee.

5.  Bajo **paso 3: Especifique un nombre**, asigne un nombre a la nueva vista.

6.  Haga clic en **Aceptar**.

La nueva vista aparecerá en el panel de vista de árbol en las actualizaciones. Se mostrará, como las vistas estándar en el panel central cuando lo selecciona.

#### <a name="to-search-for-an-update"></a>Para buscar una actualización

1.  Seleccione el **actualizaciones** nodo (o cualquier nodo situado debajo de él).

2.  En el **acciones** panel, haga clic en **búsqueda**.

3.  En el **búsqueda** ventana, en el **actualizaciones** ficha, especifique los criterios de búsqueda. Puede usar el texto de la **título**, **descripción**, y **número de artículo de Microsoft Knowledge Base (KB)** campos. Cada uno de estos elementos es una propiedad que se muestran en el **detalles** ficha en las propiedades de actualización.

#### <a name="to-view-the-properties-for-an-update"></a>Para ver las propiedades de una actualización

1.  En la consola de administración de WSUS, expanda **actualizaciones**y, a continuación, haga clic en **todas las actualizaciones**.

2.  En la lista de actualizaciones, haga clic en la actualización que desea ver.

3.  En el panel inferior, verá las secciones de propiedad diferentes:

    -   La barra de título muestra el título de la actualización; Por ejemplo, la actualización para Windows Media Player 9 de seguridad (KB911565).

    -   La sección Estado muestra el estado de instalación de la actualización (los equipos en el que debe instalarse, equipos en los que se instaló con errores, los equipos en los que se se ha instalado o no es aplicable y los equipos que no han notificado el estado de la actualización), así como información general (fecha de lanzamiento de números KB y MSRC, etcetera).

    -   La sección de descripción muestra una breve descripción de la actualización.

    -   La sección de detalles adicionales, muestra la siguiente información:

        -   El comportamiento de instalación de la actualización (o no es extraíble, solicita un reinicio, requiere la intervención del usuario o debe instalarse exclusivamente).

        -   Si tiene o no la actualización de los términos de licencia de Software de Microsoft

        -   Los productos a los que se aplica la actualización

        -   Las actualizaciones que sustituyen a esta actualización

        -   Las actualizaciones que sustituye esta actualización

        -   Los idiomas admitidos por la actualización

        -   El Id. de actualización

Tenga en cuenta que puede realizar este procedimiento en solo una actualización a la vez. Si selecciona varias actualizaciones, solo la primera actualización de la lista se mostrará en el **propiedades** panel.

## <a name="managing-updates-with-wsus"></a>Administración de actualizaciones con WSUS
Las actualizaciones se utilizan para actualizar o proporcionar un reemplazo completo para el software que está instalado en un equipo. Todas las actualizaciones que está disponible en Microsoft Update se compone de dos componentes:

-   Metadata: Proporciona información acerca de la actualización. Por ejemplo, los metadatos suministran información para las propiedades de una actualización, lo que permite averiguar para qué es útil la actualización. Los metadatos incluyen también los términos de licencia del Software de Microsoft. El paquete de metadatos descargado para una actualización es normalmente mucho menor que el paquete de archivos de actualización real.

-   Archivos de actualización: Los archivos reales de necesarios para instalar una actualización en un equipo.

Cuando se sincronizan las actualizaciones con su servidor WSUS, los archivos de actualización y los metadatos se almacenan en dos ubicaciones independientes. Los metadatos se almacenan en la base de datos de WSUS. Los archivos de actualización pueden almacenarse en el servidor WSUS o en servidores de Microsoft Update, dependiendo de cómo haya configurado las opciones de sincronización. Si decide almacenar los archivos de actualización en servidores de Microsoft Update, sólo se descargan los metadatos en el momento de la sincronización; aprobar las actualizaciones a través de la consola de WSUS y, a continuación, los equipos cliente obtención los archivos de actualización directamente desde Microsoft Update en el momento de la instalación. Para obtener más información acerca de las opciones para almacenar las actualizaciones, consulte la sección [1.3. Elegir una estrategia de almacenamiento WSUS](../plan/plan-your-wsus-deployment.md#BKMK_1.3.) del paso 1: Preparar la implementación de WSUS, en la Guía de implementación de WSUS.

Va a configurar y ejecutar sincronizaciones, adición de equipos y grupos de equipos y la implementación de actualizaciones de forma periódica. En la lista siguiente se proporciona ejemplos de tareas generales que puede llevar a cabo en la actualización de los equipos con WSUS.

-   Determinar un plan general de administración de actualización según la topología de red y ancho de banda, las necesidades de la empresa y estructura organizativa.

    -   Si se configura una jerarquía de servidores WSUS y cómo se debe estructurar la jerarquía.

    -   ¿Qué equipo agrupa para crear y cómo asignar equipos a ellos (destinatarios del lado servidor o cliente).

    -   La base de datos que se usará para los metadatos de actualización (por ejemplo, Windows Internal Database, SQL Server).

    -   Si se deben sincronizar las actualizaciones automáticamente y en qué momento.

-   Establecer las opciones de sincronización, como origen de actualización, clasificación de productos y actualizaciones, idioma, configuración de conexión, ubicación de almacenamiento y programación de sincronización.

-   Obtenga las actualizaciones y los metadatos asociados en el servidor WSUS mediante la sincronización desde Microsoft Update o un servidor WSUS ascendente.

-   Aprobar o rechazar las actualizaciones. Tiene la opción de permitir que los usuarios para instalar las actualizaciones mismas (si son administradores locales en sus equipos cliente).

-   Configurar aprobaciones automáticas. También puede configurar si desea habilitar la aprobación automática de revisiones de actualizaciones existentes o aprobar manualmente las revisiones. Si decide aprobar manualmente las revisiones, el servidor WSUS continuará usando la versión más antigua hasta que se apruebe manualmente la nueva revisión.

-   Compruebe el estado de las actualizaciones. Puede ver el estado de actualización, imprimir un informe de estado o configurar el correo electrónico para los informes de estado periódicas.

## <a name="update-products-and-classifications"></a>Actualizar productos y clasificaciones
Las actualizaciones disponibles en Microsoft Update se diferencian por producto (o familia de productos) y la clasificación.

Un producto es una edición específica de un sistema operativo o la aplicación, por ejemplo, Windows Server 2012. Una familia de productos es el sistema operativo o la aplicación de base de los cuales se derivan los productos individuales. Un ejemplo de una familia de productos es Microsoft Windows, de los cuales Windows Server 2012 es un miembro. Puede seleccionar los productos o familias de productos para el que desea que el servidor para sincronizar las actualizaciones. Puede especificar una familia de productos o productos individuales dentro de la familia. Selección de cualquier producto o familia de productos, obtendrá las actualizaciones de versiones actuales y futuras del producto.

Clasificaciones de actualización representan el tipo de actualización. Para cualquier determinado producto o familia de productos, las actualizaciones podrían estar disponibles entre varias clasificaciones de actualización (por ejemplo, la familia de Windows 7 actualizaciones críticas y actualizaciones de seguridad). En la tabla siguiente se enumera las clasificaciones de actualizaciones.

| Clasificaciones de actualizaciones  | Descripción   |
|--|--|
|Actualizaciones críticas|Correcciones para problemas específicos que solucionan los errores relacionados con no de seguridad críticos.|
|Actualizaciones de definiciones|Actualizaciones de virus u otros archivos de definición.|
|Controladores|Componentes de software diseñados para admitir el nuevo hardware.|
|Feature packs de|Nuevas versiones de características, normalmente se acumulan en los productos en la próxima versión.|
|Actualizaciones de seguridad|Correcciones de productos específicos, problemas de seguridad.|
|Service Packs|Conjuntos acumulativos de todas las revisiones, actualizaciones de seguridad, actualizaciones críticas y actualizaciones creadas desde el lanzamiento del producto. Los Service packs también podrían contener un número limitado de características o cambios de diseño solicitadas por el cliente.|
|Herramientas|Utilidades o características que ayudan a llevar a cabo una tarea o un conjunto de tareas.|
|Paquetes acumulativos de actualizaciones|Un conjunto acumulativo de revisiones, actualizaciones de seguridad, actualizaciones críticas y otras actualizaciones que se recopilan para facilitar la implementación. Un paquete acumulativo de actualizaciones por lo general tiene como destino un área específica, como seguridad o un componente específico, como Internet Information Services (IIS).|
|Actualizaciones|Correcciones para problemas específicos que solucionan errores relacionados que no son críticas, no de seguridad.|

## <a name="icons-used-for-updates-in-windows-server-update-services"></a>Iconos que se usan para las actualizaciones de Windows Server Update Services
 Actualizaciones en WSUS se representan mediante uno de los siguientes iconos.  
 Para ver estos iconos, deberá habilitar la columna de sustitución en la consola Update Services.
 
### <a name="no-icon"></a>Sin icono
 La actualización no tiene ninguna relación de sustitución con cualquier otra actualización.

 **Preocupaciones operativas:**  

 No hay ninguna preocupación operativa.  
 
### <a name="superseding-icon"></a>Icono de sustitución
 ![icono](../../media/wsus/wsus-superseding.png) Esta actualización sustituye a otras actualizaciones.

 **Preocupaciones operativas:**  

 No hay ninguna preocupación operativa.  

### <a name="superseded--superseding-icon"></a>Icono reemplazo y reemplazada
 ![icono](../../media/wsus/wsus-superseded.png) Esta actualización se ha reemplazado por otra actualización y reemplaza a otras actualizaciones.

 **Preocupaciones operativas:**  

 Reemplazar estas actualizaciones con las actualizaciones reemplaza cuando sea posible.
 
### <a name="superseded-icon"></a>Icono de sustituido
 ![icono](../../media/wsus/wsus-superseded-leaf.png) Esta actualización es reemplazada por otra actualización.

 **Preocupaciones operativas:**  

 Reemplazar estas actualizaciones con las actualizaciones reemplaza cuando sea posible.
