---
title: Mover servidores de archivos a la unidad organizativa para servidores de archivos de BranchCache
description: Este tema forma parte de la guía de implementación de BranchCache para Windows Server 2016, que muestra cómo implementar BranchCache en los modos de caché distribuida y hospedada para optimizar el uso del ancho de banda WAN en las sucursales.
manager: brianlic
ms.topic: get-started-article
ms.assetid: 56c915ec-edb1-43b0-8ad2-c93841bb566f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: c44fb11a2935c11474abd3b3cc39cfd0eeb34734
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87962349"
---
# <a name="move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>Mover servidores de archivos a la unidad organizativa para servidores de archivos de BranchCache

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este procedimiento para agregar servidores de archivos de BranchCache a una unidad organizativa (OU) en Servicios de dominio de Active Directory (AD DS).

El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Admins. del dominio** o grupo equivalente.

> [!NOTE]
> Debe crear una unidad organizativa para servidores de archivos de BranchCache en la Consola de usuarios y equipos de Active Directory antes de agregar cuentas de equipo a la unidad organizativa con este procedimiento. Para obtener más información, vea [crear la unidad organizativa de servidores de archivos de BranchCache](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md).

### <a name="to-move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>Para mover servidores de archivos a la unidad organizativa para servidores de archivos de BranchCache

1.  En un equipo en el que esté instalado AD DS, en Administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **Active Directory usuarios y equipos**. Se abre la Consola de usuarios y equipos de Active Directory.

2.  En la consola de usuarios y equipos de Active Directory, busque la cuenta de equipo para un servidor de archivos de BranchCache, haga clic con el botón primario para seleccionar la cuenta y, a continuación, arrastre y coloque la cuenta de equipo en la unidad organizativa para servidores de archivos de BranchCache que creó previamente. Por ejemplo, si creó anteriormente una unidad organizativa denominada **servidores de archivos de BranchCache**, arrastre y coloque la cuenta de equipo en la unidad organizativa **servidores de archivos de BranchCache** .

3.  Repita el paso anterior para cada servidor de archivos de BranchCache en el dominio que desea mover a la unidad organizativa.



