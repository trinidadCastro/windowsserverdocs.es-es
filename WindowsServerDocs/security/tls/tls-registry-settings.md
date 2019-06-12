---
title: Configuración del registro de seguridad de la capa (TLS) de transporte
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
ms.date: 02/28/2019
ms.openlocfilehash: 32068319aae7545675e126eed6e1ab4c914bcbcf
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812635"
---
# <a name="transport-layer-security-tls-registry-settings"></a>Configuración del registro de seguridad de la capa (TLS) de transporte

>Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows 10

Este tema de referencia para profesionales de TI contiene información de configuración del registro compatibles para la implementación de Windows del protocolo capa de transporte (TLS) y el protocolo de capa de Sockets seguros (SSL) a través de la compatibilidad de seguridad de Schannel Proveedor (SSP). Las subclaves del registro y las entradas que se trata en este tema de ayuda administrar y solucionar problemas de Schannel SSP, específicamente los protocolos TLS y SSL. 

> [!CAUTION]
> Esta información se proporciona como una referencia para usar cuando estés solucionando problemas o comprobando que se aplica la configuración necesaria.
> Te recomendamos que no modifiques directamente el registro a menos que no haya ninguna otra alternativa.
> El editor del registro y el sistema operativo de Windows no validan las modificaciones del registro antes de que se apliquen.
> Como resultado, se pueden almacenar valores incorrectos y esto puede producir errores irrecuperables en el sistema.
> Cuando sea posible, en lugar de modificar el registro directamente, usa la directiva de grupo u otras herramientas de Windows, como Microsoft Management Console (MMC) para realizar las tareas.
> Si tienes que modificar el registro, ten mucha precaución.

## <a name="certificatemappingmethods"></a>CertificateMappingMethods 

Esta entrada no existe en el registro de forma predeterminada. El valor predeterminado es que se admiten los cuatro métodos de asignación de certificados que se muestran a continuación.

Cuando una aplicación de servidor requiere la autenticación del cliente, SChannel intenta asignar automáticamente el certificado que suministró el equipo cliente a una cuenta de usuario. Puedes autenticar a los usuarios que inician sesión con un certificado de cliente creando asignaciones, que relacionan la información del certificado con una cuenta de usuario de Windows. Después de crear y habilitar una asignación de certificado, cada vez que un cliente presenta un certificado de cliente, la aplicación del servidor asocia automáticamente ese usuario a la cuenta de usuario de Windows correspondiente.

En la mayoría de los casos, un certificado se asigna a una cuenta de usuario de dos formas: 

- Se asigna un único certificado a una única cuenta de usuario (asignación uno a uno).
- Se asignan varios certificados a una única cuenta de usuario (asignación de varios a uno).

De forma predeterminada, el proveedor de Schannel usará los cuatro métodos de asignación de certificados siguientes, enumerados en orden de preferencia:

1. Asignación de certificados de Kerberos service-for-user (S4U)
2. Asignación de nombre principal de usuario
3. Asignación uno a uno (también conocida como asignación sujeto/emisor)
4. Asignación de varios a uno

Versiones aplicables: Como se indica en la lista **Aplicable a** que está al principio de este tema.

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="ciphers"></a>Ciphers

Cifrado TLS/SSL debe controlarse mediante la configuración de la orden de conjuntos de cifrado. Para obtener más información, consulte [configurar TLS Cipher Suite orden](manage-tls.md#configuring-tls-cipher-suite-order).

Para obtener información sobre el orden de los conjuntos de cifrado predeterminado que usa Schannel SSP, consulte [conjuntos de cifrado en TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx). 

## <a name="ciphersuites"></a>CipherSuites

Configurar conjuntos de cifrado TLS/SSL debe realizarse mediante la directiva de grupo, MDM o PowerShell, consulte [configurar TLS Cipher Suite orden](manage-tls.md#configuring-tls-cipher-suite-order) para obtener más información.

Para obtener información sobre el orden de los conjuntos de cifrado predeterminado que usa Schannel SSP, consulte [conjuntos de cifrado en TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx). 


## <a name="clientcachetime"></a>ClientCacheTime

Esta entrada controla la cantidad de tiempo en milisegundos que tarda el sistema operativo en expirar las entradas de caché del lado del cliente. Un valor de 0 desactiva la caché con conexión segura. Esta entrada no existe en el registro de forma predeterminada. 

La primera vez que se conecta un cliente a un servidor a través de Schannel SSP, se lleva a cabo un protocolo de enlace TLS/SSL completo. Cuando esto se completa, se almacenan en la caché de la sesión el secreto maestro, el conjunto de cifrado y los certificados, en su servidor y cliente respectivos.

A partir de Windows Server 2008 y Windows Vista, el tiempo de caché de cliente predeterminada es de 10 horas.

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

Tabla de tiempos de la caché de cliente predeterminada

## <a name="enableocspstaplingforsni"></a>EnableOcspStaplingForSni

Grapado de protocolo de estado de certificados (OCSP) en línea permite que un servidor web, servicios, como Internet Information Server (IIS), para proporcionar el estado actual de la revocación de un certificado de servidor cuando envía el certificado de servidor a un cliente durante el protocolo de enlace TLS. Esta característica reduce la carga en los servidores OCSP porque el servidor web puede almacenar en caché el estado actual de OCSP del certificado del servidor y enviarlo a varios clientes web. Sin esta característica, cada cliente web intentaría recuperar el estado actual de OCSP del certificado del servidor desde el servidor OCSP. Esto generaría una carga elevada en ese servidor OCSP. 

Además de IIS, los servicios web a través de http.sys también pueden beneficiarse de esta configuración, incluidos los servicios de federación de Active Directory (AD FS) y Proxy de aplicación Web (WAP). 

De forma predeterminada, está habilitada la compatibilidad con OCSP para sitios Web IIS que tienen un enlace seguro sencillo de (SSL/TLS). Sin embargo, esta compatibilidad no está habilitada de forma predeterminada, si el sitio Web de IIS está utilizando uno o ambos de los siguientes tipos de enlaces seguros (SSL/TLS):
- Requerir indicación de nombre de servidor
- Usar almacén de certificados centralizados

En este caso, la respuesta del servidor hello durante el protocolo de enlace TLS no incluirá un estado OCSP grapado de forma predeterminada. Este comportamiento mejora el rendimiento: La implementación de grapado de OCSP de Windows se puede escalar a cientos de certificados de servidor. Porque SNI y en CCS habilitar IIS escalar a miles de sitios Web que tienen potencialmente miles de certificados de servidor, al establecer este comportamiento esté habilitado de forma predeterminada, puede causar problemas de rendimiento.

Versiones aplicables: Todas las versiones a partir de Windows Server 2012 y Windows 8. 

Registry path: [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL]

Agregue la siguiente clave:

"EnableOcspStaplingForSni"=dword:00000001

Para deshabilitar, establezca el valor DWORD a 0:

"EnableOcspStaplingForSni"=dword:00000000

> [!NOTE] 
> La habilitación de esta clave del registro tiene un impacto del rendimiento.

## <a name="fipsalgorithmpolicy"></a>FIPSAlgorithmPolicy

Esta entrada controla el cumplimiento del Estándar federal de procesamiento de información (FIPS). El valor predeterminado es 0.

Versiones aplicables: Todas las versiones a partir de Windows Server 2012 y Windows 8. 

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\LSA

Windows Server FIPS conjuntos de cifrado: Consulte [admite conjuntos de cifrado y protocolos en el SSP de Schannel](https://technet.microsoft.com/library/dn786419.aspx).

## <a name="hashes"></a>Hashes

Los algoritmos hash TLS/SSL deben controlarse mediante la configuración de la orden de conjuntos de cifrado. Consulte [configurar TLS Cipher Suite orden](manage-tls.md#configuring-tls-cipher-suite-order) para obtener más información.

## <a name="issuercachesize"></a>IssuerCacheSize

Esta entrada controla el tamaño de la memoria caché del emisor y se usa con la asignación del emisor. Schannel SSP intenta asignar todos los emisores en la cadena de certificado del cliente, no solo el emisor directo del certificado de cliente. Cuando los emisores no se asignan a una cuenta, que es el caso típico, el servidor puede intentar asignar el mismo nombre de emisor una y otra vez, cientos de veces por segundo. 

Para evitar esto, el servidor tiene una memoria caché negativa, por lo que si un nombre de emisor no se asigna a una cuenta, se agrega a la memoria caché y Schannel SSP no intentará volver a asignar el nombre del emisor hasta que expire la entrada de caché. Esta entrada del registro especifica el tamaño de la caché. Esta entrada no existe en el registro de forma predeterminada. El valor predeterminado es 100. 

Versiones aplicables: Todas las versiones a partir de Windows Server 2008 y Windows Vista.

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="issuercachetime"></a>IssuerCacheTime

Esta entrada controla la longitud del intervalo de tiempo de espera de la caché en milisegundos. Schannel SSP intenta asignar todos los emisores en la cadena de certificado del cliente, no solo el emisor directo del certificado de cliente. En caso de que los emisores no se asignen a una cuenta, que es el caso típico, el servidor puede intentar asignar el mismo nombre de emisor una y otra vez, cientos de veces por segundo.

Para evitar esto, el servidor tiene una memoria caché negativa, por lo que si un nombre de emisor no se asigna a una cuenta, se agrega a la memoria caché y Schannel SSP no intentará volver a asignar el nombre del emisor hasta que expire la entrada de caché. Esta caché se mantiene por motivos de rendimiento, para que el sistema no siga intentando asignar a los mismos emisores. Esta entrada no existe en el registro de forma predeterminada. El valor predeterminado es 10 minutos.

Versiones aplicables: Todas las versiones a partir de Windows Server 2008 y Windows Vista.

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="keyexchangealgorithm---client-rsa-key-sizes"></a>KeyExchangeAlgorithm: tamaños de clave RSA del cliente

Esta entrada controla los tamaños de clave de RSA de cliente. 

Uso de algoritmos de intercambio de claves debe controlarse mediante la configuración de la orden de conjuntos de cifrado.

Se agregó en Windows 10, versión 1507 y Windows Server 2016.

Ruta de acceso del registro: HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\KeyExchangeAlgorithms\PKCS

Para especificar una longitud de bit de clave mínima intervalo admitido de RSA para el cliente TLS, cree un **ClientMinKeyBitLength** entrada. Esta entrada no existe en el registro de forma predeterminada. Después de haber creado la entrada, cambie el valor DWORD a la longitud en bits deseado. Si no está configurado, 1.024 bits será el mínimo. 

Para especificar una longitud de bit de clave máximo rango admitido de RSA para el cliente TLS, cree un **ClientMaxKeyBitLength** entrada. Esta entrada no existe en el registro de forma predeterminada. Después de haber creado la entrada, cambie el valor DWORD a la longitud en bits deseado. Si no está configurado, no se exige un máximo.

## <a name="keyexchangealgorithm---diffie-hellman-key-sizes"></a>KeyExchangeAlgorithm: tamaños de clave Diffie-Hellman

Esta entrada controla los tamaños de clave Diffie-Hellman. 

Uso de algoritmos de intercambio de claves debe controlarse mediante la configuración de la orden de conjuntos de cifrado.

Se agregó en Windows 10, versión 1507 y Windows Server 2016.

Ruta de acceso del registro: HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\KeyExchangeAlgorithms\Diffie-Hellman

Para especificar una longitud de bit de clave mínimo admitido intervalo de Diffie-Helman para el cliente TLS, cree un **ClientMinKeyBitLength** entrada. Esta entrada no existe en el registro de forma predeterminada. Después de haber creado la entrada, cambie el valor DWORD a la longitud en bits deseado. Si no está configurado, 1.024 bits será el mínimo. 
 
Para especificar una longitud de bit de clave máximo admitido intervalo de Diffie-Helman para el cliente TLS, cree un **ClientMaxKeyBitLength** entrada. Esta entrada no existe en el registro de forma predeterminada. Después de haber creado la entrada, cambie el valor DWORD a la longitud en bits deseado. Si no está configurado, no se exige un máximo. 
 
Para especificar la longitud en bits clave Diffie-Helman para el valor predeterminado de servidor TLS, cree un **ServerMinKeyBitLength** entrada. Esta entrada no existe en el registro de forma predeterminada. Después de haber creado la entrada, cambie el valor DWORD a la longitud en bits deseado. Si no está configurado, 2048 bits será el valor predeterminado. 

## <a name="maximumcachesize"></a>MaximumCacheSize

Esta entrada controla el número máximo de elementos almacenados en la caché. Al establecer MaximumCacheSize en 0 se deshabilita la caché de sesión del lado del servidor y se evita volver a conectarse. Al aumentar MaximumCacheSize por encima de los valores predeterminados, se hace que Lsass.exe consuma memoria adicional. Cada elemento de la caché de sesión normalmente requiere de 2 a 4 KB de memoria. Esta entrada no existe en el registro de forma predeterminada. El valor predeterminado es 20 000 elementos. 

Versiones aplicables: Todas las versiones a partir de Windows Server 2008 y Windows Vista.

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="messaging--fragment-parsing"></a>Messaging: análisis de fragmentos

________________________________________
Esta entrada controla el tamaño máximo permitido de mensajes de protocolo de enlace TLS fragmentados que va a Aceptar. No se aceptarán los mensajes que superen el tamaño permitido y se producirá un error en el protocolo de enlace TLS. Estas entradas no existen en el registro de forma predeterminada. 

Cuando se establece el valor en 0 x 0, los mensajes fragmentados no se procesan y hará que el protocolo de enlace TLS producirá un error. Esto hace que los clientes TLS o servidores en el equipo actual no conforme con las especificaciones de RFC de TLS. 

El máximo permitido se puede aumentar el tamaño de hasta 2 ^ 24-1 bytes. Permitir que un cliente o servidor leer y almacenar grandes cantidades de datos no comprobados de la red no es una buena idea y consumirá memoria adicional para cada contexto de seguridad. 

Se agregó en Windows 7 y Windows Server 2008 R2.
Hay disponible una actualización que permite a Internet Explorer en Windows XP, en Windows Vista o en Windows Server 2008 para analizar mensajes de protocolo de enlace TLS/SSL fragmentados.

Ruta de acceso del registro: HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Messaging

Para especificar un tamaño máximo permitido de mensajes de protocolo de enlace TLS fragmentados que aceptará el cliente TLS, cree un **MessageLimitClient** entrada. Después de haber creado la entrada, cambie el valor DWORD a la longitud en bits deseado. Si no está configurado, el valor predeterminado será 0 x 8000 bytes. 

Para especificar un tamaño máximo permitido de mensajes de protocolo de enlace TLS fragmentados que aceptará el servidor TLS cuando no hay ninguna autenticación de cliente, cree un **MessageLimitServer** entrada. Después de haber creado la entrada, cambie el valor DWORD a la longitud en bits deseado. Si no está configurado, el valor predeterminado será 0 x 4000 bytes. 

Para especificar un tamaño máximo permitido de mensajes de protocolo de enlace TLS fragmentados que aceptará el servidor TLS cuando no hay autenticación del cliente, cree un **MessageLimitServerClientAuth** entrada. Después de haber creado la entrada, cambie el valor DWORD a la longitud en bits deseado. Si no está configurado, el valor predeterminado será 0 x 8000 bytes. 

## <a name="sendtrustedissuerlist"></a>SendTrustedIssuerList

Esta entrada controla la marca que se usa cuando se envía la lista de emisores de confianza. En el caso de los servidores que confían en cientos de entidades de certificación para la autenticación del cliente, hay demasiados emisores para que el servidor pueda enviarlos todos al equipo cliente cuando se solicita la autenticación del cliente. En esta situación, se puede establecer esta clave del registro y en lugar de enviar una lista parcial, Schannel SSP no enviará ninguna lista al cliente.

El no enviar una lista de emisores de confianza puede afectar a lo que envía el cliente cuando se le solicita un certificado de cliente. Por ejemplo, cuando Internet Explorer recibe una solicitud de autenticación del cliente, solo muestra los certificados de cliente que encadenan hasta una de las entidades de certificación que envía el servidor. Si el servidor no envió una lista, Internet Explorer mostrará todos los certificados de cliente que están instalados del cliente. 

Este comportamiento puede ser conveniente. Por ejemplo, cuando los entornos PKI incluyen certificados cruzados, los certificados de cliente y servidor no tendrán la misma raíz de CA, por lo tanto, Internet Explorer no puede elegir un certificado que se encadene con una de las CA del servidor. Al configurar el servidor para que no envíe una lista de emisores de confianza, Internet Explorer enviará todos los certificados.

Esta entrada no existe en el registro de forma predeterminada.

Comportamiento de envío de lista de emisores de confianza predeterminado

| Versión de Windows | Time |
|-----------------|------|
| Windows Server 2012 y Windows 8 y versiones posteriores | FALSE |
| Windows Server 2008 R2 y Windows 7 y versiones anteriores | TRUE |

Versiones aplicables: Todas las versiones a partir de Windows Server 2008 y Windows Vista.

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="servercachetime"></a>ServerCacheTime

Esta entrada controla la cantidad de tiempo en milisegundos que tarda el sistema operativo en expirar las entradas de caché del lado del servidor. Un valor de 0 deshabilita la caché de sesión del lado del servidor y evita volver a conectarse. Al aumentar ServerCacheTime por encima de los valores predeterminados, se hace que Lsass.exe consuma memoria adicional. Cada elemento de la memoria caché de sesión normalmente requiere de 2 a 4 KB de memoria. Esta entrada no existe en el registro de forma predeterminada. 

Versiones aplicables: Todas las versiones a partir de Windows Server 2008 y Windows Vista.

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

Tiempo de caché del servidor predeterminado: 10 horas

## <a name="ssl-20"></a>SSL 2.0

Esta subclave controla el uso de SSL 2.0.

A partir de Windows 10, versión 1607 y Windows Server 2016, SSL 2.0 se ha quitado y ya no se admite.
Para una configuración predeterminada de SSL 2.0, consulte [protocolos en TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx). 

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para habilitar el protocolo SSL 2.0, cree un **habilitado** entrada en el cliente o servidor subclave, tal como se describe en la tabla siguiente. Esta entrada no existe en el registro de forma predeterminada. Después de haber creado la entrada, cambie el valor DWORD a 1. 

Tabla de subclaves de SSL 2.0

| Subclave | Descripción |
|--------|-------------|
| Remoto | Controla el uso de SSL 2.0 en el cliente SSL. |
| Servidor | Controla el uso de SSL 2.0 en el servidor SSL. |

Para deshabilitar SSL 2.0 para el cliente o servidor, cambie el valor DWORD a 0. Si se solicita una aplicación SSPI para usar SSL 2.0, se denegará. 

Para deshabilitar SSL 2.0 de forma predeterminada, crea un **DisabledByDefault** valor de entrada y cambie el valor DWORD a 1. Si se solicita una SSPI aplicación mediante la llamada explícita a usar SSL 2.0, es posible que se negociará. 

El ejemplo siguiente muestra deshabilitado en el registro de SSL 2.0:

![SSL 2.0 deshabilitado](images/ssl-2-registry-setting.png)


## <a name="ssl-30"></a>SSL 3.0

Esta subclave controla el uso de SSL 3.0.

A partir de Windows 10, versión 1607 y Windows Server 2016, SSL 3.0 se deshabilitó de forma predeterminada. Para la configuración predeterminada de SSL 3.0, consulte [protocolos en TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx). 

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para habilitar el protocolo SSL 3.0, cree un **habilitado** entrada en el cliente o servidor subclave, tal como se describe en la tabla siguiente.  
Esta entrada no existe en el registro de forma predeterminada. Después de haber creado la entrada, cambie el valor DWORD a 1. 

Tabla de subclaves de SSL 3.0

| Subclave | Descripción |
|--------|-------------|
| Remoto | Controla el uso de SSL 3.0 en el cliente SSL. |
| Servidor | Controla el uso de SSL 3.0 en el servidor SSL. |

Para deshabilitar SSL 3.0 de cliente o servidor, cambie el valor DWORD a 0.
Si se solicita una aplicación SSPI para utilizar SSL 3.0, se denegará. 

Para deshabilitar SSL 3.0 de forma predeterminada, crea un **DisabledByDefault** valor de entrada y cambie el valor DWORD a 1. Si una aplicación SSPI solicita explícitamente para usar SSL 3.0, es posible que se negociará. 

El ejemplo siguiente muestra deshabilitado en el registro de SSL 3.0:

![SSL 3.0 deshabilitado](images/ssl-3-registry-setting.png)

## <a name="tls-10"></a>TLS 1.0

Esta subclave controla el uso de TLS 1.0.

Para la configuración predeterminada de TLS 1.0, vea [protocolos en TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para habilitar el protocolo TLS 1.0, cree un **habilitado** entrada en el cliente o servidor subclave tal como se describe en la tabla siguiente. Esta entrada no existe en el registro de forma predeterminada. Después de haber creado la entrada, cambie el valor DWORD a 1. 

Tabla de subclaves de TLS 1.0

| Subclave | Descripción |
|--------|-------------|
| Remoto | Controla el uso de TLS 1.0 en el cliente TLS. |
| Servidor | Controla el uso de TLS 1.0 en el servidor TLS. |

Para deshabilitar TLS 1.0 para el cliente o servidor, cambie el valor DWORD a 0.
Si se solicita una aplicación SSPI para usar TLS 1.0, se denegará. 

Para deshabilitar TLS 1.0 de forma predeterminada, crea un **DisabledByDefault** valor de entrada y cambie el valor DWORD a 1. Si una aplicación SSPI solicita explícitamente para usar TLS 1.0, es posible que se negociará. 

El ejemplo siguiente muestra TLS 1.0 deshabilitada en el registro:

![TLS 1.0 deshabilitada](images/tls-registry-setting.png)

## <a name="tls-11"></a>TLS 1.1

Esta subclave controla el uso de TLS 1.1.

Para la configuración predeterminada de TLS 1.1, vea [protocolos en TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para habilitar el protocolo TLS 1.1, cree un **habilitado** entrada en el cliente o servidor subclave tal como se describe en la tabla siguiente. Esta entrada no existe en el registro de forma predeterminada. Después de haber creado la entrada, cambie el valor DWORD a 1. 

Tabla de subclaves de TLS 1.1

| Subclave | Descripción |
|--------|-------------|
| Remoto | Controla el uso de TLS 1.1 en el cliente TLS. |
| Servidor | Controla el uso de TLS 1.1 en el servidor TLS. |

Para deshabilitar TLS 1.1 para el cliente o servidor, cambie el valor DWORD a 0.
Si se solicita una aplicación SSPI para usar TLS 1.1, se denegará. 

Para deshabilitar TLS 1.1 de forma predeterminada, crea un **DisabledByDefault** valor de entrada y cambie el valor DWORD a 1. Si una aplicación SSPI solicita explícitamente para usar TLS 1.1, es posible que se negociará. 

El ejemplo siguiente muestra deshabilitado en el registro de TLS 1.1:

![TLS 1.1 deshabilitado](images/tls-11-registry-setting.png)

## <a name="tls-12"></a>TLS 1.2

Esta subclave controla el uso de TLS 1.2.

Para la configuración predeterminada de TLS 1.2, consulte [protocolos en TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para habilitar el protocolo TLS 1.2, cree un **habilitado** entrada en el cliente o servidor subclave tal como se describe en la tabla siguiente. Esta entrada no existe en el registro de forma predeterminada. Después de haber creado la entrada, cambie el valor DWORD a 1. 

Tabla de subclaves de TLS 1.2

| Subclave | Descripción |
|--------|-------------|
| Remoto | Controla el uso de TLS 1.2 en el cliente TLS. |
| Servidor | Controla el uso de TLS 1.2 en el servidor TLS. |

Para deshabilitar TLS 1.2 para el cliente o servidor, cambie el valor DWORD a 0.
Si se solicita una aplicación SSPI para usar TLS 1.2, se denegará. 

Para deshabilitar TLS 1.2 de forma predeterminada, crea un **DisabledByDefault** valor de entrada y cambie el valor DWORD a 1. Si una aplicación SSPI solicita explícitamente para usar TLS 1.2, es posible que se negociará. 

El ejemplo siguiente muestra deshabilitado en el registro de TLS 1.2:

![TLS 1.2 deshabilitado](images/tls-12-registry-setting.png)

## <a name="dtls-10"></a>DTLS 1.0

Esta subclave controla el uso de DTLS 1.0.

Para la configuración predeterminada de DTLS 1.0, vea [protocolos en TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para habilitar el protocolo DTLS 1.0, cree un **habilitado** entrada en el cliente o servidor subclave tal como se describe en la tabla siguiente. Esta entrada no existe en el registro de forma predeterminada. Después de haber creado la entrada, cambie el valor DWORD a 1. 

Tabla de subclaves de DTLS 1.0

| Subclave | Descripción |
|--------|-------------|
| Remoto | Controla el uso de DTLS 1.0 en el cliente DTLS. |
| Servidor | Controla el uso de DTLS 1.0 en el servidor DTLS. |

Para deshabilitar DTLS 1.0 para el cliente o servidor, cambie el valor DWORD a 0.
Si una aplicación SSPI de solicita para usar DTLS 1.0, se denegará. 

Para deshabilitar DTLS 1.0 de forma predeterminada, crea un **DisabledByDefault** valor de entrada y cambie el valor DWORD a 1. Si una aplicación SSPI solicita explícitamente para usar DTLS 1.0, es posible que se negociará. 

El ejemplo siguiente muestra 1.0 DTLS deshabilitado en el registro:

![1.0 DTLS deshabilitado](images/dtls-10-registry-setting.png)

## <a name="dtls-12"></a>DTLS 1.2

Esta subclave controla el uso de DTLS 1.2.

Para la configuración predeterminada de DTLS 1.2, consulte [protocolos en TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Ruta de acceso del registro: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Para habilitar el protocolo DTLS 1.2, cree un **habilitado** entrada en el cliente o servidor subclave tal como se describe en la tabla siguiente. Esta entrada no existe en el registro de forma predeterminada. Después de haber creado la entrada, cambie el valor DWORD a 1. 

Tabla de subclaves de DTLS 1.2

| Subclave | Descripción |
|--------|-------------|
| Remoto | Controla el uso de DTLS 1.2 en el cliente DTLS. |
| Servidor | Controla el uso de DTLS 1.2 en el servidor DTLS. |


Para deshabilitar DTLS 1.2 para el cliente o servidor, cambie el valor DWORD a 0.
Si una aplicación SSPI de solicita para usar DTLS 1.0, se denegará. 

Para deshabilitar DTLS 1.2 de forma predeterminada, crea un **DisabledByDefault** valor de entrada y cambie el valor DWORD a 1. Si una aplicación SSPI solicita explícitamente para usar DTLS 1.2, es posible que se negociará. 

El ejemplo siguiente muestra 1.1 DTLS deshabilitado en el registro:

![1.1 DTLS deshabilitado](images/dtls-11-registry-setting.png)


