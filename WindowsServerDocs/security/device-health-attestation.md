---
title: "Atestación de estado del dispositivo"
H1: na
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.technology:
- techgroup-security
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8e7b77a4-1c6a-4c21-8844-0df89b63f68d
author: brianlic-msft
ms.date: 10/12/2016
ms.openlocfilehash: d304ee3456f8db1e5b202c1d9221d1374a5251be
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="device-health-attestation"></a>Atestación de estado del dispositivo

>Se aplica a: Windows Server 2016

Introducida en Windows 10, versión 1507, la atestación de estado de dispositivo (DHA) incluyen lo siguiente:

-   Se integra con el marco de trabajo de Windows 10 Mobile Device Management (MDM) de acuerdo con [estándares de Open Mobile Alliance (OMA)](http://openmobilealliance.org/).

-   Admite dispositivos que tienen una plataforma de módulo de segura (TPM) aprovisionados en un formato discreto o de firmware.

-   Las empresas permite aumentar la seguridad de su organización para hardware supervisan y certificada seguridad, con mínimo o ningún impacto en el costo de la operación.

A partir de Windows Server 2016, ahora puedes ejecutar el servicio DHA como un rol de servidor dentro de la organización. Usa este tema para obtener información sobre cómo instalar y configurar el rol de servidor de atestación de estado del dispositivo.

## <a name="overview"></a>Introducción

Puedes usar DHA para evaluar el estado del dispositivo para:
  
-   Dispositivos de Windows 10 y Windows 10 Mobile que admitan TPM 1.2 o 2.0.  
-   Los dispositivos que se administran mediante Active Directory con acceso a Internet, los dispositivos que se administran mediante Active Directory sin acceso a Internet, los dispositivos administrados por Azure Active Directory o una implementación híbrida con Active Directory y Azure Active Directory local.


### <a name="dha-service"></a>Servicio DHA

El servicio DHA valida los registros de TPM y PCR para un dispositivo y, a continuación, envía un informe de DHA. Microsoft ofrece el servicio DHA de tres formas:

- **Servicio de nube DHA** DHA administrados por Microsoft de un servicio gratuito, equilibrio de carga de geográfico y optimizado para el acceso desde diferentes regiones del mundo.

- **DHA servicio local** un nuevo rol de servidor introducido en Windows Server 2016. Está disponible de forma gratuita para los clientes que tengan una licencia de Windows Server 2016.

- **Servicio de nube de Azure DHA** un host virtual de Microsoft Azure. Para ello, necesitas un host virtual y licencias para el servicio DHA local.

El servicio DHA se integra con soluciones MDM y proporciona lo siguiente: 

-   Combinar la información que se reciben de dispositivos (a través de los canales de comunicación de administración de dispositivo existente) con el informe DHA
-   Tomar una decisión de seguridad más seguro y de confianza, basada en hardware atestiguan y los datos protegidos

Este es un ejemplo que muestra cómo puedes usar DHA para ayudar a alzar el listón de protección de seguridad para los activos de la organización.

1. Puedes crear una directiva que comprueba los siguientes configuración de arranque o atributos:
  - Arranque seguro
  - BitLocker
  - ELAM
2. La solución MDM aplica esta directiva y desencadena una acción correctiva en función de los datos del informe DHA.  Por ejemplo, podría comprueba lo siguiente:
  - Se ha habilitado el arranque seguro, los códigos de confianza es auténtico puede cargar el dispositivo y el cargador de arranque de Windows no se ha alterado.
  - Arranque verificada la firma digital del kernel de Windows y los componentes que se cargan al inicia el dispositivo de confianza.
  - El arranque medido crea una traza de auditoría protegido de TPM que se pudo comprobar de forma remota.
  - Se habilitó BitLocker y que protegido los datos cuando el dispositivo se ha activado desactivado.
  - ELAM se habilitó en las primeras fases de arranque y está supervisando el tiempo de ejecución.
  
#### <a name="dha-cloud-service"></a>Servicio de nube DHA

El servicio de nube DHA ofrece las siguientes ventajas:

-   Revisa los registros de arranque de dispositivo TCG y PCR que recibe de un dispositivo que se inscribe con una solución MDM. 
-   Crea un resistente a alteraciones y el informe de evidencia de alteración (DHA informe) que describe cómo el dispositivo inicia según los datos que se recopila y protegidos por el chip TPM de un dispositivo. 
-   Ofrece el informe DHA al servidor MDM que solicitó el informe en un canal de comunicación protegido.

#### <a name="dha-on-premises-service"></a>Servicio local DHA

El servicio de locales DHA ofrecer todas las funcionalidades proporcionadas por el servicio de nube DHA.  También permite a los clientes:

 - Optimizar el rendimiento mediante la ejecución de servicio DHA en su propio centro de datos
 - Asegúrate de que el informe DHA no salir a la red

#### <a name="dha-azure-cloud-service"></a>Servicio de nube de Azure DHA

Este servicio proporciona la misma funcionalidad que el servicio local DHA, salvo que se ejecuta el servicio de nube de Azure DHA como un host virtual de Microsoft Azure.

### <a name="dha-validation-modes"></a>Modos de validación DHA

Puedes configurar el servicio DHA local para ejecutar en modo de validación EKCert o AIKCert. Cuando el servicio DHA envía un informe, indica si se emitió en modo de validación AIKCert o EKCert. Modos de validación de EKCert y AIKCert ofrecen la misma garantía de seguridad como la cadena de EKCert de confianza se mantiene actualizada.

#### <a name="ekcert-validation-mode"></a>Modo de validación EKCert

Modo de validación de EKCert está optimizado para dispositivos en organizaciones que no están conectados a Internet. Dispositivos que se conectan a un servicio DHA que se ejecuta en modo de validación de EKCert hacen **no** tienen acceso directo a Internet.

Cuando DHA se ejecuta en modo de validación de EKCert, se basa en una cadena de administrado de empresa de confianza que se deba actualizar en ocasiones (aproximadamente 5 - 10 veces al año). 

Microsoft publica paquetes agregados de raíces de confianza y de CA intermedia para fabricantes TPM aprobados (cuando estén disponibles) en un archivo en el archivo .cab sea accesible públicamente. Deberás descargar la fuente, validar la integridad e instalarlo en el servidor de atestación de estado del dispositivo.

Es un archivo de ejemplo [https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab](https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab).

#### <a name="aikcert-validation-mode"></a>Modo de validación AIKCert

Modo de validación de AIKCert está optimizado para entornos operativos que tienen acceso a Internet. Dispositivos que se conectan a un servicio DHA que se ejecuta en modo de validación de AIKCert deben tener un acceso directo a Internet y puedes obtener un certificado de AIK de Microsoft. 

## <a name="install-and-configure-the-dha-service-on-windows-server-2016"></a>Instalar y configurar el servicio DHA en Windows Server 2016

Usa las siguientes secciones para obtener DHA instalado y configurado en Windows Server 2016.

### <a name="prerequisites"></a>Requisitos previos

Para configurar y comprobar un servicio local DHA, necesitas:

- Un servidor que ejecuta Windows Server 2016.
- Crear uno (o más) de dispositivos de cliente de Windows 10 con un TPM (1.2 o 2.0) que se encuentra en estado borrar/listo ejecutando la versión más reciente de Windows Insider.
- Decide si vas a ejecutar en modo de validación EKCert o AIKCert.
- Los certificados siguientes:
  - **Certificado SSL DHA** un certificado SSL x.509 que se encadene a una raíz de confianza de empresa con una clave privada puede exportar. Este certificado protege DHA datos las comunicaciones en tránsito como servidor (servicio DHA y el servidor MDM) y el servidor para comunicaciones de cliente (servicio DHA y un dispositivo Windows 10).
  - **Certificado de firma de DHA** un certificado x.509 que se encadene a una raíz de confianza de empresa con una clave privada puede exportar. El servicio DHA usa este certificado de firma digital. 
  - **Certificado de cifrado DHA** un certificado x.509 que se encadene a una raíz de confianza de empresa con una clave privada puede exportar. El servicio DHA también usa este certificado para el cifrado. 


### <a name="install-windows-server-2016"></a>Instalar a Windows Server 2016

Instalar Windows Server 2016 mediante el método de instalación preferido, como los servicios de implementación de Windows, o ejecuta el programa de instalación desde un medio de arranque, una unidad USB o el sistema de archivos local. Si es la primera vez que se va a configurar el servicio de local DHA, debe instalar Windows Server 2016 usando el **experiencia de escritorio** opción de instalación.

### <a name="add-the-device-health-attestation-server-role"></a>Agrega el rol de servidor de atestación de estado del dispositivo

Puedes instalar el rol de servidor de atestación de estado del dispositivo y sus dependencias mediante el administrador del servidor. 

Después de instalar Windows Server 2016, el dispositivo se reinicia y se abre el administrador del servidor. Si el administrador del servidor no se inicia automáticamente, haz clic en **inicio**y, a continuación, haz clic en **administrador del servidor**.

1.  Haz clic en **agregar roles y características**.
2.  En la **antes de comenzar** página, haz clic en **siguiente**.
3.  En la **selecciona el tipo de instalación** página, haz clic en **instalación basada en rol o característica**y, a continuación, haz clic en **siguiente**.
4.  En la **servidor de destino selecciona** página, haz clic en **seleccionar un servidor desde el grupo de servidores**, selecciona el servidor y, a continuación, haz clic en **siguiente**.
5.  En la **seleccionar roles de servidor**, seleccione la **atestación de estado del dispositivo** casilla de verificación.
6.  Haz clic en **agregar características** para instalar otras necesarias características y servicios de rol.
7.  Haz clic en **siguiente**.
8.  En la **Select features** página, haz clic en **siguiente**.
9.  En la **rol de servidor Web (IIS)** página, haz clic en **siguiente**.
10. En la **seleccione los servicios de rol** página, haz clic en **siguiente**.
11. En la **servicio de atestación de estado de dispositivo** página, haz clic en **siguiente**.
12. En la **Confirmar selecciones de instalación** página, haz clic en **instalar**.
13. Cuando termine la instalación, haz clic en **cerrar**.

### <a name="install-the-signing-and-encryption-certificates"></a>Instalar los certificados de firma y cifrado

Mediante el siguiente script de Windows PowerShell para instalar los certificados de firma y cifrado. Para obtener más información acerca de la huella digital, consulta [Cómo: recuperar la huella digital de un certificado](https://msdn.microsoft.com/library/ms734695.aspx).

```
$key = Get-ChildItem Cert:\LocalMachine\My | Where-Object {$_.Thumbprint -like "<thumbprint>"}
$keyname = $key.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName
$keypath = $env:ProgramData + "\Microsoft\Crypto\RSA\MachineKeys\" + $keyname
icacls $keypath /grant <username>`:R
  
#<thumbprint>: Certificate thumbprint for encryption certificate or signing certificate
#<username>: Username for web service app pool, by default IIS_IUSRS
```

### <a name="install-the-trusted-tpm-roots-certificate-package"></a>Instalar el paquete de certificado de confianza TPM raíces

Para instalar el paquete de certificado de confianza TPM raíces, debes extraerlo, quitar cualquier cadenas de confianza que no son de confianza para la organización y, a continuación, ejecutar setup.cmd.

#### <a name="download-the-trusted-tpm-roots-certificate-package"></a>Descargar el paquete de certificado de confianza TPM raíces

Antes de instalar el paquete de certificado, puedes descargar la última lista de raíces de confianza TPM desde [https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab](https://tpmsec.microsoft.com/OnPremisesDHA/TrustedTPM.cab).

> **Importante:** antes de instalar el paquete, comprueba que está firmado digitalmente por Microsoft.

#### <a name="extract-the-trusted-certificate-package"></a>Extraer el paquete de certificados de confianza
Extrae el paquete de certificado de confianza mediante la ejecución de los siguientes comandos.
```
mkdir .\TrustedTpm
expand -F:* .\TrustedTpm.cab .\TrustedTpm
```

#### <a name="remove-the-trust-chains-for-tpm-vendors-that-are-not-trusted-by-your-organization-optional"></a>Quitar las cadenas de confianza para los proveedores TPM que estén *no* de confianza para su organización (opcional)

Eliminar las carpetas en cualquier cadenas de confianza de proveedor TPM que no son de confianza para la organización.

> **Nota:** si usa el modo de certificado AIK, la carpeta de Microsoft es necesaria para validar certificado de AIK de Microsoft.

#### <a name="install-the-trusted-certificate-package"></a>Instalar el paquete de certificados de confianza
Instalar el paquete de certificado de confianza ejecutando el script de configuración desde el archivo .cab.

```
.\setup.cmd
```

### <a name="configure-the-device-health-attestation-service"></a>Configurar el servicio de atestación de estado del dispositivo

Puedes usar Windows PowerShell para configurar el servicio DHA local.

```
Install-DeviceHealthAttestation -EncryptionCertificateThumbprint <encryption> -SigningCertificateThumbprint <signing> -SslCertificateStoreName My -SslCertificateThumbprint <ssl> -SupportedAuthenticationSchema "<schema>"

#<encryption>: Thumbprint of the encryption certificate
#<signing>: Thumbprint of the signing certificate
#<ssl>: Thumbprint of the SSL certificate
#<schema>: Comma-delimited list of supported schemas including AikCertificate, EkCertificate, and AikPub
```

### <a name="configure-the-certificate-chain-policy"></a>Configurar la directiva de la cadena de certificados

Configurar la directiva de la cadena de certificados ejecutando el siguiente script de Windows PowerShell.

```
$policy = Get-DHASCertificateChainPolicy
$policy.RevocationMode = "NoCheck"
Set-DHASCertificateChainPolicy -CertificateChainPolicy $policy
```

## <a name="dha-management-commands"></a>Comandos de administración de DHA

Estos son algunos ejemplos de Windows PowerShell que pueden ayudarte a administrar el servicio DHA.

### <a name="configure-the-dha-service-for-the-first-time"></a>Configurar el servicio DHA por primera vez

```
Install-DeviceHealthAttestation -SigningCertificateThumbprint "<HEX>" -EncryptionCertificateThumbprint "<HEX>" -SslCertificateThumbprint "<HEX>" -Force
```

### <a name="remove-the-dha-service-configuration"></a>Quitar la configuración del servicio DHA

```
Uninstall-DeviceHealthAttestation -RemoveSslBinding -Force
```
### <a name="get-the-active-signing-certificate"></a>Obtener el certificado de firma de activo

```
Get-DHASActiveSigningCertificate
```
### <a name="set-the-active-signing-certificate"></a>Establece el certificado de firma de activo

```
Set-DHASActiveSigningCertificate -Thumbprint "<hex>" -Force
```

> **Nota:** este certificado debe implementarse en el servidor que ejecuta el servicio DHA la **LocalMachine\My** almacén de certificados. Cuando se establece el certificado de firma activo, el certificado de firma de activo existente se mueve a la lista de certificados de firma inactivas.

### <a name="list-the-inactive-signing-certificates"></a>Lista de los certificados de firma inactivos
```
Get-DHASInactiveSigningCertificates
```

### <a name="remove-any-inactive-signing-certificates"></a>Quitar los certificados de firmas inactivos
```
Remove-DHASInactiveSigningCertificates -Force
Remove-DHASInactiveSigningCertificates  -Thumbprint "<hex>" -Force
```

> **Nota:** solo *una* inactiva certificado (de cualquier tipo) puede existir en el servicio en cualquier momento. Certificados deben quitarse de la lista de certificados inactivos cuando ya no son necesarios.

### <a name="get-the-active-encryption-certificate"></a>Obtener el certificado de cifrado activo

```
Get-DHASActiveEncryptionCertificate
```

### <a name="set-the-active-encryption-certificate"></a>Establece el certificado de cifrado activo

```
Set-DHASActiveEncryptionCertificate -Thumbprint "<hex>" -Force
```

El certificado debe implementarse en el dispositivo en el **LocalMachine\My** almacén de certificados. 

Cuando se establece el certificado de cifrado activo, el certificado de cifrado active existente se mueve a la lista de certificados de cifrado inactiva.

### <a name="list-the-inactive-encryption-certificates"></a>Lista de los certificados de cifrado inactivo

```
Get-DHASInactiveEncryptionCertificates
```
### <a name="remove-any-inactive-encryption-certificates"></a>Quitar los certificados de cifrado inactivo

```
Remove-DHASInactiveEncryptionCertificates -Force
Remove-DHASInactiveEncryptionCertificates -Thumbprint "<hex>" -Force 
```

### <a name="get-the-x509chainpolicy-configuration"></a>Obtener la configuración de X509ChainPolicy 

```
Get-DHASCertificateChainPolicy
```
### <a name="change-the-x509chainpolicy-configuration"></a>Cambiar la configuración de X509ChainPolicy

```
$certificateChainPolicy = Get-DHASInactiveEncryptionCertificates
$certificateChainPolicy.RevocationFlag = <X509RevocationFlag>
$certificateChainPolicy.RevocationMode = <X509RevocationMode>
$certificateChainPolicy.VerificationFlags = <X509VerificationFlags>
$certificateChainPolicy.UrlRetrievalTimeout = <TimeSpan>
Set-DHASCertificateChainPolicy = $certificateChainPolicy
```

## <a name="dha-service-reporting"></a>Informe de servicio DHA

La siguiente es una lista de los mensajes que se notifican por el servicio DHA a la solución MDM: 

- **200** Aceptar HTTP. El certificado se devuelve.
- **400** solicitud incorrecta. Formato de solicitud no válida, el certificado de estado no válido, la firma del certificado no coincidencia, el Blob de atestación de estado no válido o un Blob de estado de estado no válido. La respuesta también contiene un mensaje, como se describe el esquema de respuesta, con un código de error y un mensaje de error que se puede usar para diagnóstico.
- **500** error interno del servidor. Esto puede suceder si hay problemas que impiden que el servicio de emitir certificados.
- **503** regulación es rechazar solicitudes para evitar la sobrecarga de servidor. 
