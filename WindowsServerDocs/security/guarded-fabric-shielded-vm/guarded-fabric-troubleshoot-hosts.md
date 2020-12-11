---
description: Más información acerca de cómo solucionar problemas de hosts protegidos
title: Solución de problemas de hosts protegidos
ms.topic: article
ms.assetid: 80ea38f4-4de6-4f85-8188-33a63bb1cf81
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 09/25/2019
ms.openlocfilehash: 4a9d38e2e702610bb1a6905cdc198d3b4e708d6e
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049253"
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
Expirada                   | El host ha pasado la atestación, pero el certificado de mantenimiento que se emitió ha expirado. Asegúrese de que la hora del host y del HGS están sincronizadas.
InsecureHostConfiguration | El host no pasó la atestación porque no cumplía con las directivas de atestación configuradas en HGS. Consulte la tabla AttestationSubStatus para obtener más información.
NoConfigurado             | El host no está configurado para usar un HGS para la atestación y protección de claves. En su lugar, se configura para el modo local. Si este host se encuentra en un tejido protegido, use [set-HgsClientConfiguration](https://technet.microsoft.com/library/dn914494.aspx) para proporcionarle las direcciones URL del servidor de HGS.
Superado                    | El host ha pasado la atestación.
TransientError            | No se pudo realizar el último intento de atestación debido a un error de red, un servicio u otro error temporal. Vuelva a intentar la última operación.
TpmError                  | El host no pudo completar su último intento de atestación debido a un error con el TPM. Consulte los registros de TPM para obtener más información.
UnauthorizedHost          | El host no pasó la atestación porque no estaba autorizado para ejecutar máquinas virtuales blindadas. Asegúrese de que el host pertenece a un grupo de seguridad de confianza de HGS para ejecutar máquinas virtuales blindadas.
Desconocido                   | El host todavía no ha intentado atestar con HGS.

Cuando **AttestationStatus** se notifique como **InsecureHostConfiguration**, se rellenarán una o varias razones en el campo **AttestationSubStatus** .
En la tabla siguiente se explican los posibles valores de AttestationSubStatus y sugerencias sobre cómo resolver el problema.

AttestationSubStatus       | Qué significa y qué hay que hacer
---------------------------|-------------------------------
BitLocker                  | BitLocker no cifra el volumen del sistema operativo del host. Para resolverlo, [habilite BitLocker](/windows/security/information-protection/bitlocker/bitlocker-basic-deployment) en el volumen del sistema operativo o [deshabilite la Directiva de BitLocker en HGS](guarded-fabric-manage-hgs.md#review-attestation-policies).
CodeIntegrityPolicy        | El host no está configurado para usar una directiva de integridad de código o no usa una directiva de confianza para el servidor de HGS. Asegúrese de que se ha configurado una directiva de integridad de código, que se ha reiniciado el host y que la Directiva se ha registrado con el servidor HGS. Para obtener más información, vea [crear y aplicar una directiva de integridad de código](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#create-and-apply-a-code-integrity-policy).
DumpsEnabled               | El host está configurado para permitir volcados de memoria o volcados de memoria en directo, que no se permiten en las directivas de HGS. Para resolverlo, deshabilite los volcados en el host.
DumpEncryption             | El host está configurado para permitir volcados de memoria o volcados de memoria en directo, pero no los cifra. Deshabilite los volcados en el host o [Configure el cifrado de volcado](../../virtualization/hyper-v/manage/about-dump-encryption.md).
DumpEncryptionKey          | El host está configurado para permitir y cifrar volcados, pero no está usando un certificado conocido para HGS para cifrarlos. Para resolver esto, [actualice la clave de cifrado de volcado](../../virtualization/hyper-v/manage/about-dump-encryption.md) en el host o [Registre la clave con HGS](guarded-fabric-manage-hgs.md#authorizing-new-guarded-hosts).
FullBoot                   | El host reanudó desde un estado de suspensión o hibernación. Reinicie el host para permitir un arranque completo y limpio.
HibernationEnabled         | El host está configurado para permitir la hibernación sin cifrar el archivo de hibernación, lo que no se permite en las directivas de HGS. Deshabilite la hibernación y reinicie el host, o [Configure el cifrado de volcado](../../virtualization/hyper-v/manage/about-dump-encryption.md).
HypervisorEnforcedCodeIntegrityPolicy | El host no está configurado para usar una directiva de integridad de código forzada por hipervisor. Compruebe que la integridad del código está habilitada, configurada y aplicada por el hipervisor. Para obtener más información, consulte la [Guía de implementación de Device Guard](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-deploy-code-integrity-policies) .
Iommu                      | Las características de seguridad basadas en la virtualización del host no están configuradas para requerir un dispositivo IOMMU para la protección frente a ataques de acceso directo a memoria, según las directivas de HGS. Compruebe que el host tiene un IOMMU, que está habilitado y que Device Guard está [configurado para requerir protecciones DMA](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-enable-virtualization-based-security#enable-virtualization-based-security-vbs-and-device-guard) al iniciar vbs.
PagefileEncryption         | El cifrado del archivo de paginación no está habilitado en el host. Para resolverlo, ejecute `fsutil behavior set encryptpagingfile 1` para habilitar el cifrado del archivo de paginación. Para obtener más información, vea [fsutil Behavior](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc785435(v=ws.11)).
SecureBoot                 | El arranque seguro no está habilitado en este host o no utiliza la plantilla de arranque seguro de Microsoft. [Habilite el arranque seguro](/windows-hardware/manufacture/desktop/disabling-secure-boot#enable_secure_boot) con la plantilla de arranque seguro de Microsoft para resolver este problema.
SecureBootSettings         | La línea de base de TPM de este host no coincide con ninguno de los que es de confianza para HGS. Esto puede ocurrir cuando las entidades de inicio de UEFI, la variable DBX, la marca de depuración o las directivas personalizadas de arranque seguro se cambian mediante la instalación de nuevo hardware o software. Si confía en la configuración de hardware, firmware y software actual de este equipo, puede [capturar una nueva línea base de TPM](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#capture-the-tpm-baseline-for-each-unique-class-of-hardware) y [registrarla con HGS](guarded-fabric-manage-hgs.md#authorizing-new-guarded-hosts).
TcgLogVerification         | No se puede obtener o comprobar el registro de TCG (línea base de TPM). Esto puede indicar un problema con el firmware del host, el TPM u otros componentes de hardware. Si el host está configurado para intentar el arranque de PXE antes del arranque de Windows, un programa de arranque de red (NBP) anticuado también puede producir este error. Asegúrese de que todos los NBP están actualizados cuando está habilitado el arranque de PXE.
VirtualSecureMode          | Las características de seguridad basadas en la virtualización no se ejecutan en el host. Asegúrese de que VBS está habilitado y de que el sistema cumple las [características de seguridad](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-enable-virtualization-based-security#validate-enabled-device-guard-hardware-based-security-features)de la plataforma configuradas. Consulte la [documentación de Device Guard](/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control-deployment-guide) para obtener más información sobre los requisitos de vbs.

## <a name="modern-tls"></a>TLS moderno

Si ha implementado una directiva de grupo o ha configurado el host de Hyper-V para evitar el uso de TLS 1,0, es posible que aparezca el error "el cliente del servicio de protección de host no pudo desencapsular un protector de clave en nombre de un proceso de llamada" al intentar iniciar una máquina virtual blindada.
Esto se debe a un comportamiento predeterminado en .NET 4,6 en el que la versión de TLS predeterminada del sistema no se tiene en cuenta al negociar las versiones de TLS admitidas con el servidor HGS.

Para evitar este comportamiento, ejecute los dos comandos siguientes para configurar .NET para usar las versiones de TLS predeterminadas del sistema para todas las aplicaciones .NET.

```cmd
reg add HKLM\SOFTWARE\Microsoft\.NETFramework\v4.0.30319 /v SystemDefaultTlsVersions /t REG_DWORD /d 1 /f /reg:64
reg add HKLM\SOFTWARE\Microsoft\.NETFramework\v4.0.30319 /v SystemDefaultTlsVersions /t REG_DWORD /d 1 /f /reg:32
```

> [!WARNING]
> La configuración de versiones de TLS predeterminadas del sistema afectará a todas las aplicaciones .NET del equipo. Asegúrese de probar las claves del registro en un entorno aislado antes de implementarlas en los equipos de producción.

Para obtener más información sobre .NET 4,6 y TLS 1,0, vea [resolver el problema de tls 1,0, 2ª edición](/security/solving-tls1-problem).
