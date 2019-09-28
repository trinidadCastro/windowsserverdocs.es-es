---
title: TLS (Schannel SSP)
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: ebd3c40c-b4c0-4f6d-a00c-f90eda4691df
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 05/16/2018
ms.openlocfilehash: e103e985592e6aed150ccd3e1a87e56f19621dbe
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403388"
---
# <a name="tls-schannel-ssp-changes-in-windows-10-and-windows-server-2016"></a>Cambios de TLS (Schannel SSP) en Windows 10 y Windows Server 2016

>Se aplica a: Windows Server (canal semianual), Windows Server 2016 y Windows 10

## <a name="cipher-suite-changes"></a>Cambios en el conjunto de cifrado

Windows 10, versión 1511 y Windows Server 2016 agregan compatibilidad para la configuración del orden de los conjuntos de cifrado mediante la administración de dispositivos móviles (MDM).

Para los cambios de orden de prioridad de conjunto de cifrado, consulte [conjuntos de cifrado en Schannel](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx).

Compatibilidad agregada para los siguientes conjuntos de cifrado:

- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (RFC 5289) en Windows 10, versión 1507 y Windows Server 2016
- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (RFC 5289) en Windows 10, versión 1507 y Windows Server 2016

DisabledByDefault cambio de los siguientes conjuntos de cifrado:

- TLS_DHE_DSS_WITH_AES_256_CBC_SHA256 (RFC 5246) en Windows 10, versión 1703
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA256 (RFC 5246) en Windows 10, versión 1703
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA (RFC 5246) en Windows 10, versión 1703
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA (RFC 5246) en Windows 10, versión 1703
- TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA (RFC 5246) en Windows 10, versión 1703
- TLS_RSA_WITH_RC4_128_SHA en Windows 10, versión 1709
- TLS_RSA_WITH_RC4_128_MD5 en Windows 10, versión 1709

A partir de Windows 10, se admiten los certificados SHA 512 de la versión 1507 y Windows Server 2016 de forma predeterminada.

### <a name="rsa-key-changes"></a>Cambios de clave RSA

Windows 10, versión 1507 y Windows Server 2016 agregar opciones de configuración del registro para los tamaños de clave RSA de cliente.

Para obtener más información, consulte [KeyExchangeAlgorithm-Client RSA Key sizes](tls-registry-settings.md#keyexchangealgorithm---client-rsa-key-sizes).

### <a name="diffie-hellman-key-changes"></a>Cambios en las claves Diffie-Hellman

Windows 10, versión 1507 y Windows Server 2016 agregan opciones de configuración del registro para tamaños de clave Diffie-Hellman.

Para obtener más información, consulte [KeyExchangeAlgorithm-Diffie-Hellman clave sizes](tls-registry-settings.md#keyexchangealgorithm---diffie-hellman-key-sizes).

### <a name="sch_use_strong_crypto-option-changes"></a>Cambios de la opción SCH_USE_STRONG_CRYPTO

Con Windows 10, versión 1507 y Windows Server 2016, la opción [SCH_USE_STRONG_CRYPTO](https://msdn.microsoft.com/library/windows/desktop/aa379810.aspx) ahora deshabilita los cifrados de los valores NULL, MD5, des y Export.

## <a name="elliptical-curve-changes"></a>Cambios de curva elíptica

Windows 10, versión 1507 y Windows Server 2016 agregan directiva de grupo configuración para las curvas elípticas en configuración del equipo > Plantillas administrativas opciones de configuración de > de red > SSL. La lista de pedidos de curva ECC especifica el orden en el que se prefieren las curvas elípticas, además de habilitar las curvas admitidas que no están habilitadas. 
 
Se ha agregado compatibilidad con las siguientes curvas elípticas:

- BrainpoolP256r1 (RFC 7027) en Windows 10, versión 1507 y Windows Server 2016
- BrainpoolP384r1 (RFC 7027) en Windows 10, versión 1507 y Windows Server 2016 
- BrainpoolP512r1 (RFC 7027) en Windows 10, versión 1507 y Windows Server 2016
- Curve25519 (borrador RFC-IETF-TLS-Curve25519) en Windows 10, versión 1607 y Windows Server 2016

## <a name="dispatch-level-support-for-sealmessage--unsealmessage"></a>Compatibilidad de nivel de envío con SealMessage & UnsealMessage

Windows 10, versión 1507 y Windows Server 2016 agregan compatibilidad para SealMessage/UnsealMessage en el nivel de envío.

## <a name="dtls-12"></a>DTLS 1,2

Windows 10, versión 1607 y Windows Server 2016 agregan compatibilidad con DTLS 1,2 (RFC 6347).

## <a name="httpsys-thread-pool"></a>HTTP. Grupo de subprocesos SYS 

Windows 10, versión 1607 y Windows Server 2016 agregan la configuración del registro del tamaño del grupo de subprocesos utilizado para administrar los protocolos de enlace TLS para HTTP. Sist.

Ruta de acceso del registro: 

HKLM\SYSTEM\CurrentControlSet\Control\LSA

Para especificar un tamaño máximo de grupo de subprocesos por núcleo de CPU, cree una entrada **MaxAsyncWorkerThreadsPerCpu** . Esta entrada no existe en el registro de forma predeterminada. Después de haber creado la entrada, cambie el valor DWORD al tamaño deseado. Si no se configura, el máximo es de 2 subprocesos por núcleo de CPU.

## <a name="next-protocol-negotiation-npn-support"></a>Compatibilidad con la negociación de protocolo siguiente (NPN)

A partir de la versión 1703 de Windows 10, la negociación de protocolo siguiente (NPN) se ha quitado y ya no se admite.

## <a name="pre-shared-key-psk"></a>Clave previamente compartida (PSK)

Windows 10, versión 1607 y Windows Server 2016 agregan compatibilidad con el algoritmo de intercambio de claves PSK (RFC 4279).

Compatibilidad agregada para los siguientes conjuntos de cifrado de PSK:

- TLS_PSK_WITH_AES_128_CBC_SHA256 (RFC 5487) en Windows 10, versión 1607 y Windows Server 2016
- TLS_PSK_WITH_AES_256_CBC_SHA384 (RFC 5487) en Windows 10, versión 1607 y Windows Server 2016
- TLS_PSK_WITH_NULL_SHA256 (RFC 5487) en Windows 10, versión 1607 y Windows Server 2016
- TLS_PSK_WITH_NULL_SHA384 (RFC 5487) en Windows 10, versión 1607 y Windows Server 2016
- TLS_PSK_WITH_AES_128_GCM_SHA256 (RFC 5487) en Windows 10, versión 1607 y Windows Server 2016
- TLS_PSK_WITH_AES_256_GCM_SHA384 (RFC 5487) en Windows 10, versión 1607 y Windows Server 2016

## <a name="session-resumption-without-server-side-state-server-side-performance-improvements"></a>Reanudación de la sesión sin mejoras en el rendimiento del servidor de estado del servidor

Windows 10, versión 1507 y Windows Server 2016 proporcionan un 30% más de reanudaciones de sesión por segundo con vales de sesión en comparación con Windows Server 2012.

## <a name="session-hash-and-extended-master-secret-extension"></a>Hash de sesión y extensión secreta maestra extendida

Windows 10, versión 1507 y Windows Server 2016 agregan compatibilidad con RFC 7627: Hash de sesión de seguridad de la capa de transporte (TLS) y extensión de secreto principal extendido.

Debido a este cambio, Windows 10 y Windows Server 2016 requieren actualizaciones de [proveedor de SSL de CNG](https://msdn.microsoft.com/library/windows/desktop/ff468652.aspx) de terceros para admitir NCRYPT_SSL_INTERFACE_VERSION_3 y para describir esta nueva interfaz.


## <a name="ssl-support"></a>Compatibilidad con SSL

A partir de Windows 10, versión 1607 y Windows Server 2016, el cliente TLS y el servidor SSL 3,0 están deshabilitados de forma predeterminada. Esto significa que, a menos que la aplicación o el servicio solicite específicamente SSL 3,0 a través de la SSPI, el cliente nunca ofrecerá o aceptará SSL 3,0 y el servidor nunca seleccionará SSL 3,0.

A partir de la versión 1607 y Windows Server 2016 de Windows 10, se ha quitado SSL 2,0 y ya no se admite.

## <a name="changes-to-windows-tls-adherence-to-tls-12-requirements-for-connections-with-non-compliant-tls-clients"></a>Cambios en la adherencia de Windows TLS a los requisitos de TLS 1,2 para las conexiones con clientes TLS no compatibles

En TLS 1,2, el cliente usa la [extensión "signature_algorithms"](https://tools.ietf.org/html/rfc5246#section-7.4.1.4.1) para indicar al servidor qué pares de algoritmos de firma y hash se pueden usar en firmas digitales (es decir, certificados de servidor e intercambio de claves de servidor). La RFC 1,2 de TLS también requiere que el mensaje de certificado de servidor respete la extensión "signature_algorithms":

"Si el cliente proporcionó una extensión" signature_algorithms ", todos los certificados proporcionados por el servidor deben estar firmados por un par de algoritmos hash/firma que aparezca en esa extensión".

En la práctica, algunos clientes TLS de terceros no cumplen con la RFC 1,2 de TLS y no incluyen todos los pares firmados y algoritmos hash que están dispuestos a aceptar en la extensión "signature_algorithms" u omiten la extensión por completo (el último indica a servidor que el cliente solo admite SHA1 con RSA, DSA o ECDSA.

A menudo, un servidor TLS solo tiene un certificado configurado por extremo, lo que significa que el servidor no siempre puede proporcionar un certificado que cumpla los requisitos del cliente.

Antes de Windows 10 y Windows Server 2016, la pila de Windows TLS se adhiera estrictamente a los requisitos RFC 1,2 de TLS, lo que produce errores de conexión con clientes TLS no compatibles con RFC y problemas de interoperabilidad. En Windows 10 y Windows Server 2016, las restricciones se relajan y el servidor puede enviar un certificado que no cumple con la RFC 1,2 de TLS, si esta es la única opción del servidor. Después, el cliente puede continuar o finalizar el protocolo de enlace.

Al validar los certificados de servidor y de cliente, la pila de Windows TLS cumple estrictamente con la RFC 1,2 de TLS y solo permite la firma negociada y los algoritmos hash en los certificados de cliente y servidor.


