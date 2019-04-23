---
title: Configurar el objeto de directiva de grupo Publicación de hash para BranchCache
description: En este tema forma parte de BranchCache Deployment Guide para Windows Server 2016, que demuestra cómo implementar BranchCache en los modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN de sucursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: da74fea7-52b2-4d6d-9d21-19184eedbe3c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6e9a338dfebfb1dfadb258bcbdfcc8d75bd3ea86
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862966"
---
# <a name="configure-the-branchcache-hash-publication-group-policy-object"></a>Configurar el objeto de directiva de grupo Publicación de hash para BranchCache

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para configurar el objeto de directiva de grupo (GPO) publicación de hash para BranchCache para que todos los servidores de archivos que ha agregado a la unidad organizativa tengan la misma configuración de directiva de publicación de hash que se les aplica.  
  
El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Admins. del dominio** o grupo equivalente.  
  
> [!NOTE]  
> Antes de realizar este procedimiento, debe crear la unidad organizativa de la servidores de archivos BranchCache, mover los servidores de archivos en la unidad organizativa y crear la publicación de hash para BranchCache GPO. Para obtener más información, consulte [habilitar la publicación de Hash para servidores de archivos del miembro de dominio](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md).  
  
### <a name="to-configure-the-branchcache-hash-publication-group-policy-object"></a>Para configurar la publicación de hash para BranchCache objeto directiva de grupo  
  
1.  Ejecutar Windows PowerShell como administrador, tipo **mmc**, y, a continuación, presione ENTRAR. Se abrirá Microsoft Management Console (MMC).  
  
2.  En MMC, en el menú **Archivo**, haga clic en **Agregar o quitar complemento**. Se abre el cuadro de diálogo **Agregar o quitar complementos**.  
  
3.  En **Agregar o quitar complementos**, en **Complementos disponibles**, haga doble clic en **Administración de directivas de grupo** y, a continuación, haga clic en **Aceptar**.  
  
4.  En MMC de Administración de directivas de grupo, expanda la ruta de acceso al objeto de directiva de grupo Publicación de hash para BranchCache que creó previamente. Por ejemplo, si el nombre de su bosque es ejemplo.com, el de su dominio es ejemplo1.com y el de su objeto de directiva de grupo es **Publicación de hash para BranchCache**, expanda la siguiente ruta de acceso: **Administración de directivas de grupo**, **bosque: ejemplo.com**, **dominios**, **ejemplo1.com**, **objetos de directiva de grupo**,  **Publicación de Hash para BranchCache**.  
  
5.  Haga clic con el botón secundario en el objeto de directiva de grupo **Publicación de hash para BranchCache** y haga clic en **Editar**. Se abre la consola del Editor de administración de directivas de grupo.  
  
6.  En la consola Editor de administración de directivas de grupo, expanda la ruta de acceso siguiente: **Configuración del equipo**, **Directivas**, **Plantillas administrativas**, **Red**, **Servidor Lanman**.  
  
7.  En la consola de Editor de administración de directivas de grupo, haga clic en **Servidor Lanman**. En el panel de detalles, haga doble clic en **Publicación de hash para BranchCache**. Se abre el cuadro de diálogo **Publicación de hash para BranchCache**.  
  
8.  En el cuadro de diálogo **Publicación de hash para BranchCache**, haga clic en **Habilitado**.  
  
9. En **opciones**, haga clic en **permitir la publicación de hash para todas las carpetas compartidas**y, a continuación, haga clic en uno de los siguientes:  
  
    1.  Para habilitar la publicación de hash para todas las carpetas compartidas para todos los servidores de archivos que ha agregado a la unidad organizativa, haga clic en **permitir la publicación de hash para todas las carpetas compartidas**.  
  
    2.  Para habilitar la publicación de hash solamente para las carpetas compartidas para las que se ha habilitado BranchCache, haga clic en **Permitir la publicación de hash solo para carpetas compartidas en las que BranchCache está habilitado**.  
  
    3.  Para no permitir la publicación de hash para todas las carpetas compartidas del equipo aunque se haya habilitado BranchCache en los recursos compartidos de archivos, haga clic en **No permitir la publicación de hash en ninguna carpeta compartida**.  
  
10. Haga clic en **Aceptar**.  
  
> [!NOTE]  
> En la mayoría de los casos, debe guardar la consola MMC y actualizar la vista para que se muestren los cambios de configuración realizados.  
  


