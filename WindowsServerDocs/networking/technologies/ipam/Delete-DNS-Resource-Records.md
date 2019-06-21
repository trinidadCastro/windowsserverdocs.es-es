---
title: Eliminación de registros de recursos DNS
description: Este tema forma parte de la Guía de administración de administración de direcciones IP (IPAM) en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 366e6fd5-d563-4de3-9551-5614cbb8f2cb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ecbe5dcc452aa39a9afca7e1c8d5fe70d8d4528d
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283977"
---
# <a name="delete-dns-resource-records"></a>Eliminación de registros de recursos DNS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para eliminar uno o más registros de recursos DNS mediante la consola de cliente IPAM.  
  
El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.  
  
### <a name="to-delete-dns-resource-records"></a>Para eliminar registros de recursos DNS  
  
1.  En el administrador del servidor, haga clic en **IPAM**. Aparece la consola de cliente IPAM.  
  
2.  En el panel de navegación en **supervisión y administración**, haga clic en **zonas DNS**.  El panel de navegación se divide en un panel de navegación superior y un panel de navegación inferior.  
  
3.  Haga clic para expandir **búsqueda directa** y el dominio donde se encuentran los registros de recursos y la zona que desea eliminar. Haga clic en la zona y en el panel, haga clic en **vista actual**. Haga clic en **registros de recursos**.  
  
4.  En el panel de información, busque y seleccione los registros de recursos que desea eliminar.  
  
    ![Seleccione los registros de recursos para eliminar](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_01.jpg)  
  
5.  Haga clic en los registros seleccionados y, a continuación, haga clic en **registro de recursos DNS eliminar**.  
  
    ![Eliminar los registros](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_02.jpg)  
  
6.  El **eliminar registro de recursos DNS** abre el cuadro de diálogo. Compruebe que el servidor DNS correcto está seleccionado. Si no es así, haga clic en **servidor DNS** y seleccione el servidor desde el que desea eliminar los registros de recursos. Haga clic en **Aceptar**. IPAM elimina los registros de recursos del servidor DNS.  
  
    ![Compruebe que el servidor DNS correcto está seleccionado y eliminar registros](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_03.jpg)  
  
## <a name="see-also"></a>Vea también  
[Administración de registros de recursos DNS](DNS-Resource-Record-Management.md)  
[Administrar IPAM](Manage-IPAM.md)  
  


