---
title: Establecer el ámbito de acceso para registros de recursos DNS
description: Obtenga información sobre cómo establecer el ámbito de acceso para los registros de recursos DNS mediante la consola de cliente de IPAM.
manager: brianlic
ms.topic: article
ms.assetid: a96a8752-5678-49c5-b069-d2cce8042a51
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: ac8ef616587ac214cf75240e88d941f74de03d41
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98039395"
---
# <a name="set-access-scope-for-dns-resource-records"></a>Establecer el ámbito de acceso para registros de recursos DNS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para establecer el ámbito de acceso para los registros de recursos DNS mediante la consola de cliente de IPAM.

El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.

### <a name="to-set-access-scope-for-dns-resource-records"></a>Para establecer el ámbito de acceso para los registros de recursos DNS

1.  En Administrador del servidor, haga clic en  **IPAM**. Aparece la consola de cliente de IPAM.

2.  En el panel de navegación, haga clic en **zonas DNS**.  En el panel de navegación inferior, expanda **búsqueda directa** y busque y seleccione la zona que contiene los registros de recursos cuyo ámbito de acceso desea cambiar.

3.  En el panel de información, busque y seleccione los registros de recursos cuyo ámbito de acceso desea cambiar.

    ![Seleccionar los registros de recursos](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_02.jpg)

4.  Haga clic con el botón secundario en los registros de recursos DNS seleccionados y, a continuación, haga clic en **establecer ámbito de acceso**.

    ![Establecer el ámbito de acceso](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_03.jpg)

5.  Se abre el cuadro de diálogo **establecer ámbito de acceso** . Si es necesario para la implementación, haga clic para anular la selección de **heredar ámbito de acceso del elemento primario**. En **seleccionar el ámbito de acceso**, seleccione un elemento y, a continuación, haga clic en **Aceptar**.

    ![Seleccionar el ámbito de acceso](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_04.jpg)

## <a name="see-also"></a>Consulte también
Access Control basado en [roles](Role-based-Access-Control.md) 
 [Administrar IPAM](Manage-IPAM.md)



