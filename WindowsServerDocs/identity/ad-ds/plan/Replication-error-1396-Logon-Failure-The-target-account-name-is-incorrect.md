---
ms.assetid: 399a8bbe-3375-4bb0-b55b-5f46e7050028
title: "Error de replicación error al iniciar sesión 1396 el nombre de cuenta de destino es incorrecta"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 84799de26e1260f914d9b959357d5eed6fef62f6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="replication-error-1396-logon-failure-the-target-account-name-is-incorrect"></a>Error de replicación error al iniciar sesión 1396 el nombre de cuenta de destino es incorrecta

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


<developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd">
  <introduction>
    <para>Este artículo describen los síntomas, causa y cómo resolver réplica de Active Directory con el error de Win32 1396: "Error de inicio de sesión: el nombre de cuenta de destino es incorrecto." </para><list class="bullet"><listItem><para><link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Symptoms">Síntomas</link></para></listItem><listItem><para><link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Causes">hace</link></para></listItem><listItem><para><link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Resolutions">resoluciones</link></para></listItem></list>
  </introduction>
  <section address="BKMK_Symptoms">
              
    <title>Symptoms</title>
    <content>
      <para />
      <list class="ordered">
<listItem><para>DCDIAG reports that the Active Directory Replications test has failed with error 1396: Logon failure: The target account name is incorrect."</para><code>Testing server: &lt;Site name&gt;&lt;DC Name&gt;
Starting test: Replications
[Replications Check,&lt;DC Name&gt;] A recent replication attempt failed:
From &lt;source DC&gt; to &lt;destination DC&gt;
Naming Context: CN=&lt;DN path of naming context&gt;
<codeFeaturedElement>The replication generated an error (1396):
Logon Failure: The target account name is incorrect.</codeFeaturedElement>
The failure occurred at &lt;date&gt; &lt;time&gt;.
The last success occurred at &lt;date&gt; &lt;time&gt;.
XX failures have occurred since the last success</code></listItem><listItem><para>REPADMIN.EXE reports that the last replication attempt has failed with status 1396.</para><para>REPADMIN commands that commonly cite the 1396 status include but are not limited to:</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><tbody><tr><TD><list class="bullet"><listItem><para>REPADMIN /ADD</para></listItem><listItem><para>/Replsum REPADMIN</para></listItem><listItem><para>REPADMIN /REHOST</para></listItem><listItem><para>REPADMIN /SHOWVECTOR /LATENCY</para></listItem></list></TD><TD><list class="bullet"><listItem><para>REPADMIN /SHOWREPS</para></listItem><listItem><para>REPADMIN /SHOWREPL</para></listItem><listItem><para>REPADMIN/SyncAll</para></listItem></list></TD></tr></tbody></table><para>Sample output from "REPADMIN /SHOWREPS" depicting inbound replication from CONTOSO-DC2 to CONTOSO-DC1 failing with the "Logon Failure: The target account name is incorrect." error is shown below::</para><code>Default-First-Site-NameCONTOSO-DC1
DSA Options: IS_GC 
Site Options: (none)
DSA object GUID: b6dc8589-7e00-4a5d-b688-045aef63ec01
DSA invocationID: b6dc8589-7e00-4a5d-b688-045aef63ec01
==== INBOUND NEIGHBORS ======================================
DC=contoso,DC=com
Default-First-Site-NameCONTOSO-DC2 via RPC
DSA object GUID: 74fbe06c-932c-46b5-831b-af9e31f496b2
Last attempt @ &lt;date&gt; &lt;time&gt; failed, <codeFeaturedElement>result 1396 (0x574):
Logon Failure: The target account name is incorrect.</codeFeaturedElement>
&lt;#&gt; consecutive failure(s).
Last success @ &lt;date&gt; &lt;time&gt;.
</code></listItem><listItem><para>The <ui>Replicate now</ui> command in Active Directory Sites and Services returns "Logon Failure: The target account name is incorrect."</para><para>Right-clicking on the connection object from a source DC and choosing <ui>Replicate now</ui> fails with "Logon Failure: The target account name is incorrect." The on-screen error message is shown below:</para><para>Dialog title text:</para><para>Replicate Now</para><para>Dialog message text: </para><para>The following error occurred during the attempt to synchronize naming context &lt;partition DNS path&gt; from domain controller &lt;source DC&gt; to domain controller &lt;destination DC&gt;: Logon Failure: The target account name is incorrect. This operation will not continue. </para></listItem><listItem><para>NTDS KCC, NTDS General or Microsoft-Windows-ActiveDirectory_DomainService events with the 1396 status are logged in the Directory Services log in Event Viewer.</para><para>Active Directory events that commonly cite the 1396 status include but are not limited to:</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><thead><tr><TD><para>Identificador de evento</para></TD><TD><para>Origen del evento</para></TD><TD><para>Cadena de evento</para></TD></tr></thead><tbody><tr><TD><para>1125</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>El Asistente para la instalación del servicios de dominio de Active Directory (Dcpromo) no pudo establecer conexión con el siguiente controlador de dominio.</para></TD></tr><tr><TD><para>1645</para><para>este evento enumera el SPN de tres partes.</para></TD><TD><para>Replicación NTDS</para></TD><TD><para>Active Directory no realizó una llamada autenticado procedimiento remoto (RPC) a otro controlador de dominio porque el nombre de entidad de seguridad de servicio deseado (SPN) para el controlador de dominio de destino no está registrado en el controlador de dominio del centro de distribución de claves (KDC) que resuelve el SPN.</para></TD></tr><tr><TD><para>1655</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>Los servicios de dominio de Active Directory intentó comunicarse con el catálogo global siguiente y los intentos no tuvieron éxito.</para></TD></tr><tr><TD><para>2847</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>El Comprobador de coherencia encuentra una conexión de replicación de servicio de directorio local de solo lectura y ha intentado actualizar de forma remota en la siguiente instancia del servicio de directorio. No se pudo realizar la operación. Se volverá.</para></TD></tr><tr><TD><para>1925</para></TD><TD><para>NTDS KCC</para></TD><TD><para>Error al intentar establecer un vínculo de replicación de la partición de directorio grabable siguiente.</para></TD></tr><tr><TD><para>1926</para></TD><TD><para>NTDS KCC</para></TD><TD><para>Intentar establecer un vínculo de replicación en una partición de directorio de sólo lectura con los siguientes parámetros de error.</para></TD></tr><tr><TD><para>5781</para></TD><TD><para>NETLOGON</para></TD><TD><para> El servidor no puede registrar su nombre en DNS.</para></TD></tr></tbody></table></listItem><listItem><para>DCPROMO fails with an onscreen error</para><para>Dialog Title Text:</para><para>Active Directory Installation Failed</para><para>Dialog Message text:</para><para>The operation failed because: The Directory Service failed to create the server object for CN=NTDS Settings,CN=ServerBeingPromoted,CN=Servers,CN=Site,CN=Sites,CN=Configuration,DC=contoso,DC=com on server ReplicationSourceDC.contoso.com. </para><para>Please ensure the network credentials provided have sufficient access to add a replica. </para><para> "Logon Failure: The target account name is incorrect. "</para><para>In this case, Event ID 1645, 1168, and 1125 are logged on the server that is being promoted.</para></listItem><listItem><para>Map a drive using <embeddedLabel>net use</embeddedLabel>:</para><code>C:&gt;net use z: &lt;server_name&gt;c$
System error 1396 has occurred.
Logon Failure: The target account name is incorrect.</code><para>In this case, the server can also logging Event ID 333 in the system event log and use a high amount of virtual memory for an application such as SQL Server.</para></listItem><listItem><para>The DC time is incorrect.</para></listItem><listItem><para>The KDC will not start on an RODC after a restore of the krbtgt account for the RODC, which had been deleted. For example, after a restore, error 1396 appears. </para><para> Event ID 1645 is logged on the RODC. </para><para> Dcdiag also reports an error that it cannot update the RODC krbtgt account. </para></listItem>
</list>
    </content>
  </section>
  <section address="BKMK_Causes">
    <title>hace</title>
    <content>
      <para />
      <list class="ordered">
        <listItem>
          <para>el SPN no existe en el catálogo global buscado KDC en nombre del cliente intentar autenticarse con Kerberos.</para>
          <para>en el contexto de réplica de Active Directory, el cliente Kerberos, es el controlador de dominio, KDC realiza la búsqueda SPN de destino es probablemente el propio controlador de dominio de destino pero podría ser un controlador de dominio remoto.</para>
        </listItem>
        <listItem>
          <para>la cuenta de usuario o un servicio que debe contener el servicio de nombre de entidad de seguridad que se busca no existe en el catálogo global buscado KDC en nombre de destino intenta replicar el controlador de dominio.</para>
          <para>en el contexto de réplica de Active Directory, el origen de la cuenta de equipo de DC no existe en el catálogo global busca en el controlador de dominio en el nombre de la replicación entrante lleva a cabo de controlador de dominio de destino.</para>
        </listItem>
        <listItem>
          <para>el controlador de dominio de destino carece de un secreto LSA para el dominio de controladores de dominio de origen.</para>
        </listItem>
        <listItem>
          <para>el SPN que se buscan existe en una cuenta de equipo diferentes que el origen de controlador de dominio.</para>
        </listItem>
      </list>
    </content>
  </section>
  <section address="BKMK_Resolutions">
    <title>Resoluciones</title>
    <content>
      <list class="ordered">
        <listItem>
          <para>comprueba el registro de eventos de servicio de directorio en el controlador de dominio de destino para el evento de replicación de NTDS 1645 y ten en cuenta lo siguiente:</para>
          <para>el nombre del destino de DC</para>
          <para>el SPN que se buscan (E3514235-4B06-11D1-AB04-00C04FC2DCD2 /&lt;objeto guid para el objeto de origen de configuración de controladores de dominio NTDS&gt;/&lt;dominio de destino&gt;.&lt; tld&gt;@&lt;dominio de destino&gt;. &lt;tld&gt;</para>
          <para>el KDC que usa el controlador de dominio de destino</para>
        </listItem>
        <listItem>
          <para>desde la consola de KDC identificado en el paso 1, escribe: </para>
          <code>nltest /dsgetdc &lt;forest root DNS domain name &gt; /gc</code>
          <para>ejecutar la prueba de ubicador NLTEST inmediatamente después de un intento de replicación que se produce el error 1396 en el controlador de dominio de destino. </para>
          <para>Esto debe identificar ese GC que realiza búsquedas SPN contra KDC. </para>
          <para>El GC se busca el KDC también puede ser capturado en el evento de Microsoft-Windows-ActiveDirectory_DomainService 1655. </para>
        </listItem>
        <listItem>
          <para>Busca el SPN detectado en el paso 1 en el catálogo global detectado en el paso 2. </para>
          <code>C:&gt;repadmin /showattr Server_Name DC=corp,DC=contoso,dc=com &lt;GC used by KDC&gt; &lt;DN path of forest root domain&gt; /filter:"(serviceprincipalname=&lt;SPN cited in the NTDS Replication event 1645&gt;)" /gc /subtree /atts:cn,serviceprincipalname</code>
          <para>o</para>
          <code>C:&gt;dsquery * forestroot -scope subtree -filter "(serviceprincipalname=E3514235-4B06-11D1-AB04-00C04FC2DCD2/65cead9f-4949-46a3-a49a-f1fbfe13d2b3*)" -attr * -s Server_Name.europe.corp.contoso.com</code>
          <para>Compruebe que el objeto host para el SPN existe. </para>
          <para>Comprobar la ruta de acceso completa el objeto de host como si el objeto es CNF / conflicto alterados o resida en el contenedor perdidos. </para>
          <para>Comprueba que el origen de controladores de dominio de Active Directory replicación SPN se registra únicamente en el origen de la cuenta de equipo de controladores de dominio. </para>
          <para>Si falta el SPN de replicación, determinar si el origen de controlador de dominio registró su SPN a sí mismo, y si el SPN falta en el catálogo global utilizado por los KDC debido a la latencia de replicación simple o un error de replicación. </para>
        </listItem>
        <listItem>
          <para>Comprobar el estado del canal seguro y estado de confianza.</para>
        </listItem>
      </list>
    </content>
  </section>
  <relatedTopics>
    <externalLink>
      <linkText>operaciones de solución de problemas de Active Directory que con el error 1396: error al iniciar sesión: el nombre de cuenta de destino es incorrecto.</linkText>
      <linkUri>https://support.microsoft.com/kb/2183411/en-gb</linkUri>
    </externalLink>
  </relatedTopics>
</developerConceptualDocument>


