---
title: Visualización y administración de actualizaciones
description: 'Tema de Windows Server Update Service (WSUS): visualización y administración de actualizaciones en la consola de WSUS'
ms.prod: windows-server
ms.technology: manage-wsus
ms.topic: article
ms.assetid: ac70192b-0309-4385-b697-2e8eda51911c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a2a9f7e1f1f3f648a0cba22d599ccc64e7b424d8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80828448"
---
# <a name="viewing-and-managing-updates"></a>Visualización y administración de actualizaciones

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puede usar la consola de WSUS para ver y administrar las actualizaciones.

## <a name="viewing-updates"></a>Ver actualizaciones
En la página **actualizaciones** , puede hacer lo siguiente:

-   Ver actualizaciones. La información general de la actualización muestra las actualizaciones que se han sincronizado desde el origen de la actualización al servidor WSUS y están disponibles para su aprobación.

-   Filtrar actualizaciones. En la vista predeterminada, puede filtrar las actualizaciones por estado de aprobación y estado de instalación. La configuración predeterminada es para las actualizaciones no aprobadas que necesitan algunos clientes o que han tenido errores de instalación en algunos clientes. Puede cambiar esta vista cambiando los filtros estado de aprobación y estado de la instalación y, a continuación, haciendo clic en **Actualizar**.

-   Crear nuevas vistas de actualización. En el panel **acciones** , haga clic en **nueva vista de actualización**. Puede filtrar las actualizaciones por clasificación, producto, grupo para el que se han aprobado y fecha de sincronización. Para ordenar la lista, haga clic en el encabezado de columna correspondiente en la barra de título.

-   Buscar actualizaciones. Puede buscar una actualización individual o un conjunto de actualizaciones por título, descripción, artículo de Knowledge base o el número del centro de respuesta de seguridad de Microsoft para la actualización.

-   Vea los detalles, el estado y el historial de revisiones de cada actualización.

-   Aprobar y rechazar actualizaciones.

#### <a name="to-view-updates"></a>Para ver las actualizaciones

1.  En la consola de administración de WSUS, expanda el nodo actualizaciones y, a continuación, haga clic en todas las actualizaciones.

2.  De forma predeterminada, las actualizaciones se muestran con su título, clasificación, porcentaje instalado/no aplicable y estado de aprobación. Si desea mostrar más propiedades de actualización o distintas, haga clic con el botón secundario en la barra de encabezado de columna y seleccione las columnas adecuadas.

3.  Para ordenar por criterios diferentes, como estado de descarga, título, clasificación, fecha de lanzamiento o estado de aprobación, haga clic en el encabezado de columna correspondiente.

#### <a name="to-filter-the-list-of-updates-displayed-on-the-updates-page"></a>Para filtrar la lista de actualizaciones que se muestran en la página actualizaciones

1.  En la consola de administración de WSUS, expanda **actualizaciones**y, a continuación, haga clic en **todas las actualizaciones**.

2.  En el panel central junto a **aprobación**, seleccione el estado de aprobación deseado y, junto a **Estado** , seleccione el estado de la instalación que desee. Haz clic en **Actualizar**.

#### <a name="to-create-a-new-update-view-on-wsus"></a>Para crear una nueva vista de actualización en WSUS

1.  En la consola de administración de WSUS, expanda **actualizaciones**y, a continuación, haga clic en **todas las actualizaciones**.

2.  En el panel **acciones** , haga clic en **nueva vista de actualización**.

3.  En la ventana **Agregar vista de actualización** , en **paso 1: seleccionar propiedades**, seleccione las propiedades que necesita para filtrar la vista de actualización:

    -   Seleccione las actualizaciones están en una clasificación específica para filtrar las actualizaciones que pertenecen a una o más clasificaciones de actualización.

    -   Seleccione las actualizaciones para un producto específico para filtrar las actualizaciones de uno o varios productos o familias de productos.

    -   Seleccione se aprueban las actualizaciones para un grupo específico para filtrar según las actualizaciones aprobadas para uno o más grupos de equipos.

    -   Seleccione las actualizaciones se sincronizaron dentro de un período de tiempo específico para filtrar por las actualizaciones sincronizadas en un momento determinado.

    -   Seleccione las actualizaciones son actualizaciones de WSUS para filtrar en las actualizaciones de WSUS.

4.  En **paso 2: editar las propiedades**, haga clic en las palabras subrayadas para seleccionar los valores que desee.

5.  En **paso 3: especifique un nombre**, asigne un nombre a la nueva vista.

6.  Haga clic en **Aceptar**.

La nueva vista aparecerá en el panel vista de árbol en actualizaciones. Se mostrará, como las vistas estándar, en el panel central al seleccionarlo.

#### <a name="to-search-for-an-update"></a>Para buscar una actualización

1.  Seleccione el nodo **actualizaciones** (o cualquier nodo situado debajo).

2.  En el panel **acciones** , haga clic en **Buscar**.

3.  En la ventana **Buscar** , en la pestaña **actualizaciones** , escriba los criterios de búsqueda. Puede usar el texto de los campos **título**, **Descripción**y **número de artículo de Microsoft Knowledge base (KB)** . Cada uno de estos elementos es una propiedad que se muestra en la pestaña **detalles** de las propiedades de la actualización.

#### <a name="to-view-the-properties-for-an-update"></a>Para ver las propiedades de una actualización

1.  En la consola de administración de WSUS, expanda **actualizaciones**y, a continuación, haga clic en **todas las actualizaciones**.

2.  En la lista de actualizaciones, haga clic en la actualización que desea ver.

3.  En el panel inferior, verá las distintas secciones de propiedades:

    -   La barra de título muestra el título de la actualización. por ejemplo, la actualización de seguridad para Windows Media Player 9 (KB911565).

    -   La sección estado muestra el estado de instalación de la actualización (los equipos en los que se debe instalar, los equipos en los que se instaló con errores, los equipos en los que se ha instalado o no es aplicable, y los equipos que no han comunicado el estado de la actualización), así como información general (fecha de lanzamiento de los números de KB y MSRC, etc.).

    -   La sección Descripción muestra una breve descripción de la actualización.

    -   La sección detalles adicionales muestra la siguiente información:

        -   El comportamiento de la instalación de la actualización (ya sea extraíble o no, solicita un reinicio, requiere la intervención del usuario o debe instalarse exclusivamente).

        -   Si la actualización tiene o no términos de licencia del software de Microsoft

        -   Productos a los que se aplica la actualización

        -   Actualizaciones que sustituyen a esta actualización

        -   Actualizaciones reemplazadas por esta actualización

        -   Los idiomas admitidos por la actualización

        -   El identificador de la actualización

Tenga en cuenta que este procedimiento solo se puede realizar en una actualización cada vez. Si selecciona varias actualizaciones, en el panel **propiedades** solo se mostrará la primera actualización de la lista.

## <a name="managing-updates-with-wsus"></a>Administración de actualizaciones con WSUS
Las actualizaciones se usan para actualizar o proporcionar una sustitución completa de archivos para el software instalado en un equipo. Cada actualización que está disponible en Microsoft Update se compone de dos componentes:

-   Metadata: proporciona información acerca de la actualización. Por ejemplo, los metadatos proporcionan información para las propiedades de una actualización, lo que te permite averiguar cuál es la utilidad de actualización. Los metadatos también incluyen términos de licencia del software de Microsoft. El paquete de metadatos descargado para una actualización suele tener un tamaño bastante menor que el del paquete de archivos de actualización real.

-   Archivos de actualización: los archivos reales necesarios para instalar una actualización en un equipo.

Cuando se sincronizan las actualizaciones con su servidor WSUS, los archivos de actualización y los metadatos se almacenan en dos ubicaciones independientes. Los metadatos se almacenan en la base de datos de WSUS. Los archivos de actualización se pueden almacenar en el servidor WSUS o en servidores de Microsoft Update, en función de cómo haya configurado las opciones de sincronización. Si decide almacenar los archivos de actualización en servidores de Microsoft Update, solo se descargan los metadatos en el momento de la sincronización; las actualizaciones se aprueban a través de la consola de WSUS y, a continuación, los equipos cliente obtienen los archivos de actualización directamente desde Microsoft Update en el momento de la instalación. Para obtener más información sobre las opciones de almacenamiento de actualizaciones, consulte la sección [1,3. Elija una estrategia de almacenamiento de WSUS](../plan/plan-your-wsus-deployment.md#13-choose-a-wsus-storage-strategy) de paso 1: preparación para la implementación de WSUS, en la guía de implementación de WSUS.

Podrá configurar y ejecutar sincronizaciones, agregar equipos y grupos de equipos e implementar actualizaciones de forma periódica. En la lista siguiente se proporcionan ejemplos de tareas generales que se pueden llevar a cabo en la actualización de equipos con WSUS.

-   Determine un plan general de administración de actualizaciones en función de la topología de red y el ancho de banda, las necesidades de la empresa y la estructura organizativa.

    -   Si se va a configurar una jerarquía de servidores WSUS y cómo se debe estructurar la jerarquía.

    -   Los grupos de equipos que se van a crear y cómo asignarles equipos (destinatarios del lado servidor o del lado cliente).

    -   Qué base de datos se va a usar para actualizar los metadatos (por ejemplo, Windows Internal Database, SQL Server).

    -   Si las actualizaciones se deben sincronizar automáticamente y en qué momento.

-   Establezca las opciones de sincronización, como el origen de la actualización, la clasificación del producto y la actualización, el idioma, la configuración de conexión, la ubicación de almacenamiento y la programación de sincronización.

-   Obtener las actualizaciones y los metadatos asociados en el servidor WSUS a través de la sincronización desde Microsoft Update o un servidor WSUS ascendente.

-   Aprobar o rechazar actualizaciones. Tiene la opción de permitir que los usuarios instalen las actualizaciones (si son administradores locales en sus equipos cliente).

-   Configurar aprobaciones automáticas. También puede configurar si desea habilitar la aprobación automática de las revisiones para las actualizaciones existentes o aprobar las revisiones manualmente. Si decide aprobar las revisiones manualmente, el servidor WSUS seguirá usando la versión anterior hasta que apruebe manualmente la nueva revisión.

-   Compruebe el estado de las actualizaciones. Puede ver el estado de las actualizaciones, imprimir un informe de estado o configurar el correo electrónico para los informes de estado normales.

## <a name="update-products-and-classifications"></a>Actualizar productos y clasificaciones
Las actualizaciones disponibles en Microsoft Update se diferencian en el producto (o la familia de productos) y la clasificación.

Un producto es una edición específica de un sistema operativo o una aplicación, por ejemplo, Windows Server 2012. Una familia de productos es el sistema operativo o la aplicación de base de los cuales se derivan los productos individuales. Un ejemplo de una familia de productos es Microsoft Windows, de la que Windows Server 2012 es miembro. Puede seleccionar los productos o familias de productos para los que desea que el servidor Sincronice las actualizaciones. Puede especificar una familia de productos o productos individuales dentro de la familia. La selección de cualquier producto o familia de productos recibirá actualizaciones de las versiones actuales y futuras del producto.

Las clasificaciones de actualizaciones representan el tipo de actualización. En el caso de una familia de productos o productos determinada, las actualizaciones pueden estar disponibles entre varias clasificaciones de actualización (por ejemplo, actualizaciones críticas de la familia de Windows 7 y actualizaciones de seguridad). En la tabla siguiente se enumeran las clasificaciones de actualizaciones.

| Clasificaciones de actualizaciones  | Descripción   |
|--|--|
|Actualizaciones críticas|Revisiones de amplia difusión para problemas específicos que solucionan errores críticos no relacionados con la seguridad.|
|Actualizaciones de definiciones|Actualizaciones de virus u otros archivos de definición.|
|Controladores|Componentes de software diseñados para admitir el nuevo hardware.|
|Paquetes de características|Nuevas versiones de características, que normalmente se han incorporado en productos en la próxima versión.|
|Actualizaciones de seguridad|Revisiones ampliamente publicadas para productos específicos que abordan los problemas de seguridad.|
|Service Packs|Conjuntos acumulativos de todas las revisiones, actualizaciones de seguridad, actualizaciones críticas y actualizaciones creadas desde el lanzamiento del producto. Los Service Pack también pueden contener un número limitado de características o cambios de diseño solicitados por el cliente.|
|Herramientas|Utilidades o características que ayudan a realizar una tarea o un conjunto de tareas.|
|Paquetes acumulativos de actualizaciones|Un conjunto acumulativo de revisiones, actualizaciones de seguridad, actualizaciones críticas y otras actualizaciones que se empaquetan para facilitar la implementación. Generalmente, un resumen tiene como destino un área específica, como la seguridad, o un componente específico, como Internet Information Services (IIS).|
|Actualizaciones|Revisiones de amplia difusión para problemas específicos que solucionan errores no críticos no relacionados con la seguridad.|

## <a name="icons-used-for-updates-in-windows-server-update-services"></a>Iconos usados para las actualizaciones en Windows Server Update Services
 Las actualizaciones en WSUS se representan mediante uno de los siguientes iconos.  
 Para ver estos iconos, debe habilitar la columna sustitución en la consola de Update Services.
 
### <a name="no-icon"></a>Sin icono
 La actualización no tiene ninguna relación de sustitución con ninguna otra actualización.

 **Preocupaciones operativas:**  

 No hay ningún problema operativo.  
 
### <a name="superseding-icon"></a>Icono de sustitución
 ![icono](../../media/wsus/wsus-superseding.png) Esta actualización sustituye a otras actualizaciones.

 **Preocupaciones operativas:**  

 No hay ningún problema operativo.  

### <a name="superseded--superseding-icon"></a>Icono de sustitución de & reemplazado
 ![icono](../../media/wsus/wsus-superseded.png) Esta actualización se sustituye por otra actualización y sustituye a otras actualizaciones.

 **Preocupaciones operativas:**  

 Reemplace estas actualizaciones por las actualizaciones de reemplazo cuando sea posible.
 
### <a name="superseded-icon"></a>Icono de reemplazo
 ![icono](../../media/wsus/wsus-superseded-leaf.png) Esta actualización se ha sustituido por otra actualización.

 **Preocupaciones operativas:**  

 Reemplace estas actualizaciones por las actualizaciones de reemplazo cuando sea posible.
