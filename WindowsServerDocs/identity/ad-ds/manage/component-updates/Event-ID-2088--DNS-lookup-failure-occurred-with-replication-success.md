---
ms.assetid: 0fd7b6aa-3e50-45a3-a3a6-56982844363e
title: "Identificador de evento 2088 - se ha producido un error de búsqueda DNS con éxito de replicación"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4f223f075775f942f83a1962da28a77e85e89aa0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="event-id-2088-dns-lookup-failure-occurred-with-replication-success"></a>Identificador de evento 2088: Se produjo un error de búsqueda DNS con éxito de replicación

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

Configuración no válida de DNS puede afectar a otras operaciones esenciales en los equipos miembro, controladores de dominio o servidores de aplicaciones en el bosque de Active Directory Domain Services, incluida la autenticación de inicio de sesión o el acceso a recursos de red. 

Debería resolver inmediatamente este error de configuración de DNS para que este controlador de dominio puede resolver la dirección IP del controlador de dominio de origen mediante DNS. 

Nombre de servidor alternativo: nombre de host DNS de un error de DC1: c 4a8717eb-8e58-456-995a-c92e4add7e8e._msdcs. contoso.com 

Nota: De manera predeterminada, los errores DNS hasta 10 se muestran durante un período de 12 horas determinado, incluso si se producen errores de más de 10.  Para iniciar sesión en todos los eventos de error individuales, Establece los diagnósticos siguientes valor del registro en 1: 

Ruta de acceso del registro: Cliente de RPC DS HKLM\System\CurrentControlSet\Services\NTDS\Diagnostics\22 

Acción del usuario: 

1) Si el controlador de dominio de origen no está ya no está funcionando o del sistema operativo se ha reinstalado con un nombre de equipo diferentes o NTDSDSA objeto GUID, quitar los metadatos del controlador de dominio de origen con ntdsutil.exe, siguiendo los pasos descritos en el artículo 216498. 

2) Confirma que el controlador de dominio de origen ejecuta Active Directory y es accesible en la red escribiendo "net vista \\&lt;nombre del origen DC&gt;" o "ping &lt;nombre del origen DC&gt;". 

3) Comprueba que el controlador de dominio de origen está usando un servidor DNS válido para servicios DNS, que registran el controlador de dominio de origen registro de host y CNAME está registrado correctamente, usando la versión mejorada de DNS de DCDIAG.EXE disponible en https://www.microsoft.com/dns 

dcdiag//test: dns 

4) Comprueba que este controlador de dominio de destino está usando un servidor DNS válido para los servicios DNS ejecutando la versión mejorada de DNS de DCDIAG.EXE de comandos en la consola del controlador de dominio de destino, como sigue: 

dcdiag//test: dns 

5) Para un análisis más profundo de DNS errores consulta KB 824449: https://support.microsoft.com/?kbid=824449 

Valor de Error de datos adicional: 11004 el nombre solicitado es válido, pero no se encontró ningún dato del tipo solicitado</code>
  </introduction>
  <section>
    <title>Diagnóstico</title>
    <content>
      <para>un error al resolver el nombre del controlador de dominio de origen mediante el registro de recursos de alias (CNAME) en DNS puede ser debido a errores de configuración de DNS o retrasos en la propagación de datos DNS.</para>
    </content>
  </section>
  <section>
    <title>Resolución</title>
    <content>
      <para>continuar con las pruebas de DNS como se describe en "<link xlink:href="85b1d179-f53e-4f95-b0b8-5b1c096a8076">2087 de identificador de evento: error en la búsqueda DNS causó el error de replicación</link>."</para>
    </content>
  </section>
  <relatedTopics />
</developerConceptualDocument>


