---
title: "Configuración del registro de seguridad de la capa (TLS) de transporte"
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
author: justinha
ms.author: justinha
manager: brianlic-msft
ms.date: 08/07/2017
ms.openlocfilehash: 8ccfacc367a5d32438bebf22798479a07f6cbdfc
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="transport-layer-security-tls-registry-settings"></a>Configuración del registro de seguridad de la capa (TLS) de transporte

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016, Windows Server 2008 R2, Windows Server 2008, Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista

Este tema de referencia para profesionales de TI contiene información de configuración del registro compatibles para la implementación de Windows del protocolo de seguridad de la capa de transporte (TLS) y el protocolo de capa de Sockets seguros (SSL) a través de proveedor de soporte técnico de seguridad (SSP) de Schannel. Las subclaves de registro y las entradas que se tratan en este tema te ayudan a administra y solucionar problemas de SSP Schannel, específicamente los protocolos TLS y SSL. 

>[!Caution]
>Esta información se proporciona como una referencia a usar cuando se solución de problemas o comprobar que se ha aplicado la configuración necesaria. Te recomendamos que no modifica directamente el registro a menos que no haya ninguna otra alternativa.
>Modificaciones en el registro no se validan por el Editor del registro o el sistema operativo de Windows antes de aplicarlas. Como resultado, se pueden almacenar valores incorrectos, y esto puede provocar errores irrecuperables en el sistema. Cuando sea posible, en lugar de modificar el registro directamente, usa la directiva de grupo u otras herramientas de Windows, como Microsoft Management Console (MMC) para realizar tareas. Si debes editar el registro, Ten mucho cuidado. 

## <a name="certificatemappingmethods"></a>CertificateMappingMethods 

Esta entrada no existe en el registro de manera predeterminada. El valor predeterminado es que todos los métodos de asignación de certificados cuatro, indicados a continuación, se admiten.

Cuando una aplicación de servidor requiere autenticación de cliente, Schannel intenta automáticamente asignar el certificado que es proporcionado por el equipo cliente para una cuenta de usuario. Puede autenticar a los usuarios que inicia sesión con un certificado de cliente mediante la creación de asignaciones, que se relacionan con la información del certificado a una cuenta de usuario de Windows. Después de crear y habilitar una asignación de certificado, cada vez que un cliente presenta un certificado de cliente, la aplicación de servidor asocia automáticamente que el usuario con la cuenta de usuario de Windows apropiada.

En la mayoría de los casos, se asigna un certificado a una cuenta de usuario de una de dos maneras: 

- Un único certificado se asigna a una sola cuenta de usuario (asignación uno a uno).
- Varios certificados están asignados a una cuenta de usuario (asignación de varios a uno).

De forma predeterminada, el proveedor de Schannel usarán los siguientes métodos de cuatro asignación de certificados, que se enumeran en orden de preferencia de:

1. Asignación de certificados de Kerberos service-for-user (S4U)
2. Asignación del nombre principal de usuario
3. Asignación uno a uno (también conocida como asunto o emisor de asignación)
4. Asignación de muchos a uno

Versiones aplicables: como se indica en la **se aplica a** lista que está al principio de este tema.

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="ciphers"></a>Cifrados

Cifrado TLS/SSL debe controlarse mediante la configuración de la orden de conjuntos de cifrado. Para obtener más información, consulta [configurar un orden de conjuntos de cifrado de TLS](manage-tls.md#configuring-tls-cipher-suite-order).

Para obtener información sobre el orden de conjuntos de cifrado predeterminado que se usan el SSP Schannel, vea [conjuntos de cifrado de TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx). 

##<a name="ciphersuites"></a>Conjuntos cifrados

Configurar los conjuntos de cifrado TLS/SSL debe realizarse mediante la directiva de grupo, MDM o PowerShell, consulta [configurar un orden de conjuntos de cifrado de TLS](manage-tls.md#configuring-tls-cipher-suite-order) para obtener más información.

Para obtener información sobre el orden de conjuntos de cifrado predeterminado que se usan el SSP Schannel, vea [conjuntos de cifrado de TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx). 


## <a name="clientcachetime"></a>ClientCacheTime

Esta entrada controla la cantidad de tiempo que el sistema operativo tarda en milisegundos para que expire entradas de la caché del lado cliente. Un valor de 0 desactiva el almacenamiento en caché de conexión segura. Esta entrada no existe en el registro de manera predeterminada. 

La primera vez que un cliente se conecta a un servidor a través de SSP Schannel, una completa se realiza el protocolo de enlace TLS/SSL. Cuando esto finalice, el secreto principal, el conjunto de cifrado y los certificados se almacenan en la caché de la sesión en el servidor y cliente respectivo.

A partir de Windows Server 2008 y Windows Vista, la hora de caché de cliente predeterminada es 10 horas.

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

Hora de caché de cliente predeterminada

## <a name="fipsalgorithmpolicy"></a>FIPSAlgorithmPolicy

Esta entrada controla el cumplimiento de procesamiento Federal de información (FIPS). El valor predeterminado es 0.

Versiones aplicables: como se indica en la **se aplica a** lista que está al principio de este tema. 

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\LSA

Conjuntos de cifrado de Windows Server FIPS: consulta [conjuntos de cifrado compatibles y los protocolos en el SSP Schannel](https://technet.microsoft.com/library/dn786419.aspx).

## <a name="hashes"></a>Valores de hash

Algoritmos de hash TLS/SSL deben controlarse mediante la configuración de la orden de conjuntos de cifrado. Consulta [configurar un orden de conjuntos de cifrado de TLS](manage-tls.md#configuring-tls-cipher-suite-order) para obtener más información.

## <a name="issuercachesize"></a>IssuerCacheSize

Esta entrada controla el tamaño de la memoria caché del emisor, y se utiliza con la asignación del emisor. El SSP Schannel intentamos asignar todos los emisores de cadena de certificados del cliente, no solo el emisor directo del certificado de cliente. Cuando los emisores no se asignan a una cuenta, que es el caso típico, el servidor puede intentar asignar el mismo nombre de emisor repetidamente, cientos de veces por segundo. 

Para evitar esto, el servidor tiene una caché negativa, por lo que si no se asigna un nombre de emisor a una cuenta, se agrega a la memoria caché y el SSP Schannel no intentará asignar el nombre del emisor hasta que la entrada de caché expira. Esta entrada del registro especifica el tamaño de caché. Esta entrada no existe en el registro de manera predeterminada. El valor predeterminado es de 100. 

Versiones aplicables: como se indica en la **se aplica a** lista que está al principio de este tema.

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="issuercachetime"></a>IssuerCacheTime

Esta entrada controla la longitud del intervalo de tiempo de espera de la memoria caché en milisegundos. El SSP Schannel intentamos asignar todos los emisores de cadena de certificados del cliente, no solo el emisor directo del certificado de cliente. En el caso de que los emisores no asignarlo a una cuenta, que es el caso típico, el servidor puede intentar asignar el mismo nombre de emisor repetidamente, cientos de veces por segundo.

Para evitar esto, el servidor tiene una caché negativa, por lo que si no se asigna un nombre de emisor a una cuenta, se agrega a la memoria caché y el SSP Schannel no intentará asignar el nombre del emisor hasta que la entrada de caché expira. Esta caché se mantiene por motivos de rendimiento para que el sistema no continúa intentando la mismos emisores de mapa. Esta entrada no existe en el registro de manera predeterminada. El valor predeterminado es 10 minutos.

Versiones aplicables: como se indica en la **se aplica a** lista que está al principio de este tema.

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="keyexchangealgorithm---client-rsa-key-sizes"></a>Clave de cliente RSA KeyExchangeAlgorithm - tamaños

Esta entrada controla los tamaños de clave de RSA de cliente. 

Uso de los algoritmos de claves de intercambio debe controlarse mediante la configuración de la orden de conjuntos de cifrado.

Se agregó en Windows 10, versión 1507 y Windows Server 2016.

Ruta de acceso del registro: HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\KeyExchangeAlgorithms\PKCS

Para especificar una longitud de bits de la tecla de intervalo admitido mínimo de RSA para el cliente TLS, crear un **ClientMinKeyBitLength** entrada. Esta entrada no existe en el registro de manera predeterminada. Después de haber creado la entrada, cambiar el valor DWORD la longitud en bits deseado. Si no está configurado, 1024 bits será el mínimo. 

Para especificar una longitud de bits de la tecla de intervalo admitido máximo de RSA para el cliente TLS, crear un **ClientMaxKeyBitLength** entrada. Esta entrada no existe en el registro de manera predeterminada. Después de haber creado la entrada, cambiar el valor DWORD la longitud en bits deseado. Si no está configurado, no se aplica un máximo.

## <a name="keyexchangealgorithm---diffie-hellman-key-sizes"></a>KeyExchangeAlgorithm - tamaños de clave Diffie-Hellman

Esta entrada controla los tamaños de clave Diffie-Hellman. 

Uso de los algoritmos de claves de intercambio debe controlarse mediante la configuración de la orden de conjuntos de cifrado.

Se agregó en Windows 10, versión 1507 y Windows Server 2016.

Ruta de acceso del registro: HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\KeyExchangeAlgorithms\Diffie-Hellman

Para especificar una longitud en bits de clave mínimo compatible rango de Diffie-Helman para el cliente TLS, crear un **ClientMinKeyBitLength** entrada. Esta entrada no existe en el registro de manera predeterminada. Después de haber creado la entrada, cambiar el valor DWORD la longitud en bits deseado. Si no está configurado, 1024 bits será el mínimo. 
 
Para especificar una longitud en bits clave máximo admitido rango de Diffie-Helman para el cliente TLS, crear un **ClientMaxKeyBitLength** entrada. Esta entrada no existe en el registro de manera predeterminada. Después de haber creado la entrada, cambiar el valor DWORD la longitud en bits deseado. Si no está configurado, no se aplica un máximo. 
 
Para especificar la longitud en bits clave Diffie-Helman para el valor predeterminado de servidor TLS, crear un **ServerMinKeyBitLength** entrada. Esta entrada no existe en el registro de manera predeterminada. Después de haber creado la entrada, cambiar el valor DWORD la longitud en bits deseado. Si no se configura, 2048 bits será el valor predeterminado. 

## <a name="maximumcachesize"></a>MaximumCacheSize

Esta entrada controla el número máximo de elementos de la memoria caché. Establece en 0, MaximumCacheSize deshabilita la memoria caché de la sesión del servidor e impide que la reconexión. Aumenta MaximumCacheSize por encima de los valores predeterminados, Lsass.exe consumir memoria adicional. Normalmente, cada elemento de la caché de la sesión requiere 2a 4 KB de memoria. Esta entrada no existe en el registro de manera predeterminada. El valor predeterminado es 20.000 elementos. 

Versiones aplicables: como se indica en la **se aplica a** lista que está al principio de este tema.

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="messaging--fragment-parsing"></a>Mensajería: análisis de fragmentos

________________________________________
Esta entrada controla el tamaño máximo permitido de mensajes de protocolo de enlace TLS fragmentados que se aceptarán. No se aceptarán más grandes que el tamaño permitido de mensajes y se producirá un error en el protocolo de enlace TLS. Estas entradas no existen en el registro de manera predeterminada. 

Cuando se establece el valor en 0 x 0, mensajes fragmentados no se procesan y hará que el protocolo de enlace TLS un error. Esto hace que los clientes TLS o servidores en el equipo actual no compatible con las RFC TLS. 

El máximo permitido se puede aumentar el tamaño de hasta 2 ^ 1 de 24 bytes. Permitir que un cliente o servidor leer y almacenar grandes cantidades de datos no comprobados desde la red no es una buena idea y consume memoria adicional para cada contexto de seguridad. 

Se agrega en Windows 7 y Windows Server 2008 R2.
Hay una actualización que permite que Internet Explorer en Windows XP, en Windows Vista o en Windows Server 2008 para analizar los mensajes de protocolo de enlace TLS/SSL fragmentados.

Ruta de acceso del registro: HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Messaging

Para especificar un tamaño máximo permitido de mensajes de protocolo de enlace TLS fragmentados que el cliente TLS aceptará, crear un **MessageLimitClient** entrada. Después de haber creado la entrada, cambiar el valor DWORD la longitud en bits deseado. Si no se configura el valor predeterminado será 0 x 8000 bytes. 

Para especificar un tamaño máximo permitido de mensajes de protocolo de enlace TLS fragmentados que admita el servidor TLS cuando no hay ninguna autenticación de cliente, crear un **MessageLimitServer** entrada. Después de haber creado la entrada, cambiar el valor DWORD la longitud en bits deseado. Si no se configura el valor predeterminado será 0 x 4000 bytes. 

Para especificar un tamaño máximo permitido de mensajes de protocolo de enlace TLS fragmentados que admita el servidor TLS cuando se produce la autenticación de cliente, crear un **MessageLimitServerClientAuth** entrada. Después de haber creado la entrada, cambiar el valor DWORD la longitud en bits deseado. Si no se configura el valor predeterminado será 0 x 8000 bytes. 

## <a name="sendtrustedissuerlist"></a>SendTrustedIssuerList

Esta entrada controla la marca que se usa cuando se envía a la lista de emisores de confianza. En el caso de los servidores que confía en cientos de entidades de certificación para la autenticación de cliente, hay demasiados emisores del servidor para poder enviarlas todo en el equipo cliente cuando se solicita la autenticación de cliente. En esta situación, se puede establecer esta clave del registro y, en lugar de enviar una lista parcial, el SSP Schannel no enviará cualquier lista al cliente.

No enviar una lista de emisores de confianza puede afectar a lo que el cliente envía cuando se solicita un certificado de cliente. Por ejemplo, cuando Internet Explorer recibe una solicitud de autenticación de cliente, que solo muestren los certificados de cliente que encadenan hasta una de las entidades de certificación que se envía por el servidor. Si el servidor no envió una lista, Internet Explorer muestra todos los certificados de cliente que están instalados en el cliente. 

Este comportamiento puede ser conveniente. Por ejemplo, cuando los entornos de PKI incluyen certificados cruzados, los certificados de cliente y servidor no tendrán la misma raíz, CA; por lo tanto, Internet Explorer no puede elegir un certificado que se encadena hasta una de entidades emisoras de certificados del servidor. Al configurar el servidor para que no envíe una lista de emisor de confianza, Internet Explorer enviará todos los certificados.

Esta entrada no existe en el registro de manera predeterminada.

Comportamiento predeterminado de enviar la lista de confianza del emisor

| Versión de Windows | Tiempo |
|-----------------|------|
| Windows Server 2012 y Windows 8 y versiones posteriores | "FALSE" |
| Windows Server 2008 R2 y Windows 7 y versiones anteriores | TRUE |

Versiones aplicables: como se indica en la **se aplica a** lista que está al principio de este tema.

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="servercachetime"></a>ServerCacheTime

Esta entrada controla la cantidad de tiempo en milisegundos que el sistema operativo tarda en expiran entradas de la caché del lado servidor. Un valor de 0 deshabilita la caché de la sesión del servidor y evita la reconexión. Aumenta ServerCacheTime por encima de los valores predeterminados, Lsass.exe consumir memoria adicional. Normalmente, cada elemento de la memoria caché de sesión requiere 2a 4 KB de memoria. Esta entrada no existe en el registro de manera predeterminada. 

Versiones aplicables: como se indica en la **se aplica a** lista que está al principio de este tema.

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

Hora de memoria caché del servidor de forma predeterminada: 10 horas

## <a name="ssl-20"></a>SSL 2.0

Esta subclave controla el uso de SSL 2.0.

A partir de Windows 10, versión 1607 y Windows Server 2016, SSL 2.0 se ha quitado y ya no es compatible.
Para una configuración predeterminada de SSL 2.0, consulta [protocolos TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx). 

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para habilitar el protocolo SSL 2.0, crear una **habilitado** entrada en la subclave apropiada. Esta entrada no existe en el registro de manera predeterminada. Después de haber creado la entrada, cambiar el valor DWORD en 1. Para deshabilitar el protocolo, cambia el valor DWORD en 0.

Tabla de subclave SSL 2.0

| Subclave | Descripción |
|--------|-------------|
| Cliente | Controla el uso de SSL 2.0 en el cliente SSL. |
| Servidor | Controla el uso de SSL 2.0 en el servidor SSL. |
| DisabledByDefault | Marca esta opción para deshabilitar SSL 2.0 de manera predeterminada. |

## <a name="ssl-30"></a>SSL 3.0

Esta subclave controla el uso de SSL 3.0.

A partir de Windows 10, versión 1607 y Windows Server 2016, SSL 3.0 se ha deshabilitado de manera predeterminada. Para la configuración predeterminada de SSL 3.0, consulta [protocolos TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx). 

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para habilitar el protocolo SSL 3.0, crea una entrada habilitado en la subclave apropiada. Esta entrada no existe en el registro de manera predeterminada. Después de haber creado la entrada, cambiar el valor DWORD en 1. Para deshabilitar el protocolo, cambia el valor DWORD en 0.

Tabla de subclave SSL 3.0

| Subclave | Descripción |
|--------|-------------|
| Cliente | Controla el uso de SSL 3.0 en el cliente SSL. |
| Servidor | Controla el uso de SSL 3.0 en el servidor SSL. |
| DisabledByDefault | Marca esta opción para deshabilitar SSL 3.0 de manera predeterminada. |

## <a name="tls-10"></a>TLS 1.0

Esta subclave controla el uso de TLS 1.0.

Para la configuración predeterminada de TLS 1.0, consulta consulta [protocolos TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para deshabilitar el protocolo TLS 1.0, crear una **habilitado** entrada en la subclave apropiada. Esta entrada no existe en el registro de manera predeterminada. Después de haber creado la entrada, cambiar el valor DWORD en 0. Para habilitar el protocolo, cambia el valor DWORD en 1.

Tabla de subclave TLS 1.0

| Subclave | Descripción |
|--------|-------------|
| Cliente | Controla el uso de TLS 1.0 en el cliente TLS. |
| Servidor | Controla el uso de TLS 1.0 en el servidor TLS. |
| DisabledByDefault | Marca esta opción para deshabilitar TLS 1.0 de manera predeterminada. |

## <a name="tls-11"></a>TLS 1.1

Esta subclave controla el uso de TLS 1.1.

Para la configuración predeterminada de TLS 1.1, consulta [protocolos TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

>[!Note] 
>Para que TLS 1.1 esté activado y negocia en servidores que ejecutan Windows Server 2008 R2, debes crear la **DisabledByDefault** entrada en la subclave apropiada (cliente, servidor) y la establece en "0". La entrada no se verán en el registro y se ha establecido en "1" de manera predeterminada.

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para deshabilitar el protocolo TLS 1.1, crear una **habilitado** entrada en la subclave apropiada. Esta entrada no existe en el registro de manera predeterminada. Después de haber creado la entrada, cambiar el valor DWORD en 0. Para habilitar el protocolo, cambia el valor DWORD en 1.

Tabla de subclave TLS 1.1

| Subclave | Descripción |
|--------|-------------|
| Cliente | Controla el uso de TLS 1.1 en el cliente TLS. |
| Servidor | Controla el uso de TLS 1.1 en el servidor TLS. |
| DisabledByDefault | Marca esta opción para deshabilitar TLS 1.1 de manera predeterminada. |

## <a name="tls-12"></a>TLS 1.2

Esta subclave controla el uso de TLS 1.2.

Para la configuración predeterminada de TLS 1.2, consulta [protocolos TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

>[!Note] 
>Para que TLS 1.2 esté activado y negocia en servidores que ejecutan Windows Server 2008 R2, debes crear la **DisabledByDefault** entrada en la subclave apropiada (cliente, servidor) y la establece en "0". La entrada no se verán en el registro y se ha establecido en "1" de manera predeterminada.

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para deshabilitar el protocolo TLS 1.2, crear una **habilitado** entrada en la subclave apropiada. Esta entrada no existe en el registro de manera predeterminada. Después de haber creado la entrada, cambiar el valor DWORD en 0. Para habilitar el protocolo, cambia el valor DWORD en 1.

Tabla de subclave TLS 1.2

| Subclave | Descripción |
|--------|-------------|
| Cliente | Controla el uso de TLS 1.2 en el cliente TLS. |
| Servidor | Controla el uso de TLS 1.2 en el servidor TLS. |
| DisabledByDefault | Marca esta opción para deshabilitar TLS 1.2 de forma predeterminada. |

## <a name="dtls-10"></a>DTLS 1.0

Esta subclave controla el uso de DTLS 1.0.

Para la configuración predeterminada de DTLS 1.0, consulta [protocolos TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para deshabilitar el protocolo DTLS 1.0, crear una **habilitado** entrada en la subclave apropiada. Esta entrada no existe en el registro de manera predeterminada. Después de haber creado la entrada, cambiar el valor DWORD en 0. Para habilitar el protocolo, cambia el valor DWORD en 1.

Tabla de subclave DTLS 1.0

| Subclave | Descripción |
|--------|-------------|
| Cliente | Controla el uso de DTLS 1.0 en el cliente DTLS. |
| Servidor | Controla el uso de DTLS 1.0 en el servidor DTLS. |
| DisabledByDefault | Marca esta opción para deshabilitar DTLS 1.0 de manera predeterminada. |

## <a name="dtls-12"></a>DTLS 1.2

Esta subclave controla el uso de DTLS 1.2.

Para la configuración predeterminada de DTLS 1.2, consulta [protocolos TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para deshabilitar el protocolo DTLS 1.2, crear una **habilitado** entrada en la subclave apropiada. Esta entrada no existe en el registro de manera predeterminada. Después de haber creado la entrada, cambiar el valor DWORD en 0. Para habilitar el protocolo, cambia el valor DWORD en 1.

Tabla de subclave DTLS 1.2

| Subclave | Descripción |
|--------|-------------|
| Cliente | Controla el uso de DTLS 1.2 en el cliente DTLS. |
| Servidor | Controla el uso de DTLS 1.2 en el servidor DTLS. |
| DisabledByDefault | Marca esta opción para deshabilitar DTLS 1.2 de forma predeterminada. |

