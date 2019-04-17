---
title: Mover los servidores de archivos a la unidad organizativa de servidores de archivos BranchCache
description: En este tema es parte de la BranchCache implementación Guía para Windows Server 2016, que se muestra cómo implementar BranchCache en modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN en sucursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 56c915ec-edb1-43b0-8ad2-c93841bb566f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 09afb4936545cb1f5bb14573261008ff18badd4d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>Mover los servidores de archivos a la unidad organizativa de servidores de archivos BranchCache

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este procedimiento para agregar BranchCache servidores de archivos a una unidad organizativa (OU) en los servicios de dominio de Active Directory (AD DS).  
  
Pertenencia a **administradores de dominio**, o equivalente, es lo mínimo necesario para realizar este procedimiento.  
  
> [!NOTE]  
> Debes crear una unidad organizativa BranchCache los servidores de archivos en la consola de equipos y usuarios de Active Directory antes de agregar cuentas de equipo a la unidad organizativa con este procedimiento. Para obtener más información, consulta [crear la unidad organizativa de BranchCache archivo servidores](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md).  
  
### <a name="to-move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>Para mover los servidores de archivos para la BranchCache archivo unidad organizativa de servidores  
  
1.  En un equipo donde está instalado AD DS, en el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **equipos y usuarios de Active Directory**. Abre la consola de equipos y usuarios de Active Directory.  
  
2.  En la consola usuarios y equipos de usuarios de Active Directory, busque la cuenta de equipo para un servidor de archivos BranchCache, hacer clic para seleccionar la cuenta y, a continuación, arrastra y coloca la cuenta de equipo en los servidores de archivos BranchCache unidad organizativa que creaste anteriormente. Por ejemplo, si ya has creado una unidad organizativa denominada **servidores de archivos BranchCache**, arrastrar y colocar la cuenta de equipo en el **servidores de archivos BranchCache** unidad organizativa.  
  
3.  Repite el paso anterior para cada servidor de archivos BranchCache en el dominio que quieras mover a la unidad organizativa.  
  


