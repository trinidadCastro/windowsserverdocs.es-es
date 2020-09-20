---
title: Implementar una puerta de enlace del centro de administración de Windows en Azure
description: Cómo implementar una puerta de enlace del centro de administración de Windows en Azure
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.date: 04/12/2019
ms.localizationpriority: medium
ms.openlocfilehash: 4fd03195feb275bd56c6958f8436c607829c8392
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766668"
---
# <a name="deploy-windows-admin-center-in-azure"></a>Implementación de Windows Admin Center en Azure

## <a name="deploy-using-script"></a>Implementación mediante el script

Puede descargar [Deploy-WACAzVM.ps1](https://aka.ms/deploy-wacazvm) que se ejecutarán desde [Azure Cloud Shell](https://shell.azure.com) para configurar una puerta de enlace del centro de administración de Windows en Azure. Este script puede crear todo el entorno, incluido el grupo de recursos.

[Saltar a los pasos de implementación manual](#deploy-manually-on-an-existing-azure-virtual-machine)

### <a name="prerequisites"></a>Requisitos previos

* Configure su cuenta en [Azure Cloud Shell](https://shell.azure.com). Si esta es la primera vez que usa Cloud Shell, se le pedirá que asocie o cree una cuenta de almacenamiento de Azure con Cloud Shell.
* En un Cloud Shell de **PowerShell** , vaya a su directorio principal: ```PS Azure:\> cd ~```
* Para cargar el ```Deploy-WACAzVM.ps1``` archivo, arrástrelo y colóquelo desde el equipo local en cualquier parte de la ventana de Cloud Shell.

Si especifica su propio certificado:

* Cargue el certificado en [Azure Key Vault](/azure/key-vault/key-vault-whatis). En primer lugar, cree un almacén de claves en Azure portal y, a continuación, cargue el certificado en el almacén de claves. Como alternativa, puede usar Azure portal para generar un certificado.

### <a name="script-parameters"></a>Parámetros de script

* **ResourceGroupName** -[cadena] especifica el nombre del grupo de recursos donde se creará la máquina virtual.

* **Name** -[cadena] especifica el nombre de la máquina virtual.

* **Credential** -[PSCredential] especifica las credenciales de la máquina virtual.

* **Rutamsi** -[cadena] especifica la ruta de acceso local del MSI del centro de administración de Windows al implementar el centro de administración de Windows en una máquina virtual existente. Tiene como valor predeterminado la versión de https://aka.ms/WACDownload si se omite.

* **VaultName** -[cadena] especifica el nombre del almacén de claves que contiene el certificado.

* **CertName** -[cadena] especifica el nombre del certificado que se va a usar para la instalación de MSI.

* **GenerateSslCert** -[switch] true si el MSI debe generar un certificado SSL autofirmado.

* **PortNumber** -[int] especifica el número de puerto SSL para el servicio del centro de administración de Windows. El valor predeterminado es 443 si se omite.

* **OpenPorts** -[int []] especifica los puertos abiertos para la máquina virtual.

* **Location** -[cadena] especifica la ubicación de la máquina virtual.

* **Size** -[cadena] especifica el tamaño de la máquina virtual. Si se omite, se toma como valor predeterminado "Standard_DS1_v2".

* **Image** -[cadena] especifica la imagen de la máquina virtual. Si se omite, se toma como valor predeterminado "Win2016Datacenter".

* **VirtualNetworkName** -[cadena] especifica el nombre de la red virtual para la máquina virtual.

* **SubnetName** -[cadena] especifica el nombre de la subred de la máquina virtual.

* **SecurityGroupName** -[cadena] especifica el nombre del grupo de seguridad de la máquina virtual.

* **PublicIpAddressName** -[cadena] especifica el nombre de la dirección IP pública de la máquina virtual.

* **InstallWACOnly** -[switch] establézcalo en true si WAC debe instalarse en una máquina virtual de Azure ya existente.

Hay dos opciones diferentes para la implementación de MSI y el certificado usado para la instalación de MSI. El MSI puede descargarse desde aka.ms/WACDownload o, si se implementa en una máquina virtual existente, se puede proporcionar la ruta de archivo de un MSI localmente en la máquina virtual. El certificado puede encontrarse en Azure Key Vault o bien el MSI generará un certificado autofirmado.

### <a name="script-examples"></a>Ejemplos de script

En primer lugar, defina las variables comunes necesarias para los parámetros del script.

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

#### <a name="example-1-use-the-script-to-deploy-wac-gateway-on-a-new-vm-in-a-new-virtual-network-and-resource-group-use-the-msi-from-akamswacdownload-and-a-self-signed-cert-from-the-msi"></a>Ejemplo 1: usar el script para implementar la puerta de enlace de WAC en una nueva máquina virtual en una nueva red virtual y un grupo de recursos. Use el archivo MSI de aka.ms/WACDownload y un certificado autofirmado del archivo MSI.

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

#### <a name="example-2-same-as-1-but-using-a-certificate-from-azure-key-vault"></a>Ejemplo 2: igual que #1, pero usar un certificado de Azure Key Vault.

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

#### <a name="example-3-using-a-local-msi-on-an-existing-vm-to-deploy-wac"></a>Ejemplo 3: uso de un MSI local en una máquina virtual existente para implementar WAC.

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

### <a name="requirements-for-vm-running-the-windows-admin-center-gateway"></a>Requisitos de la máquina virtual que ejecuta la puerta de enlace del centro de administración de Windows

El puerto 443 (HTTPS) debe estar abierto.
Con las mismas variables definidas para el script, puede usar el código siguiente en Azure Cloud Shell para actualizar el grupo de seguridad de red:

```powershell
$nsg = Get-AzNetworkSecurityGroup -Name $SecurityGroupName -ResourceGroupName $ResourceGroupName
$newNSG = Add-AzNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name ssl-rule -Description "Allow SSL" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 443
Set-AzNetworkSecurityGroup -NetworkSecurityGroup $newNSG
```

### <a name="requirements-for-managed-azure-vms"></a>Requisitos para máquinas virtuales de Azure administradas

El puerto 5985 (WinRM sobre HTTP) debe estar abierto y tener un agente de escucha activo.
Puede usar el código siguiente en Azure Cloud Shell para actualizar los nodos administrados. ```$ResourceGroupName``` y ```$Name``` usan las mismas variables que el script de implementación, pero tendrá que usar el ```$Credential``` específico para la máquina virtual que administra.

```powershell
Enable-AzVMPSRemoting -ResourceGroupName $ResourceGroupName -Name $Name
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any} -Credential $Credential
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {winrm create winrm/config/Listener?Address=*+Transport=HTTP} -Credential $Credential
```

## <a name="deploy-manually-on-an-existing-azure-virtual-machine"></a>Implementar manualmente en una máquina virtual de Azure existente

Antes de instalar el centro de administración de Windows en la máquina virtual de puerta de enlace deseada, instale un certificado SSL que se usará para la comunicación HTTPS, o bien puede usar un certificado autofirmado generado por el centro de administración de Windows. Sin embargo, recibirá una advertencia al intentar conectarse desde un explorador si elige la última opción. Para omitir esta advertencia en Edge, haga clic en **detalles > vaya a la página web** o, en Chrome, seleccione **avanzadas > continúe con [Página Web]**. Se recomienda usar solo certificados autofirmados para entornos de prueba.

> [!NOTE]
> Estas instrucciones son para instalar en Windows Server con experiencia de escritorio, no en una instalación Server Core.

1. [Descargue el centro de administración de Windows](../overview.md) en el equipo local.

2. Establezca una conexión de escritorio remoto a la máquina virtual, copie el MSI desde la máquina local y péguelo en la máquina virtual.

3. Haga doble clic en el archivo MSI para iniciar la instalación y siga las instrucciones del asistente. Tenga en cuenta lo siguiente:

   - De forma predeterminada, el instalador usa el puerto recomendado 443 (HTTPS). Si desea seleccionar un puerto diferente, tenga en cuenta que también debe abrir ese puerto en el firewall.

   - Si ya ha instalado un certificado SSL en la máquina virtual, asegúrese de seleccionar esa opción y escriba la huella digital.

4. Iniciar el servicio del centro de administración de Windows (ejecutar C:/archivos de programa/Centro de administración de Windows/sme.exe)

[Obtenga más información sobre la implementación del centro de administración de Windows.](../deploy/install.md)

### <a name="configure-the-gateway-vm-to-enable-https-port-access"></a>Configuración de la máquina virtual de puerta de enlace para habilitar el acceso de Puerto HTTPS:

1. Vaya a la máquina virtual en el Azure Portal y seleccione **redes**.

2. Seleccione **Agregar regla de puerto de entrada** y seleccione **https** en **servicio**.

> [!NOTE]
> Si elige un puerto distinto del valor predeterminado 443, elija **personalizado** en servicio y escriba el puerto que eligió en el paso 3 en **intervalos de puertos**.

### <a name="accessing-a-windows-admin-center-gateway-installed-on-an-azure-vm"></a>Acceso a una puerta de enlace del centro de administración de Windows instalada en una máquina virtual de Azure

Llegados a este punto, debería poder acceder al centro de administración de Windows desde un explorador moderno (Edge o Chrome) en el equipo local. para ello, vaya al nombre DNS de la máquina virtual de la puerta de enlace.

> [!NOTE]
> Si ha seleccionado un puerto distinto de 443, puede tener acceso al centro de administración de Windows; para ello, vaya a https:// \<DNS name of your VM\> :\<custom port\>

Al intentar obtener acceso al centro de administración de Windows, el explorador solicitará las credenciales para tener acceso a la máquina virtual en la que está instalado el centro de administración de Windows. Aquí tendrá que especificar las credenciales que se encuentran en el grupo local usuarios o administradores locales de la máquina virtual.

Para agregar otras máquinas virtuales a la red virtual, asegúrese de que WinRM se ejecuta en las máquinas virtuales de destino mediante la ejecución de lo siguiente en PowerShell o el símbolo del sistema en la máquina virtual de destino: `winrm quickconfig`

Si no se ha unido a un dominio de la máquina virtual de Azure, la máquina virtual se comporta como un servidor en grupo de trabajo, por lo que debe asegurarse de que tiene en cuenta el [uso del centro de administración de Windows en un grupo de trabajo](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup).