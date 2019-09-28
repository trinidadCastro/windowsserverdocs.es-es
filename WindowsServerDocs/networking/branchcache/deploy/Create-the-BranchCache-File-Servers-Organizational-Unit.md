---
title: Crear la unidad organizativa para servidores de archivos de BranchCache
description: Este tema forma parte de la guía de implementación de BranchCache para Windows Server 2016, que muestra cómo implementar BranchCache en los modos de caché distribuida y hospedada para optimizar el uso del ancho de banda WAN en las sucursales.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 2cda192f-6b45-4e6c-88d9-70ca179ddb94
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f058dbec42997f22106666b014508191a8861694
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406483"
---
# <a name="create-the-branchcache-file-servers-organizational-unit"></a>Crear la unidad organizativa para servidores de archivos de BranchCache

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este procedimiento para crear una unidad organizativa (OU) en Servicios de dominio de Active Directory (AD DS) para servidores de archivos de BranchCache.  
  
El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Admins. del dominio** o grupo equivalente.  
  
### <a name="to-create-the-branchcache-file-servers-organizational-unit"></a>Para crear la unidad organizativa para servidores de archivos de BranchCache  
  
1.  En un equipo en el que esté instalado AD DS, en Administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **Active Directory usuarios y equipos**. Se abre la Consola de usuarios y equipos de Active Directory.  
  
2.  En la Consola de usuarios y equipos de Active Directory, haga clic con el botón secundario en el dominio al que desea agregar una unidad organizativa. Por ejemplo, si el nombre del dominio es ejemplo.com, haga clic con el botón secundario en **ejemplo.com**. Seleccione **Nuevo** y haga clic en **Unidad organizativa**. Se abre el cuadro **de diálogo nuevo objeto: unidad organizativa** .  
  
3.  En el cuadro de diálogo **nuevo objeto: unidad organizativa** , en **nombre**, escriba un nombre para la nueva unidad organizativa. Por ejemplo, si desea asignar a la unidad organizativa el nombre Servidores de archivos de BranchCache, escriba **Servidores de archivos de BranchCache** y, a continuación, haga clic en **Aceptar**.  
  


