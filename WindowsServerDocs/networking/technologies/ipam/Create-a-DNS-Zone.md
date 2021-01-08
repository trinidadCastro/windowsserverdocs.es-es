---
title: Creación de un alias DNS
description: Obtenga información sobre cómo crear una zona DNS mediante la consola de cliente de IPAM.
manager: brianlic
ms.topic: article
ms.assetid: a030ff51-a815-4fc4-b26d-aae41c3e4ce5
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 04e1e393a5f434ae171ed25da4324955bea4e595
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98039695"
---
# <a name="create-a-dns-zone"></a>Creación de un alias DNS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para crear una zona DNS mediante la consola de cliente de IPAM.

El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.

### <a name="to-create-a-dns-zone"></a>Creación de una zona DNS

1.  En Administrador del servidor, haga clic en  **IPAM**. Aparece la consola de cliente de IPAM.

2.  En el panel de navegación, en **supervisión y administración**, haga clic en **servidores DNS y DHCP**. En el panel de información, haga clic en **tipo de servidor** y, a continuación, haga clic en **DNS**. Todos los servidores DNS administrados por IPAM se enumeran en los resultados de la búsqueda.

3.  Busque el servidor en el que desea agregar una zona y haga clic con el botón secundario en el servidor.  Haga clic en **crear zona DNS**.

    ![Creación de una zona DNS](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_01a.jpg)

4.  Se abre el cuadro de diálogo **crear zona DNS** . En **propiedades generales**, seleccione una categoría de zona, un tipo de zona y escriba un nombre en **nombre de zona**. Seleccione también los valores adecuados para la implementación en **propiedades avanzadas** y, a continuación, haga clic en **Aceptar**.

    ![Propiedades avanzadas](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_02a.jpg)

## <a name="see-also"></a>Consulte también
Administración de zonas [DNS](DNS-Zone-Management.md) 
 [Administrar IPAM](Manage-IPAM.md)



