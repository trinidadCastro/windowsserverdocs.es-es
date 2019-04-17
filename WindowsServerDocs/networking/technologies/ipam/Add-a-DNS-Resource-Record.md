---
title: Agregar un registro de recursos DNS
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
ms.assetid: 5379373f-a3d9-4f51-b6fc-bf0f6df1d244
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e14a59e9f172b20e85a34d2299e3733a796adafc
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="add-a-dns-resource-record"></a>Agregar un registro de recursos DNS

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para agregar uno o más registros de recursos DNS nuevo mediante la consola IPAM del cliente.  
  
Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para realizar este procedimiento.  
  
### <a name="to-add-a-dns-resource-record"></a>Para agregar un registro de recursos DNS  
  
1.  En el administrador del servidor, haz clic en **IPAM**. Aparece la consola IPAM del cliente.  
  
2.  En el panel de navegación, en **controlar y administrar**, haz clic en **zonas DNS**.  El panel de navegación se divide en un panel de navegación superior y un panel de navegación inferior.  
  
3.  En el panel de navegación inferior, haga clic en **búsqueda directa**. Todas las zonas de búsqueda de DNS administrado IPAM directa se muestran en los resultados de búsqueda del panel de pantalla. Haz clic en la zona donde quieres agregar un registro de recursos y, a continuación, haz clic en **registro de recursos de DNS agregar**.  
  
    ![Agregar el registro de recursos DNS](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_01.jpg)
  
4.  La **agrega registros de recursos de DNS** abre el cuadro de diálogo. En **propiedades de recurso de registro**, haz clic en **servidor DNS** y selecciona el servidor DNS que quieres agregar uno o más nuevos registros de recursos. En **configurar recursos DNS**, haz clic en **nueva**.  
  
    ![Configurar los registros de recursos DNS](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_02.jpg)  
  
5.  El cuadro de diálogo se expanda para revelar **nuevo registro de recursos**. Haz clic en **tipo de registro de recurso**.  
  
    ![Tipo de registro de recursos](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_03.jpg)  
  
6.  Se muestra la lista de tipos de registros de recursos. Haz clic en el tipo de registro de recursos que quieres agregar.  
  
    ![Selecciona el tipo de registro para agregar](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_04.jpg)  
  
7.  En **nuevo registro de recursos,** en **nombre**, escribe un nombre de registro de recursos. En **dirección IP**, escribe una dirección IP y, a continuación, selecciona las propiedades de registro de recursos que son adecuadas para la implementación. Haz clic en **Agregar recurso registro**.  
  
    ![Agregar el registro de recursos](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_06.jpg)  
  
8.  Si no desea crear nuevos registros de recursos adicionales, haz clic en **Aceptar**. Si quieres crear nuevos registros de recursos adicionales, haz clic en **nueva**.  
  
    ![Haz clic en Aceptar o nueva](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_01.jpg)
  
9. El cuadro de diálogo se expanda para revelar **nuevo registro de recursos**. Haz clic en **tipo de registro de recurso**. Se muestra la lista de tipos de registros de recursos. Haz clic en el tipo de registro de recursos que quieres agregar.  
  
10. En **nuevo registro de recursos,** en **nombre**, escribe un nombre de registro de recursos. En **dirección IP**, escribe una dirección IP y, a continuación, selecciona las propiedades de registro de recursos que son adecuadas para la implementación. Haz clic en **Agregar recurso registro**.  
  
    ![Agregar el registro de recursos](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_02.jpg)  
  
11. Si quieres agregar más registros de recursos, repite el proceso de creación de registros. Cuando haya terminado de crear nuevos registros de recursos, haz clic en **aplicar**.  
  
    ![Creación de registros de recursos completo](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_03.jpg)  
  
12. La **Agregar recurso registro** cuadro de diálogo muestra un resumen de los registros de recursos mientras IPAM crea los registros de recursos en el servidor DNS que especificaste. Cuando los registros se crean correctamente, el **estado** del registro es **éxito**.  
  
    ![Estado de la adición de registro](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_04.jpg)  
  
13. Haz clic en **Aceptar**.  
  
## <a name="see-also"></a>Consulta también  
[Administración de registros de recursos DNS](DNS-Resource-Record-Management.md)  
[Administrar IPAM](Manage-IPAM.md)  
  


