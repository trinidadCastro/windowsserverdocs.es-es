---
title: Crear la unidad organizativa para servidores de archivos de BranchCache
description: Obtenga información sobre cómo crear una unidad organizativa (OU) en Active Directory Domain Services (AD DS) para los servidores de archivos de BranchCache.
manager: brianlic
ms.topic: how-to
ms.assetid: 2cda192f-6b45-4e6c-88d9-70ca179ddb94
ms.author: lizross
author: eross-msft
ms.date: 01/05/2021
ms.openlocfilehash: ff436166eedb4a3214361267f1d508ebe3acc6e4
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97950301"
---
# <a name="create-the-branchcache-file-servers-organizational-unit"></a>Crear la unidad organizativa para servidores de archivos de BranchCache

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este procedimiento para crear una unidad organizativa (OU) en Servicios de dominio de Active Directory (AD DS) para servidores de archivos de BranchCache.

El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Admins. del dominio** o grupo equivalente.

### <a name="to-create-the-branchcache-file-servers-organizational-unit"></a>Para crear la unidad organizativa para servidores de archivos de BranchCache

1.  En un equipo en el que esté instalado AD DS, en Administrador del servidor, haga clic en **herramientas** y, a continuación, haga clic en **Active Directory usuarios y equipos**. Se abre la Consola de usuarios y equipos de Active Directory.

2.  En la Consola de usuarios y equipos de Active Directory, haga clic con el botón secundario en el dominio al que desea agregar una unidad organizativa. Por ejemplo, si el nombre del dominio es ejemplo.com, haga clic con el botón secundario en **ejemplo.com**. Seleccione **Nuevo** y haga clic en **Unidad organizativa**. Se abre el cuadro **de diálogo nuevo objeto: unidad organizativa** .

3.  En el cuadro de diálogo **nuevo objeto: unidad organizativa** , en **nombre**, escriba un nombre para la nueva unidad organizativa. Por ejemplo, si desea asignar a la unidad organizativa el nombre Servidores de archivos de BranchCache, escriba **Servidores de archivos de BranchCache** y, a continuación, haga clic en **Aceptar**.



