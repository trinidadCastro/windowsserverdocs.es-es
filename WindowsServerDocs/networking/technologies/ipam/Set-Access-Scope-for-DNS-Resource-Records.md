---
title: Establecer el ámbito de acceso para registros de recursos DNS
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
ms.assetid: a96a8752-5678-49c5-b069-d2cce8042a51
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2e63c82ef0c58a9b4392ad8b9b1fc896d075ab71
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882916"
---
# <a name="set-access-scope-for-dns-resource-records"></a>Establecer el ámbito de acceso para registros de recursos DNS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para establecer el ámbito de acceso para los registros de recursos DNS mediante la consola de cliente IPAM.  
  
El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.  
  
### <a name="to-set-access-scope-for-dns-resource-records"></a>Para establecer el ámbito de acceso para los registros de recursos DNS  
  
1.  En el administrador del servidor, haga clic en **IPAM**. Aparece la consola de cliente IPAM.  
  
2.  En el panel de navegación, haga clic en **zonas DNS**.  En el panel de navegación inferior, expanda **búsqueda directa** y busque y seleccione la zona que contiene los registros de recursos cuyo ámbito de acceso que desea cambiar.  
  
3.  En el panel de información, busque y seleccione los registros de recursos cuyo ámbito de acceso que desea cambiar.  
  
    ![Seleccione los registros de recursos](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_02.jpg)  
  
4.  Haga clic en los registros de recursos DNS seleccionados y, a continuación, haga clic en **establecer ámbito de acceso**.  
  
    ![Establecer el ámbito de acceso](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_03.jpg)  
  
5.  El **establecer ámbito de acceso** abre el cuadro de diálogo. Si es necesario para la implementación, haga clic para anular la selección de **heredar ámbito de acceso de elemento primario**. En **seleccionar el ámbito de acceso**, seleccione un elemento y, a continuación, haga clic en **Aceptar**.  
  
    ![Seleccione el ámbito de acceso](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_04.jpg)  
  
## <a name="see-also"></a>Vea también  
[Control de acceso basado en roles](Role-based-Access-Control.md)  
[Administrar IPAM](Manage-IPAM.md)  
  


