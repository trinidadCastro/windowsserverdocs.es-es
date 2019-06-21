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
ms.openlocfilehash: a3fa1838096d910505faf9a2c5bd819b3a256fe2
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280420"
---
# <a name="deploy-windows-admin-center-in-azure"></a>Implementar Windows Admin Center en Azure

## <a name="deploy-using-script"></a>La implementación con script

Puede descargar [implementar WACAzVM.ps1](https://aka.ms/deploy-wacazvm) que se ejecutará desde [Azure Cloud Shell](https://shell.azure.com) para configurar una puerta de enlace de Windows Admin Center en Azure. Este script puede crear todo el entorno, incluido el grupo de recursos.

[Saltar a los pasos de implementación manual](#deploy-manually-on-an-existing-azure-virtual-machine)

### <a name="prerequisites"></a>Requisitos previos

* Configuración de la cuenta en [Azure Cloud Shell](https://shell.azure.com). Si se trata de la primera vez que usa Cloud Shell, se le pedirá que asociar o crear una cuenta de almacenamiento de Azure con Cloud Shell.
* En un **PowerShell** Cloud Shell, vaya a su directorio particular: ```PS Azure:\> cd ~```
* Para cargar el ```Deploy-WACAzVM.ps1``` de archivo, arrastre y quítelo de la máquina local en cualquier lugar en la ventana Cloud Shell.

Si especifica su propio certificado:

* Cargue el certificado que [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis). En primer lugar, cree un almacén de claves en Azure Portal y luego cargue el certificado al almacén de claves. Como alternativa, puede usar el Portal de Azure para generar un certificado para usted.

### <a name="script-parameters"></a>Parámetros de script

* **ResourceGroupName** -[String] Especifica el nombre del grupo de recursos donde se creará la máquina virtual.

* **Nombre** -[String] Especifica el nombre de la máquina virtual.

* **Credencial** -[PSCredential] especifica las credenciales para la máquina virtual.

* **Rutamsi** -[String] especifica la ruta de acceso local de Windows Admin Center MSI al implementar Windows Admin Center en una máquina virtual existente. El valor predeterminado es la versión de http://aka.ms/WACDownload si se omite.

* **VaultName** -[String] Especifica el nombre del almacén de claves que contiene el certificado.

* **CertName** -[String] Especifica el nombre del certificado que se usará para la instalación de MSI.

* **GenerateSslCert** -[modificador] True si el archivo MSI debe generar un certificado ssl firmado propio.

* **PortNumber** -[int] Especifica el número de puerto ssl para el servicio de Windows Admin Center. El valor predeterminado es 443 si se omite.

* **OpenPorts** -[int] especifica los puertos abiertos para la máquina virtual.

* **Ubicación** -[String] especifica la ubicación de la máquina virtual.

* **Tamaño** -[String] Especifica el tamaño de la máquina virtual. El valor predeterminado es "Standard_DS1_v2" si se omite.

* **Imagen** -[String] especifica la imagen de la máquina virtual. El valor predeterminado es "Win2016Datacenter" si se omite.

* **VirtualNetworkName** -[String] Especifica el nombre de la red virtual para la máquina virtual.

* **SubnetName** -[String] Especifica el nombre de la subred de la máquina virtual.

* **SecurityGroupName** -[String] Especifica el nombre del grupo de seguridad para la máquina virtual.

* **PublicIpAddressName** -[String] Especifica el nombre de la dirección IP pública para la máquina virtual.

* **InstallWACOnly** -[modificador] se establece en True si WAC debe instalarse en una máquina virtual de Azure ya existente.

Hay 2 opciones diferentes para el archivo MSI para implementar y el certificado usado para la instalación de MSI. O bien se puede descargar el archivo MSI desde aka.ms/WACDownload o, si la implementación en una máquina virtual existente, se puede proporcionar la ruta del archivo de un archivo MSI localmente en la máquina virtual. El certificado puede encontrarse en cualquier almacén de claves de Azure o se generará un certificado autofirmado mediante el archivo MSI.

### <a name="script-examples"></a>Ejemplos de script

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

#### <a name="example-1-use-the-script-to-deploy-wac-gateway-on-a-new-vm-in-a-new-virtual-network-and-resource-group-use-the-msi-from-akamswacdownload-and-a-self-signed-cert-from-the-msi"></a>Ejemplo 1: Usar la secuencia de comandos para implementar la puerta de enlace de WAC en una nueva máquina virtual en un nuevo grupo de recursos y la red virtual. Use el archivo MSI desde aka.ms/WACDownload y un certificado autofirmado desde el archivo MSI.

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

#### <a name="example-2-same-as-1-but-using-a-certificate-from-azure-key-vault"></a>Ejemplo 2: Igual que #1, pero con un certificado de Azure Key Vault.

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

#### <a name="example-3-using-a-local-msi-on-an-existing-vm-to-deploy-wac"></a>Ejemplo 3: Usar una MSI local en una máquina virtual existente para implementar WAC.

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

### <a name="requirements-for-vm-running-the-windows-admin-center-gateway"></a>Requisitos para la máquina virtual que ejecuta la puerta de enlace de Windows Admin Center

El puerto 443 (HTTPS) debe estar abierto.
Con las mismas variables definidas para la secuencia de comandos, puede usar el código siguiente en Azure Cloud Shell para actualizar el grupo de seguridad de red:

```powershell
$nsg = Get-AzNetworkSecurityGroup -Name $SecurityGroupName -ResourceGroupName $ResourceGroupName
$newNSG = Add-AzNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name ssl-rule -Description "Allow SSL" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 443
Set-AzNetworkSecurityGroup -NetworkSecurityGroup $newNSG
```

### <a name="requirements-for-managed-azure-vms"></a>Requisitos para Azure de VM administrada

Puerto 5985 (WinRM a través de HTTP) debe estar abierto y tiene un agente de escucha activo.
Puede usar el código siguiente en Azure Cloud Shell para actualizar los nodos administrados. ```$ResourceGroupName``` y ```$Name``` usar las mismas variables como el script de implementación, pero deberá usar el ```$Credential``` específicos de la máquina virtual que está administrando.

```powershell
Enable-AzVMPSRemoting -ResourceGroupName $ResourceGroupName -Name $Name
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any} -Credential $Credential
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {winrm create winrm/config/Listener?Address=*+Transport=HTTP} -Credential $Credential
```

## <a name="deploy-manually-on-an-existing-azure-virtual-machine"></a>Implementar manualmente en una máquina virtual de Azure

Antes de instalar Windows Admin Center en la máquina virtual de puerta de enlace deseado, instale un certificado SSL que se usará para la comunicación HTTPS o puede elegir usar un certificado autofirmado generado por Windows Admin Center. Sin embargo, recibirá una advertencia al intentar conectarse desde un explorador si elige esta última opción. Puede omitir esta advertencia en Edge, haga clic en **detalles > Ir la página Web** o, en Chrome, seleccionando **opciones avanzadas > continuar con la página [Web]** . Se recomienda que solo se usan certificados autofirmados para entornos de prueba.

> [!NOTE]
> Estas instrucciones son para la instalación en Windows Server con experiencia de escritorio, no en una instalación Server Core. 

1. [Descarga Windows Admin Center](https://aka.ms/windowsadmincenter) en el equipo local.

2. Establecer una conexión a escritorio remota a la máquina virtual, a continuación, copie el archivo MSI desde el equipo local y pegue en la máquina virtual.

3. Haga doble clic en el archivo MSI para comenzar la instalación y siga las instrucciones del asistente. Tener en cuenta lo siguiente:

   - De forma predeterminada, el instalador usa el puerto recomendado 443 (HTTPS). Si desea seleccionar un puerto diferente, tenga en cuenta que deberá abrir ese puerto en el servidor de seguridad. 

   - Si ya ha instalado un certificado SSL en la máquina virtual, asegúrese de seleccionar esa opción y escriba la huella digital.

4. Iniciar el servicio de Windows Admin Center (ejecutar C:/Program archivos/Windows Administrador Center/sme.exe)

[Más información acerca de la implementación de Windows Admin Center.](../deploy/install.md)

### <a name="configure-the-gateway-vm-to-enable-https-port-access"></a>Configurar la puerta de enlace de máquina virtual para habilitar el acceso a los puertos HTTPS: 

1. Vaya a la máquina virtual en Azure portal y seleccione **redes**. 

2. Seleccione **Agregar regla de puerto de entrada** y seleccione **HTTPS** en **servicio**. 

> [!NOTE]
> Si ha elegido un puerto distinto del valor predeterminado de 443, elija **personalizado** en el servicio y escriba el puerto que eligió en el paso 3 en **intervalos de puertos**. 

### <a name="accessing-a-windows-admin-center-gateway-installed-on-an-azure-vm"></a>Obtener acceso a una puerta de enlace de Windows Admin Center en una máquina virtual de Azure

En este momento, debe poder tener acceso a Windows Admin Center desde un explorador moderno (Edge o Chrome) en el equipo local, vaya al nombre DNS de la máquina virtual de puerta de enlace. 

> [!NOTE]
> Si ha seleccionado un puerto distinto de 443, puede tener acceso a Windows Admin Center, vaya a https://\<nombre DNS de la máquina virtual\>:\<puerto personalizado\>

Cuando se intenta acceder a Windows Admin Center, el explorador le solicitará las credenciales para tener acceso a la máquina virtual en el que está instalado Windows Admin Center. Aquí deberá especificar las credenciales que se encuentran en los usuarios locales o el grupo de administradores locales de la máquina virtual. 

Para agregar otras máquinas virtuales en la red virtual, asegúrese de que WinRM se está ejecutando en las máquinas virtuales de destino ejecutando lo siguiente en PowerShell o la línea de comandos en la máquina virtual de destino: `winrm quickconfig`

Si se aún no lo ha unido al dominio la máquina virtual de Azure, la máquina virtual se comporta como un servidor en el grupo de trabajo, por lo que necesitará para asegurarse de que se tiene en cuenta [mediante Windows Admin Center en un grupo de trabajo](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup).