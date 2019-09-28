---
title: Anular el registro de NPS de un dominio de Active Directory
description: Puede usar este tema para registrar un servidor que ejecuta servidor de directivas de redes en Windows Server 2016 en el dominio predeterminado de NPS o en otro dominio.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 68a94616-3c29-45bd-bd33-e4c578f119e1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3225c42ab14e2ea1bc283f520b14c09ebc2254c4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396082"
---
# <a name="unregister-an-nps-from-an-active-directory-domain"></a>Anular el registro de NPS de un dominio de Active Directory

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En el proceso de administración de la implementación de NPS, puede que le resulte útil trasladar un NPS a otro dominio, reemplazar un NPS o retirar un NPS. 

Al trasladar o retirar un NPS, puede anular el registro del NPS en los dominios de Active Directory en los que el NPS tiene permiso para leer las propiedades de las cuentas de usuario en Active Directory.

El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.

## <a name="to-unregister-an-nps"></a>Para anular el registro de un NPS

1. En el controlador de dominio, en Administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **Active Directory usuarios y equipos**. Se abre la Consola de usuarios y equipos de Active Directory.

2. Haga clic en **usuarios**y, a continuación, haga doble clic en **servidores RAS e IAS**.

3. Haga clic en la pestaña **miembros** y, a continuación, seleccione el NPS que desea eliminar del registro.

4. Haga clic en **quitar**, haga clic en **sí**y, a continuación, haga clic en **Aceptar**.

