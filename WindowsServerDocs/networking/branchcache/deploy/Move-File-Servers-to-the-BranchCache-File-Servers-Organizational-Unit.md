---
title: Mover servidores de archivos a la unidad organizativa para servidores de archivos de BranchCache
description: En este tema forma parte de BranchCache Deployment Guide para Windows Server 2016, que demuestra cómo implementar BranchCache en los modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN de sucursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 56c915ec-edb1-43b0-8ad2-c93841bb566f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 037b354bb6725ac7f91fc323b81bbdf15d03ac15
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885906"
---
# <a name="move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>Mover servidores de archivos a la unidad organizativa para servidores de archivos de BranchCache

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este procedimiento para agregar servidores de archivos de BranchCache a una unidad organizativa (OU) en Servicios de dominio de Active Directory (AD DS).  
  
El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Admins. del dominio** o grupo equivalente.  
  
> [!NOTE]  
> Debe crear una unidad organizativa para servidores de archivos de BranchCache en la Consola de usuarios y equipos de Active Directory antes de agregar cuentas de equipo a la unidad organizativa con este procedimiento. Para obtener más información, consulte [crear unidad organizativa de servidores de archivos de BranchCache](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md).  
  
### <a name="to-move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>Para mover servidores de archivos a la unidad organizativa para servidores de archivos de BranchCache  
  
1.  En un equipo donde se instala AD DS, en el administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **equipos y usuarios de Active Directory**. Se abre la Consola de usuarios y equipos de Active Directory.  
  
2.  En la consola de usuarios y equipos de Active Directory, busque la cuenta de equipo para un servidor de archivos de BranchCache, haga clic con el botón primario para seleccionar la cuenta y, a continuación, arrastre y coloque la cuenta de equipo en la unidad organizativa para servidores de archivos de BranchCache que creó previamente. Por ejemplo, si ha creado previamente una unidad organizativa denominada **servidores de archivos de BranchCache**, arrastre y coloque la cuenta de equipo en el **servidores de archivos de BranchCache** OU.  
  
3.  Repita el paso anterior para cada servidor de archivos de BranchCache en el dominio que desea mover a la unidad organizativa.  
  


