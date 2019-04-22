---
redirect_url: guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md
title: 'Blindadas de las máquinas virtuales blindadas para inquilinos: crear una nueva máquina virtual local y la mueve a un tejido protegido'
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 0ca1efa0-01f9-4b6f-87d4-c66db00d7d70
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 2da1e33d24fa6d68815f4fbc0891be0616004856
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817476"
---
# <a name="shielded-vms-for-tenants---creating-a-new-shielded-vm-on-premises-and-moving-it-to-a-guarded-fabric"></a>Blindadas de las máquinas virtuales blindadas para inquilinos: crear una nueva máquina virtual local y la mueve a un tejido protegido

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016


<!-- NOTE THAT THIS FILE HAS A "redirect_url" LINE IN THE METADATA. EVENTUALLY WE WILL PROBABLY STRIP OUT THE DETAILED METADATA AND THE CONTENT BELOW, SO IT'S PURELY A REDIRECTED TOPIC. However, as of mid-November 2016, we're still deciding. -->



En este tema se describe los pasos para crear una máquina virtual blindada con solo Hyper-V; es decir, sin Virtual Machine Manager, los discos de plantilla o un archivo de datos de blindaje. Esto es un escenario poco común para la nube pública más entornos de hospedaje, pero puede resultar útil cuando se prueba a un tejido protegido o de empresa escenarios donde se mueve una máquina virtual desde una estructura de departamentos que comparten la infraestructura de TI y deben cifrarse antes de la migración.

Para entender cómo encaja este tema en el proceso general de la implementación de máquinas virtuales blindadas, consulte [hospeda los pasos de configuración del proveedor de servicio para hosts protegidos y máquinas virtuales blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md).

## <a name="import-the-guardian-configuration-on-the-tenant-hyper-v-server"></a>Importar la configuración de protección en el servidor de Hyper-V de inquilino

1.  Antes de comenzar el procedimiento, asegúrese de que se encuentra en un host de Hyper-V que ejecuta Windows Server 2016 con los siguientes roles y características instaladas:

    - Rol

        - Hyper-V

    - Características

        - Herramientas de administración remota del servidor\\herramientas de administración de características\\blindadas herramientas de la máquina virtual

    > [!NOTE]
    > El host se usa aquí debe *no* ser un host en el tejido protegido. Se trata de un host independiente que se preparan las máquinas virtuales antes de que se mueven en el tejido protegido.

2.  Antes de poder ejecutar una máquina virtual blindada en esta máquina, deberá garantizar la que seguridad basada en la virtualización está habilitado y ejecutándose. Ejecute el siguiente comando en una ventana de Windows PowerShell con privilegios elevados para instalar la característica de compatibilidad de Hyper-V de guardián de Host. Esto configurará todos los valores necesarios en el equipo para poder ejecutar máquinas virtuales blindadas.

        Install-WindowsFeature HostGuardian

3.  Deberá adquirir los metadatos del guardián para el tejido protegido que se ejecutará la máquina virtual. Estos metadatos se usan para autorizar a ese tejido la ejecute la máquina virtual blindada. Cómo obtener esta información será diferente para cada proveedor de servicios de hospedaje o enterprise. El proveedor de hospedaje (o, si tiene acceso a la red de tejido protegido) puede adquirir esta información mediante la ejecución del siguiente comando de Windows PowerShell:

        Invoke-WebRequest 'http://hgs.bastion.local/KeyProtection/service/metadata/2014-07/metadata.xml' -OutFile .\RelecloudGuardian.xml

    En el ejemplo anterior, "hgs" es el nombre de red distribuida del clúster HGS y "bastion.local" es el nombre del dominio HGS.

4.  Para importar la clave de protección, ya que lo necesitará en un procedimiento posterior, ejecute el siguiente comando.

    Para &lt;ruta de acceso&gt;&lt;Filename&gt;, sustituir la ruta de acceso y nombre de archivo del código XML de archivo se guarda en el paso anterior, por ejemplo: **C:\\temp\\GuardianKey.xml**

    Para &lt;GuardianName&gt;, especifique un nombre para el proveedor de hospedaje o centro de datos empresarial, por ejemplo, **HostingProvider1**. Registre el nombre para el siguiente procedimiento.

    Incluir **- AllowUntrustedRoot** sólo si el servidor HGS se configuró con los certificados autofirmados. (Estos certificados forman parte del servicio de protección de clave en el HGS.)

        Import-HgsGuardian -Path '<Path><Filename>' -Name '<GuardianName>' -AllowUntrustedRoot

## <a name="create-a-new-shielded-virtual-machine-on-the-host"></a>Cree una nueva máquina virtual blindada en el host

En este procedimiento, creará una máquina virtual en el host de Hyper-V y prepararlos para la exportación a su proveedor de hospedaje o el administrador del centro de datos, que puede ejecutarlo en un host protegido.

Como parte del procedimiento, creará un Protector de clave que contiene dos elementos importantes:

-   **Propietario**: En el Protector de clave - o bien más probable es que el grupo que trabaja, que comparte los elementos de seguridad, como certificados - se identifica como "propietario" de la máquina virtual. Su identidad como propietario se representa mediante un certificado que, si ejecuta los comandos como se muestra, se genera como un certificado autofirmado. Si lo desea, puede usar un certificado respaldado por la infraestructura de PKI y omite la **- AllowUntrustedRoot** parámetro en los comandos.

-   **Tutores**: También en el Protector de clave, el proveedor de hospedaje o centro de datos de empresa (que se ejecuta HGS y hosts protegidos) se identifica como "protección". La protección está representado por la clave de protección que ha importado en el procedimiento anterior, [importar la configuración de protección en el servidor de Hyper-V inquilino](#import-the-guardian-configuration-on-the-tenant-hyper-v-server).

Para ver una ilustración que muestra el Protector de clave, que es un elemento en un archivo de datos de blindaje, consulte [lo es con los datos de blindaje y por qué es necesario?](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary).

1.  En un host de Hyper-V de inquilino para crear una máquina virtual nueva generación 2, ejecute el siguiente comando.

    Para &lt;ShieldedVMname&gt;, especifique un nombre para la máquina virtual, por ejemplo: **ShieldVM1**
    
    Para &lt;VHDPath&gt;, especifique una ubicación para almacenar el VHDX de la máquina virtual, por ejemplo: **C:\\VMs\\ShieldVM1\\ShieldVM1.vhdx**
    
    Para &lt;nnGB&gt;, especifique un tamaño para VHDX, por ejemplo: **60GB**

        New-VM -Generation 2 -Name "<ShieldedVMname>" -NewVHDPath <VHDPath>.vhdx -NewVHDSizeBytes <nnGB>

2.  Instalar un sistema operativo compatible (cliente de Windows Server 2012 o posterior, Windows 8 o superior) en la máquina virtual y habilitar la conexión a Escritorio remoto y la regla de firewall correspondiente. Registrar la dirección IP o nombre DNS; de la máquina virtual la necesitará para conectarse remotamente a él.

3.  Use RDP para conectarse a la máquina virtual de forma remota y comprobar que RDP y el firewall están configurados correctamente. Como parte del proceso de blindaje, se deshabilitarán acceso mediante consola a la máquina virtual mediante Hyper-V, por lo que es importante asegurarse de que puede administrar el sistema de forma remota a través de la red.

4.  Para crear un nuevo Protector de clave (descrito al principio de esta sección), ejecute el siguiente comando.

    Para &lt;GuardianName&gt;, utilice el nombre especificado en el procedimiento anterior, por ejemplo: **HostingProvider1**

    Incluir **- AllowUntrustedRoot** para permitir que los certificados autofirmados.

        $Guardian = Get-HgsGuardian -Name '<GuardianName>'

        $Owner = New-HgsGuardian -Name 'Owner' -GenerateCertificates

        $KP = New-HgsKeyProtector -Owner $Owner -Guardian $Guardian -AllowUntrustedRoot

    Si desea más de un centro de datos poder ejecutar la máquina virtual blindada (por ejemplo, un sitio de recuperación ante desastres y un proveedor de nube pública), puede proporcionar una lista de protecciones para el **-Guardian** parámetro. Para obtener más información, consulte [New-HgsKeyProtector] (https://docs.microsoft.com/powershell/module/hgsclient/new-hgskeyprotector?view=win10-ps.

5.  Para habilitar el vTPM usando el Protector de clave, ejecute el siguiente comando. Para &lt;ShieldedVMname&gt;, use el mismo nombre de máquina virtual usado en pasos anteriores.

        $VMName="<ShieldedVMname>"

        Stop-VM -Name $VMName -Force

        Set-VMKeyProtector -VMName $VMName -KeyProtector $KP.RawData

        Set-VMSecurityPolicy -VMName $VMName -Shielded $true

        Enable-VMTPM -VMName $VMName

6.  Para iniciar la máquina virtual para comprobar que el protector de clave es trabajar con certificados de propietario local, ejecute el siguiente comando.

        Start-VM -Name $VMName

7.  Compruebe que la máquina virtual se ha iniciado en la consola de Hyper-V.

8.  Use RDP para conectarse a la máquina virtual de forma remota y habilitar BitLocker en todas las particiones de todos los discos duros virtuales que están conectados a la máquina virtual blindada.

    > [!IMPORTANT]
    > Antes de continuar con el paso siguiente, espere a que finalice en todas las particiones donde haya habilitado el cifrado de BitLocker.

9.  Apague la máquina virtual cuando esté listo para moverla al tejido protegido.

10.  En el servidor de Hyper-V de inquilino, exporte la máquina virtual mediante la herramienta de su elección (Windows PowerShell o administrador de Hyper-V). A continuación, organizar los archivos que se copiarán en un host protegido mantenido por el proveedor de hospedaje o centro de datos empresarial.

11.  **Para el centro de datos de empresa o de proveedor de hospedaje**:

    Importar la máquina virtual blindada mediante el Administrador de Hyper-V o Windows PowerShell. Debe importar el archivo de configuración de máquina virtual desde el propietario de la máquina virtual con el fin de iniciar la máquina virtual. Esto es porque el protector de clave y TPM virtual de la máquina virtual se almacenan en el archivo de configuración. Si la máquina virtual está configurada para ejecutarse en el tejido protegido, debería poder iniciar correctamente.

## <a name="see-also"></a>Vea también

- [Hospedaje de los pasos de configuración del proveedor de servicio para hosts protegidos y máquinas virtuales blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Las máquinas virtuales blindadas y tejido protegido](guarded-fabric-and-shielded-vms-top-node.md)
