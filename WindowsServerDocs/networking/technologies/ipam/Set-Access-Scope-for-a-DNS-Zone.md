---
title: Establecer el ámbito de acceso para una zona DNS
description: Obtenga información sobre cómo establecer el ámbito de acceso para una zona DNS mediante la consola de cliente de IPAM.
manager: brianlic
ms.topic: article
ms.assetid: 6a211dde-80eb-4888-b5bb-4e28fe8dc7df
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 8d679dcc0e16e1dc3e38e11247637463a26ed0f5
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98040145"
---
# <a name="set-access-scope-for-a-dns-zone"></a>Establecer el ámbito de acceso para una zona DNS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para establecer el ámbito de acceso para una zona DNS mediante la consola de cliente de IPAM.

El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.

### <a name="to-set-the-access-scope-for-a-dns-zone"></a>Para establecer el ámbito de acceso para una zona DNS

1.  En Administrador del servidor, haga clic en  **IPAM**. Aparece la consola de cliente de IPAM.

2.  En el panel de navegación, haga clic en **zonas DNS**. En el panel de información, haga clic con el botón secundario en la zona DNS cuyo ámbito de acceso desea cambiar y, a continuación, haga clic en **establecer ámbito de acceso**.

    ![Establecer el ámbito de acceso](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_02.jpg)

3.  Se abre el cuadro de diálogo **establecer ámbito de acceso** . Si es necesario para la implementación, haga clic para anular la selección de **heredar ámbito de acceso del elemento primario**. En **seleccionar el ámbito de acceso**, seleccione un elemento y, a continuación, haga clic en **Aceptar**.

    ![Seleccionar el ámbito de acceso](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_03.jpg)

4.  En el panel de información de la consola del cliente IPAM, compruebe que se ha cambiado el ámbito de acceso de la zona.

    ![Comprobar que se ha cambiado el ámbito de acceso de la zona](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_04.jpg)

## <a name="see-also"></a>Consulte también
Access Control basado en [roles](Role-based-Access-Control.md) 
 [Administrar IPAM](Manage-IPAM.md)



