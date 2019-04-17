---
title: "TLS - Introducción SSL (Schannel SSP)"
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c8836345-16bb-4dcc-8d2b-2b9b687456a3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: b846ed54a1f7c8ef7a85ea9f836ffa75d0036ae6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="overview-of-tls---ssl-schannel-ssp"></a>Información general de TLS - SSL (Schannel SSP)

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

En este tema para profesionales de TI describe los cambios en la funcionalidad en el Schannel soporte proveedor seguridad (SSP), que incluye la seguridad de la capa de transporte (TLS), la capa de Sockets seguros (SSL) y los protocolos de autenticación de seguridad de capa de transporte de datagramas (DTLS) para Windows Server 2012 R2, Windows Server 2012, Windows 8.1 y Windows 8.

Schannel es un proveedor de soporte técnico de seguridad (SSP) que implementa los protocolos de autenticación estándar SSL, TLS y DTLS Internet. La interfaz de proveedor de soporte técnico de seguridad (SSPI) es una API usada por los sistemas Windows para realizar funciones relacionadas con la seguridad, incluida la autenticación. La SSPI funciona como una interfaz común a varios proveedores de compatibilidad para seguridad (SSP), incluida la SSP. Schannel

Para obtener más información sobre la implementación de Microsoft de TLS y SSL en el SSP Schannel, consulta el [referencia técnica de TLS/SSL (2003)](https://technet.microsoft.com/library/cc784149(v=ws.10).aspx).


##<a name="tlsssl-schannel-ssp-features"></a>Características TLS/SSL (Schannel SSP)
A continuación describen las características de TLS en la SSP. Schannel

### <a name="tls-session-resumption"></a>Reanudación de la sesión TLS
El protocolo de seguridad de la capa de transporte (TLS), un componente de proveedor de soporte técnico de seguridad de Schannel, se usa para proteger los datos que se envían entre aplicaciones a través de una red de confianza. TLS/SSL puede usarse para autenticar servidores y equipos cliente y también para cifrar mensajes entre las partes autenticados.

Dispositivos que se conectan con frecuencia TLS con los servidores deben volver a conectar debido a la expiración de la sesión. Windows 8.1 y Windows Server 2012 R2 admiten ahora RFC 5077 (TLS reanudación de la sesión sin estado en el servidor). Esta modificación proporciona a dispositivos de Windows Phone y Windows RT:

-   Uso de recursos reducido en el servidor

-   Reduce el ancho de banda, lo que mejora la eficacia de las conexiones de cliente

-   Reduce el tiempo invertido para el protocolo de enlace TLS debido a reanudaciones de la conexión.

> [!NOTE]
> La implementación de cliente de RFC 5077 se agregó en Windows 8.

Para obtener información acerca de reanudación de la sesión TLS sin estado, consulta el documento IETF [5077 RFC.](http://www.ietf.org/rfc/rfc5077)

### <a name="application-protocol-negotiation"></a>Negociación del protocolo de aplicación
 Windows Server 2012 R2 y Windows 8.1 admiten negociación del protocolo de aplicación de cliente TLS para que las aplicaciones pueden aprovechar protocolos como parte del desarrollo HTTP 2.0 estándar y los usuarios pueden acceder a los servicios en línea como Google y Twitter con aplicaciones que se ejecutan en el protocolo SPDY.

**Cómo funciona**

Aplicaciones cliente y servidor habilitar extensión de negociación del protocolo de aplicación al proporcionar listas de protocolo de aplicación compatibles identificadores, en orden descendente según su preferencia. El cliente TLS indica que admite la negociación de protocolo de aplicación, incluida la extensión de negociación de protocolo de capa de aplicación (ALPN) con una lista de los protocolos admitidos por el cliente en el mensaje ClientHello.

Cuando el cliente TLS realiza la solicitud al servidor, el servidor TLS lee su lista de protocolo compatible para el protocolo de aplicación preferida que también admite el cliente. Si se encuentra tales un protocolo, el servidor responde con el identificador de protocolo seleccionado y continúa con el protocolo de enlace como de costumbre. Si no hay ningún protocolo de aplicación común, el servidor envía una alerta de error grave de protocolo de enlace.

### <a name="BKMK_TrustedIssuers"></a>Administración de emisores de confianza para la autenticación de cliente
Cuando se requiere con SSL o TLS autenticación del equipo cliente, el servidor puede configurarse para enviar una lista de emisores de certificados de confianza. Esta lista contiene el conjunto de emisores de certificados que confíe en el servidor y es una sugerencia para el equipo cliente como el certificado de cliente que se seleccione si hay varios certificados presentes. Además, la cadena de certificados que del equipo cliente envía al servidor debe validarse con la lista de emisores de confianza configurado.

Antes de Windows Server 2012 y Windows 8, aplicaciones o procesos que usan el SSP Schannel (incluidos HTTP.sys y IIS) pueden proporcionar una lista de los emisores de confianza que admiten para la autenticación de cliente a través de una lista de certificados de confianza (CTL).

En Windows Server 2012 y Windows 8, se realizaron cambios en el proceso de autenticación subyacentes para que:

-   Ya no se admite la administración de la lista en función de CTL emisor de confianza.

-   El comportamiento para enviar la lista de emisor de confianza de manera predeterminada está desactivado: ahora es el valor predeterminado de la clave del registro de SendTrustedIssuerList 0 (desactivado de manera predeterminada) en lugar de 1.

-   Se conserva la compatibilidad con versiones anteriores de los sistemas operativos Windows.

**¿El valor que esto agrega?**

A partir de Windows Server 2012, el uso de la CTL se ha reemplazado con una implementación de certificados basado en la tienda. Esto permite facilidad de uso más familiar a través de la existente commandlets de administración de certificados del proveedor de PowerShell, así como herramientas de línea de comandos como certutil.exe.

Aunque el tamaño máximo de la lista de entidades de certificación de confianza que el SSP Schannel admite (16 KB) sigue siendo el mismo que en Windows Server 2008 R2, en Windows Server 2012, hay un certificado dedicado nuevo almacén de emisores de autenticación de cliente para que los certificados no relacionados no están incluidos en el mensaje.

**¿Cómo funciona?**

En Windows Server 2012, la lista de emisores de confianza se configura mediante los almacenes de certificados; almacén de certificados de equipo global de un valor predeterminado y que es opcional por sitio. El origen de la lista se determinará como sigue:

-   Si hay un almacén de credenciales específicas configurado para el sitio, se usará como el origen

-   Si no existe ningún certificado en el almacén definido por la aplicación, Schannel comprueba el **emisores de autenticación de cliente** almacenar en el equipo local y, si hay certificados, usa ese almacén como el origen. Si se encuentra ningún certificado en cualquiera de las tiendas, se comprueba el almacén raíces de confianza.

-   Si ninguna de las tiendas globales o locales contienen los certificados, el proveedor de Schannel utilizará el **entidades de confianza raíz Certifictation** almacenar como el origen de la lista de emisores de confianza. (Este es el comportamiento de Windows Server 2008 R2).

Si la **entidades Certifictation raíz de confianza** almacén que se usó contiene una mezcla de raíz (autofirmado) y certificados de emisor de la entidad de (certificación CA), solo los certificados de emisor de la entidad de certificación se enviará al servidor de manera predeterminada.

**Cómo configurar Schannel para usar el almacén de certificados de confianza de emisores**

La arquitectura de SSP Schannel en Windows Server 2012 usará de forma predeterminada las tiendas, como se describió anteriormente para administrar la lista de emisores de confianza. Aún puedes usar la existente commandlets de administración de certificados del proveedor de PowerShell, así como herramientas de línea de comandos como Certutil para administrar los certificados.

Para obtener información sobre la administración de certificados mediante el proveedor de PowerShell, consulta [Cmdlets de administración de AD CS en Windows](https://technet.microsoft.com/library/hh848365(v=wps.620).aspx).

Para obtener información sobre la administración de certificados mediante la utilidad de certificado, consulta [certutil.exe](https://technet.microsoft.com/library/cc732443.aspx).

Para obtener información sobre qué datos, incluida la tienda definido por la aplicación, se definen para una credencial Schannel, vea [estructura SCHANNEL_CRED (Windows)](https://msdn.microsoft.com/library/windows/desktop/aa379810(v=vs.85).aspx).

**Valores predeterminados para los modos de confianza**

Hay tres cliente confiar en modos de autenticación admitidos por el proveedor de Schannel. El modo de confianza controla cómo se realiza la validación de la cadena de certificados del cliente y una configuración de todo el sistema controla la REG_DWORD "ClientAuthTrustMode" en HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\Schannel.

|Valor|Modo de confianza|Descripción|
|-----|-------|--------|
|0|Equipo de confianza (predeterminado)|Requiere que se ha emitido el certificado de cliente mediante un certificado en la lista de emisores de confianza.|
|1|Confianza exclusiva raíz|Requiere que un cliente de cadenas a un certificado raíz contenida en el almacén especificado por el llamador de confianza emisor de certificados. También se debe emitir el certificado por un emisor de la lista de emisores de confianza|
|2|Confianza exclusiva de la entidad de certificación|Requiere una cadena de certificados de cliente a un certificado de CA intermedia o un certificado raíz en el especificado por el llamador de confianza tienda emisor.|

Para obtener información acerca de los errores de autenticación debido a problemas de configuración de emisores de confianza, consulta el artículo de Knowledge Base [280256](https://support.microsoft.com/kb/2802568).

### <a name="BKMK_SNI"></a>Compatibilidad con TLS las extensiones de nombre de servidor indicador (SNI)
Característica de indicación del nombre de servidor extiende los protocolos SSL y TLS para permitir la identificación correcta del servidor al muchas imágenes virtual se ejecutan en un solo servidor. Para proteger correctamente la comunicación entre un equipo cliente y un servidor, el equipo cliente solicita un certificado digital del servidor. Después de que el servidor responde a la solicitud y envía el certificado, el equipo cliente lo examina, usa para cifrar la comunicación y continúa con el intercambio de solicitud y respuesta normal. Sin embargo, en un escenario de hospedaje virtual, varios dominios, cada uno con su propio certificado potencialmente diferentes, se hospedan en un servidor. En este caso, el servidor no tiene ninguna manera de saber de antemano que certificados para enviar al equipo cliente. SNI permite que el equipo cliente informar el dominio de destino anteriormente en el protocolo y permite al servidor seleccionar correctamente el certificado correcto.

**¿El valor que esto agrega?**

Esta funcionalidad adicional:

-   Permite hospedar varios sitios Web SSL en un único IP y la combinación de puerto

-   Reduce el uso de memoria cuando se hospedan varios sitios Web SSL en un único servidor web

-   Permite a los usuarios más conectarte simultáneamente a mi SSL, los sitios Web

-   Te permite proporcionar sugerencias a los usuarios finales a través de la interfaz del equipo para seleccionar el certificado correcto durante el proceso de autenticación de cliente.

**Cómo funciona**

El SSP Schannel mantiene una memoria caché de los Estados de conexión de cliente permitidos para los clientes. Esto permite que los equipos cliente a conectar rápidamente con el servidor SSL sin sujeto a un protocolo de enlace SSL completa en las visitas posteriores.  Este uso eficiente de administración de certificados permite más sitios se hospeden en un solo Windows Server 2012 en comparación con versiones anteriores del sistema operativo.

Se ha mejorado la selección de certificados por el usuario final, ya que permite crear una lista de nombres de emisor del certificado probable que proporcionar al usuario final con sugerencias en cuál elegir. Esta lista es configurable mediante Directiva de grupo.

### <a name="BKMK_DTLS"></a>Seguridad de la capa de transporte de datagramas (DTLS)
El protocolo de la versión 1.0 de DTLS se ha agregado al proveedor de soporte técnico de seguridad Schannel. El protocolo DTLS proporciona privacidad comunicaciones para los protocolos de datagramas. El protocolo permite a cliente-servidor aplicaciones se comuniquen de forma que se ha diseñado para evitar interceptaciones, manipulación o falsificación de mensajes. El protocolo DTLS se basa en el protocolo de seguridad de la capa de transporte (TLS) y proporciona seguridad equivalente garantías, lo que reduce la necesidad de usar IPsec o diseñar un protocolo de seguridad de nivel de aplicación personalizada.

**¿El valor que esto agrega?**

Datagramas son comunes en streaming de medios, como juego o segura videoconferencia. Agregar el protocolo DTLS al proveedor de Schannel te ofrece la posibilidad de usar el modelo que resultará familiar SSPI de Windows en la protección de la comunicación entre los equipos cliente y servidores. DTLS está diseñado expresamente para ser similar como TLS como sea posible, para minimizar nuevo invento de seguridad y para maximizar la cantidad de la reutilización de código y de infraestructura.

**Cómo funciona**

Las aplicaciones que usan DTLS sobre UDP pueden usar el modelo SSPI en Windows Server 2012 y Windows 8. Determinados conjuntos de cifrado están disponibles para la configuración, similar a cómo se puede configurar TLS. Schannel continúa usando el proveedor criptográfico CNG que aprovecha las ventajas de la certificación de FIPS 140, que se introdujo en Windows Vista.

### <a name="BKMK_Deprecated"></a>Funcionalidad en desuso
En el SSP Schannel para Windows Server 2012 y Windows 8, no hay ningún características en desuso o funcionalidad.

## <a name="see-also"></a>Consulta también
-   [Modelo de seguridad de la nube privada - funcionalidad del contenedor](https://social.technet.microsoft.com/wiki/contents/articles/6756.private-cloud-security-model-wrapper-functionality.aspx)



