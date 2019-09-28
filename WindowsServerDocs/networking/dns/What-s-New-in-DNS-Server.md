---
title: Novedades del servidor DNS en Windows Server
description: En este tema se proporciona información general sobre las nuevas características del servidor DNS en Windows Server 2016 y versiones posteriores.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: c9cecb94-3cd5-4da7-9a3e-084148b8226b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: de502d7be023d12e3350063e467a60356b2472c4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406239"
---
# <a name="whats-new-in-dns-server-in-windows-server"></a>Novedades del servidor DNS en Windows Server

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se describen las funciones de servidor de sistema de nombres de dominio (DNS) nuevas o modificadas en Windows Server 2016.  
  
En Windows Server 2016, el servidor DNS ofrece compatibilidad mejorada en las áreas siguientes.  
  
|Funcionalidad|Nueva o mejorada|Descripción|  
|-----------------|-------------------|---------------|  
|Directivas de DNS|Nuevo|Puede configurar directivas DNS para especificar el modo en que un servidor DNS responde a las consultas DNS. Las respuestas DNS pueden basarse en la dirección IP del cliente (ubicación), la hora del día y otros parámetros. Las directivas de DNS permiten el DNS, la administración del tráfico, el equilibrio de carga, el DNS de cerebro dividido y otros escenarios que tienen en cuenta la ubicación.|  
|Limitación de velocidad de respuesta (RRL)|Nuevo|Puede habilitar la limitación de la velocidad de respuesta en los servidores DNS. Al hacerlo, evita la posibilidad de que los sistemas malintencionados usen los servidores DNS para iniciar un ataque de denegación de servicio en un cliente DNS.|  
|Autenticación basada en DNS de entidades con nombre (SUNDANÉS)|Nuevo|Puede usar los registros TLSA (autenticación de seguridad de la capa de transporte) para proporcionar información a los clientes DNS que indiquen en qué CA deben esperar un certificado para su nombre de dominio. Esto evita ataques de tipo "Man in the Middle", en los que alguien podría dañar la memoria caché de DNS para apuntar a su propio sitio web y proporcionar un certificado emitido por una CA diferente.|  
|Compatibilidad con registros desconocidos|Nuevo|Puede agregar registros que no son compatibles explícitamente con el servidor DNS de Windows mediante la funcionalidad de registros desconocidos.|  
|Sugerencias de raíz IPv6|Nuevo|Puede usar la compatibilidad con las sugerencias de raíz IPV6 nativas para realizar la resolución de nombres de Internet mediante los servidores raíz IPV6.|  
|Compatibilidad con Windows PowerShell|Mejorada|Hay nuevos cmdlets de Windows PowerShell disponibles para el servidor DNS.|  
  
## <a name="dns-policies"></a>Directivas de DNS

Puede usar la Directiva DNS para la administración del tráfico basada en la ubicación geográfica, las respuestas de DNS inteligentes basadas en la hora del día, para administrar un solo servidor DNS configurado para la implementación de Split @ no__t-0brain, aplicar filtros en consultas DNS, etc. Los elementos siguientes proporcionan más detalles acerca de estas capacidades.

-   **Equilibrio de carga de la aplicación.** Cuando haya implementado varias instancias de una aplicación en diferentes ubicaciones, puede usar la Directiva de DNS para equilibrar la carga de tráfico entre las distintas instancias de aplicación, asignando dinámicamente la carga de tráfico para la aplicación.

-   **Administración del tráfico basado en Geo @ no__t-1Location.** Puede usar la Directiva de DNS para permitir que los servidores DNS principal y secundario respondan a las consultas de cliente DNS en función de la ubicación geográfica del cliente y el recurso al que el cliente intenta conectarse, proporcionando al cliente la dirección IP del recurso. 

-   **Divida DNS de cerebro.** Con el DNS dividido @ no__t-0brain, los registros DNS se dividen en diferentes ámbitos de zona en el mismo servidor DNS y los clientes DNS reciben una respuesta en función de si los clientes son internos o externos. Puede configurar el DNS dividido @ no__t-0brain para las zonas integradas Active Directory o para las zonas de los servidores DNS independientes.

-   **Filtra.** Puede configurar la Directiva de DNS para crear filtros de consulta basados en los criterios que proporcione. Los filtros de consulta de la Directiva de DNS permiten configurar el servidor DNS para responder de forma personalizada en función de la consulta DNS y el cliente DNS que envía la consulta DNS. 
-   **Análisis forense.** Puede usar la Directiva de DNS para redirigir a los clientes DNS malintencionados a una dirección IP que no sea de no__t-0existent en lugar de dirigirlos al equipo al que intentan acceder.

-   **Hora del redireccionamiento basado en el día.** Puede usar la Directiva de DNS para distribuir el tráfico de aplicaciones entre diferentes instancias distribuidas geográficamente de una aplicación mediante el uso de directivas DNS basadas en la hora del día. 
  
También puede usar directivas DNS para Active Directory zonas DNS integradas.

Para obtener más información, consulte la guía del escenario de la [Directiva DNS](deploy/DNS-Policies-Overview.md).

## <a name="response-rate-limiting"></a>Limitación de velocidad de respuesta

Puede establecer la configuración de RRL para controlar cómo responder a las solicitudes a un cliente DNS cuando el servidor recibe varias solicitudes que tienen como destino el mismo cliente. Al hacerlo, puede impedir que alguien envíe un ataque por denegación de servicio (dos) mediante los servidores DNS. Por ejemplo, una red bot puede enviar solicitudes al servidor DNS mediante la dirección IP de un tercer equipo como solicitante. Sin RRL, los servidores DNS podrían responder a todas las solicitudes y inundar el tercer equipo. Al usar RRL, puede configurar las siguientes opciones:  
  
-   **Respuestas por segundo**. Este es el número máximo de veces que se proporcionará la misma respuesta a un cliente en un segundo.  
  
-   **Errores por segundo**. Este es el número máximo de veces que se enviará una respuesta de error al mismo cliente en un segundo.  
  
-   **Ventana**. Es el número de segundos durante los cuales se suspenderán las respuestas a un cliente si se realizan demasiadas solicitudes.  
  
-   **Tasa de fugas**. Esta es la frecuencia con la que el servidor DNS responderá a una consulta durante el tiempo que se suspenden las respuestas. Por ejemplo, si el servidor suspende las respuestas a un cliente durante 10 segundos y la tasa de fugas es 5, el servidor seguirá respondiendo a una consulta por cada 5 consultas enviadas. Esto permite a los clientes legítimos obtener respuestas incluso cuando el servidor DNS está aplicando la limitación de velocidad de respuesta en su subred o FQDN.  
  
-   **Tasa de TC**. Se utiliza para indicar al cliente que intente conectarse con TCP cuando se suspenda la respuesta al cliente. Por ejemplo, si la tasa de TC es 3 y el servidor suspende las respuestas a un cliente determinado, el servidor emitirá una solicitud de conexión TCP por cada 3 consultas recibidas. Asegúrese de que el valor de la tasa de TC es inferior a la tasa de fugas, para dar al cliente la opción de conectarse a través de TCP antes de filtrar las respuestas.  
  
-   **Número máximo de respuestas**. Este es el número máximo de respuestas que el servidor emitirá a un cliente mientras se suspenden las respuestas.  
  
-   **Dominios de lista blanca**. Esta es una lista de dominios que se van a excluir de la configuración de RRL.  
  
-   **Subredes de lista blanca**. Esta es una lista de subredes que se van a excluir de la configuración de RRL.  
  
-   **Interfaces de servidor de lista blanca**. Esta es una lista de las interfaces de servidor DNS que se van a excluir de la configuración de RRL.  
  
## <a name="dane-support"></a>Compatibilidad con SUNDANÉS

Puede usar la compatibilidad con SUNDANÉS \(RFC 6394 y 6698 @ no__t-1 para especificar a los clientes DNS en qué CA deben esperar que se emitan los certificados para los nombres de dominios hospedados en el servidor DNS. Esto evita una forma de ataque de tipo "Man in the Middle", donde alguien puede dañar una memoria caché de DNS y apuntar un nombre DNS a su propia dirección IP.  
  
Por ejemplo, Imagine que hospeda un sitio web seguro que usa SSL en www.contoso.com con un certificado de una autoridad conocida denominada CA1. Es posible que alguien siga pudiendo obtener un certificado para www.contoso.com desde una entidad de certificación distinta, no conocida, denominada CA2. A continuación, la entidad que hospeda el sitio web de www.contoso.com falso podría dañar la memoria caché de DNS de un cliente o servidor para apuntar a www.contoto.com a su sitio falso. Al usuario final se le presentará un certificado de CA2 y puede simplemente confirmarlo y conectarse al sitio falso. Con SUNDANÉS, el cliente realizará una solicitud al servidor DNS para que contoso.com solicite el registro TLSA y aprenderá que el certificado de www.contoso.com fue issued by CA1. Si se presenta un certificado de otra CA, la conexión se anula.  
  
## <a name="unknown-record-support"></a>Compatibilidad con registros desconocidos

Un "registro desconocido" es un RR cuyo formato de RDATA no es conocido para el servidor DNS. La compatibilidad recién agregada para tipos de registros desconocidos (RFC 3597) significa que puede Agregar los tipos de registro no admitidos a las zonas del servidor DNS de Windows en el formato binario en el formato de conexión. El solucionador de almacenamiento en caché de Windows ya tiene la capacidad de procesar tipos de registros desconocidos. El servidor DNS de Windows no realizará ningún procesamiento específico del registro para los registros desconocidos, pero lo enviará en respuestas si se reciben consultas.  
  
## <a name="ipv6-root-hints"></a>Sugerencias de raíz IPv6

Las sugerencias de raíz IPV6, publicadas por IANA, se han agregado al servidor DNS de Windows. Las consultas de nombre de Internet ahora pueden usar servidores raíz IPv6 para realizar resoluciones de nombres.

## <a name="windows-powershell-support"></a>Compatibilidad con Windows PowerShell

Los siguientes nuevos cmdlets y parámetros de Windows PowerShell se introdujeron en Windows Server 2016.
  
-   **Add-DnsServerRecursionScope**. Este cmdlet crea un nuevo ámbito de recursividad en el servidor DNS. Las directivas de DNS usan los ámbitos de recursividad para especificar una lista de los reenviadores que se van a usar en una consulta DNS.  
  
-   **Remove-DnsServerRecursionScope**. Este cmdlet quita los ámbitos de recursividad existentes.  
  
-   **Set-DnsServerRecursionScope**. Este cmdlet cambia la configuración de un ámbito de recursividad existente.  
  
-   **Get-DnsServerRecursionScope**. Este cmdlet recupera información sobre los ámbitos de recursividad existentes.  
  
-   **Add-DnsServerClientSubnet**. Este cmdlet crea una nueva subred de cliente DNS. Las directivas DNS usan las subredes para identificar dónde se encuentra un cliente DNS.  
  
-   **Remove-DnsServerClientSubnet**. Este cmdlet quita las subredes de cliente DNS existentes.  
  
-   **Set-DnsServerClientSubnet**. Este cmdlet cambia la configuración de una subred de cliente DNS existente.  
  
-   **Get-DnsServerClientSubnet**. Este cmdlet recupera información sobre las subredes de cliente DNS existentes.  
  
-   **Add-DnsServerQueryResolutionPolicy**. Este cmdlet crea una nueva Directiva de resolución de consultas DNS. Las directivas de resolución de consultas de DNS se usan para especificar cómo se responde una consulta, en función de criterios diferentes.  
  
-   **Remove-DnsServerQueryResolutionPolicy**. Este cmdlet quita las directivas DNS existentes.  
  
-   **Set-DnsServerQueryResolutionPolicy**. Este cmdlet cambia la configuración de una directiva DNS existente.  
  
-   **Get-DnsServerQueryResolutionPolicy**. Este cmdlet recupera información sobre las directivas DNS existentes.  
  
-   **Enable-DnsServerPolicy**. Este cmdlet habilita las directivas DNS existentes.  
  
-   **Disable-DnsServerPolicy**. Este cmdlet deshabilita las directivas DNS existentes.  
  
-   **Add-DnsServerZoneTransferPolicy**. Este cmdlet crea una nueva Directiva de transferencia de zona del servidor DNS. Las directivas de transferencia de zona DNS especifican si se debe denegar u omitir una transferencia de zona en función de criterios diferentes.  
  
-   **Remove-DnsServerZoneTransferPolicy**. Este cmdlet quita las directivas de transferencia de zona de servidor DNS existentes.  
  
-   **Set-DnsServerZoneTransferPolicy**. Este cmdlet cambia la configuración de una directiva de transferencia de zona de servidor DNS existente.  
  
-   **Get-DnsServerResponseRateLimiting**. Este cmdlet recupera la configuración de RRL.  
  
-   **Set-DnsServerResponseRateLimiting**. Este cmdlet cambia el dicha extendería de RRL.  
  
-   **Add-DnsServerResponseRateLimitingExceptionlist**. Este cmdlet crea una lista de excepciones de RRL en el servidor DNS.  
  
-   **Get-DnsServerResponseRateLimitingExceptionlist**. Este cmdlet recupera las listas de excception de RRL.  
  
-   **Remove-DnsServerResponseRateLimitingExceptionlist**. Este cmdlet quita una lista de excepciones de RRL existente.  
  
-   **Set-DnsServerResponseRateLimitingExceptionlist**. Este cmdlet cambia las listas de excepciones de RRL.  
  
-   **Add-DnsServerResourceRecord**. Este cmdlet se actualizó para admitir el tipo de registro desconocido.  
  
-   **Get-DnsServerResourceRecord**. Este cmdlet se actualizó para admitir el tipo de registro desconocido.  
  
-   **Remove-DnsServerResourceRecord**. Este cmdlet se actualizó para admitir el tipo de registro desconocido.  
  
-   **Set-DnsServerResourceRecord**. Este cmdlet se actualizó para admitir el tipo de registro desconocido

Para obtener más información, vea los siguientes temas de referencia de comandos de Windows Server 2016 Windows PowerShell.

- [Módulo DnsServer](https://docs.microsoft.com/powershell/module/dnsserver/?view=win10-ps)
- [Módulo DnsClient](https://docs.microsoft.com/powershell/module/dnsclient/?view=win10-ps)

## <a name="see-also"></a>Vea también  
  
-   [Novedades del cliente DNS](What-s-New-in-DNS-Client.md)  
  

  

