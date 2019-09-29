---
title: Crear el objeto de directiva de grupo Publicación de hash para BranchCache
description: Este tema forma parte de la guía de implementación de BranchCache para Windows Server 2016, que muestra cómo implementar BranchCache en los modos de caché distribuida y hospedada para optimizar el uso del ancho de banda WAN en las sucursales.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: c3d33bed-83ef-4eb8-acf9-0719ecb4a931
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 10230b57075943a5d92dce7155e794293157cba4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356639"
---
# <a name="create-the-branchcache-hash-publication-group-policy-object"></a>Crear el objeto de directiva de grupo Publicación de hash para BranchCache

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para crear la publicación de hash de BranchCache directiva de grupo objeto (GPO).  
  
El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Admins. del dominio** o grupo equivalente.  
  
> [!NOTE]  
> Antes de realizar este procedimiento, debe crear la unidad organizativa para servidores de archivos de BranchCache y mover los servidores de archivos a la unidad organizativa. Para obtener más información, vea [Habilitar la publicación de hash para servidores de archivos de miembros de dominio](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md).  
  
### <a name="to-create-the-branchcache-hash-publication-group-policy-object"></a>Para crear la publicación de hash de BranchCache directiva de grupo objeto  
  
1.  Abra Windows PowerShell, escriba **mmc**y presione ENTRAR. Se abrirá Microsoft Management Console (MMC).  
  
2.  En MMC, en el menú **Archivo**, haga clic en **Agregar o quitar complemento**. Se abre el cuadro de diálogo **Agregar o quitar complementos**.  
  
3.  En **Agregar o quitar complementos**, en **Complementos disponibles**, haga doble clic en **Administración de directivas de grupo** y, a continuación, haga clic en **Aceptar**.  
  
4.  En MMC de Administración de directivas de grupo, expanda la ruta de acceso a la unidad organizativa para servidores de archivos de BranchCache que creó previamente. Por ejemplo, si el nombre de su bosque es ejemplo.com, el de su dominio es ejemplo1.com y el de su unidad organizativa es Servidores de archivo de BranchCache, expanda la siguiente ruta de acceso: **Administración de directiva de grupo**, **bosque: example.com**, **dominios**, **example1.com**, servidores de **archivos de BranchCache**.  
  
5.  Haga clic con el botón secundario en **servidores de archivos de BranchCache**y, a continuación, haga clic en **crear un GPO en este dominio y vincularlo aquí**. Se abre el cuadro de diálogo **Nuevo GPO**. En **nombre**, escriba un nombre para el nuevo GPO. Por ejemplo, si desea que el nombre del objeto sea Publicación de hash para BranchCache, escriba **Publicación de hash para BranchCache**. Haga clic en **Aceptar**.  
  


