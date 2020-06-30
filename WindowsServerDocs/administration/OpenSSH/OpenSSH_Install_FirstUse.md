---
title: Instalación de OpenSSH para Windows Server
description: Instalación del cliente y el servidor de OpenSSH para Windows Server con las opciones de configuración de Windows o Windows PowerShell.
ms.date: 09/27/2019
ms.topic: conceptual
contributor: maertendMSFT
author: maertendmsft
ms.openlocfilehash: 8ed486adb9519fbc0f87cc3142386bec8211d783
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85469790"
---
# <a name="installation-of-openssh-for-windows-server-2019-and-windows-10"></a>Instalación de OpenSSH para Windows Server 2019 y Windows 10

El cliente y el servidor de OpenSSH son componentes instalables por separado de Windows Server 2019 y Windows 10 1809.
Los usuarios con estas versiones de Windows deben seguir las instrucciones que se indican a continuación para instalar y configurar OpenSSH.

> [!NOTE]
> Los usuarios que han adquirido OpenSSH desde el repositorio de GitHub de PowerShell (https://github.com/PowerShell/OpenSSH-Portable) deben usar aquellas instrucciones y __no__ estas.

## <a name="installing-openssh-from-the-settings-ui-on-windows-server-2019-or-windows-10-1809"></a>Instalación de OpenSSH desde la interfaz de usuario de configuración de Windows Server 2019 o Windows 10 1809

El cliente y el servidor de OpenSSH son características instalables de Windows 10 1809.

Para instalar OpenSSH, inicia Configuración y ve a Aplicaciones > Aplicaciones y características > Administrar características opcionales.

Examina esta lista para ver si el cliente de OpenSSH ya está instalado. Si no es así, en la parte superior de la página, selecciona "Agregar una característica" y, a continuación:

* Para instalar el cliente de OpenSSH, busca "Cliente de OpenSSH" y, a continuación, haz clic en "Instalar".
* Para instalar el servidor de OpenSSH, busca "Servidor de OpenSSH" y, a continuación, haz clic en "Instalar".

Una vez finalizada la instalación, vuelve a Aplicaciones > Aplicaciones y características > Administrar características opcionales y deberías ver los componentes de OpenSSH en la lista.

> [!NOTE]
> La instalación del servidor de OpenSSH creará y habilitará una regla de firewall denominada "OpenSSH-Server-In-TCP". Esto permite el tráfico SSH entrante en el puerto 22.

## <a name="installing-openssh-with-powershell"></a>Instalación de OpenSSH con PowerShell

Para instalar OpenSSH con PowerShell, primero inicia PowerShell como administrador.
Para asegurarte de que las características de OpenSSH están disponibles para la instalación:

```powershell
Get-WindowsCapability -Online | ? Name -like 'OpenSSH*'

# This should return the following output:

Name  : OpenSSH.Client~~~~0.0.1.0
State : NotPresent
Name  : OpenSSH.Server~~~~0.0.1.0
State : NotPresent
```

A continuación, instala las características de cliente o servidor:

```powershell
# Install the OpenSSH Client
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

# Install the OpenSSH Server
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0

# Both of these should return the following output:

Path          :
Online        : True
RestartNeeded : False
```

## <a name="uninstalling-openssh"></a>Desinstalación de OpenSSH

Para desinstalar OpenSSH desde la Configuración de Windows, inicia Configuración y ve a Aplicaciones > Aplicaciones y características > Administrar características opcionales.
En la lista de características instaladas, selecciona el componente Cliente de OpenSSH o Servidor de OpenSSH y, a continuación, selecciona Desinstalar.

Para desinstalar OpenSSH con PowerShell, usa uno de los siguientes comandos:

```powershell
# Uninstall the OpenSSH Client
Remove-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

# Uninstall the OpenSSH Server
Remove-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

Es posible que sea necesario reiniciar Windows después de quitar OpenSSH si el servicio estaba en uso en el momento en que se desinstaló.


## <a name="initial-configuration-of-ssh-server"></a>Configuración inicial del servidor de SSH

Para configurar el servidor de OpenSSH para su uso inicial en Windows, inicia PowerShell como administrador y, luego, ejecuta los siguientes comandos para iniciar el servicio SSHD:

```powershell
Start-Service sshd
# OPTIONAL but recommended:
Set-Service -Name sshd -StartupType 'Automatic'
# Confirm the Firewall rule is configured. It should be created automatically by setup.
Get-NetFirewallRule -Name *ssh*
# There should be a firewall rule named "OpenSSH-Server-In-TCP", which should be enabled
# If the firewall does not exist, create one
New-NetFirewallRule -Name sshd -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
```

## <a name="initial-use-of-ssh"></a>Uso inicial de SSH

Una vez que hayas instalado el servidor de OpenSSH en Windows, puedes probarlo rápidamente con PowerShell desde cualquier dispositivo Windows con el cliente SSH instalado.
En PowerShell, escribe el siguiente comando:

```powershell
Ssh username@servername
```

La primera conexión a cualquier servidor generará un mensaje similar al siguiente:

```
The authenticity of host 'servername (10.00.00.001)' can't be established.
ECDSA key fingerprint is SHA256:(<a large string>).
Are you sure you want to continue connecting (yes/no)?
```

La respuesta debe ser "sí" o "no".
Si respondes Sí, se agregará ese servidor a la lista de hosts de SSH conocidos del sistema local.

En este momento se te pedirá la contraseña. Como medida de seguridad, la contraseña no se mostrará a medida que escribes.

Una vez que te conectes, verás un símbolo del sistema de comandos similar al siguiente:

```
domain\username@SERVERNAME C:\Users\username>
```

El shell predeterminado que usa el servidor de OpenSSH de Windows es el shell de comandos de Windows.

