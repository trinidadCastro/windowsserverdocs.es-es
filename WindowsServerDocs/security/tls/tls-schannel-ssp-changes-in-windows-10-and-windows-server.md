---
title: TLS (Schannel SSP)
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: ebd3c40c-b4c0-4f6d-a00c-f90eda4691df
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: 6e1a229bfc7063597f8994c71bbaec5637d084a4
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="tls-schannel-ssp-changes-in-windows-10-and-windows-server-2016"></a>Cambios TLS (Schannel SSP) en Windows 10 y Windows Server 2016

>Se aplica a: Windows Server 2016 y Windows 10

## <a name="cipher-suite-changes"></a>Cambios del conjunto de cifrado

Windows 10, versión 1511 y Windows Server 2016 agregan compatibilidad para la configuración de orden de conjuntos de cifrado con administración de dispositivos móviles (MDM).

Para cambios de orden de prioridad de conjunto de cifrado, consulta [conjuntos de cifrado en Schannel](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx).

Se agregó soporte técnico para los siguientes conjuntos de cifrado:

- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (RFC 5289) en Windows 10, versión 1507 y Windows Server 2016
- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (RFC 5289) en Windows 10, versión 1507 y Windows Server 2016

DisabledByDefault cambiar por los conjuntos de cifrados siguientes:

- TLS_DHE_DSS_WITH_AES_256_CBC_SHA256 (RFC 5246) en Windows 10, versión 1703
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA256 (RFC 5246) en Windows 10, versión 1703
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA (RFC 5246) en Windows 10, versión 1703
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA (RFC 5246) en Windows 10, versión 1703
- TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA (RFC 5246) en Windows 10, versión 1703
- TLS_RSA_WITH_RC4_128_SHA en Windows 10, versión 1709
- TLS_RSA_WITH_RC4_128_MD5 en Windows 10, versión 1709

A partir de Windows 10, versión 1507 y Windows Server 2016, se admiten SHA 512 certificados de manera predeterminada.

### <a name="rsa-key-changes"></a>Cambios importantes de RSA

Windows 10, versión 1507 y Windows Server 2016 agregan opciones de configuración del registro para tamaños de clave RSA de cliente.

Para obtener más información, consulta [KeyExchangeAlgorithm - tamaños de clave de cliente RSA](tls-registry-settings.md#keyexchangealgorithm---client-rsa-key-sizes).

### <a name="diffie-hellman-key-changes"></a>Cambios importantes Diffie-Hellman

Windows 10, versión 1507 y Windows Server 2016 agregan opciones de configuración del registro para tamaños de clave Diffie-Hellman.

Para obtener más información, consulta [KeyExchangeAlgorithm - tamaños de clave Diffie-Hellman](tls-registry-settings.md#keyexchangealgorithm---diffie-hellman-key-sizes).

### <a name="schusestrongcrypto-option-changes"></a>Cambios de la opción SCH_USE_STRONG_CRYPTO

Con Windows 10, versión 1507 y Windows Server 2016, [SCH_USE_STRONG_CRYPTO](https://msdn.microsoft.com/library/windows/desktop/aa379810.aspx) opción ahora deshabilita NULL, MD5, DES y exportar cifrados.

## <a name="elliptical-curve-changes"></a>Cambios de curva elípticas

Windows 10, versión 1507 y Windows Server 2016 agregan configuración de directiva de grupo para curvas elípticas en configuración del equipo > plantillas administrativas > red > Opciones de configuración de SSL. La lista de orden de la curva de ECC especifica el orden en el que se prefiere curvas elípticas así como permite curvas compatibles que no están habilitadas. 
 
Se agregó soporte técnico de las curvas elípticas siguientes:

- BrainpoolP256r1 (RFC 7027) en Windows 10, versión 1507 y Windows Server 2016
- BrainpoolP384r1 (RFC 7027) en Windows 10, versión 1507 y Windows Server 2016 
- BrainpoolP512r1 (RFC 7027) en Windows 10, versión 1507 y Windows Server 2016
- Curve25519 (RFC borrador-ietf-tls-curve25519) en Windows 10, versión 1607 y Windows Server 2016

## <a name="dispatch-level-support-for-sealmessage--unsealmessage"></a>Compatibilidad de niveles de distribución SealMessage & UnsealMessage

Windows 10, versión 1507 y Windows Server 2016 agregan compatibilidad para SealMessage/UnsealMessage en el nivel de distribución.

## <a name="dtls-12"></a>DTLS 1.2

Windows 10, versión 1607 y Windows Server 2016 agregan compatibilidad para DTLS 1.2 (RFC 6347).

## <a name="httpsys-thread-pool"></a>HTTP. Grupo de subprocesos del sistema 

Windows 10, versión 1607 y Windows Server 2016 agregan configuración de registro del tamaño del grupo de subprocesos que se usan para controlar los protocolos de enlace TLS HTTP. SYS.

Ruta de acceso del registro: 

HKLM\System\CurrentControlSet\Control\LSA

Para especificar un tamaño máximo de subprocesos del grupo por el núcleo de CPU, crear un **MaxAsyncWorkerThreadsPerCpu** entrada. Esta entrada no existe en el registro de manera predeterminada. Después de haber creado la entrada, cambiar el valor DWORD al tamaño deseado. Si no está configurado, el máximo es 2 subprocesos por el núcleo de CPU.

## <a name="next-protocol-negotiation-npn-support"></a>Siguiente soporte técnico de negociación del protocolo (NPN)

A partir de Windows 10 versión 1703, negociación de protocolo siguiente (NPN) se ha quitado y ya no es compatible.

## <a name="pre-shared-key-psk"></a>Clave precompartida (PSK)

Windows 10, versión 1607 y Windows Server 2016 agregan compatibilidad para el algoritmo de intercambio de claves de PSK (RFC 4279).

Se agregó soporte técnico para los conjuntos de cifrado PSK siguientes:

- TLS_PSK_WITH_AES_128_CBC_SHA256 (RFC 5487) en Windows 10, versión 1607 y Windows Server 2016
- TLS_PSK_WITH_AES_256_CBC_SHA384(RFC 5487) en Windows 10, versión 1607 y Windows Server 2016
- TLS_PSK_WITH_NULL_SHA256 (RFC 5487) en Windows 10, versión 1607 y Windows Server 2016
- TLS_PSK_WITH_NULL_SHA384 (RFC 5487) en Windows 10, versión 1607 y Windows Server 2016
- TLS_PSK_WITH_AES_128_GCM_SHA256 (RFC 5487) en Windows 10, versión 1607 y Windows Server 2016
- TLS_PSK_WITH_AES_256_GCM_SHA384 (RFC 5487) en Windows 10, versión 1607 y Windows Server 2016

## <a name="session-resumption-without-server-side-state-server-side-performance-improvements"></a>Reanudación de la sesión sin mejoras de rendimiento de servidor de estado en el servidor

Windows 10, versión 1507 y Windows Server 2016 proporcionan reanudaciones de sesión del 30% más por segundo vales de sesión en comparación con Windows Server 2012.

## <a name="session-hash-and-extended-master-secret-extension"></a>Hash de sesión y la extensión de la clave secreta de maestro extendida

Windows 10, versión 1507 y Windows Server 2016 agregan compatibilidad para RFC 7627: Hash de la sesión de seguridad de la capa de transporte (TLS) y la extensión de la clave secreta de maestro extendido.

Debido a este cambio, Windows 10 y Windows Server 2016 requiere parte 3 [proveedor CNG SSL](https://msdn.microsoft.com/library/windows/desktop/ff468652.aspx) actualizaciones para admitir NCRYPT_SSL_INTERFACE_VERSION_3 y para describir esta nueva interfaz.


## <a name="ssl-support"></a>Compatibilidad con SSL

A partir de Windows 10, versión 1607 y Windows Server 2016, el cliente TLS y el servidor SSL 3.0 está deshabilitado de manera predeterminada. Esto significa que, a menos que la aplicación o servicio solicite específicamente SSL 3.0 a través de la SSPI, el cliente nunca se ofrecen o Aceptar SSL 3.0 y el servidor nunca seleccionará SSL 3.0.

A partir de Windows 10, versión 1607 y Windows Server 2016, SSL 2.0 se ha quitado y ya no es compatible.

## <a name="changes-to-windows-tls-adherence-to-tls-12-requirements-for-connections-with-non-compliant-tls-clients"></a>Cambios en el cumplimiento de TLS Windows TLS 1.2 requisitos para conexiones con los clientes TLS no compatible

En TLS 1.2, el cliente usa la [extensión "signature_algorithms"](https://tools.ietf.org/html/rfc5246#section-7.4.1.4.1) para indicar al servidor que pares de algoritmo de firma o hash pueden usarse en las firmas digitales (es decir, el intercambio de claves de servidor y certificados de servidor). La solicitud de cambio de TLS 1.2 también requiere que el mensaje de certificado de servidor respetar la extensión "signature_algorithms":

"Si el cliente no proporciona una extensión de"signature_algorithms", a continuación, todos los certificados proporcionados por el servidor deben firmarse mediante un par de algoritmo de firma del hash que aparece en esa extensión."

En la práctica, algunos clientes TLS de terceros no cumplir con TLS 1.2 RFC y errores para incluir todos los la firma y hash pares de algoritmo están dispuestos a acepte en la extensión "signature_algorithms" u omite la extensión por completo (la segunda opción indica al servidor que el cliente solo admite SHA1 con RSA, DSA o ECDSA).

Un servidor TLS con frecuencia solo tiene un certificado que se configuran para cada extremo, lo que significa que el servidor no puede proporcionar siempre un certificado que cumpla los requisitos del cliente.

Antes de Windows 10 y Windows Server 2016, la pila de Windows TLS estrictamente se ha adherido a TLS requisitos de RFC 1.2, lo que provocaría errores de conexión con los clientes TLS no conformes RFC y problemas de interoperabilidad. En Windows 10 y Windows Server 2016, las restricciones son menos estrictas y el servidor pueda enviar un certificado que no cumplan con TLS 1.2 RFC, si esa es la única opción del servidor. El cliente, a continuación, se puede continuar o finalizar el protocolo de enlace.

Al validar certificados de cliente y servidor, la pila de Windows TLS estrictamente cumple con la solicitud de cambio de TLS 1.2 y solo permite la firma negociado y los algoritmos hash en los certificados de servidor y cliente.


