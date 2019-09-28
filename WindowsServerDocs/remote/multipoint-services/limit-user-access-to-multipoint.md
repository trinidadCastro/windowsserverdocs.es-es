---
title: Limitar el acceso de los usuarios al servidor
description: Obtenga información sobre cómo conceder o denegar el acceso a multipoint Services para usuarios y grupos
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4cabd4f1-a764-4be6-bc6e-0a5f5566390c
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 62f2a3f9b94ac3f0474636c34e8ec1f81c568cad
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389059"
---
# <a name="limit-users-access-to-the-multipoint-server"></a>Limitar el acceso de los usuarios al servidor Multipoint
Tanto si une MultiPoint Server a un dominio de Active Directory como si usa cuentas de usuario locales, todos los usuarios tendrán acceso a MultiPoint Server de forma predeterminada. Antes de permitir que los usuarios inicien sesión en estaciones en el entorno de Multipoint Services, debe restringir el acceso al servidor.  
  
Cualquier usuario del grupo Escritorio remoto usuarios puede iniciar sesión en MultiPoint Server. De forma predeterminada, el grupo de usuarios todos es miembro del grupo Escritorio remoto usuarios y, por lo tanto, cada usuario local y usuario de dominio pueden iniciar sesión en el servidor multipoint. Para restringir el acceso a MultiPoint Server, quite el grupo de usuarios todos del grupo Escritorio remoto usuarios y, a continuación, agregue usuarios o grupos específicos al grupo usuarios de Escritorio remoto.  
  
## <a name="add-or-remove-users-or-groups-to-the-remote-desktop-users-group"></a>Adición o eliminación de usuarios o grupos al grupo de usuarios de Escritorio remoto  
  
1.  En la pantalla **Inicio** , Abra **Administración de equipos**.  
  
2.  En el árbol de consola, en **usuarios y grupos locales**, haga clic en **grupos**.  
  
3.  Haga doble clic en **escritorio remoto usuarios**y siga las instrucciones para agregar o quitar usuarios.  
  
    -   Para restringir el acceso general al servidor, quite el grupo todos.  
  
    -   Para conceder a los usuarios de MultiPoint Server acceso a las estaciones, agregue cada cuenta local o cada usuario de dominio o cuenta de grupo al grupo Escritorio remoto usuarios.  