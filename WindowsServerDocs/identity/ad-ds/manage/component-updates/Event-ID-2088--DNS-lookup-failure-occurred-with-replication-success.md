---
ms.assetid: 0fd7b6aa-3e50-45a3-a3a6-56982844363e
title: ID. de evento 2088-error de búsqueda de DNS con replicación correcta
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: a54b13121933d2780ada9e68a9a7656709947c22
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/03/2020
ms.locfileid: "87518863"
---
# <a name="event-id-2088-dns-lookup-failure-occurred-with-replication-success"></a>Id. de evento 2088: se produjo un error de búsqueda de DNS con replicación correcta

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cuando un controlador de dominio de destino que ejecuta Windows Server 2003 con Service Pack 1 (SP1) recibe el ID. de evento 2088 en el registro de eventos del servicio de directorio, se produce un error al intentar resolver el identificador único global (GUID) en el registro de recursos de alias (CNAME) en una dirección IP para el controlador de dominio de origen. Sin embargo, el controlador de dominio de destino probó otros medios para resolver el nombre y se realizó correctamente mediante el nombre de dominio completo (FQDN) o el nombre NetBIOS del controlador de dominio de origen. Aunque la replicación se realizó correctamente, el problema del sistema de nombres de dominio (DNS) debe diagnosticarse y resolverse.

A continuación se muestra un ejemplo del texto del evento:

```
Log Name: Directory Service
Source: Microsoft-Windows-ActiveDirectory_DomainService
Date: 3/15/2008  9:20:11 AM
Event ID: 2088
Task Category: DS RPC Client
Level: Warning
Keywords: Classic
User: ANONYMOUS LOGON
Computer: DC3.contoso.com
Description:
Active Directory could not use DNS to resolve the IP address of the source domain controller listed below. To maintain the consistency of Security groups, group policy, users and computers and their passwords, Active Directory Domain Services successfully replicated using the NetBIOS or fully qualified computer name of the source domain controller.
```

La configuración de DNS no válida puede afectar a otras operaciones esenciales en los equipos miembro, controladores de dominio o servidores de aplicaciones de este bosque de Active Directory Domain Services, incluida la autenticación de inicio de sesión o el acceso a los recursos de red.

Debes resolver inmediatamente este error de configuración de DNS para que este controlador de dominio pueda resolver la dirección IP del controlador de dominio de origen mediante DNS.

Nombre de servidor alternativo: error de DC1 nombre de host DNS: 4a8717eb-8e58-456c-995a-c92e4add7e8e. _msdcs. contoso. com

Nota: de forma predeterminada, solo se muestran hasta 10 errores de DNS durante un período de 12 horas determinado, incluso si se producen más de 10 errores.  Para registrar todos los eventos de error individuales, establezca el siguiente valor del registro de diagnóstico en 1:

Ruta de acceso del registro: cliente RPC de HKLM\System\CurrentControlSet\Services\NTDS\Diagnostics\22 DS

Acción del usuario:

1) Si el controlador de dominio de origen ya no funciona o su sistema operativo se ha reinstalado con un nombre de equipo o GUID de objeto de NTDSDSA diferente, quite los metadatos del controlador de dominio de origen con ntdsutil.exe, siguiendo los pasos descritos en el artículo de MSKB 216498.

2) Confirme que el controlador de dominio de origen se está ejecutando Active Directory y que es accesible en la red. para ello, escriba "net view \\ <source DC name> " o "ping <source DC name> ".

3) Compruebe que el controlador de dominio de origen usa un servidor DNS válido para los servicios DNS y que el registro del host del controlador de dominio de origen y el registro CNAME están registrados correctamente, mediante la versión mejorada de DNS de DCDIAG.EXE disponible en<https://www.microsoft.com/dns>

dcdiag/test: DNS

4) Compruebe que este controlador de dominio de destino usa un servidor DNS válido para los servicios DNS; para ello, ejecute la versión mejorada de DNS de DCDIAG.EXE comando en la consola del controlador de dominio de destino, como se indica a continuación:

dcdiag/test: DNS

5) Para más información sobre el análisis de errores de DNS, consulte KB 824449:<https://support.microsoft.com/?kbid=824449>

Valor de error de datos adicionales: 11004 el nombre solicitado es válido, pero no se encontró </code> ningún dato del tipo solicitado</introduction>
  <section>
    <title>Diagnóstico</title>
    <content>
      <para>Un error al resolver el nombre del controlador de dominio de origen mediante el registro de recursos de alias (CNAME) en DNS puede deberse a errores de configuración o retrasos de DNS en la propagación de datos DNS.</para>
    </content>
  </section>
  <section>
    <title>Resolución</title>
    <content>
      <para>Continúe con las pruebas de DNS tal y como se describe en ID. de &quot; <link xlink:href="85b1d179-f53e-4f95-b0b8-5b1c096a8076">evento 2087: error de búsqueda DNS que provocó un error en la replicación</link>.&quot;</para>
    </content>
  </section>
  <relatedTopics />
</developerConceptualDocument>
