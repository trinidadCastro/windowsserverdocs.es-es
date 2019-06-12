---
title: Confirme que los hosts protegidos pueden dar fe
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 7485796b-b840-4678-9b33-89e9710fbbc7
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 02/05/2019
ms.openlocfilehash: 87878eba785c0e1cc50454a74b2af4a159e88e12
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443662"
---
# <a name="confirm-guarded-hosts-can-attest"></a>Confirme que los hosts protegidos pueden dar fe 

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016


Un administrador del tejido debe confirmar que los hosts de Hyper-V pueden ejecutarse como hosts protegidos. Complete los pasos siguientes en al menos un host protegido:

1.  Si no ha instalado ya el rol Hyper-V y la característica de compatibilidad de Hyper-V de guardián de Host, instalarlos con el siguiente comando:

        Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart

2.  Asegúrese de que el host de Hyper-V puede resolver el nombre DNS de HGS y tiene conectividad de red con acceso al puerto 80 (o 443 si configurar HTTPS) en el servidor HGS.

2.  Configurar el host de la protección de clave y direcciones URL de atestación:

    - **A través de Windows PowerShell**: Puede configurar la protección de clave y direcciones URL de atestación ejecutando el siguiente comando en una consola de Windows PowerShell con privilegios elevados. Para &lt;FQDN&gt;, utilice el nombre de dominio completo (FQDN) del clúster HGS (por ejemplo, hgs.bastion.local, o pida al administrador HGS para ejecutar el **Get HgsServer** cmdlet en el servidor HGS para recuperar el Direcciones URL).

        `Set-HgsClientConfiguration -AttestationServerUrl 'http://<FQDN>/Attestation' -KeyProtectionServerUrl 'http://<FQDN>/KeyProtection'`

        Para configurar un servidor HGS de reserva, repita este comando y especifique las direcciones URL de reserva para los servicios de atestación y protección de clave. Para obtener más información, consulte [configuración de reserva](guarded-fabric-manage-branch-office.md#fallback-configuration). 

    - **A través de VMM**: Si usa System Center 2016 - Virtual Machine Manager (VMM), puede configurar la atestación y protección de clave de las direcciones URL en VMM. Para obtener más información, consulte [configuración global de HGS](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-hosts#configure-global-hgs-settings) en **aprovisionar hosts protegidos en VMM**.
    
    >**Notas de la**
    > - Si el Administrador de HGS [habilitado HTTPS en el servidor HGS](guarded-fabric-configure-hgs-https.md), empezar a las direcciones URL con `https://`.
    > - Si el administrador HGS había habilitado HTTPS en el servidor HGS y usa un certificado autofirmado, deberá importar el certificado en el almacén entidades emisoras de certificados raíz de confianza en todos los hosts. Para ello, ejecute el siguiente comando en cada host:<br>
        `Import-Certificate -FilePath "C:\temp\HttpsCertificate.cer" -CertStoreLocation Cert:\LocalMachine\Root`
    
3.  Para iniciar un intento de atestación en el host y ver el estado de atestación, ejecute el siguiente comando:

        Get-HgsClientConfiguration

    La salida del comando indica si el host pasado la atestación y ahora está protegido. Si `IsHostGuarded` no devuelve **True**, puede ejecutar la herramienta de diagnóstico HGS [Get HgsTrace](https://technet.microsoft.com/library/mt718831.aspx), debe investigar. Para ejecutar diagnósticos, escriba el siguiente comando en un símbolo del sistema de Windows PowerShell con privilegios elevados en el host:

        Get-HgsTrace -RunDiagnostics -Detailed

    > [!IMPORTANT]
    > Si usa Windows Server 2019 o Windows 10, versión 1809 y está usando directivas de integridad de código, `Get-HgsTrace` devolver un error para el **directiva de integridad de código activo** diagnóstico.
    > Puede ignorar este resultado cuando es el solo errores diagnóstico.

## <a name="next-step"></a>Paso siguiente

> [!div class="nextstepaction"]
> [Implementar máquinas virtuales blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)

## <a name="see-also"></a>Vea también

- [Implementar el servicio de protección de Host (HGS)](guarded-fabric-deploying-hgs-overview.md)
- [Implementar máquinas virtuales blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)

