---
title: Configurar el objeto de directiva de grupo de publicación de Hash de BranchCache
description: En este tema es parte de la BranchCache implementación Guía para Windows Server 2016, que se muestra cómo implementar BranchCache en modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN en sucursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: da74fea7-52b2-4d6d-9d21-19184eedbe3c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6f528c9f0a8a39b286ad7ac4fa728d93c311f779
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="configure-the-branchcache-hash-publication-group-policy-object"></a>Configurar el objeto de directiva de grupo de publicación de Hash de BranchCache

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este procedimiento para configurar la publicación de hash BranchCache objeto de directiva de grupo (GPO) para que todos los servidores de archivos que hayas agregado a la unidad organizativa tengan la misma configuración de directiva de publicación de hash que se les aplican.  
  
Pertenencia a **administradores de dominio**, o equivalente, es lo mínimo necesario para realizar este procedimiento.  
  
> [!NOTE]  
> Antes de realizar este procedimiento, debes crear la unidad organizativa de la servidores de archivo BranchCache, mueve los servidores de archivos en la unidad organizativa y crear la publicación de hash BranchCache GPO. Para obtener más información, consulta [habilitar Hash de publicación para servidores de archivos de miembro de dominio](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md).  
  
### <a name="to-configure-the-branchcache-hash-publication-group-policy-object"></a>Para configurar la publicación de hash BranchCache objeto de directiva de grupo  
  
1.  Ejecutar Windows PowerShell como administrador, tipo **mmc**, y, a continuación, presione ENTRAR. Abre Microsoft Management Console (MMC).  
  
2.  En la consola de MMC, en la **archivo** menú, haz clic en **agregar o quitar complemento**. La **agregar o quitar complementos** abre el cuadro de diálogo.  
  
3.  En **agregar o quitar complementos**, en **complementos disponibles**, haz doble clic en **Group Policy Management**y, a continuación, haz clic en **Aceptar**.  
  
4.  En MMC de administración de directiva de grupo, expanda la ruta de acceso a la publicación de hash BranchCache GPO que creaste anteriormente. Por ejemplo, si el bosque se denomina example.com, el dominio se denomina example1.com y con el nombre del GPO **BranchCache Hash de publicación**, expande la ruta de acceso siguiente: **Group Policy Management**, **bosque: example.com**, **dominios**, **example1.com**, **objetos de directiva de grupo**, **BranchCache Hash de publicación**.  
  
5.  Haz clic en el **BranchCache Hash de publicación** GPO y haz clic en **editar**. Abre la consola del Editor de administración de directivas de grupo.  
  
6.  En la consola del Editor de administración de directivas de grupo, expanda la siguiente ruta de acceso: **configuración del equipo**, **directivas**, **plantillas administrativas**, **red**, **Lanman Server**.  
  
7.  En la consola del Editor de administración de directivas de grupo, haz clic en **Lanman Server**. En el panel de detalles, haz doble clic en **Hash de publicación para BranchCache**. La **Hash de publicación para BranchCache** abre el cuadro de diálogo.  
  
8.  En la **Hash de publicación para BranchCache** cuadro de diálogo, haz clic en **habilitado**.  
  
9. En **opciones**, haz clic en **permitir la publicación de hash para todas las carpetas compartidas**y, a continuación, haz clic en uno de los siguientes:  
  
    1.  Para habilitar la publicación de hash para todas las carpetas compartidas para todos los servidores que agregaste a la unidad organizativa de archivos, haz clic en **permitir la publicación de hash para todas las carpetas compartidas**.  
  
    2.  Para habilitar la publicación de hash solo para carpetas compartidas que BranchCache está habilitado, haz clic en **permitir la publicación de hash solo para carpetas compartidas en el que está habilitada BranchCache**.  
  
    3.  Para impedir la publicación de hash para todas las carpetas compartirán en el equipo incluso si BranchCache está habilitado en recursos compartidos de archivos, haz clic en **no permitir la publicación de hash en todas las carpetas compartidas**.  
  
10. Haz clic en **Aceptar**.  
  
> [!NOTE]  
> En la mayoría de los casos, debes guardar la consola de MMC y actualizar la vista para mostrar los cambios de configuración realizados.  
  


