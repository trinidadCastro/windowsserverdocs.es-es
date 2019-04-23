---
title: Solución de problemas del servicio guardián de Host
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 80ea38f4-4de6-4f85-8188-33a63bb1cf81
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 7fe01039b47c36d940973fba97d25c401f5af8a7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851376"
---
# <a name="troubleshooting-guarded-hosts"></a>Solución de problemas de los Hosts protegidos

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

En este tema se describe las soluciones a problemas habituales detectados al implementar ni el funcionamiento de un host de Hyper-V protegido en el tejido protegido.
Si no está seguro de la naturaleza del problema, primero intente ejecutar el [protegidos de los diagnósticos de fabric](guarded-fabric-troubleshoot-diagnostics.md) hace que en los hosts de Hyper-V para reducir el potencial.

## <a name="guarded-host-feature"></a>Protegerse de la característica de Host

Si experimenta problemas con el host de Hyper-V, primero asegúrese de que el **con Hyper-V de protección de Host** se instala la característica.
Sin esta característica, el host de Hyper-V le faltará algunos valores de configuración crítica y software que le permiten pasar la atestación y aprovisionar máquinas virtuales blindadas.

Para comprobar si está instalada la característica, use el administrador del servidor o ejecute el siguiente comando en una ventana de PowerShell con privilegios elevados:

```powershell
Get-WindowsFeature HostGuardian
```

Si la característica no está instalada, instálelo con el siguiente comando de PowerShell:

```powershell
Install-WindowsFeature HostGuardian -Restart
```

## <a name="attestation-failures"></a>Errores de atestación

Si un host no pasar la atestación con el servicio de protección de Host, no podrá ejecutar máquinas virtuales blindadas.
La salida de [Get-HgsClientConfiguration](https://technet.microsoft.com/library/dn914500.aspx) en ese host le mostrará información sobre el motivo del error de atestación de ese host.

En la tabla siguiente se explica los valores que pueden aparecer en el **AttestationStatus** campo y posibles pasos siguientes, si procede.

AttestationStatus         | Explicación
--------------------------|------------
Expirado                   | El host pasado la atestación anteriormente, pero se ha emitido el certificado de mantenimiento ha expirado. Asegúrese de que el host y el tiempo HGS estén sincronizados.
InsecureHostConfiguration | El host no superó la atestación porque no cumplía con las directivas de atestación configurado en HGS. Para obtener más información, consulte la tabla AttestationSubStatus.
NotConfigured             | El host no está configurado para usar un HGS para atestación y protección de claves. Se configura para el modo local, en su lugar. Si este host se encuentra en un tejido protegido, utilice [Set-HgsClientConfiguration](https://technet.microsoft.com/library/dn914494.aspx) que proporcionarla con las direcciones URL para el servidor HGS.
Pasado                    | El host pasado la atestación.
TransientError            | Error del último intento de atestación debido a una red, servicio u otro error temporal. Vuelva a intentar la última operación.
TpmError                  | El host no pudo completar el último intento de atestación debido a un error con el TPM. Para obtener más información, consulte los registros de TPM.
UnauthorizedHost          | El host no superó la atestación porque no se ha autorizado para ejecutar máquinas virtuales blindadas. Asegúrese de que el host pertenezca a un grupo de seguridad de confianza HGS para ejecutar máquinas virtuales blindadas.
Unknown                   | El host no se ha intentado dar fe con HGS todavía.


Cuando **AttestationStatus** se notifica como **InsecureHostConfiguration**, uno o varios motivos se rellenarán en la **AttestationSubStatus** campo.
En la tabla siguiente se explica los valores posibles para AttestationSubStatus y sugerencias sobre cómo resolver el problema.

AttestationSubStatus       | Lo que significa y qué hacer
---------------------------|-------------------------------
BitLocker                  | Volumen del sistema operativo del host no está cifrada por BitLocker. Para resolver este problema, [habilitar BitLocker](https://technet.microsoft.com/itpro/windows/keep-secure/bitlocker-basic-deployment) en el volumen del sistema operativo o [deshabilitar la directiva de BitLocker en HGS](guarded-fabric-manage-hgs.md#review-attestation-policies).
CodeIntegrityPolicy        | El host no está configurado para usar una directiva de integridad de código o no usa una directiva de confianza para el servidor HGS. Asegúrese de una directiva de integridad de código se ha configurado, que se ha reiniciado el host y la directiva está registrada con el servidor HGS. Para obtener más información, consulte [crear y aplicar una directiva de integridad de código](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#create-and-apply-a-code-integrity-policy).
DumpsEnabled               | El host está configurado para permitir que los volcados de memoria o en vivo los volcados de memoria, lo cual no está permitido por las directivas HGS. Para resolver este problema, deshabilite los volcados de memoria en el host.
DumpEncryption             | El host está configurado para permitir que los volcados de memoria o memoria en vivo volcados de memoria, pero no cifra los volcados de memoria. Deshabilítelo volcados de memoria en el host o [configurar el cifrado de volcado de memoria](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption).
DumpEncryptionKey          | El host está configurado para permitir y cifrar los volcados de memoria, pero no usa un certificado que se sabe que HGS para cifrarlos. Para resolver este problema, [actualizar la clave de cifrado de volcado de memoria](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption) en el host o [registrar la clave con HGS](guarded-fabric-manage-hgs.md#authorizing-new-guarded-hosts).
FullBoot                   | El host reanuda desde un estado de suspensión o hibernación. Reinicie el host para permitir un arranque limpio y completo.
HibernationEnabled         | El host está configurado para permitir la hibernación sin cifrar el archivo de hibernación, lo cual no está permitido por las directivas HGS. Deshabilite la hibernación y reinicie el host, o [configurar el cifrado de volcado de memoria](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption).
HypervisorEnforcedCodeIntegrityPolicy | El host no está configurado para usar una directiva de integridad de código aplicada por hipervisor. Compruebe que la integridad de código está habilitada, configurada y aplicada por el hipervisor. Consulte la [Guía de implementación de Device Guard](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-deploy-code-integrity-policies) para obtener más información.
Iommu                      | Características de seguridad basada en la virtualización del host no están configuradas para requerir un dispositivo IOMMU para protección contra los ataques de acceso directo a memoria, según sea necesario por las directivas HGS. Compruebe que el host tiene un IOMMU, que está habilitado y que Device Guard es [configurado para requerir que las protecciones de DMA](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-enable-virtualization-based-security#enable-virtualization-based-security-vbs-and-device-guard) al iniciar VBS.
PagefileEncryption         | Cifrado de archivos de página no está habilitado en el host. Para resolver este problema, ejecute `fsutil behavior set encryptpagingfile 1` para habilitar el cifrado del archivo de página. Para obtener más información, consulte [comportamiento de fsutil](https://technet.microsoft.com/library/cc785435.aspx).
SecureBoot                 | Arranque seguro está usando ya no está habilitado en este host o no sea la plantilla de arranque seguro de Microsoft. [Habilite el arranque seguro](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/disabling-secure-boot#enable_secure_boot) con la plantilla de arranque seguro de Microsoft para resolver este problema.
SecureBootSettings         | La línea de base TPM en este host no coincide con cualquiera de esos HGS de confianza. Esto puede ocurrir cuando se cambian su UEFI autoridades de lanzamiento, DBX variable, indicador de depuración o las directivas personalizadas de arranque seguro mediante la instalación de nuevo hardware o software. Si confía en el hardware actual, el firmware y la configuración de software de esta máquina, puede [capturar una nueva línea de base TPM](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#capture-the-tpm-baseline-for-each-unique-class-of-hardware) y [registrarlo con HGS](guarded-fabric-manage-hgs.md#authorizing-new-guarded-hosts).
TcgLogVerification         | No se obtenido o comprobar el registro TCG (línea de base TPM). Esto puede indicar un problema con el firmware del host, el TPM u otros componentes de hardware. Si el host está configurado para tratar de arranque PXE antes de arrancar Windows, un programa de arranque neto obsoletos (NBP) también pueden producir este error. Asegúrese de que todos los NBP está actualizados cuando está habilitado el arranque PXE.
VirtualSecureMode          | Características de seguridad basada en virtualización no se está ejecutando en el host. Asegúrese VBS está habilitado y que su sistema cumple configurado [características de seguridad de la plataforma](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-enable-virtualization-based-security#validate-enabled-device-guard-hardware-based-security-features). Consulte la [documentación de Device Guard](https://technet.microsoft.com/itpro/windows/keep-secure/device-guard-deployment-guide) para obtener más información acerca de los requisitos de VBS.
