---
title: TLS (Schannel SSP)
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: ebd3c40c-b4c0-4f6d-a00c-f90eda4691df
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 05/16/2018
ms.openlocfilehash: 030fd81e0c6ba0423f1fa73e680006766cf2b180
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890776"
---
# <a name="tls-schannel-ssp-changes-in-windows-10-and-windows-server-2016"></a>Cambios TLS (Schannel SSP) en Windows 10 y Windows Server 2016

>Se aplica a: Windows Server (canal semianual), Windows Server 2016 y Windows 10

## <a name="cipher-suite-changes"></a>Cambios del conjunto de cifrado

Windows 10, versión 1511 y Windows Server 2016 agregan compatibilidad para la configuración de orden de conjuntos de cifrado mediante administración de dispositivos móviles (MDM).

Para los cambios de orden de prioridad de cifrado suite, consulte [conjuntos de cifrado en Schannel](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx).

Se agregó compatibilidad para los siguientes conjuntos de cifrado:

- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (RFC 5289) en Windows 10, versión 1507 y Windows Server 2016
- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (RFC 5289) en Windows 10, versión 1507 y Windows Server 2016

DisabledByDefault cambiar para los siguientes conjuntos de cifrado:

- TLS_DHE_DSS_WITH_AES_256_CBC_SHA256 (RFC 5246) en Windows 10, versión 1703
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA256 (RFC 5246) en Windows 10, versión 1703
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA (RFC 5246) en Windows 10, versión 1703
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA (RFC 5246) en Windows 10, versión 1703
- TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA (RFC 5246) en Windows 10, versión 1703
- TLS_RSA_WITH_RC4_128_SHA en Windows 10, versión 1709
- TLS_RSA_WITH_RC4_128_MD5 en Windows 10, versión 1709

A partir de Windows 10, versión 1507 y Windows Server 2016, se admiten certificados de SHA 512 de forma predeterminada.

### <a name="rsa-key-changes"></a>Cambios de clave RSA

Windows 10, versión 1507 y Windows Server 2016 agregan opciones de configuración del registro para los tamaños de clave RSA de cliente.

Para obtener más información, consulte [KeyExchangeAlgorithm: tamaños de clave RSA de cliente](tls-registry-settings.md#keyexchangealgorithm---client-rsa-key-sizes).

### <a name="diffie-hellman-key-changes"></a>Cambios de clave Diffie-Hellman

Windows 10, versión 1507 y Windows Server 2016 agregan opciones de configuración del registro para los tamaños de clave Diffie-Hellman.

Para obtener más información, consulte [KeyExchangeAlgorithm: tamaños de clave Diffie-Hellman](tls-registry-settings.md#keyexchangealgorithm---diffie-hellman-key-sizes).

### <a name="schusestrongcrypto-option-changes"></a>Cambios de la opción SCH_USE_STRONG_CRYPTO

Con Windows 10, versión 1507 y Windows Server 2016, [SCH_USE_STRONG_CRYPTO](https://msdn.microsoft.com/library/windows/desktop/aa379810.aspx) opción ahora deshabilita NULL, MD5, DES y exportar cifrados.

## <a name="elliptical-curve-changes"></a>Cambios de curva elípticos

Windows 10, versión 1507 y Windows Server 2016 agregan configuración de directiva de grupo para curvas elípticos en configuración del equipo > plantillas administrativas > red > Opciones de configuración de SSL. La lista de pedidos de la curva de ECC especifica el orden en el que se prefieren curvas elípticos así como permite curvas compatibles que no están habilitadas. 
 
Se agregó compatibilidad para las curvas elípticos siguientes:

- BrainpoolP256r1 (RFC 7027) en Windows 10, versión 1507 y Windows Server 2016
- BrainpoolP384r1 (RFC 7027) en Windows 10, versión 1507 y Windows Server 2016 
- BrainpoolP512r1 (RFC 7027) en Windows 10, versión 1507 y Windows Server 2016
- Curve25519 (RFC draft-ietf-tls-curve25519) en Windows 10, versión 1607 y Windows Server 2016

## <a name="dispatch-level-support-for-sealmessage--unsealmessage"></a>Compatibilidad de nivel de envío para SealMessage & UnsealMessage

Windows 10, versión 1507 y Windows Server 2016 agregar compatibilidad con SealMessage/UnsealMessage en el nivel de envío.

## <a name="dtls-12"></a>DTLS 1.2

Windows 10, versión 1607 y Windows Server 2016 agregan compatibilidad para DTLS 1.2 (RFC 6347).

## <a name="httpsys-thread-pool"></a>HTTP. Grupo de subprocesos SYS 

Windows 10, versión 1607 y Windows Server 2016 agregan la configuración del registro del tamaño del grupo de subprocesos usado para controlar los protocolos de enlace TLS para HTTP. SYS.

Ruta de acceso del registro: 

HKLM\SYSTEM\CurrentControlSet\Control\LSA

Para especificar un tamaño máximo de subprocesos del grupo por núcleo de CPU, cree un **MaxAsyncWorkerThreadsPerCpu** entrada. Esta entrada no existe en el registro de forma predeterminada. Después de haber creado la entrada, cambie el valor DWORD en el tamaño deseado. Si no está configurado, el máximo es 2 subprocesos por núcleo de CPU.

## <a name="next-protocol-negotiation-npn-support"></a>Soporte técnico de negociación de protocolo (NPN) siguiente

A partir de Windows 10 versión 1703, el siguiente protocolo de negociación (NPN) se ha quitado y ya no se admite.

## <a name="pre-shared-key-psk"></a>Clave precompartida (PSK)

Windows 10, versión 1607 y Windows Server 2016 agregan compatibilidad para el algoritmo de intercambio de claves PSK (RFC 4279).

Se agregó compatibilidad para los conjuntos de cifrado PSK siguientes:

- TLS_PSK_WITH_AES_128_CBC_SHA256 (RFC 5487) en Windows 10, versión 1607 y Windows Server 2016
- TLS_PSK_WITH_AES_256_CBC_SHA384(RFC 5487) en Windows 10, versión 1607 y Windows Server 2016
- TLS_PSK_WITH_NULL_SHA256 (RFC 5487) en Windows 10, versión 1607 y Windows Server 2016
- TLS_PSK_WITH_NULL_SHA384 (RFC 5487) en Windows 10, versión 1607 y Windows Server 2016
- TLS_PSK_WITH_AES_128_GCM_SHA256 (RFC 5487) en Windows 10, versión 1607 y Windows Server 2016
- TLS_PSK_WITH_AES_256_GCM_SHA384 (RFC 5487) en Windows 10, versión 1607 y Windows Server 2016

## <a name="session-resumption-without-server-side-state-server-side-performance-improvements"></a>Reanudación de la sesión sin mejoras de rendimiento del servidor de estado del lado servidor

Windows 10, versión 1507 y Windows Server 2016 proporcionan 30% más reanudaciones de sesión por segundo con vales de sesión en comparación con Windows Server 2012.

## <a name="session-hash-and-extended-master-secret-extension"></a>Hash de la sesión y extensión de secreto maestro extendido

Windows 10, versión 1507 y Windows Server 2016 agregan compatibilidad para RFC 7627: Hash de sesión TLS (seguridad) de capa de transporte y extender la extensión de secreto maestro.

Debido a este cambio, Windows 10 y Windows Server 2016 requiere parte 3ª [proveedor CNG SSL](https://msdn.microsoft.com/library/windows/desktop/ff468652.aspx) actualizaciones para admitir NCRYPT_SSL_INTERFACE_VERSION_3 y para describir esta nueva interfaz.


## <a name="ssl-support"></a>Compatibilidad con SSL

A partir de Windows 10, versión 1607 y Windows Server 2016, el cliente TLS y el servidor SSL 3.0 está deshabilitado de forma predeterminada. Esto significa que, a menos que la aplicación o servicio solicita específicamente SSL 3.0 a través de la SSPI, el cliente nunca se ofrecen o aceptan SSL 3.0 y el servidor nunca seleccionará SSL 3.0.

A partir de Windows 10 versión 1607 y Windows Server 2016, SSL 2.0 se ha quitado y ya no se admite.

## <a name="changes-to-windows-tls-adherence-to-tls-12-requirements-for-connections-with-non-compliant-tls-clients"></a>Cambios realizados en el cumplimiento de Windows TLS requisitos de TLS 1.2 para las conexiones con clientes TLS que no son compatibles

En TLS 1.2, el cliente utiliza el ["signature_algorithms" extensión](https://tools.ietf.org/html/rfc5246#section-7.4.1.4.1) para indicar al servidor qué pares de algoritmo de firma o hash pueden usarse en las firmas digitales (es decir, los certificados de servidor e intercambio de claves de servidor). La RFC de TLS 1.2 también requiere que el mensaje de certificado de servidor respetar la extensión "signature_algorithms":

"Si el cliente proporciona una extensión"signature_algorithms", a continuación, todos los certificados proporcionados por el servidor deben estar firmados por un par de algoritmo hash y firma que aparece en esa extensión."

En la práctica, algunos clientes TLS de terceros no cumplir con la RFC de TLS 1.2 y no incluir todas la firma y hash de pares de algoritmos que están dispuestos a Aceptar en la extensión "signature_algorithms" u omite por completo la extensión (estos últimos indican a el servidor que el cliente solo admite SHA1 con RSA, DSA o ECDSA).

Un servidor TLS a menudo solo tiene un certificado configurado por el punto de conexión, lo que significa que el servidor no puede proporcionar siempre un certificado que cumpla los requisitos del cliente.

Antes de Windows 10 y Windows Server 2016, la pila TLS Windows minuciosamente TLS 1.2 requisitos de RFC, lo que produce errores de conexión con los clientes TLS que no es compatible con RFC y problemas de interoperabilidad. En Windows 10 y Windows Server 2016, las restricciones son menos estrictos y el servidor puede enviar un certificado que no cumple con RFC de TLS 1.2, si eso es la única opción del servidor. El cliente, a continuación, puede continuar o finalizar el protocolo de enlace.

Al validar los certificados de cliente y servidor, la pila TLS Windows estrictamente cumple con la RFC de TLS 1.2 y solo permite los algoritmos de hash y firma negociado los certificados de cliente y servidor.


