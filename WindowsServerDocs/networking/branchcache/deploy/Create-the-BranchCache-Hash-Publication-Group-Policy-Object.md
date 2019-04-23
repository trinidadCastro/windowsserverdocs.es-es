---
title: Crear el objeto de directiva de grupo Publicación de hash para BranchCache
description: En este tema forma parte de BranchCache Deployment Guide para Windows Server 2016, que demuestra cómo implementar BranchCache en los modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN de sucursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: c3d33bed-83ef-4eb8-acf9-0719ecb4a931
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 15e89b961d20b6f14065392553e413374358062d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865436"
---
# <a name="create-the-branchcache-hash-publication-group-policy-object"></a>Crear el objeto de directiva de grupo Publicación de hash para BranchCache

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para crear el objeto de directiva de grupo (GPO) publicación de hash para BranchCache.  
  
El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Admins. del dominio** o grupo equivalente.  
  
> [!NOTE]  
> Antes de realizar este procedimiento, debe crear la unidad organizativa para servidores de archivos de BranchCache y mover los servidores de archivos a la unidad organizativa. Para obtener más información, consulte [habilitar la publicación de Hash para servidores de archivos del miembro de dominio](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md).  
  
### <a name="to-create-the-branchcache-hash-publication-group-policy-object"></a>Para crear el objeto de directiva de grupo de la publicación de hash de BranchCache  
  
1.  Abra Windows PowerShell, escriba **mmc**y presione ENTRAR. Se abrirá Microsoft Management Console (MMC).  
  
2.  En MMC, en el menú **Archivo**, haga clic en **Agregar o quitar complemento**. Se abre el cuadro de diálogo **Agregar o quitar complementos**.  
  
3.  En **Agregar o quitar complementos**, en **Complementos disponibles**, haga doble clic en **Administración de directivas de grupo** y, a continuación, haga clic en **Aceptar**.  
  
4.  En MMC de Administración de directivas de grupo, expanda la ruta de acceso a la unidad organizativa para servidores de archivos de BranchCache que creó previamente. Por ejemplo, si el nombre de su bosque es ejemplo.com, el de su dominio es ejemplo1.com y el de su unidad organizativa es Servidores de archivo de BranchCache, expanda la siguiente ruta de acceso: **Administración de directivas de grupo**, **bosque: ejemplo.com**, **dominios**, **ejemplo1.com**, **servidores de archivos de BranchCache**.  
  
5.  Haga clic en **servidores de archivos de BranchCache**y, a continuación, haga clic en **crear un GPO en este dominio y vincularlo aquí**. Se abre el cuadro de diálogo **Nuevo GPO**. En **nombre**, escriba un nombre para el nuevo GPO. Por ejemplo, si desea que el nombre del objeto sea Publicación de hash para BranchCache, escriba **Publicación de hash para BranchCache**. Haga clic en **Aceptar**.  
  


