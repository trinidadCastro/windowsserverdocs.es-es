---
title: Ver registros de recursos DNS para una zona de DNS
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
ms.assetid: 375feefc-949e-47c3-9e61-35b79e021966
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 786c1ee8fd673bd17465ab9586dd1e0bcfd7971c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="view-dns-resource-records-for-a-dns-zone"></a>Ver registros de recursos DNS para una zona de DNS

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para ver los registros de recursos DNS para una zona DNS en la consola IPAM del cliente.  
  
Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para realizar este procedimiento.  
  
### <a name="to-view-dns-resource-records-for-a-zone"></a>Para ver los registros de recursos DNS para una zona  
  
1.  En el administrador del servidor, haz clic en **IPAM**. Aparece la consola IPAM del cliente.  
  
2.  En el panel de navegación, en **controlar y administrar**, haz clic en **zonas DNS**.  El panel de navegación se divide en un panel de navegación superior y un panel de navegación inferior.  
  
3.  En el panel de navegación inferior, haga clic en **búsqueda directa**y, a continuación, expande la lista de dominio y zona para buscar y seleccionar la zona que deseas ver. Por ejemplo, si tienes una zona denominada Dublín, haz clic en **dublin**.  
  
    ![Selecciona la zona que deseas ver](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01a.jpg)  

  
4.  En el panel de la pantalla, la vista predeterminada es de los servidores DNS para la zona. Para cambiar la vista, haz clic en **vista actual**y, a continuación, haz clic en **registros de recursos**.  
  
    ![Cambiar la vista a los registros de recursos](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_Zone_RR_02.jpg)  
  
5.  Se muestran los registros de recursos DNS para la zona. Para filtrar los registros, escribe el texto que quieres buscar en **filtro**.  
  
    ![Escribe texto para filtrar registros](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01c.jpg)  
  
6.  Para filtrar los registros de recursos por tipo de registro, el ámbito de acceso u otros criterios, haz clic en **agregar criterios**y, a continuación, realice las selecciones de la lista de criterios y haz clic en **agregar**.  
  
    ![Usa criterios para filtrar registros](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01d.jpg)  
  
## <a name="see-also"></a>Consulta también  
[Administración de zonas de DNS](DNS-Zone-Management.md)  
[Administrar IPAM](Manage-IPAM.md)  
  


