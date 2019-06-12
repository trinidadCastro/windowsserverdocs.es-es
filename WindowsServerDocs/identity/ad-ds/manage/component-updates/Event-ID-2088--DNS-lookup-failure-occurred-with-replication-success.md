---
ms.assetid: 0fd7b6aa-3e50-45a3-a3a6-56982844363e
title: 'Id. de evento 2088: se ha producido un error de búsqueda DNS con éxito de replicación'
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e0c5e838290a8ebf33f0f7891dc10f8b00e5bcba
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442652"
---
# <a name="event-id-2088-dns-lookup-failure-occurred-with-replication-success"></a>Id. de evento 2088: Se ha producido un error de búsqueda DNS con éxito de replicación

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

Configuración de DNS no válido puede afectar a otras operaciones importantes en los equipos miembros, los controladores de dominio o servidores de aplicaciones en este bosque de Active Directory Domain Services, incluida la autenticación de inicio de sesión o acceso a los recursos de red. 

Debe resolver inmediatamente este error de configuración de DNS para que este controlador de dominio pueda resolver la dirección IP del controlador de dominio de origen mediante DNS. 

Nombre de servidor alternativo: Nombre de host DNS de DC1 con errores: 4a8717eb-8e58-456c-995a-c92e4add7e8e._msdcs.contoso.com 

Nota: De forma predeterminada, se muestran hasta 10 errores de DNS en cualquier período de 12 horas determinado, incluso si se producen más de 10 errores.  Para registrar todos los eventos de error individuales, establezca el diagnóstico siguiente valor del registro en 1: 

Ruta de acceso del registro: HKLM\System\CurrentControlSet\Services\NTDS\Diagnostics\22 DS RPC Client 

Acción del usuario: 

1) Si el controlador de dominio de origen está ya no funciona o su sistema operativo se ha reinstalado con un nombre de equipo diferente o GUID, del objeto NTDSDSA quitar los metadatos del controlador de dominio de origen con ntdsutil.exe, siguiendo los pasos descritos en el artículo 216498. 

2) Confirme que el controlador de dominio de origen está ejecutando Active Directory y es accesible en la red escribiendo "net view \\ &lt;nombre de DC de origen&gt;" o "ping &lt;nombre de DC de origen&gt;". 

3) Compruebe que el controlador de dominio de origen usa un servidor DNS válido para los servicios DNS y que el controlador de dominio de origen registro de host y CNAME de registro están registrados correctamente, con la versión mejorada de DNS de DCDIAG. EXE disponible en <https://www.microsoft.com/dns> 

dcdiag /test:dns 

4) Compruebe que este controlador de dominio de destino usa un servidor DNS válido para los servicios DNS mediante la ejecución de la versión de DNS mejorado de DCDIAG. Comando EXE en la consola del controlador de dominio de destino, como se indica a continuación: 

dcdiag /test:dns 

5) Para realizar un análisis de errores DNS, consulte 824449 KB: <https://support.microsoft.com/?kbid=824449> 

Datos adicionales Valor de error: 11004 el nombre solicitado es válido, pero no se encontró ningún dato del tipo solicitado</code> </introduction>
  <section>
    <title>Diagnóstico más detallado</title>
    <content>
      <para>No se puede resolver el nombre del controlador de dominio de origen utilizando el registro de recursos de alias (CNAME) en DNS puede ser debido a errores de configuración de DNS o retrasos en la propagación de los datos DNS.</para>
    </content>
  </section>
  <section>
    <title>Resolución</title>
    <content>
      <para>Continuar con la prueba de DNS como se describe en &quot; <link xlink:href="85b1d179-f53e-4f95-b0b8-5b1c096a8076">identificador de evento 2087: Error de búsqueda de DNS provocó un error de replicación</link>.&quot;</para>
    </content>
  </section>
  <relatedTopics />
</developerConceptualDocument>


