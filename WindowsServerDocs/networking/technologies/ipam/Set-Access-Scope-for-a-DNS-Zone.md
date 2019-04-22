---
title: Establecer el ámbito de acceso para una zona DNS
description: Este tema forma parte de la Guía de administración de administración de direcciones IP (IPAM) en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a211dde-80eb-4888-b5bb-4e28fe8dc7df
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 73d96a9be7c13afbe4c96d46fffefc0096046c8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813986"
---
# <a name="set-access-scope-for-a-dns-zone"></a>Establecer el ámbito de acceso para una zona DNS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para establecer el ámbito de acceso para una zona DNS mediante la consola de cliente IPAM.  
  
El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.  
  
### <a name="to-set-the-access-scope-for-a-dns-zone"></a>Para establecer el ámbito de acceso para una zona DNS  
  
1.  En el administrador del servidor, haga clic en **IPAM**. Aparece la consola de cliente IPAM.  
  
2.  En el panel de navegación, haga clic en **zonas DNS**. En el panel de información, haga clic en la zona DNS para el que desea cambiar el ámbito de acceso. y, a continuación, haga clic en **establecer ámbito de acceso**.  
  
    ![Establecer el ámbito de acceso](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_02.jpg)  
  
3.  El **establecer ámbito de acceso** abre el cuadro de diálogo. Si es necesario para la implementación, haga clic para anular la selección de **heredar ámbito de acceso de elemento primario**. En **seleccionar el ámbito de acceso**, seleccione un elemento y, a continuación, haga clic en **Aceptar**.  
  
    ![Seleccione el ámbito de acceso](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_03.jpg)  
  
4.  En el panel de información de consola del cliente IPAM, compruebe que se cambia el ámbito de acceso para la zona.  
  
    ![Compruebe que se cambia el ámbito de acceso para la zona](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_04.jpg)  
  
## <a name="see-also"></a>Vea también  
[Control de acceso basado en roles](Role-based-Access-Control.md)  
[Administrar IPAM](Manage-IPAM.md)  
  


