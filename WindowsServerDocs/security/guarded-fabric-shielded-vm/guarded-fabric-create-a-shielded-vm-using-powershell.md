---
title: Crear una máquina virtual blindada mediante PowerShell
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 76be26e107bd16165367d5432e1dd757dea2f9b4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855416"
---
# <a name="create-a-shielded-vm-using-powershell"></a>Crear una máquina virtual blindada mediante PowerShell

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

En producción, normalmente usaría un administrador de tejido (por ejemplo, VMM) para implementar las máquinas virtuales blindadas. Sin embargo, los pasos que se muestra a continuación le permiten implementar y validar todo el escenario sin un administrador de tejido.

En pocas palabras, creará un disco de plantilla, un archivo de datos de blindaje, un archivo de respuesta de instalación desatendida y otros artefactos de seguridad en cualquier equipo, a continuación, copie estos archivos en un host protegido y aprovisionar la máquina virtual blindada.

## <a name="create-a-signed-template-disk"></a>Crear un disco de plantilla firmada

Para crear una nueva máquina virtual blindada, necesita primero un disco de plantilla de máquina virtual blindado que está cifrado previamente con su volumen de sistema operativo (o particiones de arranque y raíz en Linux) firmado.
Siga los vínculos siguientes para obtener más información sobre cómo crear un disco de plantilla.

- [Preparar un disco de plantilla de Windows](guarded-fabric-create-a-shielded-vm-template.md)
- [Preparar un disco de plantilla de Linux](guarded-fabric-create-a-linux-shielded-vm-template.md)

También necesitará una copia del catálogo de firmas de volumen del disco para crear el archivo de datos de blindaje.
Para guardar este archivo, ejecute el siguiente comando en el equipo donde se creó el disco de plantilla:

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath "C:\temp\MyTemplateDisk.vhdx" -VolumeSignatureCatalogPath "C:\temp\MyTemplateDiskCatalog.vsc"
```

## <a name="download-guardian-metadata"></a>Descargar los metadatos del guardián

Para cada uno de los tejidos de virtualización donde desea ejecutar la máquina virtual blindada, deberá obtener metadatos del guardián para los clústeres HGS de tejidos.
El proveedor de hospedaje debe ser capaz de proporcionar esta información.

Si se encuentra en un entorno empresarial y puede comunicarse con el servidor HGS, están disponible en los metadatos del guardián *http://\<HGSCLUSTERNAME\>/KeyProtection/service/metadata/2014-07/metadata.xml*

## <a name="create-shielding-data-pdk-file"></a>Crear archivo de datos de blindaje (PDK)

Los datos de blindaje se crean y que pertenecen a los propietarios de máquina virtual de inquilino y contienen secretos necesarios para crear máquinas virtuales blindadas que se deben proteger desde el administrador del tejido, tales como contraseña del Administrador de la máquina virtual blindada.
Los datos de blindaje se cifran, que sólo los servidores HGS y el inquilino pueden descifrarlo.
Una vez creado por el propietario del inquilino o de máquinas virtuales, debe copiarse el archivo PDK resultante en el tejido protegido.
Para obtener más información, consulte [lo es con los datos de blindaje y por qué es necesario?](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary).

Además, necesitará un archivo de respuesta de instalación desatendida (unattend.xml para Windows, varía para Linux). Consulte [crear un archivo de respuesta](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file) para obtener instrucciones sobre qué incluir en el archivo de respuesta.

Las máquinas virtuales blindadas instalado, ejecute los siguientes cmdlets en un equipo con herramientas de administración remota del servidor.
Si está creando un PDK para una VM de Linux, debe hacerlo en un servidor que ejecuta Windows Server, versión 1709 o posterior.

 
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
    
## <a name="provision-shielded-vm-on-a-guarded-host"></a>Aprovisionar máquinas virtuales blindadas en un host protegido
Copie el archivo de disco de plantilla (ServerOS.vhdx) y el archivo PDK (contoso.pdk) en el host protegido y prepárese para la implementación.

En el host protegido, instale el módulo de PowerShell de herramientas de tejido protegido, que contiene el cmdlet New-ShieldedVM para simplificar el proceso de aprovisionamiento. Si el host protegido tiene acceso a Internet, ejecute el siguiente comando:

```powershell
Install-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0
```

También puede descargar el módulo en otro equipo que tiene Internet acceder y copiar el módulo resultante `C:\Program Files\WindowsPowerShell\Modules` en el host protegido.

```powershell
Save-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0 -Path C:\temp\
```

Una vez que el módulo está instalado, está listo para aprovisionar la máquina virtual blindada.

```powershell
New-ShieldedVM -Name 'MyShieldedVM' -TemplateDiskPath 'C:\temp\MyTemplateDisk.vhdx' -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -Wait
```

Si el disco de plantilla contiene un sistema operativo basado en Linux, incluya el `-Linux` marca cuando ejecute el comando:

```powershell
New-ShieldedVM -Name 'MyLinuxVM' -TemplateDiskPath 'C:\temp\MyTemplateDisk.vhdx' -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -Wait -Linux
```

Compruebe el contenido de ayuda con `Get-Help New-ShieldedVM -Full` para obtener más información sobre otras opciones que puede pasar al cmdlet.

Una vez que la máquina virtual finalice el aprovisionamiento, introducirá la fase de especialización específica del sistema operativo, tras el cual estará listo para su uso.
Asegúrese de conectar la máquina virtual a una red válida para que pueda conectarse a ella una vez que se está ejecutando (mediante RDP, SSH, PowerShell o la herramienta de administración preferidos).

## <a name="running-shielded-vms-on-a-hyper-v-cluster"></a>Que se ejecutan las máquinas virtuales blindadas en un clúster de Hyper-V

Si está intentando implementar máquinas virtuales blindadas en hosts protegidos en clúster (con un clúster de conmutación por error de Windows), puede configurar la máquina virtual blindada para tener alta disponibilidad mediante el siguiente cmdlet:

```powershell
Add-ClusterVirtualMachineRole -VMName 'MyShieldedVM' -Cluster <Hyper-V cluster name>
```

La máquina virtual blindada ahora pueden en vivo migra dentro del clúster.

## <a name="next-step"></a>Paso siguiente

>[!div class="nextstepaction"]
[Implementar un blindadas con VMM](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)