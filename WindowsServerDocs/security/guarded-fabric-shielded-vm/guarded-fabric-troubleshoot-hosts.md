---
title: Solución de problemas del servicio de protección de host
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 80ea38f4-4de6-4f85-8188-33a63bb1cf81
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: e2685e33a215d0c5f97fe414b7458371930e862b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386358"
---
# <a name="troubleshooting-guarded-hosts"></a>Solución de problemas de hosts protegidos

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

En este tema se describen las soluciones a problemas comunes que se producen al implementar o operar un host de Hyper-V protegido en el tejido protegido.
Si no está seguro de la naturaleza del problema, primero intente ejecutar el diagnóstico de [tejido protegido](guarded-fabric-troubleshoot-diagnostics.md) en los hosts de Hyper-V para reducir las posibles causas.

## <a name="guarded-host-feature"></a>Característica de host protegido

Si tiene problemas con el host de Hyper-V, asegúrese primero de que la característica de **compatibilidad con Hyper-v de protección de host** está instalada.
Sin esta característica, el host de Hyper-V no tendrá algunos valores de configuración y software críticos que le permitan pasar la atestación y aprovisionar máquinas virtuales blindadas.

Para comprobar si está instalada la característica, use Administrador del servidor o ejecute el siguiente comando en una ventana de PowerShell con privilegios elevados:

```powershell
Get-WindowsFeature HostGuardian
```

Si la característica no está instalada, instálela con el siguiente comando de PowerShell:

```powershell
Install-WindowsFeature HostGuardian -Restart
```

## <a name="attestation-failures"></a>Errores de atestación

Si un host no pasa la atestación con el servicio de protección de host, no podrá ejecutar máquinas virtuales blindadas.
La salida de [Get-HgsClientConfiguration](https://technet.microsoft.com/library/dn914500.aspx) en ese host mostrará información acerca de por qué se produjo un error en la atestación de ese host.

En la tabla siguiente se explican los valores que pueden aparecer en el campo **AttestationStatus** y los posibles pasos siguientes, si es necesario.

AttestationStatus         | Explicación
--------------------------|------------
Expirado                   | El host ha pasado la atestación anteriormente, pero el certificado de mantenimiento que se emitió ha expirado. Asegúrese de que el host y el tiempo del HGS están sincronizados.
InsecureHostConfiguration | El host no pasó la atestación porque no cumplía con las directivas de atestación configuradas en HGS. Consulte la tabla AttestationSubStatus para obtener más información.
NotConfigured             | El host no está configurado para usar un HGS para la atestación y protección de claves. En su lugar, se configura para el modo local. Si este host se encuentra en un tejido protegido, use [set-HgsClientConfiguration](https://technet.microsoft.com/library/dn914494.aspx) para proporcionarle las direcciones URL del servidor de HGS.
Ha                    | El host ha pasado la atestación.
TransientError            | No se pudo realizar el último intento de atestación debido a un error de red, un servicio u otro error temporal. Vuelva a intentar la última operación.
TpmError                  | El host no pudo completar su último intento de atestación debido a un error con el TPM. Consulte los registros de TPM para obtener más información.
UnauthorizedHost          | El host no pasó la atestación porque no estaba autorizado para ejecutar máquinas virtuales blindadas. Asegúrese de que el host pertenece a un grupo de seguridad de confianza de HGS para ejecutar máquinas virtuales blindadas.
Unknown                   | El host todavía no ha intentado atestar con HGS.


Cuando **AttestationStatus** se notifique como **InsecureHostConfiguration**, se rellenarán una o varias razones en el campo **AttestationSubStatus** .
En la tabla siguiente se explican los posibles valores de AttestationSubStatus y sugerencias sobre cómo resolver el problema.

AttestationSubStatus       | Qué significa y qué hacer
---------------------------|-------------------------------
BitLocker                  | BitLocker no cifra el volumen del sistema operativo del host. Para resolverlo, [habilite BitLocker](https://technet.microsoft.com/itpro/windows/keep-secure/bitlocker-basic-deployment) en el volumen del sistema operativo o [deshabilite la Directiva de BitLocker en HGS](guarded-fabric-manage-hgs.md#review-attestation-policies).
CodeIntegrityPolicy        | El host no está configurado para usar una directiva de integridad de código o no usa una directiva de confianza para el servidor de HGS. Asegúrese de que se ha configurado una directiva de integridad de código, que se ha reiniciado el host y que la Directiva se ha registrado con el servidor HGS. Para obtener más información, vea [crear y aplicar una directiva de integridad de código](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#create-and-apply-a-code-integrity-policy).
DumpsEnabled               | El host está configurado para permitir volcados de memoria o volcados de memoria en directo, que no se permiten en las directivas de HGS. Para resolverlo, deshabilite los volcados en el host.
DumpEncryption             | El host está configurado para permitir volcados de memoria o volcados de memoria en directo, pero no los cifra. Deshabilite los volcados en el host o [Configure el cifrado de volcado](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption).
DumpEncryptionKey          | El host está configurado para permitir y cifrar volcados, pero no está usando un certificado conocido para HGS para cifrarlos. Para resolver esto, [actualice la clave de cifrado de volcado](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption) en el host o [Registre la clave con HGS](guarded-fabric-manage-hgs.md#authorizing-new-guarded-hosts).
FullBoot                   | El host reanudó desde un estado de suspensión o hibernación. Reinicie el host para permitir un arranque completo y limpio.
HibernationEnabled         | El host está configurado para permitir la hibernación sin cifrar el archivo de hibernación, lo que no se permite en las directivas de HGS. Deshabilite la hibernación y reinicie el host, o [Configure el cifrado de volcado](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption).
HypervisorEnforcedCodeIntegrityPolicy | El host no está configurado para usar una directiva de integridad de código forzada por hipervisor. Compruebe que la integridad del código está habilitada, configurada y aplicada por el hipervisor. Para obtener más información, consulte la [Guía de implementación de Device Guard](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-deploy-code-integrity-policies) .
IOMMU                      | Las características de seguridad basadas en la virtualización del host no están configuradas para requerir un dispositivo IOMMU para la protección frente a ataques de acceso directo a memoria, según las directivas de HGS. Compruebe que el host tiene un IOMMU, que está habilitado y que Device Guard está [configurado para requerir protecciones DMA](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-enable-virtualization-based-security#enable-virtualization-based-security-vbs-and-device-guard) al iniciar vbs.
PagefileEncryption         | El cifrado del archivo de paginación no está habilitado en el host. Para resolver este paso, ejecute `fsutil behavior set encryptpagingfile 1` para habilitar el cifrado del archivo de paginación. Para obtener más información, vea [fsutil Behavior](https://technet.microsoft.com/library/cc785435.aspx).
SecureBoot                 | El arranque seguro no está habilitado en este host o no utiliza la plantilla de arranque seguro de Microsoft. [Habilite el arranque seguro](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/disabling-secure-boot#enable_secure_boot) con la plantilla de arranque seguro de Microsoft para resolver este problema.
SecureBootSettings         | La línea de base de TPM de este host no coincide con ninguno de los que es de confianza para HGS. Esto puede ocurrir cuando las entidades de inicio de UEFI, la variable DBX, la marca de depuración o las directivas personalizadas de arranque seguro se cambian mediante la instalación de nuevo hardware o software. Si confía en la configuración de hardware, firmware y software actual de este equipo, puede [capturar una nueva línea base de TPM](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#capture-the-tpm-baseline-for-each-unique-class-of-hardware) y [registrarla con HGS](guarded-fabric-manage-hgs.md#authorizing-new-guarded-hosts).
TcgLogVerification         | No se puede obtener o comprobar el registro de TCG (línea base de TPM). Esto puede indicar un problema con el firmware del host, el TPM u otros componentes de hardware. Si el host está configurado para intentar el arranque de PXE antes del arranque de Windows, un programa de arranque de red (NBP) anticuado también puede producir este error. Asegúrese de que todos los NBP están actualizados cuando está habilitado el arranque de PXE.
VirtualSecureMode          | Las características de seguridad basadas en la virtualización no se ejecutan en el host. Asegúrese de que VBS está habilitado y de que el sistema cumple las [características de seguridad](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-enable-virtualization-based-security#validate-enabled-device-guard-hardware-based-security-features)de la plataforma configuradas. Consulte la [documentación de Device Guard](https://technet.microsoft.com/itpro/windows/keep-secure/device-guard-deployment-guide) para obtener más información sobre los requisitos de vbs.
