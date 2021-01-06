---
title: Mover servidores de archivos a la unidad organizativa para servidores de archivos de BranchCache
description: Obtenga información acerca de cómo agregar servidores de archivos de BranchCache a una unidad organizativa (OU) en Active Directory Domain Services (AD DS).
manager: brianlic
ms.topic: get-started-article
ms.assetid: 56c915ec-edb1-43b0-8ad2-c93841bb566f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 82a9d9b1d4c65e7058a19b6496ccea348a1117b1
ms.sourcegitcommit: 029b1e19ce11160d5f988046e04a83e8ab5a60dc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/05/2021
ms.locfileid: "97904550"
---
# <a name="move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>Mover servidores de archivos a la unidad organizativa para servidores de archivos de BranchCache

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este procedimiento para agregar servidores de archivos de BranchCache a una unidad organizativa (OU) en Servicios de dominio de Active Directory (AD DS).

El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Admins. del dominio** o grupo equivalente.

> [!NOTE]
> Debe crear una unidad organizativa para servidores de archivos de BranchCache en la Consola de usuarios y equipos de Active Directory antes de agregar cuentas de equipo a la unidad organizativa con este procedimiento. Para obtener más información, vea [crear la unidad organizativa de servidores de archivos de BranchCache](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md).

### <a name="to-move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>Para mover servidores de archivos a la unidad organizativa para servidores de archivos de BranchCache

1.  En un equipo en el que esté instalado AD DS, en Administrador del servidor, haga clic en **herramientas** y, a continuación, haga clic en **Active Directory usuarios y equipos**. Se abre la Consola de usuarios y equipos de Active Directory.

2.  En la consola de usuarios y equipos de Active Directory, busque la cuenta de equipo para un servidor de archivos de BranchCache, haga clic con el botón primario para seleccionar la cuenta y, a continuación, arrastre y coloque la cuenta de equipo en la unidad organizativa para servidores de archivos de BranchCache que creó previamente. Por ejemplo, si creó anteriormente una unidad organizativa denominada **servidores de archivos de BranchCache**, arrastre y coloque la cuenta de equipo en la unidad organizativa **servidores de archivos de BranchCache** .

3.  Repita el paso anterior para cada servidor de archivos de BranchCache en el dominio que desea mover a la unidad organizativa.



