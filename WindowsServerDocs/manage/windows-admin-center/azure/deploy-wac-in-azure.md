---
title: Implementar una puerta de enlace de Windows Admin Center en Azure
description: Cómo implementar una puerta de enlace de Windows Admin Center en Azure
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.date: 04/12/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: af766c2e0502d9fe633cae42d999db5cbffc32c8
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297100"
---
# Implementar Windows Admin Center en Azure

## Implementar mediante script

Puedes descargar [WACAzVM.ps1 de implementar](https://aka.ms/deploy-wacazvm) que se ejecuta desde el [Shell en la nube de Azure](https://shell.azure.com) para configurar una puerta de enlace de Windows Admin Center en Azure. Este script puede crear todo el entorno, incluido el grupo de recursos.

[Accesos directos a los pasos de implementación manual](#deploy-manually-on-an-existing-azure-virtual-machine)

### Requisitos previos

* Configurar tu cuenta en [El Shell en la nube de Azure](https://shell.azure.com). Si es la primera vez mediante el Shell en la nube, se te pedirá que asociar o crear una cuenta de Azure storage con el Shell en la nube.
* En un Shell de **PowerShell** en la nube, ve a tu directorio principal: ```PS Azure:\> cd ~```
* Para cargar el ```Deploy-WACAzVM.ps1``` , arrastrar y soltar desde el equipo local a cualquier lugar archivos en la ventana de Shell en la nube.

Si se especifica tu propio certificado:

* Cargar el certificado al [Almacén de claves de Azure](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis). En primer lugar, crea un almacén de claves en el Portal de Azure, cargar el certificado en el almacén de claves. Como alternativa, puedes usar el Portal de Azure para generar un certificado para TI.

### Parámetros de script

* **ResourceGroupName** - [String] Especifica el nombre del grupo de recursos donde se creará la máquina virtual.

* **Nombre** : [String] Especifica el nombre de la máquina virtual.

* **Credential** - [PSCredential] especifica las credenciales de la máquina virtual.

* **RutaDeAccesoMSI** - [String] especifica la ruta de acceso local de Windows Admin Center MSI al implementar Windows Admin Center en una máquina virtual existente. El valor predeterminado es la versión de http://aka.ms/WACDownload si se omite.

* **VaultName** - [String] Especifica el nombre del almacén de claves que contiene el certificado.

* **CertName** - [String] Especifica el nombre del certificado que se usará para la instalación de MSI.

* **GenerateSslCert** - [modificador] True si el archivo MSI debe generar un certificado autofirmado ssl firmado.

* **NúmeroDePuerto** - [int] Especifica el número de puerto ssl para el servicio de Windows Admin Center. El valor predeterminado es 443 si se omite.

* **OpenPorts** - [int []] especifica los puertos abiertos para la máquina virtual.

* **Ubicación** : [String] especifica la ubicación de la máquina virtual.

* **Tamaño** : [String] Especifica el tamaño de la máquina virtual. El valor predeterminado es "Standard_DS1_v2" si se omite.

* **Imagen** - [String] especifica la imagen de la máquina virtual. El valor predeterminado es "Win2016Datacenter" si se omite.

* **VirtualNetworkName** - [String] Especifica el nombre de la red virtual para la máquina virtual.

* **SubnetName** - [String] Especifica el nombre de la subred de la máquina virtual.

* **SecurityGroupName** - [String] Especifica el nombre del grupo de seguridad de la máquina virtual.

* **PublicIpAddressName** - [String] Especifica el nombre de la dirección IP pública de la máquina virtual.

* **InstallWACOnly** - [modificador] configurado en True si WAC debe instalarse en una VM de Azure existentes.

Existen 2 opciones diferentes para el archivo MSI para implementar y el certificado utilizado para la instalación de MSI. El archivo MSI o bien puede descargarse desde aka.ms/WACDownload, o bien, si la implementación en una máquina virtual existente, se puede proporcionar la ruta del archivo de un archivo MSI localmente en la máquina virtual. El certificado puede encontrarse en cualquier almacén de claves de Azure o MSI generará un certificado autofirmado.

### Ejemplos de script

En primer lugar, definir variables comunes necesarias para los parámetros de la secuencia de comandos.

```PowerShell
$ResourceGroupName = "wac-rg1" 
$VirtualNetworkName = "wac-vnet"
$SecurityGroupName = "wac-nsg"
$SubnetName = "wac-subnet"
$VaultName = "wac-key-vault"
$CertName = "wac-cert"
$Location = "westus"
$PublicIpAddressName = "wac-public-ip"
$Size = "Standard_D4s_v3"
$Image = "Win2016Datacenter"
$Credential = Get-Credential
```

#### Ejemplo 1: Usar el script para implementar la puerta de enlace de WAC en una nueva máquina virtual en un nuevo grupo de recursos y de red virtual. Usar el archivo MSI desde aka.ms/WACDownload y un certificado autofirmado de MSI.

```PowerShell
$scriptParams = @{
    ResourceGroupName = $ResourceGroupName
    Name = "wac-vm1"
    Credential = $Credential
    VirtualNetworkName = $VirtualNetworkName
    SubnetName = $SubnetName
    GenerateSslCert = $true
}
./Deploy-WACAzVM.ps1 @scriptParams
```

#### Ejemplo 2: Igual que #1, pero con un certificado del almacén de claves de Azure.

```PowerShell
$scriptParams = @{
    ResourceGroupName = $ResourceGroupName
    Name = "wac-vm2"
    Credential = $Credential
    VirtualNetworkName = $VirtualNetworkName
    SubnetName = $SubnetName
    VaultName = $VaultName
    CertName = $CertName
}
./Deploy-WACAzVM.ps1 @scriptParams
```

#### Ejemplo 3: Usar un MSI local en una máquina virtual existente para implementar WAC.

```PowerShell
$MsiPath = "C:\Users\<username>\Downloads\WindowsAdminCenter<version>.msi"
$scriptParams = @{
    ResourceGroupName = $ResourceGroupName
    Name = "wac-vm3"
    Credential = $Credential
    MsiPath = $MsiPath
    InstallWACOnly = $true
    GenerateSslCert = $true
}
./Deploy-WACAzVM.ps1 @scriptParams
```

### Requisitos para la máquina virtual que se ejecuta la puerta de enlace de Windows Admin Center

El puerto 443 (HTTPS) debe estar abierto.
Con las mismas variables definidas para el alfabeto, puedes usar el siguiente código en el Shell en la nube de Azure para actualizar el grupo de seguridad de red:

```powershell
$nsg = Get-AzNetworkSecurityGroup -Name $SecurityGroupName -ResourceGroupName $ResourceGroupName
$newNSG = Add-AzNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name ssl-rule -Description "Allow SSL" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 443
Set-AzNetworkSecurityGroup -NetworkSecurityGroup $newNSG
```

### Requisitos para administrado VM de Azure

Puerto 5985 (WinRM a través de HTTP) debe estar abierto y tener un agente de escucha activo.
Puedes usar el siguiente código en el Shell en la nube de Azure para actualizar los nodos administrados. ```$ResourceGroupName``` y ```$Name``` usar las mismas variables como el script de implementación, pero tendrás que usar el ```$Credential``` específico de la máquina virtual se está administrando.

```powershell
Enable-AzVMPSRemoting -ResourceGroupName $ResourceGroupName -Name $Name
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any} -Credential $Credential
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {winrm create winrm/config/Listener?Address=*+Transport=HTTP} -Credential $Credential
```

## Implementar manualmente en una máquina virtual de Azure existente

Antes de instalar Windows Admin Center en la máquina virtual de puerta de enlace deseado, instala un certificado SSL que se usará para la comunicación HTTPS, o también puedes usar un certificado autofirmado generado por Windows Admin Center. Sin embargo, obtendrá una advertencia al intentar conectarse desde un explorador, si eliges la última opción. Puedes omitir esta advertencia en Edge haciendo clic en **los detalles > acceder a la página Web** o, en Chrome, seleccionando **Avanzadas > continuar a [página Web]**. Te recomendamos que uses solo certificados autofirmados para entornos de prueba.

> [!NOTE]
> Estas instrucciones son para instalar en Windows Server con experiencia de escritorio, no en una instalación Server Core. 

1. [Descargar Windows Admin Center](https://aka.ms/windowsadmincenter) en el equipo local.

2. Establecer una conexión a Escritorio remoto a la máquina virtual, a continuación, copia el archivo MSI desde el equipo local y pegarlo en la máquina virtual.

3. Haz doble clic en el archivo MSI para comenzar la instalación y sigue las instrucciones del asistente. Tener en cuenta lo siguiente:

   - De manera predeterminada, el instalador utiliza el puerto recomendado 443 (HTTPS). Si quieres seleccionar un puerto diferente, ten en cuenta que debes abrir ese puerto en el servidor de seguridad. 

   - Si ya ha instalado un certificado SSL en la máquina virtual, asegúrate de seleccionar esa opción y escribe la huella digital.

4. Iniciar el servicio de Windows Admin Center (Ejecutar programa/C: archivos o Windows Admin Center/sme.exe)

[Más información sobre la implementación de Windows Admin Center.](../deploy/install.md)

### Configurar la puerta de enlace de máquina virtual para habilitar el acceso de puerto HTTPS: 

1. Ve a la máquina virtual en el portal de Azure y selecciona **redes**. 

2. Seleccione **Agregar regla de puerto de entrada** y **HTTPS** en **el servicio**. 

> [!NOTE]
> Si has elegido un puerto que no sea el valor predeterminado 443, elige **personalizado** en el servicio y escribe el puerto que eligió en el paso 3 en **intervalos de puertos**. 

### Acceso a una puerta de enlace de Windows Admin Center instalado en una VM de Azure

En este punto, debe tener acceso a Windows Admin Center desde un navegador moderno (Edge o Chrome) en el equipo local, desplázate hasta el nombre DNS de la máquina virtual de puerta de enlace. 

> [!NOTE]
> Si has seleccionado un puerto 443, puede tener acceso a Windows Admin Center navegar a https://\<DNS nombre de tu VM\>:\<custom port\>

Cuando se intenta acceder a Windows Admin Center, el explorador pedirá las credenciales acceder a la máquina virtual en el que está instalado Windows Admin Center. Aquí, tendrás que escribir las credenciales que se encuentran en los usuarios locales o el grupo de administradores locales de la máquina virtual. 

Para agregar otras máquinas virtuales en el VNet, asegúrate de que WinRM se ejecuta en el destino de las máquinas virtuales ejecutando lo siguiente en PowerShell o el símbolo del sistema en el destino de la máquina virtual: `winrm quickconfig`

Si se no has unido al dominio la VM de Azure, la máquina virtual se comporta como un servidor de grupo de trabajo, por lo que tendrás que tener en que cuenta para [usar Windows Admin Center en un grupo de trabajo](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup).