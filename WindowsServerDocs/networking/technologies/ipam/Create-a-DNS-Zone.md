---
title: Creación de un alias DNS
description: Este tema forma parte de la guía de administración de la administración de direcciones IP (IPAM) en Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: a030ff51-a815-4fc4-b26d-aae41c3e4ce5
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 0635c91b3dd61db6df41c2836cc5efc65d9de520
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87970792"
---
# <a name="create-a-dns-zone"></a>Creación de un alias DNS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para crear una zona DNS mediante la consola de cliente de IPAM.

El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.

### <a name="to-create-a-dns-zone"></a>Creación de una zona DNS

1.  En Administrador del servidor, haga clic en **IPAM**. Aparece la consola de cliente de IPAM.

2.  En el panel de navegación, en **supervisión y administración**, haga clic en **servidores DNS y DHCP**. En el panel de información, haga clic en **tipo de servidor**y, a continuación, haga clic en **DNS**. Todos los servidores DNS administrados por IPAM se enumeran en los resultados de la búsqueda.

3.  Busque el servidor en el que desea agregar una zona y haga clic con el botón secundario en el servidor.  Haga clic en **crear zona DNS**.

    ![Creación de una zona DNS](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_01a.jpg)

4.  Se abre el cuadro de diálogo **crear zona DNS** . En **propiedades generales**, seleccione una categoría de zona, un tipo de zona y escriba un nombre en **nombre de zona**. Seleccione también los valores adecuados para la implementación en **propiedades avanzadas**y, a continuación, haga clic en **Aceptar**.

    ![Propiedades avanzadas](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_02a.jpg)

## <a name="see-also"></a>Consulte también
Administración de zonas [DNS](DNS-Zone-Management.md) 
 [Administrar IPAM](Manage-IPAM.md)



