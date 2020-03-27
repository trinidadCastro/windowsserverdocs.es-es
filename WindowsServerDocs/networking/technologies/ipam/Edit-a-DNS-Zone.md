---
title: Edición de una zona DNS
description: Este tema forma parte de la guía de administración de la administración de direcciones IP (IPAM) en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a35164e1-11ad-47c8-9843-580d30c70d07
ms.author: lizross
author: eross-msft
ms.openlocfilehash: b2fa8ff6742dc393a5a2cc962c7f849049b5ef31
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312420"
---
# <a name="edit-a-dns-zone"></a>Edición de una zona DNS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para editar una zona DNS en la consola de cliente de IPAM.  
  
El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.  
  
### <a name="to-edit-a-dns-zone"></a>Para editar una zona DNS  
  
1.  En Administrador del servidor, haga clic en **IPAM**. Aparece la consola de cliente de IPAM.  
  
2.  En el panel de navegación, en **supervisión y administración**, haga clic en **zonas DNS**. El panel de navegación se divide en un panel de navegación superior y un panel de navegación inferior.  
  
3.  En el panel de navegación inferior, realice una de las selecciones siguientes:  
  
    -   Búsqueda directa  
  
    -   Búsqueda inversa IPv4  
  
    -   Búsqueda inversa IPv6  
  
4.  Por ejemplo, seleccione búsqueda inversa IPv4.  
  
    ![Seleccionar un tipo de zona](../../media/Edit-a-DNS-Zone/ipam_EditZone_01.jpg)  
  
5.  En el panel de información, haga clic con el botón secundario en la zona que desea editar y, a continuación, haga clic en **Editar zona DNS**.  
  
    ![Editar zona DNS](../../media/Edit-a-DNS-Zone/ipam_EditZone_02.jpg)  
  
6.  Se abre el cuadro de diálogo **Editar zona DNS** con la página **General** seleccionada. Si es necesario, edite las propiedades de la zona general: **servidor DNS**, **categoría de zona**y **tipo de zona**y, a continuación, haga clic en **aplicar** o, si las ediciones están completas, en **Aceptar**.  
  
    ![Editar propiedades de la zona y guardar](../../media/Edit-a-DNS-Zone/ipam_EditZone_03a.jpg)  
  
7.  En el cuadro de diálogo **Editar zona DNS** , haga clic en **Opciones avanzadas**. Se abre la página propiedades **avanzadas** de la zona. Si es necesario, edite las propiedades que desea cambiar y, a continuación, haga clic en **aplicar** o, si las ediciones se han completado, en **Aceptar**.  
  
    ![Editar propiedades de zona avanzadas](../../media/Edit-a-DNS-Zone/ipam_EditZone_04a.jpg)  
  
8.  Si es necesario, seleccione los nombres de página de propiedades de zona adicionales (servidores de nombres, SOA, transferencias de zona), realice las modificaciones y haga clic en **aplicar** o en **Aceptar**. Para revisar todas las ediciones de la zona, haga clic en **Resumen**y, a continuación, haga clic en **Aceptar**.  
  
## <a name="see-also"></a>Consulta también  
[Administración de zonas DNS](DNS-Zone-Management.md)  
[Administrar IPAM](Manage-IPAM.md)  
  


