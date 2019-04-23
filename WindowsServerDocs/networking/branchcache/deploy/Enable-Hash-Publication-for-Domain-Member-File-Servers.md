---
title: Habilitar la publicación de hash para servidores de archivos que son miembros del dominio
description: En este tema forma parte de BranchCache Deployment Guide para Windows Server 2016, que demuestra cómo implementar BranchCache en los modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN de sucursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: a3f1f7c4-d9b2-43e6-8bfa-fac707bbd4d3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 174e83c950d2aff8afba4f05641a74861b9a7938
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865466"
---
# <a name="enable-hash-publication-for-domain-member-file-servers"></a>Habilitar la publicación de hash para servidores de archivos que son miembros del dominio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Cuando usa servicios de dominio de Active Directory (AD DS), puede usar la directiva de grupo de dominio para habilitar la publicación de hash para BranchCache para varios servidores de archivos. Para ello, debe crear una unidad organizativa (OU), agregar servidores de archivos a la unidad organizativa, cree una objeto de directiva de grupo (GPO) publicación de hash para BranchCache y, a continuación, configure el GPO.  
  
Vea los temas siguientes para habilitar la publicación de hash para varios servidores de archivos.  
  
-   [Crear la unidad organizativa de los servidores de archivos de BranchCache](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md)  
  
-   [Mover servidores de archivos a la unidad organizativa de los servidores de archivos de BranchCache](../../branchcache/deploy/Move-File-Servers-to-the-BranchCache-File-Servers-Organizational-Unit.md)  
  
-   [Crear el objeto de directiva de grupo de publicación de Hash de BranchCache](../../branchcache/deploy/Create-the-BranchCache-Hash-Publication-Group-Policy-Object.md)  
  
-   [Configurar el objeto de directiva de grupo de publicación de Hash de BranchCache](../../branchcache/deploy/Configure-the-BranchCache-Hash-Publication-Group-Policy-Object.md)  
  


