---
title: Edición de una zona DNS
description: Obtenga información sobre cómo editar una zona DNS en la consola de cliente de IPAM.
manager: brianlic
ms.topic: article
ms.assetid: a35164e1-11ad-47c8-9843-580d30c70d07
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: e0cf2667c8992ee94a663f78cf3f3fc7c7a82b24
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98040185"
---
# <a name="edit-a-dns-zone"></a>Edición de una zona DNS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para editar una zona DNS en la consola de cliente de IPAM.

El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.

### <a name="to-edit-a-dns-zone"></a>Para editar una zona DNS

1.  En Administrador del servidor, haga clic en **IPAM**. Aparece la consola de cliente de IPAM.

2.  En el panel de navegación, en **supervisión y administración**, haga clic en **zonas DNS**. El panel de navegación se divide en un panel de navegación superior y un panel de navegación inferior.

3.  En el panel de navegación inferior, realice una de las selecciones siguientes:

    -   Búsqueda directa

    -   Búsqueda inversa IPv4

    -   Búsqueda inversa IPv6

4.  Por ejemplo, seleccione búsqueda inversa IPv4.

    ![Seleccionar un tipo de zona](../../media/Edit-a-DNS-Zone/ipam_EditZone_01.jpg)

5.  En el panel de información, haga clic con el botón secundario en la zona que desea editar y, a continuación, haga clic en **Editar zona DNS**.

    ![Editar zona DNS](../../media/Edit-a-DNS-Zone/ipam_EditZone_02.jpg)

6.  Se abre el cuadro de diálogo **Editar zona DNS** con la página **General** seleccionada. Si es necesario, edite las propiedades de la zona general: **servidor DNS**, **categoría de zona** y **tipo de zona** y, a continuación, haga clic en **aplicar** o, si las ediciones están completas, en **Aceptar**.

    ![Editar propiedades de la zona y guardar](../../media/Edit-a-DNS-Zone/ipam_EditZone_03a.jpg)

7.  En el cuadro de diálogo **Editar zona DNS** , haga clic en **Opciones avanzadas**. Se abre la página propiedades **avanzadas** de la zona. Si es necesario, edite las propiedades que desea cambiar y, a continuación, haga clic en **aplicar** o, si las ediciones se han completado, en **Aceptar**.

    ![Editar propiedades de zona avanzadas](../../media/Edit-a-DNS-Zone/ipam_EditZone_04a.jpg)

8.  Si es necesario, seleccione los nombres de página de propiedades de zona adicionales (servidores de nombres, SOA, transferencias de zona), realice las modificaciones y haga clic en **aplicar** o en **Aceptar**. Para revisar todas las ediciones de la zona, haga clic en **Resumen** y, a continuación, haga clic en **Aceptar**.

## <a name="see-also"></a>Consulte también
Administración de zonas [DNS](DNS-Zone-Management.md) 
 [Administrar IPAM](Manage-IPAM.md)



