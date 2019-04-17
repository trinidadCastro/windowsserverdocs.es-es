---
title: Registrar un servidor NPS en un dominio de Active Directory
description: Puedes usar este tema para registrar un servidor que ejecuta el servidor de directivas de red en Windows Server 2016 en el dominio de forma predeterminada del servidor NPS o en otro dominio.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2de954fd-a7d8-4cc6-85b1-b0c3c06f788f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7ba18f6c994e8b15da3a07a3e37550d5dbff24af
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="register-an-nps-server-in-an-active-directory-domain"></a>Registrar un servidor NPS en un dominio de Active Directory

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para registrar un servidor que ejecuta el servidor de directivas de red en Windows Server 2016 en el dominio de forma predeterminada del servidor NPS o en otro dominio.

## <a name="register-an-nps-server-in-its-default-domain"></a>Registrar un servidor NPS en su dominio predeterminado

Puedes usar este procedimiento para registrar un servidor NPS en el dominio donde el servidor es un miembro de dominio. 

Servidores NPS deben registrarse en Active Directory para que tengan permiso para leer las propiedades de marcado de cuentas de usuario durante el proceso de autorización. Registrar un servidor NPS agrega el servidor para la **RAS y servidores IAS** grupo en Active Directory.

Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para realizar estos procedimientos.

### <a name="to-register-an-nps-server-in-its-default-domain"></a>Para registrar un servidor NPS en su dominio predeterminado


1. En el servidor NPS, en el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **el servidor de directivas de red**. Abre la consola del servidor de directivas de red.

2. Haz clic en **NPS (Local)**y, a continuación, haz clic en **registrar el servidor en Active Directory**. La **el servidor de directivas de red** abre el cuadro de diálogo.

3. En **el servidor de directivas de red**, haz clic en **Aceptar**y, a continuación, haz clic en **Aceptar** nuevamente.

## <a name="register-an-nps-server-in-another-domain"></a>Registrar un servidor NPS en otro dominio

Para proporcionar un servidor NPS con permiso para leer las propiedades de marcado de cuentas de usuario en Active Directory, el servidor NPS debe registrarse en el dominio donde residen las cuentas.

Puedes usar este procedimiento para registrar un servidor NPS en un dominio donde el servidor NPS no es un miembro de dominio.

Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para realizar estos procedimientos.

### <a name="to-register-an-nps-server-in-another-domain"></a>Para registrar un servidor NPS en otro dominio

1. En el controlador de dominio en el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **equipos y usuarios de Active Directory**. Abre la consola de equipos y usuarios de Active Directory.

2. En el árbol de consola, navega al dominio donde quieres que el servidor NPS para leer la información de cuenta de usuario y, a continuación, haz clic en el **usuarios** carpeta. 

3. En el panel de detalles, haz clic en **RAS y servidores IAS**y, a continuación, haz clic en **propiedades**. La **RAS y propiedades de los servidores de IAS** abre el cuadro de diálogo.

4. En la **RAS y propiedades de los servidores de IAS** cuadro de diálogo, haz clic en el **miembros** pestaña, agrega cada uno de los servidores NPS que quieras registrar en el dominio y, a continuación, haz clic en **Aceptar**.


### <a name="to-register-an-nps-server-in-another-domain-by-using-netsh-commands-for-nps"></a>Para registrar un servidor NPS en otro dominio mediante comandos Netsh para NPS

1. Abre windows PowerShell o el símbolo del sistema. 

2. Escribe lo siguiente en la ventana de símbolo del sistema: **netsh nps agregar registeredserver**&nbsp;*dominio*&nbsp;*server*, y, a continuación, presione ENTRAR.

>[!NOTE]
>En el comando anterior, *dominio* es el nombre de dominio DNS del dominio donde desee registrar el servidor NPS, y *server* es el nombre de equipo del servidor NPS.

