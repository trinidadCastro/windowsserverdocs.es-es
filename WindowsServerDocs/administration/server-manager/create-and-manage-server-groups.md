---
title: crear y administrar grupos de servidores
description: Administrador de servidores
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9d5b1be8-49fd-4ff7-9580-e4ff21fe4b17
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 32e20040e2cb075e447c0d03d48676c7011a5a92
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889486"
---
# <a name="create-and-manage-server-groups"></a>crear y administrar grupos de servidores

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema describe cómo crear grupos de servidores personalizados definidos por el usuario en Administrador del servidor en Windows Server.

## <a name="BKMK_groups"></a>Grupos de servidores
Los servidores que agregue al grupo de servidores se muestran en el **todos los servidores** página en el administrador del servidor. Puede crear grupos de servidores personalizados que haya agregado. Grupos de servidores le permiten ver y administrar un subconjunto pequeño del grupo de servidores como una unidad lógica; Por ejemplo, puede crear un grupo denominado **servidores de contabilidad** para todos los servidores de la organización del departamento de contabilidad, o un grupo denominado **Chicago** para todos los servidores que se ubican geográficamente en Chicago. Después de crear un grupo de servidores, la página del grupo principal en el administrador del servidor muestra información acerca de los eventos, servicios, contadores de rendimiento, resultados del analizador de procedimientos recomendados e instalado roles y características para el grupo como un todo.

Los servidores pueden ser miembros de más de un grupo.

#### <a name="to-create-a-new-server-group"></a>Para crear un nuevo grupo de servidores

1.  En el **administrar** menú, haga clic en **crear grupo de servidores**.

2.  En el cuadro de texto **Nombre de grupo de servidores** , escriba un nombre descriptivo para el grupo de servidores, como **Servidores de cuentas**.

3.  Agregar servidores a la **seleccionado** lista del grupo de servidores o agregar otros servidores al grupo mediante la **active directory**, **DNS**, o **importar**pestañas. Para obtener más información acerca de cómo usar estas pestañas, consulte [agregar servidores al administrador del servidor](add-servers-to-server-manager.md) en esta guía.

4.  Cuando haya terminado de agregar servidores, haga clic en **Aceptar**. El nuevo grupo se muestra en el panel de navegación del administrador del servidor en el **todos los servidores** grupo.

#### <a name="to-edit-an-existing-server-group"></a>Para editar un grupo existente de servidores

1.  Realice una de las siguientes acciones:

    -   En el panel de navegación del administrador del servidor, haga clic en un grupo de servidores y, a continuación, haga clic en **Editar grupo de servidores**.

    -   En la página principal del grupo de servidores, abra el **tareas** menú en el **servidores** icono y, a continuación, haga clic en **Editar grupo de servidores**.

2.  cambiar el nombre del grupo, o agregar o quitar servidores del grupo.

    > [!NOTE]
    > eliminación de servidores de un grupo de servidores no elimina servidores del administrador del servidor. Los servidores que elimina de un grupo se mantienen en el grupo **Todos los servidores** , en el grupo de servidores.

3.  Cuando haya terminado de realizar cambios en el grupo, haga clic en **Aceptar**.

#### <a name="to-delete-an-existing-server-group"></a>Para eliminar un grupo existente de servidores

1.  Realice una de las siguientes acciones:

    -   En el panel de navegación del administrador del servidor, haga clic en un grupo de servidores y, a continuación, haga clic en **eliminar grupo de servidores**.

    -   En la página principal del grupo de servidores, abra el **tareas** menú en el **servidores** icono y, a continuación, haga clic en **eliminar grupo de servidores**.

2.  Cuando se le pregunte si está seguro de eliminar el grupo de servidores, haga clic en **Sí**.

    > [!NOTE]
    > eliminar un grupo de servidores no elimina servidores del administrador del servidor. Los servidores que se encontraban en un grupo eliminado se mantienen en el grupo **Todos los servidores** , en el grupo de servidores.

3.  Cuando haya terminado de realizar cambios en el grupo, haga clic en **Aceptar**.

## <a name="see-also"></a>Vea también
[Agregar servidores al administrador del servidor](add-servers-to-server-manager.md)
[administrador del servidor](server-manager.md)



