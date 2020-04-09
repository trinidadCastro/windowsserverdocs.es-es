---
title: crear y administrar grupos de servidores
description: Administrador de servidores
ms.prod: windows-server
ms.technology: manage-server-manager
ms.topic: article
ms.assetid: 9d5b1be8-49fd-4ff7-9580-e4ff21fe4b17
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2f4ad512c55bcd1391ad55bdbdeb9a2ba3bfd7f0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851548"
---
# <a name="create-and-manage-server-groups"></a>crear y administrar grupos de servidores

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se describe cómo crear grupos de servidores personalizados definidos por el usuario en Administrador del servidor en Windows Server.

## <a name="server-groups"></a><a name=BKMK_groups></a>Grupos de servidores
Los servidores que se agregan al grupo de servidores se muestran en la página **todos los servidores** de administrador del servidor. Puede crear grupos de servidores personalizados que haya agregado. Los grupos de servidores le permiten ver y administrar un subconjunto más pequeño del grupo de servidores como unidad lógica; por ejemplo, puede crear un grupo denominado **servidores de contabilidad** para todos los servidores del Departamento de contabilidad de su organización o un grupo denominado **Chicago** para todos los servidores ubicados geográficamente en Chicago. Después de crear un grupo de servidores, la Página principal del grupo en Administrador del servidor muestra información acerca de los eventos, servicios, contadores de rendimiento, Analizador de procedimientos recomendados resultados y roles y características instalados para el grupo en su conjunto.

Los servidores pueden ser miembros de más de un grupo.

#### <a name="to-create-a-new-server-group"></a>Para crear un nuevo grupo de servidores

1.  En el menú **administrar** , haga clic en **Crear grupo de servidores**.

2.  En el cuadro de texto **Nombre de grupo de servidores** , escriba un nombre descriptivo para el grupo de servidores, como **Servidores de cuentas**.

3.  Agregue servidores a la lista **seleccionada** desde el grupo de servidores o agregue otros servidores al grupo mediante las pestañas **Active Directory**, **DNS**o **importar** . Para obtener más información sobre cómo usar estas pestañas, consulte [agregar servidores a administrador del servidor](add-servers-to-server-manager.md) en esta guía.

4.  Cuando haya terminado de agregar servidores, haga clic en **Aceptar**. El nuevo grupo se muestra en el panel de navegación Administrador del servidor bajo el grupo **todos los servidores** .

#### <a name="to-edit-an-existing-server-group"></a>Para editar un grupo existente de servidores

1.  Lleve a cabo cualquiera de las siguientes opciones.

    -   En el panel de navegación Administrador del servidor, haga clic con el botón secundario en un grupo de servidores y, a continuación, haga clic en **Editar Grupo de servidores**.

    -   En la Página principal del grupo de servidores, abra el menú **tareas** en el icono **servidores** y, a continuación, haga clic en **Editar Grupo de servidores**.

2.  Cambie el nombre del grupo o agregue o quite servidores del grupo.

    > [!NOTE]
    > la eliminación de servidores de un grupo de servidores no quita servidores de Administrador del servidor. Los servidores que elimina de un grupo se mantienen en el grupo **Todos los servidores** , en el grupo de servidores.

3.  Cuando haya terminado de realizar cambios en el grupo, haga clic en **Aceptar**.

#### <a name="to-delete-an-existing-server-group"></a>Para eliminar un grupo existente de servidores

1.  Lleve a cabo cualquiera de las siguientes opciones.

    -   En el panel de navegación Administrador del servidor, haga clic con el botón secundario en un grupo de servidores y haga clic en **Eliminar grupo de servidores**.

    -   En la Página principal del grupo de servidores, abra el menú **tareas** en el icono **servidores** y, a continuación, haga clic en **Eliminar grupo de servidores**.

2.  Cuando se le pregunte si está seguro de eliminar el grupo de servidores, haga clic en **Sí**.

    > [!NOTE]
    > la eliminación de un grupo de servidores no quita los servidores de Administrador del servidor. Los servidores que se encontraban en un grupo eliminado se mantienen en el grupo **Todos los servidores** , en el grupo de servidores.

3.  Cuando haya terminado de realizar cambios en el grupo, haga clic en **Aceptar**.

## <a name="see-also"></a>Consulta también
[agregar servidores al
de Administrador del servidor](add-servers-to-server-manager.md) [Administrador del servidor](server-manager.md)



