---
ms.assetid: 399a8bbe-3375-4bb0-b55b-5f46e7050028
title: 'Error de replicación 1396: error de inicio de sesión; el nombre de la cuenta de destino es incorrecto'
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 55e93eb24707fb1a583d060f44131ac794b00d22
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442554"
---
# <a name="replication-error-1396-logon-failure-the-target-account-name-is-incorrect"></a>Error de replicación 1396: error de inicio de sesión; el nombre de la cuenta de destino es incorrecto

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


<developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd"> <introduction>
    <para>En este artículo se describe los síntomas, causas y cómo resolver la replicación de Active Directory con error de Win32 1396: &quot;Error de inicio de sesión: El nombre de cuenta de destino es incorrecto.&quot; </para>
    <list class="bullet">
      <listItem>
        <para>
          <link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Symptoms">Síntomas</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Causes">Causas</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Resolutions">Soluciones</link>
        </para>
      </listItem>
    </list>
  </introduction>
  <section address="BKMK_Symptoms">
    <title>Síntomas</title>
    <content>
      <para />
      <list class="ordered">
<listItem><para>DCDIAG informes que ha fallado la prueba de replicaciones de Active Directory con error 1396: Error de inicio de sesión: El nombre de cuenta de destino es incorrecto.&quot;</para><code>Testing server: &lt;Site name&gt;&lt;DC Name&gt;
Starting test: Replications
[Replications Check,&lt;DC Name&gt;] A recent replication attempt failed:
From &lt;source DC&gt; to &lt;destination DC&gt;
Naming Context: CN=&lt;DN path of naming context&gt;
<codeFeaturedElement>The replication generated an error (1396):
Logon Failure: The target account name is incorrect.</codeFeaturedElement>
The failure occurred at &lt;date&gt; &lt;time&gt;.
The last success occurred at &lt;date&gt; &lt;time&gt;.
XX failures have occurred since the last success</code></listItem><listItem><para>REPADMIN. EXE informa de que el último intento de replicación no ha estado 1396.</para><para>REPADMIN comandos que aparecen normalmente con el estado 1396 incluyen, pero no se limitan a:</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><tbody><tr><TD><list class="bullet"><listItem><para>REPADMIN /ADD</para></listItem><listItem><para>REPADMIN/REPLSUM.</para></listItem><listItem><para>REPADMIN /REHOST</para></listItem><listItem><para>REPADMIN /SHOWVECTOR /LATENCY</para></listItem></list></TD><TD><list class="bullet"><listItem><para>REPADMIN /SHOWREPS</para></listItem><listItem><para>REPADMIN /SHOWREPL</para></listItem><listItem><para>REPADMIN /SYNCALL</para></listItem></list></TD></tr></tbody></table><para>Un ejemplo de salida &quot;REPADMIN /SHOWREPS&quot; que representan la replicación de entrada desde CONTOSO-DC2 a CONTOSO-DC1 genera un error con el &quot;error de inicio de sesión: El nombre de cuenta de destino es incorrecto. &quot; error se muestra a continuación:</para><code>Default-First-Site-NameCONTOSO-DC1
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
</code></listItem><listItem><para>El <ui>Replicar ahora</ui> comando en servicios y sitios de Active Directory devuelve &quot;error de inicio de sesión: El nombre de cuenta de destino es incorrecto.&quot;</para><para>Con el botón secundario en el objeto de conexión de un controlador de dominio de origen y elegir <ui>Replicar ahora</ui> se produce un error con &quot;error de inicio de sesión: El nombre de cuenta de destino es incorrecto.&quot; El que aparecen en pantalla se muestra el mensaje de error siguiente:</para><para>Texto de título del cuadro de diálogo:</para><para>Replicar ahora</para><para>Texto de mensaje del cuadro de diálogo: </para><para>Se produjo el error siguiente durante el intento de sincronizar el contexto de nomenclatura &lt;ruta de acceso DNS de partición&gt; del controlador de dominio &lt;DC de origen&gt; al controlador de dominio &lt;destino DC&gt;: Error de inicio de sesión: El nombre de cuenta de destino es incorrecto. Esta operación no continuará. </para></listItem><listItem><para>NTDS KCC NTDS generales o Microsoft-Windows-ActiveDirectory_DomainService eventos con el estado 1396 se registran en el registro de servicios de directorio en el Visor de eventos.</para><para>Eventos de Active Directory que se suelen citan el estado 1396 incluyen, pero no se limitan a:</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><thead><tr><TD><para>Id. de evento</para></TD><TD><para>Origen del evento</para></TD><TD><para>Cadena de eventos</para></TD></tr></thead><tbody><tr><TD><para>1125</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>El Asistente de instalación de servicios de dominio de Active Directory (Dcpromo) no pudo establecer conexión con el siguiente controlador de dominio.</para></TD></tr><tr><TD><para>1645</para><para>Este evento enumera lo SPN de tres partes.</para></TD><TD><para>Replicación de NTDS</para></TD><TD><para>Active Directory no realizó una llamada a procedimiento remoto (RPC) autenticada a otro controlador de dominio porque el nombre principal de servicio (SPN) deseado para el controlador de dominio de destino no está registrado en el controlador de dominio del Centro de distribución principal (KDC) que resuelve el SPN.</para></TD></tr><tr><TD><para>1655</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>Servicios de dominio de Active Directory intentó comunicarse con el siguiente catálogo global y los intentos no tuvieron éxito.</para></TD></tr><tr><TD><para>2847</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>El Comprobador de coherencia se encuentra una conexión de replicación para el servicio de directorio local de solo lectura y se intentó actualizarlo de forma remota en la siguiente instancia de servicio de directorio. Error en la operación. Se reintentará.</para></TD></tr><tr><TD><para>1925</para></TD><TD><para>NTDS KCC</para></TD><TD><para>Error al intentar establecer un vínculo de replicación para la siguiente partición de directorio de escritura.</para></TD></tr><tr><TD><para>1926</para></TD><TD><para>NTDS KCC</para></TD><TD><para>El intento para establecer un vínculo de replicación para una partición de directorio de sólo lectura con los siguientes parámetros de error.</para></TD></tr><tr><TD><para>5781</para></TD><TD><para>NETLOGON</para></TD><TD><para> El servidor no puede registrar su nombre en DNS.</para></TD></tr></tbody></table></listItem><listItem><para>DCPROMO se produce un error en la pantalla</para><para>Texto de título del cuadro de diálogo:</para><para>Error de instalación de Active Directory</para><para>Texto de mensaje del cuadro de diálogo:</para><para>Error en la operación porque: El servicio de directorio no se pudo crear el objeto de servidor para CN = Configuración NTDS, CN = ServerBeingPromoted, CN = Servers, CN = Site, CN = Sites, CN = Configuration, DC = contoso, DC = com en el servidor ReplicationSourceDC.contoso.com. </para><para>Asegúrese de que las credenciales de red proporcionadas tienen suficientes derechos de acceso para agregar una réplica. </para><para>
&quot;Error de inicio de sesión: El nombre de cuenta de destino es incorrecto. &quot;</para><para>En este caso, el Id. de evento 1125, 1645 y 1168 se registran en el servidor que se está promocionando.</para></listItem><listItem><para>Asignar una unidad mediante <embeddedLabel>net use</embeddedLabel>:</para><code>C:&gt;net use z: &lt;server_name&gt;c$
System error 1396 has occurred.
Logon Failure: The target account name is incorrect.</code><para>En este caso, el servidor puede también iniciar 333 del Id. de evento en el registro de eventos del sistema y use una gran cantidad de memoria virtual de una aplicación, como SQL Server.</para></listItem><listItem><para>La hora del controlador de dominio no es correcta.</para></listItem><listItem><para>El KDC no se iniciará en un RODC después de una restauración de la cuenta krbtgt para el RODC, que se había eliminado. Por ejemplo, después de una restauración, aparece el error 1396. </para><para>
Id. de evento 1645 se registra en el RODC. </para><para>
DCDiag también notifica un error que no se puede actualizar la cuenta krbtgt RODC. </para></listItem>
</list>
    </content>
  </section>
  <section address="BKMK_Causes">
    <title>Causas</title>
    <content>
      <para />
      <list class="ordered">
        <listItem>
          <para>El SPN no existe en el catálogo global que realiza la búsqueda del KDC en nombre del cliente al intentar autenticar con Kerberos.</para>
          <para>En el contexto de replicación de Active Directory, el cliente Kerberos es el DC de destino, el KDC de realizar la búsqueda de SPN es probablemente el propio controlador de dominio de destino, pero podría ser un controlador de dominio remoto.</para>
        </listItem>
        <listItem>
          <para>El usuario o cuenta de servicio que debe contener el nombre principal de servicio que se está buscando no existe en el catálogo global que realiza la búsqueda del KDC en nombre de destino intenta replicar el controlador de dominio.</para>
          <para>En el contexto de replicación de Active Directory, la cuenta de equipo del controlador de dominio de origen no existe en el catálogo global que se busca en el controlador de dominio en nombre de la replicación entrante de rendimiento de DC de destino.</para>
        </listItem>
        <listItem>
          <para>El controlador de dominio de destino no tiene un secreto LSA para el dominio de los controladores de dominio de origen.</para>
        </listItem>
        <listItem>
          <para>El SPN que se están buscando existe en una cuenta de equipo diferente que el controlador de dominio de origen.</para>
        </listItem>
      </list>
    </content>
  </section>
  <section address="BKMK_Resolutions">
    <title>Soluciones</title>
    <content>
      <list class="ordered">
        <listItem>
          <para>Compruebe el registro de eventos del servicio de directorio en el controlador de dominio de destino para eventos de replicación de NTDS 1645 y tenga en cuenta lo siguiente:</para>
          <para>El nombre del controlador de dominio de destino</para>
          <para>El SPN que se está buscando (E3514235-4B06-11D1-AB04-00C04FC2DCD2 /&lt;guid para el objeto de configuración NTDS de los controladores de dominio de origen del objeto&gt;/&lt;dominio de destino&amp;amp; gt;. &amp;amp; lt; tld&amp;amp; gt; @&lt;dominio de destino&gt;.&lt; tld&gt;</para>
          <para>El KDC que se usa el controlador de dominio de destino</para>
        </listItem>
        <listItem>
          <para>Desde la consola del KDC identificado en el paso 1, escriba: </para>
          <code>nltest /dsgetdc &lt;forest root DNS domain name &gt; /gc</code>
          <para>Ejecute la prueba de localizador NLTEST inmediatamente después de un intento de replicación que se produce el error 1396 en el controlador de dominio de destino. </para>
          <para>Esto debe identificar ese GC que realiza búsquedas de SPN en el KDC. </para>
          <para>El catálogo global que se está buscando el KDC también podría capturarse en eventos de Microsoft-Windows-ActiveDirectory_DomainService 1655.</para>
        </listItem>
        <listItem>
          <para>Buscar SPN detectado en el paso 1 en el catálogo global detectado en el paso 2.</para>
          <code>C:&gt;repadmin /showattr Server_Name DC=corp,DC=contoso,dc=com &lt;GC used by KDC&gt; &lt;DN path of forest root domain&gt; /filter:&quot;(serviceprincipalname=&lt;SPN cited in the NTDS Replication event 1645&gt;)&quot; /gc /subtree /atts:cn,serviceprincipalname</code>
          <para>O bien,</para>
          <code>C:&gt;dsquery * forestroot -scope subtree -filter &quot;(serviceprincipalname=E3514235-4B06-11D1-AB04-00C04FC2DCD2/65cead9f-4949-46a3-a49a-f1fbfe13d2b3*)&quot; -attr * -s Server_Name.europe.corp.contoso.com</code>
          <para>Compruebe que existe el objeto host para el SPN.</para>
          <para>Compruebe la ruta de acceso de DN para el objeto host incluido si el objeto es CNF / conflicto altera o se encuentra en el contenedor de perdidos y encontrados.</para>
          <para>Compruebe que el origen de controladores de dominio de Active Directory Replication SPN esté registrado solo en la cuenta de equipo de controladores de dominio de origen.</para>
          <para>Si falta el SPN de replicación, determine si el DC de origen ha registrado su SPN consigo misma, y si el SPN que falta en el GC utilizado por el KDC debido a la latencia de replicación simple o un error de replicación.</para>
        </listItem>
        <listItem>
          <para>Comprobar el estado de canal seguro y estado de confianza.</para>
        </listItem>
      </list>
    </content>
  </section>
  <relatedTopics>
    <externalLink>
      <linkText>Solución de problemas de operaciones de Active Directory que generan el error 1396: Error de inicio de sesión: El nombre de cuenta de destino es incorrecto.</linkText>
      <linkUri><a href="https://support.microsoft.com/kb/2183411/en-gb" data-raw-source="https://support.microsoft.com/kb/2183411/en-gb">https://support.microsoft.com/kb/2183411/en-gb</a></linkUri>
    </externalLink>
  </relatedTopics>
</developerConceptualDocument>


