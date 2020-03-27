---
title: Paso 1 planear la infraestructura de DirectAccess básica
description: Este tema forma parte de la guía de implementación de un único servidor de DirectAccess con el Asistente para Introducción para Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ''
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 9c71ef26f9e4ba5d20705827109d9ad22fe5c7ab
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80308894"
---
# <a name="step-1-plan-the-basic-directaccess-infrastructure"></a>Paso 1 planear la infraestructura de DirectAccess básica
El primer paso para una implementación básica de DirectAccess en un único servidor es planear la infraestructura necesaria para la implementación. En este tema se describen los pasos para la planificación de la infraestructura:  
  
|Tarea|Descripción|  
|----|--------|  
|Planear la topología de red y la configuración|Decide dónde colocar el servidor de DirectAccess \(en el perímetro, o detrás de una traducción de direcciones de red \(NAT\) dispositivo o firewall\)y planea el direccionamiento IP y el enrutamiento.|  
|Planear requisitos de firewall|Planea la configuración de los firewalls perimetrales para permitir el acceso de DirectAccess.|  
|Planear requisitos de certificados|DirectAccess puede usar Kerberos o certificados para la autenticación de cliente. Como puedes ver en esta implementación básica de DirectAccess, se configura automáticamente un proxy Kerberos y se autentica mediante credenciales de Active Directory.|  
|Planear los requisitos de DNS|Planifica la configuración DNS para el servidor de DirectAccess, los servidores de infraestructura y la conectividad de clientes.|  
|Planear Active Directory|Planea los requisitos de los controladores de dominio y de Active Directory.|  
|Planear objetos de directiva de grupo|Decide qué GPO se necesitan en tu organización y cómo crearlos o editarlos.|  
  
No es necesario completar las tareas de planificación en un orden específico.  
  
## <a name="plan-network-topology-and-settings"></a><a name="bkmk_1_1_Network_svr_top_settings"></a>Planear la topología de red y la configuración  
  
### <a name="plan-network-adapters-and-ip-addressing"></a>Planear los adaptadores de red y el direccionamiento IP  
  
1.  Identifica la topología de adaptadores de red que quieres usar. DirectAccess se puede configurar con una de las opciones siguientes:  
  
    -   Con dos adaptadores de red, ya sea en el perímetro con un adaptador de red conectado a Internet y el otro a la red interna, o detrás de un dispositivo NAT, firewall o enrutador, con un adaptador de red conectado a una red perimetral y el otro a la interna Storage.  
  
    -   Detrás de un dispositivo NAT con un adaptador de red: el servidor de DirectAccess se instala detrás de un dispositivo NAT y el único adaptador de red está conectado a la red interna.  
  
2.  Identifica tus requisitos de direccionamiento IP:  
  
    DirectAccess usa IPv6 con IPsec para crear una conexión segura entre los equipos cliente de DirectAccess y la red corporativa interna. Sin embargo, DirectAccess no requiere necesariamente conectividad con Internet IPv6 ni compatibilidad nativa con IPv6 en las redes internas. En su lugar, configura y usa automáticamente tecnologías de transición IPv6 para Tunelizar el tráfico IPv6 a través de Internet IPv4 \(6to4, Teredo, IP\-HTTPS\) y a través de la intranet\-solo IPv4 \(NAT64 o ISATAP\). Para obtener información general acerca de estas tecnologías de transición, consulta los siguientes recursos:  
  
    -   [Tecnologías de transición IPv6](https://technet.microsoft.com/library/bb726951.aspx)  
  
    -   [Especificación del Protocolo de túnel IP-HTTPS](https://msdn.microsoft.com/library/dd358571(PROT.10).aspx)  
  
3.  Configura los adaptadores y el direccionamiento obligatorios según la tabla siguiente. Para las implementaciones detrás de un dispositivo NAT con un solo adaptador de red, configure las direcciones IP solo con la columna **adaptador de red interno** .  
  
    ||Adaptador de red externo|Adaptador de red interno<sup>1</sup>|Requisitos de enrutamiento|  
    |-|--------------|--------------------|------------|  
    |Intranet IPv4 e Internet IPv4|Configure lo siguiente:<br /><br />-Una dirección IPv4 pública estática con la máscara de subred adecuada.<br />-Una dirección IPv4 de puerta de enlace predeterminada del firewall de Internet o del proveedor de servicios Internet local \(ISP\) enrutador.|Configure lo siguiente:<br /><br />-Una dirección de intranet IPv4 con la máscara de subred adecuada.<br />-Una conexión\-sufijo DNS específico del espacio de nombres de la intranet. También debes configurar un servidor DNS en la interfaz interna.<br />-No configure una puerta de enlace predeterminada en ninguna interfaz de la intranet.|Para configurar el servidor de DirectAccess de manera que tenga acceso a todas las subredes de la red IPv4 interna, haz lo siguiente:<br /><br />1. Enumere los espacios de direcciones IPv4 para todas las ubicaciones de la intranet.<br />2. Use los comandos **Route add \-p** o **netsh interface IPv4 Add Route** para agregar los espacios de direcciones IPv4 como rutas estáticas en la tabla de enrutamiento IPv4 del servidor de DirectAccess.|  
    |Internet IPv6 e intranet IPv6|Configure lo siguiente:<br /><br />-Use la configuración de la dirección configurada de forma automática proporcionada por su ISP.<br />-Use el comando **Route Print** para asegurarse de que existe una ruta IPv6 predeterminada que apunta al enrutador de ISP en la tabla de enrutamiento de IPv6.<br />: Determine si los enrutadores de ISP e Intranet están usando las preferencias de enrutador predeterminadas descritas en RFC 4191 y el uso de una preferencia predeterminada mayor que los enrutadores de la Intranet local. Si ambas condiciones se cumplen, no se necesita ninguna otra configuración para la ruta predeterminada. La preferencia mayor para el enrutador del ISP asegura que la ruta IPv6 predeterminada activa del servidor de DirectAccess señala a Internet por IPv6.<br /><br />Como el servidor de DirectAccess es un enrutador IPv6, si tienes una infraestructura IPv6 nativa, la interfaz de Internet también puede tener acceso a los controladores de dominio de la intranet. En este caso, agregue filtros de paquetes al controlador de dominio de la red perimetral que impidan la conectividad a la dirección IPv6 de Internet\-interfaz orientada al servidor de DirectAccess.|Configure lo siguiente:<br /><br />-Si no utiliza los niveles de preferencia predeterminados, configure las interfaces de la intranet con el comando **netsh interface ipv6 set InterfaceIndex ignoredefaultroutes\=habilitado** . Este comando garantiza que las rutas predeterminadas adicionales que señalen a enrutadores de la intranet no se agregarán a la tabla de enrutamiento IPv6. Puede obtener el índiceInterfaz de las interfaces de la intranet a partir de lo que muestra el comando netsh interface show interface.|Si la intranet es IPv6, haz lo siguiente para configurar el servidor de DirectAccess para que tenga acceso a todas las ubicaciones IPv6:<br /><br />1. Enumere los espacios de direcciones IPv6 para todas las ubicaciones de la intranet.<br />2. Use el comando **netsh interface ipv6 add Route** para agregar los espacios de direcciones IPv6 como rutas estáticas en la tabla de enrutamiento IPv6 del servidor de DirectAccess.|  
    |Internet IPv4 e intranet IPv6|El servidor de DirectAccess reenvía el tráfico de la ruta IPv6 predeterminada mediante la interfaz del adaptador 6to4 de Microsoft con una retransmisión 6to4 en Internet IPv4. Puede configurar un servidor de DirectAccess para la dirección IPv4 de la retransmisión 6to4 de Microsoft en la red Internet de IPv4 \(usar cuando IPv6 nativo no está implementado en la red corporativa\) con el siguiente comando: netsh interface ipv6 6to4 Set Relay Name\=192.88.99.1 State State\=Enabled Command.|||  
  
    > [!NOTE]  
    > Observe lo siguiente:  
    >   
    > 1.  Si se ha asignado una dirección IPv4 pública al cliente de DirectAccess, este usará la tecnología de transición 6to4 para conectarse a la intranet. Si el cliente de DirectAccess no puede conectarse al servidor de DirectAccess con 6to4, usará IP\-HTTPS.  
    > 2.  Los equipos cliente IPv6 nativos pueden conectar con el servidor de DirectAccess a través de IPv6 nativo, y no se necesita ninguna tecnología de transición.  
  
### <a name="plan-firewall-requirements"></a><a name="ConfigFirewalls"></a>Planear los requisitos del firewall  
Si el servidor de DirectAccess se encuentra detrás de un firewall perimetral, se requerirán las excepciones siguientes para el tráfico de DirectAccess cuando el servidor de DirectAccess se encuentre en Internet IPv4:  
  
-   tráfico 6to4: protocolo IP 41 entrante y saliente.  
  
-   IP\-HTTPS: Protocolo de control de transmisión \(TCP\) puerto de destino 443 y puerto de origen TCP 443 de salida.  
  
-   Si implementas DirectAccess con un solo adaptador de red e instalas el servidor de ubicación de red en el servidor de DirectAccess, también deberás agregar el puerto TCP 62000 a las excepciones.  
  
    > [!NOTE]  
    > Esta exención se realiza en el servidor de DirectAccess. El resto de excepciones se realizan en el firewall perimetral.  
  
Se necesitarán las siguientes excepciones para el tráfico de DirectAccess cuando el servidor de DirectAccess se encuentre en Internet IPv6:  
  
-   Protocolo IP 50  
  
-   Puerto de destino UDP 500 de entrada y puerto de origen UDP 500 de salida.  
  
Si usas firewalls adicionales, aplica las siguientes excepciones de firewall de la red interna para el tráfico de DirectAccess:  
  
-   ISATAP-protocolo 41 entrante y saliente  
  
-   TCP\/UDP para todo el tráfico IPv4\/IPv6  
  
### <a name="plan-certificate-requirements"></a><a name="bkmk_1_2_CAs_and_certs"></a>Planear los requisitos de certificado  
Los certificados para IPsec necesitan un certificado de equipo usado por equipos cliente de DirectAccess al establecer la conexión IPsec entre el cliente y el servidor de DirectAccess y, además, un certificado de equipo usado por los servidores de DirectAccess para establecer conexiones IPsec con clientes de DirectAccess. Para DirectAccess en Windows Server 2012 R2 y Windows Server 2012, no es obligatorio usar estos certificados IPsec. El Asistente para introducción configura el servidor de DirectAccess para que actúe como proxy Kerberos a fin de realizar autenticación IPSec sin requerir certificados.
  
1.  **Servidor IP\-https**. Al configurar DirectAccess, el servidor de DirectAccess se configura automáticamente para que actúe como el agente de escucha web HTTPS\-IP. El sitio IP\-HTTPS requiere un certificado de sitio web y los equipos cliente deben poder ponerse en contacto con la lista de revocación de certificados \(sitio\) CRL para el certificado. El Asistente para habilitar DirectAccess intenta usar el certificado SSTP de VPN. Si SSTP no está configurado, comprueba si hay un certificado para IP\-HTTPS en el almacén personal de la máquina. Si no hay ninguno disponible, se crea automáticamente un certificado auto\-firmado.
  
2.  **Servidor de ubicación de red**. El servidor de ubicación de red es un sitio web que se usa para detectar si los equipos cliente se encuentran en la red corporativa. El servidor de ubicación de red requiere un certificado de sitio Web. Los clientes de DirectAccess tienen que poder contactar con el sitio de la CRL para obtener el certificado. El Asistente para habilitar acceso remoto comprueba si existe un certificado para el servidor de ubicación de red en el almacén personal del equipo. Si no está presente, crea automáticamente un certificado auto\-firmado.
  
En la tabla siguiente encontrarás un resumen de los requisitos de certificación para cada uno de estos:  
  
|Autenticación IPsec|Servidor IP\-HTTPS|Servidor de ubicación de red|  
|------------|----------|--------------|  
|Se requiere una entidad de certificación interna para emitir certificados de equipo al servidor y a los clientes de DirectAccess para la autenticación IPsec cuando no se usa el proxy Kerberos para la autenticación.|CA pública: se recomienda usar una entidad de certificación pública para emitir el certificado de IP\-HTTPS, lo que garantiza que el punto de distribución de CRL esté disponible externamente.|CA interna: puede usar una CA interna para emitir el certificado de sitio web del servidor de ubicación de red. Asegúrate de que el punto de distribución de CRL tenga alta disponibilidad desde la red interna.|  
||CA interna: puede usar una CA interna para emitir el certificado IP\-HTTPS; sin embargo, debe asegurarse de que el punto de distribución de CRL está disponible externamente.|Certificado auto\-firmado: puede usar un certificado auto\-firmado para el sitio web del servidor de ubicación de red; sin embargo, no puede usar un certificado auto\-firmado en implementaciones multisitio.|  
||Certificado auto\-firmado: puede usar un certificado auto\-firmado para el servidor IP\-HTTPS; sin embargo, debe asegurarse de que el punto de distribución de CRL está disponible externamente. No se puede usar un certificado firmado por\-en una implementación multisitio.||  
  
#### <a name="plan-certificates-for-ip-https-and-network-location-server"></a><a name="bkmk_website_cert_IPHTTPS"></a>Planear certificados para IP\-HTTPS y el servidor de ubicación de red  
Si quieres aprovisionar un certificado para estos objetivos, consulta [Implementar un único servidor de DirectAccess con configuración avanzada](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md). Si no hay ningún certificado disponible, el Asistente para Introducción crea automáticamente certificados Auto\-firmados para estos propósitos.
  
> [!NOTE]
> Si aprovisiona certificados para IP\-HTTPS y el servidor de ubicación de red manualmente, asegúrese de que los certificados tengan un nombre de sujeto. Si el certificado no tiene un nombre de sujeto, sino que tiene un nombre alternativo, el Asistente para DirectAccess no lo aceptará.  
  
#### <a name="plan-dns-requirements"></a>Planear los requisitos de DNS  
En las implementaciones de DirectAccess se requiere DNS para lo siguiente:  
  
-   **Solicitudes de cliente de DirectAccess**. DNS se usa para resolver las solicitudes de equipos cliente de DirectAccess que no están ubicados en la red interna. Los clientes de DirectAccess intentan conectarse al servidor de ubicación de red de DirectAccess para determinar si están ubicados en Internet o en la red corporativa: Si la conexión es correcta, los clientes se determinan en la intranet y No se usa DirectAccess y las solicitudes de cliente se resuelven mediante el servidor DNS configurado en el adaptador de red del equipo cliente. Si se producen errores de conexión, se da por hecho que los clientes están en Internet. Los clientes de DirectAccess usarán la tabla de directivas de resolución de nombres \(NRPT\) para determinar qué servidor DNS se usará para resolver las solicitudes de nombres. Puedes especificar que los clientes tengan que usar DNS64 de DirectAccess para resolver nombres, o bien un servidor DNS interno alternativo. Para llevar a cabo la resolución de nombres, los clientes de DirectAccess usan la tabla NRPT para identificar cómo gestionar una solicitud. Los clientes solicitan un FQDN o un nombre de etiqueta de\-único, como http:\/\/interno. Si un único nombre de etiqueta de\-es requests, se anexa un sufijo DNS para crear un FQDN. Si la consulta DNS coincide con una entrada de la NRPT y se especifica DNS4 o un servidor DNS de intranet para la entrada, la consulta para la resolución de nombres se enviará mediante el servidor especificado. Si existe una coincidencia pero no se especifica ningún servidor DNS, esto indica una regla de exención y se aplicará la resolución de nombres normal.  
  
    Al agregar un nuevo sufijo a la NRPT en la consola de administración de DirectAccess, podrás detectar automáticamente los servidores DNS predeterminados para el sufijo si haces clic en el botón **Detectar**. La detección automática funciona de la siguiente manera:  
  
    1.  Si la red corporativa es IPv4\-, o IPv4 e IPv6, la dirección predeterminada es la dirección DNS64 del adaptador interno en el servidor de DirectAccess.  
  
    2.  Si la red corporativa está basada en IPv6\-, la dirección predeterminada es la dirección IPv6 de los servidores DNS de la red corporativa.  
  
-   **Servidores de infraestructura**
  
    1.  **Servidor de ubicación de red**. Los clientes de DirectAccess intentan contactar con el servidor de ubicación de red para determinar si están en la red interna. Los clientes de la red interna deben ser capaces de resolver el nombre del servidor de ubicación de red, pero debe evitarse que resuelvan el nombre si se encuentran en Internet. Para que esto suceda, de manera predeterminada, el FQDN del servidor de ubicación de red se agregará como una regla de exención a la NRPT. Además, al configurar DirectAccess, se crean automáticamente las reglas siguientes:  
  
        1.  Una regla de sufijo DNS para el dominio raíz o para el nombre de dominio del servidor de DirectAccess y las direcciones IPv6 correspondientes a los servidores DNS de la intranet configurados en el servidor de DirectAccess. Por ejemplo, si el servidor de DirectAccess pertenece al dominio corp.contoso.com, se creará una regla para el sufijo DNS corp.contoso.com.  
  
        2.  Una regla de exención para el FQDN del servidor de ubicación de red. Por ejemplo, si la dirección URL del servidor de ubicación de red es https:\/\/nls.corp.contoso.com, se crea una regla de exención para el FQDN nls.corp.contoso.com.  
  
        **Servidor IP\-https**. El servidor de DirectAccess actúa como un agente de escucha de IP\-HTTPS y usa su certificado de servidor para autenticarse en los clientes IP\-HTTPS. Los clientes de DirectAccess que usan servidores DNS públicos deben poder resolver el nombre HTTPS del\-IP.  
  
        **Comprobadores de la conectividad**. DirectAccess crea un sondeo web predeterminado que usan los equipos cliente de DirectAccess para comprobar la conectividad a la red interna. Para comprobar si el sondeo funciona correctamente es necesario registrar de forma manual los nombres siguientes en DNS:  
  
        1.  DirectAccess\-webprobehost: debe resolverse en la dirección IPv4 interna del servidor de DirectAccess o en la dirección IPv6 en un entorno de solo\-IPv6.  
  
        2.  corpconnectivityhost de DirectAccess\-: debe resolverse en localhost \(dirección de\) de bucle invertido. Es necesario crear un registro A y un registro AAAA: el registro A con el valor 127.0.0.1 y el registro AAAA con el valor compuesto a partir del prefijo NAT64 con los últimos 32 bits como 127.0.0.1. El prefijo NAT64 se puede recuperar ejecutando el cmdlet Get\-netnattransitionconfiguration.  
  
        Se pueden crear comprobadores de conectividad adicionales con otras direcciones web a través de HTTP o PING. Debe existir una entrada DNS por cada comprobador de conectividad.  
  
#### <a name="dns-server-requirements"></a>Requisitos del servidor DNS  
  
-   En el caso de los clientes de DirectAccess, debe usar un servidor DNS que ejecute Windows Server 2008, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016 o cualquier servidor DNS que admita IPv6.  
  
> [!NOTE]  
> No se recomienda que utilices los servidores DNS que ejecutan Windows Server 2003 al implementar DirectAccess. Aunque los servidores DNS de Windows Server 2003 admiten registros IPv6, Windows Server 2003 ya no es compatible con Microsoft. Además, no deberías implementar DirectAccess si los controladores de dominio ejecutan Windows Server 2003 debido a un problema con el servicio de replicación de archivos. Para obtener más información, consulte [configuraciones no admitidas de DirectAccess](../DirectAccess-Unsupported-Configurations.md).  
  
### <a name="plan-the-network-location-server"></a><a name="bkmk_1_4_NLS"></a>Planear el servidor de ubicación de red  
El servidor de ubicación de red es un sitio web que se usa para detectar si los clientes de DirectAccess están ubicados en la red corporativa. Los clientes de la red corporativa no usan DirectAccess para acceder a los recursos internos, sino que se conectan directamente.  
  
El Asistente para introducción configura automáticamente el servidor de ubicación de red en el servidor de DirectAccess, y el sitio web se crea automáticamente al implementar DirectAccess. Esto permite una instalación sencilla, sin que sea necesario usar una infraestructura de certificados.
  
Si desea implementar un servidor de ubicación de red y no usar certificados\-firmados, consulte [implementar un único servidor de DirectAccess con configuración avanzada](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).
  
### <a name="plan-active-directory"></a><a name="bkmk_1_6_AD"></a>Active Directory del plan  
DirectAccess usa Active Directory y Active Directory objetos de directiva de grupo de la siguiente manera:
  
-   **Autenticación**. Active Directory se usa para la autenticación. El túnel de DirectAccess usa la autenticación Kerberos para que el usuario pueda acceder a recursos internos.
  
-   **Objetos de directiva de grupo**. DirectAccess recopila opciones de configuración de los objetos de directiva de grupo que se aplican a servidores y a clientes de DirectAccess.
  
-   **Grupos de seguridad**. DirectAccess usa grupos de seguridad para recopilar e identificar equipos cliente de DirectAccess y servidores de DirectAccess. Las directivas de grupo se aplican en el grupo de seguridad correspondiente.

**Requisitos de Active Directory**  
  
Al planificar Active Directory para una implementación de DirectAccess, se necesita lo siguiente:  
  
-   Al menos un controlador de dominio instalado en Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008. 
  
    Si el controlador de dominio está en una red perimetral \(y, por tanto, se puede tener acceso a él desde el adaptador de red\-orientado a Internet del servidor de DirectAccess\) impedir que el servidor de DirectAccess lo alcance agregando filtros de paquetes en el controlador de dominio para evitar la conectividad con la dirección IP del adaptador de Internet.  
  
-   El servidor de DirectAccess debe ser miembro del dominio.  
  
-   Los clientes de DirectAccess deben ser miembros del dominio. Los clientes pueden pertenecer a:  
  
    -   Cualquier dominio del mismo bosque que el servidor de DirectAccess.  
  
    -   Cualquier dominio que tenga dos\-forma de confiar en el dominio del servidor de DirectAccess.  
  
    -   Cualquier dominio de un bosque que tenga dos\-forma de confianza con el bosque al que pertenece el dominio de DirectAccess.  
  
> [!NOTE]
> - El servidor de DirectAccess no puede ser un controlador de dominio.  
> - El controlador de dominio de Active Directory que se usa para DirectAccess no debe ser accesible desde el adaptador de Internet externo del servidor de DirectAccess \(el adaptador no debe estar en el perfil de dominio del firewall de Windows\).  
  
### <a name="plan-group-policy-objects"></a><a name="bkmk_1_7_GPOs"></a>Planear objetos directiva de grupo  
La configuración de DirectAccess configurada al configurar DirectAccess se recopila en objetos de directiva de grupo \(GPO\). Se rellenan dos GPO distintos con opciones de DirectAccess, y se distribuyen así:  
  
-   **GPO de cliente de DirectAccess**. Este GPO contiene las opciones de configuración de clientes, incluida la configuración de las tecnologías de transición IPv6, las entradas de la tabla NRPT y las reglas de seguridad de Firewall de Windows con seguridad avanzada. El GPO se aplica a los grupos de seguridad especificados para los equipos cliente.  
  
-   **GPO de servidor de DirectAccess**. Este GPO contiene las opciones de configuración de DirectAccess que se aplican a cualquier servidor configurado como servidor de DirectAccess en tu implementación. Asimismo, contiene las reglas de seguridad de conexión del Firewall de Windows con seguridad avanzada.  
  
Los GPO se pueden configurar de dos maneras:  
  
1.  **Automáticamente**. Puedes especificar que se creen automáticamente. Se especifica un nombre predeterminado para cada GPO. El Asistente para introducción creará los GPO automáticamente.
  
2.  **Manualmente**. Puedes usar los GPO que predefinió el administrador de Active Directory.
  
Ten en cuenta que, después de configurar DirectAccess para que use unos GPO específicos, no se puede configurar para que use otros GPO.
  
> [!IMPORTANT]
> Independientemente de si usas GPO configurados automáticamente o manualmente, tendrás que agregar una directiva para la detección de vínculos de baja velocidad si tus clientes van a usar 3G. La directiva de grupo ruta de acceso de la **Directiva: configurar directiva de grupo detección de vínculos de baja velocidad** es: **configuración del equipo \/ directivas \/ plantillas administrativas \/ sistema**\/ Directiva de grupo.  
  
> [!CAUTION]  
> Use el procedimiento siguiente para realizar una copia de seguridad de todos los objetos de directiva de grupo de DirectAccess antes de ejecutar los cmdlets de DirectAccess: [copia de seguridad y restauración de la configuración de DirectAccess](https://go.microsoft.com/fwlink/?LinkID=257928)  
  
#### <a name="automatically-created-gpos"></a>GPO creados automáticamente\-  
Tenga en cuenta lo siguiente al usar automáticamente los GPO creados\-:  
  
Los GPO creados automáticamente se aplican según los parámetros de ubicación y destino del vínculo, de la siguiente manera:  
  
-   Para el GPO del servidor de DirectAccess, los parámetros de ubicación y de vínculo apuntan al dominio que contiene el servidor de DirectAccess.  
  
-   Cuando se crean los GPO de cliente, la ubicación se establece para un solo dominio donde se crea el GPO. El nombre del GPO se busca en cada dominio y se rellena con opciones de configuración de DirectAccess (si existen). El destino del vínculo se establece en la raíz del dominio donde se creó el GPO. Se crea un GPO para cada dominio que contenga equipos cliente, y el GPO se vincula a la raíz del dominio correspondiente.  
  
Al usar GPO creados automáticamente, el administrador del servidor de DirectAccess necesitará los permisos siguientes para aplicar la configuración de DirectAccess:  
  
-   Permisos de creación de GPO para cada dominio.  
  
-   Permisos de vinculación para todas las raíces de dominios de clientes seleccionadas.  
  
-   Permisos de vinculación para todas las raíces de dominio de GPO de servidor.  
  
-   Los GPO necesitan permisos de seguridad de creación, edición, eliminación y modificación.  
  
-   Se recomienda que el administrador de DirectAccess tenga permisos de lectura de GPO para cada dominio requerido. Esto permite que DirectAccess pueda comprobar que no existan GPO con nombres duplicados al crearlos.  
  
Ten en cuenta que si no existen los permisos correctos para vincular GPO, se mostrará una advertencia. La operación de DirectAccess continuará, pero no se producirá la vinculación. Si se muestra esta advertencia, los vínculos no se crearán automáticamente, incluso aunque se agreguen los permisos posteriormente. En su lugar, el administrador deberá crear los vínculos manualmente.  
  
#### <a name="manually-created-gpos"></a>GPO creados manualmente\-  
Tenga en cuenta lo siguiente al usar los GPO creados manualmente\-:  
  
-   Los GPO deben existir antes de ejecutar el Asistente para introducción de acceso remoto.  
  
-   Al usar manualmente los GPO creados\-, para aplicar la configuración de DirectAccess, el administrador de DirectAccess necesita permisos de GPO completos \(editar, eliminar, modificar\) de seguridad en los GPO creados manualmente\-.  
  
-   Al usar GPO creados manualmente, se busca un vínculo al GPO en todo el dominio. Si el GPO no está vinculado en el dominio, se creará un vínculo automáticamente en la raíz del dominio. Si no están disponibles los permisos necesarios para crear el vínculo, se mostrará una advertencia.  
  
Ten en cuenta que si no existen los permisos correctos para vincular GPO, se mostrará una advertencia. La operación de DirectAccess continuará, pero no se producirá la vinculación. Si aparece esta advertencia, los vínculos no se crearán automáticamente, incluso aunque se agreguen los permisos posteriormente. En su lugar, el administrador deberá crear los vínculos manualmente.  
  
#### <a name="recovering-from-a-deleted-gpo"></a>Recuperación de un GPO eliminado  
Si un servidor de DirectAccess, un cliente o un GPO de servidor de aplicaciones se ha eliminado por accidente y no hay ninguna copia de seguridad disponible, debe quitar los valores de configuración y volver a\-configurar. Si hay una copia de seguridad disponible, puedes usarla para restaurar el GPO.  
  
La **Administración de DirectAccess** mostrará el siguiente mensaje de error: no se **puede encontrar el GPO <GPO name>** . Para quitar las opciones de configuración, sigue estos pasos:  
  
1.  Ejecute el cmdlet de PowerShell **uninstall\-RemoteAccess**.  
  
2.  Vuelva\-abrir **Administración de DirectAccess**.  
  
3.  Verás un mensaje de error que indica que no se encontró el GPO. Haz clic en **Quitar opciones de configuración**. Una vez finalizado, el servidor se restaurará a un estado\-configurado.  
  
### <a name="next-step"></a><a name="BKMK_Links"></a>Paso siguiente  
  
-   [Paso 2: planear la implementación básica de DirectAccess](da-basic-plan-s2-deployment.md)  
  
