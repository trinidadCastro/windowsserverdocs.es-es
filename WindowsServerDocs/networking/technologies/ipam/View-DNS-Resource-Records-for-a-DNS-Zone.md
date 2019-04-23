---
title: Visualización de registros de recursos DNS para una zona DNS
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
ms.assetid: 375feefc-949e-47c3-9e61-35b79e021966
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9cd1184c9f9d603b82ac43829d4c8f7946f26561
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867986"
---
# <a name="view-dns-resource-records-for-a-dns-zone"></a>Visualización de registros de recursos DNS para una zona DNS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para ver los registros de recursos DNS para una zona DNS en la consola de cliente IPAM.  
  
El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.  
  
### <a name="to-view-dns-resource-records-for-a-zone"></a>Para ver los registros de recursos DNS para una zona  
  
1.  En el administrador del servidor, haga clic en **IPAM**. Aparece la consola de cliente IPAM.  
  
2.  En el panel de navegación en **supervisión y administración**, haga clic en **zonas DNS**.  El panel de navegación se divide en un panel de navegación superior y un panel de navegación inferior.  
  
3.  En el panel de navegación inferior, haga clic en **búsqueda directa**y, a continuación, expanda la lista de dominio y zona para buscar y seleccionar la zona que desea ver. Por ejemplo, si tiene una zona denominada Dublín, haga clic en **Dublín**.  
  
    ![Seleccione la zona que desea ver](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01a.jpg)  

  
4.  En el panel de información, la vista predeterminada es de los servidores DNS de la zona. Para cambiar la vista, haga clic en **vista actual**y, a continuación, haga clic en **registros de recursos**.  
  
    ![Cambiar la vista a los registros de recursos](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_Zone_RR_02.jpg)  
  
5.  Se muestran los registros de recursos DNS para la zona. Para filtrar los registros, escriba el texto que desea buscar en **filtro**.  
  
    ![Escriba el texto para filtrar registros](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01c.jpg)  
  
6.  Para filtrar los registros de recursos por tipo de registro, el ámbito de acceso u otros criterios, haga clic en **agregar criterios**y, a continuación, realice las selecciones en la lista de criterios y haga clic en **agregar**.  
  
    ![Utilizar criterios para filtrar registros](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01d.jpg)  
  
## <a name="see-also"></a>Vea también  
[Administración de zonas DNS](DNS-Zone-Management.md)  
[Administrar IPAM](Manage-IPAM.md)  
  


