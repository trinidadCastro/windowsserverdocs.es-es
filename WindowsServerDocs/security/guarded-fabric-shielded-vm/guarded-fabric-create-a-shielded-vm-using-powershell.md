---
title: Creación de una máquina virtual blindada mediante PowerShell
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 888177c1288216c28f7d4c0a667fd81e93bdce8c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402400"
---
# <a name="create-a-shielded-vm-using-powershell"></a>Creación de una máquina virtual blindada mediante PowerShell

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

En producción, normalmente usaría un administrador de tejido (por ejemplo, VMM) para implementar máquinas virtuales blindadas. Sin embargo, los pasos que se muestran a continuación permiten implementar y validar todo el escenario sin un administrador de tejido.

En pocas palabras, creará un disco de plantilla, un archivo de datos de blindaje, un archivo de respuesta de instalación desatendida y otros artefactos de seguridad en cualquier equipo y, después, copiará estos archivos en un host protegido y aprovisionará la máquina virtual blindada.

## <a name="create-a-signed-template-disk"></a>Creación de un disco de plantilla firmado

Para crear una nueva máquina virtual blindada, primero necesita un disco de plantilla de máquina virtual blindada que esté cifrado previamente con el volumen del sistema operativo (o las particiones de arranque y raíz en Linux) firmado.
Siga los vínculos siguientes para obtener más información sobre cómo crear un disco de plantilla.

- [Preparar un disco de plantilla de Windows](guarded-fabric-create-a-shielded-vm-template.md)
- [Preparación de un disco de plantilla de Linux](guarded-fabric-create-a-linux-shielded-vm-template.md)

También necesitará una copia del catálogo de firmas de volumen del disco para crear el archivo de datos de blindaje.
Para guardar este archivo, ejecute el siguiente comando en el equipo en el que creó el disco de plantilla:

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath "C:\temp\MyTemplateDisk.vhdx" -VolumeSignatureCatalogPath "C:\temp\MyTemplateDiskCatalog.vsc"
```

## <a name="download-guardian-metadata"></a>Descargar los metadatos de Guardian

Para cada tejido de virtualización en el que quiera ejecutar la máquina virtual blindada, deberá obtener los metadatos de protección para los clústeres de HGS de los tejidos.
El proveedor de hospedaje debe ser capaz de proporcionar esta información.

Si se encuentra en un entorno empresarial y puede comunicarse con el servidor HGS, los metadatos de Guardian están disponibles en *http://\<HGSCLUSTERNAME @ no__t-2/KeyProtection/Service/Metadata/2014-07/Metadata. XML*

## <a name="create-shielding-data-pdk-file"></a>Creación de un archivo de datos de blindaje (PDK)

Los datos de blindaje se crean y son propiedad de los propietarios de máquinas virtuales de los inquilinos y contienen los secretos necesarios para crear máquinas virtuales blindadas que se deben proteger del administrador del tejido, como la contraseña de administrador de la máquina virtual blindada.
Los datos de blindaje se cifran de forma que solo los servidores y el inquilino de HGS puedan descifrarlos.
Una vez creado por el propietario de la máquina virtual o el inquilino, el archivo PDK resultante debe copiarse en el tejido protegido.
Para obtener más información, consulte [¿Qué son los datos de blindaje y por qué es necesario?](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary).

Además, necesitará un archivo de respuesta de instalación desatendida (Unattend. XML para Windows, varía en Linux). Vea [crear un archivo de respuesta](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file) para obtener instrucciones sobre lo que se debe incluir en el archivo de respuesta.

Ejecute los siguientes cmdlets en un equipo con la Herramientas de administración remota del servidor para las máquinas virtuales blindadas instaladas.
Si va a crear un PDK para una máquina virtual Linux, debe hacerlo en un servidor que ejecute Windows Server, versión 1709 o posterior.

 
```powershell
# Create owner certificate, don't lose this!
# The certificate is stored at Cert:\LocalMachine\Shielded VM Local Certificates
$Owner = New-HgsGuardian –Name 'Owner' –GenerateCertificates
 
# Import the HGS guardian for each fabric you want to run your shielded VM
$Guardian = Import-HgsGuardian -Path C:\HGSGuardian.xml -Name 'TestFabric'
 
# Create the PDK file
# The "Policy" parameter describes whether the admin can see the VM's console or not
# Use "EncryptionSupported" if you are testing out shielded VMs and want to debug any issues during the specialization process
New-ShieldingDataFile -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -Owner $Owner –Guardian $guardian –VolumeIDQualifier (New-VolumeIDQualifier -VolumeSignatureCatalogFilePath 'C:\temp\MyTemplateDiskCatalog.vsc' -VersionRule Equals) -WindowsUnattendFile 'C:\unattend.xml' -Policy Shielded
```
    
## <a name="provision-shielded-vm-on-a-guarded-host"></a>Aprovisionar una máquina virtual blindada en un host protegido
Copie el archivo de disco de plantilla (Serveros. vhdx) y el archivo PDK (contoso. PDK) en el host protegido para preparar la implementación.

En el host protegido, instale el módulo de PowerShell herramientas de tejido protegido, que contiene el cmdlet New-ShieldedVM para simplificar el proceso de aprovisionamiento. Si el host protegido tiene acceso a Internet, ejecute el siguiente comando:

```powershell
Install-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0
```

También puede descargar el módulo en otro equipo que tenga acceso a Internet y copiar el módulo resultante en `C:\Program Files\WindowsPowerShell\Modules` en el host protegido.

```powershell
Save-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0 -Path C:\temp\
```

Una vez instalado el módulo, está listo para aprovisionar la máquina virtual blindada.

```powershell
New-ShieldedVM -Name 'MyShieldedVM' -TemplateDiskPath 'C:\temp\MyTemplateDisk.vhdx' -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -Wait
```

Si el disco de plantilla contiene un sistema operativo basado en Linux, incluya el indicador `-Linux` al ejecutar el comando:

```powershell
New-ShieldedVM -Name 'MyLinuxVM' -TemplateDiskPath 'C:\temp\MyTemplateDisk.vhdx' -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -Wait -Linux
```

Consulte el contenido de la ayuda con `Get-Help New-ShieldedVM -Full` para obtener más información sobre otras opciones que puede pasar al cmdlet.

Una vez que la máquina virtual finaliza el aprovisionamiento, entrará en la fase de especialización específica del sistema operativo, después de la cual estará lista para su uso.
Asegúrese de conectar la máquina virtual a una red válida para que pueda conectarse a ella una vez que se esté ejecutando (mediante RDP, PowerShell, SSH o su herramienta de administración preferida).

## <a name="running-shielded-vms-on-a-hyper-v-cluster"></a>Ejecución de máquinas virtuales blindadas en un clúster de Hyper-V

Si intenta implementar máquinas virtuales blindadas en hosts protegidos en clúster (con un clúster de conmutación por error de Windows), puede configurar la máquina virtual blindada para que tenga una alta disponibilidad mediante el siguiente cmdlet:

```powershell
Add-ClusterVirtualMachineRole -VMName 'MyShieldedVM' -Cluster <Hyper-V cluster name>
```

Ahora se puede migrar en vivo la máquina virtual blindada dentro del clúster.

## <a name="next-step"></a>Paso siguiente

> [!div class="nextstepaction"]
> [Implementación de un blindado mediante VMM](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)