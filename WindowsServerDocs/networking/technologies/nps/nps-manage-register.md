---
title: Registrar un NPS en un dominio de Active Directory
description: Puede utilizar este tema para registrar un servidor que ejecuta el servidor de directivas de red en Windows Server 2016 en el dominio predeterminado NPS o en otro dominio.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2de954fd-a7d8-4cc6-85b1-b0c3c06f788f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4a289ec519e5107576becf2905cd881cf9def190
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877656"
---
# <a name="register-an-nps-in-an-active-directory-domain"></a>Registrar un NPS en un dominio de Active Directory

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para registrar un servidor que ejecuta el servidor de directivas de red en Windows Server 2016 en el dominio predeterminado NPS o en otro dominio.

## <a name="register-an-nps-in-its-default-domain"></a>Registrar un NPS en el dominio predeterminado

Puede usar este procedimiento para registrar un NPS en el dominio donde el servidor es miembro del dominio. 

NPSs deben estar registrados en Active Directory para que tengan permiso para leer las propiedades de marcado de cuentas de usuario durante el proceso de autorización. Registrar un NPS agrega el servidor a la **servidores RAS e IAS** grupo en Active Directory.

El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.

### <a name="to-register-an-nps-in-its-default-domain"></a>Para registrar un NPS en el dominio predeterminado


1. En el NPS, en el administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **servidor de directivas de red**. Se abre la consola de servidor de directivas de red.

2. Haga clic en **NPS (Local)** y, a continuación, haga clic en **Registrar servidor en Active Directory**. Se abrirá el cuadro de diálogo **Servidor de directivas de redes**.

3. En **Servidor de directivas de redes**, haga clic en **Aceptar** y, a continuación, en **Aceptar** de nuevo.

## <a name="register-an-nps-in-another-domain"></a>Registrar un NPS en otro dominio

Para proporcionar un NPS con permiso para leer las propiedades de marcado de cuentas de usuario en Active Directory, NPS debe registrarse en el dominio donde residen las cuentas.

Puede usar este procedimiento para registrar un NPS en un dominio donde el NPS no es miembro del dominio.

El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.

### <a name="to-register-an-nps-in-another-domain"></a>Para registrar un NPS en otro dominio

1. En el controlador de dominio en el administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **equipos y usuarios de Active Directory**. Se abre la Consola de usuarios y equipos de Active Directory.

2. En el árbol de consola, desplácese hasta el dominio donde desea que el NPS para leer información de la cuenta de usuario y, a continuación, haga clic en el **usuarios** carpeta. 

3. En el panel de detalles, haga clic en **servidores RAS e IAS**y, a continuación, haga clic en **propiedades**. El **RAS e IAS servidores propiedades** abre el cuadro de diálogo.

4. En el **RAS e IAS servidores propiedades** cuadro de diálogo, haga clic en el **miembros** pestaña, cada uno de los NPSs que desee registrar en el dominio y, a continuación, haga clic en agregar **Aceptar**.


### <a name="to-register-an-nps-in-another-domain-by-using-netsh-commands-for-nps"></a>Para registrar un NPS en otro dominio mediante los comandos Netsh para NPS

1. Abra el símbolo del sistema o windows PowerShell. 

2. Escriba lo siguiente en el símbolo del sistema: **netsh nps agregar registeredserver** &nbsp; *dominio* &nbsp; *server*, y, a continuación, presione ENTRAR.

>[!NOTE]
>En el comando anterior, *dominio* es el nombre de dominio DNS del dominio donde desea registrar el NPS, y *server* es el nombre del equipo NPS.

