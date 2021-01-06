---
title: Habilitar la publicación de hash para servidores de archivos que son miembros del dominio
description: Obtenga información acerca de cómo habilitar la publicación de hash de BranchCache para varios servidores de archivos.
manager: brianlic
ms.topic: get-started-article
ms.assetid: a3f1f7c4-d9b2-43e6-8bfa-fac707bbd4d3
ms.author: lizross
author: eross-msft
ms.openlocfilehash: beb6c20cf9f750ed0296b96acb6253b5352385fb
ms.sourcegitcommit: 029b1e19ce11160d5f988046e04a83e8ab5a60dc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/05/2021
ms.locfileid: "97904820"
---
# <a name="enable-hash-publication-for-domain-member-file-servers"></a>Habilitar la publicación de hash para servidores de archivos que son miembros del dominio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Cuando use Active Directory Domain Services (AD DS), puede usar directiva de grupo de dominio para habilitar la publicación de hash de BranchCache para varios servidores de archivos. Para ello, debe crear una unidad organizativa (OU), agregar servidores de archivos a la unidad organizativa, crear una publicación de hash de BranchCache directiva de grupo objeto (GPO) y, a continuación, configurar el GPO.

Vea los temas siguientes para habilitar la publicación de hash para varios servidores de archivos.

-   [Crear la unidad organizativa para servidores de archivos de BranchCache](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md)

-   [Traslado de servidores de archivos a la unidad organizativa de servidores de archivos de BranchCache](../../branchcache/deploy/Move-File-Servers-to-the-BranchCache-File-Servers-Organizational-Unit.md)

-   [Crear el objeto de directiva de grupo Publicación de hash para BranchCache](../../branchcache/deploy/Create-the-BranchCache-Hash-Publication-Group-Policy-Object.md)

-   [Configurar el objeto de directiva de grupo de publicación de hash de BranchCache](../../branchcache/deploy/Configure-the-BranchCache-Hash-Publication-Group-Policy-Object.md)



