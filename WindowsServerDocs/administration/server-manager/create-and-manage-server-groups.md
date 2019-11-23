---
title: crear y administrar grupos de servidores
description: Administrador de servidores
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 74075f091cd707f6faf73567c1dce6f22a2f6753
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383221"
---
# <a name="create-and-manage-server-groups"></a>crear y administrar grupos de servidores

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se describe cómo crear grupos de servidores personalizados definidos por el usuario en Administrador del servidor en Windows Server.

## <a name="BKMK_groups"></a>Grupos de servidores
Los servidores que se agregan al grupo de servidores se muestran en la página **todos los servidores** de administrador del servidor. Puede crear grupos de servidores personalizados que haya agregado. Los grupos de servidores le permiten ver y administrar un subconjunto más pequeño del grupo de servidores como unidad lógica; por ejemplo, puede crear un grupo denominado **servidores de contabilidad** para todos los servidores del Departamento de contabilidad de su organización o un grupo denominado **Chicago** para todos los servidores ubicados geográficamente en Chicago. Después de crear un grupo de servidores, la Página principal del grupo en Administrador del servidor muestra información acerca de los eventos, servicios, contadores de rendimiento, Analizador de procedimientos recomendados resultados y roles y características instalados para el grupo en su conjunto.

Los servidores pueden ser miembros de más de un grupo.

#### <a name="to-create-a-new-server-group"></a>Para crear un nuevo grupo de servidores

1.  En el menú **administrar** , haga clic en **Crear grupo de servidores**.

2.  En el cuadro de texto **Nombre de grupo de servidores** , escriba un nombre descriptivo para el grupo de servidores, como **Servidores de cuentas**.

3.  Agregue servidores a la lista **seleccionada** desde el grupo de servidores o agregue otros servidores al grupo mediante las pestañas **Active Directory**, **DNS**o **importar** . Para obtener más información sobre cómo usar estas pestañas, consulte [agregar servidores a administrador del servidor](add-servers-to-server-manager.md) en esta guía.

4.  Cuando haya terminado de agregar servidores, haga clic en **Aceptar**. El nuevo grupo se muestra en el panel de navegación Administrador del servidor bajo el grupo **todos los servidores** .

#### <a name="to-edit-an-existing-server-group"></a>Para editar un grupo existente de servidores

1.  Realice una de las siguientes acciones:

    -   En el panel de navegación Administrador del servidor, haga clic con el botón secundario en un grupo de servidores y, a continuación, haga clic en **Editar Grupo de servidores**.

    -   En la Página principal del grupo de servidores, abra el menú **tareas** en el icono **servidores** y, a continuación, haga clic en **Editar Grupo de servidores**.

2.  Cambie el nombre del grupo o agregue o quite servidores del grupo.

    > [!NOTE]
    > la eliminación de servidores de un grupo de servidores no quita servidores de Administrador del servidor. Los servidores que elimina de un grupo se mantienen en el grupo **Todos los servidores** , en el grupo de servidores.

3.  Cuando haya terminado de realizar cambios en el grupo, haga clic en **Aceptar**.

#### <a name="to-delete-an-existing-server-group"></a>Para eliminar un grupo existente de servidores

1.  Realice una de las siguientes acciones:

    -   En el panel de navegación Administrador del servidor, haga clic con el botón secundario en un grupo de servidores y haga clic en **Eliminar grupo de servidores**.

    -   En la Página principal del grupo de servidores, abra el menú **tareas** en el icono **servidores** y, a continuación, haga clic en **Eliminar grupo de servidores**.

2.  Cuando se le pregunte si está seguro de eliminar el grupo de servidores, haga clic en **Sí**.

    > [!NOTE]
    > la eliminación de un grupo de servidores no quita los servidores de Administrador del servidor. Los servidores que se encontraban en un grupo eliminado se mantienen en el grupo **Todos los servidores** , en el grupo de servidores.

3.  Cuando haya terminado de realizar cambios en el grupo, haga clic en **Aceptar**.

## <a name="see-also"></a>Consulta también
[agregar servidores al
de Administrador del servidor](add-servers-to-server-manager.md) [Administrador del servidor](server-manager.md)



