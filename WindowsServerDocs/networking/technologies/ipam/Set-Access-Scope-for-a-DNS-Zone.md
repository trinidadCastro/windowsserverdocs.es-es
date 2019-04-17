---
title: Establecer el ámbito de acceso para una zona de DNS
description: Este tema es parte de la Guía de administración de administración de direcciones IP (IPAM) en Windows Server 2016.
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
ms.openlocfilehash: a4e84f249e57df6bfd04203c8b825ff5d4d643b2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="set-access-scope-for-a-dns-zone"></a>Establecer el ámbito de acceso para una zona de DNS

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para establecer el ámbito de acceso para una zona DNS mediante la consola IPAM del cliente.  
  
Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para realizar este procedimiento.  
  
### <a name="to-set-the-access-scope-for-a-dns-zone"></a>Para establecer el ámbito de acceso para una zona DNS  
  
1.  En el administrador del servidor, haz clic en **IPAM**. Aparece la consola IPAM del cliente.  
  
2.  En el panel de navegación, haz clic en **zonas DNS**. En el panel de la pantalla, haz clic en la zona DNS para la que quieres cambiar el ámbito de acceso. y, a continuación, haz clic en **establecer el ámbito de acceso**.  
  
    ![Establecer el ámbito de acceso](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_02.jpg)  
  
3.  La **establecer el ámbito de acceso** abre el cuadro de diálogo. Si es necesario para la implementación, haga clic para anular la selección de **heredar de ámbito de acceso de un primario**. En **seleccione el ámbito de acceso**, selecciona un elemento y, a continuación, haz clic en **Aceptar**.  
  
    ![Selecciona el ámbito de acceso](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_03.jpg)  
  
4.  En el panel de pantalla de la consola IPAM cliente, comprueba que se ha cambiado el ámbito de acceso para la zona.  
  
    ![Comprueba que se ha cambiado el ámbito de acceso para la zona](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_04.jpg)  
  
## <a name="see-also"></a>Consulta también  
[Control de acceso basadas en roles](Role-based-Access-Control.md)  
[Administrar IPAM](Manage-IPAM.md)  
  


