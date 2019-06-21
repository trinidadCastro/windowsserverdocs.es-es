---
title: Paso 1 Plan la infraestructura de DirectAccess básica
description: Este tema forma parte de la Guía de implementar un único servidor de DirectAccess mediante el Asistente de iniciado para Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d518be944ef57d1f26d1bdab8984155863c98630
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283407"
---
# <a name="step-1-plan-the-basic-directaccess-infrastructure"></a>Paso 1 Plan la infraestructura de DirectAccess básica
El primer paso para una implementación básica de DirectAccess en un solo servidor es planear la infraestructura necesaria para la implementación. En este tema se describen los pasos para la planificación de la infraestructura:  
  
|Tarea|Descripción|  
|----|--------|  
|Planear la topología de red y la configuración|Decidir dónde colocar el servidor de DirectAccess \(en el borde o detrás de una traducción de direcciones de red \(NAT\) firewall o un dispositivo\)y planee el direccionamiento IP y enrutamiento.|  
|Planear requisitos de firewall|Planea la configuración de los firewalls perimetrales para permitir el acceso de DirectAccess.|  
|Planear requisitos de certificados|DirectAccess puede usar Kerberos o certificados para la autenticación de cliente. Como puedes ver en esta implementación básica de DirectAccess, se configura automáticamente un proxy Kerberos y se autentica mediante credenciales de Active Directory.|  
|Planear los requisitos de DNS|Planifica la configuración DNS para el servidor de DirectAccess, los servidores de infraestructura y la conectividad de clientes.|  
|Planear Active Directory|Planea los requisitos de los controladores de dominio y de Active Directory.|  
|Planear objetos de directiva de grupo|Decide qué GPO se necesitan en tu organización y cómo crearlos o editarlos.|  
  
No es necesario completar las tareas de planificación en un orden específico.  
  
## <a name="bkmk_1_1_Network_svr_top_settings"></a>Planear la configuración y topología de red  
  
### <a name="plan-network-adapters-and-ip-addressing"></a>Planear los adaptadores de red y el direccionamiento IP  
  
1.  Identifica la topología de adaptadores de red que quieres usar. DirectAccess se puede configurar con una de las opciones siguientes:  
  
    -   Con dos adaptadores de red: ya sea en el perímetro con un adaptador de red conectado a Internet y el otro a la red interna o detrás de NAT, firewall o un dispositivo enrutador, con un adaptador de red conectado a una red perimetral y el otro para interno red.  
  
    -   Detrás de un dispositivo NAT con un adaptador de red: el servidor de DirectAccess se instala detrás de un dispositivo NAT y el único adaptador de red está conectado a la red interna.  
  
2.  Identifica tus requisitos de direccionamiento IP:  
  
    DirectAccess usa IPv6 con IPsec para crear una conexión segura entre los equipos cliente de DirectAccess y la red corporativa interna. Sin embargo, DirectAccess no requiere necesariamente conectividad con Internet IPv6 ni compatibilidad nativa con IPv6 en las redes internas. En su lugar, configura automáticamente y usa tecnologías de transición IPv6 para tunelizar el tráfico IPv6 a través de Internet IPv4 \(6to4, Teredo, IP\-HTTPS\) y a través de su IPv4\-solo intranet \( NAT64 o ISATAP\). Para obtener información general acerca de estas tecnologías de transición, consulta los siguientes recursos:  
  
    -   [Tecnologías de transición IPv6](https://technet.microsoft.com/library/bb726951.aspx)  
  
    -   [Especificación del protocolo de túnel IP-HTTPS](https://msdn.microsoft.com/library/dd358571(PROT.10).aspx)  
  
3.  Configura los adaptadores y el direccionamiento obligatorios según la tabla siguiente. Para implementaciones detrás de un dispositivo NAT con un único adaptador de red, configurar las direcciones IP solo mediante la **adaptador de red interno** columna.  
  
    ||Adaptador de red externo|Adaptador de red interno<sup>1</sup>|Requisitos de enrutamiento|  
    |-|--------------|--------------------|------------|  
    |Intranet IPv4 e Internet IPv4|Configura lo siguiente:<br /><br />-Una dirección IPv4 pública estática con la máscara de subred adecuada.<br />-Puerta de enlace predeterminada dirección IPv4 de su firewall de Internet o un proveedor de servicios de Internet local \(ISP\) enrutador.|Configura lo siguiente:<br /><br />-Una dirección de intranet IPv4 con la máscara de subred adecuada.<br />-Una conexión\-sufijo DNS específico de su espacio de nombres de intranet. También debes configurar un servidor DNS en la interfaz interna.<br />-No configure una puerta de enlace predeterminada en las interfaces de la intranet.|Para configurar el servidor de DirectAccess de manera que tenga acceso a todas las subredes de la red IPv4 interna, haz lo siguiente:<br /><br />1.  Enumere los espacios de direcciones IPv4 para todas las ubicaciones de la intranet.<br />2.  Use la **Agregar ruta \-p** o **netsh interface ipv4 Agregar ruta** espacios de direcciones de comandos para agregar el IPv4 como rutas estáticas en la tabla de enrutamiento IPv4 del servidor de DirectAccess.|  
    |Internet IPv6 e intranet IPv6|Configura lo siguiente:<br /><br />-Use la configuración de dirección configurada automáticamente proporcionada por su ISP.<br />-Use el **enrutar impresión** comando para asegurarse de que existe una ruta de IPv6 predeterminada que apunta al enrutador del ISP en la tabla de enrutamiento de IPv6.<br />-Determine si los enrutadores del ISP y de intranet están usando preferencias del enrutador predeterminadas descritas en RFC 4191 y usando una preferencia predeterminada mayor que los enrutadores de la intranet local. Si ambas condiciones se cumplen, no se necesita ninguna otra configuración para la ruta predeterminada. La preferencia mayor para el enrutador del ISP asegura que la ruta IPv6 predeterminada activa del servidor de DirectAccess señala a Internet por IPv6.<br /><br />Como el servidor de DirectAccess es un enrutador IPv6, si tienes una infraestructura IPv6 nativa, la interfaz de Internet también puede tener acceso a los controladores de dominio de la intranet. En este caso, agregar filtros de paquetes al controlador de dominio en la red perimetral que impidan que la dirección IPv6 de Internet\-orientado a la interfaz del servidor de DirectAccess.|Configura lo siguiente:<br /><br />-Si no usas los niveles de preferencia de forma predeterminada, configura las interfaces de intranet con el **netsh interface ipv6 establecer InterfaceIndex ignoredefaultroutes\=habilitado** comando. Este comando garantiza que las rutas predeterminadas adicionales que señalen a enrutadores de la intranet no se agregarán a la tabla de enrutamiento IPv6. Para conocer el índice de las interfaces de la intranet, usa el comando “netsh interface show interface”.|Si la intranet es IPv6, haz lo siguiente para configurar el servidor de DirectAccess para que tenga acceso a todas las ubicaciones IPv6:<br /><br />1.  Enumere los espacios de direcciones IPv6 para todas las ubicaciones de la intranet.<br />2.  Usa el comando **netsh interface ipv6 add route** para agregar los espacios de direcciones IPv6 como rutas estáticas a la tabla de enrutamiento IPv6 del servidor de DirectAccess.|  
    |Internet IPv4 e intranet IPv6|El servidor de DirectAccess reenvía el tráfico de la ruta IPv6 predeterminada mediante la interfaz del adaptador 6to4 de Microsoft con una retransmisión 6to4 en Internet IPv4. Puede configurar un servidor de DirectAccess para la dirección IPv4 de la retransmisión 6to4 de Microsoft en Internet por IPv4 \(usa cuando no se implementa IPv6 nativa en la red corporativa\) con el comando siguiente: netsh interfaz ipv6 6to4 set relay nombre\=192.88.99.1 estado\=habilita el comando.|||  
  
    > [!NOTE]  
    > Tenga en cuenta lo siguiente:  
    >   
    > 1.  Si se ha asignado una dirección IPv4 pública al cliente de DirectAccess, este usará la tecnología de transición 6to4 para conectarse a la intranet. Si el cliente de DirectAccess no puede conectarse al servidor de DirectAccess con 6to4, usará IP\-HTTPS.  
    > 2.  Los equipos cliente IPv6 nativos pueden conectar con el servidor de DirectAccess a través de IPv6 nativo, y no se necesita ninguna tecnología de transición.  
  
### <a name="ConfigFirewalls"></a>Planear los requisitos de firewall  
Si el servidor de DirectAccess se encuentra detrás de un firewall perimetral, se requerirán las excepciones siguientes para el tráfico de DirectAccess cuando el servidor de DirectAccess se encuentre en Internet IPv4:  
  
-   tráfico 6to4: protocolo IP 41 entrante y saliente.  
  
-   IP\-HTTPS Transmission Control Protocol \(TCP\) el puerto 443 de destino y puerto de origen TCP 443 de salida.  
  
-   Si implementas DirectAccess con un solo adaptador de red e instalas el servidor de ubicación de red en el servidor de DirectAccess, también deberás agregar el puerto TCP 62000 a las excepciones.  
  
    > [!NOTE]  
    > Esta exención se realiza en el servidor de DirectAccess. El resto de excepciones se realizan en el firewall perimetral.  
  
Se necesitarán las siguientes excepciones para el tráfico de DirectAccess cuando el servidor de DirectAccess se encuentre en Internet IPv6:  
  
-   Protocolo IP 50  
  
-   Puerto de destino UDP 500 de entrada y puerto de origen UDP 500 de salida.  
  
Si usas firewalls adicionales, aplica las siguientes excepciones de firewall de la red interna para el tráfico de DirectAccess:  
  
-   ISATAP: protocolo 41 de entrada y salida  
  
-   TCP\/UDP para IPv4 todas\/tráfico IPv6  
  
### <a name="bkmk_1_2_CAs_and_certs"></a>Planear los requisitos de certificado  
Los certificados para IPsec necesitan un certificado de equipo usado por equipos cliente de DirectAccess al establecer la conexión IPsec entre el cliente y el servidor de DirectAccess y, además, un certificado de equipo usado por los servidores de DirectAccess para establecer conexiones IPsec con clientes de DirectAccess. Para DirectAccess en Windows Server 2012 R2 y Windows Server 2012, no es obligatorio el uso de estos certificados IPsec. El Asistente para introducción configura el servidor de DirectAccess para que actúe como proxy Kerberos a fin de realizar autenticación IPSec sin requerir certificados.
  
1.  **IP\-servidor HTTPS**. Al configurar DirectAccess, el servidor de DirectAccess se configura automáticamente para que actúe como la dirección IP\-agente de escucha HTTPS. La dirección IP\-sitio HTTPS requiere un certificado de sitio Web y los equipos cliente deben ser capaz de ponerse en contacto con la lista de revocación de certificados \(CRL\) sitio para el certificado. El Asistente para habilitar DirectAccess intenta usar el certificado SSTP de VPN. Si SSTP no está configurado, comprueba si un certificado para IP\-HTTPS está presente en el almacén personal del equipo. Si ninguno está disponible, crea automáticamente un autoservicio\-certificado autofirmado.
  
2.  **Servidor de ubicación de red**. El servidor de ubicación de red es un sitio Web usado para detectar si los equipos cliente están ubicados en la red corporativa. El servidor de ubicación de red requiere un certificado de sitio web. Los clientes de DirectAccess tienen que poder contactar con el sitio de la CRL para obtener el certificado. El Asistente para habilitar el acceso remoto comprueba si hay un certificado de servidor de ubicación de red en el almacén personal del equipo. Si no está presente, crea automáticamente un autoservicio\-certificado autofirmado.
  
En la tabla siguiente encontrarás un resumen de los requisitos de certificación para cada uno de estos:  
  
|Autenticación IPsec|IP\-servidor HTTPS|Servidor de ubicación de red|  
|------------|----------|--------------|  
|Una CA interna es necesaria para emitir certificados de equipo en el servidor de DirectAccess y los clientes para la autenticación IPsec si no se usa el proxy Kerberos para la autenticación|Entidad de certificación pública: se recomienda usar una CA pública para emitir la dirección IP\-certificado HTTPS, esto garantiza que el punto de distribución CRL esté disponible externamente.|CA interna: puede usar una CA interna para emitir el certificado de sitio Web del servidor de ubicación de red. Asegúrate de que el punto de distribución de CRL tenga alta disponibilidad desde la red interna.|  
||CA interna: puede usar una CA interna para emitir la dirección IP\-certificado HTTPS; sin embargo, debe asegurarse de que el punto de distribución CRL esté disponible externamente.|Self\-firmó el certificado: puede usar un autoservicio\-certificado autofirmado para el sitio Web servidor de ubicación de red; sin embargo, no se puede usar un autoservicio\-firmó el certificado en implementaciones multisitio.|  
||Self\-firmó el certificado: puede usar un autoservicio\-certificado autofirmado para la dirección IP\-servidor HTTPS; sin embargo, debe asegurarse de que el punto de distribución CRL esté disponible externamente. Un autoservicio\-certificado firmado no se puede usar en una implementación multisitio.||  
  
#### <a name="bkmk_website_cert_IPHTTPS"></a>Planear certificados para IP\-servidor de ubicación de red y HTTPS  
Si quieres aprovisionar un certificado para estos objetivos, consulta [Implementar un único servidor de DirectAccess con configuración avanzada](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md). Si hay disponible ningún certificado, el Asistente para introducción crea automáticamente self\-certificados autofirmados para estos fines.
  
> [!NOTE]
> Si se aprovisiona certificados para IP\-HTTPS y el servidor de ubicación de red manualmente, aseguran de que los certificados tienen un nombre de sujeto. Si el certificado no tiene un nombre de sujeto, sino que tiene un nombre alternativo, el Asistente para DirectAccess no lo aceptará.  
  
#### <a name="plan-dns-requirements"></a>Planear los requisitos de DNS  
En las implementaciones de DirectAccess se requiere DNS para lo siguiente:  
  
-   **Solicitudes de cliente de DirectAccess**. DNS se usa para resolver las solicitudes de equipos cliente de DirectAccess que no están ubicados en la red interna. Los clientes de DirectAccess intentan conectarse al servidor de ubicación de red de DirectAccess para determinar si están ubicados en Internet o en la red corporativa: Si la conexión es correcta, se determinará que los clientes están en la intranet, por lo que no se usará DirectAccess y las solicitudes de clientes se resolverán mediante el servidor DNS configurado en el adaptador de red del equipo cliente. Si se producen errores de conexión, se da por hecho que los clientes están en Internet. Los clientes de DirectAccess usarán la tabla de directivas de resolución de nombres \(NRPT\) para determinar qué servidor DNS que se usará al resolver solicitudes de nombres. Puedes especificar que los clientes tengan que usar DNS64 de DirectAccess para resolver nombres, o bien un servidor DNS interno alternativo. Para llevar a cabo la resolución de nombres, los clientes de DirectAccess usan la tabla NRPT para identificar cómo gestionar una solicitud. Los clientes solicitan un FQDN o una sola\-nombre de etiqueta como http:\/\/interno. Si una sola\-nombre de etiqueta es de solicitudes, se anexa un sufijo DNS para crear un FQDN. Si la consulta DNS coincide con una entrada de la NRPT y se especifica DNS4 o un servidor DNS de intranet para la entrada, la consulta para la resolución de nombres se enviará mediante el servidor especificado. Si existe una coincidencia pero no se especifica ningún servidor DNS, esto indica una regla de exención y se aplicará la resolución de nombres normal.  
  
    Al agregar un nuevo sufijo a la NRPT en la consola de administración de DirectAccess, podrás detectar automáticamente los servidores DNS predeterminados para el sufijo si haces clic en el botón **Detectar**. La detección automática funciona de la siguiente manera:  
  
    1.  Si la red corporativa es IPv4\-en función, o IPv4 e IPv6, la dirección predeterminada es la dirección DNS64 del adaptador interno en el servidor de DirectAccess.  
  
    2.  Si la red corporativa es IPv6\-basado, la dirección predeterminada es la dirección IPv6 de servidores DNS de la red corporativa.  
  
-   **Servidores de infraestructura**
  
    1.  **Servidor de ubicación de red**. Los clientes de DirectAccess intentan contactar con el servidor de ubicación de red para determinar si están en la red interna. Los clientes de la red interna deben ser capaces de resolver el nombre del servidor de ubicación de red, pero debe evitarse que resuelvan el nombre si se encuentran en Internet. Para que esto suceda, de manera predeterminada, el FQDN del servidor de ubicación de red se agregará como una regla de exención a la NRPT. Además, al configurar DirectAccess, se crean automáticamente las reglas siguientes:  
  
        1.  Una regla de sufijo DNS para el dominio raíz o para el nombre de dominio del servidor de DirectAccess y las direcciones IPv6 correspondientes a los servidores DNS de la intranet configurados en el servidor de DirectAccess. Por ejemplo, si el servidor de DirectAccess pertenece al dominio corp.contoso.com, se creará una regla para el sufijo DNS corp.contoso.com.  
  
        2.  Una regla de exención para el FQDN del servidor de ubicación de red. Por ejemplo, si la URL del servidor de ubicación de red es https:\/\/nls.corp.contoso.com, se crea una regla de exención para el FQDN nls.corp.contoso.com.  
  
        **IP\-servidor HTTPS**. El servidor de DirectAccess actúa como una dirección IP\-escucha HTTPS y usa su servidor de certificados para autenticar a IP\-los clientes HTTPS. La dirección IP\-nombre HTTPS debe ser capaz de resolver los clientes de DirectAccess que usan servidores DNS públicos.  
  
        **Comprobadores de la conectividad**. DirectAccess crea un sondeo web predeterminado que usan los equipos cliente de DirectAccess para comprobar la conectividad a la red interna. Para comprobar si el sondeo funciona correctamente es necesario registrar de forma manual los nombres siguientes en DNS:  
  
        1.  DirectAccess\-webprobehost: debe proporcionar la dirección IPv4 interna del servidor de DirectAccess, o a la dirección IPv6 en IPv6\-solo entorno.  
  
        2.  DirectAccess\-corpconnectivityhost: debe resolver en localhost \(bucle invertido\) dirección. Es necesario crear un registro A y un registro AAAA: el registro A con el valor 127.0.0.1 y el registro AAAA con el valor compuesto a partir del prefijo NAT64 con los últimos 32 bits como 127.0.0.1. El prefijo NAT64 se puede recuperar ejecutando el cmdlet get\-netnattransitionconfiguration.  
  
        Se pueden crear comprobadores de conectividad adicionales con otras direcciones web a través de HTTP o PING. Debe existir una entrada DNS por cada comprobador de conectividad.  
  
#### <a name="dns-server-requirements"></a>Requisitos del servidor DNS  
  
-   Para los clientes de DirectAccess, debe usar un servidor DNS que se está ejecutando Windows Server 2008, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016 o cualquier servidor DNS que admita IPv6.  
  
> [!NOTE]  
> No se recomienda que utilices los servidores DNS que ejecutan Windows Server 2003 al implementar DirectAccess. Aunque los servidores DNS de Windows Server 2003 admiten registros IPv6, Windows Server 2003 ya no es compatible con Microsoft. Además, no deberías implementar DirectAccess si los controladores de dominio ejecutan Windows Server 2003 debido a un problema con el servicio de replicación de archivos. Para obtener más información, consulte [configuraciones no admitidas de DirectAccess](../DirectAccess-Unsupported-Configurations.md).  
  
### <a name="bkmk_1_4_NLS"></a>Planear el servidor de ubicación de red  
El servidor de ubicación de red es un sitio web que se usa para detectar si los clientes de DirectAccess están ubicados en la red corporativa. Los clientes de la red corporativa no usan DirectAccess para acceder a los recursos internos, sino que se conectan directamente.  
  
El Asistente para introducción configura automáticamente el servidor de ubicación de red en el servidor de DirectAccess, y el sitio web se crea automáticamente al implementar DirectAccess. Esto permite una instalación sencilla, sin que sea necesario usar una infraestructura de certificados.
  
Si desea implementar un servidor de ubicación de red y no usar self\-certificados autofirmados, consulte [implementar un único servidor de DirectAccess con configuración avanzada](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).
  
### <a name="bkmk_1_6_AD"></a>Planear Active Directory  
DirectAccess usa Active Directory y objetos de directiva de grupo de Active Directory como sigue:
  
-   **Autenticación**. Active Directory se usa para la autenticación. El túnel de DirectAccess usa la autenticación Kerberos para que el usuario pueda acceder a recursos internos.
  
-   **Objetos de directiva de grupo**. DirectAccess recopila opciones de configuración de los objetos de directiva de grupo que se aplican a servidores y a clientes de DirectAccess.
  
-   **Grupos de seguridad**. DirectAccess usa grupos de seguridad para recopilar e identificar equipos cliente de DirectAccess y servidores de DirectAccess. Las directivas de grupo se aplican en el grupo de seguridad correspondiente.

**Requisitos de Active Directory**  
  
Al planificar Active Directory para una implementación de DirectAccess, se necesita lo siguiente:  
  
-   Al menos un controlador de dominio instalado en Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008. 
  
    Si el controlador de dominio se encuentra en una red perimetral \(y, por tanto, es accesible desde Internet\-accesible desde el adaptador de red del servidor de DirectAccess\) impedir que se acceda a él; agregando filtros de paquetes en el servidor de DirectAccess el controlador de dominio para impedir la conectividad a la dirección IP del adaptador de Internet.  
  
-   El servidor de DirectAccess debe ser miembro del dominio.  
  
-   Los clientes de DirectAccess deben ser miembros del dominio. Los clientes pueden pertenecer a:  
  
    -   Cualquier dominio del mismo bosque que el servidor de DirectAccess.  
  
    -   Cualquier dominio que tenga una\-confianza bidireccional con el dominio del servidor de DirectAccess.  
  
    -   Cualquier dominio en un bosque que tenga una\-confianza bidireccional con el bosque al que pertenece el dominio de DirectAccess.  
  
> [!NOTE]
> - El servidor de DirectAccess no puede ser un controlador de dominio.  
> - El controlador de dominio de Active Directory usado para DirectAccess no debe ser accesible desde el adaptador de Internet externo del servidor de DirectAccess \(el adaptador no debe estar en el perfil de dominio de Windows Firewall\).  
  
### <a name="bkmk_1_7_GPOs"></a>Objetos de directiva de grupo de planes  
Configuración de DirectAccess seleccionadas al configurar DirectAccess se recopila en objetos de directiva de grupo \(GPO\). Se rellenan dos GPO distintos con opciones de DirectAccess, y se distribuyen así:  
  
-   **GPO de cliente de DirectAccess**. Este GPO contiene las opciones de configuración de clientes, incluida la configuración de las tecnologías de transición IPv6, las entradas de la tabla NRPT y las reglas de seguridad de Firewall de Windows con seguridad avanzada. El GPO se aplica a los grupos de seguridad especificados para los equipos cliente.  
  
-   **GPO de servidor de DirectAccess**. Este GPO contiene las opciones de configuración de DirectAccess que se aplican a cualquier servidor configurado como servidor de DirectAccess en tu implementación. Asimismo, contiene las reglas de seguridad de conexión del Firewall de Windows con seguridad avanzada.  
  
Los GPO se pueden configurar de dos maneras:  
  
1.  **Automáticamente**. Puedes especificar que se creen automáticamente. Se especifica un nombre predeterminado para cada GPO. El Asistente para introducción creará los GPO automáticamente.
  
2.  **Manualmente**. Puedes usar los GPO que predefinió el administrador de Active Directory.
  
Ten en cuenta que, después de configurar DirectAccess para que use unos GPO específicos, no se puede configurar para que use otros GPO.
  
> [!IMPORTANT]
> Independientemente de si usas GPO configurados automáticamente o manualmente, tendrás que agregar una directiva para la detección de vínculos de baja velocidad si tus clientes van a usar 3G. La ruta de acceso de la directiva de grupo para **directiva: Configurar la detección de vínculo de baja velocidad de directiva de grupo** es: **Configuración del equipo \/ políticas \/ plantillas administrativas \/ sistema \/ directiva de grupo**.  
  
> [!CAUTION]  
> Haz lo siguiente para realizar una copia de seguridad de todos los objetos de directiva de grupo de DirectAccess antes de ejecutar los cmdlets de DirectAccess: [Copia de seguridad y restaurar la configuración de DirectAccess](https://go.microsoft.com/fwlink/?LinkID=257928)  
  
#### <a name="automatically-created-gpos"></a>Automáticamente\-GPO creados  
Tenga en cuenta lo siguiente cuando se usa automáticamente\-GPO creados:  
  
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
  
#### <a name="manually-created-gpos"></a>Manualmente\-GPO creados  
Tenga en cuenta lo siguiente cuando se usa de forma manual\-GPO creados:  
  
-   Los GPO deben existir antes de ejecutar el Asistente para introducción de acceso remoto.  
  
-   Cuando se usa de forma manual\-GPO creados, para aplicar la configuración de DirectAccess el cliente de DirectAccess administrador necesita permisos totales \(editar, eliminar, modificar seguridad\) en el manual\-GPO creados.  
  
-   Al usar GPO creados manualmente, se busca un vínculo al GPO en todo el dominio. Si el GPO no está vinculado en el dominio, se creará un vínculo automáticamente en la raíz del dominio. Si no están disponibles los permisos necesarios para crear el vínculo, se mostrará una advertencia.  
  
Ten en cuenta que si no existen los permisos correctos para vincular GPO, se mostrará una advertencia. La operación de DirectAccess continuará, pero no se producirá la vinculación. Si aparece esta advertencia, los vínculos no se crearán automáticamente, incluso aunque se agreguen los permisos posteriormente. En su lugar, el administrador deberá crear los vínculos manualmente.  
  
#### <a name="recovering-from-a-deleted-gpo"></a>Recuperación de un GPO eliminado  
Si un servidor de DirectAccess, cliente o servidor de aplicaciones de GPO se ha eliminado por accidente y no hay disponible ninguna copia de seguridad, debe quitar la configuración y re\-volver a configurar. Si hay una copia de seguridad disponible, puedes usarla para restaurar el GPO.  
  
Se mostrará el siguiente mensaje de error en **Administración de DirectAccess**: **GPO <GPO name> no se encuentra**. Para quitar las opciones de configuración, sigue estos pasos:  
  
1.  Ejecute el cmdlet de PowerShell **desinstalar\-remoteaccess**.  
  
2.  Re\-abrir **administración de DirectAccess**.  
  
3.  Verás un mensaje de error que indica que no se encontró el GPO. Haz clic en **Quitar opciones de configuración**. Cuando haya finalizado, el servidor se restaurará a una anulación\-configurado estado.  
  
### <a name="BKMK_Links"></a>Paso siguiente  
  
-   [Paso 2: Planear la implementación básica de DirectAccess](da-basic-plan-s2-deployment.md)  
  
