---
title: Visualización de registros de recursos DNS para una zona DNS
description: Este tema forma parte de la guía de administración de la administración de direcciones IP (IPAM) en Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ipam
ms.topic: article
ms.assetid: 375feefc-949e-47c3-9e61-35b79e021966
ms.author: lizross
author: eross-msft
ms.openlocfilehash: dfd772f89d913cd75b8c31ef4e5236f3cc11263c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854848"
---
# <a name="view-dns-resource-records-for-a-dns-zone"></a>Visualización de registros de recursos DNS para una zona DNS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para ver los registros de recursos DNS para una zona DNS en la consola de cliente de IPAM.  
  
El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.  
  
### <a name="to-view-dns-resource-records-for-a-zone"></a>Para ver los registros de recursos DNS de una zona  
  
1.  En Administrador del servidor, haga clic en **IPAM**. Aparece la consola de cliente de IPAM.  
  
2.  En el panel de navegación, en **supervisión y administración**, haga clic en **zonas DNS**.  El panel de navegación se divide en un panel de navegación superior y un panel de navegación inferior.  
  
3.  En el panel de navegación inferior, haga clic en **búsqueda directa**y, a continuación, expanda la lista dominio y zona para buscar y seleccionar la zona que desea ver. Por ejemplo, si tiene una zona denominada Dublín, haga clic en **Dublín**.  
  
    ![Seleccione la zona que desea ver](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01a.jpg)  

  
4.  En el panel de información, la vista predeterminada es de los servidores DNS de la zona. Para cambiar la vista, haga clic en **vista actual**y, a continuación, haga clic en **registros de recursos**.  
  
    ![Cambiar la vista a los registros de recursos](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_Zone_RR_02.jpg)  
  
5.  Se muestran los registros de recursos DNS para la zona. Para filtrar los registros, escriba el texto que desea buscar en **filtro**.  
  
    ![Escribir texto para filtrar registros](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01c.jpg)  
  
6.  Para filtrar los registros de recursos por tipo de registro, ámbito de acceso u otros criterios, haga clic en **Agregar criterios**y, a continuación, realice selecciones en la lista criterios y haga clic en **Agregar**.  
  
    ![Usar criterios para filtrar registros](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01d.jpg)  
  
## <a name="see-also"></a>Consulta también  
[Administración de zonas DNS](DNS-Zone-Management.md)  
[Administrar IPAM](Manage-IPAM.md)  
  


