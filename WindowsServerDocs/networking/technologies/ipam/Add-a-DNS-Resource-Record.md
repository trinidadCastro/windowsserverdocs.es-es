---
title: Adición de un registro de recursos DNS
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
ms.assetid: 5379373f-a3d9-4f51-b6fc-bf0f6df1d244
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 868b27a2a2b2005c3cf54d544d2534ae66ae0d98
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849486"
---
# <a name="add-a-dns-resource-record"></a>Adición de un registro de recursos DNS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para agregar uno o más registros de recursos DNS nuevo mediante la consola de cliente IPAM.  
  
El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.  
  
### <a name="to-add-a-dns-resource-record"></a>Para agregar un registro de recursos DNS  
  
1.  En el administrador del servidor, haga clic en **IPAM**. Aparece la consola de cliente IPAM.  
  
2.  En el panel de navegación en **supervisión y administración**, haga clic en **zonas DNS**.  El panel de navegación se divide en un panel de navegación superior y un panel de navegación inferior.  
  
3.  En el panel de navegación inferior, haga clic en **búsqueda directa**. Todas las zonas de búsqueda de DNS administrados por IPAM directa se muestran en los resultados de búsqueda del panel de visualización. Haga clic en la zona donde desea agregar un registro de recursos y, a continuación, haga clic en **registro de recursos DNS agregar**.  
  
    ![Agregar registro de recursos DNS](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_01.jpg)
  
4.  El **agregar registros de recursos DNS** abre el cuadro de diálogo. En **propiedades de registro de recurso**, haga clic en **servidor DNS** y seleccione el servidor DNS en la que desea agregar uno o más registros de recursos nuevo. En **registros de recursos de configuración de DNS**, haga clic en **New**.  
  
    ![Configurar registros de recursos DNS](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_02.jpg)  
  
5.  El cuadro de diálogo se expande para mostrar **nuevo registro de recursos**. Haga clic en **tipo de registro de recurso**.  
  
    ![Tipo de registro de recursos](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_03.jpg)  
  
6.  Se muestra la lista de tipos de registros de recursos. Haga clic en el tipo de registro de recursos que desea agregar.  
  
    ![Seleccione el tipo de registro para agregar](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_04.jpg)  
  
7.  En **nuevo registro de recursos,** en **nombre**, escriba un nombre de registro de recursos. En **dirección IP**, escriba una dirección IP y, a continuación, seleccione las propiedades del registro de recursos que son adecuadas para su implementación. Haga clic en **agregar registro de recursos**.  
  
    ![Agregar registro de recursos](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_06.jpg)  
  
8.  Si no desea crear nuevos registros de recursos adicionales, haga clic en **Aceptar**. Si desea crear nuevos registros de recursos adicionales, haga clic en **New**.  
  
    ![Haga clic en Aceptar o nuevo](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_01.jpg)
  
9. El cuadro de diálogo se expande para mostrar **nuevo registro de recursos**. Haga clic en **tipo de registro de recurso**. Se muestra la lista de tipos de registros de recursos. Haga clic en el tipo de registro de recursos que desea agregar.  
  
10. En **nuevo registro de recursos,** en **nombre**, escriba un nombre de registro de recursos. En **dirección IP**, escriba una dirección IP y, a continuación, seleccione las propiedades del registro de recursos que son adecuadas para su implementación. Haga clic en **agregar registro de recursos**.  
  
    ![Agregar registro de recursos](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_02.jpg)  
  
11. Si desea agregar más registros de recursos, repita el proceso de creación de registros. Cuando haya terminado de crear nuevos registros de recursos, haga clic en **aplicar**.  
  
    ![Creación de registros de recursos completo](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_03.jpg)  
  
12. El **agregar registro de recursos** cuadro de diálogo muestra un resumen de los registros de recursos mientras IPAM crea los registros de recursos en el servidor DNS que especificó. Cuando los registros se crean correctamente, el **estado** del registro es **éxito**.  
  
    ![Estado de la adición de registro](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_04.jpg)  
  
13. Haga clic en **Aceptar**.  
  
## <a name="see-also"></a>Vea también  
[Administración de registros de recursos DNS](DNS-Resource-Record-Management.md)  
[Administrar IPAM](Manage-IPAM.md)  
  


