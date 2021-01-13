---
title: Registrar un NPS en un dominio de Active Directory
description: Obtenga información sobre cómo registrar un servidor que ejecuta servidor de directivas de redes en Windows Server 2016 en el dominio predeterminado de NPS o en otro dominio.
manager: brianlic
ms.topic: article
ms.assetid: 2de954fd-a7d8-4cc6-85b1-b0c3c06f788f
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 1cd7f5432cc1b141fc2b202170d2cc4ebd4b4f82
ms.sourcegitcommit: decb6c8caf4851b13af271d926c650d010a6b9e9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/13/2021
ms.locfileid: "98177484"
---
# <a name="register-an-nps-in-an-active-directory-domain"></a>Registrar un NPS en un dominio de Active Directory

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para registrar un servidor que ejecuta servidor de directivas de redes en Windows Server 2016 en el dominio predeterminado de NPS o en otro dominio.

## <a name="register-an-nps-in-its-default-domain"></a>Registrar un NPS en su dominio predeterminado

Puede usar este procedimiento para registrar un NPS en el dominio en el que el servidor es miembro del dominio.

NPSs debe estar registrado en Active Directory para que tengan permiso para leer las propiedades de acceso telefónico de las cuentas de usuario durante el proceso de autorización. El registro de un NPS agrega el servidor al grupo de **servidores RAS e IAS** en Active Directory.

El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.

### <a name="to-register-an-nps-in-its-default-domain"></a>Para registrar un NPS en su dominio predeterminado


1. En el NPS, en Administrador del servidor, haga clic en **herramientas** y, a continuación, haga clic en **servidor de directivas de redes**. Se abre la consola del servidor de directivas de redes.

2. Haga clic con el botón secundario en **NPS (local)** y, a continuación, haga clic en **registrar servidor en Active Directory**. Se abrirá el cuadro de diálogo **Servidor de directivas de redes**.

3. En **Servidor de directivas de redes**, haga clic en **Aceptar** y, a continuación, en **Aceptar** de nuevo.

## <a name="register-an-nps-in-another-domain"></a>Registrar un NPS en otro dominio

Para proporcionar a un NPS permiso para leer las propiedades de acceso telefónico de las cuentas de usuario en Active Directory, el NPS debe estar registrado en el dominio donde residen las cuentas.

Puede usar este procedimiento para registrar un NPS en un dominio en el que el NPS no sea miembro de dominio.

El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.

### <a name="to-register-an-nps-in-another-domain"></a>Para registrar un NPS en otro dominio

1. En el controlador de dominio, en Administrador del servidor, haga clic en **herramientas** y, a continuación, haga clic en **Active Directory usuarios y equipos**. Se abre la Consola de usuarios y equipos de Active Directory.

2. En el árbol de consola, navegue hasta el dominio en el que desea que el NPS Lea la información de la cuenta de usuario y, a continuación, haga clic en la carpeta **usuarios** .

3. En el panel de detalles, haga clic con el botón secundario en **servidores RAS e IAS** y, a continuación, haga clic en **propiedades**. Se abrirá el cuadro de diálogo **propiedades de servidor RAS e IAS** .

4. En el cuadro de diálogo **propiedades de servidores RAS e IAS** , haga clic en la pestaña **miembros** , agregue cada uno de los NPSs que desee registrar en el dominio y, a continuación, haga clic en **Aceptar**.


### <a name="to-register-an-nps-in-another-domain-by-using-netsh-commands-for-nps"></a>Para registrar un NPS en otro dominio mediante los comandos Netsh para NPS

1. Abra el símbolo del sistema o Windows PowerShell.

2. Escriba lo siguiente en el símbolo del sistema: **netsh NPS Add registeredserver** &nbsp; *Domain* &nbsp; *Server* y, a continuación, presione Entrar.

>[!NOTE]
>En el comando anterior, *Domain* es el nombre de dominio DNS del dominio en el que desea registrar el NPS y *Server* es el nombre del equipo NPS.

