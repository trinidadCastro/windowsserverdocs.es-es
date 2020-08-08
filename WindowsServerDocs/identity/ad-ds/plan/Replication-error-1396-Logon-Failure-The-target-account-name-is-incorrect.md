---
ms.assetid: 399a8bbe-3375-4bb0-b55b-5f46e7050028
title: 'Error de replicación 1396: error de inicio de sesión; el nombre de la cuenta de destino es incorrecto'
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: acd42fc1290639dddf9dbc283a873c8b7fca49c2
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87972282"
---
# <a name="replication-error-1396-logon-failure-the-target-account-name-is-incorrect"></a>Error de replicación 1396: error de inicio de sesión; el nombre de la cuenta de destino es incorrecto

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


<developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd"> <introduction>
    <para>En este artículo se describen los síntomas, la causa y el modo de resolver Active Directory errores de replicación con Win32 error 1396: &quot; error de inicio de sesión: el nombre de la cuenta de destino no es correcto.&quot; </para>
    <list class="bullet"> <listItem>
        <para>
          <link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Symptoms">Síntomas</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Causes">Hace que</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Resolutions">Resoluciones</link>
        </para>
      </listItem>
    </list>
  </introduction>
  <section address="BKMK_Symptoms">
    <title>Síntomas</title>
    <content>
      <para />
      <list class="ordered">
<listItem><para>DCDIAG informa de que se ha producido el error 1396 en la prueba de replicaciones de Active Directory: error de inicio de sesión: el nombre de la cuenta de destino no es correcto.&quot;</para><code>Testing server: &lt;Site name&gt;&lt;DC Name&gt;
Starting test: Replications
[Replications Check,&lt;DC Name&gt;] A recent replication attempt failed:
From &lt;source DC&gt; to &lt;destination DC&gt;
Naming Context: CN=&lt;DN path of naming context&gt;
<codeFeaturedElement>The replication generated an error (1396):
Logon Failure: The target account name is incorrect.</codeFeaturedElement>
The failure occurred at &lt;date&gt; &lt;time&gt;.
The last success occurred at &lt;date&gt; &lt;time&gt;.
XX failures have occurred since the last success</code></listItem><listItem><para>REPADMIN.EXE informa de que se ha producido un error en el último intento de replicación con el estado 1396.</para><para>Los comandos REPADMIN que suelen mencionar el estado 1396 incluyen, pero no se limitan a:</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><tbody><tr><TD><list class="bullet"><listItem><para>REPADMIN/ADD</para></listItem><listItem><para>REPADMIN/REPLSUM</para></listItem><listItem><para>REPADMIN/REHOST</para></listItem><listItem><para>REPADMIN/SHOWVECTOR/LATENCY</para></listItem></list></TD><TD><list class="bullet"><listItem><para>REPADMIN/SHOWREPS</para></listItem><listItem><para>REPADMIN/SHOWREPL</para></listItem><listItem><para>REPADMIN/SYNCALL</para></listItem></list></TD></tr></tbody></table><para>Salida de ejemplo de &quot; repadmin/showreps &quot; que describe la replicación de entrada de Contoso-DC2 a contoso-DC1 con error de &quot; Inicio de sesión: el nombre de la cuenta de destino es incorrecto. el &quot; error se muestra a continuación:</para><code>Default-First-Site-NameCONTOSO-DC1
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
</code></listItem><listItem><para>El comando <ui>Replicar ahora</ui> en Active Directory sitios y servicios devuelve &quot; un error de inicio de sesión: el nombre de la cuenta de destino no es correcto.&quot;</para><para>Al hacer clic con el botón derecho en el objeto de conexión desde un controlador de dominio de origen y elegir <ui>Replicar ahora</ui> se produce un &quot; error de inicio de sesión: el nombre de la cuenta de destino es incorrecto. &quot; A continuación se muestra el mensaje de error en pantalla:</para><para>Texto del título del cuadro de diálogo:</para><para>Replicar ahora</para><para>Texto del mensaje de diálogo: </para><para>Se produjo el siguiente error al intentar sincronizar la &lt; ruta de acceso DNS &gt; de la partición del contexto de nomenclatura del DC de origen del controlador de dominio &lt; &gt; al controlador de dominio &lt; destino &gt; : error de inicio de sesión: el nombre de la cuenta de destino no es correcto. Esta operación no continuará. </para></listItem><listItem><para>Los eventos NTDS KCC, NTDS general o Microsoft-Windows-ActiveDirectory_DomainService con el estado 1396 se registran en el registro de servicios de directorio en Visor de eventos.</para><para>Active Directory eventos que suelen citar el estado 1396 incluyen, pero no se limitan a:</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><thead><tr><TD><para>Id. de evento</para></TD><TD><para>Origen de eventos</para></TD><TD><para>Cadena de evento</para></TD></tr></thead><tbody><tr><TD><para>1125</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>El Asistente para instalación de Active Directory Domain Services (Dcpromo) no pudo establecer conexión con el siguiente controlador de dominio.</para></TD></tr><tr><TD><para>1645</para><para>Este evento muestra el SPN de tres partes.</para></TD><TD><para>Replicación de NTDS</para></TD><TD><para>Active Directory no realizó una llamada a procedimiento remoto (RPC) autenticada a otro controlador de dominio porque el nombre principal de servicio (SPN) deseado para el controlador de dominio de destino no está registrado en el controlador de dominio del Centro de distribución principal (KDC) que resuelve el SPN.</para></TD></tr><tr><TD><para>1655</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>Active Directory Domain Services intentó comunicarse con el catálogo global siguiente y los intentos fueron incorrectos.</para></TD></tr><tr><TD><para>2847</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>El comprobador de coherencia de la información encontró una conexión de replicación para el servicio de directorio local de solo lectura e intentó actualizarla de forma remota en la siguiente instancia del servicio de directorio. Error en la operación. Se volverá a intentar.</para></TD></tr><tr><TD><para>1925</para></TD><TD><para>KCC NTDS</para></TD><TD><para>Error al intentar establecer un vínculo de replicación para la siguiente partición de directorio grabable.</para></TD></tr><tr><TD><para>1926</para></TD><TD><para>KCC NTDS</para></TD><TD><para>Error al intentar establecer un vínculo de replicación en una partición de directorio de solo lectura con los siguientes parámetros.</para></TD></tr><tr><TD><para>5781</para></TD><TD><para>NETLOGON</para></TD><TD><para> El servidor no puede registrar su nombre en DNS.</para></TD></tr></tbody></table></listItem><listItem><para>DCPROMO produce un error en pantalla</para><para>Texto del título del cuadro de diálogo:</para><para>Error en la instalación de Active Directory</para><para>Texto del mensaje de diálogo:</para><para>No se pudo realizar la operación porque: el servicio de directorio no pudo crear el objeto de servidor para CN = NTDS Settings, CN = ServerBeingPromoted, CN = servers, CN = site, CN = Sites, CN = Configuration, DC = Contoso, DC = com en el servidor ReplicationSourceDC.contoso.com. </para><para>Asegúrese de que las credenciales de red proporcionadas tienen acceso suficiente para agregar una réplica. </para><para>
&quot;Error de inicio de sesión: el nombre de la cuenta de destino no es correcto. &quot;</para><para>En este caso, se registra el ID. de evento 1645, 1168 y 1125 en el servidor que se está promocionando.</para></listItem><listItem><para>Asignar una unidad mediante <embeddedLabel>net use</embeddedLabel>:</para><code>C:&gt;net use z: &lt;server_name&gt;c$
System error 1396 has occurred.
Logon Failure: The target account name is incorrect.</code><para>En este caso, el servidor también puede registrar el ID. de evento 333 en el registro de eventos del sistema y usar una gran cantidad de memoria virtual para una aplicación como SQL Server.</para></listItem><listItem><para>La hora del controlador de dominio es incorrecta.</para></listItem><listItem><para>El KDC no se iniciará en un RODC después de una restauración de la cuenta krbtgt para el RODC, que se eliminó. Por ejemplo, después de una restauración, aparece el error 1396. </para><para>
El ID. de evento 1645 está registrado en el RODC. </para><para>
Dcdiag también informa de un error que indica que no puede actualizar la cuenta krbtgt de RODC. </para></listItem>
</list>
    </content>
  </section>
  <section address="BKMK_Causes">
    <title>Causas</title>
    <content>
      <para />
      <list class="ordered">
        <listItem>
          <para>El SPN no existe en el catálogo global buscado por el KDC en nombre del cliente que intenta autenticarse mediante Kerberos.</para>
          <para>En el contexto de la replicación de Active Directory, el cliente Kerberos es el controlador de dominio de destino, el KDC que realiza la búsqueda de SPN es probablemente el propio controlador de dominio de destino, pero podría ser un controlador de dominio remoto.</para>
        </listItem>
        <listItem>
          <para>El usuario o la cuenta de servicio que debe contener el nombre de entidad de seguridad de servicio que se está buscando no existe en el catálogo global que busca el KDC en nombre del controlador de dominio de destino que intenta replicarse.</para>
          <para>En el contexto de la replicación de Active Directory, la cuenta de equipo del controlador de dominio de origen no existe en el catálogo global que busca el controlador de dominio en nombre del controlador de dominio de destino que realiza la replicación de entrada.</para>
        </listItem>
        <listItem>
          <para>El DC de destino carece de un secreto LSA para el dominio DC de origen.</para>
        </listItem>
        <listItem>
          <para>El SPN que se está buscando existe en una cuenta de equipo distinta de la del controlador de dominio de origen.</para>
        </listItem>
      </list>
    </content>
  </section>
  <section address="BKMK_Resolutions">
    <title>Soluciones</title>
    <content>
      <list class="ordered">
        <listItem>
          <para>Compruebe el registro de eventos del servicio de directorio en el controlador de dominio de destino para el evento de replicación NTDS 1645 y tenga en cuenta lo siguiente:</para>
          <para>Nombre del controlador de dominio de destino.</para>
          <para>El SPN que se está buscando (E3514235-4B06-11D1-AB04-00C04FC2DCD2/ &lt; GUID del objeto para el origen de los DC de origen configuración NTDS &gt; / &lt; dominio de destino &amp; amp; gt;. &amp; amp; lt; TLD &amp; amp; gt; @ &lt; dominio de destino &gt; . &lt; EU&gt;</para>
          <para>El KDC utilizado por el controlador de dominio de destino</para>
        </listItem>
        <listItem>
          <para>En la consola del KDC identificado en el paso 1, escriba: </para>
          <code>nltest /dsgetdc &lt;forest root DNS domain name &gt; /gc</code>
          <para>Ejecute la prueba de ubicación de NLTEST inmediatamente después de un intento de replicación que produce el error 1396 en el controlador de dominio de destino. </para>
          <para>Esto debe identificar el GC con el que el KDC realiza búsquedas de SPN. </para>
          <para>El GC que se está buscando en el KDC también puede capturarse en el evento 1655 de Microsoft-Windows-ActiveDirectory_DomainService.</para>
        </listItem>
        <listItem>
          <para>Busque el SPN detectado en el paso 1 en el catálogo global detectado en el paso 2.</para>
          <code>C:&gt;repadmin /showattr Server_Name DC=corp,DC=contoso,dc=com &lt;GC used by KDC&gt; &lt;DN path of forest root domain&gt; /filter:&quot;(serviceprincipalname=&lt;SPN cited in the NTDS Replication event 1645&gt;)&quot; /gc /subtree /atts:cn,serviceprincipalname</code>
          <para>O</para>
          <code>C:&gt;dsquery * forestroot -scope subtree -filter &quot;(serviceprincipalname=E3514235-4B06-11D1-AB04-00C04FC2DCD2/65cead9f-4949-46a3-a49a-f1fbfe13d2b3*)&quot; -attr * -s Server_Name.europe.corp.contoso.com</code>
          <para>Compruebe que el objeto host del SPN existe.</para>
          <para>Compruebe la ruta de acceso de DN para el objeto host, incluido si el objeto es CNF/conflicto alterado o reside en el contenedor perdido y encontrado.</para>
          <para>Compruebe que los DC de origen Active Directory SPN de replicación están registrados solo en la cuenta de equipo de DC de origen.</para>
          <para>Si falta el SPN de replicación, determine si el controlador de dominio de origen ha registrado su SPN en sí mismo y si el SPN falta en el GC que usa el KDC debido a una latencia de replicación simple o un error de replicación.</para>
        </listItem>
        <listItem>
          <para>Compruebe el estado del canal seguro y el mantenimiento de la confianza.</para>
        </listItem>
      </list>
    </content>
  </section>
  <relatedTopics>
    <externalLink>
      <linkText>Solución de problemas de operaciones de Active Directory que producen el error 1396: error de inicio de sesión: el nombre de la cuenta de destino no es correcto.</linkText>
      <linkUri><a href="https://support.microsoft.com/kb/2183411/en-gb" data-raw-source="https://support.microsoft.com/kb/2183411/en-gb">https://support.microsoft.com/kb/2183411/en-gb</a></linkUri>
    </externalLink>
  </relatedTopics>
</developerConceptualDocument>


