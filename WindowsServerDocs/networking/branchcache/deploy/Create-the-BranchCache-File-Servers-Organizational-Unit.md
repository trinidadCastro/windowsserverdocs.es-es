---
title: Crear la unidad organizativa de servidores de archivos BranchCache
description: En este tema es parte de la BranchCache implementación Guía para Windows Server 2016, que se muestra cómo implementar BranchCache en modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN en sucursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 2cda192f-6b45-4e6c-88d9-70ca179ddb94
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a92cb3110e4aecb1ef09a45ed14173305722c655
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="create-the-branchcache-file-servers-organizational-unit"></a>Crear la unidad organizativa de servidores de archivos BranchCache

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este procedimiento para crear una unidad organizativa (OU) en servicios de dominio de Active Directory (AD DS) BranchCache para servidores de archivos.  
  
Pertenencia a **administradores de dominio**, o equivalente, es lo mínimo necesario para realizar este procedimiento.  
  
### <a name="to-create-the-branchcache-file-servers-organizational-unit"></a>Para crear la unidad organizativa de BranchCache archivo servidores  
  
1.  En un equipo donde está instalado AD DS, en el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **equipos y usuarios de Active Directory**. Abre la consola de equipos y usuarios de Active Directory.  
  
2.  En la consola usuarios y equipos de usuarios de Active Directory, haz clic en el dominio al que quieres agregar una unidad organizativa. Por ejemplo, si el dominio se denomina example.com, haga clic **example.com**. Elija **nueva**y, a continuación, haz clic en **unidad organizativa**. La **nuevo objeto - unidad organizativa** abre el cuadro de diálogo.  
  
3.  En la **nuevo objeto - unidad organizativa** cuadro de diálogo **nombre**, escribe un nombre para la nueva unidad organizativa. Por ejemplo, si quieres que los servidores de archivos de la unidad organizativa BranchCache el nombre, escribe **servidores de archivos BranchCache**y, a continuación, haz clic en **Aceptar**.  
  


