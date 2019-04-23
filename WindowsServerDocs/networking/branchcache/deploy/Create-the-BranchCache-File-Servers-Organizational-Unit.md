---
title: Crear la unidad organizativa para servidores de archivos de BranchCache
description: En este tema forma parte de BranchCache Deployment Guide para Windows Server 2016, que demuestra cómo implementar BranchCache en los modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN de sucursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 2cda192f-6b45-4e6c-88d9-70ca179ddb94
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b7b26ec5808f5b11141e81dc5e738c83c94ef6b8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874086"
---
# <a name="create-the-branchcache-file-servers-organizational-unit"></a>Crear la unidad organizativa para servidores de archivos de BranchCache

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este procedimiento para crear una unidad organizativa (OU) en Servicios de dominio de Active Directory (AD DS) para servidores de archivos de BranchCache.  
  
El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Admins. del dominio** o grupo equivalente.  
  
### <a name="to-create-the-branchcache-file-servers-organizational-unit"></a>Para crear la unidad organizativa para servidores de archivos de BranchCache  
  
1.  En un equipo donde se instala AD DS, en el administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **equipos y usuarios de Active Directory**. Se abre la Consola de usuarios y equipos de Active Directory.  
  
2.  En la Consola de usuarios y equipos de Active Directory, haga clic con el botón secundario en el dominio al que desea agregar una unidad organizativa. Por ejemplo, si el nombre del dominio es ejemplo.com, haga clic con el botón secundario en **ejemplo.com**. Seleccione **Nuevo** y haga clic en **Unidad organizativa**. El **objeto nuevo: unidad organizativa** abre el cuadro de diálogo.  
  
3.  En el **objeto nuevo: unidad organizativa** cuadro de diálogo **nombre**, escriba un nombre para la nueva unidad organizativa. Por ejemplo, si desea asignar a la unidad organizativa el nombre Servidores de archivos de BranchCache, escriba **Servidores de archivos de BranchCache** y, a continuación, haga clic en **Aceptar**.  
  


