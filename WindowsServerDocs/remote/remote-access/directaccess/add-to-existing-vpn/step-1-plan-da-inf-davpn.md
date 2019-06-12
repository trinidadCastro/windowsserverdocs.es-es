---
title: Paso 1 Plan de la infraestructura de DirectAccess
description: Este tema forma parte de la Guía de agregar DirectAccess a una implementación de acceso remoto existente (VPN) para Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4ca50ea8-6987-4081-acd5-5bf9ead62acd
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0a761f644fbae489124392f195465bf2acf8e66f
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66805093"
---
# <a name="step-1-plan-directaccess-infrastructure"></a>Paso 1 Plan de la infraestructura de DirectAccess

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

El primer paso a la hora de planear la implementación de acceso remoto en un único servidor consiste en prever la infraestructura necesaria para dicha implementación. En este tema se describen los pasos para la planificación de la infraestructura:  
  
|Tarea|Descripción|  
|----|--------|  
|Planear la topología de red y la configuración|Decide dónde colocar el servidor de acceso remoto (en el perímetro, detrás de un firewall o detrás de un dispositivo de traducción de direcciones de red [NAT]), y planea el direccionamiento IP y el enrutamiento.|  
|Planear requisitos de firewall|Planea la configuración de los firewalls perimetrales para permitir el paso de tráfico de Acceso remoto.|  
|Planear requisitos de certificados|Acceso remoto puede usar Kerberos o certificados para la autenticación de clientes. En esta implementación básica de acceso remoto, Kerberos se configura automáticamente y la autenticación se realiza con un certificado autofirmado emitido automáticamente por el servidor de acceso remoto.|  
|Planear los requisitos de DNS|Planea la configuración de DNS para el servidor de acceso remoto, los servidores de infraestructura, las opciones de resolución local de nombres y la conectividad de clientes.|  
|Planear Active Directory|Planea los requisitos de los controladores de dominio y de Active Directory.|  
|Planear objetos de directiva de grupo|Decide qué GPO se necesitan en tu organización y cómo crearlos o editarlos.|  
  
No es necesario completar las tareas de planificación en un orden específico.  
  
## <a name="plan-network-topology-and-settings"></a>Planear la topología de red y la configuración  
  
### <a name="plan-network-adapters-and-ip-addressing"></a>Planear los adaptadores de red y el direccionamiento IP  
  
1. Identifica la topología de adaptadores de red que quieres usar. El acceso remoto se puede configurar de las siguientes maneras:  
  
    - Con dos adaptadores de red: Ya sea en el perímetro con un adaptador de red conectado a Internet y el otro a la red interna o detrás de NAT, firewall o un dispositivo enrutador, con un adaptador de red conectado a una red perimetral y el otro a la red interna.  
  
    - Detrás de un dispositivo NAT con un adaptador de red: El servidor de acceso remoto se instala detrás de un dispositivo NAT y el único adaptador de red está conectado a la red interna.  
  
2. Identifica tus requisitos de direccionamiento IP:  
  
    DirectAccess usa IPv6 con IPsec para crear una conexión segura entre los equipos cliente de DirectAccess y la red corporativa interna. Sin embargo, DirectAccess no requiere necesariamente conectividad con Internet IPv6 ni compatibilidad nativa con IPv6 en las redes internas. En su lugar, configura y usa automáticamente tecnologías de transición IPv6 para tunelizar el tráfico IPv6 a través de Internet IPv4 (6to4, Teredo, IP-HTTPS) y a través de la intranet solo IPv4 (mediante NAT64 o ISATAP). Para obtener información general acerca de estas tecnologías de transición, consulta los siguientes recursos:  
  
    - [Tecnologías de transición IPv6](https://technet.microsoft.com/library/bb726951.aspx)  
  
    - [Especificación del protocolo de túnel IP-HTTPS](https://msdn.microsoft.com/library/dd358571.aspx)  
  
3. Configura los adaptadores y el direccionamiento obligatorios según la tabla siguiente. Para implementaciones detrás de un dispositivo NAT con un único adaptador de red, configure las direcciones IP utilizando solo la columna "adaptador de red interna".  
  
    |Tipo de dirección IP|Adaptador de red externo|Adaptador de red interno|Requisitos de enrutamiento|  
    |-|--------------|--------------------|------------|  
    |Intranet IPv4 e Internet IPv4|Configura lo siguiente:<br/><br/>-Una dirección IPv4 pública estática con las máscaras de subred adecuada.<br/>-Puerta de enlace predeterminada dirección IPv4 del enrutador de proveedor (ISP) de servicios de Internet local o firewall de Internet.|Configura lo siguiente:<br/><br/>-Una dirección de intranet IPv4 con la máscara de subred adecuada.<br/>-Un sufijo DNS específico de la conexión del espacio de nombres de intranet. También se debe configurar el servidor DNS en la interfaz interna.<br/>-No configure una puerta de enlace predeterminada en las interfaces de la intranet.|Para configurar el servidor de acceso remoto de manera que tenga acceso a todas las subredes de la red IPv4 interna, haz lo siguiente:<br/><br/>1.  Enumere los espacios de direcciones IPv4 para todas las ubicaciones de la intranet.<br/>2.  Usa los comandos **route add -p** o **netsh interface ipv4 add route** para agregar los espacios de direcciones IPv4 como rutas estáticas a la tabla de enrutamiento IPv4 del servidor de acceso remoto.|  
    |Internet IPv6 e intranet IPv6|Configura lo siguiente:<br/><br/>-Use la configuración de dirección configurada automáticamente proporcionada por su ISP.<br/>-Use el **enrutar impresión** comando para asegurarse de que existe una ruta de IPv6 predeterminada que apunta al enrutador del ISP en la tabla de enrutamiento de IPv6.<br/>-Determine si los enrutadores del ISP y de intranet están usando preferencias del enrutador predeterminadas descritas en RFC 4191 y usando una preferencia predeterminada mayor que los enrutadores de la intranet local. Si ambas condiciones se cumplen, no se necesita ninguna otra configuración para la ruta predeterminada. La preferencia mayor para el enrutador del ISP asegura que la ruta IPv6 predeterminada activa del servidor de acceso remoto apunta a Internet IPv6.<br/><br/>Como el servidor de acceso remoto es un enrutador IPv6, si tienes una infraestructura IPv6 nativa, la interfaz de Internet también puede tener acceso a los controladores de dominio de la intranet. En este caso, agrega filtros de paquetes al controlador de dominio de la red perimetral que impidan que el servidor de acceso remoto conecte con la dirección IPv6 de la interfaz accesible desde Internet.|Configura lo siguiente:<br/><br/>-Si no usas los niveles de preferencia de forma predeterminada, configura las interfaces de intranet con el **netsh interface ipv6 establecer InterfaceIndex ignoredefaultroutes = habilitado** comando. Este comando garantiza que las rutas predeterminadas adicionales que señalen a enrutadores de la intranet no se agregarán a la tabla de enrutamiento IPv6. Para conocer el índice de las interfaces de la intranet, usa el comando “netsh interface show interface”.|Si la intranet es IPv6, haz lo siguiente para configurar el servidor de acceso remoto para que tenga acceso a todas las ubicaciones IPv6:<br/><br/>1.  Enumere los espacios de direcciones IPv6 para todas las ubicaciones de la intranet.<br/>2.  Usa el comando **netsh interface ipv6 add route** para agregar los espacios de direcciones IPv6 como rutas estáticas a la tabla de enrutamiento IPv6 del servidor de acceso remoto.|  
    |Internet IPv6 e intranet IPv4|El servidor de acceso remoto reenvía el tráfico de la ruta IPv6 predeterminada a través del Adaptador 6to4 de Microsoft con una retransmisión 6to4 en Internet por IPv4. Puedes configurar un servidor de acceso remoto para la dirección IPv4 de la retransmisión 6to4 de Microsoft en Internet IPv4 (se usa cuando no hay implementado IPv6 nativo en la red corporativa) con el comando netsh interface ipv6 6to4 set relay name=192.88.99.1 state=enabled.|||  
  
    > [!NOTE]
    > 1. Si se ha asignado una dirección IPv4 pública al cliente de DirectAccess, este usará la tecnología de transición 6to4 para conectarse a la intranet. Si el cliente de DirectAccess no puede conectarse al servidor de DirectAccess mediante 6to4, usará IP-HTTPS.  
    > 2. Los equipos cliente IPv6 nativos pueden conectarse al servidor de acceso remoto a través de IPv6 nativo, y no se necesita ninguna tecnología de transición.  
  
### <a name="plan-firewall-requirements"></a>Planear requisitos de firewall

Si el servidor de acceso remoto está detrás de un firewall perimetral, se necesitan las siguientes excepciones para el tráfico de acceso remoto cuando el servidor de acceso remoto se encuentre en Internet IPv4:  
  
- 6to4 del tráfico IP - protocolo 41 de entrada y salida.  
  
- IP-HTTPS-puerto de destino 443 de protocolo de Control de transmisión (TCP) y el puerto de origen TCP 443 de salida.  
  
- Si implementas el acceso remoto con un único adaptador de red y lo instalas en el servidor de ubicación de red en el servidor de acceso remoto, también deberás agregar el puerto TCP 62000 a las excepciones.  
  
Se necesitarán las siguientes excepciones para el tráfico de acceso remoto cuando el servidor de acceso remoto se encuentre en Internet IPv6:  
  
- Protocolo IP 50  
  
- Puerto de destino UDP 500 de entrada y puerto de origen UDP 500 de salida.  
  
Si usa firewalls adicionales, aplique las siguientes excepciones de firewall de la red interna para el tráfico de acceso remoto:  
  
- ISATAP: protocolo 41 de entrada y salida  
  
- TCP/UDP para todo el tráfico IPv4/IPv6  
  
### <a name="plan-certificate-requirements"></a>Planear requisitos de certificados

Entre los requisitos de certificados para IPsec se incluye un certificado de equipo que los equipos cliente de DirectAccess usan al establecer la conexión IPsec entre el cliente y el servidor de acceso remoto, y un certificado de equipo que los servidores de acceso remoto usan para establecer conexiones IPsec con los clientes de DirectAccess. Para DirectAccess en Windows Server 2012 no es obligatorio el uso de estos certificados IPsec. El Asistente para habilitar DirectAccess configura el servidor de acceso remoto para que actúe como proxy Kerberos a fin de realizar la autenticación de IPsec sin necesidad de certificados.  
  
1. **Servidor IP-HTTPS**: Al configurar el acceso remoto, el servidor de acceso remoto se configura automáticamente para que actúe como el agente de escucha IP-HTTPS. El sitio IP-HTTPS requiere un certificado de sitio web y los equipos cliente deben poder ponerse en contacto con el sitio de la lista de revocación de certificados (CRL) para consultar el certificado. El Asistente para habilitar DirectAccess intenta usar el certificado SSTP de VPN. Si SSTP no está configurado, comprueba si en el almacén personal del equipo hay un certificado para IP-HTTPS. Si no hay ninguno disponible, crea automáticamente un certificado autofirmado.  
  
2. **Servidor de ubicación de red**: El servidor de ubicación de red es un sitio web que se usa para detectar si los equipos cliente están ubicados en la red corporativa. El servidor de ubicación de red requiere un certificado de sitio web. Los clientes de DirectAccess deben poder contactar con el sitio de la CRL para obtener el certificado. El Asistente para habilitar DirectAccess comprueba si hay un servidor de ubicación de red en el almacén personal del equipo. Si no hay ninguno, creará automáticamente un certificado autofirmado.  
  
En la tabla siguiente encontrarás un resumen de los requisitos de certificación para cada uno de estos:  
  
|Autenticación IPsec|Servidor IP-HTTPS|Servidor de ubicación de red|  
|------------|----------|--------------|  
|Una CA interna es necesaria para emitir certificados de equipo para el servidor de acceso remoto y los clientes para la autenticación IPsec si no se usa el proxy Kerberos para la autenticación|Entidad de certificación pública: Se recomienda usar una CA pública para emitir el certificado IP-HTTPS, esto garantiza que el punto de distribución CRL esté disponible externamente.|Entidad de certificación interna: Puedes usar una entidad de certificación interna para emitir el certificado de sitio web del servidor de ubicación de red. Asegúrate de que el punto de distribución de CRL tenga alta disponibilidad desde la red interna.|  
||Entidad de certificación interna: Puedes usar una entidad de certificación interna para emitir el certificado IP-HTTPS; sin embargo, debes asegurarte de que el punto de distribución CRL esté disponible externamente.|Certificado autofirmado: Puede usar un certificado autofirmado para el sitio Web servidor de ubicación de red; Sin embargo, no puede usar un certificado autofirmado en implementaciones multisitio.|  
||Certificado autofirmado: Puedes usar un certificado autofirmado para el servidor IP-HTTPS; sin embargo, debes asegurarte de que el punto de distribución CRL esté disponible externamente. Los certificados autofirmados no se pueden usar en implementaciones multisitio.||  
  
#### <a name="plan-certificates-for-ip-https"></a>Planear certificados para IP-HTTPS

El servidor de acceso remoto actúa como agente de escucha de IP-HTTPS y tienes que instalar manualmente un certificado de sitio web HTTPS en el servidor. A la hora de planear, ten en cuenta lo siguiente:  
  
- Se recomienda usar una entidad de certificación pública para que haya CRL disponibles.  
  
- En el campo de sujeto, especifica la dirección IPv4 del adaptador de Internet del servidor de acceso remoto o el FQDN de la dirección URL de IP-HTTPS (la dirección ConnectTo). Si el servidor de acceso remoto está ubicado detrás de un dispositivo NAT, hay que especificar el nombre público o la dirección del dispositivo NAT.  
  
- El nombre común del certificado debe coincidir con el nombre del sitio IP-HTTPS.  
  
- En el campo Uso mejorado de claves, use el identificador de objeto (OID) Autenticación de servidor.  
  
- Para el campo Puntos de distribución CRL, indica un punto de distribución CRL al que puedan obtener acceso los clientes de DirectAccess que estén conectados a Internet.  
  
- El certificado IP-HTTPS debe tener una clave privada.  
  
- El certificado IP-HTTPS se debe importar directamente al almacén personal.  
  
- Los nombres de certificado IP-HTTPS pueden contener comodines.  
  
#### <a name="plan-website-certificates-for-the-network-location-server"></a>Planear certificados de sitio web para el servidor de ubicación de red

A la hora de planear el sitio web del servidor de ubicación de red, ten en cuenta lo siguiente:  
  
- En el campo Asunto, especifica una dirección IP de la interfaz de intranet del servidor de ubicación de red o el FQDN de la dirección URL de la ubicación de red.  
  
- Para el campo Uso mejorado de clave, usa el OID Autenticación de servidor.  
  
- Para el campo Puntos de distribución CRL, un punto de distribución de CRL que sea accesible por los clientes de DirectAccess que están conectados a la intranet. Este punto de distribución CRL no debe ser accesible desde fuera de la red interna.  
  
- Si más adelante tienes previsto configurar una implementación multisitio o en clúster, el nombre del certificado no debe coincidir con el nombre interno del servidor de acceso remoto.  

    > [!NOTE]  
    > Asegúrate de que los certificados de IP-HTTPS y el servidor de ubicación de red tengan un **Nombre de sujeto**. Si el certificado no tiene un **Nombre e sujeto** sino que tiene un **Nombre alternativo**, el Asistente para acceso remoto no lo aceptará.  
  
#### <a name="plan-dns-requirements"></a>Planear los requisitos de DNS

En una implementación de acceso remoto, se necesita DNS para lo siguiente:  
  
- **Las solicitudes de cliente de DirectAccess**: DNS se usa para resolver las solicitudes de equipos cliente de DirectAccess que no están ubicados en la red interna. Los clientes de DirectAccess intentan conectarse al servidor de ubicación de red de DirectAccess para determinar si están ubicados en Internet o en la red corporativa: Si la conexión es correcta, se determinará que los clientes están en la intranet, por lo que no se usará DirectAccess y las solicitudes de clientes se resolverán mediante el servidor DNS configurado en el adaptador de red del equipo cliente. Si se producen errores de conexión, se da por hecho que los clientes están en Internet. Los clientes de DirectAccess usarán la tabla de directivas de resolución de nombres (NRPT) para determinar el servidor DNS que usarán para resolver solicitudes de nombres. Puedes especificar que los clientes tengan que usar DNS64 de DirectAccess para resolver nombres, o bien un servidor DNS interno alternativo. Para llevar a cabo la resolución de nombres, los clientes de DirectAccess usan la tabla NRPT para identificar cómo gestionar una solicitud. Los clientes solicitan un FQDN o nombre de etiqueta única, como <https://internal>. Si se solicita un nombre de etiqueta única, se anexa un sufijo DNS para crear un FQDN. Si la consulta DNS coincide con una entrada de la NRPT y se especifica DNS4 o un servidor DNS de intranet para la entrada, la consulta para la resolución de nombres se enviará mediante el servidor especificado. Si existe una coincidencia pero no se especifica ningún servidor DNS, esto indica una regla de exención y se aplicará la resolución de nombres normal.  
  
    Ten en cuenta que cuando se agrega un nuevo sufijo a la tabla NRPT en la consola Administración de acceso remoto, los servidores DNS predeterminados para el sufijo se pueden detectar automáticamente haciendo clic en el botón **Detectar**. La detección automática funciona de la siguiente manera:  
  
    1. Si la red corporativa se basa en IPv4 o usa IPv4 e IPv6, la dirección predeterminada es la dirección DNS64 del adaptador interno en el servidor de acceso remoto.  
  
    2. Si la red corporativa se basa en IPv6, la dirección predeterminada es la dirección IPv6 de los servidores DNS en la red corporativa.  
  
-  **Servidores de infraestructura**  
  
    1. **Servidor de ubicación de red**: Los clientes de DirectAccess intentan contactar con el servidor de ubicación de red para determinar si están en la red interna. Los clientes de la red interna deben ser capaces de resolver el nombre del servidor de ubicación de red, pero debe evitarse que resuelvan el nombre si se encuentran en Internet. Para que esto suceda, de manera predeterminada, el FQDN del servidor de ubicación de red se agregará como una regla de exención a la NRPT. Además, cuando se configura el acceso remoto, se crean automáticamente las siguientes reglas:  
  
        1. Una regla de sufijo DNS para el dominio raíz o el nombre de dominio del servidor de acceso remoto y las direcciones IPv6 correspondientes a los servidores DNS de la intranet configurados en el servidor de acceso remoto. Por ejemplo, si el servidor de acceso remoto es miembro del dominio corp.contoso.com, se crea una regla para el sufijo DNS .corp.contoso.com.  
  
        2. Una regla de exención para el FQDN del servidor de ubicación de red. Por ejemplo, si la URL del servidor de ubicación de red es <https://nls.corp.contoso.com>, se crea una regla de exención para el FQDN nls.corp.contoso.com.  
  
        **Servidor IP-HTTPS**: El servidor de acceso remoto actúa como agente de escucha IP-HTTPS y usa su certificado de servidor para autenticar a los clientes IP-HTTPS. Los clientes de DirectAccess que usan servidores DNS públicos deben poder resolver el nombre IP-HTTPS.  
  
        **Comprobadores de conectividad**: Acceso remoto crea un sondeo web predeterminado utilizado por los equipos de cliente de DirectAccess usan para comprobar la conectividad a la red interna. Para comprobar si el sondeo funciona correctamente es necesario registrar de forma manual los nombres siguientes en DNS:  
  
        1.  DirectAccess-webprobehost debe proporcionar la dirección IPv4 interna del servidor de acceso remoto, o la dirección IPv6 en un entorno de solo IPv6.  
  
        2.  DirectAccess-corpconnectivityhost debe resolver en la dirección de host local (bucle invertido). Es necesario crear un registro A y un registro AAAA: el registro A con el valor 127.0.0.1 y el registro AAAA con el valor compuesto a partir del prefijo NAT64 con los últimos 32 bits como 127.0.0.1. El prefijo NAT64 se puede recuperar si se ejecuta el cmdlet get-netnattransitionconfiguration.  
  
            > [!NOTE]  
            > Esto solo es válido en un entorno de solo IPv4. En un entorno de IPv4+IPv6 o en un entorno de solo IPv6, solo se debe crear un registro AAAA con la dirección IP de bucle invertido ::1.  
  
        Se pueden crear comprobadores de conectividad adicionales con otras direcciones web a través de HTTP o PING. Debe existir una entrada DNS por cada comprobador de conectividad.  
  
#### <a name="dns-server-requirements"></a>Requisitos del servidor DNS  
  
- Para los clientes de DirectAccess, debe usar un servidor DNS que ejecutan Windows Server 2003, Windows Server 2008, Windows Server 2008 R2, Windows Server 2012 o cualquier servidor DNS que admita IPv6.  
  
### <a name="plan-active-directory"></a>Planear Active Directory

Acceso remoto usa Active Directory y objetos de directiva de grupo de Active Directory como sigue:  
  
- **Autenticación**: Active Directory se usa para la autenticación. El túnel de intranet usa la autenticación Kerberos para que el usuario acceda a los recursos internos.  
  
- **Objetos de directiva de grupo**: Acceso remoto recopila las opciones de configuración en los objetos de directiva de grupo que se aplican a servidores de acceso remoto, clientes y servidores de aplicaciones internos.  
  
- **Grupos de seguridad**: Acceso remoto usa grupos de seguridad para recopilar e identificar equipos cliente de DirectAccess y servidores de acceso remoto. Las directivas de grupo se aplican en el grupo de seguridad correspondiente.  
  
- **Directivas IPsec extendidas**: Acceso remoto puede usar la autenticación de IPsec y el cifrado entre los clientes y el servidor de acceso remoto. Puedes extender el cifrado y la autenticación IPsec a los servidores de aplicaciones internos especificados.   
  
#### <a name="active-directory-requirements"></a>Requisitos de Active Directory  
  
A la hora de planear Active Directory para una implementación de acceso remoto, se necesita lo siguiente:  
  
- Al menos un controlador de dominio instalado en los sistemas operativos Windows Server 2012, Windows Server 2008 R2 Windows Server 2008 o Windows Server 2003.  
  
    Si el controlador de dominio está en una red perimetral (y, por lo tanto, es accesible desde el adaptador de red con conexión a Internet del servidor de acceso remoto), impide que el servidor de acceso remoto acceda a él; para ello, agrega filtros de paquetes al controlador de dominio para impedir que el adaptador de Internet conecte con la dirección IP.  
  
- El servidor de acceso remoto debe ser un miembro del dominio.  
  
- Los clientes de DirectAccess deben ser miembros del dominio. Los clientes pueden pertenecer a:  
  
    - Cualquier dominio del mismo bosque que el servidor de acceso remoto.  
  
    - Cualquier dominio que tenga una confianza bidireccional con el dominio del servidor de acceso remoto.  
  
    - Cualquier dominio de un bosque que tenga una confianza bidireccional con el bosque al que pertenece el dominio de acceso remoto.  
  
> [!NOTE]  
> - El servidor de acceso remoto no puede ser un controlador de dominio.  
> - El controlador de dominio de Active Directory que se usa para acceso remoto no debe ser accesible desde el adaptador de Internet externo del servidor de acceso remoto (el adaptador no debe estar en el perfil de dominio del Firewall de Windows).  
  
### <a name="plan-group-policy-objects"></a>Planear objetos de directiva de grupo

La configuración de DirectAccess se lleva a cabo al configurar el acceso remoto y se recopila en objetos de directiva de grupo (GPO). Se rellenan tres tipos de GPO diferentes con las opciones de configuración de DirectAccess, y se distribuyen como sigue:  
  
- **GPO de cliente de DirectAccess**: Este GPO contiene las opciones de configuración de clientes, incluida la configuración de las tecnologías de transición IPv6, las entradas de la tabla NRPT y las reglas de seguridad de Firewall de Windows con seguridad avanzada. El GPO se aplica a los grupos de seguridad especificados para los equipos cliente.  
  
- **Servidor de DirectAccess GPO**: Este GPO contiene las opciones de configuración de DirectAccess que se aplican a cualquier servidor configurado como servidor de acceso remoto en la implementación. Asimismo, contiene las reglas de seguridad de conexión del Firewall de Windows con seguridad avanzada.  
  
- **GPO de servidores de aplicación**: Este GPO contiene la configuración de los servidores de aplicación seleccionados a los que opcionalmente extiendes la autenticación y el cifrado de los clientes de DirectAccess. Si la autenticación y el cifrado no están extendidos, no se usa este GPO.  
  
El Asistente para habilitar DirectAccess crea automáticamente los GPO y se especifica un nombre predeterminado para cada GPO.  
  
> [!CAUTION]  
> Haz lo siguiente para realizar una copia de seguridad de todos los objetos de directiva de grupo de acceso remoto antes de ejecutar los cmdlets de DirectAccess: [Copia de seguridad y restaurar la configuración de acceso remoto](https://go.microsoft.com/fwlink/?LinkID=257928)  
  
Los GPO se pueden configurar de dos maneras:  
  
1. **Automáticamente**: Puedes especificar que se creen automáticamente. Se especifica un nombre predeterminado para cada GPO.  
  
2. **Manualmente**: Puedes usar los GPO que predefinió el administrador de Active Directory.  
  
Ten en cuenta que, después de configurar DirectAccess para que use unos GPO específicos, no se puede configurar para que use otros GPO.  
  
#### <a name="automatically-created-gpos"></a>GPO creados automáticamente

Ten en cuenta lo siguiente al usar GPO creados automáticamente:  
  
Los GPO creados automáticamente se aplican según los parámetros de ubicación y destino del vínculo, de la siguiente manera:  
  
- Para el GPO de servidor de DirectAccess, los parámetros de ubicación y de vínculo apuntan al dominio que contiene el servidor de acceso remoto.  
  
- Cuando se crean los GPO de cliente, la ubicación se establece para un solo dominio donde se crea el GPO. El nombre del GPO se busca en cada dominio y se rellena con opciones de configuración de DirectAccess (si existen). El destino del vínculo se establece en la raíz del dominio donde se creó el GPO. Se crea un GPO para cada dominio que contiene equipos cliente o servidores de aplicación, y el GPO se vincula a la raíz de su dominio respectivo.  
  
Cuando se usan GPO creados automáticamente para aplicar la configuración de DirectAccess, el administrador del servidor de acceso remoto requiere los siguientes permisos:  
  
- Permisos de creación de GPO para cada dominio.  
  
- Permisos de vinculación para todas las raíces de dominios de clientes seleccionadas.  
  
- Permisos de vinculación para todas las raíces de dominio de GPO de servidor.  
  
- Los GPO necesitan permisos de seguridad de creación, edición, eliminación y modificación.  
  
- Se recomienda que el administrador de acceso remoto tenga permisos de lectura en los GPO en todos los dominios necesarios. Esto permite que el acceso remoto compruebe que no haya GPO con nombres duplicados al crearlos.  
  
Ten en cuenta que si no existen los permisos correctos para vincular GPO, se mostrará una advertencia. La operación de acceso remoto continuará pero no se producirá la vinculación. Si aparece esta advertencia, los vínculos no se crearán automáticamente, incluso después de que se agreguen los permisos. En su lugar, el administrador deberá crear los vínculos manualmente.  
  
#### <a name="manually-created-gpos"></a>GPO creados manualmente

Ten en cuenta lo siguiente al usar GPO creados manualmente:  
  
- Los GPO deben existir antes de ejecutar el Asistente para instalación de acceso remoto.  
  
- Cuando se usan GPO creados manualmente, para aplicar la configuración de DirectAccess, el administrador de acceso remoto necesita permisos totales (seguridad de edición, eliminación y modificación) en los GPO creados manualmente.  
  
- Al usar GPO creados manualmente, se busca un vínculo al GPO en todo el dominio. Si el GPO no está vinculado en el dominio, se creará un vínculo automáticamente en la raíz del dominio. Si no están disponibles los permisos necesarios para crear el vínculo, se mostrará una advertencia.  
  
Ten en cuenta que si no existen los permisos correctos para vincular GPO, se mostrará una advertencia. La operación de acceso remoto continuará pero no se producirá la vinculación. Si aparece esta advertencia, los vínculos no se crearán automáticamente, incluso aunque se agreguen los permisos posteriormente. En su lugar, el administrador deberá crear los vínculos manualmente.  
  
#### <a name="recovering-from-a-deleted-gpo"></a>Recuperación de un GPO eliminado

Si un GPO de servidor de acceso directo, de cliente o de servidor de aplicaciones se elimina accidentalmente y no hay una copia de seguridad disponible, tienes que quitar la configuración y volver a configurarlo. Si hay una copia de seguridad disponible, puedes usarla para restaurar el GPO.  
  
**Administración de acceso remoto** mostrará el siguiente mensaje de error: **No se puede encontrar el GPO (nombre del GPO)** . Para quitar las opciones de configuración, sigue estos pasos:  
  
1. Ejecuta el cmdlet de PowerShell **Uninstall-remoteaccess**.  
  
2. Vuelva a abrir **administración de acceso remoto**.  
  
3. Verás un mensaje de error que indica que no se encontró el GPO. Haz clic en **Quitar opciones de configuración**. Cuando se complete la operación, el servidor se restaurará a un estado sin configurar.  

