---
title: Creación de una zona DNS
description: Este tema forma parte de la Guía de administración de administración de direcciones IP (IPAM) en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a030ff51-a815-4fc4-b26d-aae41c3e4ce5
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 83e3865308fd45e88b800753b20ab298f9a14c96
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282251"
---
# <a name="create-a-dns-zone"></a>Creación de una zona DNS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para crear una zona DNS mediante la consola de cliente IPAM.  
  
El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.  
  
### <a name="to-create-a-dns-zone"></a>Para crear una zona DNS  
  
1.  En el administrador del servidor, haga clic en **IPAM**. Aparece la consola de cliente IPAM.  
  
2.  En el panel de navegación en **supervisión y administración**, haga clic en **servidores DNS y DHCP**. En el panel de información, haga clic en **tipo de servidor**y, a continuación, haga clic en **DNS**. Todos los servidores DNS administrados por IPAM se muestran en los resultados de búsqueda.  
  
3.  Busque el servidor donde desea agregar una zona y haga clic en el servidor.  Haga clic en **crear zona DNS**.  
  
    ![Crear zona DNS](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_01a.jpg)  
  
4.  El **crear zona de DNS** abre el cuadro de diálogo. En **propiedades generales**, seleccione una categoría de zona, un tipo de zona y escriba un nombre en **nombre de la zona**. Seleccione también los valores adecuados para su implementación en **propiedades avanzadas**y, a continuación, haga clic en **Aceptar**.  
  
    ![Propiedades avanzadas](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_02a.jpg)  
  
## <a name="see-also"></a>Vea también  
[Administración de zonas DNS](DNS-Zone-Management.md)  
[Administrar IPAM](Manage-IPAM.md)  
  


