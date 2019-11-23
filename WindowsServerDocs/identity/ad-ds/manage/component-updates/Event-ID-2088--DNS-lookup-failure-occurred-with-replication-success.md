---
ms.assetid: 0fd7b6aa-3e50-45a3-a3a6-56982844363e
title: ID. de evento 2088-error de búsqueda de DNS con replicación correcta
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: d51cbcc93a8decbcb72a1e91854a09345507511d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71368911"
---
# <a name="event-id-2088-dns-lookup-failure-occurred-with-replication-success"></a>Id. de evento 2088: se produjo un error de búsqueda de DNS con replicación correcta

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

    
    <developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd">
      <introduction>
    <para>When a destination domain controller running Windows Server 2003 with Service Pack 1 (SP1) receives Event ID 2088 in the Directory Service event log, attempts to resolve the globally unique identifier (GUID) in the alias (CNAME) resource record to an IP address for the source domain controller failed. However, the destination domain controller tried other means to resolve the name and succeeded by using either the fully qualified domain name (FQDN) or the NetBIOS name of the source domain controller. Although replication was successful, the Domain Name System (DNS) problem should be diagnosed and resolved. </para>
    <para>The following is an example of the event text: </para>
    <code>Log Name: Directory Service

    Source: Microsoft-Windows-ActiveDirectory_DomainService
    Date: 3/15/2008  9:20:11 AM
    Event ID: 2088
    Task Category: DS RPC Client 
    Level: Warning
    Keywords: Classic
    User: ANONYMOUS LOGON
    Computer: DC3.contoso.com
    Description:
    Active Directory could not use DNS to resolve the IP address of the 
    source domain controller listed below. To maintain the consistency 
    of Security groups, group policy, users and computers and their passwords, 
    Active Directory Domain Services successfully replicated using the NetBIOS 
    or fully qualified computer name of the source domain controller. 

La configuración de DNS no válida puede afectar a otras operaciones esenciales en los equipos miembro, controladores de dominio o servidores de aplicaciones de este bosque de Active Directory Domain Services, incluida la autenticación de inicio de sesión o el acceso a los recursos de red. 

Debes resolver inmediatamente este error de configuración de DNS para que este controlador de dominio pueda resolver la dirección IP del controlador de dominio de origen mediante DNS. 

Nombre de servidor alternativo: error de DC1 nombre de host DNS: 4a8717eb-8e58-456c-995a-c92e4add7e8e. _msdcs. contoso. com 

Nota: de forma predeterminada, solo se muestran hasta 10 errores de DNS durante un período de 12 horas determinado, incluso si se producen más de 10 errores.  Para registrar todos los eventos de error individuales, establezca el siguiente valor del registro de diagnóstico en 1: 

Ruta de acceso del registro: cliente RPC de HKLM\System\CurrentControlSet\Services\NTDS\Diagnostics\22 DS 

Acción del usuario: 

1) Si el controlador de dominio de origen ya no funciona o su sistema operativo se ha reinstalado con un nombre de equipo o un GUID de objeto NTDSDSA diferentes, quite los metadatos del controlador de dominio de origen con Ntdsutil. exe, siguiendo los pasos descritos en el artículo MSKB 216498. 

2) Confirme que el controlador de dominio de origen se está ejecutando Active Directory y que es accesible en la red. para ello, escriba "net view \\&lt;nombre del DC de origen&gt;" o "ping &lt;nombre del DC de origen&gt;". 

3) Compruebe que el controlador de dominio de origen usa un servidor DNS válido para los servicios DNS y que el registro del host del controlador de dominio de origen y el registro CNAME se han registrado correctamente, mediante la versión mejorada de DNS de DCDIAG. EXE disponible en <https://www.microsoft.com/dns> 

dcdiag/test: DNS 

4) Compruebe que este controlador de dominio de destino esté usando un servidor DNS válido para los servicios DNS; para ello, ejecute la versión mejorada de DNS de DCDIAG. En la consola del controlador de dominio de destino, como se indica a continuación: 

dcdiag/test: DNS 

5) Para más información sobre el análisis de errores de DNS, consulte KB 824449: <https://support.microsoft.com/?kbid=824449> 

Valor de error de datos adicional: 11004 el nombre solicitado es válido, pero no se encontró ningún dato del tipo solicitado</code> </introduction>
  <section>
       de
    <content>de 
    <title>diagnóstico</title><para>Un error al resolver el nombre del controlador de dominio de origen mediante el registro de recursos de alias (CNAME) en DNS puede deberse a errores de configuración o retrasos de DNS en la propagación de datos DNS.</para>
    </content>
  </section>
  <section>
    <content>de 
    <title>resolución</title> 
      <para>Continúe con las pruebas de DNS como se describe en &quot;ID. de <link xlink:href="85b1d179-f53e-4f95-b0b8-5b1c096a8076">evento 2087: error de búsqueda de DNS que causó un error en la replicación</link>.&quot;</para>
    </content>
  </section>
  <relatedTopics />
</developerConceptualDocument>


