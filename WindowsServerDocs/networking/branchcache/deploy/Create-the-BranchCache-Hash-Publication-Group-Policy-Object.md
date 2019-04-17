---
title: Crear el objeto de directiva de grupo de publicación de Hash de BranchCache
description: En este tema es parte de la BranchCache implementación Guía para Windows Server 2016, que se muestra cómo implementar BranchCache en modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN en sucursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: c3d33bed-83ef-4eb8-acf9-0719ecb4a931
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4510d13c806adc2db46dfccce02a5a1b2814a2c4
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="create-the-branchcache-hash-publication-group-policy-object"></a>Crear el objeto de directiva de grupo de publicación de Hash de BranchCache

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este procedimiento para crear la publicación de hash BranchCache objeto de directiva de grupo (GPO).  
  
Pertenencia a **administradores de dominio**, o equivalente, es lo mínimo necesario para realizar este procedimiento.  
  
> [!NOTE]  
> Antes de realizar este procedimiento, debes crear la unidad organizativa de BranchCache archivo servidores y mover los servidores de archivos en la unidad organizativa. Para obtener más información, consulta [habilitar Hash de publicación para servidores de archivos de miembro de dominio](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md).  
  
### <a name="to-create-the-branchcache-hash-publication-group-policy-object"></a>Para crear la publicación de hash BranchCache objeto de directiva de grupo  
  
1.  Abre Windows PowerShell, escribe **mmc**, y, a continuación, presione ENTRAR. Abre Microsoft Management Console (MMC).  
  
2.  En la consola de MMC, en la **archivo** menú, haz clic en **agregar o quitar complemento**. La **agregar o quitar complementos** abre el cuadro de diálogo.  
  
3.  En **agregar o quitar complementos**, en **complementos disponibles**, haz doble clic en **Group Policy Management**y, a continuación, haz clic en **Aceptar**.  
  
4.  En MMC de administración de directiva de grupo, expanda la ruta de acceso a los servidores de archivos BranchCache unidad organizativa que creaste anteriormente. Por ejemplo, si el bosque se denomina example.com, el dominio se denomina example1.com y la unidad organizativa se denomina BranchCache servidores de archivos, expanda la siguiente ruta de acceso: **Group Policy Management**, **bosque: example.com**, **dominios**, **example1.com**, **servidores de archivos BranchCache**.  
  
5.  Haz clic en **servidores de archivos BranchCache**y, a continuación, haz clic en **crear un GPO en este dominio y vincularlo aquí**. La **nuevo GPO** abre el cuadro de diálogo. En **nombre**, escribe un nombre para el nuevo GPO. Por ejemplo, si quieres nombrar el objeto BranchCache Hash de publicación, escriba **BranchCache Hash de publicación**. Haz clic en **Aceptar**.  
  


