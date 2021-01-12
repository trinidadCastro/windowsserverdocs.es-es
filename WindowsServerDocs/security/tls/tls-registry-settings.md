---
title: Configuración del Registro de Seguridad de la capa de transporte (TLS)
description: Obtenga información acerca de la configuración del registro compatible para la implementación de Windows del protocolo seguridad de la capa de transporte (TLS).
ms.topic: article
author: justinha
ms.author: justinha
manager: brianlic
ms.date: 02/28/2019
ms.openlocfilehash: 474b8d709fca920290459601e7c214d8167ff1a2
ms.sourcegitcommit: d42b80f947dbfa8660d982be67d77745a28081e5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2021
ms.locfileid: "98113401"
---
# <a name="transport-layer-security-tls-registry-settings"></a>Configuración del Registro de Seguridad de la capa de transporte (TLS)

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019, Windows Server 2016 y Windows 10

Este tema de referencia para profesionales de ti contiene información de configuración del registro compatible para la implementación de Windows del protocolo seguridad de la capa de transporte (TLS) y el protocolo Capa de sockets seguros (SSL) a través del proveedor de compatibilidad para seguridad de Schannel (SSP).
Las subclaves del registro y las entradas que se describen en este tema le ayudan a administrar y solucionar problemas de Schannel SSP, específicamente los protocolos TLS y SSL.

> [!CAUTION]
> Esta información se proporciona como una referencia para usar cuando estés solucionando problemas o comprobando que se aplica la configuración necesaria.
> Te recomendamos que no modifiques directamente el registro a menos que no haya ninguna otra alternativa.
> El editor del registro y el sistema operativo de Windows no validan las modificaciones del registro antes de que se apliquen.
> Como resultado, se pueden almacenar valores incorrectos y esto puede producir errores irrecuperables en el sistema.
> Cuando sea posible, en lugar de modificar el registro directamente, usa la directiva de grupo u otras herramientas de Windows, como Microsoft Management Console (MMC) para realizar las tareas.
> Si tienes que modificar el registro, ten mucha precaución.

## <a name="certificatemappingmethods"></a>CertificateMappingMethods

Esta entrada no existe en el registro de forma predeterminada.
El valor predeterminado es que se admiten los cuatro métodos de asignación de certificados que se muestran a continuación.

Cuando una aplicación de servidor requiere la autenticación del cliente, SChannel intenta asignar automáticamente el certificado que suministró el equipo cliente a una cuenta de usuario.
Puedes autenticar a los usuarios que inician sesión con un certificado de cliente creando asignaciones, que relacionan la información del certificado con una cuenta de usuario de Windows.
Después de crear y habilitar una asignación de certificado, cada vez que un cliente presenta un certificado de cliente, la aplicación del servidor asocia automáticamente ese usuario a la cuenta de usuario de Windows correspondiente.

En la mayoría de los casos, un certificado se asigna a una cuenta de usuario de dos formas:

- Se asigna un único certificado a una única cuenta de usuario (asignación uno a uno).
- Se asignan varios certificados a una única cuenta de usuario (asignación de varios a uno).

De forma predeterminada, el proveedor de Schannel usará los cuatro métodos de asignación de certificados siguientes, enumerados en orden de preferencia:

1. Asignación de certificados de Kerberos service-for-user (S4U)
2. Asignación de nombre principal de usuario
3. Asignación uno a uno (también conocida como asignación sujeto/emisor)
4. Asignación de varios a uno

Versiones aplicables: como se indica en la lista **se aplica a** que se encuentra al principio de este tema.

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="ciphers"></a>Ciphers

Los cifrados de TLS/SSL deben controlarse configurando el orden de los conjuntos de cifrado. Para obtener más información, consulte [configuración del orden del conjunto de cifrado TLS](manage-tls.md#configuring-tls-cipher-suite-order).

Para obtener información sobre el orden predeterminado de los conjuntos de cifrado que usa Schannel SSP, consulte [conjuntos de cifrado en TLS/SSL (Schannel SSP)](/windows/win32/secauthn/cipher-suites-in-schannel).

## <a name="ciphersuites"></a>CipherSuites

La configuración de conjuntos de cifrado TLS/SSL debe realizarse mediante la Directiva de grupo, MDM o PowerShell, consulte Configuración del orden de los conjuntos de [cifrado TLS](manage-tls.md#configuring-tls-cipher-suite-order) para obtener más información.

Para obtener información sobre el orden predeterminado de los conjuntos de cifrado que usa Schannel SSP, consulte [conjuntos de cifrado en TLS/SSL (Schannel SSP)](/windows/win32/secauthn/cipher-suites-in-schannel).


## <a name="clientcachetime"></a>ClientCacheTime

Esta entrada controla la cantidad de tiempo en milisegundos que tarda el sistema operativo en expirar las entradas de caché del lado del cliente.
Un valor de 0 desactiva la caché con conexión segura.
Esta entrada no existe en el registro de forma predeterminada.

La primera vez que se conecta un cliente a un servidor a través de Schannel SSP, se lleva a cabo un protocolo de enlace TLS/SSL completo.
Cuando esto se completa, se almacenan en la caché de la sesión el secreto maestro, el conjunto de cifrado y los certificados, en su servidor y cliente respectivos.

A partir de Windows Server 2008 y Windows Vista, la hora de la caché del cliente predeterminada es de 10 horas.

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

Tabla de tiempos de la caché de cliente predeterminada

## <a name="enableocspstaplingforsni"></a>EnableOcspStaplingForSni

El grapado del Protocolo de estado de certificados en línea (OCSP) permite que un servidor Web, como Internet Information Services (IIS), proporcione el estado de revocación actual de un certificado de servidor cuando envía el certificado de servidor a un cliente durante el protocolo de enlace de TLS.
Esta característica reduce la carga en los servidores OCSP porque el servidor web puede almacenar en caché el estado actual de OCSP del certificado de servidor y enviarlo a varios clientes Web.
Sin esta característica, cada cliente web intentaría recuperar el estado de OCSP actual del certificado de servidor del servidor OCSP.
Esto generaría una carga elevada en el servidor OCSP.

Además de IIS, los servicios web a través de http.sys también pueden beneficiarse de esta configuración, como Servicios de federación de Active Directory (AD FS) (AD FS) y el proxy de aplicación web (WAP).

De forma predeterminada, la compatibilidad con OCSP está habilitada para sitios web de IIS que tienen un enlace seguro simple (SSL/TLS).
Sin embargo, esta compatibilidad no está habilitada de forma predeterminada si el sitio web de IIS usa uno o ambos de los siguientes tipos de enlaces seguros (SSL/TLS):
- Requerir la indicación de nombre de servidor
- Usar almacén de certificados centralizados

En este caso, la respuesta de Hello del servidor durante el protocolo de enlace TLS no incluirá un estado de OCSP grapado de forma predeterminada.
Este comportamiento mejora el rendimiento: la implementación de grapado de OCSP de Windows se escala a cientos de certificados de servidor.
Dado que SNI y CCS permiten que IIS se escale a miles de sitios web que pueden tener miles de certificados de servidor, establecer este comportamiento en habilitado de forma predeterminada puede provocar problemas de rendimiento.

Versiones aplicables: todas las versiones a partir de Windows Server 2012 y Windows 8.

Ruta de acceso del registro: [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL]

Agregue la siguiente clave:

"EnableOcspStaplingForSni" = dword: 00000001

Para deshabilitar, establezca el valor DWORD en 0:

"EnableOcspStaplingForSni" = dword: 00000000

> [!NOTE]
> La habilitación de esta clave del registro tiene un impacto potencial en el rendimiento.

## <a name="fipsalgorithmpolicy"></a>FIPSAlgorithmPolicy

Esta entrada controla el cumplimiento del Estándar federal de procesamiento de información (FIPS).
El valor predeterminado es 0.

Versiones aplicables: todas las versiones a partir de Windows Server 2012 y Windows 8.

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\LSA

Conjuntos de cifrado FIPS de Windows Server: consulte [los conjuntos de cifrado y los protocolos admitidos en el SSP de Schannel](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn786419(v=ws.11)).

## <a name="hashes"></a>Códigos hash

Los algoritmos hash de TLS/SSL deben controlarse configurando el orden de los conjuntos de cifrado.
Consulte [configuración del orden del conjunto de cifrado TLS](manage-tls.md#configuring-tls-cipher-suite-order) para obtener más información.

## <a name="issuercachesize"></a>IssuerCacheSize

Esta entrada controla el tamaño de la memoria caché del emisor y se usa con la asignación del emisor.
Schannel SSP intenta asignar todos los emisores en la cadena de certificados del cliente, no solo el emisor directo del certificado de cliente.
Cuando los emisores no se asignan a una cuenta, que es el caso típico, el servidor puede intentar asignar el mismo nombre de emisor una y otra vez, cientos de veces por segundo.

Para evitar esto, el servidor tiene una memoria caché negativa, por lo que si un nombre de emisor no se asigna a una cuenta, se agrega a la memoria caché y Schannel SSP no intentará volver a asignar el nombre del emisor hasta que expire la entrada de caché.
Esta entrada del registro especifica el tamaño de la caché.
Esta entrada no existe en el registro de forma predeterminada.
El valor predeterminado es 100.

Versiones aplicables: todas las versiones a partir de Windows Server 2008 y Windows Vista.

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="issuercachetime"></a>IssuerCacheTime

Esta entrada controla la longitud del intervalo de tiempo de espera de la caché en milisegundos.
Schannel SSP intenta asignar todos los emisores en la cadena de certificados del cliente, no solo el emisor directo del certificado de cliente.
En caso de que los emisores no se asignen a una cuenta, que es el caso típico, el servidor puede intentar asignar el mismo nombre de emisor una y otra vez, cientos de veces por segundo.

Para evitar esto, el servidor tiene una memoria caché negativa, por lo que si un nombre de emisor no se asigna a una cuenta, se agrega a la memoria caché y Schannel SSP no intentará volver a asignar el nombre del emisor hasta que expire la entrada de caché.
Esta caché se mantiene por motivos de rendimiento, para que el sistema no siga intentando asignar a los mismos emisores.
Esta entrada no existe en el registro de forma predeterminada.
El valor predeterminado es 10 minutos.

Versiones aplicables: todas las versiones a partir de Windows Server 2008 y Windows Vista.

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="keyexchangealgorithm---client-rsa-key-sizes"></a>KeyExchangeAlgorithm: tamaños de clave RSA del cliente

Esta entrada controla los tamaños de clave RSA del cliente.

El uso de algoritmos de intercambio de claves se debe controlar configurando el orden de los conjuntos de cifrado.

Agregado en Windows 10, versión 1507 y Windows Server 2016.

Ruta de acceso del registro: HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\KeyExchangeAlgorithms\PKCS

Para especificar un intervalo mínimo admitido de longitud de bit de clave RSA para el cliente TLS, cree una entrada **ClientMinKeyBitLength** .
Esta entrada no existe en el registro de forma predeterminada.
Después de haber creado la entrada, cambie el valor DWORD a la longitud de bit deseada.
Si no se configura, 1024 bits será el mínimo.

Para especificar un intervalo máximo compatible de longitud de bit de clave RSA para el cliente TLS, cree una entrada **ClientMaxKeyBitLength** .
Esta entrada no existe en el registro de forma predeterminada.
Después de haber creado la entrada, cambie el valor DWORD a la longitud de bit deseada.
Si no se configura, no se aplica un máximo.

## <a name="keyexchangealgorithm---diffie-hellman-key-sizes"></a>Tamaños de clave de KeyExchangeAlgorithm-Diffie-Hellman

Esta entrada controla los tamaños de clave de Diffie-Hellman.

El uso de algoritmos de intercambio de claves se debe controlar configurando el orden de los conjuntos de cifrado.

Agregado en Windows 10, versión 1507 y Windows Server 2016.

Ruta de acceso del registro: HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\KeyExchangeAlgorithms\Diffie-Hellman

Para especificar un intervalo mínimo admitido de Diffie-Helman longitud de bit de clave para el cliente TLS, cree una entrada **ClientMinKeyBitLength** .
Esta entrada no existe en el registro de forma predeterminada.
Después de haber creado la entrada, cambie el valor DWORD a la longitud de bit deseada.
Si no se configura, 1024 bits será el mínimo.

Para especificar un intervalo máximo compatible de Diffie-Helman longitud de bit de clave para el cliente TLS, cree una entrada **ClientMaxKeyBitLength** .
Esta entrada no existe en el registro de forma predeterminada.
Después de haber creado la entrada, cambie el valor DWORD a la longitud de bit deseada.
Si no se configura, no se aplica un máximo.

Para especificar el Diffie-Helman longitud de bit de clave para el servidor TLS predeterminado, cree una entrada **ServerMinKeyBitLength** .
Esta entrada no existe en el registro de forma predeterminada.
Después de haber creado la entrada, cambie el valor DWORD a la longitud de bit deseada.
Si no se configura, el valor predeterminado es 2048 bits.

## <a name="maximumcachesize"></a>MaximumCacheSize

Esta entrada controla el número máximo de elementos almacenados en la caché.
Al establecer MaximumCacheSize en 0 se deshabilita la caché de sesión del lado del servidor y se evita volver a conectarse.
Al aumentar MaximumCacheSize por encima de los valores predeterminados, se hace que Lsass.exe consuma memoria adicional.
Normalmente, cada elemento de la caché de sesión requiere de 2 a 4 KB de memoria.
Esta entrada no existe en el registro de forma predeterminada.
El valor predeterminado es 20 000 elementos.

Versiones aplicables: todas las versiones a partir de Windows Server 2008 y Windows Vista.

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="messaging--fragment-parsing"></a>Mensajería: Análisis de fragmentos

________________________________________
Esta entrada controla el tamaño máximo permitido de los mensajes de protocolo de enlace TLS fragmentados que se aceptarán.
No se aceptarán mensajes mayores que el tamaño permitido y se producirá un error en el protocolo de enlace de TLS.
Estas entradas no existen en el registro de forma predeterminada.

Al establecer el valor en 0X0, no se procesan los mensajes fragmentados y se produce un error en el protocolo de enlace de TLS.
Esto hace que los clientes o servidores TLS de la máquina actual no sean compatibles con las RFC de TLS.

El tamaño máximo permitido puede aumentar hasta 2 ^ 24-1 bytes.
Permitir que un cliente o servidor Lea y almacene grandes cantidades de datos no comprobados de la red no es una buena idea y consumirá memoria adicional para cada contexto de seguridad.

Agregado en Windows 7 y Windows Server 2008 R2.
Hay disponible una actualización que permite a Internet Explorer en Windows XP, en Windows Vista o en Windows Server 2008 analizar mensajes de enlace TLS/SSL fragmentados.

Ruta de acceso del registro: HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Messaging

Para especificar un tamaño máximo permitido de mensajes de protocolo de enlace TLS fragmentados que el cliente TLS acepte, cree una entrada **MessageLimitClient** .
Después de haber creado la entrada, cambie el valor DWORD a la longitud de bit deseada.
Si no se configura, el valor predeterminado será 0x8000 bytes.

Para especificar un tamaño máximo permitido de mensajes de protocolo de enlace TLS fragmentados que el servidor TLS aceptará cuando no haya autenticación del cliente, cree una entrada **MessageLimitServer** .
Después de haber creado la entrada, cambie el valor DWORD a la longitud de bit deseada.
Si no se configura, el valor predeterminado será 0x4000 bytes.

Para especificar un tamaño máximo permitido de mensajes de protocolo de enlace TLS fragmentados que el servidor TLS aceptará cuando haya autenticación del cliente, cree una entrada **MessageLimitServerClientAuth** .
Después de haber creado la entrada, cambie el valor DWORD a la longitud de bit deseada.
Si no se configura, el valor predeterminado será 0x8000 bytes.

## <a name="sendtrustedissuerlist"></a>SendTrustedIssuerList

Esta entrada controla la marca que se usa cuando se envía la lista de emisores de confianza.
En el caso de los servidores que confían en cientos de entidades de certificación para la autenticación del cliente, hay demasiados emisores para que el servidor pueda enviarlos todos al equipo cliente cuando se solicita la autenticación del cliente.
En esta situación, se puede establecer esta clave del registro y en lugar de enviar una lista parcial, Schannel SSP no enviará ninguna lista al cliente.

El no enviar una lista de emisores de confianza puede afectar a lo que envía el cliente cuando se le solicita un certificado de cliente.
Por ejemplo, cuando Internet Explorer recibe una solicitud de autenticación del cliente, solo muestra los certificados de cliente que encadenan hasta una de las entidades de certificación que envía el servidor.
Si el servidor no envió una lista, Internet Explorer mostrará todos los certificados de cliente que están instalados del cliente.

Este comportamiento puede ser conveniente.
Por ejemplo, cuando los entornos PKI incluyen certificados cruzados, los certificados de cliente y servidor no tendrán la misma entidad de certificación raíz; por lo tanto, Internet Explorer no puede elegir un certificado que se encadene a una de las CA del servidor.
Al configurar el servidor para que no envíe una lista de emisores de confianza, Internet Explorer enviará todos los certificados.

Esta entrada no existe en el registro de forma predeterminada.

Comportamiento predeterminado de enviar lista de emisores de confianza

| Versión de Windows | Time |
|-----------------|------|
| Windows Server 2012 y Windows 8 y versiones posteriores | false |
| Windows Server 2008 R2 y Windows 7 y versiones anteriores | true |

Versiones aplicables: todas las versiones a partir de Windows Server 2008 y Windows Vista.

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="servercachetime"></a>ServerCacheTime

Esta entrada controla la cantidad de tiempo en milisegundos que tarda el sistema operativo en expirar las entradas de caché del lado del servidor.
Un valor de 0 deshabilita la caché de sesión del lado del servidor y evita volver a conectarse.
Al aumentar ServerCacheTime por encima de los valores predeterminados, se hace que Lsass.exe consuma memoria adicional.
Normalmente, cada elemento de la caché de sesión requiere de 2 a 4 KB de memoria.
Esta entrada no existe en el registro de forma predeterminada.

Versiones aplicables: todas las versiones a partir de Windows Server 2008 y Windows Vista.

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

Tiempo de caché de servidor predeterminado: 10 horas

## <a name="ssl-20"></a>SSL 2.0

Esta subclave controla el uso de SSL 2.0.

A partir de Windows 10, versión 1607 y Windows Server 2016, se ha quitado SSL 2,0 y ya no se admite.
Para obtener una configuración predeterminada de SSL 2,0, consulte [protocolos en TLS/SSL (Schannel SSP)](/windows/win32/secauthn/protocols-in-tls-ssl--schannel-ssp-).

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para habilitar el protocolo SSL 2,0, cree una entrada **habilitada** en la subclave Client o Server, tal y como se describe en la tabla siguiente.
Esta entrada no existe en el registro de forma predeterminada.
Después de haber creado la entrada, cambie el valor DWORD a 1.

Tabla de subclaves de SSL 2.0

| Subclave | Descripción |
|--------|-------------|
| Cliente | Controla el uso de SSL 2,0 en el cliente SSL. |
| Server | Controla el uso de SSL 2,0 en el servidor SSL. |

Para deshabilitar SSL 2,0 para el cliente o el servidor, cambie el valor DWORD a 0.
Si una aplicación SSPI solicita usar SSL 2,0, se le denegará.

Para deshabilitar SSL 2,0 de forma predeterminada, cree una entrada **DisabledByDefault** y cambie el valor DWORD a 1.
Si un explícita de aplicación SSPI solicita el uso de SSL 2,0, se puede negociar.

En el siguiente ejemplo se muestra SSL 2,0 deshabilitado en el registro:

![SSL 2,0 deshabilitado](images/ssl-2-registry-setting.png)


## <a name="ssl-30"></a>SSL 3.0

Esta subclave controla el uso de SSL 3.0.

A partir de Windows 10, versión 1607 y Windows Server 2016, SSL 3,0 se ha deshabilitado de forma predeterminada.
Para la configuración predeterminada de SSL 3,0, consulte [protocolos en TLS/SSL (Schannel SSP)](/windows/win32/secauthn/protocols-in-tls-ssl--schannel-ssp-).

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para habilitar el protocolo SSL 3,0, cree una entrada **habilitada** en la subclave Client o Server, tal y como se describe en la tabla siguiente.
Esta entrada no existe en el registro de forma predeterminada.
Después de haber creado la entrada, cambie el valor DWORD a 1.

Tabla de subclaves de SSL 3.0

| Subclave | Descripción |
|--------|-------------|
| Cliente | Controla el uso de SSL 3,0 en el cliente SSL. |
| Server | Controla el uso de SSL 3,0 en el servidor SSL. |

Para deshabilitar SSL 3,0 para el cliente o el servidor, cambie el valor DWORD a 0.
Si una aplicación SSPI solicita usar SSL 3,0, se le denegará.

Para deshabilitar SSL 3,0 de forma predeterminada, cree una entrada **DisabledByDefault** y cambie el valor DWORD a 1.
Si una aplicación SSPI solicita explícitamente el uso de SSL 3,0, se puede negociar.

En el siguiente ejemplo se muestra SSL 3,0 deshabilitado en el registro:

![SSL 3,0 deshabilitado](images/ssl-3-registry-setting.png)

## <a name="tls-10"></a>TLS 1.0

Esta subclave controla el uso de TLS 1.0.

Para obtener la configuración predeterminada de TLS 1,0, consulte [protocolos en TLS/SSL (Schannel SSP)](/windows/win32/secauthn/protocols-in-tls-ssl--schannel-ssp-).

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para habilitar el protocolo TLS 1,0, cree una entrada **habilitada** en la subclave Client o Server tal y como se describe en la tabla siguiente.
Esta entrada no existe en el registro de forma predeterminada.
Después de haber creado la entrada, cambie el valor DWORD a 1.

Tabla de subclaves de TLS 1.0

| Subclave | Descripción |
|--------|-------------|
| Cliente | Controla el uso de TLS 1,0 en el cliente TLS. |
| Server | Controla el uso de TLS 1,0 en el servidor TLS. |

Para deshabilitar TLS 1,0 para el cliente o el servidor, cambie el valor DWORD a 0.
Si una aplicación SSPI solicita usar TLS 1,0, se le denegará.

Para deshabilitar TLS 1,0 de forma predeterminada, cree una entrada **DisabledByDefault** y cambie el valor DWORD a 1.
Si una aplicación SSPI solicita explícitamente el uso de TLS 1,0, se puede negociar.

En el ejemplo siguiente se muestra TLS 1,0 deshabilitado en el registro:

![TLS 1.0 deshabilitado](images/tls-registry-setting.png)

## <a name="tls-11"></a>TLS 1.1

Esta subclave controla el uso de TLS 1.1.

Para obtener la configuración predeterminada de TLS 1,1, consulte [protocolos en TLS/SSL (Schannel SSP)](/windows/win32/secauthn/protocols-in-tls-ssl--schannel-ssp-).

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para habilitar el protocolo TLS 1,1, cree una entrada **habilitada** en la subclave Client o Server tal y como se describe en la tabla siguiente.
Esta entrada no existe en el registro de forma predeterminada.
Después de haber creado la entrada, cambie el valor DWORD a 1.

Tabla de subclaves de TLS 1.1

| Subclave | Descripción |
|--------|-------------|
| Cliente | Controla el uso de TLS 1,1 en el cliente TLS. |
| Server | Controla el uso de TLS 1,1 en el servidor TLS. |

Para deshabilitar TLS 1,1 para el cliente o el servidor, cambie el valor DWORD a 0.
Si una aplicación SSPI solicita usar TLS 1,1, se le denegará.

Para deshabilitar TLS 1,1 de forma predeterminada, cree una entrada **DisabledByDefault** y cambie el valor DWORD a 1.
Si una aplicación SSPI solicita explícitamente el uso de TLS 1,1, se puede negociar.

En el ejemplo siguiente se muestra TLS 1,1 deshabilitado en el registro:

![TLS 1,1 deshabilitado](images/tls-11-registry-setting.png)

## <a name="tls-12"></a>TLS 1.2

Esta subclave controla el uso de TLS 1.2.

Para obtener la configuración predeterminada de TLS 1,2, consulte [protocolos en TLS/SSL (Schannel SSP)](/windows/win32/secauthn/protocols-in-tls-ssl--schannel-ssp-).

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para habilitar el protocolo TLS 1,2, cree una entrada **habilitada** en la subclave Client o Server tal y como se describe en la tabla siguiente.
Esta entrada no existe en el registro de forma predeterminada.
Después de haber creado la entrada, cambie el valor DWORD a 1.

Tabla de subclaves de TLS 1.2

| Subclave | Descripción |
|--------|-------------|
| Cliente | Controla el uso de TLS 1,2 en el cliente TLS. |
| Server | Controla el uso de TLS 1,2 en el servidor TLS. |

Para deshabilitar TLS 1,2 para el cliente o el servidor, cambie el valor DWORD a 0.
Si una aplicación SSPI solicita usar TLS 1,2, se le denegará.

Para deshabilitar TLS 1,2 de forma predeterminada, cree una entrada **DisabledByDefault** y cambie el valor DWORD a 1.
Si una aplicación SSPI solicita explícitamente el uso de TLS 1,2, se puede negociar.

En el ejemplo siguiente se muestra TLS 1,2 deshabilitado en el registro:

![TLS 1,2 deshabilitado](images/tls-12-registry-setting.png)

## <a name="dtls-10"></a>DTLS 1.0

Esta subclave controla el uso de DTLS 1,0.

Para ver la configuración predeterminada de DTLS 1,0, consulte [protocolos en TLS/SSL (Schannel SSP)](/windows/win32/secauthn/protocols-in-tls-ssl--schannel-ssp-).

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para habilitar el protocolo DTLS 1,0, cree una entrada **habilitada** en la subclave Client o Server tal y como se describe en la tabla siguiente.
Esta entrada no existe en el registro de forma predeterminada.
Después de haber creado la entrada, cambie el valor DWORD a 1.

Tabla de subclave DTLS 1,0

| Subclave | Descripción |
|--------|-------------|
| Cliente | Controla el uso de DTLS 1,0 en el cliente de DTLS. |
| Server | Controla el uso de DTLS 1,0 en el servidor de DTLS. |

Para deshabilitar DTLS 1,0 para el cliente o el servidor, cambie el valor DWORD a 0.
Si una aplicación SSPI solicita que use DTLS 1,0, se le denegará.

Para deshabilitar DTLS 1,0 de forma predeterminada, cree una entrada **DisabledByDefault** y cambie el valor DWORD a 1.
Si una aplicación SSPI solicita explícitamente el uso de DTLS 1,0, se puede negociar.

En el ejemplo siguiente se muestra DTLS 1,0 deshabilitado en el registro:

![DTLS 1,0 deshabilitado](images/dtls-10-registry-setting.png)

## <a name="dtls-12"></a>DTLS 1,2

Esta subclave controla el uso de DTLS 1,2.

Para ver la configuración predeterminada de DTLS 1,2, consulte [protocolos en TLS/SSL (Schannel SSP)](/windows/win32/secauthn/protocols-in-tls-ssl--schannel-ssp-).

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para habilitar el protocolo DTLS 1,2, cree una entrada **habilitada** en la subclave Client o Server tal y como se describe en la tabla siguiente.
Esta entrada no existe en el registro de forma predeterminada.
Después de haber creado la entrada, cambie el valor DWORD a 1.

Tabla de subclave DTLS 1,2

| Subclave | Descripción |
|--------|-------------|
| Cliente | Controla el uso de DTLS 1,2 en el cliente de DTLS. |
| Server | Controla el uso de DTLS 1,2 en el servidor de DTLS. |


Para deshabilitar DTLS 1,2 para el cliente o el servidor, cambie el valor DWORD a 0.
Si una aplicación SSPI solicita que use DTLS 1,0, se le denegará.

Para deshabilitar DTLS 1,2 de forma predeterminada, cree una entrada **DisabledByDefault** y cambie el valor DWORD a 1.
Si una aplicación SSPI solicita explícitamente el uso de DTLS 1,2, se puede negociar.

En el ejemplo siguiente se muestra DTLS 1,1 deshabilitado en el registro:

![DTLS 1,1 deshabilitado](images/dtls-11-registry-setting.png)