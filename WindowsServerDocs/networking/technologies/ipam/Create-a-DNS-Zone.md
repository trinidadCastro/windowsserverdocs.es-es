---
title: Creación de una zona DNS
description: Este tema forma parte de la guía de administración de la administración de direcciones IP (IPAM) en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a030ff51-a815-4fc4-b26d-aae41c3e4ce5
ms.author: lizross
author: eross-msft
ms.openlocfilehash: a9762d15d0b95954623bbefdec38696885676975
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312636"
---
# <a name="create-a-dns-zone"></a>Creación de una zona DNS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para crear una zona DNS mediante la consola de cliente de IPAM.  
  
El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.  
  
### <a name="to-create-a-dns-zone"></a>Para crear una zona DNS  
  
1.  En Administrador del servidor, haga clic en **IPAM**. Aparece la consola de cliente de IPAM.  
  
2.  En el panel de navegación, en **supervisión y administración**, haga clic en **servidores DNS y DHCP**. En el panel de información, haga clic en **tipo de servidor**y, a continuación, haga clic en **DNS**. Todos los servidores DNS administrados por IPAM se enumeran en los resultados de la búsqueda.  
  
3.  Busque el servidor en el que desea agregar una zona y haga clic con el botón secundario en el servidor.  Haga clic en **crear zona DNS**.  
  
    ![Crear zona DNS](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_01a.jpg)  
  
4.  Se abre el cuadro de diálogo **crear zona DNS** . En **propiedades generales**, seleccione una categoría de zona, un tipo de zona y escriba un nombre en **nombre de zona**. Seleccione también los valores adecuados para la implementación en **propiedades avanzadas**y, a continuación, haga clic en **Aceptar**.  
  
    ![Propiedades avanzadas](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_02a.jpg)  
  
## <a name="see-also"></a>Consulta también  
[Administración de zonas DNS](DNS-Zone-Management.md)  
[Administrar IPAM](Manage-IPAM.md)  
  


