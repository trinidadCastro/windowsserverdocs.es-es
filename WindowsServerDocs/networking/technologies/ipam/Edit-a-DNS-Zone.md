---
title: Edición de una zona DNS
description: Este tema forma parte de la Guía de administración de administración de direcciones IP (IPAM) en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a35164e1-11ad-47c8-9843-580d30c70d07
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d688790828cb58b9fca2a17c95212c064532bb22
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282166"
---
# <a name="edit-a-dns-zone"></a>Edición de una zona DNS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para editar una zona DNS en la consola de cliente IPAM.  
  
El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.  
  
### <a name="to-edit-a-dns-zone"></a>Para editar una zona DNS  
  
1.  En el administrador del servidor, haga clic en **IPAM**. Aparece la consola de cliente IPAM.  
  
2.  En el panel de navegación en **supervisión y administración**, haga clic en **zonas DNS**. El panel de navegación se divide en un panel de navegación superior y un panel de navegación inferior.  
  
3.  En el panel de navegación inferior, realice una de las selecciones siguientes:  
  
    -   Búsqueda hacia delante  
  
    -   Búsqueda inversa IPv4  
  
    -   Búsqueda inversa IPv6  
  
4.  Por ejemplo, seleccione búsqueda inversa de IPv4.  
  
    ![Seleccione un tipo de zona](../../media/Edit-a-DNS-Zone/ipam_EditZone_01.jpg)  
  
5.  En el panel, haga clic en la zona que desea editar y, a continuación, haga clic en **editar zona de DNS**.  
  
    ![Editar la zona de DNS](../../media/Edit-a-DNS-Zone/ipam_EditZone_02.jpg)  
  
6.  El **editar zona de DNS** abre el cuadro de diálogo con el **General** página seleccionada. Si es necesario, edite las propiedades generales de zona: **Servidor DNS**, **categoría zona**, y **tipo de zona**y, a continuación, haga clic en **aplicar** o, si termine las modificaciones, **Aceptar**.  
  
    ![Editar propiedades de la zona y guardar](../../media/Edit-a-DNS-Zone/ipam_EditZone_03a.jpg)  
  
7.  En el **editar zona de DNS** cuadro de diálogo, haga clic en **avanzadas**. El **avanzadas** abre la página de propiedades de la zona. Si es necesario, modifique las propiedades que desee cambiar y, a continuación, haga clic en **aplicar** o, si termine las modificaciones, **Aceptar**.  
  
    ![Editar propiedades de zona avanzada](../../media/Edit-a-DNS-Zone/ipam_EditZone_04a.jpg)  
  
8.  Si es necesario, seleccione los nombres de página de propiedades de zona adicional (servidores de nombres, SOA, las transferencias de zona), realice los cambios y haga clic en **aplicar** o **Aceptar**. Para revisar todos los cambios de zona, haga clic en **resumen**y, a continuación, haga clic en **Aceptar**.  
  
## <a name="see-also"></a>Vea también  
[Administración de zonas DNS](DNS-Zone-Management.md)  
[Administrar IPAM](Manage-IPAM.md)  
  


