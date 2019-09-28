---
title: Establecer el ámbito de acceso para registros de recursos DNS
description: Este tema forma parte de la guía de administración de la administración de direcciones IP (IPAM) en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a96a8752-5678-49c5-b069-d2cce8042a51
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b1790f2cbf84fd68f33ca30d2fe7663dde824240
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405644"
---
# <a name="set-access-scope-for-dns-resource-records"></a>Establecer el ámbito de acceso para registros de recursos DNS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para establecer el ámbito de acceso para los registros de recursos DNS mediante la consola de cliente de IPAM.  
  
El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.  
  
### <a name="to-set-access-scope-for-dns-resource-records"></a>Para establecer el ámbito de acceso para los registros de recursos DNS  
  
1.  En Administrador del servidor, haga clic en **IPAM**. Aparece la consola de cliente de IPAM.  
  
2.  En el panel de navegación, haga clic en **zonas DNS**.  En el panel de navegación inferior, expanda **búsqueda directa** y busque y seleccione la zona que contiene los registros de recursos cuyo ámbito de acceso desea cambiar.  
  
3.  En el panel de información, busque y seleccione los registros de recursos cuyo ámbito de acceso desea cambiar.  
  
    ![Seleccionar los registros de recursos](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_02.jpg)  
  
4.  Haga clic con el botón secundario en los registros de recursos DNS seleccionados y, a continuación, haga clic en **establecer ámbito de acceso**.  
  
    ![Establecer el ámbito de acceso](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_03.jpg)  
  
5.  Se abre el cuadro de diálogo **establecer ámbito de acceso** . Si es necesario para la implementación, haga clic para anular la selección de **heredar ámbito de acceso del elemento primario**. En **seleccionar el ámbito de acceso**, seleccione un elemento y, a continuación, haga clic en **Aceptar**.  
  
    ![Seleccionar el ámbito de acceso](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_04.jpg)  
  
## <a name="see-also"></a>Vea también  
[Access Control basado en roles](Role-based-Access-Control.md)  
[Administrar IPAM](Manage-IPAM.md)  
  


