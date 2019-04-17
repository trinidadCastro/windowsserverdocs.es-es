---
title: Novedades en el servidor DNS en Windows Server
description: Este tema proporciona una introducción de nuevas características de servidor DNS en Windows Server 2016 y versiones posteriores
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: c9cecb94-3cd5-4da7-9a3e-084148b8226b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3ddb8920c045f231dcf5286283d9895ef6ffff47
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-dns-server-in-windows-server"></a>Novedades en el servidor DNS en Windows Server

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Este tema describe la funcionalidad del servidor de sistema de nombres de dominio (DNS) que es nuevo o cambiado en Windows Server 2016.  
  
En Windows Server 2016, servidor DNS ofrece compatibilidad mejorada en las siguientes áreas.  
  
|Funcionalidad|Nuevas o mejoradas|Descripción|  
|-----------------|-------------------|---------------|  
|Directivas DNS|Nuevo|Puedes configurar las directivas DNS para especificar cómo un servidor DNS responde a las consultas DNS. Las respuestas de DNS pueden basarse en la dirección IP del cliente (ubicación), hora del día y otros parámetros. Las directivas DNS permiten DNS con reconocimiento de ubicación, administración del tráfico, equilibrio de carga, produzca DNS y otros escenarios.|  
|Limitación (RRL) de la velocidad de respuesta|Nuevo|Puedes habilitar la limitación de velocidad de respuesta en los servidores DNS. Al hacer esto, evitar la posibilidad de sistemas malintencionados con los servidores DNS para iniciar un ataque de denegación de servicio en un cliente DNS.|  
|Autenticación basada en DNS de entidades con nombre (DANE)|Nuevo|Puedes usar los registros TLSA (autenticación de seguridad de capa de transporte) para proporcionar información a los clientes DNS que se indican qué CA esperan un certificado de tu nombre de dominio. Esto impide que los ataques de man-in-the-middle donde alguien podría dañar la caché para apuntar a su propio sitio Web y proporcionar un certificado que emitieron por una CA diferentes.|  
|Compatibilidad con el registro desconocido|Nuevo|Puedes agregar los registros que no se admiten explícitamente en el servidor DNS de Windows mediante la funcionalidad de registro desconocido.|  
|Sugerencias de raíz IPv6|Nuevo|Puedes usar el IPV6 nativo admiten sugerencias de raíz para realizar la resolución de nombre de internet con los servidores de raíz IPV6.|  
|Soporte técnico de Windows PowerShell|Mejorado|Nuevos cmdlets de PowerShell de Windows están disponibles para el servidor DNS.|  
  
## <a name="dns-policies"></a>Directivas DNS

Directiva de DNS para la administración de tráfico en función de ubicación geográfica, las respuestas de DNS inteligentes en función de la hora del día, puedes usar para administrar un único servidor DNS configurado para la implementación de split\ cerebro, la aplicación de filtros en las consultas DNS y mucho más. Los elementos siguientes proporcionan más detalles sobre estas funcionalidades.

-   **Equilibrio de carga de aplicación.** Cuando se implementaron varias instancias de una aplicación en distintas ubicaciones, puedes usar la directiva de DNS para equilibrar la carga de tráfico entre las instancias de la aplicación diferente, asignar dinámicamente la carga de tráfico de la aplicación.

-   **Administración de tráfico de basada en ubicación Geo\.** Puedes usar la directiva de DNS para permitir que los servidores DNS principal y secundario responder a las consultas del cliente DNS según la ubicación geográfica del cliente y el recurso al que intenta conectarse, el cliente proporciona al cliente con la dirección IP del recurso más cercana. 

-   **Dividir cerebro DNS.** Con split\ cerebro DNS, registros DNS se dividen en distintos ámbitos zona en el mismo servidor DNS y clientes DNS reciban una respuesta en función de si los clientes son interna o externa. Puedes configurar split\ cerebro DNS para las zonas integradas de Active Directory o para las zonas en los servidores DNS independiente.

-   **Filtrado.** Puedes configurar la directiva DNS para crear filtros de consulta que se basan en los criterios que proporciones. Filtros de consultas en la directiva de DNS te permiten configurar el servidor DNS para responder de manera personalizada, en función de la consulta DNS y el cliente DNS que envía la consulta DNS. 
-   **Análisis.** Puedes usar la directiva de DNS para redirigir a malintencionados clientes DNS a una dirección IP de non\ existente en lugar de dirigiéndolos al equipo que está intentando llegar a.

-   **Hora del día en función de redirección.** Puedes usar la directiva de DNS para distribuir el tráfico de la aplicación en diferentes instancias geográficamente de una aplicación mediante las directivas DNS que se basan en el momento del día. 
  
También puedes usar directivas DNS para Active Directory integrado DNS zonas.

Para obtener más información, consulta el [DNS directiva escenario guía](deploy/DNS-Policies-Overview.md).

## <a name="response-rate-limiting"></a>Limitación de velocidad de respuesta

Puede configurar RRL para controlar cómo responder a las solicitudes de un cliente DNS cuando el servidor recibe varias solicitudes de selección del destino al mismo cliente. Al hacer esto, puede evitar que alguien envío de un ataque de denegación de servicio (Dos) con los servidores DNS. Por ejemplo, un componente de red puede enviar solicitudes al servidor DNS mediante la dirección IP de un tercer equipo como el solicitante. Sin RRL, los servidores DNS pueden responder a todas las solicitudes, inundación el tercer equipo. Cuando usas RRL, puedes configurar la siguiente configuración:  
  
-   **Respuestas por segundo**. Este es el máximo número de veces que se le dará la misma respuesta a un cliente en un segundo.  
  
-   **Errores por segundo**. Este es el máximo número de veces que se enviará una respuesta de error en el mismo cliente en un segundo.  
  
-   **Ventana**. Este es el número de segundos para que las respuestas a un cliente, se suspenderá si se realizan demasiadas solicitudes.  
  
-   **Fuga**. Esta es la frecuencia con el servidor DNS responderá a una consulta durante el tiempo que se suspenden las respuestas. Por ejemplo, si el servidor suspende las respuestas a un cliente de 10 segundos y la velocidad de pérdida es 5, el servidor aún responde a una consulta para cada 5 consultas enviada. Esto permite a los clientes legítimos obtener respuestas incluso cuando el servidor DNS esté aplicando una limitación en la subred o el FQDN de velocidad de respuesta.  
  
-   **Velocidad de TC**. Esto se usa para indicar al cliente que se intenta conectar con TCP cuando están suspendidas respuestas al cliente. Por ejemplo, si la velocidad de TC es 3, y el servidor suspende las respuestas a un cliente determinado, el servidor emite una solicitud de conexión TCP para cada 3 consultas recibidas. Asegúrate de que el valor de velocidad de TC es menor que la velocidad de pérdida, proporcionar al cliente la opción de conectarlo a través de TCP antes de pérdida de respuestas.  
  
-   **Respuestas máximas**. Este es el número máximo de las respuestas que a un cliente emitirá el servidor mientras se suspenden las respuestas.  
  
-   **Lista blanca dominios**. Se trata de una lista de dominios para excluir de la configuración de RRL.  
  
-   **Lista blanca subredes**. Se trata de una lista de subredes se excluirán del configuración RRL.  
  
-   **Interfaces de servidor de la lista blanca**. Se trata de una lista de interfaces de servidor DNS para excluir de la configuración de RRL.  
  
## <a name="dane-support"></a>Soporte técnico DANE

Puede utilizar la DANE \ (RFC 6394 y 6698\) para especificar a los clientes DNS, lo que esperan emisión de nombres de dominios de certificados de CA hospedado en el servidor DNS. Esto impide que una forma de ataque man-in-the-middle donde alguien es capaz de dañar una memoria caché DNS y seleccione un nombre DNS de su propia dirección IP.  
  
Por ejemplo, supongamos que host un sitio Web seguro que usa SSL a www.contoso.com mediante un certificado de una entidad conocida denominada CA1. Es posible que todavía será capaz una persona pueda obtener un certificado para www.contoso.com desde una diferente, no para-conocido, certificado de entidad emisora denominada CA2. A continuación, la entidad que aloja el sitio Web de www.contoso.com falsos posible que pueda dañar la caché de un cliente o servidor para apuntar www.contoto.com a su sitio falso. El usuario final se muestra un certificado de CA2 y puede simplemente acuse de recibo y conectarse al sitio Web falso. Con DANE, el cliente se realiza una solicitud al servidor DNS para contoso.com pedir el registro TLSA y obtener que el certificado para www.contoso.com era problemas por CA1. Si se presenta con un certificado de CA de otro, se cancelará la conexión.  
  
## <a name="unknown-record-support"></a>Compatibilidad con el registro desconocido

"Registro desconocido" es un RR cuyo formato RDATA no se sabe que el servidor DNS. El recién agregado compatibilidad con tipos de registros desconocido (RFC 3597) significa que puedes agregar los tipos de registro no compatibles en las zonas de servidor DNS de Windows en el formato binario de cable. Las ventanas de resolución de caché ya tiene la capacidad para procesar los tipos de registro desconocido. Servidor DNS de Windows no harán ningún procesamiento de registro específico para los registros desconocidos, pero enviará la fecha en las respuestas si se reciben consultas para ella.  
  
## <a name="ipv6-root-hints"></a>Sugerencias de raíz IPv6

Las sugerencias de raíz IPV6, como publicó IANA, se agregaron en el servidor DNS de windows. Las consultas de nombre de internet ahora pueden usar servidores de raíz IPv6 para llevar a cabo las resoluciones de nombre.

## <a name="windows-powershell-support"></a>Soporte técnico de Windows PowerShell

Los siguientes cmdlets de Windows PowerShell nuevo y parámetros se introdujeron en Windows Server 2016.
  
-   **DnsServerRecursionScope agregar**. Este cmdlet crea un nuevo ámbito recursividad en el servidor DNS. Ámbitos de recursión se usan por las directivas DNS para especificar una lista de servidores de reenvío para usarse en las consultas DNS.  
  
-   **Quitar DnsServerRecursionScope**. Este cmdlet quita ámbitos recursividad existentes.  
  
-   **Conjunto DnsServerRecursionScope**. Este cmdlet cambia la configuración de un ámbito de recursión existente.  
  
-   **Get-DnsServerRecursionScope**. Este cmdlet recupera información sobre los ámbitos de recursión existentes.  
  
-   **DnsServerClientSubnet agregar**. Este cmdlet crea una nueva subred del cliente DNS. Las subredes se usan por las directivas DNS para identificar dónde se encuentra un cliente DNS.  
  
-   **Quitar DnsServerClientSubnet**. Este cmdlet quita subredes existentes del cliente DNS.  
  
-   **Conjunto DnsServerClientSubnet**. Este cmdlet cambia la configuración de una subred existente del cliente DNS.  
  
-   **Get-DnsServerClientSubnet**. Este cmdlet recupera información sobre subredes existentes del cliente DNS.  
  
-   **DnsServerQueryResolutionPolicy agregar**. Este cmdlet crea una nueva directiva de resolución de consulta DNS. Directivas de resolución de consulta DNS se usan para especificar cómo hacerlo, o si se responde una consulta, en función de distintos criterios.  
  
-   **Quitar DnsServerQueryResolutionPolicy**. Este cmdlet quita las directivas existentes de DNS.  
  
-   **Conjunto DnsServerQueryResolutionPolicy**. Este cmdlet cambia la configuración de una directiva existente de DNS.  
  
-   **Get-DnsServerQueryResolutionPolicy**. Este cmdlet recupera información sobre las directivas existentes de DNS.  
  
-   **Habilitar DnsServerPolicy**. Este cmdlet permite políticas DNS existente.  
  
-   **Deshabilitar DnsServerPolicy**. Este cmdlet deshabilita las directivas existentes de DNS.  
  
-   **DnsServerZoneTransferPolicy agregar**. Este cmdlet crea una nueva directiva de transferencia de zona de servidor DNS. Directivas de transferencia de zona DNS especifican si deseas denegar o pasar por alto una transferencia de zona basada en distintos criterios.  
  
-   **Quitar DnsServerZoneTransferPolicy**. Este cmdlet quita directivas de transferencia de zona de servidor DNS existentes.  
  
-   **Conjunto DnsServerZoneTransferPolicy**. Este cmdlet cambia la configuración de una directiva de transferencia de zona de servidor DNS existente.  
  
-   **Get-DnsServerResponseRateLimiting**. Este cmdlet recupera la configuración de RRL.  
  
-   **Conjunto DnsServerResponseRateLimiting**. Este cmdlet cambia RRL settigns.  
  
-   **DnsServerResponseRateLimitingExceptionlist agregar**. Este cmdlet crea una lista de excepciones RRL en el servidor DNS.  
  
-   **Get-DnsServerResponseRateLimitingExceptionlist**. Este cmdlet recupera RRL excception listas.  
  
-   **Quitar DnsServerResponseRateLimitingExceptionlist**. Este cmdlet quita una lista de excepciones RRL existente.  
  
-   **Conjunto DnsServerResponseRateLimitingExceptionlist**. Este cmdlet cambia RRL listas de excepciones.  
  
-   **DnsServerResourceRecord agregar**. Este cmdlet se ha actualizado para admitir el tipo de registro desconocido.  
  
-   **Get-DnsServerResourceRecord**. Este cmdlet se ha actualizado para admitir el tipo de registro desconocido.  
  
-   **Quitar DnsServerResourceRecord**. Este cmdlet se ha actualizado para admitir el tipo de registro desconocido.  
  
-   **Conjunto DnsServerResourceRecord**. Este cmdlet se ha actualizado para admitir el tipo de registro desconocido

Para obtener más información, consulta los siguientes temas de referencia de comandos de Windows Server 2016 Windows PowerShell.

- [Módulo servidorDNS](https://technet.microsoft.com/itpro/powershell/windows/dns-server/index)
- [Módulo DnsClient](https://technet.microsoft.com/itpro/powershell/windows/dns-client/index)

## <a name="see-also"></a>Consulta también  
  
-   [Novedades en el cliente DNS](What-s-New-in-DNS-Client.md)  
  

  

