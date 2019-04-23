---
ms.assetid: 0f21951c-b1bf-43bb-a329-bbb40c58c876
title: 'Error de replicación 1753: no hay más puntos de conexión disponibles desde el asignador de puntos de conexión'
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e429c87a2194ecfaf02c3d6c579eda75293250d4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827516"
---
# <a name="replication-error-1753-there-are-no-more-endpoints-available-from-the-endpoint-mapper"></a>Error de replicación 1753: no hay más puntos de conexión disponibles desde el asignador de puntos de conexión

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


<developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd"> <introduction>
    <para>Este tema explica los síntomas, causas y cómo resolver Active Directory replication error 8524 la operación DSA no puede continuar debido a un error de búsqueda DNS.</para>
    <list class="bullet">
      <listItem>
        <para>
          <link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_Symptoms">Síntomas</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_Cause">Causa</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_Resolutions">Soluciones</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_MoreInfo">Obtener más información</link>
        </para>
      </listItem>
    </list>
  </introduction>
  <section address="BKMK_Symptoms">
    <title>Síntomas</title>
    <content>
      <para>En este artículo se describe los síntomas, causa y resolución de los pasos para las operaciones de Active Directory que generan el error de Win32 1753: "No hay no hay más extremos disponibles desde el endpoint mapper."</para>
      <list class="ordered">
        <listItem>
          <para>DCDIAG informes que ha fallado la prueba de conectividad, prueba de replicaciones de Active Directory o KnowsOfRoleHolders Error 1753: "No hay no hay más extremos disponibles desde el endpoint mapper."</para>
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
<listItem><para>REPADMIN. EXE notifica que ese error de duplicación con estado 1753.</para><para>REPADMIN comandos que aparecen normalmente con el estado 1753 pueden incluir, pero no se limitan a:</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><tbody><tr><TD><list class="bullet"><listItem><para>REPADMIN/REPLSUM.</para></listItem><listItem><para>REPADMIN /SHOWREPL</para></listItem></list></TD><TD><list class="bullet"><listItem><para>REPADMIN /SHOWREPS</para></listItem><listItem><para>REPADMIN /SYNCALL</para></listItem></list></TD></tr></tbody></table><para>Salida de ejemplo de "REPADMIN /SHOWREPS" que representan la replicación de entrada desde CONTOSO-DC2 a CONTOSO-DC1 produzca el error "se denegó el acceso de replicación" se muestra a continuación:</para><code>Default-First-Site-NameCONTOSO-DC1
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

</code></listItem><listItem><para>El <ui>comprobar la topología de replicación</ui> comando en sitios de Active Directory y servicios devuelve "No hay no hay más extremos disponibles desde el endpoint mapper."</para><para>Con el botón secundario en el objeto de conexión de un controlador de dominio de origen y elegir <ui>comprobar la topología de replicación</ui> se produce un error con "No hay no hay más extremos disponibles desde el endpoint mapper." El que aparecen en pantalla se muestra el mensaje de error siguiente:</para><para>Texto de título del cuadro de diálogo: Comprobar la topología de replicación</para><para>Texto de mensaje del cuadro de diálogo: </para><para>Se produjo el error siguiente durante el intento de ponerse en contacto con el controlador de dominio: No hay no hay más extremos disponibles desde el endpoint mapper.</para></listItem><listItem><para>El <ui>Replicar ahora</ui> comando en sitios de Active Directory y servicios devuelve "no hay no hay más extremos disponibles desde el endpoint mapper."</para><para>Con el botón secundario en el objeto de conexión de un controlador de dominio de origen y elegir <ui>Replicar ahora</ui> se produce un error con "No hay no hay más extremos disponibles desde el endpoint mapper." El que aparecen en pantalla se muestra el mensaje de error siguiente:</para><para>Texto de título del cuadro de diálogo: Replicar ahora</para><para>Texto de mensaje del cuadro de diálogo: Se produjo el error siguiente durante el intento de sincronizar el contexto de nomenclatura &lt;% nombre de partición de directorio %&gt; del controlador de dominio &lt;DC de origen&gt; al controlador de dominio &lt;deDCdedestino&gt;:</para><para>

No hay no hay más extremos disponibles desde el endpoint mapper.</para><para>La operación no continuará.</para></listItem><listItem><para>NTDS KCC NTDS generales o Microsoft-Windows-ActiveDirectory_DomainService eventos con el estado-2146893022 se registran en el registro de servicios de directorio en el Visor de eventos.</para><para>Eventos de Active Directory que se suelen citan el estado-2146893022 incluyen, pero no se limitan a:</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><thead><tr><TD><para>Id. de evento</para></TD><TD><para>Origen del evento</para></TD><TD><para>Cadena de eventos</para></TD></tr></thead><tbody><tr><TD><para>1655</para></TD><TD><para>NTDS General</para></TD><TD><para>Active Directory intentó comunicarse con el siguiente catálogo global y los intentos no tuvieron éxito.</para></TD></tr><tr><TD><para>1925</para></TD><TD><para>NTDS KCC</para></TD><TD><para>Error al intentar establecer un vínculo de replicación para la siguiente partición de directorio de escritura.</para></TD></tr><tr><TD><para>1265</para></TD><TD><para>NTDS KCC</para></TD><TD><para>Error en un intento por Comprobador de coherencia de información (KCC) para agregar un acuerdo de replicación para el siguiente directorio partición y el origen de controlador de dominio.</para></TD></tr></tbody></table></listItem>
</list>
    </content>
  </section>
  <section address="BKMK_Cause">
    <title>Causa</title>
    <content>
      <para>El diagrama siguiente muestra el flujo de trabajo RPC empezando con el registro de la aplicación de servidor con el asignador de punto de conexión de RPC (EPM) en el paso 1 para la transferencia de datos desde el cliente RPC a la aplicación cliente en el paso 7. </para>
      <para>&lt;ADDS_RPCWorkflow&gt;</para>
      <para>Pasos 1 a 7 mapa a las siguientes operaciones:</para>
      <list class="ordered">
        <listItem>
          <para>Aplicación de servidor registra sus puntos de conexión con el asignador de punto de conexión de RPC (EPM) </para>
        </listItem>
        <listItem>
          <para>Cliente realiza una llamada RPC (nombre del usuario, sistema operativo o aplicación iniciado un funcionamiento) </para>
        </listItem>
        <listItem>
          <para>RPC del cliente se pone en contacto con los equipos de destino EPM y pedir el punto de conexión completar la llamada de cliente </para>
        </listItem>
        <listItem>
          <para>Responde EPM del equipo servidor con un punto de conexión </para>
        </listItem>
        <listItem>
          <para>RPC del cliente que pone en contacto con la aplicación de servidor </para>
        </listItem>
        <listItem>
          <para>Aplicación de servidor se ejecuta la llamada, devuelve el resultado al cliente RPC </para>
        </listItem>
        <listItem>
          <para>RPC del cliente pasa el resultado a la aplicación cliente</para>
        </listItem>
      </list>
      <para>Error 1753 genera un error de entre los pasos 3 # y #4. En concreto, el error 1753 significa que el cliente RPC (controlador de dominio de destino) fue capaz de ponerse en contacto con el servidor de RPC (controlador de dominio de origen) a través del puerto 135, pero el EPM en el servidor de RPC (controlador de dominio de origen) no pudo encontrar la aplicación de RPC de interés y devuelve el error en el servidor de 1753. La presencia del error 1753 indica que el cliente RPC (controlador de dominio de destino) recibió la respuesta de error del lado servidor desde el servidor de RPC (origen de replicación de AD DC) a través de la red. </para>
      <para>Causas raíz específica del error 1753 son: </para>
      <list class="ordered">
        <listItem>
          <para>La aplicación de servidor se ha iniciado nunca (es decir, nunca se intentó el paso 1 en el diagrama "más información" situado encima de).</para>
        </listItem>
        <listItem>
          <para>Inicia la aplicación de servidor, pero se produjo algún error durante la inicialización que impidió su registro con el Endpoint Mapper de RPC (es decir, paso 1 en el diagrama "más información" anterior se intentó pero no se pudo).</para>
        </listItem>
        <listItem>
          <para>La aplicación de servidor se inició, pero posteriormente muerto. (es decir, paso 1 en el diagrama "más información" anterior se completó correctamente, pero se deshizo más adelante, porque el servidor muerto).</para>
        </listItem>
        <listItem>
          <para>La aplicación de servidor anulado su registro manualmente sus puntos de conexión (similares a 3, pero intencionado. Pero es probable que no se incluye por integridad.)</para>
        </listItem>
        <listItem>
          <para>El cliente RPC (controlador de dominio de destino) puede establecer contacto con un servidor RPC diferente que el previsto debido a un nombre para el error de asignación de IP en DNS, WINS o archivo Lmhosts/host.</para>
        </listItem>
      </list>
      <para>Error 1753 no está causado por: </para>
      <list class="bullet">
        <listItem>
          <para>Una falta de conectividad de red entre el cliente RPC (controlador de dominio de destino) y el servidor de RPC (controlador de dominio de origen) a través del puerto 135</para>
        </listItem>
        <listItem>
          <para>Una falta de conectividad de red entre el servidor RPC (controlador de dominio de origen) mediante el puerto 135 y el cliente RPC (controlador de dominio de destino) a través del puerto efímero. </para>
        </listItem>
        <listItem>
          <para>Un error de coincidencia de contraseña o la imposibilidad por el controlador de dominio de origen para descifrar un paquete cifrado de Kerberos </para>
        </listItem>
      </list>
      <para> </para>
    </content>
  </section>
  <section address="BKMK_Resolutions">
    <title>Soluciones</title>
    <content>
      <list class="ordered">
        <listItem>
          <para>
            <embeddedLabel>Compruebe que se ha iniciado el servicio de registro de su servicio con el asignador de extremos</embeddedLabel>
          </para>
          <para>Para Windows 2000 y controladores de dominio de Windows Server 2003: asegúrese de que el DC de origen se arranca en modo normal. </para>
          <para>
Para Windows Server 2008 o Windows Server 2008 R2: desde la consola del controlador de dominio de origen, inicie el Administrador de servicios (services.msc) y compruebe que la <embeddedLabel>Active Directory Domain Services</embeddedLabel> servicio se está ejecutando. </para>
        </listItem>
        <listItem>
          <para>
            <embeddedLabel>Compruebe que ese cliente RPC (controlador de dominio de destino) conectado al servidor RPC adecuado (controlador de dominio de origen)</embeddedLabel>
          </para>
          <para>Todos los controladores de dominio en un bosque de Active Directory registrarán un registro CNAME de controlador de dominio en la zona _msdcs. &lt;dominio raíz del bosque&gt; zona DNS con independencia de los dominios que residen en el bosque. El registro CNAME de controlador de dominio se deriva el <embeddedLabel>objectGUID</embeddedLabel> atributo del objeto configuración NTDS para cada controlador de dominio. </para>
          <para>Al realizar operaciones basadas en replicación, un controlador de dominio de destino consulta DNS para el registro CNAME de controladores de dominio de origen. El registro CNAME contiene el nombre completo del equipo de DC de origen que se usa para derivar la dirección IP de los controladores de dominio de origen a través de la búsqueda en caché de cliente DNS de Host / LMHost archivo lookup, un host / registro AAAA de DNS o WINS. </para>
          <para>Obsoleto objetos de configuración NTDS y asignaciones de nombre a IP incorrectas en DNS, WINS, Host y LMHOST archivos pueden hacer que el cliente RPC (controlador de dominio de destino) para conectarse al servidor RPC incorrecto (controlador de dominio de origen). Además, la asignación de nombre a IP incorrecta puede hacer que el cliente RPC (controlador de dominio de destino) para conectarse a un equipo que aún no tiene la aplicación de servidor de RPC de interés (el rol de Active Directory en este caso) instalado. (Ejemplo: un registro de host antiguos para DC2 contiene la dirección IP del equipo miembro o DC3). </para>
          <para>Compruebe que el objectGUID para el controlador de dominio de origen que existe en el destino de copia de los controladores de dominio de Active Directory coincide con el objectGUID de DC de origen almacenado en el origen de copia de los controladores de dominio de Active Directory. Si hay una discrepancia, utilice repadmin /showobjmeta en el objeto de configuración ntds para ver cuál corresponde al último promoción del controlador de dominio de origen (Sugerencia: comparar las marcas de fecha para el objeto configuración NTDS fecha de creación de /showobjmeta frente a la última fecha de promoción en el archivo de dcpromo.log de controladores de dominio de origen. Es posible que deba usar la última modificación / creación de la fecha de la DCPROMO. Archivo de registro). Si los GUID de objeto no son idéntico, el destino es probable que el controlador de dominio tiene un objeto de configuración NTDS obsoleto para el DC de origen cuyo registro CNAME hace referencia a un registro de host con un nombre incorrecto para la asignación de direcciones IP. </para>
          <para>En el controlador de dominio de destino, ejecute IPCONFIG /ALL para determinar qué servidores DNS el DC está usando para la resolución de destino:</para>
          <code>c:&gt;ipconfig /all</code>
          <para>En el controlador de dominio de destino, ejecute NSLOOKUP en el registro CNAME de controlador de dominio completo de los controladores de dominio de origen:</para>
          <code>c:&gt;nslookup -type=cname &lt;fully qualified cname of source DC&gt; &lt;destination DCs primary DNS Server IP &gt;
c:&gt;nslookup -type=cname &lt;fully qualified cname of source DC&gt; &lt;destination DCs secondary DNS Server IP&gt;</code>
          <para>Comprobar que la dirección IP devuelta por NSLOOKUP "posee" el nombre de host / identidad de seguridad de lo DC de origen:</para>

          <code>C:&gt;NBTSTAT -A &lt;IP address returned by NSLOOKUP in the step above&gt;</code>
          <para>o bien</para>
          <para>Inicie sesión en la consola del controlador de dominio de origen, ejecute "IPCONFIG" desde el símbolo del sistema y compruebe que el DC de origen pertenece a la dirección IP devuelta por el comando NSLOOKUP anterior</para>
          <para>Busque el host obsoleto o duplicado para las asignaciones de IP en DNS</para>
          <code>NSLOOKUP -type=hostname &lt;single label hostname of source DC&gt; &lt;primary DNS Server IP on destination DC&gt;
NSLOOKUP -type=hostname &lt;single label hostname of source DC&gt; &lt;secondary DNS Server IP on destination DC&gt;

NSLOOKUP -type=hostname &lt;fully qualified computer name of source DC&gt; &lt;primary DNS Server IP on destination DC&gt;
NSLOOKUP -type=hostname &lt;fully qualified computer name of source DC&gt; &lt;secondary DNS Server IP on dest. DC&gt;</code>
<para>Si hay direcciones IP no válidas en los registros de host, investigue si está habilitada y configurada correctamente la limpieza de DNS. </para><para>Si las pruebas por encima o a un seguimiento de red no muestra una consulta con nombre devuelve una dirección IP válida, considere la posibilidad de las entradas obsoletas en los archivos del HOST, archivos LMHOSTS y servidores WINS. Tenga en cuenta que los servidores DNS también puede configurarse para realizar la resolución de reserva de WINS.</para>
</listItem>
        <listItem>
          <para>
            <embeddedLabel>Compruebe que la aplicación de servidor (Active Directory etc) se ha registrado con el asignador de extremos en el servidor RPC (controlador de dominio de origen)</embeddedLabel>
          </para>
          <para>Active Directory utiliza una combinación de los puertos conocidos y dinámicamente registrados. Esta tabla enumeran los conocidos puertos y protocolos usados por los controladores de dominio de Active Directory.</para>
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
                  <para>Microsoft-DS</para>
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
                  <para>SSL DE LDAP</para>
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
                  <para>Servidor del catálogo global (GC)</para>
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
                  <para>Servidor del catálogo global (GC)</para>
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
          <para>Los puertos conocidos no se registran con el asignador de extremos. </para>
          <para>Active Directory y otras aplicaciones también registran servicios que reciben los puertos asignados dinámicamente en el intervalo de puerto efímero de RPC. Estas aplicaciones de servidor RPC se asignan dinámicamente puertos entre 49152 y 65535 intervalo en los equipos de Windows Server 2008 y Windows Server 2008 R2 y los puertos TCP entre 1024 y 5000 en equipos con Windows 2000 y Windows Server 2003. El puerto RPC usado por la replicación puede ser codificada de forma rígida en el registro mediante los pasos documentados en <externalLink> <linkText>artículo de KB 224196</linkText><linkUri>https://support.microsoft.com/kb/224196</linkUri></externalLink>. Active Directory se sigue para registrarse en el EPM cuando se configura para utilizar un puerto codificado. </para>
          <para>Compruebe que la aplicación de servidor RPC de interés se ha registrado con el endpoint mapper de RPC en el servidor de RPC (el DC de origen en el caso de la replicación de AD). </para>
          <para>Hay varias maneras de realizar esta tarea, pero uno consiste en instalar y ejecutar PORTQRY desde un símbolo del sistema con privilegios de administrador en la consola del origen de controlador de dominio mediante la sintaxis: </para>
          <code>c:\&gt;portquery -n &lt;source DC&gt; -e 135 &gt;file.txt</code>
          <para>En la salida de portqry, tenga en cuenta los números de puerto registrados dinámicamente por "MS NT directorio DRS interfaz" (UUID =... 351) para el <embeddedLabel>protocolo ncacn_ip_tcp</embeddedLabel>. El fragmento de código siguiente muestra la salida portquery desde un controlador de dominio de Windows Server 2008 R2 y el UUID / par del protocolo que se utiliza específicamente en Active Directory se resalta en <embeddedLabel>negrita</embeddedLabel>: </para>
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
          <para>Otras formas de resolver este error:</para>
          <list class="ordered">
            <listItem>
              <para>Compruebe que el DC de origen se ha reiniciado en modo normal y que el rol de sistema operativo y el controlador de dominio en el DC de origen ha iniciado completamente.</para>
            </listItem>
            <listItem>
              <para>Compruebe que se está ejecutando el servicio de dominio de Active Directory. Si el servicio está detenido o no se ha configurado con valores de inicio de forma predeterminada, restablecer los valores de inicio predeterminada, reinicie el controlador de dominio modificada, a continuación, vuelva a intentar la operación.</para>
            </listItem>
            <listItem>
              <para>Compruebe que el estado de valor y el servicio de inicio para el servicio RPC y ubicación de RPC es correcto para la versión del sistema operativo del cliente de RPC (controlador de dominio de destino) y el servidor de RPC (controlador de dominio de origen). Si el servicio está detenido o no se ha configurado con valores de inicio de forma predeterminada, restablecer los valores de inicio predeterminada, reinicie el controlador de dominio modificada, a continuación, vuelva a intentar la operación.</para>
              <para>Además, asegúrese de que el contexto de servicio coincide con los valores predeterminados aparecen en la tabla siguiente.</para>
              <table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11">
                <thead>
                  <tr>
                    <TD>
                      <para>Servicio</para>
                    </TD>
                    <TD>
                      <para>Estado predeterminado (tipo de inicio) en Windows Server 2003 y versiones posteriores </para>
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
                      <para>Inicia (automático)</para>
                    </TD>
                    <TD>
                      <para>Inicia (automático)</para>
                    </TD>
                  </tr>
                  <tr>
                    <TD>
                      <para>Localizador de llamadas a procedimiento remoto</para>
                    </TD>
                    <TD>
                      <para>Null o detenido (Manual)</para>
                    </TD>
                    <TD>
                      <para>Inicia (automático)</para>
                    </TD>
                  </tr>
                </tbody>
              </table>
            </listItem>
            <listItem>
              <para>Compruebe que no se ha restringido el tamaño del intervalo de puertos dinámicos. A continuación, se muestra la sintaxis de Windows Server 2008 y Windows Server 2008 R2 NETSH para enumerar el intervalo de puertos RPC:</para>
              <code>&gt;netsh int ipv4 show dynamicport tcp
&gt;netsh int ipv4 show dynamicport udp
&gt;netsh int ipv6 show dynamicport tcp
&gt;netsh int ipv6 show dynamicport udp</code>
            </listItem>
            <listItem>
              <para>Compruebe que las definiciones de puerto rígida definidas en KB 224196 están dentro del intervalo de puertos dinámicos para la versión de sistema operativo de los controladores de dominio de origen.</para>
              <para>Revisión <externalLink> <linkText>artículo de KB 224196</linkText> <linkUri> https://support.microsoft.com/kb/224196 </linkUri> </externalLink> y asegúrese de que el puerto codificado se encuentra dentro del intervalo de puertos efímeros para la versión del sistema operativo del controlador de dominio de origen.</para>
            </listItem>
            <listItem>
              <para>Compruebe que la clave ClientProtocols existe bajo HKLM\Software\Microsoft\Rpc y contiene los siguientes valores predeterminados de 5:</para>
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
    <title>Obtener más información</title>
    <content>
      <para>
        <embeddedLabel>Ejemplo de un nombre incorrecto al error RPC que causan 1753 frente a-2146893022 de asignación de IP: el nombre de entidad de seguridad de destino es incorrecto</embeddedLabel>
      </para>
      <para>El dominio contoso.com consta de DC1 y DC2 con x.x.1.1 de direcciones IP y x.x.1.2. El host "A" / registros "AAAA" para DC2 se registran correctamente en todos los servidores DNS configurados para DC1. Además, el archivo de HOSTS en DC1 contiene una entrada de asignación de nombre de host DC2s completo a x.x.1.2 de dirección IP. Más adelante, cambia la dirección IP de DC2 de X.X.1.2 X.X.1.3 y un nuevo equipo miembro está unido al dominio con x.x.1.2 de dirección IP. La replicación de AD intentos desencadenan por la <ui>Replicar ahora</ui> comando en el complemento Servicios y sitios de Active Directory produce error 1753 tal como se muestra en el siguiente mensaje de seguimiento:</para>
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
      <para>En el marco <embeddedLabel>10</embeddedLabel>, el asignador de puntos finales de los controladores de dominio de origen de las consultas del DC de destino a través del puerto 135 para la clase de servicio de replicación de Active Directory UUID E351... </para>
      <para>En el marco <embeddedLabel>11</embeddedLabel>, el origen de controlador de dominio, en este caso, un equipo miembro que no hospede aún el rol de controlador de dominio y, por tanto, no se ha registrado el E351... UUID para el servicio de replicación con su EPM local responde con el error simbólico EP_S_NOT_REGISTERED que se asigna a 1753 de error decimal, hexadecimal error 0x6d9 y error descriptivo "hay no hay más extremos disponibles desde el endpoint mapper".</para>
      <para>Más adelante, se promociona el equipo de miembro con x.x.1.2 de dirección IP como una réplica "MayberryDC" en el dominio contoso.com. Nuevamente, el <ui>Replicar ahora</ui> comando se usa para desencadenar la replicación, pero se produce un error en este momento con el que aparecen en pantalla error "el nombre principal de destino es incorrecto." El equipo cuyo adaptador de red se asigna la dirección IP dirección x.x.1.2 <placeholder>es</placeholder> un controlador de dominio, actualmente se arranca en modo normal y se ha registrado el E351... servicio de replicación UUID con su EPM local pero no es el propietario de la identidad de seguridad o nombre de DC2 y no puede descifrar la solicitud de Kerberos de DC1, por lo que la solicitud falla ahora con el error "el nombre principal de destino es correcto". El error se asigna a-2146893022 de error decimal o hexadecimal error 0x80090322. </para>
      <para>Estas asignaciones de IP de host no válidas podrían deberse a entradas obsoletas de host / archivos Lmhosts, alojar un / registros AAAA en DNS o WINS. </para>
      <para>Resumen: En este ejemplo no se pudo porque una asignación de IP de host no válido (en el archivo HOST en este caso) ocasionó que el controlador de dominio de destino resolver a un "origen" controlador de dominio que no tenía los servicios de dominio de Active Directory servicio se está ejecutando (o incluso instalados con este propósito), por lo que la replicación SPN no se ha registrado y el DC de origen ha devuelto el error 1753. En el segundo caso, una asignación de IP de host no válido (nuevo en el archivo HOST) provocó el controlador de dominio para conectarse a un controlador de dominio que se había registrado el E351 destino... SPN de replicación, pero ese origen tenía una identidad diferente del nombre de host y la seguridad del controlador de dominio de origen deseada por lo que no se pudo los intentos con error-2146893022: El nombre principal de destino es incorrecto.</para>
    </content>
  </section>
  <relatedTopics>
    <externalLink>
      <linkText>Solución de problemas de operaciones de Active Directory que generan el error 1753: No hay no hay más extremos disponibles desde el endpoint mapper. </linkText> 
      <linkUri> https://support.microsoft.com/kb/2089874 </linkUri> 
    </externalLink> 
<externalLink> <linkText>839880 errores Endpoint Mapper de RPC de solución de problemas mediante las herramientas de soporte técnico de Windows Server 2003 desde el CD del productodeartículodeKB</linkText> <linkUri> https://support.microsoft.com/kb/839880 </linkUri> </externalLink> 
<externalLink> <linkText>832017 servicio red y la información general sobre requisitos de puerto para el sistema de Windows Server de artículo de KB</linkText> <linkUri> https://support.microsoft.com/kb/832017/ </linkUri> </externalLink> 
<externalLink> <linkText>224196 tráfico de replicación de Active Directory restringe y tráfico de RPC de cliente a un puerto específico del artículo de KB</linkText> <linkUri> https://support.microsoft.com/kb/224196/ </linkUri> </externalLink> 
<externalLink> <linkText>154596 artículo de KB sobre cómo configurar la asignación de puertos dinámicos RPC para trabajar con firewalls</linkText> <linkUri> https://support.microsoft.com/kb/154596 </linkUri> </externalLink> <externalLink> <linkText>Cómo funciona la RPC</linkText><linkUri>https://msdn.microsoft.com/library/aa373935(VS.85).aspx</linkUri></externalLink><externalLink><linkText>cómo prepara el servidor para una conexión</linkText> <linkUri> https://msdn.microsoft.com/library/aa373938(VS.85).aspx </linkUri> </externalLink> 
<externalLink> <linkText>Cómo el cliente establece una conexión</linkText> <linkUri> https://msdn.microsoft.com/library/aa373937(VS.85).aspx </linkUri> </externalLink> <externalLink> <linkText>Registrar la interfaz</linkText><linkUri>https://msdn.microsoft.com/library/aa375357(VS.85).aspx</linkUri></externalLink><externalLink><linkText>hacer que el servidor Disponible en la red</linkText><linkUri>https://msdn.microsoft.com/library/aa373974(VS.85).aspx</linkUri></externalLink><externalLink><linkText>extremos registrados</linkText> <linkUri> https://msdn.microsoft.com/library/aa375255(VS.85).aspx </linkUri> </externalLink> <externalLink> <linkText>Escuchar las llamadas al cliente</linkText> <linkUri> https://msdn.microsoft.com/library/aa373966(VS.85).aspx </linkUri> </externalLink> <externalLink> <linkText>Cómo el cliente establece una conexión</linkText><linkUri>https://msdn.microsoft.com/library/aa373937(VS.85).aspx</linkUri></externalLink><externalLink><linkText>restringir A un puerto específico de tráfico RPC del cliente y tráfico de replicación del directorio de Active</linkText><linkUri>https://support.microsoft.com/kb/224196</linkUri></externalLink><externalLink><linkText>SPN para un controlador de dominio de destino en AD DS</linkText><linkUri>https://msdn.microsoft.com/library/dd207688(PROT.13).aspx</linkUri></externalLink></relatedTopics>
</developerConceptualDocument>


