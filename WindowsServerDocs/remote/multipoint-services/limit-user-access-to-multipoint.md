---
title: Limitar el acceso de usuario al servidor
description: Obtenga información sobre cómo conceder o denegar el acceso a MultiPoint Services para usuarios y grupos
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4cabd4f1-a764-4be6-bc6e-0a5f5566390c
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 1466e19152847a6c7d88f77162c50ec73a5a7d27
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830816"
---
# <a name="limit-users-access-to-the-multipoint-server"></a>Limitar el acceso de usuarios al servidor Multipoint
Si se une el servidor MultiPoint a un dominio de Active Directory o si usa cuentas de usuario local, todos los usuarios tienen acceso a MultiPoint server de forma predeterminada. Antes de permitir que los usuarios inicien sesión en las estaciones en su entorno de MultiPoint Services, debe restringir el acceso al servidor.  
  
Cualquier usuario en el grupo de usuarios de escritorio remoto puede iniciar sesión en MultiPoint server. De forma predeterminada, el grupo de usuarios todos los usuarios es un miembro del grupo usuarios de escritorio remoto y, por lo tanto, cada usuario local y el usuario de dominio pueden iniciar sesión en el servidor MultiPoint. Para restringir el acceso al servidor MultiPoint, quite el todos usuario grupo desde el grupo de usuarios de escritorio remoto y, a continuación, agregar usuarios o grupos específicos para el grupo de usuarios de escritorio remoto.  
  
## <a name="add-or-remove-users-or-groups-to-the-remote-desktop-users-group"></a>Agregar o quitar usuarios o grupos al grupo usuarios de escritorio remoto  
  
1.  Desde el **iniciar** pantalla, abra **administración de equipos**.  
  
2.  En el árbol de consola, bajo **usuarios y grupos locales**, haga clic en **grupos**.  
  
3.  Haga doble clic en **Remote Desktop Users**y siga las instrucciones para agregar o quitar usuarios.  
  
    -   Para restringir el acceso general al servidor, quite el grupo todos.  
  
    -   Para asignar al servidor MultiPoint a los usuarios acceso a las estaciones, agregue cada cuenta local o cada cuenta de usuario o grupo de dominio al grupo usuarios de escritorio remoto.  