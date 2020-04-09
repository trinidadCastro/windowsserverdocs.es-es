---
title: Certificación de estado del dispositivo
ms.technology: techgroup-security
ms.topic: article
ms.assetid: 8e7b77a4-1c6a-4c21-8844-0df89b63f68d
author: brianlic-msft
ms.author: brianlic
ms.date: 10/12/2016
ms.openlocfilehash: 2e810a2a20e7c5bdc404077760e259468cecd24e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857038"
---
# <a name="device-health-attestation"></a>Certificación de estado del dispositivo

>Se aplica a: Windows Server 2016

Introducido en Windows 10, versión 1507, Atestación de estado de dispositivo (DHA) incluye lo siguiente:

-    Se integra con la plataforma de Administración de dispositivos móviles (MDM) de Windows 10 junto con los [estándares de la alianza móvil abierta (OMA)](http://openmobilealliance.org/).

-    Admite dispositivos que tienen un Módulo de plataforma segura (TPM) aprovisionado en formato discreto o de firmware.

-    Permite a las empresas aumentar la seguridad de su organización al supervisar el hardware y atestiguar la seguridad con mínimo impacto, o sin él, en el costo de la operación.

A partir de Windows Server 2016, ahora puede ejecutar el servicio DHA como un rol de servidor dentro de su organización. Este tema aprenderá a instalar y configurar el rol de servidor de Atestación de estado de dispositivo.

## <a name="overview"></a>Información general

Puede usar DHA para evaluar el estado de dispositivo para:
  
-    Dispositivos Windows 10 y Windows 10 Mobile que admiten TPM 1.2 o 2.0.  
-    Dispositivos locales administrados mediante Active Directory con acceso a Internet, dispositivos administrados mediante Active Directory sin acceso a Internet, dispositivos administrados por Azure Active Directory o una implementación híbrida con Active Directory y Azure Active Directory.


### <a name="dha-service"></a>Servicio DHA

El servicio DHA valida los registros de TPM y PCR para un dispositivo y, a después, emite un informe DHA. Microsoft ofrece el servicio DHA de tres maneras:

- **Servicio en la nube DHA**: un servicio DHA administrado por Microsoft que es gratuito, de carga equilibrada geográfica y optimizado para el acceso desde distintas regiones del mundo.

- **Servicio local DHA** : un nuevo rol de servidor incluido en Windows Server 2016. Está disponible de manera gratuita para los clientes con una licencia de Windows Server 2016.

- **Servicio en la nube DHA de Azure**: un host virtual en Microsoft Azure. Para ello, necesita un host virtual y licencias para el servicio local DHA.

El servicio DHA se integra con las soluciones MDM y proporciona lo siguiente: 

-    Combinar la información que reciben de dispositivos (a través de canales de comunicación de administración de dispositivos existentes) con el informe DHA
-    Tomar una decisión de seguridad más segura y confiable basada en el hardware atestiguado y en datos protegidos

Este es un ejemplo que muestra cómo puede utilizar DHA para aumentar la protección de seguridad para los recursos de su organización.

1. Va a crear una directiva que comprueba los atributos o configuración de arranque siguientes:
   - Arranque seguro
   - BitLocker
   - ELAM
2. La solución MDM aplica esta directiva y desencadena una acción correctora basándose en los datos del informe DHA.  Por ejemplo, puede comprobar lo siguiente:
   - Se habilitó el arranque seguro, el dispositivo cargó un código de confianza auténtico y no se alteró el cargador de arranque de Windows.
   - El arranque seguro comprobó correctamente la firma digital del kernel de Windows y los componentes que se cargaron al iniciar el dispositivo.
   - El arranque medido creó una pista de auditoría protegida por TPM que se puede comprobar de forma remota.
   - Se habilitó BitLocker y protegió los datos cuando se desconectó el dispositivo.
   - ELAM se habilitó en las primeras fases de arranque y supervisa el tiempo de ejecución.
  
#### <a name="dha-cloud-service"></a>Servicio en la nube DHA

El servicio en la nube DHA ofrece las siguientes ventajas:

-    Revisa los registros de arranque de dispositivos TCG y PCR que recibe de un dispositivo que se inscribe con una solución MDM. 
-    Crea un informe resistente a alteraciones y visible a alteraciones (informe DHA) que describe cómo se inició el dispositivo según los datos que se recopilan y protegen por el chip TPM de un dispositivo. 
-    Proporciona el informe DHA al servidor MDM que solicitó el informe en un canal de comunicación protegido.

#### <a name="dha-on-premises-service"></a>Servicio local DHA

El servicio local DHA ofrece todas las funcionalidades que proporcionaba el servicio en la nube DHA.  También permite a los clientes:

 - Optimizar el rendimiento mediante la ejecución de servicio DHA en su propio centro de datos
 - Asegurarse de que el informe DHA no sale de su red

#### <a name="dha-azure-cloud-service"></a>Servicio en la nube DHA de Azure

Este servicio proporciona la misma funcionalidad que el servicio local DHA, excepto que el servicio en la nube DHA de Azure se ejecuta como un host virtual en Microsoft Azure.

### <a name="dha-validation-modes"></a>Modos de validación DHA

Puede configurar el servicio local DHA para que se ejecute en modo de validación EKCert o AIKCert. Cuando el servicio DHA emite un informe, indica si se emitió en modo de validación AIKCert o EKCert. Los modos de validación EKCert y AIKCert ofrecen el miso control de seguridad siempre y cuando la cadena de EKCert de confianza se mantenga actualizada.

#### <a name="ekcert-validation-mode"></a>Modo de validación EKCert

El modo de validación EKCert está optimizado para dispositivos de organizaciones que no están conectados a Internet. Los dispositivos que se conectan a un servicio DHA que se ejecutan en modo de validación EKCert **no** tienen acceso directo a Internet.

Cuando DHA se ejecuta en modo de validación EKCert, depende de una cadena de administrada empresarial de confianza que tiene que actualizar ocasionalmente (aproximadamente 5 - 10 veces al año). 

Microsoft publica paquetes agregados de raíces de confianza y a una CA intermedia para los fabricantes de TPM aprobados (cuando estén disponibles) en un archivo .cab accesible públicamente. Debe descargar la fuente, validar su integridad e instalarla en el servidor que ejecuta la Atestación de estado de dispositivo.

Un archivo de ejemplo se [https://go.microsoft.com/fwlink/?linkid=2097925](https://go.microsoft.com/fwlink/?linkid=2097925).

#### <a name="aikcert-validation-mode"></a>Modo de validación AIKCert

El modo de validación AIKCert está optimizado para entornos operativos que tienen acceso a Internet. Los dispositivos que se conectan a un servicio DHA que se ejecuta en modo de validación AIKCert deben tener acceso directo a Internet y poder obtener un certificado AIK de Microsoft. 

## <a name="install-and-configure-the-dha-service-on-windows-server-2016"></a>Instalación y configuración del servicio DHA en Windows Server 2016

Utilice las siguientes secciones para instalar y configurar DHA en Windows Server 2016.

### <a name="prerequisites"></a>Requisitos previos

Para configurar y comprobar un servicio local DHA, necesita:

- Un servidor que ejecuta Windows Server 2016.
- Un dispositivo (o más) de cliente de Windows 10 con un TPM (1.2 0 2.0) que se encuentre en un estado cifrado o listo y que ejecute la compilación más reciente de Windows Insider.
- Decida si va a ejecutar en modo de validación EKCert o AIKCert.
- Los certificados siguientes:
  - **Certificado SSL DHA**: un certificado SSL x.509 que se vincula a una raíz de confianza de la empresa con una clave privada exportable. Este certificado protege las comunicaciones de datos DHA en tránsito, incluidas las comunicaciones entre servidores (servicio DHA y servidor MDM) y entre servidor y cliente (servicio DHA y un dispositivo de Windows 10).
  - **Certificado de firma DHA**: un certificado x.509 que se vincula a una raíz de confianza de la empresa con una clave privada+ exportable. El servicio DHA utiliza este certificado para la firma digital. 
  - **Certificado de firma DHA**: un certificado x.509 que se vincula a una raíz de confianza de la empresa con una clave privada exportable. El servicio DHA también utiliza este certificado para el cifrado. 


### <a name="install-windows-server-2016"></a>Instalación de Windows Server 2016

Instale Windows Server 2016 usando el método de instalación que prefiere, como Servicios de implementación de Windows, o mediante la ejecución del programa de instalación desde medios de arranque, una unidad USB o el sistema de archivos local. Si esta es la primera vez que se va a configurar el servicio local DHA, debe instalar Windows Server 2016 con la opción de instalación **Experiencia de escritorio**.

### <a name="add-the-device-health-attestation-server-role"></a>Adición del rol de servidor de Atestación de estado de dispositivo

Puede instalar el rol de servidor de Atestación de estado de dispositivo y sus dependencias mediante el Administrador del servidor. 

Después de instalar Windows Server 2016, el dispositivo se reinicia y se abre el Administrador del servidor. Si Administrador del servidor no se inicia automáticamente, haga clic en **Inicio** y después escriba **Administrador del servidor**.

1.    Haga clic en **Agregar roles y características**.
2.    En la página **Antes de comenzar**, haz clic en **Siguiente**.
3.    En la página **Seleccionar tipo de instalación**, haga clic en **Instalación basada en características o en roles** y, a continuación, en **Siguiente**.
4.    En la página **Seleccionar servidor de destino**, haga clic en **Seleccionar un servidor del grupo de servidores**, seleccione un servidor y haga clic en **Siguiente**.
5.    En la página **Seleccionar roles de servidor**, active la casilla **Atestación de mantenimiento del dispositivo**.
6.    Haga clic en **Agregar características** para instalar otros servicios de rol y características necesarios.
7.    Haga clic en **Siguiente**.
8.    En la página **Seleccionar características**, haz clic en **Siguiente**.
9.    En la página **Rol Servidor web (IIS)** , haz clic en **Siguiente**.
10.    En la página **Seleccionar servicios de rol**, haz clic en **Siguiente**.
11.    En la página **Servicio de atestación de estado de dispositivo**, haga clic en **Siguiente**.
12.    En la página **Confirmar selecciones de instalación** , haga clic en **Instalar**.
13.    Cuando haya finalizado la instalación, haga clic en **Cerrar**.

### <a name="install-the-signing-and-encryption-certificates"></a>Instalación de los certificados de firma y cifrado

Utilice el script de Windows PowerShell siguiente para instalar los certificados de firma y cifrado. Para obtener más información acerca de la huella digital, consulte [Cómo: recuperar la huella digital de un certificado](https://msdn.microsoft.com/library/ms734695.aspx).

```
$key = Get-ChildItem Cert:\LocalMachine\My | Where-Object {$_.Thumbprint -like "<thumbprint>"}
$keyname = $key.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName
$keypath = $env:ProgramData + "\Microsoft\Crypto\RSA\MachineKeys\" + $keyname
icacls $keypath /grant <username>`:R
  
#<thumbprint>: Certificate thumbprint for encryption certificate or signing certificate
#<username>: Username for web service app pool, by default IIS_IUSRS
```

### <a name="install-the-trusted-tpm-roots-certificate-package"></a>Instalación del paquete de certificado raíz TPM de confianza

Para instalar el paquete de certificado raíz TPM de confianza, debe extraerlo, quitar las cadenas de confianza que no sean de confianza para su organización y, después, ejecutar setup.cmd.

#### <a name="download-the-trusted-tpm-roots-certificate-package"></a>Descarga del paquete de certificado raíz TPM de confianza

Antes de instalar el paquete de certificado, puede descargar la lista más reciente de raíces de TPM de confianza de [https://go.microsoft.com/fwlink/?linkid=2097925](https://go.microsoft.com/fwlink/?linkid=2097925).

> **Importante:** Antes de instalar el paquete, compruebe que está firmado digitalmente por Microsoft.

#### <a name="extract-the-trusted-certificate-package"></a>Extracción del paquete de certificado de confianza
Extraiga el paquete de certificado de confianza mediante la ejecución de los comandos siguientes.
```
mkdir .\TrustedTpm
expand -F:* .\TrustedTpm.cab .\TrustedTpm
```

#### <a name="remove-the-trust-chains-for-tpm-vendors-that-are-not-trusted-by-your-organization-optional"></a>Quite las cadenas de confianza para los proveedores TPM que *no* sean de confianza para su organización (opcional)

Elimine las carpetas de cualquier cadena de confianza de proveedor TPM que no sea de confianza para su organización.

> **Nota:** Si utiliza el modo de certificado AIK, es preciso que la carpeta de Microsoft valide los certificados de AIK emitidos por Microsoft.

#### <a name="install-the-trusted-certificate-package"></a>Instalación del paquete de certificado de confianza
Instale el paquete de certificado de confianza mediante la ejecución del script de instalación desde el archivo .cab.

```
.\setup.cmd
```

### <a name="configure-the-device-health-attestation-service"></a>Configuración del servicio Atestación de estado de dispositivo

Puede usar Windows PowerShell para configurar el servicio local DHA.

```
Install-DeviceHealthAttestation -EncryptionCertificateThumbprint <encryption> -SigningCertificateThumbprint <signing> -SslCertificateStoreName My -SslCertificateThumbprint <ssl> -SupportedAuthenticationSchema "<schema>"

#<encryption>: Thumbprint of the encryption certificate
#<signing>: Thumbprint of the signing certificate
#<ssl>: Thumbprint of the SSL certificate
#<schema>: Comma-delimited list of supported schemas including AikCertificate, EkCertificate, and AikPub
```

### <a name="configure-the-certificate-chain-policy"></a>Configuración de la directiva de la cadena de certificados

Configure la directiva de la cadena de certificados mediante la ejecución del siguiente script de Windows PowerShell.

```
$policy = Get-DHASCertificateChainPolicy
$policy.RevocationMode = "NoCheck"
Set-DHASCertificateChainPolicy -CertificateChainPolicy $policy
```

## <a name="dha-management-commands"></a>Comandos de administración DHA

Estos son algunos ejemplos de Windows PowerShell que pueden ayudarle a administrar el servicio DHA.

### <a name="configure-the-dha-service-for-the-first-time"></a>Configuración del servicio DHA por primera vez

```
Install-DeviceHealthAttestation -SigningCertificateThumbprint "<HEX>" -EncryptionCertificateThumbprint "<HEX>" -SslCertificateThumbprint "<HEX>" -Force
```

### <a name="remove-the-dha-service-configuration"></a>Supresión de la configuración del servicio DHA

```
Uninstall-DeviceHealthAttestation -RemoveSslBinding -Force
```
### <a name="get-the-active-signing-certificate"></a>Obtención del certificado de firma activo

```
Get-DHASActiveSigningCertificate
```
### <a name="set-the-active-signing-certificate"></a>Establecimiento del certificado de firma activo

```
Set-DHASActiveSigningCertificate -Thumbprint "<hex>" -Force
```

> **Nota:** Este certificado debe implementarse en el servidor que ejecuta el servicio DHA en el almacén de certificados **LocalMachine\My**. Cuando se establece el certificado de firma activo, el certificado de firma activo existente se mueve a la lista de certificados de firma inactivos.

### <a name="list-the-inactive-signing-certificates"></a>Lista de los certificados de firma inactivos
```
Get-DHASInactiveSigningCertificates
```

### <a name="remove-any-inactive-signing-certificates"></a>Supresión de los certificados de firma inactivos
```
Remove-DHASInactiveSigningCertificates -Force
Remove-DHASInactiveSigningCertificates  -Thumbprint "<hex>" -Force
```

> **Nota:** Solo puede existir *un* certificado inactivo (de cualquier tipo) en el servicio en cualquier momento. Los certificados deben quitarse de la lista de certificados inactivos cuando ya no sean necesarios.

### <a name="get-the-active-encryption-certificate"></a>Obtención del certificado de cifrado activo

```
Get-DHASActiveEncryptionCertificate
```

### <a name="set-the-active-encryption-certificate"></a>Establecimiento del certificado de cifrado activo

```
Set-DHASActiveEncryptionCertificate -Thumbprint "<hex>" -Force
```

El certificado debe implementarse en el dispositivo en el almacén de certificados **LocalMachine\My**. 

Cuando se establece el certificado de cifrado activo, el certificado de cifrado activo existente se mueve a la lista de certificados de cifrado inactivos.

### <a name="list-the-inactive-encryption-certificates"></a>Lista de los certificados de cifrado inactivos

```
Get-DHASInactiveEncryptionCertificates
```
### <a name="remove-any-inactive-encryption-certificates"></a>Supresión de los certificados de cifrado inactivos

```
Remove-DHASInactiveEncryptionCertificates -Force
Remove-DHASInactiveEncryptionCertificates -Thumbprint "<hex>" -Force 
```

### <a name="get-the-x509chainpolicy-configuration"></a>Obtención de la configuración de X509ChainPolicy 

```
Get-DHASCertificateChainPolicy
```
### <a name="change-the-x509chainpolicy-configuration"></a>Cambio de la configuración de X509ChainPolicy

```
$certificateChainPolicy = Get-DHASInactiveEncryptionCertificates
$certificateChainPolicy.RevocationFlag = <X509RevocationFlag>
$certificateChainPolicy.RevocationMode = <X509RevocationMode>
$certificateChainPolicy.VerificationFlags = <X509VerificationFlags>
$certificateChainPolicy.UrlRetrievalTimeout = <TimeSpan>
Set-DHASCertificateChainPolicy = $certificateChainPolicy
```

## <a name="dha-service-reporting"></a>Informes de servicio DHA

Lo siguiente es una lista de mensajes notificados por el servicio DHA a la solución MDM: 

- **200** HTTP OK. Se devuelve el certificado.
- **400** Solicitud incorrecta. Formato de solicitud no válido, certificado de mantenimiento no válido, la firma del certificado no coincide, el blob de atestación de estado no válido o un blob de estado de mantenimiento no válido. La respuesta también contiene un mensaje, como se describe en el esquema de respuesta con un código de error y un mensaje de error que se puede usar para diagnóstico.
- **500** Error interno del servidor. Esto puede ocurrir si hay problemas que impiden que el servicio emita los certificados.
- **503** La limitación rechaza las solicitudes para evitar la sobrecarga del servidor. 
