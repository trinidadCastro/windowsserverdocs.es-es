---
title: Habilitar la publicación de hash para servidores de archivos que no son miembros del dominio
description: En este tema forma parte de BranchCache Deployment Guide para Windows Server 2016, que demuestra cómo implementar BranchCache en los modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN de sucursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 11584b73-f9e2-4530-afa5-b8df970e6b24
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 00be97abbd583e4c5e762775ea563ba0720d5142
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814956"
---
# <a name="enable-hash-publication-for-non-domain-member-file-servers"></a>Habilitar la publicación de hash para servidores de archivos que no son miembros del dominio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para configurar la publicación de hash para BranchCache mediante Directiva de grupo del equipo local en un servidor de archivos que se está ejecutando Windows Server 2016 con la **BranchCache para archivos de red** servicio de rol de los servicios de archivos rol de servidor instalado.  
  
Este procedimiento está previsto para su uso en un servidor de archivos que no sea miembro del dominio. Si realiza este procedimiento en un servidor de archivos que es miembro del dominio y también configura BranchCache utilizando la directiva de grupo de dominio, la configuración de esta directiva invalida la configuración de la directiva de grupo local.  
  
El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o un grupo equivalente.  
  
> [!NOTE]  
> Si tiene uno o más servidores de archivos que son miembros del dominio, puede agregarlos a una unidad organizativa (OU) en Servicios de dominio de Active Directory y utilizar después la directiva de grupo para configurar de una vez la publicación de hash para todos los servidores de archivos, en lugar de configurar individualmente cada servidor de archivos. Para obtener más información, consulte [habilitar la publicación de Hash para servidores de archivos del miembro de dominio](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md).  
  
### <a name="to-enable-hash-publication-for-one-file-server"></a>Para habilitar la publicación de hash para un servidor de archivos  
  
1.  Abra Windows PowerShell, escriba **mmc**y presione ENTRAR. Se abrirá Microsoft Management Console (MMC).  
  
2.  En MMC, en el menú **Archivo**, haga clic en **Agregar o quitar complemento**. Se abre el cuadro de diálogo **Agregar o quitar complementos**.  
  
3.  En **Agregar o quitar complementos**, en **Complementos disponibles**, haga doble clic en **Editor de objetos de directiva de grupo**. Se abre el Asistente para directivas de grupo con el objeto Equipo local seleccionado. Haga clic en **Finalizar**y, a continuación, en **Aceptar**.  
  
4.  En MMC, en el Editor de directivas de grupo local, expanda la siguiente ruta de acceso: **Directiva de equipo local**, **configuración del equipo**, **plantillas administrativas**, **red**, **servidor Lanman**. Haga clic en **Servidor Lanman**.  
  
5.  En el panel de detalles, haga doble clic en **Publicación de hash para BranchCache**. Se abre el cuadro de diálogo **Publicación de hash para BranchCache**.  
  
6.  En el cuadro de diálogo **Publicación de hash para BranchCache**, haga clic en **Habilitado**.  
  
7.  En **opciones**, haga clic en **permitir la publicación de hash para todas las carpetas compartidas**y, a continuación, haga clic en uno de los siguientes:  
  
    1.  Para habilitar la publicación de hash para todas las carpetas compartidas en este equipo, haga clic en **permitir la publicación de hash para todas las carpetas compartidas**.  
  
    2.  Para habilitar la publicación de hash solamente para las carpetas compartidas para las que se ha habilitado BranchCache, haga clic en **Permitir la publicación de hash solo para carpetas compartidas en las que BranchCache está habilitado**.  
  
    3.  Para no permitir la publicación de hash para todas las carpetas compartidas del equipo aunque se haya habilitado BranchCache en los recursos compartidos de archivos, haga clic en **No permitir la publicación de hash en ninguna carpeta compartida**.  
  
8.  Haga clic en **Aceptar**.  
  


