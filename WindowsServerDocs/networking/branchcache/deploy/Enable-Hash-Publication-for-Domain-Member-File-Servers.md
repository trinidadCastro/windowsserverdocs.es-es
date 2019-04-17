---
title: Habilitar el Hash de publicación para servidores de archivos de miembro de dominio
description: En este tema es parte de la BranchCache implementación Guía para Windows Server 2016, que se muestra cómo implementar BranchCache en modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN en sucursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: a3f1f7c4-d9b2-43e6-8bfa-fac707bbd4d3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 318879eae82d37f68acbc18cdb21ae5290f6d02b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="enable-hash-publication-for-domain-member-file-servers"></a>Habilitar el Hash de publicación para servidores de archivos de miembro de dominio

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Cuando estás usando los servicios de dominio de Active Directory (AD DS), puedes usar la directiva de grupo de dominio para habilitar la publicación de hash de BranchCache para varios servidores de archivos. Para ello, debes crear una unidad organizativa (OU), agregar servidores de archivos a la unidad organizativa, crear una publicación de hash BranchCache objeto de directiva de grupo (GPO) y, a continuación, configura el GPO.  
  
Consulta los siguientes temas para habilitar la publicación de hash para varios servidores de archivos.  
  
-   [Crear la unidad organizativa de servidores de archivos BranchCache](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md)  
  
-   [Mover los servidores de archivos a la unidad organizativa de servidores de archivos BranchCache](../../branchcache/deploy/Move-File-Servers-to-the-BranchCache-File-Servers-Organizational-Unit.md)  
  
-   [Crear el objeto de directiva de grupo de publicación de Hash de BranchCache](../../branchcache/deploy/Create-the-BranchCache-Hash-Publication-Group-Policy-Object.md)  
  
-   [Configurar el objeto de directiva de grupo de publicación de Hash de BranchCache](../../branchcache/deploy/Configure-the-BranchCache-Hash-Publication-Group-Policy-Object.md)  
  


