---
title: WSUS y el sitio del catálogo
description: 'Tema de Windows Server Update Service (WSUS): cómo importar revisiones en WSUS accediendo el sitio del catálogo de Microsoft Update'
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f19a8659-5a96-4fdd-a052-29e4547fe51a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7cced9a930b63baa79b4addb429c562c38d6da01
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843886"
---
# <a name="wsus-and-the-catalog-site"></a>WSUS y el sitio del catálogo

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El sitio del catálogo es la ubicación de Microsoft desde el que puede importar las revisiones y controladores de hardware.

## <a name="the-microsoft-update-catalog-site"></a>El sitio del catálogo de Microsoft Update
Para importar revisiones en WSUS, debe tener acceso el sitio del catálogo de Microsoft Update desde un equipo WSUS. Cualquier equipo que tenga la consola de administración de WSUS instalada, si es un servidor WSUS, o no se puede usar para importar revisiones desde el sitio del catálogo. Debe estar iniciado en el equipo como administrador para importar las revisiones.

#### <a name="to-access-the-microsoft-update-catalog-site"></a>Para acceder al sitio de catálogo de Microsoft Update

1.  En la consola de administración de WSUS, seleccione el nodo superior del servidor o **actualizaciones**y en el **acciones** panel, haga clic **importar actualizaciones**. Se abrirá una ventana del explorador en el sitio Web del catálogo de Microsoft Update.

2.  Para tener acceso a las actualizaciones de este sitio, debe instalar el control activeX de catálogo de Microsoft Update.

3.  Puede examinar este sitio para los controladores de hardware y las revisiones de Windows. Cuando haya encontrado los que desee, agregarlos a la cesta.

4.  Cuando haya terminado de exploración, vaya a la cesta y haga clic en Importar para importar las actualizaciones. Para descargar las actualizaciones sin importarlos, desactive la **importar directamente en Windows Server Update Services** casilla de verificación.

Actualizaciones aprobadas que se importan desde el sitio de catálogo de Microsoft Update se descargan la próxima vez que el servidor WSUS sincroniza. No se descargan en el momento de importar desde el sitio del catálogo de Microsoft Update.

Tenga en cuenta que debe tener acceso el sitio del catálogo de Microsoft Update aunque la consola de WSUS para asegurarse de que las actualizaciones se importan en un formato compatible con WSUS. Si tiene acceso manualmente el sitio Web del catálogo de Microsoft Update, las actualizaciones que descargan no se importan en el servidor WSUS, pero en su lugar, se descargan como individuo *. Archivos MSU. WSUS no tiene actualmente un mecanismo compatible para la importación de archivos en el \*. Formato MSU.

Si ejecuta el Asistente Liberador de espacio en el servidor, pueden quitarse actualizaciones importadas desde el catálogo de Microsoft Update que se establecen como no aprobado o rechazado desde el servidor WSUS. Si se quitan, se pueden volver a importar el catálogo de Microsoft Update.

> [!NOTE]
> Puede quitar las actualizaciones que se importan desde el catálogo de Microsoft Update que se establecen como no aprobado o rechazado, mediante la ejecución de la limpieza del servidor de WSUS de asistente. Puede volver a importar las actualizaciones que se han quitado previamente de los sistemas WSUS a través del catálogo de Microsoft Update.

## <a name="restricting-access-to-hotfixes"></a>Restringir el acceso a las revisiones
Los administradores WSUS es posible que considere la posibilidad de restringir el acceso a las revisiones que han descargado desde el sitio del catálogo de Microsoft Update. Para hacer que esta restricción siga los pasos siguientes:

#### <a name="to-restrict-access-to-hotfixes"></a>Para restringir el acceso a las revisiones

1.  Habilitar la autenticación de Windows en la raíz virtual del contenido de IIS.

    -   Inicie el Administrador de IIS.

    -   Navegue hasta el nodo de contenido en el sitio web de administración de WSUS.

    -   En el **contenido principal** panel, haga doble clic en el **autenticación** opción.

    -   Seleccione **autenticación anónima** y haga clic en **deshabilitar** en el **acciones** panel de la derecha.

    -   Seleccione **Windows autenticación** y haga clic en **habilitar** en el **acciones** panel de la derecha.

2.  Cree un grupo de destino WSUS para los equipos que necesitan la revisión y agregarlos al grupo. Para obtener más información acerca de los equipos y grupos, consulte [equipos de administración de cliente de WSUS y grupos de equipos WSUS](managing-wsus-client-computers-and-wsus-computer-groups.md) en esta sección y guía [3.3. Configurar grupos de equipos WSUS](../deploy/2-configure-wsus.md#BKMK_ConfigcomputerGroups) del paso 3: Configurar WSUS, en la Guía de implementación de WSUS.

3.  Descargar los archivos de la revisión.

4.  Establezca los permisos de estos archivos para que solo las cuentas de equipo de esas máquinas pueden leerlas. También deberá permitir el acceso completo de cuenta de servicio de red a los archivos.

5.  Aprobar la revisión para el grupo de destino WSUS creado en el paso 2.

## <a name="importing-updates-in-different-languages"></a>Importación de actualizaciones en distintos idiomas
El sitio Web del catálogo de Microsoft Update incluye las actualizaciones que admiten varios idiomas. Es muy **importante** para que coincida con los idiomas admitidos por el servidor WSUS con los lenguajes compatibles con las actualizaciones. Si el servidor WSUS no es compatible con todos los idiomas incluidos en la actualización, la actualización no se implementará en los equipos cliente. Del mismo modo, si se ha descargado en el servidor WSUS pero aún no se ha implementado en los equipos cliente una actualización de compatibilidad con varios idiomas, y anula la selección de un administrador uno de los lenguajes incluye la actualización, la actualización no se implementará en los clientes.
