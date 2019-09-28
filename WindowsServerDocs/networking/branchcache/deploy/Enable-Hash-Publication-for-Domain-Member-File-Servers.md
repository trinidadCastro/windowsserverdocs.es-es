---
title: Habilitar la publicación de hash para servidores de archivos que son miembros del dominio
description: Este tema forma parte de la guía de implementación de BranchCache para Windows Server 2016, que muestra cómo implementar BranchCache en los modos de caché distribuida y hospedada para optimizar el uso del ancho de banda WAN en las sucursales.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: a3f1f7c4-d9b2-43e6-8bfa-fac707bbd4d3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1e450b9a2282cb4820b8802aa6d36e822f56ca12
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356590"
---
# <a name="enable-hash-publication-for-domain-member-file-servers"></a>Habilitar la publicación de hash para servidores de archivos que son miembros del dominio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Cuando use Active Directory Domain Services (AD DS), puede usar directiva de grupo de dominio para habilitar la publicación de hash de BranchCache para varios servidores de archivos. Para ello, debe crear una unidad organizativa (OU), agregar servidores de archivos a la unidad organizativa, crear una publicación de hash de BranchCache directiva de grupo objeto (GPO) y, a continuación, configurar el GPO.  
  
Vea los temas siguientes para habilitar la publicación de hash para varios servidores de archivos.  
  
-   [Crear la unidad organizativa de servidores de archivos de BranchCache](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md)  
  
-   [Traslado de servidores de archivos a la unidad organizativa de servidores de archivos de BranchCache](../../branchcache/deploy/Move-File-Servers-to-the-BranchCache-File-Servers-Organizational-Unit.md)  
  
-   [Crear el objeto de directiva de grupo de publicación de hash de BranchCache](../../branchcache/deploy/Create-the-BranchCache-Hash-Publication-Group-Policy-Object.md)  
  
-   [Configurar el objeto de directiva de grupo de publicación de hash de BranchCache](../../branchcache/deploy/Configure-the-BranchCache-Hash-Publication-Group-Policy-Object.md)  
  


