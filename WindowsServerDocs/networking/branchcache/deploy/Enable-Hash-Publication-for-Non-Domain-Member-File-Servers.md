---
title: Habilitar la publicación de Hash no del dominio miembro servidores de archivos
description: En este tema es parte de la BranchCache implementación Guía para Windows Server 2016, que se muestra cómo implementar BranchCache en modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN en sucursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 11584b73-f9e2-4530-afa5-b8df970e6b24
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4cb3d217115cff3a9b30ee11acb7ba0de6672b24
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="enable-hash-publication-for-non-domain-member-file-servers"></a>Habilitar la publicación de Hash no del dominio miembro servidores de archivos

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este procedimiento para configurar la publicación de hash para BranchCache mediante Directiva de grupo del equipo local en un servidor de archivos que se ejecuta Windows Server 2016 con la **BranchCache para archivos de red** servicio de rol de instalado el rol de servidor de servicios de archivo.  
  
Este procedimiento está pensado para usarse en un servidor de archivos no del dominio miembro. Si realizan este procedimiento en un servidor de archivos de miembro de dominio y configura también BranchCache mediante la directiva de grupo de dominio, configuración de directiva de grupo de dominio invalida la configuración de directiva de grupo local.  
  
Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para realizar este procedimiento.  
  
> [!NOTE]  
> Si tienes uno o varios servidores de archivos de miembro de dominio, puede agregarlos a una unidad organizativa (OU) en los servicios de dominio de Active Directory y, a continuación, usar la directiva de grupo para configurar la publicación de hash de todos los servidores de archivos al mismo tiempo, en lugar de configurar individualmente cada servidor de archivos. Para obtener más información, consulta [habilitar Hash de publicación para servidores de archivos de miembro de dominio](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md).  
  
### <a name="to-enable-hash-publication-for-one-file-server"></a>Para habilitar la publicación de hash de un servidor de archivos  
  
1.  Abre Windows PowerShell, escribe **mmc**, y, a continuación, presione ENTRAR. Abre Microsoft Management Console (MMC).  
  
2.  En la consola de MMC, en la **archivo** menú, haz clic en **agregar o quitar complemento**. La **agregar o quitar complementos** abre el cuadro de diálogo.  
  
3.  En **agregar o quitar complementos**, en **complementos disponibles**, haz doble clic en **el Editor de objetos de directiva de grupo**. Abre el Asistente para la directiva de grupo con el objeto de equipo Local seleccionado. Haz clic en **finalizar**y, a continuación, haz clic en **Aceptar**.  
  
4.  En el Editor de directivas de grupo Local de MMC, expande la ruta de acceso siguiente: **directiva de equipo Local**, **configuración del equipo**, **plantillas administrativas**, **red**, **Lanman Server**. Haz clic en **Lanman Server**.  
  
5.  En el panel de detalles, haz doble clic en **Hash de publicación para BranchCache**. La **Hash de publicación para BranchCache** abre el cuadro de diálogo.  
  
6.  En la **Hash de publicación para BranchCache** cuadro de diálogo, haz clic en **habilitado**.  
  
7.  En **opciones**, haz clic en **permitir la publicación de hash para todas las carpetas compartidas**y, a continuación, haz clic en uno de los siguientes:  
  
    1.  Para habilitar la publicación de hash para todas las carpetas compartidas en este equipo, haz clic en **permitir la publicación de hash para todas las carpetas compartidas**.  
  
    2.  Para habilitar la publicación de hash solo para carpetas compartidas que BranchCache está habilitado, haz clic en **permitir la publicación de hash solo para carpetas compartidas en el que está habilitada BranchCache**.  
  
    3.  Para impedir la publicación de hash para todas las carpetas compartirán en el equipo incluso si BranchCache está habilitado en recursos compartidos de archivos, haz clic en **no permitir la publicación de hash en todas las carpetas compartidas**.  
  
8.  Haz clic en **Aceptar**.  
  


