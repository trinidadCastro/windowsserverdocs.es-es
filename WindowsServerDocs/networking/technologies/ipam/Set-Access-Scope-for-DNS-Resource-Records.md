---
title: Ámbito de acceso de conjunto de registros de recursos DNS
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
ms.assetid: a96a8752-5678-49c5-b069-d2cce8042a51
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 06c33a633975497e80863cc8d42b14a0f9ac8193
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="set-access-scope-for-dns-resource-records"></a>Ámbito de acceso de conjunto de registros de recursos DNS

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para establecer el ámbito de acceso de registros de recursos DNS mediante la consola IPAM cliente.  
  
Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para realizar este procedimiento.  
  
### <a name="to-set-access-scope-for-dns-resource-records"></a>Para establecer el ámbito de acceso de los registros de recursos DNS  
  
1.  En el administrador del servidor, haz clic en **IPAM**. Aparece la consola IPAM del cliente.  
  
2.  En el panel de navegación, haz clic en **zonas DNS**.  En el panel de navegación inferior, expanda **búsqueda directa** y busca y selecciona la zona que contiene los registros de recursos cuyo ámbito de acceso que desee cambiar.  
  
3.  En el panel de la pantalla, busca y selecciona los registros de recursos cuyo ámbito de acceso que desee cambiar.  
  
    ![Selecciona los registros de recursos](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_02.jpg)  
  
4.  Haz clic en los registros de recursos DNS seleccionados y, a continuación, haz clic en **establecer el ámbito de acceso**.  
  
    ![Establecer el ámbito de acceso](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_03.jpg)  
  
5.  La **establecer el ámbito de acceso** abre el cuadro de diálogo. Si es necesario para la implementación, haga clic para anular la selección de **heredar de ámbito de acceso de un primario**. En **seleccione el ámbito de acceso**, selecciona un elemento y, a continuación, haz clic en **Aceptar**.  
  
    ![Selecciona el ámbito de acceso](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_04.jpg)  
  
## <a name="see-also"></a>Consulta también  
[Control de acceso basadas en roles](Role-based-Access-Control.md)  
[Administrar IPAM](Manage-IPAM.md)  
  


