---
redirect_url: guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md
title: 'Máquinas virtuales blindadas para inquilinos: creación de una nueva máquina virtual blindada en el entorno local y su traslado a un tejido protegido'
ms.prod: windows-server
ms.topic: article
ms.assetid: 0ca1efa0-01f9-4b6f-87d4-c66db00d7d70
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: a5ca3ab29b83d0cb6cb2d55507471790f65800a2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856728"
---
# <a name="shielded-vms-for-tenants---creating-a-new-shielded-vm-on-premises-and-moving-it-to-a-guarded-fabric"></a>Máquinas virtuales blindadas para inquilinos: creación de una nueva máquina virtual blindada en el entorno local y su traslado a un tejido protegido

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

En este tema se describen los pasos para crear una máquina virtual blindada solo con Hyper-V; es decir, sin Virtual Machine Manager, discos de plantilla o un archivo de datos de blindaje. Este es un escenario poco común para la mayoría de los entornos de hospedaje en la nube pública, pero puede ser útil cuando se prueba un tejido protegido o en escenarios empresariales donde se mueve una máquina virtual de un tejido de departamento a una infraestructura de ti compartida y se debe cifrar antes de la migración.

Para entender cómo se ajusta este tema en el proceso general de implementación de máquinas virtuales blindadas, consulte [los pasos de configuración de proveedor de servicio de hospedaje para hosts protegidos y máquinas virtuales blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md).

## <a name="import-the-guardian-configuration-on-the-tenant-hyper-v-server"></a>Importación de la configuración de protección en el servidor de Hyper-V de inquilino

1.  Antes de comenzar el procedimiento, asegúrese de que se encuentre en un host de Hyper-V que ejecute Windows Server 2016 con los siguientes roles y características instalados:

    - Role

        - Hyper-V

    - Características

        - Herramientas de administración de características de Herramientas de administración remota del servidor\\\\herramientas de máquinas virtuales blindadas

    > [!NOTE]
    > El host que se usa aquí *no* debe ser un host en el tejido protegido. Se trata de un host independiente en el que se preparan las máquinas virtuales antes de moverlas al tejido protegido.

2.  Antes de poder ejecutar una máquina virtual blindada en este equipo, debe asegurarse de que la seguridad basada en la virtualización esté habilitada y en ejecución. Ejecute el siguiente comando en una ventana de Windows PowerShell con privilegios elevados para instalar la característica de compatibilidad de Hyper-V de protección de host. Esto configurará todas las opciones de configuración necesarias en el equipo para poder ejecutar máquinas virtuales blindadas.

        Install-WindowsFeature HostGuardian

3.  Tendrá que adquirir los metadatos de protección para el tejido protegido en el que se ejecutará la máquina virtual. Estos metadatos se usan para autorizar a ese tejido a ejecutar la máquina virtual blindada. La forma de obtener esta información será diferente para cada proveedor de servicios de hosting o empresa. El anfitrión (o usted, si tiene acceso a la red de tejido protegido) puede adquirir esta información ejecutando el siguiente comando de Windows PowerShell:

        Invoke-WebRequest 'http://hgs.bastion.local/KeyProtection/service/metadata/2014-07/metadata.xml' -OutFile .\RelecloudGuardian.xml

    En el ejemplo anterior, "HGS" es el nombre de red distribuida del clúster HGS y "bastión. local" es el nombre del dominio HGS.

4.  Para importar la clave de protección, que necesitará en un procedimiento posterior, ejecute el siguiente comando.

    Por &lt;ruta de acceso&gt;&lt;nombre de archivo&gt;, sustituya la ruta de acceso y el nombre del archivo XML que guardó en el paso anterior, por ejemplo: **C:\\temp\\GuardianKey. XML**

    En &lt;&gt;GuardianName, especifique un nombre para el proveedor de hospedaje o el centro de recursos de empresa, por ejemplo, **HostingProvider1**. Registre el nombre para el procedimiento siguiente.

    Incluya **-AllowUntrustedRoot** solo si el servidor HGS se configuró con certificados autofirmados. (Estos certificados forman parte del servicio de protección de claves en HGS).

        Import-HgsGuardian -Path '<Path><Filename>' -Name '<GuardianName>' -AllowUntrustedRoot

## <a name="create-a-new-shielded-virtual-machine-on-the-host"></a>Crear una nueva máquina virtual blindada en el host

En este procedimiento, creará una máquina virtual en el host de Hyper-V y la preparará para su exportación al proveedor de hospedaje o al administrador del centro de usuarios, que puede ejecutarla en un host protegido.

Como parte del procedimiento, creará un protector de clave que contiene dos elementos importantes:

-   **Propietario**: en el protector de clave, es probable que el grupo con el que trabaja, que comparte elementos de seguridad, como los certificados, se identifique como "propietario" de la máquina virtual. Su identidad como propietario está representada por un certificado que, si ejecuta los comandos tal como se muestra, se genera como un certificado autofirmado. Opcionalmente, puede usar un certificado respaldado por la infraestructura de PKI y omitir el parámetro **-AllowUntrustedRoot** en los comandos.

-   **Protecciones**: Además, en el protector de clave, el proveedor de hospedaje o el centro de recursos de la empresa (que ejecuta el HGS y los hosts protegidos) se identifican como "tutor". La protección está representada por la clave de protección que importó en el procedimiento anterior, [importe la configuración de protección en el servidor de Hyper-V de inquilino](#import-the-guardian-configuration-on-the-tenant-hyper-v-server).

Para ver una ilustración que muestra el protector de clave, que es un elemento de un archivo de datos de blindaje, vea [¿Qué son los datos de blindaje y por qué es necesario?](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary).

1. En un host de Hyper-V de inquilino, para crear una nueva máquina virtual de generación 2, ejecute el siguiente comando.

   En &lt;&gt;ShieldedVMname, especifique un nombre para la máquina virtual, por ejemplo: **ShieldVM1**
    
   En &lt;&gt;VHDPath, especifique una ubicación para almacenar el VHDX de la máquina virtual, por ejemplo: **C:\\vm\\ShieldVM1\\ShieldVM1. VHDX.**
    
   En &lt;&gt;nnGB, especifique un tamaño para el VHDX, por ejemplo: **60 GB**

       New-VM -Generation 2 -Name "<ShieldedVMname>" -NewVHDPath <VHDPath>.vhdx -NewVHDSizeBytes <nnGB>

2. Instale un sistema operativo compatible (Windows Server 2012 o posterior, cliente de Windows 8 o posterior) en la máquina virtual y habilite la conexión a escritorio remoto y la regla de Firewall correspondiente. Grabe la dirección IP o el nombre DNS de la máquina virtual; lo necesitará para conectarse de forma remota a él.

3. Use RDP para conectarse de forma remota a la máquina virtual y comprobar que RDP y el Firewall están configurados correctamente. Como parte del proceso de blindaje, se deshabilitará el acceso de la consola a la máquina virtual a través de Hyper-V, por lo que es importante asegurarse de que puede administrar el sistema de forma remota a través de la red.

4. Para crear un nuevo protector de clave (que se describe al principio de esta sección), ejecute el siguiente comando.

   En &lt;&gt;GuardianName, use el nombre que especificó en el procedimiento anterior, por ejemplo: **HostingProvider1**

   Incluya **-AllowUntrustedRoot** para permitir certificados autofirmados.

       $Guardian = Get-HgsGuardian -Name '<GuardianName>'

       $Owner = New-HgsGuardian -Name 'Owner' -GenerateCertificates

       $KP = New-HgsKeyProtector -Owner $Owner -Guardian $Guardian -AllowUntrustedRoot

   Si desea que más de un centro de información pueda ejecutar la máquina virtual blindada (por ejemplo, un sitio de recuperación ante desastres y un proveedor de nube pública), puede proporcionar una lista de protecciones al parámetro **-Guardian** . Para obtener más información, vea [New-HgsKeyProtector] (https://docs.microsoft.com/powershell/module/hgsclient/new-hgskeyprotector?view=win10-ps.

5. Para habilitar vTPM mediante el protector de clave, ejecute el siguiente comando. En &lt;&gt;ShieldedVMname, use el mismo nombre de máquina virtual usado en los pasos anteriores.

       $VMName="<ShieldedVMname>"

       Stop-VM -Name $VMName -Force

       Set-VMKeyProtector -VMName $VMName -KeyProtector $KP.RawData

       Set-VMSecurityPolicy -VMName $VMName -Shielded $true

       Enable-VMTPM -VMName $VMName

6. Para iniciar la máquina virtual con el fin de comprobar que el protector de claves está trabajando con certificados de propietario local, ejecute el siguiente comando.

       Start-VM -Name $VMName

7. Compruebe que la máquina virtual se ha iniciado en la consola de Hyper-V.

8. Use RDP para conectarse de forma remota a la máquina virtual y habilitar BitLocker en todas las particiones de todos los Vhdx que están conectados a la máquina virtual blindada.

   > [!IMPORTANT]
   > Antes de continuar con el paso siguiente, espere a que finalice el cifrado de BitLocker en todas las particiones donde lo habilitó.

9. Apague la máquina virtual cuando esté listo para moverla al tejido protegido.

10. En el servidor de Hyper-V de inquilino, exporte la máquina virtual con la herramienta que prefiera (Administrador de Hyper-V o Windows PowerShell). A continuación, organice los archivos que se van a copiar en un host protegido mantenido por el proveedor de hospedaje o el centro de recursos de empresa.

11. **Para el proveedor de hospedaje o el centro de recursos de empresa**:

    Importe la máquina virtual blindada mediante el administrador de Hyper-V o Windows PowerShell. Debe importar el archivo de configuración de máquina virtual desde el propietario de la máquina virtual para iniciar la máquina virtual. Esto se debe a que el protector de clave y el TPM virtual de la máquina virtual se almacenan en el archivo de configuración. Si la máquina virtual está configurada para ejecutarse en el tejido protegido, debería poder iniciarse correctamente.

## <a name="see-also"></a>Vea también

- [Pasos de configuración del proveedor de servicios de hospedaje para hosts protegidos y máquinas virtuales blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [VM blindadas y tejido protegido](guarded-fabric-and-shielded-vms-top-node.md)
