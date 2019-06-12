---
title: TLS / SSL (Schannel SSP) información general sobre
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c8836345-16bb-4dcc-8d2b-2b9b687456a3
author: justinha
ms.author: justinha
manager: brianlic-msft
ms.date: 05/16/2018
ms.openlocfilehash: b1af556200c9dd497bac835f1480479cca075dab
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447286"
---
# <a name="overview-of-tls---ssl-schannel-ssp"></a>Información general de TLS / SSL (Schannel SSP)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10

En este tema para profesionales de TI se describe los cambios en la funcionalidad en el proveedor seguridad Schannel soporte técnico (SSP), que incluye la seguridad de capa de transporte (TLS), la capa de Sockets seguros (SSL) y la seguridad de capa de transporte de datagrama (DTLS) protocolos de autenticación de Windows Server 2012 R2, Windows Server 2012, Windows 8.1 y Windows 8.

Schannel es un proveedor de compatibilidad para seguridad (SSP) que implementa los protocolos de autenticación estándar de Internet de SSL y TLS y DTLS. La interfaz del proveedor de compatibilidad para seguridad (SSPI) es una API que usan los sistemas Windows para realizar funciones basadas en la seguridad, incluida la autenticación. SSPI funciona como una interfaz común a varios proveedores de compatibilidad para seguridad (SSP), incluido el Schannel SSP.

Para obtener más información acerca de la implementación de Microsoft de TLS y SSL en el SSP de Schannel, consulte el [referencia técnica de TLS/SSL (2003)](https://technet.microsoft.com/library/cc784149(v=ws.10).aspx).


## <a name="tlsssl-schannel-ssp-features"></a>Características TLS/SSL (Schannel SSP)
A continuación describen las características de TLS en el SSP. Schannel

### <a name="tls-session-resumption"></a>Reanudación de la sesión de TLS
El protocolo de Seguridad de la capa de transporte (TLS), un componente del proveedor de soporte técnico de seguridad de Schannel, se usa para proteger los datos que se envían entre las aplicaciones de una red que no sea de confianza. Puede usarse TLS/SSL para autenticar servidores y equipos cliente y también para cifrar mensajes entre las partes autenticadas.

Los dispositivos que conectan TLS con los servidores con frecuencia necesitan volver a conectarse debido a la expiración de la sesión. Windows 8.1 y Windows Server 2012 R2 ahora admiten la RFC 5077 (reanudación de sesión TLS sin estado en el servidor). Esta modificación proporciona a dispositivos Windows Phone y Windows RT:

-   Un uso de recursos reducido en el servidor

-   Un ancho de banda reducido, lo que mejora la eficacia de las conexiones de cliente

-   Reduce el tiempo empleado para el protocolo de enlace TLS debido a reanudaciones de la conexión.

> [!NOTE]
> La implementación de cliente de RFC 5077 se agregó en Windows 8.

Para obtener información acerca de la reanudación de la sesión TLS sin estado, consulte el documento IETF [RFC 5077.](http://www.ietf.org/rfc/rfc5077)

### <a name="application-protocol-negotiation"></a>Negociación de protocolo de aplicación
 Windows Server 2012 R2 y Windows 8.1 admiten negociación del protocolo de aplicación de TLS de cliente por lo que las aplicaciones puedan aprovechar los protocolos como parte del desarrollo estándar de HTTP 2.0 y los usuarios pueden acceder a servicios en línea como Google y Twitter mediante aplicaciones que ejecutan el protocolo SPDY.

**Cómo funciona**

Las aplicaciones de cliente y servidor habilitan la extensión de la negociación del protocolo de aplicaciones proporcionando listas de ID de protocolo de aplicación compatibles en orden descendente de preferencia. El cliente TLS indica que admite la negociación de protocolos de aplicación mediante la inclusión de la extensión de Negociación de protocolo de capa de aplicación (ALPN) con una lista de protocolos admitidos por el cliente en el mensaje ClientHello.

Cuando el cliente TLS realiza la solicitud al servidor, el servidor TLS lee su lista de protocolos admitidos para obtener el protocolo de aplicación preferido que también admite el cliente. Si se encuentra este tipo de protocolo, el servidor responde con el identificador de protocolo seleccionado y continúa con el protocolo de enlace como de costumbre. Si no hay ningún protocolo de aplicación común, el servidor envía una alerta de error grave de protocolo de enlace.

### <a name="BKMK_TrustedIssuers"></a>Administración de emisores de confianza para la autenticación de cliente
Cuando se requiere la autenticación del equipo cliente mediante SSL o TLS, el servidor puede configurarse para enviar una lista de emisores de certificados de confianza. Esta lista contiene el conjunto de emisores de certificados en los que confiará el servidor y es una sugerencia para el equipo cliente en cuanto a qué certificado de cliente seleccionar si hay varios certificados presentes. Además, la cadena de certificados que el equipo cliente envía al servidor debe validarse con la lista de emisores de confianza configurados.

Antes de Windows Server 2012 y Windows 8, las aplicaciones o procesos que usaron Schannel SSP (incluido IIS y HTTP.sys) podrían proporcionar una lista de emisores de confianza que admitieron para la autenticación de cliente a través de una lista de certificados de confianza (CTL).

En Windows Server 2012 y Windows 8, los cambios realizados en el proceso de autenticación subyacente para que:

-   Ya no se admite la administración de listas de emisores de confianza basada en CTL.

-   El comportamiento de envío de la lista de emisores de confianza de forma predeterminada está desactivada: El valor predeterminado de la clave de registro SendTrustedIssuerList ahora es 0 (desactivado de forma predeterminada) en lugar de 1.

-   Se conserva la compatibilidad con versiones anteriores de sistemas operativos Windows.

**¿Qué valor Esto aporta?**

A partir de Windows Server 2012, el uso de CTL se ha reemplazado por una implementación basada en el almacén de certificados. Esto permite una manejabilidad más familiar a través de los cmdlets de administración de certificados existentes del proveedor de PowerShell, además de herramientas de línea de comandos como certutil.exe.

Aunque el tamaño máximo de la lista de entidades de certificación de confianza que Schannel SSP admite (16 KB) sigue siendo el mismo que en Windows Server 2008 R2, en Windows Server 2012 es un nuevo almacén de certificados dedicados para emisores de autenticación de cliente para que los certificados no relacionados no se incluyen en el mensaje.

**¿Cómo funciona?**

En Windows Server 2012, la lista de emisores de confianza se configura mediante almacenes de certificados; almacén de certificados de equipo global de un valor predeterminado y otro opcional por sitio. El origen de la lista se determinará como sigue:

-   Si hay un almacén de credenciales específico configurado para el sitio, se usará como origen

-   Si no existe ningún certificado en el almacén definido por la aplicación, entonces Schannel comprueba el almacén de **Emisores de autenticación de cliente** en el equipo local y, si están presentes los certificados, usa ese almacén como el origen. Si no se encuentra ningún certificado en ningún almacén, se comprueba el almacén de raíces de confianza.

-   Si ninguno de los almacenes local o global contiene certificados, el proveedor de Schannel usará el **entidades de certificación raíz de confianza** almacenar como origen de la lista de emisores de confianza. (Este es el comportamiento para Windows Server 2008 R2).

Si el **entidades de certificación raíz de confianza** almacén que se usó contiene una mezcla de raíz (autofirmados) y los certificados del emisor de certificación emisora (CA), solo los certificados de emisor de CA se enviarán al servidor de forma predeterminada.

**Cómo configurar Schannel para usar el almacén de certificados de emisores de confianza**

La arquitectura del SSP de Schannel en Windows Server 2012 usará de forma predeterminada los almacenes como se describió anteriormente para administrar la lista de emisores de confianza. Todavía puede usar los cmdlets de administración certificados existente del proveedor de PowerShell, además de herramientas de línea de comandos como Certutil para administrar certificados.

Para obtener información acerca de cómo administrar certificados mediante el proveedor de PowerShell, consulte [Cmdlets de administración de AD CS en Windows](https://technet.microsoft.com/library/hh848365(v=wps.620).aspx).

Para obtener información acerca de cómo administrar certificados mediante la utilidad de certificado, consulte [certutil.exe](https://technet.microsoft.com/library/cc732443.aspx).

Para obtener información acerca de los datos, incluido el almacén definido por la aplicación, se definen para una credencial de Schannel, consulte [estructura SCHANNEL_CRED (Windows)](https://msdn.microsoft.com/library/windows/desktop/aa379810(v=vs.85).aspx).

**Valores predeterminados para los modos de confianza**

Hay tres modos de confianza de autenticación de cliente admitidos por el proveedor de Schannel. El modo de confianza controla cómo se realiza la validación de la cadena de certificados del cliente y es una configuración de todo el sistema controlada por REG_DWORD "ClientAuthTrustMode" en HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\Schannel .

|Valor|Modo de confianza|Descripción|
|-----|-------|--------|
|0|Confianza de la máquina (valor predeterminado)|Requiere la emisión del certificado de cliente por parte de un certificado de la lista de emisores de confianza.|
|1|Confianza de raíz exclusiva|Requiere que un certificado de cliente se encadene con un certificado raíz contenido en el almacén del emisor de confianza especificado por el emisor. El certificado también debe ser emitido por un emisor de la lista de emisores de confianza|
|2|Confianza de CA exclusiva|Requiere que un certificado de cliente se encadene con un certificado de entidad de certificación intermedio o un certificado raíz del almacén del emisor de confianza especificado por el emisor.|

Para obtener información acerca de los errores de autenticación debido a problemas de configuración de emisores de confianza, consulte el artículo de Knowledge Base [280256](https://support.microsoft.com/kb/2802568).

### <a name="BKMK_SNI"></a>Soporte TLS para extensiones de indicador de nombre de servidor (SNI)
La característica Indicación de nombre de servidor extiende los protocolos SSL y TLS para permitir la identificación adecuada del servidor cuando se ejecuta una gran cantidad de imágenes virtuales en un solo servidor. Para asegurar la comunicación de forma adecuada entre un equipo cliente y un servidor, el equipo cliente solicita al servidor un certificado digital. Una vez que el servidor responde la solicitud y envía el certificado, el equipo cliente lo examina, lo usa para cifrar la comunicación y continúa con en intercambio normal de solicitud-respuesta. Sin embargo, en un escenario de hospedaje virtual, se hospedan varios dominios, cada uno con un certificado potencialmente diferente, en un solo servidor. En este caso, el servidor no puede saber de antemano qué certificado se envía al equipo cliente. SNI permite que el equipo cliente informe antes al dominio de destino en el protocolo, y esto permite que el servidor seleccione correctamente el certificado adecuado.

**¿Qué valor Esto aporta?**

Esta funcionalidad adicional:

-   Permite hospedar varios sitios web SSL en una combinación de puerto e IP única.

-   Reduce el uso de memoria cuando se hospedan varios sitios web SSL en un servidor web único.

-   Permite que una mayor cantidad de usuarios se conecten a mis sitios web SSL simultáneamente.

-   Permite proporcionar sugerencias a los usuarios finales a través de la interfaz del equipo para seleccionar el certificado correcto durante un proceso de autenticación del cliente.

**Cómo funciona**

Schannel SSP mantiene una caché en memoria de los estados de conexión de clientes permitidos para los clientes. Esto permite que los equipos cliente puedan volver a conectarse rápidamente con el servidor SSL sin someterse a un protocolo de enlace SSL completo en las visitas siguientes.  Este uso eficaz de administración de certificados permite más sitios puedan hospedarse en un solo Windows Server 2012 en comparación con versiones anteriores del sistema operativo.

La selección de certificados por parte de los usuarios finales se ha mejorado, ya que permite preparar una lista de nombres de emisores de certificados probables con sugerencias para el usuario final acerca de cuál debe elegir. Esta lista puede configurarse mediante la directiva de grupo.

### <a name="BKMK_DTLS"></a>Seguridad de la capa de transporte del datagrama (DTLS)
La versión 1.0 del protocolo DTLS se ha agregado al proveedor de compatibilidad para seguridad Schannel. El protocolo DTLS proporciona privacidad en las comunicaciones de los protocolos de datagramas. El protocolo permite que las aplicaciones de cliente/servidor se comuniquen de acuerdo con el modo en que fueron diseñadas para evitar interceptaciones, alteraciones o falsificación de mensajes. El protocolo DTLS se basa en el protocolo de seguridad de la capa de transporte (TLS) y proporciona garantías de seguridad equivalentes, lo que reduce la necesidad de usar IPsec o diseñar un protocolo personalizado de seguridad de la capa de la aplicación.

**¿Qué valor Esto aporta?**

Los datagramas son comunes en streaming multimedia, como juegos o protegida videoconferencia. Si agrega el protocolo DTLS al proveedor de Schannel, podrá usar el conocido modelo Windows SSPI para proteger la comunicación entre los equipos cliente y los servidores. DTLS está diseñado intencionalmente para ser tan similar a TLS como sea posible, tanto para minimizar la nueva invención de seguridad como para maximizar la reutilización de los códigos y la infraestructura.

**Cómo funciona**

Las aplicaciones que usan DTLS mediante UDP pueden usar el modelo SSPI en Windows Server 2012 y Windows 8. Hay disponibles algunos conjuntos de cifrado para la configuración, de manera similar al modo en que se configura TLS. Schannel continúa usando el proveedor criptográfico CNG que aprovecha la certificación de FIPS 140, que se introdujo en Windows Vista.

### <a name="BKMK_Deprecated"></a>Funcionalidad desusada
En el SSP de Schannel para Windows Server 2012 y Windows 8, hay no hay características ni funcionalidades desusadas.

## <a name="see-also"></a>Vea también
-   [Modelo de seguridad de la nube privada: funcionalidad contenedora](https://social.technet.microsoft.com/wiki/contents/articles/6756.private-cloud-security-model-wrapper-functionality.aspx)



