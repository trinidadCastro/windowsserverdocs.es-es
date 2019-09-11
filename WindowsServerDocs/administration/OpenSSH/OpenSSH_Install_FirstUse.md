---
ms.date: 01/07/2019
ms.topic: conceptual
keywords: OpenSSH, SSH, SSHD, instalación y configuración
contributor: maertendMSFT
author: maertendMSFT
title: Instalación de OpenSSH para Windows
ms.openlocfilehash: 6a5d4d47fbb3f962c2a19582eb0a72810145a28c
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866876"
---
# <a name="installation-of-openssh-for-windows-server-2019-and-windows-10"></a>Instalación de OpenSSH para Windows Server 2019 y Windows 10 #

El cliente OpenSSH y el servidor OpenSSH son componentes instalables por separado en Windows Server 2019 y Windows 10 1809.
Los usuarios con estas versiones de Windows deben seguir las instrucciones que se indican a continuación para instalar y configurar OpenSSH. 

> [!NOTE] 
> Los usuarios que han adquirido OpenSSH desde el repositorio de https://github.com/PowerShell/OpenSSH-Portable) GitHub de PowerShell (deben usar las instrucciones de allí y __no deben__ usar estas instrucciones. 


## <a name="installing-openssh-from-the-settings-ui-on-windows-server-2019-or-windows-10-1809"></a>Instalación de OpenSSH desde la interfaz de usuario de configuración en Windows Server 2019 o Windows 10 1809

El cliente y el servidor OpenSSH son características instalables de Windows 10 1809. 

Para instalar OpenSSH, inicie configuración y vaya a aplicaciones > Aplicaciones y características > administrar características opcionales. 

Examine esta lista para ver si el cliente OpenSSH ya está instalado. Si no es así, en la parte superior de la página, seleccione "agregar una característica" y, a continuación: 

* Para instalar el cliente OpenSSH, busque "cliente OpenSSH" y, a continuación, haga clic en "instalar". 
* Para instalar el servidor OpenSSH, busque "OpenSSH Server" y, a continuación, haga clic en "instalar". 

Una vez finalizada la instalación, vuelva a aplicaciones > Aplicaciones y características > administrar características opcionales y debería ver los componentes OpenSSHs que aparecen en la lista.

> [!NOTE]
> La instalación del servidor OpenSSH creará y habilitará una regla de Firewall denominada "OpenSSH-Server-in-TCP". Esto permite el tráfico SSH entrante en el puerto 22. 

## <a name="installing-openssh-with-powershell"></a>Instalación de OpenSSH con PowerShell 

Para instalar OpenSSH con PowerShell, primero inicie PowerShell como administrador.
Para asegurarse de que las características OpenSSH están disponibles para la instalación:

```powershell
Get-WindowsCapability -Online | ? Name -like 'OpenSSH*'

# This should return the following output:

Name  : OpenSSH.Client~~~~0.0.1.0
State : NotPresent
Name  : OpenSSH.Server~~~~0.0.1.0
State : NotPresent
```

A continuación, instale las características de cliente y servidor:

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

Para desinstalar OpenSSH mediante la configuración de Windows, inicie configuración y vaya a aplicaciones > Aplicaciones y características > administrar características opcionales. En la lista de características instaladas, seleccione el cliente OpenSSH o el componente servidor OpenSSH y, a continuación, seleccione Desinstalar.

Para desinstalar OpenSSH con PowerShell, use uno de los siguientes comandos:

```powershell
# Uninstall the OpenSSH Client
Remove-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

# Uninstall the OpenSSH Server
Remove-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

Es posible que sea necesario reiniciar Windows después de quitar OpenSSH, si el servicio está en uso en el momento en que se desinstaló.


## <a name="initial-configuration-of-ssh-server"></a>Configuración inicial del servidor SSH

Para configurar el servidor OpenSSH para su uso inicial en Windows, inicie PowerShell como administrador y luego ejecute los siguientes comandos para iniciar el servicio SSHD:

```powershell
Start-Service sshd
# OPTIONAL but recommended:
Set-Service -Name sshd -StartupType 'Automatic'
# Confirm the Firewall rule is configured. It should be created automatically by setup. 
Get-NetFirewallRule -Name *ssh*
# There should be a firewall rule named "OpenSSH-Server-In-TCP", which should be enabled 
```

## <a name="initial-use-of-ssh"></a>Uso inicial de SSH

Una vez que haya instalado el servidor OpenSSH en Windows, puede probarlo rápidamente con PowerShell desde cualquier dispositivo Windows con el cliente SSH instalado. En PowerShell, escriba el siguiente comando: 

```powershell
Ssh username@servername
```

La primera conexión a cualquier servidor producirá un mensaje similar al siguiente:

```
The authenticity of host 'servername (10.00.00.001)' can't be established.
ECDSA key fingerprint is SHA256:(<a large string>).
Are you sure you want to continue connecting (yes/no)?
```

La respuesta debe ser "sí" o "no". Si responde sí, se agregará ese servidor a la lista de hosts de ssh conocidos del sistema local.

En este momento se le pedirá la contraseña. Como medida de seguridad, la contraseña no se mostrará a medida que escribe. 

Una vez que se conecte, verá un símbolo del sistema de comandos similar al siguiente:

```
domain\username@SERVERNAME C:\Users\username>
```

El shell predeterminado que usa Windows OpenSSH Server es el shell de comandos de Windows. 

