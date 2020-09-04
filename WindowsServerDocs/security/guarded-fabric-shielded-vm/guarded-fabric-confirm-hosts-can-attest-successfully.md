---
title: Confirmar que los hosts protegidos pueden atestiguar
ms.topic: article
ms.assetid: 7485796b-b840-4678-9b33-89e9710fbbc7
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 09/25/2019
ms.openlocfilehash: cda60c65772a41322a20b7277a7d7c80a4daf9e1
ms.sourcegitcommit: a640c2d7f2d21d7cd10a9be4496e1574e5e955f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2020
ms.locfileid: "89446778"
---
# <a name="confirm-guarded-hosts-can-attest"></a>Confirmar que los hosts protegidos pueden atestiguar

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

Un administrador de tejido debe confirmar que los hosts de Hyper-V pueden ejecutarse como hosts protegidos. Complete los pasos siguientes en al menos un host protegido:

1. Si aún no ha instalado el rol de Hyper-V y la característica de compatibilidad de Hyper-V con protección de host, instálelos con el siguiente comando:

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ```

2. Asegúrese de que el host de Hyper-V pueda resolver el nombre DNS del HGS y que tenga conectividad de red para llegar al puerto 80 (o 443 si configura HTTPS) en el servidor de HGS.

3. Configure las direcciones URL de atestación y protección de claves del host:

    - **A través de Windows PowerShell**: puede configurar la protección de claves y las direcciones URL de atestación ejecutando el siguiente comando en una consola de Windows PowerShell con privilegios elevados. En &lt; FQDN &gt; , use el nombre de dominio completo (FQDN) del clúster de HGS (por ejemplo, HGS. bastión. local o pida al administrador de HGS que ejecute el cmdlet **Get-HgsServer** en el servidor de HGS para recuperar las direcciones URL).

        ```PowerShell
        Set-HgsClientConfiguration -AttestationServerUrl 'http://<FQDN>/Attestation' -KeyProtectionServerUrl 'http://<FQDN>/KeyProtection'
         ```

        Para configurar un servidor HGS de reserva, repita este comando y especifique las direcciones URL de reserva para los servicios de protección de claves y atestación. Para obtener más información, consulte [configuración de reserva](guarded-fabric-manage-branch-office.md#fallback-configuration).

    - **A través de VMM**: si usa System Center Virtual Machine Manager (VMM), puede configurar las direcciones URL de atestación y protección de claves en VMM. Para obtener más información, consulte [configuración global de HGS](/system-center/vmm/guarded-deploy-host?view=sc-vmm-2019#configure-global-hgs-settings) en **aprovisionamiento de hosts protegidos en VMM**.

    >**Notas**
    > - Si el administrador de HGS ha [habilitado HTTPS en el servidor de HGS](guarded-fabric-configure-hgs-https.md), inicie las direcciones URL con `https://` .
    > - Si el administrador de HGS ha habilitado HTTPS en el servidor de HGS y ha usado un certificado autofirmado, tendrá que importar el certificado en el almacén de entidades de certificación raíz de confianza en todos los hosts. Para ello, ejecute el siguiente comando en cada host:
       ```PowerShell
       Import-Certificate -FilePath "C:\temp\HttpsCertificate.cer" -CertStoreLocation Cert:\LocalMachine\Root
       ```
    > - Si ha configurado el cliente de HGS para usar HTTPS y ha deshabilitado TLS 1,0 en todo el sistema, consulte nuestra [Guía de TLS moderna](guarded-fabric-troubleshoot-hosts.md#modern-tls) .

4. Para iniciar un intento de atestación en el host y ver el estado de la atestación, ejecute el siguiente comando:

    ```powershell
    Get-HgsClientConfiguration
    ```

    La salida del comando indica si el host ha pasado la atestación y ahora está protegido. Si no `IsHostGuarded` devuelve **true**, puede ejecutar la herramienta de diagnóstico de HGS, [Get-HgsTrace](https://technet.microsoft.com/library/mt718831.aspx), para investigar. Para ejecutar diagnósticos, escriba el siguiente comando en un símbolo del sistema de Windows PowerShell con privilegios elevados en el host:

    ```powershell
    Get-HgsTrace -RunDiagnostics -Detailed
    ```

    > [!IMPORTANT]
    > Si usa Windows Server 2019 o Windows 10, versión 1809 o posterior, y usa directivas de integridad de código, `Get-HgsTrace` devuelva un error para el diagnóstico activo de la **Directiva de integridad de código** .
    > Puede omitir este resultado de forma segura cuando sea el único diagnóstico con errores.

## <a name="next-step"></a>Paso siguiente

> [!div class="nextstepaction"]
> [Implementar máquinas virtuales blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)

## <a name="additional-references"></a>Referencias adicionales

- [Implementación del Servicio de protección de host (HGS)](guarded-fabric-deploying-hgs-overview.md)
- [Implementar máquinas virtuales blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
