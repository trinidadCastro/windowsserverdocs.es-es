---
title: Eliminar registros de recursos DNS
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
ms.assetid: 366e6fd5-d563-4de3-9551-5614cbb8f2cb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f9ebfeca1da9e36cd00272113f2e86c33174074b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="delete-dns-resource-records"></a>Eliminar registros de recursos DNS

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para eliminar uno o más registros de recursos DNS mediante la consola IPAM cliente.  
  
Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para realizar este procedimiento.  
  
### <a name="to-delete-dns-resource-records"></a>Para eliminar registros de recursos DNS  
  
1.  En el administrador del servidor, haz clic en **IPAM**. Aparece la consola IPAM del cliente.  
  
2.  En el panel de navegación, en **controlar y administrar**, haz clic en **zonas DNS**.  El panel de navegación se divide en un panel de navegación superior y un panel de navegación inferior.  
  
3.  Haga clic para expandir **búsqueda directa** y el dominio donde se encuentran los registros de recursos y la zona que quieres eliminar. Haz clic en la zona y en el panel, haz clic en **vista actual**. Haz clic en **registros de recursos**.  
  
4.  En el panel de la pantalla, busca y selecciona los registros de recursos que quieres eliminar.  
  
    ![Selecciona los registros de recursos para eliminar](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_01.jpg)  
  
5.  Haz clic en los registros seleccionados y, a continuación, haz clic en **registro de recursos de DNS eliminar**.  
  
    ![Eliminar los registros](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_02.jpg)  
  
6.  La **eliminar el registro de recursos de DNS** abre el cuadro de diálogo. Comprueba que el servidor DNS correcto está seleccionado. Si no lo está, haz clic en **servidor DNS** y selecciona el servidor desde el que quieres eliminar los registros de recursos. Haz clic en **Aceptar**. IPAM elimina los registros de recursos del servidor DNS.  
  
    ![Compruebe que el servidor DNS correcto está activado y eliminar registros](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_03.jpg)  
  
## <a name="see-also"></a>Consulta también  
[Administración de registros de recursos DNS](DNS-Resource-Record-Management.md)  
[Administrar IPAM](Manage-IPAM.md)  
  


