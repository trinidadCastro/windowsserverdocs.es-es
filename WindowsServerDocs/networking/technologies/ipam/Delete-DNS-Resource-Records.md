---
title: Eliminación de registros de recursos DNS
description: Este tema forma parte de la guía de administración de la administración de direcciones IP (IPAM) en Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ipam
ms.topic: article
ms.assetid: 366e6fd5-d563-4de3-9551-5614cbb8f2cb
ms.author: lizross
author: eross-msft
ms.openlocfilehash: a5658e53aebf9f6fa20a5de9f3f7cb5a0287b408
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860678"
---
# <a name="delete-dns-resource-records"></a>Eliminación de registros de recursos DNS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para eliminar uno o más registros de recursos DNS mediante la consola de cliente de IPAM.  
  
El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.  
  
### <a name="to-delete-dns-resource-records"></a>Para eliminar registros de recursos DNS  
  
1.  En Administrador del servidor, haga clic en **IPAM**. Aparece la consola de cliente de IPAM.  
  
2.  En el panel de navegación, en **supervisión y administración**, haga clic en **zonas DNS**.  El panel de navegación se divide en un panel de navegación superior y un panel de navegación inferior.  
  
3.  Haga clic para expandir **búsqueda directa** y el dominio donde se encuentran los registros de zona y de recursos que desea eliminar. Haga clic en la zona y, en el panel de información, haga clic en **vista actual**. Haga clic en **registros de recursos**.  
  
4.  En el panel de información, busque y seleccione los registros de recursos que desea eliminar.  
  
    ![Seleccionar los registros de recursos que se van a eliminar](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_01.jpg)  
  
5.  Haga clic con el botón secundario en los registros seleccionados y, a continuación, haga clic en **Eliminar registro de recursos DNS**.  
  
    ![Eliminar los registros](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_02.jpg)  
  
6.  Se abre el cuadro de diálogo **Eliminar registro de recursos DNS** . Compruebe que está seleccionado el servidor DNS correcto. Si no lo está, haga clic en **servidor DNS** y seleccione el servidor desde el que desea eliminar los registros de recursos. Haga clic en **Aceptar**. IPAM elimina los registros de recursos del servidor DNS.  
  
    ![Comprobar que el servidor DNS correcto está seleccionado y eliminar registros](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_03.jpg)  
  
## <a name="see-also"></a>Consulta también  
[Administración de registros de recursos DNS](DNS-Resource-Record-Management.md)  
[Administrar IPAM](Manage-IPAM.md)  
  


