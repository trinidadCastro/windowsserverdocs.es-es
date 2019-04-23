---
title: Novedades en el servidor DNS en Windows Server
description: En este tema proporciona información general sobre las nuevas características de servidor DNS en Windows Server 2016 y versiones posteriores
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: c9cecb94-3cd5-4da7-9a3e-084148b8226b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 665e411eda834a59c6dbe3581611b9b58bd006f2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833576"
---
# <a name="whats-new-in-dns-server-in-windows-server"></a>Novedades en el servidor DNS en Windows Server

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Este tema describe la funcionalidad del servidor de sistema de nombres de dominio (DNS) que es nuevos o han cambiado en Windows Server 2016.  
  
En Windows Server 2016, el servidor DNS ofrece compatibilidad mejorada en las áreas siguientes.  
  
|Funcionalidad|Nueva o mejorada|Descripción|  
|-----------------|-------------------|---------------|  
|Directivas DNS|Nuevo|Puede configurar directivas DNS para especificar cómo un servidor DNS responde a las consultas DNS. Las respuestas DNS pueden basarse en la dirección IP del cliente (ubicación), hora del día y otros parámetros. Las directivas DNS permiten DNS con reconocimiento de ubicación, administración del tráfico, equilibrio de carga, DNS de cerebro dividido y otros escenarios.|  
|Tasa de respuesta limitar (RRL)|Nuevo|Puede habilitar la limitación de velocidad de respuesta en los servidores DNS. De esta manera, evitar la posibilidad de sistemas y malintencionados mediante los servidores DNS para iniciar un ataque de denegación de servicio en un cliente DNS.|  
|Autenticación basada en DNS de entidades con nombre (panel)|Nuevo|Puede usar los registros TLSA (autenticación de seguridad de capa de transporte) para proporcionar información a los clientes DNS que se indican qué entidad emisora de certificados que deben esperar un certificado de nombre de dominio. Esto evita los ataques de man-in-the-middle donde alguien podría dañar la memoria caché DNS para que apunte a su propio sitio Web y proporcionar un certificado que emitieron por una CA diferente.|  
|Compatibilidad con el registro desconocido|Nuevo|Puede agregar los registros que no están admitidos explícitamente por el servidor DNS de Windows mediante la funcionalidad de registro desconocido.|  
|Sugerencias de raíz de IPv6|Nuevo|Puede usar IPV6 nativo admiten sugerencias de raíz para realizar la resolución de nombres de internet mediante los servidores raíz de IPV6.|  
|Compatibilidad con Windows PowerShell|Mejorada|Nuevos cmdlets de Windows PowerShell están disponibles para el servidor DNS.|  
  
## <a name="dns-policies"></a>Directivas DNS

Puede usar la directiva de DNS para la ubicación geográfica en función de administración del tráfico, las respuestas DNS inteligentes que según la hora del día, y administrar un solo servidor DNS configurado para la división\-implementación cerebro, aplicar filtros en las consultas de DNS y mucho más. Los elementos siguientes proporcionan más detalles sobre estas capacidades.

-   **Equilibrio de carga de aplicación.** Cuando implemente varias instancias de una aplicación en diferentes ubicaciones, puede usar la directiva de DNS para equilibrar la carga de tráfico entre las instancias de aplicación diferente, asignar dinámicamente la carga de tráfico de la aplicación.

-   **Geográfica\-ubicación en función de administración del tráfico.** Puede usar Directiva de DNS para permitir que los servidores DNS principal y secundario responder a consultas de cliente DNS basadas en la ubicación geográfica del cliente y el recurso al que está intentando conectarse, el cliente proporciona al cliente con la dirección IP de la más cercana recurso. 

-   **Dividir el DNS de cerebro.** Con división\-cerebral DNS, los registros DNS se dividen en distintos ámbitos de zona en el mismo servidor DNS y los clientes DNS reciben una respuesta basándose en si los clientes son los clientes internos o externos. Puede configurar división\-cerebral DNS para las zonas integradas en Active Directory o para las zonas en los servidores DNS independientes.

-   **El filtrado.** Puede configurar la directiva DNS para crear filtros de consulta que se basan en criterios que suministre. Filtros de consulta en la directiva DNS permiten configurar el servidor DNS para responder de manera personalizada basada en la consulta DNS y el cliente DNS que envía la consulta DNS. 
-   **Análisis forense.** Puede usar Directiva de DNS para redirigir los clientes malintencionados de DNS a un no\-dirección IP existente en lugar de dirigiéndolas al equipo que están intentando alcanzar.

-   **Hora del día en función de redirección.** Puede usar la directiva de DNS para distribuir el tráfico de aplicación a través de distintas instancias distribuidas geográficamente de una aplicación mediante el uso de las directivas DNS que se basan en la hora del día. 
  
También puede usar las directivas DNS para DNS integrado en Active Directory zonas.

Para obtener más información, consulte el [Guía del escenario de directiva DNS](deploy/DNS-Policies-Overview.md).

## <a name="response-rate-limiting"></a>Limitación de velocidad de respuesta

Puede configurar opciones de RRL para controlar cómo responder a las solicitudes a un cliente DNS cuando el servidor recibe varias solicitudes como destino al mismo cliente. Al hacerlo, se puede evitar que alguien envíe un ataque por denegación de servicio (Dos) mediante los servidores DNS. Por ejemplo, un bot de net puede enviar solicitudes al servidor DNS con la dirección IP de un tercer equipo como el solicitante. Sin RRL, los servidores DNS podrían responder a todas las solicitudes, saturar el tercer equipo. Cuando usas RRL, puede configurar las siguientes opciones:  
  
-   **Respuestas por segundo**. Este es el número máximo de veces que se le ofrecerá la misma respuesta a un cliente en un segundo.  
  
-   **Errores por segundo**. Este es el número máximo de veces que se enviará una respuesta de error para el mismo cliente en un segundo.  
  
-   **Ventana**. Este es el número de segundos para que las respuestas a un cliente se suspenderá si se realizan demasiadas solicitudes.  
  
-   **Tasa de pérdida**. Se trata de la frecuencia con el servidor DNS responderá a una consulta durante el tiempo que se suspenden las respuestas. Por ejemplo, si el servidor suspende las respuestas a un cliente durante 10 segundos y la tasa de pérdida es 5, el servidor todavía responderá a una consulta para cada 5 consultas enviadas. Esto permite que los clientes legítimos obtener respuestas incluso cuando el servidor DNS está aplicando en su subred o el FQDN de limitación de velocidad de respuesta.  
  
-   **Tasa de TC**. Esto se utiliza para indicar al cliente que se intenta conectar con TCP, cuando se suspenden las respuestas al cliente. Por ejemplo, si la tasa de TC es 3, y el servidor suspende las respuestas a un cliente determinado, el servidor emitirá una solicitud de conexión TCP para cada 3 consultas recibidas. Asegúrese de que el valor de velocidad de TC es menor que la tasa de pérdida, para ofrecer al cliente la opción para conectarse a través de TCP antes de la pérdida de las respuestas.  
  
-   **Las respuestas máximas**. Este es el número máximo de respuestas que emitirá el servidor a un cliente mientras se suspenden las respuestas.  
  
-   **Dominios de la lista blanca**. Se trata de una lista de dominios que desea excluir de la configuración de RRL.  
  
-   **Las subredes de la lista blanca**. Se trata de una lista de subredes que se excluirán de la configuración de RRL.  
  
-   **Interfaces de servidor en la lista blanca**. Se trata de una lista de interfaces de servidor DNS que desea excluir de la configuración de RRL.  
  
## <a name="dane-support"></a>Soporte técnico de panel

Puede usar la compatibilidad con panel \(6394 RFC y 6698\) especificar a los clientes DNS qué esperan los certificados se emiten desde para los nombres de dominios de la entidad de certificación alojada en el servidor DNS. Esto evita una forma de ataque man-in-the-middle donde alguien es capaz de dañar una memoria caché DNS y seleccione un nombre DNS a su propia dirección IP.  
  
Por ejemplo, imagine que hospeda un sitio Web seguro que usa SSL en www.contoso.com mediante un certificado de una entidad muy conocido denominado CA1. Alguien posible que pueda obtener un certificado para www.contoso.com desde una diferente, no por lo que-conocido, certificado de entidad denominado CA2. A continuación, la entidad que aloja el sitio Web de www.contoso.com falsa puede dañar la memoria caché DNS de un cliente o servidor para que apunte www.contoto.com a su sitio falsa. El usuario final aparecerá un certificado de CA2 y es posible que simplemente acuse de recibo y conectarse al sitio Web falso. Con el panel, el cliente podría realizar una solicitud al servidor DNS para "contoso.com" que solicita el registro TLSA y aprenda que el certificado para www.contoso.com era problemas por CA1. Si se presenta con un certificado de otra entidad de certificación, se anula la conexión.  
  
## <a name="unknown-record-support"></a>Compatibilidad con el registro desconocido

Un "registro desconocido" es un RR cuyo formato RDATA no se sabe que el servidor DNS. La compatibilidad agregada para tipos de registro desconocido (RFC 3597) significa que puede agregar los tipos de registro no admitidos en las zonas de DNS de Windows server en formato binario en el cable. Las ventanas de resolución de caché ya tiene la capacidad para procesar los tipos de registro desconocido. Servidor DNS de Windows no realizará ningún procesamiento registro específico para los registros desconocidos, pero le enviará en las respuestas si se recibe una consulta para ella.  
  
## <a name="ipv6-root-hints"></a>Sugerencias de raíz de IPv6

Las sugerencias de raíz de IPV6, como se publica en IANA, se agregaron al servidor DNS de windows. Las consultas de nombres de internet ahora pueden usar los servidores raíz de IPv6 para realizar la resolución de nombres.

## <a name="windows-powershell-support"></a>Compatibilidad con Windows PowerShell

Los siguientes nuevos cmdlets de Windows PowerShell y los parámetros se introdujeron en Windows Server 2016.
  
-   **Add-DnsServerRecursionScope**. Este cmdlet crea un nuevo ámbito de recursividad en el servidor DNS. Ámbitos de recursividad se usan las directivas de DNS para especificar una lista de reenviadores para usarse en una consulta DNS.  
  
-   **Remove-DnsServerRecursionScope**. Este cmdlet quita los ámbitos de recursividad existentes.  
  
-   **Set-DnsServerRecursionScope**. Este cmdlet cambia la configuración de un ámbito de recursividad existente.  
  
-   **Get-DnsServerRecursionScope**. Este cmdlet recupera información sobre los ámbitos de recursividad existente.  
  
-   **Add-DnsServerClientSubnet**. Este cmdlet crea una nueva subred de cliente DNS. Las subredes se usan las directivas de DNS para identificar donde se encuentra un cliente DNS.  
  
-   **Remove-DnsServerClientSubnet**. Este cmdlet quita las subredes de cliente DNS existentes.  
  
-   **Set-DnsServerClientSubnet**. Este cmdlet cambia la configuración de una subred de cliente DNS existente.  
  
-   **Get-DnsServerClientSubnet**. Este cmdlet recupera información acerca de las subredes de cliente DNS existentes.  
  
-   **Add-DnsServerQueryResolutionPolicy**. Este cmdlet crea una nueva directiva de resolución de consultas DNS. Directivas de resolución de consultas DNS se usan para especificar cómo hacerlo, o si una consulta se responde, según criterios diferentes.  
  
-   **Remove-DnsServerQueryResolutionPolicy**. Este cmdlet quita las directivas existentes de DNS.  
  
-   **Set-DnsServerQueryResolutionPolicy**. Este cmdlet cambia la configuración de una directiva DNS existente.  
  
-   **Get-DnsServerQueryResolutionPolicy**. Este cmdlet recupera información sobre las directivas existentes de DNS.  
  
-   **Enable-DnsServerPolicy**. Este cmdlet permite que las directivas existentes de DNS.  
  
-   **Disable-DnsServerPolicy**. Este cmdlet deshabilita las directivas existentes de DNS.  
  
-   **Add-DnsServerZoneTransferPolicy**. Este cmdlet crea una nueva directiva de transferencia de zona de servidor DNS. Las directivas de transferencia de zona DNS especifican si desea denegar u omitir una transferencia de zona según criterios diferentes.  
  
-   **Remove-DnsServerZoneTransferPolicy**. Este cmdlet quita las directivas de transferencia de zona de servidor DNS existentes.  
  
-   **Set-DnsServerZoneTransferPolicy**. Este cmdlet cambia la configuración de una directiva de transferencia de zona de servidor DNS existente.  
  
-   **Get-DnsServerResponseRateLimiting**. Este cmdlet recupera la configuración de RRL.  
  
-   **Set-DnsServerResponseRateLimiting**. Este cmdlet cambia la configuración de RRL.  
  
-   **Add-DnsServerResponseRateLimitingExceptionlist**. Este cmdlet crea una lista de excepciones RRL en el servidor DNS.  
  
-   **Get-DnsServerResponseRateLimitingExceptionlist**. Este cmdlet recupera RRL excception listas.  
  
-   **Remove-DnsServerResponseRateLimitingExceptionlist**. Este cmdlet quita una lista de excepciones RRL existente.  
  
-   **Set-DnsServerResponseRateLimitingExceptionlist**. Este cmdlet cambia RRL listas de excepciones.  
  
-   **Add-DnsServerResourceRecord**. Este cmdlet se ha actualizado para admitir el tipo de registro desconocido.  
  
-   **Get-DnsServerResourceRecord**. Este cmdlet se ha actualizado para admitir el tipo de registro desconocido.  
  
-   **Remove-DnsServerResourceRecord**. Este cmdlet se ha actualizado para admitir el tipo de registro desconocido.  
  
-   **Set-DnsServerResourceRecord**. Este cmdlet se ha actualizado para admitir el tipo de registro desconocido

Para obtener más información, consulte los siguientes temas de referencia de comandos de Windows PowerShell de Windows Server 2016.

- [Módulo DnsServer](https://docs.microsoft.com/powershell/module/dnsserver/?view=win10-ps)
- [Módulo DnsClient](https://docs.microsoft.com/powershell/module/dnsclient/?view=win10-ps)

## <a name="see-also"></a>Vea también  
  
-   [Novedades en el cliente DNS](What-s-New-in-DNS-Client.md)  
  

  

