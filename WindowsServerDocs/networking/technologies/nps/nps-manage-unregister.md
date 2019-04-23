---
title: Anular el registro de NPS de un dominio de Active Directory
description: Puede utilizar este tema para registrar un servidor que ejecuta el servidor de directivas de red en Windows Server 2016 en el dominio predeterminado NPS o en otro dominio.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 68a94616-3c29-45bd-bd33-e4c578f119e1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8fe4773efd89aeb413b3793f874ad6a1b030294a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864356"
---
# <a name="unregister-an-nps-from-an-active-directory-domain"></a>Anular el registro de NPS de un dominio de Active Directory

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En el proceso de administración de la implementación de NPS, le resultará útil para mover un NPS a otro dominio, para reemplazar un NPS o retirar un NPS. 

Al mover o retirar un NPS, puede anular el registro NPS en los dominios de Active Directory donde el NPS tiene permiso para leer las propiedades de las cuentas de usuario en Active Directory.

El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.

## <a name="to-unregister-an-nps"></a>Para anular el registro NPS

1. En el controlador de dominio en el administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **equipos y usuarios de Active Directory**. Se abre la Consola de usuarios y equipos de Active Directory.

2. Haga clic en **usuarios**y, a continuación, haga doble clic en **servidores RAS e IAS**.

3. Haga clic en el **miembros** pestaña y, a continuación, seleccione el NPS que desea anular el registro.

4. Haga clic en **quitar**, haga clic en **Sí**y, a continuación, haga clic en **Aceptar**.

