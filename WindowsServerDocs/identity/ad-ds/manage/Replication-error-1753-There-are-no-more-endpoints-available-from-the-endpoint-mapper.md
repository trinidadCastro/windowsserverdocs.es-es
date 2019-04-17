---
ms.assetid: 0f21951c-b1bf-43bb-a329-bbb40c58c876
title: "Error de replicación 1753 allí son no hay más extremos disponibles desde el asignador de extremo"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0e7412f5edc6c206888551fdc250883b5c0ced3e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="replication-error-1753-there-are-no-more-endpoints-available-from-the-endpoint-mapper"></a>Error de replicación 1753 allí son no hay más extremos disponibles desde el asignador de extremo

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


<developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd">
  <introduction>
    <para>Este tema explica los síntomas, las causas y cómo resolver Active Directory replicación error 8524 DSA la operación es no se puede continuar debido a un error de búsqueda DNS.</para>
    <list class="bullet">
      <listItem><para><link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_Symptoms">Síntomas</link></para></listItem><listItem><para><link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_Cause">causa</link></para></listItem><listItem><para><link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_Resolutions">resoluciones</link></para></listItem><listItem><para><link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_MoreInfo">obtener más información</link></para></listItem>
    </list>
  </introduction>
  <section address="BKMK_Symptoms">
              
    <title>Symptoms</title>
    <content>
      <para>This article describes symptoms, cause and resolution steps for Active Directory operations that fail with Win32 error 1753: "There are no more endpoints available from the endpoint mapper."</para>
      <list class="ordered">
        <listItem>
          <para>DCDIAG reports that the Connectivity test, Active Directory Replications test or KnowsOfRoleHolders test has failed with error 1753: "There are no more endpoints available from the endpoint mapper."</para>
          <code>Testing server: &lt;site&gt;&lt;DC Name&gt;
Starting test: Connectivity
* Active Directory LDAP Services Check
* Active Directory RPC Services Check
[&lt;DC Name&gt;] <codeFeaturedElement>DsBindWithSpnEx() failed with error 1753,
There are no more endpoints available from the endpoint mapper..</codeFeaturedElement>
Printing RPC Extended Error Info:
Error Record 1, ProcessID is &lt;process ID&gt; (DcDiag) 
System Time is: &lt;date&gt; &lt;time&gt;
Generating component is 2 (RPC runtime)
Status is 1753: There are no more endpoints available from the endpoint mapper. Detection location is 500
NumberOfParameters is 4
Unicode string: ncacn_ip_tcp
Unicode string: &lt;source DC object GUID&gt;._msdcs.contoso.com
Long val: -481213899
Long val: 65537
Error Record 2, ProcessID is 700 (DcDiag) 
System Time is: &lt;date&gt; &lt;time&gt;
Generating component is 2 (RPC runtime)
<codeFeaturedElement>Status is 1753: There are no more endpoints available from the endpoint mapper.</codeFeaturedElement>
NumberOfParameters is 1
Unicode string: 1025
[Replications Check,&lt;DC Name&gt;] A recent replication attempt failed:
From &lt;source DC&gt; to &lt;destination DC&gt;
Naming Context: &lt;DN path of directory partition&gt;
The replication generated an error <codeFeaturedElement>(1753):
There are no more endpoints available from the endpoint mapper.</codeFeaturedElement> 
The failure occurred at &lt;date&gt; &lt;time&gt;.
The last success occurred at &lt;date&gt; &lt;time&gt;.
3 failures have occurred since the last success.
The directory on &lt;DC name&gt; is in the process.
of starting up or shutting down, and is not available.
Verify machine is not hung during boot.
</code>
        </listItem>
<listItem><para>REPADMIN.EXE reports that replication attempt has failed with status 1753.</para><para>REPADMIN commands that commonly cite the 1753 status include but are not limited to:</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><tbody><tr><TD><list class="bullet"><listItem><para>/ Replsum REPADMIN</para></listItem><listItem><para>REPADMIN /SHOWREPL</para></listItem></list></TD><TD><list class="bullet"><listItem><para>REPADMIN /SHOWREPS</para></listItem><listItem><para>REPADMIN/SyncAll</para></listItem></list></TD></tr></tbody></table><para>Sample output from "REPADMIN /SHOWREPS" depicting inbound replication from CONTOSO-DC2 to CONTOSO-DC1 failing with the "replication access was denied" error is shown below:</para><code>Default-First-Site-NameCONTOSO-DC1
DSA Options: IS_GC 
Site Options: (none)
DSA object GUID: b6dc8589-7e00-4a5d-b688-045aef63ec01
DSA invocationID: b6dc8589-7e00-4a5d-b688-045aef63ec01
==== INBOUND NEIGHBORS ======================================
DC=contoso,DC=com
Default-First-Site-NameCONTOSO-DC2 via RPC
DSA object GUID: 74fbe06c-932c-46b5-831b-af9e31f496b2
Last attempt @ &lt;date&gt; &lt;time&gt; failed, <codeFeaturedElement>result 1753 (0x6d9):
There are no more endpoints available from the endpoint mapper.</codeFeaturedElement>
&lt;#&gt; consecutive failure(s).
Last success @ &lt;date&gt; &lt;time&gt;.

</code></listItem><listItem><para>The <ui>Check Replication Topology</ui> command in Active Directory Sites and Services returns "There are no more endpoints available from the endpoint mapper."</para><para>Right-clicking on the connection object from a source DC and choosing <ui>Check Replication Topology</ui> fails with "There are no more endpoints available from the endpoint mapper." The on-screen error message is shown below:</para><para>Dialog title text: Check Replication Topology</para><para>Dialog message text: </para><para>The following error occurred during the attempt to contact the domain controller: There are no more endpoints available from the endpoint mapper.</para></listItem><listItem><para>The <ui>Replicate now</ui> command in Active Directory Sites and Services returns "there are no more endpoints available from the endpoint mapper."</para><para>Right-clicking on the connection object from a source DC and choosing <ui>Replicate now</ui> fails with "There are no more endpoints available from the endpoint mapper." The on-screen error message is shown below:</para><para>Dialog title text: Replicate Now</para><para>Dialog message text: The following error occurred during the attempt to synchronize naming context &lt;%directory partition name%&gt; from Domain Controller &lt;Source DC&gt; to Domain Controller &lt;Destination DC&gt;:</para><para>

No hay ninguna más extremos disponibles desde el asignador. </para><para>La operación no continuará</para></listItem><listItem><para>NTDS KCC generales de NTDS o Microsoft-Windows-ActiveDirectory_DomainService eventos con el estado de-2146893022 se registran en el registro de servicios de directorio en el Visor de eventos. </para><para>Eventos de active Directory que citan normalmente el estado de-2146893022 incluyen pero no se limitan a:</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><thead><tr><TD><para>Identificador de evento</para></TD><TD><para>Origen del evento</para></TD><TD><para>Cadena de evento</para></TD></tr></thead><tbody><tr><TD><para>1655</para></TD><TD><para>NTDS General</para></TD><TD><para>Active Directory intentó comunicarse con el catálogo global siguiente y los intentos no tuvieron éxito.</para></TD></tr><tr><TD><para>1925</para></TD><TD><para>NTDS KCC</para></TD><TD><para>Error al intentar establecer un vínculo de replicación de la partición de directorio grabable siguiente.</para></TD></tr><tr><TD><para>1265</para></TD><TD><para>NTDS KCC</para></TD><TD><para>El Comprobador de coherencia de conocimientos (KCC) para agregar un acuerdo de replicación siguiente directorio partición y el origen de controlador de dominio de error.</para></TD></tr></tbody></table></listItem>
</list>
    </content>
  </section>
  <section address="BKMK_Cause">
    <title>Causa</title>
    <content>
      <para>el siguiente diagrama muestra el flujo de trabajo RPC inicio con el registro de la aplicación del servidor con asignador de extremo de RPC (EPM) en el paso 1 para la transferencia de datos desde el cliente RPC a la aplicación cliente en el paso 7. </para>
      <para>&lt;ADDS_RPCWorkflow&gt;</para>
      <para>pasos 1a 7 se asignan a las siguientes operaciones:</para>
      <list class="ordered">
        <listItem>
          <para>servidor aplicación registra sus extremos con asignador de extremo de RPC (EPM) </para>
        </listItem>
        <listItem>
          <para>cliente realiza una llamada RPC (en el nombre de un usuario, sistema operativo o la aplicación inicia operación) </para>
        </listItem>
        <listItem>
          <para>cliente lateral RPC contactos los equipos de destino EPM y solicitar el extremo completar la llamada del cliente </para>
        </listItem>
        <listItem>
          <para>EPM del equipo servidor responde con un extremo de </para>
        </listItem>
        <listItem>
          <para>RPC del cliente que pone en contacto con la aplicación del servidor </para>
        </listItem>
        <listItem>
          <para>aplicación del servidor ejecuta la llamada, devuelve el resultado al cliente RPC </para>
        </listItem>
        <listItem>
          <para>RPC del cliente, pasa el resultado a la aplicación cliente</para>
        </listItem>
      </list>
      <para>Error 1753 genera un error entre los pasos #3 y 4 #. Específicamente, error 1753 significa que el cliente RPC (controlador de dominio de destino) se puede establecer contacto con el servidor RPC (origen DC) a través del puerto 135 la EPM en el servidor RPC (origen DC) no pudo encontrar la aplicación de RPC de interés y devuelve el error en el servidor 1753. La presencia del error 1753 indica que el cliente RPC (controlador de dominio de destino) recibe la respuesta de error del lado servidor desde el servidor RPC (origen de replicación de AD DC) a través de la red. </para>
      <para>Causas específicos para el error 1753 incluyen: </para>
      <list class="ordered">
        <listItem>
          <para>nunca se inicia la aplicación del servidor (es decir, nunca se intentó el paso 1 en el diagrama de "más información" que se encuentre encima). </para>
        </listItem>
        <listItem>
          <para>Inicia la aplicación de servidor, pero se produjo algún error durante la inicialización que impidió que realizara registrándose asignador de extremos RPC (es decir, paso 1 en el diagrama de "más información" anterior se intentó pero no se pudo). </para>
        </listItem>
        <listItem>
          <para>Comenzado pero muerto posteriormente la aplicación del servidor. (es decir, paso 1 en el diagrama de "más información" anterior se realizó correctamente, pero se deshacer más adelante, porque el servidor muerto). </para>
        </listItem>
        <listItem>
          <para>La aplicación del servidor no manualmente registrado sus extremos (similares a 3, pero de forma intencionada. Pero no es probable incluidas para ser más exactos). </para>
        </listItem>
        <listItem>
          <para>Un servidor RPC diferente de lo previsto debido a un nombre al error de asignación de IP en el archivo DNS, WINS o host o Lmhosts contactar con el cliente de la RPC (controlador de dominio de destino). </para>
        </listItem>
      </list>
      <para>Error 1753 no está causado por: </para>
      <list class="bullet">
        <listItem>
          <para>una falta de conectividad de red entre el cliente RPC (controlador de dominio de destino) y el servidor RPC (origen DC) a través del puerto 135</para>
        </listItem>
        <listItem>
          <para>una falta de conectividad de red entre el servidor RPC (origen DC) con el puerto 135 y el cliente RPC (controlador de dominio de destino) en el puerto efímero. </para>
        </listItem>
        <listItem>
          <para>Una falta de coincidencia de contraseña o la incapacidad por el origen de controlador de dominio para descifrar un paquete cifrado Kerberos</para>
        </listItem>
      </list>
      <para></para>
    </content>
  </section>
  <section address="BKMK_Resolutions">
    <title>Resolutions</title>
    <content>
      <list class="ordered">
        <listItem>
          <para>
            <embeddedLabel>Verify that the service registering its service with the endpoint mapper has started</embeddedLabel>
          </para>
          <para>For Windows 2000 and Windows Server 2003 DCs: ensure that the source DC is booted into normal mode. </para>
          <para> For Windows Server 2008 or Windows Server 2008 R2: from the console of the source DC, start Services Manager (services.msc) and verify that the <embeddedLabel>Active Directory Domain Services</embeddedLabel> service is running. </para>
        </listItem>
        <listItem>
          <para>
            <embeddedLabel>Verify that RPC client (destination DC) connected to the intended RPC Server (source DC)</embeddedLabel>
          </para>
          <para>All DCs in a common Active Directory forest register a domain controller CNAME record in the _msdcs.&lt;forest root domain&gt; DNS zone regardless of what domain they reside in within the forest. The DC CNAME record is derived from the <embeddedLabel>objectGUID</embeddedLabel> attribute of the NTDS Settings object for each domain controller. </para>
          <para>When performing replication-based operations, a destination DC queries DNS for the source DCs CNAME record. The CNAME record contains the source DC fully qualified computer name which is used to derive the source DCs IP address via DNS client cache lookup, Host / LMHost file lookup, host A / AAAA record in DNS, or WINS. </para>
          <para>Stale NTDS Settings objects and bad name-to-IP mappings in DNS, WINS, Host and LMHOST files may cause the RPC client (destination DC) to connect to the wrong RPC Server (Source DC). Furthermore, the bad name-to-IP mapping may cause the RPC client (destination DC) to connect to a computer that does not even have the RPC Server Application of interest (the Active Directory role in this case) installed. (Example: a stale host record for DC2 contains the IP address of DC3 or a member computer). </para>
          <para>Verify that the objectGUID for the source DC that exists in the destination DCs copy of Active Directory matches the source DC objectGUID stored in the source DCs copy of Active Directory. If there is a discrepancy, use repadmin /showobjmeta on the ntds settings object to see which one corresponds to last promotion of the source DC (hint: compare date stamps for the NTDS Settings object create date from /showobjmeta against the last promotion date in the source DCs dcpromo.log file. You may have to use the last modify / create date of the DCPROMO.LOG file itself). If the object GUIDs are not identical, the destination DC likely has a stale NTDS Settings object for the source DC whose CNAME record refers to a host record with a bad name to IP mapping. </para>
          <para>On the destination DC, run IPCONFIG /ALL to determine which DNS Servers the destination DC is using for name resolution:</para>
          <code>c:&gt;ipconfig /all</code>
          <para>On the destination DC, run NSLOOKUP against the source DCs fully qualified DC CNAME record:</para>
          <code>c:&gt;nslookup -type=cname &lt;fully qualified cname of source DC&gt; &lt;destination DCs primary DNS Server IP &gt;
c:&gt;nslookup -type=cname &lt;fully qualified cname of source DC&gt; &lt;destination DCs secondary DNS Server IP&gt;</code>
          <para>Verify that the IP address returned by NSLOOKUP "owns" the host name / security identity of the source DC:</para>

          <code>C:&gt;NBTSTAT -A &lt;IP address returned by NSLOOKUP in the step above&gt;</code>
          <para>or</para>
          <para>Log onto the console of the source DC, run "IPCONFIG" from the CMD prompt and verify that the source DC owns the IP address returned by the NSLOOKUP command above</para>
          <para>Check for stale / duplicate host to IP mappings in DNS</para>
          <code>NSLOOKUP -type=hostname &lt;single label hostname of source DC&gt; &lt;primary DNS Server IP on destination DC&gt;
NSLOOKUP -type=hostname &lt;single label hostname of source DC&gt; &lt;secondary DNS Server IP on destination DC&gt;

NSLOOKUP -type=hostname &lt;fully qualified computer name of source DC&gt; &lt;primary DNS Server IP on destination DC&gt;
NSLOOKUP -type=hostname &lt;fully qualified computer name of source DC&gt; &lt;secondary DNS Server IP on dest. DC&gt;</code>
<para>If invalid IP addresses exist in host records, investigate whether DNS scavenging is enabled and properly configured. </para><para>If the tests above or a network trace doesn't show a name query returning an invalid IP address, consider stale entries in HOST files, LMHOSTS files and WINS Servers. Note that DNS Servers can also be configured to perform WINS fallback name resolution.</para>
</listItem>
        <listItem>
          <para>
            <embeddedLabel>Verify that the server application (Active Directory et al) has registered with the endpoint mapper on the RPC server (source DC)</embeddedLabel>
          </para>
          <para>Active Directory uses a mix of well-known and dynamically registered ports. This table lists well known ports and protocols used by Active Directory domain controllers.</para>
          <table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11">
            <thead>
              <tr>
                <TD>
                  <para>Aplicación de servidor RPC</para>
                </TD>
                <TD>
                  <para>Puerto</para>
                </TD>
                <TD>
                  <para>TCP</para>
                </TD>
                <TD>
                  <para>UDP</para>
                </TD>
              </tr>
            </thead>
            <tbody>
              <tr>
                <TD>
                  <para>Servidor DNS</para>
                </TD>
                <TD>
                  <para>53</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>Kerberos</para>
                </TD>
                <TD>
                  <para>88</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>Servidor LDAP</para>
                </TD>
                <TD>
                  <para>389</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>Microsoft DS</para>
                </TD>
                <TD>
                  <para>445</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>SSL LDAP</para>
                </TD>
                <TD>
                  <para>636</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>Servidor de catálogo global</para>
                </TD>
                <TD>
                  <para>3268</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para />
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>Servidor de catálogo global</para>
                </TD>
                <TD>
                  <para>3269</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para />
                </TD>
              </tr>
            </tbody>
          </table>
          <para>Los puertos conocidos no están registrados con el asignador de extremos. </para>
          <para>Active Directory y otras aplicaciones también registran servicios que reciben de puertos asignados dinámicamente en el intervalo de puerto efímero RPC. Estas aplicaciones de servidor RPC se asignan dinámicamente los puertos TCP entre 1024 y 5000 en equipos con Windows 2000 y Windows Server 2003 y entre 49152 y 65535 intervalo en equipos con Windows Server 2008 y Windows Server 2008 R2. El puerto RPC usado la replicación puede ser codificado de forma rígida en el registro con los pasos documentados en <externalLink><linkText>artículo de Knowledge Base 224196</linkText><linkUri>https://support.microsoft.com/kb/224196</linkUri></externalLink>. Active Directory continúa registrarse con el EPM cuando se configura para utilizar un puerto codificado de forma rígida. </para>
          <para>Comprueba que la aplicación de servidor RPC de interés se ha registrado con el asignador RPC en el servidor RPC (el origen de controlador de dominio en el caso de replicación de AD). </para>
          <para>Hay varias formas de realizar esta tarea pero uno es instalar y ejecutar PORTQRY desde un símbolo del sistema con privilegios de administrador en la consola del origen de controlador de dominio mediante la sintaxis: </para>
          <code>c:\&gt;portquery -n &lt;source DC&gt; -e 135 &gt;file.txt</code>
          <para>en el resultado portqry, ten en cuenta los números de puerto dinámicamente registrados por "MS NT directorio DRS interfaz" (UUID = 351...) para el <embeddedLabel>protocolo ncacn_ip_tcp</embeddedLabel>. El siguiente fragmento muestra la salida portquery desde un controlador de dominio de Windows Server 2008 R2 y el UUID / par de protocolo que se usan específicamente Active Directory resaltados en <embeddedLabel>negrita</embeddedLabel>: </para>
          <code>UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_np:CONTOSO-DC01[\pipe\lsass] 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_np:CONTOSO-DC01[\PIPE\protected_storage] 
<codeFeaturedElement>UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_ip_tcp:CONTOSO-DC01[49156]</codeFeaturedElement> 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_http:CONTOSO-DC01[49157] 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_http:CONTOSO-DC01[6004]</code>
          <para />
        </listItem>
        <listItem>
          <para>otras maneras posibles para resolver este error:</para>
          <list class="ordered">
            <listItem>
              <para>comprueba que el origen de controlador de dominio se arranca en modo normal y que la función de controlador de dominio y sistema operativo en el origen de controlador de dominio ha iniciado completamente. </para>
            </listItem>
            <listItem>
              <para>Comprobar que se está ejecutando el servicio de dominio de Active Directory. Si el servicio actualmente se detiene o no se ha configurado con los valores de inicio predeterminados, restablecer los valores de inicio predeterminados, reinicie el controlador de dominio modificada, a continuación, vuelva a intentar la operación. </para>
            </listItem>
            <listItem>
              <para>Comprueba que el estado de servicio y el valor de inicio de servicio RPC y ubicador de RPC es correcto para la versión del sistema operativo del cliente de RPC (controlador de dominio de destino) y el servidor RPC (origen de controlador de dominio). Si el servicio actualmente se detiene o no se ha configurado con los valores de inicio predeterminados, restablecer los valores de inicio predeterminados, reinicie el controlador de dominio modificada, a continuación, vuelva a intentar la operación. </para>
              <para>Además, asegúrate de que el contexto del servicio coincida con valores de configuración de forma predeterminada en la siguiente tabla.</para>
              <table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11">
                <thead>
                  <tr>
                    <TD>
                      <para>Servicio</para>
                    </TD>
                    <TD>
                      <para>Estado predeterminado (tipo de inicio) en Windows Server 2003 y posteriores </para>
                    </TD>
                    <TD>
                      <para>Estado predeterminado (tipo de inicio) en Windows Server 2000</para>
                    </TD>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <TD>
                      <para>Llamada a procedimiento remoto</para>
                    </TD>
                    <TD>
                      <para>Inicia (automática)</para>
                    </TD>
                    <TD>
                      <para>Inicia (automática)</para>
                    </TD>
                  </tr>
                  <tr>
                    <TD>
                      <para>Localizador de llamada a procedimiento remoto</para>
                    </TD>
                    <TD>
                      <para>Null o se detuvo (Manual)</para>
                    </TD>
                    <TD>
                      <para>Inicia (automática)</para>
                    </TD>
                  </tr>
                </tbody>
              </table>
            </listItem>
            <listItem>
              <para>Comprueba que el tamaño del intervalo de puertos dinámicos no se ha restringido. A continuación se muestra la sintaxis de Windows Server 2008 y Windows Server 2008 R2 NETSH para enumerar el intervalo de puertos RPC:</para>
              <code>&gt;netsh int ipv4 show dynamicport tcp
&gt;netsh int ipv4 show dynamicport udp
&gt;netsh int ipv6 show dynamicport tcp
&gt;netsh int ipv6 show dynamicport udp</code>
            </listItem>
            <listItem>
              <para>comprueba que las definiciones de puerto codificado de forma rígida definidas en KB 224196 están dentro del intervalo de dinámica de puertos para la versión de SO de controladores de dominio de origen. </para>
              <para>Revisión <externalLink><linkText>artículo de Knowledge Base 224196</linkText><linkUri>https://support.microsoft.com/kb/224196</linkUri></externalLink> y asegurarse de que el puerto codificado de forma rígida esté dentro del intervalo de puerto efímero para la versión del sistema operativo el origen del controlador de dominio. </para>
            </listItem>
            <listItem>
              <para>Comprueba que la clave ClientProtocols existe en HKLM\Software\Microsoft\Rpc y contiene los siguientes valores predeterminados de 5:</para>
              <code>ncacn_http REG_SZ rpcrt4.dll
ncacn_ip_tcp REG_SZ rpcrt4.dll
<codeFeaturedElement>ncacn_nb_tcp REG_SZ rpcrt4.dll</codeFeaturedElement>
ncacn_np REG_SZ rpcrt4.dll
ncacn_ip_udp REG_SZ rpcrt4.dll</code>
            </listItem>
          </list>
        </listItem>
      </list>
    </content>
  </section>
  <section address="BKMK_MoreInfo">
    <title>More information</title>
    <content>
      <para>
        <embeddedLabel>Example of a bad name to IP mapping causing RPC error 1753 vs. -2146893022: the target principal name is incorrect</embeddedLabel>
      </para>
      <para>The contoso.com domain consists of DC1 and DC2 with IP addresses x.x.1.1 and x.x.1.2. The host "A" / "AAAA" records for DC2 are correctly registered on all of the DNS Servers configured for DC1. In addition, the HOSTS file on DC1 contains an entry mapping DC2s fully qualified hostname to IP address x.x.1.2. Later, DC2's IP address changes from X.X.1.2 to X.X.1.3 and a new member computer is joined to the domain with IP address x.x.1.2. AD Replication attempts triggered by the <ui>Replicate now</ui> command in Active Directory Sites and Services snap-in fails with error 1753 as shown in the trace below:</para>
      <code>F# SRC    DEST    Operation 
1 x.x.1.1 x.x.1.2 ARP:Request, x.x.1.1 asks for x.x.1.2
2 x.x.1.2 x.x.1.1 ARP:Response, x.x.1.2 at 00-13-72-28-C8-5E
3 x.x.1.1 x.x.1.2 TCP:Flags=......S., SrcPort=50206, DstPort=DCE endpoint resolution(135)
4 x.x.1.2 x.x.1.1 ARP:Request, x.x.1.2 asks for x.x.1.1
5 x.x.1.1 x.x.1.2 ARP:Response, x.x.1.1 at 00-15-5D-42-2E-00
6 x.x.1.2 x.x.1.1 TCP:Flags=...A..S., SrcPort=DCE endpoint resolution(135)
7 x.x.1.1 x.x.1.2 TCP:Flags=...A...., SrcPort=50206, DstPort=DCE endpoint resolution(135)
8 x.x.1.1 x.x.1.2 MSRPC:c/o Bind: UUID{E1AF8308-5D1F-11C9-91A4-08002B14A0FA} EPT(EPMP) 
9 x.x.1.2 x.x.1.1 MSRPC:c/o Bind Ack: Call=0x2 Assoc Grp=0x5E68 Xmit=0x16D0 Recv=0x16D0 
<codeFeaturedElement>10</codeFeaturedElement> x.x.1.1 x.x.1.2 EPM:Request: ept_map: NDR, DRSR(DRSR) {E3514235-4B06-11D1-AB04-00C04FC2DCD2} [DCE endpoint resolution(135)]
<codeFeaturedElement>11</codeFeaturedElement> x.x.1.2 x.x.1.1 EPM:Response: ept_map: 0x16C9A0D6 - EP_S_NOT_REGISTERED
</code>
      <para>At frame <embeddedLabel>10</embeddedLabel>, the destination DC queries the source DCs end point mapper over port 135 for the Active Directory replication service class UUID E351... </para>
      <para>In frame <embeddedLabel>11</embeddedLabel>, the source DC, in this case a member computer that does not yet host the DC role and therefore has not registered the E351... UUID for the Replication service with its local EPM responds with symbolic error EP_S_NOT_REGISTERED which maps to decimal error 1753, hex error 0x6d9 and friendly error "there are no more endpoints available from the endpoint mapper".</para>
      <para>Later, the member computer with IP address x.x.1.2 gets promoted as a replica "MayberryDC" in the contoso.com domain. Again, the <ui>Replicate now</ui> command is used to trigger replication but this time fails with the on-screen error "The target principal name is incorrect." The computer whose network adapter is assigned the IP address x.x.1.2 <placeholder>is</placeholder> a domain controller, is currently booted into normal mode and has registered the E351... replication service UUID with its local EPM but it does not own the name or security identity of DC2 and cannot decrypt the Kerberos request from DC1 so the request now fails with error "The target principal name is incorrect." The error maps to decimal error -2146893022 / hex error 0x80090322. </para>
      <para>Such invalid host-to-IP mappings could be caused by stale entries in host / lmhost files, host A / AAAA registrations in DNS, or WINS. </para>
      <para>Summary: This example failed because an invalid host-to-IP mapping (in the HOST file in this case) caused the destination DC to resolve to a "source" DC that did not have the Active Directory Domain Services service running (or even installed for that matter) so the replication SPN was not yet registered and the source DC returned error 1753. In the second case, an invalid host-to-IP mapping (again in the HOST file) caused the destination DC to connect to a DC that had registered the E351... replication SPN but that source had a different hostname and security identity than the intended source DC so the attempts failed with error -2146893022: The target principal name is incorrect.</para>
    </content>
  </section>
  <relatedTopics>
    <externalLink>
      <linkText>Solución de problemas de operaciones de Active Directory que con el error 1753: no hay ningún extremo más disponible desde el asignador de extremos. </linkText> 
      <linkUri>https://support.microsoft.com/kb/2089874</linkUri>
    </externalLink>
<externalLink><linkText>839880 errores de solución de problemas de asignador de RPC con las herramientas de soporte técnico de Windows Server 2003 desde el CD del producto del artículo KB</linkText><linkUri>https://support.microsoft.com/kb/839880</linkUri></externalLink>
<externalLink><linkText>KB artículo 832017 servicio información general y red puerto requisitos para el sistema de Windows Server</linkText><linkUri>https://support.microsoft.com/kb/832017/</linkUri></externalLink>
<externalLink><linkText>224196 tráfico de replicación de restricción de Active Directory y el tráfico RPC de cliente a un puerto específico del artículo KB</linkText><linkUri>https://support.microsoft.com/kb/224196/</linkUri></externalLink>
<externalLink><linkText>KB artículo 154596 cómo configurar la asignación dinámica de puertos RPC para trabajar con firewalls</linkText><linkUri>https://support.microsoft.com/kb/154596</linkUri></externalLink><externalLink><linkText>funcionamiento de RPC</linkText><linkUri>https://msdn.microsoft.com/library/aa373935(VS.85).aspx</linkUri> </externalLink> <externalLink> <linkText>Cómo el servidor prepara para una conexión</linkText><linkUri>https://msdn.microsoft.com/library/aa373938(VS.85).aspx</linkUri></externalLink>
<externalLink><linkText>cómo el cliente establece una conexión</linkText><linkUri>https://msdn.microsoft.com/library/aa373937(VS.85).aspx</linkUri></externalLink><externalLink><linkText>registrar la interfaz</linkText><linkUri>https://msdn.microsoft.com/library/aa375357(VS.85).aspx</linkUri></externalLink><externalLink><linkText>distribuyendo el servidor en la red</linkText><linkUri>https://msdn.microsoft.com/library/aa373974(VS.85).aspx</linkUri></externalLink><externalLink><linkText>registrar extremos</linkText><linkUri>https://msdn.microsoft.com/library/aa375255(VS.85).aspx</linkUri></externalLink><externalLink><linkText>escuchar llamadas</linkText><linkUri>https://msdn.microsoft.com/library/aa373966(VS.85).aspx</linkUri></externalLink><externalLink><linkText>cómo el cliente establece una conexión</linkText><linkUri>https://msdn.microsoft.com/library/aa373937(VS.85).aspx</linkUri> </externalLink> <externalLink> <linkText>Tráfico de replicación de restricción de Active Directory y cliente RPC el tráfico a un puerto específico</linkText><linkUri>https://support.microsoft.com/kb/224196</linkUri></externalLink><externalLink><linkText>SPN de un controlador de dominio de destino en AD DS</linkText><linkUri>https://msdn.microsoft.com/library/dd207688(PROT.13).aspx</linkUri></externalLink></relatedTopics>
</developerConceptualDocument>


