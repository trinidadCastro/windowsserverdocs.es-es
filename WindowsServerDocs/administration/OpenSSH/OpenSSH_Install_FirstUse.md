---
ms.date: 01/07/2019
ms.topic: conceptual
keywords: OpenSSH, SSH, SSHD, instalar, configurar
contributor: maertendMSFT
author: maertendMSFT
title: Instalación de OpenSSH para Windows
ms.openlocfilehash: f617b01ee7dabd4897f99e374420f673e209e145
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859566"
---
# <a name="installation-of-openssh-for-windows-server-2019-and-windows-10"></a>Instalación de OpenSSH para Windows Server 2019 y Windows 10 #

El cliente OpenSSH y el servidor de OpenSSH son componentes por separado y en Windows Server 2019 y Windows 10 1809.
Los usuarios con estas versiones de Windows deben usar las instrucciones siguientes para instalar y configurar OpenSSH. 

> [!NOTE] 
> Los usuarios que adquirió OpenSSH desde el repositorio de Github de PowerShell (https://github.com/PowerShell/OpenSSH-Portable) debe usar las instrucciones a partir de ahí, y __no debería__ siga estas instrucciones. 


## <a name="installing-openssh-from-the-settings-ui-on-windows-server-2019-or-windows-10-1809"></a>Instalación de OpenSSH desde la configuración de la interfaz de usuario en Windows Server 2019 o Windows 10 1809

Servidor y cliente OpenSSH son instalables características de Windows 10 1809. 

Para instalar OpenSSH, configuración del inicio, a continuación, vaya a las aplicaciones > aplicaciones y características > Administrar características opcionales. 

Analizar esta lista para ver si ya está instalado el cliente OpenSSH. Si no es así, a continuación, en la parte superior de la página Seleccione "Agregar una característica", a continuación: 

* Para instalar al cliente OpenSSH, busque a "OpenSSH Client", a continuación, haga clic en "Instalar". 
* Para instalar al servidor de OpenSSH, busque "OpenSSH Server", a continuación, haga clic en "Instalar". 

Una vez finalizada la instalación, se devuelven a las aplicaciones > aplicaciones y características > Administrar características opcionales y debería ver los componentes de OpenSSH enumerados.

> [!NOTE]
> Instalar servidor de OpenSSH creará y habilitar una regla de firewall denominada "OpenSSH-Server-en-TCP". Esto permite el tráfico SSH entrante en el puerto 22. 

## <a name="installing-openssh-with-powershell"></a>Instalación de OpenSSH con PowerShell 

Para instalar OpenSSH mediante PowerShell, primero inicie PowerShell como administrador.
Para asegurarse de que las características de OpenSSH están disponibles para su instalación:

```powershell
Get-WindowsCapability -Online | ? Name -like 'OpenSSH*'

# This should return the following output:

Name  : OpenSSH.Client~~~~0.0.1.0
State : NotPresent
Name  : OpenSSH.Server~~~~0.0.1.0
State : NotPresent
```

A continuación, instale las características de servidor o cliente:

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

Para desinstalar OpenSSH usando la configuración de Windows, configuración de inicio, a continuación, vaya a las aplicaciones > aplicaciones y características > Administrar características opcionales. En la lista de las características instaladas, seleccione el componente cliente OpenSSH o OpenSSH Server y luego seleccione Desinstalar.

Para desinstalar OpenSSH mediante PowerShell, use uno de los siguientes comandos:

```powershell
# Uninstall the OpenSSH Client
Remove-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

# Uninstall the OpenSSH Server
Remove-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

Puede ser necesario reiniciar Windows después de quitar OpenSSH, si el servicio está en uso en el momento en se ha desinstalado.


## <a name="initial-configuration-of-ssh-server"></a>Configuración inicial del servidor SSH

Para configurar el servidor de OpenSSH para su uso inicial en Windows, inicie PowerShell como administrador, a continuación, ejecute los siguientes comandos para iniciar el servicio SSHD:

```powershell
Start-Service sshd
# OPTIONAL but recommended:
Set-Service -Name sshd -StartupType 'Automatic'
# Confirm the Firewall rule is configured. It should be created automatically by setup. 
Get-NetFirewallRule -Name *ssh*
# There should be a firewall rule named "OpenSSH-Server-In-TCP", which should be enabled 
```

## <a name="initial-use-of-ssh"></a>Uso inicial de SSH

Una vez haya instalado al servidor de OpenSSH en Windows, puede probar rápidamente con PowerShell desde cualquier dispositivo de Windows con el cliente SSH instalado. En PowerShell, escriba el siguiente comando: 

```powershell
Ssh username@servername
```

La primera conexión a cualquier servidor generará un mensaje similar al siguiente:

```
The authenticity of host 'servername (10.00.00.001)' can't be established.
ECDSA key fingerprint is SHA256:(<a large string>).
Are you sure you want to continue connecting (yes/no)?
```

La respuesta debe ser "yes" o "no". Si responde Sí agregará ese servidor en el sistema local de la lista de conocidos ssh hosts.

Se le pedirá la contraseña en este momento. Como precaución de seguridad, no se mostrará la contraseña a medida que escribe. 

Una vez conectado, verá un símbolo del shell de comando similar al siguiente:

```
domain\username@SERVERNAME C:\Users\username>
```

El shell predeterminado usado por Windows OpenSSH server es el shell de comandos de Windows. 

