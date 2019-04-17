---
title: Crear un filtro de archivos
description: "En este artículo se describe cómo crear un filtro de archivos"
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c1f261eb926eca3ead58b87aeb00a5060b9d957c
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="create-a-file-screen"></a>Crear un filtro de archivos

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Al crear un nuevo filtro de archivos, puedes elegir guardar una plantilla de filtro de archivos que se base en las propiedades personalizadas del filtro de archivos que definas. La ventaja de esto es que se mantiene un vínculo entre los filtros de archivos y la plantilla que se usa para crearlos, de modo que en el futuro, los cambios que se realicen en la plantilla pueden aplicarse a todos los filtros de archivos que deriven de ella. Se trata de una característica que simplifica la implementación de los cambios de las directivas de almacenamiento, ya que ofrece un punto central donde se pueden llevar a cabo todas las actualizaciones.

## <a name="to-create-a-file-screen-with-custom-properties"></a>Para crear un filtro de archivos con las propiedades personalizadas

1.  En **Administración del filtrado de archivos**, haz clic en el nodo **Filtros de archivos**.

2.  Haz clic con el botón derecho en **Filtros de archivos** y haz clic en **Crear filtro de archivos** (o selecciona **Crear filtro de archivos** en el panel **Acciones**). Se abrirá el cuadro de diálogo **Crear filtro de archivos**.

3.  En **Ruta de acceso al filtro de archivos**, escribe el nombre o busca la carpeta a la que se aplicará el filtro de archivos. El filtro de archivos se aplicará a la carpeta seleccionada y a todas sus subcarpetas.

4.  En **¿Cómo desea configurar las propiedades del filtro de archivos?**, haz clic en **Definir propiedades personalizadas del filtro de archivos** y luego haz clic en **Propiedades personalizadas**. Se abrirá el cuadro de diálogo **Propiedades del filtro de archivos**.

5.  Si quieres copiar las propiedades de una plantilla existente para usarla como base del filtro de archivos, selecciona una plantilla de la lista desplegable **Copiar propiedades de la plantilla**. Luego haz clic en **Copiar**.

    En el cuadro de diálogo **Propiedades del filtro de archivos**, modifica o establece los siguientes valores en la pestaña **Configuración**:

6.  En **Tipo de filtrado**, haz clic en las opciones **Filtrado activo** o **Filtrado pasivo**. (El filtrado activo impide que los usuarios guarden archivos que formen parte de los grupos de archivos bloqueados y genera notificaciones cuando los usuarios intentan guardar archivos no autorizados. El filtrado pasivo envía notificaciones configuradas, pero no impide que los usuarios guarden archivos).

7.  En **Grupos de archivos**, selecciona cada grupo de archivos que quieres incluir en el filtro de archivos. (Para seleccionar la casilla de verificación del grupo de archivos, haz doble clic en la etiqueta de grupo de archivos).

    Si quieres ver los tipos de archivo que un grupo de archivos incluye y excluye, haz clic en la etiqueta de grupo de archivos y luego haz clic en **Editar**. Para crear un nuevo grupo de archivos, haz clic en **Crear**.

8.  También puedes configurar el **Administrador de recursos del servidor de archivos** para generar una o varias notificaciones si estableces las opciones de las pestañas **Mensaje de correo electrónico**, **Registro de eventos**, **Comando** e **Informe**. Para obtener más información sobre las opciones de notificación de filtros de archivos, consulta [Crear una plantilla de filtro de archivos](create-file-screen-template.md).

9.  Después de seleccionar todas las propiedades del filtro de archivos que quieres usar, haz clic en **Aceptar** para cerrar el cuadro de diálogo **Propiedades del filtro de archivos**.

10. En el cuadro de diálogo **Crear filtro de archivos**, haz clic en **Crear** para guardar el filtro de archivos. Se abrirá el cuadro de diálogo **Guardar las propiedades personalizadas como plantilla**.

11. Selecciona el tipo de filtro personalizado de archivos que quieres crear:

    -   Para guardar una plantilla que se base en estas propiedades personalizadas (recomendado), haz clic en **Guardar las propiedades personalizadas como plantilla** y escribe un nombre para la plantilla. Esta opción aplicará la plantilla al nuevo filtro de archivos y puedes usar la plantilla para crear filtros de archivos adicionales en el futuro. Esto te permitirá actualizar automáticamente más adelante los filtros de archivos mediante la actualización de la plantilla.
    -   Si no quieres guardar una plantilla al guardar el filtro de archivos, haz clic en **Guardar el filtro de archivos personalizado sin crear una plantilla**.

12. Haz clic en **Aceptar**.

## <a name="see-also"></a>Consulta también

-   [Administración del filtrado de archivos](file-screening-management.md)
-   [Definir grupos de archivos para el filtrado](define-file-groups-for-screening.md)
-   [Crear una plantilla de filtro de archivos](create-file-screen-template.md)
-   [Editar las propiedades de la plantilla de filtro de archivos](edit-file-screen-template-properties.md)


