---
title: Anular el registro de un servidor NPS desde un dominio de Active Directory
description: Puedes usar este tema para registrar un servidor que ejecuta el servidor de directivas de red en Windows Server 2016 en el dominio de forma predeterminada del servidor NPS o en otro dominio.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 68a94616-3c29-45bd-bd33-e4c578f119e1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 55c3b00146706831351ce63d1e5b74f45d7b9be1
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="unregister-an-nps-server-from-an-active-directory-domain"></a>Anular el registro de un servidor NPS desde un dominio de Active Directory

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

En el proceso de administrar la implementación del servidor NPS, puede resultarle útil para mover un servidor NPS a otro dominio, reemplazar un servidor NPS o retirar un servidor NPS. 

Cuando se mueve o de retirada de un servidor NPS, puedes anular el registro del servidor NPS en los dominios de Active Directory donde el servidor NPS tiene permiso para leer las propiedades de cuentas de usuario en Active Directory.

Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para realizar estos procedimientos.

## <a name="to-unregister-an-nps-server"></a>Para anular el registro de un servidor NPS

1. En el controlador de dominio en el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **equipos y usuarios de Active Directory**. Abre la consola de equipos y usuarios de Active Directory.

2. Haz clic en **usuarios**y, a continuación, haz doble clic en **servidores RAS e IAS**.

3. Haz clic en el **miembros** pestaña y, a continuación, selecciona el servidor NPS que desea anular el registro.

4. Haz clic en **quitar**, haz clic en **Sí**y, a continuación, haz clic en **Aceptar**.

