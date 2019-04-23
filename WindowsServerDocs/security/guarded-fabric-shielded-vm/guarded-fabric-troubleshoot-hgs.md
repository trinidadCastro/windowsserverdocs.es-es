---
title: Solución de problemas del servicio guardián de Host
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 424b8090-0692-49a6-9dc4-3c0e77d74b80
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.openlocfilehash: 2dc9a612fa9760a6ca5f05efe1c287fd0872a1d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861256"
---
# <a name="troubleshooting-the-host-guardian-service"></a>Solución de problemas del servicio guardián de Host

> Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se describe las soluciones a problemas habituales detectados al implementar ni el funcionamiento de un servidor de servicio de protección de Host (HGS) en un tejido protegido.
Si no está seguro de la naturaleza del problema, primero intente ejecutar el [protegidos de los diagnósticos de fabric](guarded-fabric-troubleshoot-diagnostics.md) hace que en los servidores HGS y hosts de Hyper-V para reducir el potencial.

## <a name="certificates"></a>Certificados

HGS requiere varios certificados para poder funcionar, incluido el cifrado configurado por el administrador y firma de certificado, así como un certificado de atestación HGS propio administra.
Si estos certificados no están correcta, podrá HGS atender las solicitudes de los hosts de Hyper-V que quieran dar fe o desbloquear protectores de clave para las máquinas virtuales blindadas.
Las secciones siguientes tratan problemas comunes relacionados con certificados configurados en HGS.

### <a name="certificate-permissions"></a>Permisos de certificado

HGS debe poder tener acceso a las claves públicas y privadas de los certificados agregados a HGS por la huella digital del certificado de firma y el cifrado.
En concreto, el grupo administrado cuenta de servicio (gMSA) que se ejecuta el servicio HGS necesita acceso a las claves.
Para buscar la gMSA usa HGS, ejecute el siguiente comando en un símbolo del sistema con privilegios elevados de PowerShell en el servidor HGS:

```powershell
(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName
```

Cómo conceder el acceso a la cuenta de gMSA para usar la clave privada depende de dónde se almacena la clave: en el equipo como un archivo de certificado local, en un módulo de seguridad de hardware (HSM), o mediante un proveedor de almacenamiento de claves personalizados de terceros.

#### <a name="grant-access-to-software-backed-private-keys"></a>Conceder acceso a las claves privadas de software de seguridad

Si está utilizando un certificado autofirmado o un certificado emitido por una entidad de certificación que sea **no** almacenados en un módulo de seguridad de hardware o el proveedor de almacenamiento de claves personalizado, puede cambiar los permisos de clave privados mediante la realización de la pasos siguientes:

1. Administrador de certificados local abierta (certlm.msc)
2. Expanda **Personal > certificados** y busque el certificado de firma o cifrado que desea actualizar.
3. Haga clic con el certificado y seleccione **todas las tareas > Administrar claves privadas**.
4. Haga clic en **agregar** conceder acceso a un nuevo usuario con la clave privada del elegirlos en esta pantalla.
5. En el selector de objetos, escriba el nombre de cuenta de gMSA para HGS encontrada anteriormente, a continuación, haga clic en **Aceptar**.
6. Asegúrese de que tiene la gMSA **lectura** acceso al certificado.
7. Haga clic en **Aceptar** para cerrar la ventana de permiso.

Si se están ejecutando HGS en Server Core o administra el servidor de forma remota, no podrá administrar las claves privadas mediante el Administrador de certificados local.
En su lugar, deberá descargar el [módulo de PowerShell de herramientas de tejido protegido](https://www.powershellgallery.com/packages/GuardedFabricTools) que le permitirá administrar los permisos en PowerShell.

1. Abra una consola de PowerShell con privilegios elevados en el equipo de Server Core o usar la comunicación remota de PowerShell con una cuenta que tenga permisos de administrador local en el HGS.
2. Ejecute los comandos siguientes para instalar el módulo de PowerShell de herramientas de tejido protegido y conceda acceso de la cuenta de gMSA para la clave privada.

```powershell
$certificateThumbprint = '<ENTER CERTIFICATE THUMBPRINT HERE>'

# Install the Guarded Fabric Tools module, if necessary
Install-Module -Name GuardedFabricTools -Repository PSGallery

# Import the module into the current session
Import-Module -Name GuardedFabricTools

# Get the certificate object
$cert = Get-Item "Cert:\LocalMachine\My\$certificateThumbprint"

# Get the gMSA account name
$gMSA = (Get-IISAppPool -Name KeyProtection).ProcessModel.UserName

# Grant the gMSA read access to the certificate
$cert.Acl = $cert.Acl | Add-AccessRule $gMSA Read Allow
```

#### <a name="grant-access-to-hsm-or-custom-provider-backed-private-keys"></a>Conceder acceso a los HSM o personalizadas respaldado por el proveedor de las claves privadas

Si las claves privadas del certificado están respaldadas por un módulo de seguridad de hardware (HSM) o un proveedor personalizado de almacenamiento de claves (KSP), el modelo de permiso dependerá de su proveedor de software específico.
Para obtener resultados óptimos, consulte la documentación del proveedor o sitio de soporte técnico para obtener información sobre la clave privada cómo se administran los permisos para su dispositivo o software específico.

Algunos módulos de seguridad de hardware no admiten la concesión de acceso de cuentas de usuario específico con la clave privada; en su lugar, le permiten el acceso a la cuenta de equipo a todas las claves en un conjunto de claves específico.
Para estos dispositivos, generalmente es suficiente permitir el acceso al equipo a las claves y HGS podrán aprovechar esa conexión.

**Sugerencias para HSM**

A continuación son opciones de configuración sugerido que le ayudarán a correctamente usar claves respaldada por HSM con HGS basadas en Microsoft y experiencias de sus asociados de negocios.
Estas sugerencias se proporcionan para su comodidad y no se garantiza que sea correcta en el momento de lectura, ni tampoco están respaldadas por los fabricantes HSM.
Para obtener información exacta que pertenecen a su dispositivo específico, si tiene más preguntas, póngase en contacto con el fabricante del HSM.

Marca HSM/serie      | Sugerencia
----------------------|-------------
Gemalto SafeNet       | Asegúrese de que la propiedad de uso de clave en el archivo de solicitud de certificado se establece en 0xa0, lo que permite el certificado que se usará para la firma y cifrado. Además, debe conceder a la cuenta de gMSA *leer* acceso a la clave privada mediante la herramienta Administrador de certificados local (consulte los pasos anteriores).
Thales nShield        | Asegúrese de que cada nodo HGS tiene acceso al mundo de seguridad que contiene las claves de firma y cifrado. No es necesario configurar los permisos específicos de la gMSA.
Utimaco CryptoServers | Asegúrese de que la propiedad de uso de clave en el archivo de solicitud de certificado se establece en 0 x 13, lo que permite el certificado que se usará para el cifrado, descifrado y firma.

### <a name="certificate-requests"></a>Solicitudes de certificado

Si utiliza una entidad de certificación para emitir los certificados en un entorno de infraestructura de clave pública (PKI), deberá asegurarse de que la solicitud de certificado incluye los requisitos mínimos para el uso HGS de esas claves.

**Certificados de firma**

Propiedad CSR | Valor requerido
-------------|---------------
Algoritmo    | RSA
Tamaño de clave     | Al menos 2048 bits
Uso de la clave    | Signature/Sign/DigitalSignature

**Certificados de cifrado**

Propiedad CSR | Valor requerido
-------------|---------------
Algoritmo    | RSA
Tamaño de clave     | Al menos 2048 bits
Uso de la clave    | Cifrado/cifrar/DataEncipherment

**Plantillas de servicios de certificados de Active Directory**

Si usa plantillas de certificado de servicios de certificados de Active Directory (ADCS) para crear los certificados, se recomienda use una plantilla con la configuración siguiente:

Propiedad de la plantilla ADCS | Valor requerido
-----------------------|---------------
Categoría del proveedor      | Proveedor de almacenamiento de claves
Nombre del algoritmo         | RSA
Tamaño mínimo de clave       | 2048
Finalidad                | Firma y cifrado
Extensión de uso de claves    | Cifrado de datos de clave de cifrado, firma digital ("permitir el cifrado de datos de usuario")


### <a name="time-drift"></a>Desfase de tiempo

Si la hora del servidor se desvíe significativamente desde que el de otros nodos HGS o hosts de Hyper-V en el tejido protegido, pueden producirse problemas con la validez de certificado del firmante de atestación.
El certificado del firmante atestación se crea y se renueva en segundo plano en HGS y se usa para firmar los certificados de mantenimiento emitidos a los hosts protegidos por el servicio de atestación.

Para actualizar el certificado del firmante atestación, ejecute el siguiente comando en un símbolo del sistema con privilegios elevados de PowerShell.

```powershell
Start-ScheduledTask -TaskPath \Microsoft\Windows\HGSServer -TaskName 
AttestationSignerCertRenewalTask
```

Como alternativa, puede ejecutar manualmente la tarea programada abriendo **programador de tareas** (taskschd.msc), vaya a **biblioteca del programador de tareas > Microsoft > Windows > HGSServer** y ejecutar el tarea denominada **AttestationSignerCertRenewalTask**.

## <a name="switching-attestation-modes"></a>Cambiar entre modos de atestación

Si cambia HGS del modo TPM a modo de Active Directory o viceversa, usando la [conjunto HgsServer](https://technet.microsoft.com/library/mt652180.aspx) cmdlet, puede tardar hasta 10 minutos para cada nodo del clúster de HGS para empezar a aplicar el nuevo modo de atestación.
Este comportamiento es normal.
Se recomienda que no se quitan las directivas que permite a los hosts del modo de atestación anterior hasta que haya comprobado que todos los hosts son avalar correctamente mediante el nuevo modo de atestación.

**Problema conocido al cambiar de TPM a modo de AD**

Si inicializa el clúster HGS en modo TPM y después cambia al modo de Active Directory, hay un problema conocido que impedirá que otros nodos del clúster HGS cambiar al nuevo modo de atestación.
Para asegurarse de que todos los servidores HGS son aplicar el modo de atestación correcto, ejecute `Set-HgsServer -TrustActiveDirectory` **en cada nodo** del clúster de HGS.
Este problema no es aplicable si cambia de modo TPM al modo AD *y* el clúster se configuró originalmente en el modo de AD.

Puede comprobar el modo de atestación de su servidor HGS ejecutando [Get HgsServer](https://technet.microsoft.com/library/mt652162.aspx).

## <a name="memory-dump-encryption-policies"></a>Directivas de cifrado de volcado de memoria

Si está intentando configurar las directivas de cifrado de volcado de memoria y no ve el valor predeterminado HGS volcar directivas (Hgs\_NoDumps, Hgs\_DumpEncryption y Hgs\_DumpEncryptionKey) o el cmdlet de directiva de volcado de memoria ( Add-HgsAttestationDumpPolicy), es probable que no tiene la actualización acumulativa más reciente instalada.
Para solucionar esto, [actualizar el servidor HGS](guarded-fabric-manage-hgs.md#patching-hgs) a la última actualización acumulativa de Windows y [activar las nuevas directivas de atestación](guarded-fabric-manage-hgs.md#updates-requiring-policy-activation).
Asegúrese de que actualizar los hosts de Hyper-V para la misma actualización acumulativa antes de activar las nuevas directivas de certificación, como los hosts que no tienen las nuevas capacidades de cifrado de volcado de memoria instaladas probablemente se producirá un error atestación una vez activada la póliza HGS.

## <a name="endorsement-key-certificate-error-messages"></a>Mensajes de error de certificado de clave de aprobación

Al registrar un host mediante la [agregar HgsAttestationTpmHost](https://docs.microsoft.com/powershell/module/hgsattestation/add-hgsattestationtpmhost) cmdlet, se extraen dos identificadores TPM desde el archivo del identificador de plataforma proporcionada: el certificado de clave de aprobación (EKcert) y la clave de aprobación pública (EKpub).
El EKcert identifica al fabricante del TPM, que proporciona garantías de que el TPM es auténtico y fabricado a través de la cadena de suministro normal.
El EKpub identifica ese TPM específico y es una de las medidas que HGS usa para conceder a un host de acceso para ejecutar máquinas virtuales blindadas.

Recibirá un error al registrar un host TPM si se cumple alguna de las dos condiciones:
1. El archivo del identificador de plataforma **no** contienen un certificado de clave de aprobación
2. El archivo del identificador de plataforma contiene un certificado de clave de aprobación, pero ese certificado **no de confianza** en el sistema

Algunos fabricantes de TPM no incluyen EKcerts en sus TPM.
Si sospecha que esto es el caso de TPM, confirme con su OEM que el TPM no deben tener un EKcert y usar el `-Force` marca para registrar manualmente el host con HGS.
Si el TPM debe tener un EKcert pero no se encontró ninguno en el archivo del identificador de plataforma, asegúrese de que usa una consola de PowerShell del administrador (elevado) cuando se ejecuta [Get PlatformIdentifier](https://docs.microsoft.com/powershell/module/platformidentifier/get-platformidentifier) en el host.

Si recibe el error que su EKcert no es de confianza, asegúrese de que tiene [instalado el paquete de certificados de raíz de confianza TPM](guarded-fabric-install-trusted-tpm-root-certificates.md) en cada servidor HGS y que el certificado raíz para el proveedor del TPM está presente en del equipo local  **TrustedTPM\_RootCA** almacenar. Cualquier certificado intermedio aplicable también debe instalarse en el **TrustedTPM\_IntermediateCA** almacenar en el equipo local.
Después de instalar los certificados intermedios y raíz, debe ser capaz de ejecutar `Add-HgsAttestationTpmHost` correctamente.
