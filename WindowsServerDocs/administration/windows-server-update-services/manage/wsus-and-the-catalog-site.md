---
title: WSUS y el sitio del catálogo
description: 'Tema de Windows Server Update Service (WSUS): Cómo importar revisiones en WSUS accediendo al sitio del catálogo de Microsoft Update'
ms.prod: windows-server
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
ms.openlocfilehash: c30fad5f38a1b3088c6b4d12ac92d8b8a15aee83
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361452"
---
# <a name="wsus-and-the-catalog-site"></a>WSUS y el sitio del catálogo

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El sitio del catálogo es la ubicación de Microsoft desde la que puede importar revisiones y controladores de hardware.

## <a name="the-microsoft-update-catalog-site"></a>Sitio del catálogo de Microsoft Update
Para importar revisiones en WSUS, debe tener acceso al sitio del catálogo de Microsoft Update desde un equipo WSUS. Cualquier equipo que tenga instalada la consola de administración de WSUS, ya sea un servidor WSUS o no, se puede usar para importar revisiones desde el sitio del catálogo. Debe haber iniciado sesión en el equipo como administrador para importar las revisiones.

#### <a name="to-access-the-microsoft-update-catalog-site"></a>Para tener acceso al sitio del catálogo de Microsoft Update

1.  En la consola de administración de WSUS, seleccione el nodo de servidor superior o **actualizaciones**y, en el panel **acciones** , haga clic en **importar actualizaciones**. Se abrirá una ventana del explorador en el sitio web del catálogo de Microsoft Update.

2.  Para obtener acceso a las actualizaciones en este sitio, debe instalar el control activeX del catálogo de Microsoft Update.

3.  Puede examinar este sitio para ver las revisiones de Windows y los controladores de hardware. Cuando haya encontrado los que desee, agréguelos a la cesta.

4.  Cuando haya terminado de examinar, vaya a la cesta y haga clic en importar para importar las actualizaciones. Para descargar las actualizaciones sin importarlas, desactive la casilla **importar directamente en Windows Server Update Services** .

Las actualizaciones aprobadas importadas desde el sitio del catálogo de Microsoft Update se descargan la próxima vez que se sincronice el servidor WSUS. No se descargan en el momento de la importación desde el sitio del catálogo de Microsoft Update.

Tenga en cuenta que debe tener acceso al sitio del catálogo de Microsoft Update a través de la consola de WSUS para asegurarse de que las actualizaciones se importan en un formato compatible con WSUS. Si tiene acceso al sitio web del catálogo de Microsoft Update manualmente, las actualizaciones que descargue no se importan en el servidor WSUS, sino que se descargan como individuales *. Archivos MSU. WSUS no tiene actualmente un mecanismo compatible para la importación de archivos en el \*. Formato MSU.

Si ejecuta el Asistente para la limpieza del servidor, las actualizaciones importadas del catálogo de Microsoft Update que están establecidas como no aprobadas o rechazadas pueden quitarse del servidor WSUS. Si se quitan, se pueden volver a importar desde el catálogo de Microsoft Update.

> [!NOTE]
> Puede quitar las actualizaciones que se importan desde el catálogo de Microsoft Update que están establecidas como no aprobadas o rechazadas, mediante la ejecución del Asistente para la limpieza del servidor WSUS. Puede volver a importar las actualizaciones que se han quitado previamente de los sistemas WSUS a través del catálogo de Microsoft Update.

## <a name="restricting-access-to-hotfixes"></a>Restringir el acceso a las revisiones
Los administradores de WSUS pueden considerar la posibilidad de restringir el acceso a las revisiones que han descargado del sitio de catálogo de Microsoft Update. Para realizar esta restricción, siga estos pasos:

#### <a name="to-restrict-access-to-hotfixes"></a>Para restringir el acceso a las revisiones

1.  Habilite la autenticación de Windows en la raíz de contenido de IIS.

    -   Inicie el administrador de IIS.

    -   Navegue hasta el nodo de contenido en el sitio web de administración de WSUS.

    -   En el panel de **Inicio de contenido** , haga doble clic en la opción **autenticación** .

    -   Seleccione **autenticación anónima** y haga clic en **deshabilitar** en el panel **acciones** de la derecha.

    -   Seleccione **autenticación de Windows** y haga clic en **Habilitar** en el panel **acciones** de la derecha.

2.  Cree un grupo de destino de WSUS para los equipos que necesitan la revisión y agréguelos al grupo. Para obtener más información acerca de los equipos y grupos, consulte [Administración de equipos cliente de WSUS y grupos](managing-wsus-client-computers-and-wsus-computer-groups.md) de equipos WSUS en esta guía, y la sección [3,3. Configure los grupos de equipos de WSUS @ no__t-0 del paso 3: Configure WSUS en la guía de implementación de WSUS.

3.  Descargue los archivos de la revisión.

4.  Establezca los permisos de estos archivos para que solo las cuentas de equipo de esos equipos puedan leerlos. También necesitará permitir el acceso total a la cuenta de servicio de red a los archivos.

5.  Apruebe la revisión para el grupo de destino de WSUS creado en el paso 2.

## <a name="importing-updates-in-different-languages"></a>Importar actualizaciones en distintos idiomas
El sitio web del catálogo de Microsoft Update incluye actualizaciones que admiten varios idiomas. Es muy **importante** que coincida con los idiomas admitidos por el servidor WSUS con los idiomas admitidos por estas actualizaciones. Si el servidor WSUS no admite todos los idiomas incluidos en la actualización, la actualización no se implementará en los equipos cliente. Del mismo modo, si se ha descargado una actualización que admite varios idiomas en el servidor WSUS pero aún no se ha implementado en los equipos cliente, y un administrador anula la selección de uno de los idiomas incluidos en la actualización, la actualización no se implementará en los clientes.
