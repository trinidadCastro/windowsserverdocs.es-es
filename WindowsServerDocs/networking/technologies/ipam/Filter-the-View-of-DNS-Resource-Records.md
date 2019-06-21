---
title: Filtrado de la vista de registros de recursos DNS
description: Este tema forma parte de la Guía de administración de administración de direcciones IP (IPAM) en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5b80294a-7325-476b-84eb-69f0d051e8b2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cc3f2b8ec6e7c5149ef6351639fbbf8f0def8be8
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283936"
---
# <a name="filter-the-view-of-dns-resource-records"></a>Filtrado de la vista de registros de recursos DNS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se puede utilizar para filtrar la vista de registros de recursos DNS en la consola de cliente IPAM.  
  
El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.  
  
### <a name="to-filter-the-view-of-dns-resource-records"></a>Para filtrar la vista de registros de recursos DNS  
  
1.  En el administrador del servidor, haga clic en **IPAM**. Aparece la consola de cliente IPAM.  
  
2.  En el panel de navegación en **supervisión y administración**, haga clic en **zonas DNS**.  El panel de navegación se divide en un panel de navegación superior y un panel de navegación inferior.  
  
3.  En el panel de navegación inferior, haga clic en **búsqueda directa**. Todas las zonas de búsqueda de DNS administrados por IPAM directa se muestran en los resultados de búsqueda del panel de visualización.  
  
4.  Haga clic en la zona cuyos registros desea ver y filtrar.  
  
5.  En el panel de información, haga clic en **vista actual**y, a continuación, haga clic en **registros de recursos**. Los registros de recursos para la zona se muestran en el panel de información.  
  
6.  En el panel de información, haga clic en **agregar criterios**.  
  
    ![Agregar criterios](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_01.jpg)  
  
7.  Seleccione un criterio en la lista desplegable. Por ejemplo, si desea ver un tipo de registro específico, haga clic en **tipo de registro**.  
  
    ![Seleccione un criterio](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_02.jpg)  
  
8.  Haz clic en **Agregar**.  
  
    ![Agregue los criterios](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_03.jpg)  
  
9. **Tipo de registro** se agrega como un parámetro de búsqueda. Escriba el texto para el tipo de registro que desea buscar. Por ejemplo, si desea ver solo los registros SRV, escriba **SRV**.  
  
    ![Especifique el tipo de registro que desea buscar](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_04.jpg)  
  
10. Presione ENTRAR. Los registros de recursos DNS se filtran según los criterios y buscar frases que especificó.  
  
    ![Ejecute el filtro](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_05.jpg)  
  
## <a name="see-also"></a>Vea también  
[Administración de registros de recursos DNS](DNS-Resource-Record-Management.md)  
[Administrar IPAM](Manage-IPAM.md)  
  


