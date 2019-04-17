---
title: Filtra la vista de recursos DNS
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
ms.assetid: 5b80294a-7325-476b-84eb-69f0d051e8b2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 35c0e822daa9f2c8c49ae7e6f2f40ec0411cb6fa
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="filter-the-view-of-dns-resource-records"></a>Filtra la vista de recursos DNS

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para filtrar la vista de recursos de DNS en la consola IPAM del cliente.  
  
Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para realizar este procedimiento.  
  
### <a name="to-filter-the-view-of-dns-resource-records"></a>Para filtrar la vista de recursos DNS  
  
1.  En el administrador del servidor, haz clic en **IPAM**. Aparece la consola IPAM del cliente.  
  
2.  En el panel de navegación, en **controlar y administrar**, haz clic en **zonas DNS**.  El panel de navegación se divide en un panel de navegación superior y un panel de navegación inferior.  
  
3.  En el panel de navegación inferior, haga clic en **búsqueda directa**. Todas las zonas de búsqueda de DNS administrado IPAM directa se muestran en los resultados de búsqueda del panel de pantalla.  
  
4.  Haz clic en la zona cuyos que quieres ver y filtrar los registros.  
  
5.  En el panel, haz clic en **vista actual**y, a continuación, haz clic en **registros de recursos**. En el panel, se muestran los registros de recursos para la zona.  
  
6.  En el panel, haz clic en **agregar criterios**.  
  
    ![Agregar criterios](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_01.jpg)  
  
7.  Selecciona un criterio de la lista desplegable. Por ejemplo, si quieres ver un tipo de registro específico, haga clic en **tipo de registro**.  
  
    ![Selecciona los criterios de](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_02.jpg)  
  
8.  Haz clic en **agregar**.  
  
    ![Agregar los criterios](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_03.jpg)  
  
9. **Tipo de registro** se agrega como un parámetro de búsqueda. Escribe texto para el tipo de registro que deseas buscar. Por ejemplo, si quieres ver solo los registros SRV, escribe **SRV**.  
  
    ![Especificar el tipo de registro que deseas buscar](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_04.jpg)  
  
10. Presiona ENTRAR. Los registros de recursos DNS se filtran según los criterios y buscar frases que especificaste.  
  
    ![Ejecutar el filtro](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_05.jpg)  
  
## <a name="see-also"></a>Consulta también  
[Administración de registros de recursos DNS](DNS-Resource-Record-Management.md)  
[Administrar IPAM](Manage-IPAM.md)  
  


