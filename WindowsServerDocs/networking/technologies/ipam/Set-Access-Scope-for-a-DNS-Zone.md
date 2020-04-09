---
title: Establecer el ámbito de acceso para una zona DNS
description: Este tema forma parte de la guía de administración de la administración de direcciones IP (IPAM) en Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ipam
ms.topic: article
ms.assetid: 6a211dde-80eb-4888-b5bb-4e28fe8dc7df
ms.author: lizross
author: eross-msft
ms.openlocfilehash: ca1899a4d49639b047b672c42b9743fb27df8a67
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860608"
---
# <a name="set-access-scope-for-a-dns-zone"></a>Establecer el ámbito de acceso para una zona DNS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para establecer el ámbito de acceso para una zona DNS mediante la consola de cliente de IPAM.  
  
El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.  
  
### <a name="to-set-the-access-scope-for-a-dns-zone"></a>Para establecer el ámbito de acceso para una zona DNS  
  
1.  En Administrador del servidor, haga clic en **IPAM**. Aparece la consola de cliente de IPAM.  
  
2.  En el panel de navegación, haga clic en **zonas DNS**. En el panel de información, haga clic con el botón secundario en la zona DNS cuyo ámbito de acceso desea cambiar y, a continuación, haga clic en **establecer ámbito de acceso**.  
  
    ![Establecer el ámbito de acceso](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_02.jpg)  
  
3.  Se abre el cuadro de diálogo **establecer ámbito de acceso** . Si es necesario para la implementación, haga clic para anular la selección de **heredar ámbito de acceso del elemento primario**. En **seleccionar el ámbito de acceso**, seleccione un elemento y, a continuación, haga clic en **Aceptar**.  
  
    ![Seleccionar el ámbito de acceso](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_03.jpg)  
  
4.  En el panel de información de la consola del cliente IPAM, compruebe que se ha cambiado el ámbito de acceso de la zona.  
  
    ![Comprobar que se ha cambiado el ámbito de acceso de la zona](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_04.jpg)  
  
## <a name="see-also"></a>Consulta también  
[Access Control basado en roles](Role-based-Access-Control.md)  
[Administrar IPAM](Manage-IPAM.md)  
  


