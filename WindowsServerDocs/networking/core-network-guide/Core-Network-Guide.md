---
title: Componentes de la red principal
description: Esta guía proporciona instrucciones sobre cómo planear e implementar los componentes principales necesarios para una red plenamente funcional y un nuevo dominio de Active Directory en un nuevo bosque con Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: b3cd60f7-d380-4712-9a78-0a8f551e1121
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ef764356c5f74eb0aff15753e7f83a020c68c091
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446534"
---
# <a name="core-network-components"></a>Componentes de la red principal

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Esta guía proporciona instrucciones sobre cómo planear e implementar los componentes principales necesarios para una red plenamente funcional y un nuevo dominio de Active Directory en un bosque nuevo.

> [!NOTE]
> Esta guía está disponible para su descarga en formato Microsoft Word desde la Galería de TechNet. Para obtener más información, consulte [Guía de red de Core para Windows Server 2016](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683).

En esta guía se incluyen las siguientes secciones.

- [Acerca de esta guía](#BKMK_about)

- [Introducción a la red Core](#BKMK_overview)

- [Planeamiento de las redes principales](#BKMK_planning)

- [Implementación de red principal](#BKMK_deployment)

- [Recursos técnicos adicionales](#BKMK_resources)

- [Apéndices A E](#BKMK_appendix)

## <a name="BKMK_about"></a>Acerca de esta guía
Esta guía está diseñada para administradores de sistema y de red que están instalando una nueva red o que desean crear una red basada en dominios para reemplazar una red compuesta por grupos de trabajo. El escenario de implementación que se proporciona en esta guía es particularmente útil si prevé la necesidad de agregar más servicios y características a la red en el futuro.

Se recomienda que revise las guías de diseño e implementación de todas las tecnologías que se usan en este escenario de implementación para ayudarle a determinar si esta guía proporciona los servicios y la configuración que necesita.

Una red principal es una colección de hardware, dispositivos y software de red que proporciona los servicios fundamentales para satisfacer las necesidades de las tecnologías de la información (TI) de la organización.

Una red principal de Windows Server le ofrece muchas ventajas, entre las que se incluyen las siguientes.

-   Protocolos principales para la conexión de red entre equipos y otros dispositivos compatibles con Protocolo de control de transmisión/Protocolo de Internet (TCP/IP). TCP/IP es un conjunto de protocolos estándar pensado para conectar equipos y crear redes. TCP/IP es un software de protocolo de red proporcionado con los sistemas operativos de Microsoft Windows que implementa y admite el conjunto de protocolos TCP/IP.

-   Asignación automática de dirección IP de Protocolo de configuración dinámica de host (DHCP) a equipos y otros dispositivos que están configurados como clientes DHCP. La configuración manual de direcciones IP en todos los equipos de la red es una tarea que consume mucho tiempo y es menos flexible que la opción de proporcionar dinámicamente a equipos y otros dispositivos configuraciones de direcciones IP usando un servidor DHCP.

-   Servicio de resolución de nombres del Sistema de nombres de dominio (DNS). Con DNS, los usuarios, equipos, aplicaciones y servicios pueden usar el nombre de dominio completo de un equipo o un dispositivo para encontrar la dirección IP de dicho equipo o dispositivo.

-   Un bosque, que es uno o más dominios de Active Directory que comparten las mismas definiciones de clase y atributo (esquema), información de sitio y replicación (configuración) y capacidades de búsqueda para todo el bosque (catálogo global).

-   Un dominio raíz del bosque, que es el primer dominio creado en un nuevo bosque. Los grupos Administradores de empresas y Administradores de esquema, que son grupos administrativos para todo el bosque, se encuentran en el dominio raíz del bosque. Además, un dominio raíz del bosque, como los demás dominios, es una colección de objetos de equipo, usuario y grupo definidos por el administrador en Servicios de dominio de Active Directory (AD DS). Estos objetos comparten una base de datos de directorios común y directivas de seguridad. También comparten relaciones de seguridad con otros dominios, si se agregan dominios a medida que la organización crece. El servicio de directorio también almacena datos de directorio y permite que los equipos, aplicaciones y usuarios autorizados tengan acceso a los datos.

-   Una base de datos de cuentas de usuario y equipo. El servicio de directorio proporciona una base de datos de cuentas de usuario centralizada que le permite crear cuentas de usuario y equipo para las personas y equipos que están autorizados para conectarse a la red y tener acceso a recursos de red, como aplicaciones, bases de datos, carpetas y archivos compartidos e impresoras.

Una red principal también le permite escalar la red a medida que crece la organización y cambian los requisitos de TI. Por ejemplo, con una red principal puede agregar dominios, subredes IP, servicios de acceso remoto, servicios inalámbricos y otras características y roles de servidor proporcionados por Windows Server 2016.

### <a name="network-hardware-requirements"></a>Requisitos de hardware de red
Para implementar correctamente una red principal, debe implementar el hardware de red, incluido lo siguiente:

-   Cableado Ethernet, Fast Ethernet o Gigabyte Ethernet

-   Un concentrador, conmutador de nivel 2 o 3, enrutador u otro dispositivo que realice la función de retransmitir tráfico de red entre equipos y dispositivos.

-   Equipos que satisfagan los requisitos mínimos de hardware para sus respectivos sistemas operativos de cliente y servidor.

## <a name="what-this-guide-does-not-provide"></a>Qué no incluye esta guía
Esta guía no proporciona instrucciones para implementar lo siguiente:

-   Hardware de red, como cableado, enrutadores, conmutadores y concentradores

-   Recursos de red adicionales, como impresoras y servidores de archivos

-   Conectividad a Internet

-   Acceso remoto

-   Acceso inalámbrico

-   Implementación de equipos cliente

> [!NOTE]
> Los equipos que ejecutan sistemas operativos de cliente de Windows se configuran de forma predeterminada para recibir concesiones de direcciones IP del servidor DHCP. Por lo tanto, no es necesaria ninguna configuración adicional de DHCP o del protocolo de Internet versión 4 (IPv4) de los equipos cliente.

## <a name="technology-overviews"></a>Introducción a las tecnologías
En las siguientes secciones, se ofrece información general sobre las tecnologías necesarias que se implementan para crear una red principal.

### <a name="active-directory-domain-services"></a>Active Directory Domain Services
Un directorio es una estructura jerárquica que almacena información sobre objetos en la red, como usuarios y equipos. Un servicio de directorio, como AD DS, proporciona los métodos para almacenar datos de directorio y hacer que estos datos estén disponibles para los administradores y usuarios de la red. Por ejemplo, AD DS almacena información acerca de las cuentas de usuario, incluidos los nombres, direcciones de correo electrónico, contraseñas y números de teléfono y permite que otros usuarios autorizados en la misma red tener acceso a esta información.

### <a name="dns"></a>DNS
DNS es un protocolo de resolución de nombres para redes TCP/IP, como Internet o una red de organización. Los servidores DNS hospedan la información que permite a los equipos cliente y a los servicios resolver nombres DNS alfanuméricos fácilmente reconocibles en las direcciones IP que usan los equipos para comunicarse entre ellos.

### <a name="dhcp"></a>DHCP
DHCP es un estándar IP que sirve para simplificar la administración de la configuración IP del host. El estándar DHCP ofrece el uso de servidores DHCP como una forma de administrar la asignación dinámica de direcciones IP y demás detalles de configuración relacionados para los clientes habilitados para DHCP de la red.

DHCP le permite usar un servidor DHCP para asignar dinámicamente una dirección IP a un equipo o a otro dispositivo (como una impresora) de la red local. Cada equipo de una red TCP/IP debe tener una dirección IP única, ya que la dirección IP y su máscara de subred relacionada identifican el equipo host y la subred a la cual está conectado el equipo. Si usa DHCP, puede estar seguro de que todos los equipos que están configurados como clientes DHCP reciben una dirección IP que sea apropiada para la ubicación de red y la subred; además, al usar las opciones de DHCP, como puertas de enlace y servidores DNS predeterminados, puede proporcionar de forma automática a los clientes de DHCP la información que necesitan para funcionar correctamente en la red.

En redes basadas en TCP/IP, DHCP reduce la complejidad y la cantidad de trabajo administrativo necesario para la reconfiguración de los equipos.

### <a name="tcpip"></a>TCP/IP
TCP/IP en Windows Server 2016 es la siguiente:

-   Software de red basado en protocolos de red estándar del sector.

-   Un protocolo de red corporativa enrutable que admite la conexión del equipo basado en Windows en entornos de red de área local (LAN) y de red de área extensa (WAN).

-   Tecnologías y utilidades principales para la conexión del equipo basado en Windows con sistemas distintos con el objetivo de compartir información.

-   Una base para obtener acceso a servicios globales de Internet, como World Wide Web y servidores FTP (Protocolo de transferencia de archivos).

-   Un marco cliente/servidor entre plataformas escalable y sólido.

TCP/IP proporciona utilidades de TCP/IP básicas que permiten a los equipos basados en Windows conectarse y compartir información con otros sistemas de Microsoft y sistemas que no son de Microsoft, incluidos:

-    Windows Server 2016

-   Windows 10

-    Windows Server 2012 R2

-   Windows 8.1

-    Windows Server 2012

-   Windows 8

-    Windows Server 2008 R2

-    Windows 7

-    Windows Server 2008

-   Windows Vista

-   Hosts de Internet

-   Sistemas Apple Macintosh

-   Grandes sistemas (mainframes) IBM

-   Sistemas UNIX y Linux

-   Sistemas OpenVMS

-   Impresoras de red

-   Tabletas y teléfonos celulares con Ethernet con cable o tecnología de 802.11 inalámbrica habilitada

## <a name="BKMK_overview"></a>Introducción a la red Core
En la siguiente ilustración se muestra la topología de una red principal de Windows Server.

![Topología de red de Windows Server Core](../media/Core-Network-Guide/cng16_overview.jpg)

> [!NOTE]
> En esta guía también se incluyen instrucciones para agregar a la topología de red servidores opcionales Servidor de directivas de redes (NPS) y Servidor web (IIS) a fin de conformar la base para soluciones de acceso a red seguro, como implementaciones cableadas e inalámbricas 802.1X que se pueden implementar usando las guías complementarias de red principal. Para obtener más información, vea [Implementación de características opcionales para la autenticación de acceso a redes y servicios web](#BKMK_optionalfeatures).

### <a name="core-network-components"></a>Componentes de la red principal
A continuación se detallan los componentes de una red principal.

##### <a name="router"></a>Enrutador
En esta guía de implementación se proporcionan instrucciones para implementar una red principal con dos subredes separadas por un enrutador con reenvío de DHCP habilitado. Sin embargo, se puede implementar un conmutador de nivel 2 o nivel 3, o un concentrador, según los requisitos y recursos de los que se disponga. Si se implementa un conmutador, deberá tener la capacidad de reenviar DHCP; de lo contrario, se deberá colocar un servidor DHCP en cada subred. Si implementa un concentrador, estará implementando una sola subred, por lo que no será necesario habilitar el reenvío de DHCP o colocar otro ámbito en el servidor DHCP.

##### <a name="static-tcpip-configurations"></a>Configuración de TCP/IP estática
Los servidores de esta implementación se configuran con direcciones IPv4 estáticas. Los equipos cliente se configuran de manera predeterminada para que reciban concesiones de direcciones IP procedentes del servidor DHCP.

##### <a name="active-directory-domain-services-global-catalog-and-dns-server-dc1"></a>Catálogo global de Active Directory Domain Services y servidor DNS DC1
Tanto los servicios de dominio de Active Directory (AD DS) y del sistema de nombres de dominio (DNS) están instalados en este servidor, llamado DC1, que proporciona el directorio de nombres y servicios de resolución para todos los equipos y dispositivos de la red.

##### <a name="dhcp-server-dhcp1"></a>Servidor DHCP DHCP1
El servidor DHCP, llamado DHCP1, se configura con un ámbito que proporciona concesiones de direcciones del protocolo de Internet (IP) a los equipos de la subred local. Si se configura el reenvío de DHCP en los enrutadores, también se puede configurar el servidor DHCP con otros ámbitos para que provea concesiones de direcciones IP a los equipos de otras subredes.

##### <a name="client-computers"></a>Equipos cliente
Los equipos que ejecutan sistemas operativos de cliente de Windows se configuran de forma predeterminada como clientes DHCP, que obtienen direcciones IP y las opciones de DHCP de forma automática desde el servidor DHCP.

## <a name="BKMK_planning"></a>Planeamiento de las redes principales
Antes de implementar una red principal, es necesario planear los siguientes elementos.

-   [Planear subredes](#bkmk_NetFndtn_Pln_Subnt)

-   [Planear la configuración básica de todos los servidores](#bkmk_NetFndtn_Pln_AllSrvrs)

-   [Planear la implementación de DC1](#bkmk_NetFndtn_Pln_AD-DNS-01)

-   [Planeación de acceso al dominio](#bkmk_NetFndtn_Pln_DomAccess)

-   [Planear la implementación de DHCP1](#bkmk_NetFndtn_Pln_DHCP-01)

Las siguientes secciones contienen información más detallada sobre cada uno de estos elementos.

> [!NOTE]
> Para obtener ayuda con la planificación de la implementación, consulte también [Apéndice E - hoja de preparación de planeación de red de Core](#BKMK_E).

### <a name="bkmk_NetFndtn_Pln_Subnt"></a>Planear subredes
En una red con protocolo TCP/IP, los enrutadores sirven para interconectar el hardware y software usados en los distintos segmentos de la red física llamados subredes. Los enrutadores también se usan para reenviar paquetes IP entre cada una de las subredes. Averigüe cuál es el diseño físico de la red, incluido el número de enrutadores y subredes necesario, antes de continuar con las instrucciones de esta guía.

Además, para configurar los servidores en la red con direcciones IP estáticas, debe determinar el intervalo de direcciones IP que desea usar para la subred en la que están ubicados los servidores principales de la red. En esta guía, la dirección IP privada intervalos 10.0.0.1 - 10.0.0.254 y 10.0.1.1 - 10.0.1.254 se utilizan como ejemplos, pero puede usar cualquier intervalo de direcciones IP privadas que prefiera.

> [!IMPORTANT]
> Después de seleccionar los intervalos de direcciones IP que desea usar para cada subred, asegúrese de configurar los enrutadores con una dirección IP del mismo intervalo de direcciones IP que se usó en la subred donde está instalado el enrutador. Por ejemplo, si el enrutador está configurado de forma predeterminada con la dirección IP 192.168.1.1, pero está instalando el enrutador en una subred con un intervalo de direcciones IP de 10.0.0.0/24, debe volver a configurar el enrutador para usar una dirección IP dentro del intervalo de direcciones IP 10.0.0.0/24.

Los siguientes intervalos de direcciones IP privadas aparecen especificados en la solicitud de comentarios (RFC) de Internet 1918:

-   10.0.0.0 - 10.255.255.255

-   172.16.0.0 - 172.31.255.255

-   192.168.0.0 - 192.168.255.255

Cuando se usan los intervalos de direcciones IP privadas especificados en la RFC 1918, no se puede conectar directamente a Internet mediante una dirección IP privada, ya que los enrutadores del proveedor de acceso a Internet (ISP) descartan automáticamente las solicitudes cuya procedencia o destino es una de esas direcciones. Para agregar más adelante conectividad a Internet a la red principal, deberá contratarla con un ISP para obtener una dirección IP pública.

> [!IMPORTANT]
> Cuando se usan direcciones IP privadas, debe usar algún tipo de proxy o servidor de traducción de direcciones de red (NAT) para convertir los intervalos de direcciones IP privadas de la red local en una dirección IP pública que se pueda enrutar en Internet. La mayoría de los enrutadores proporciona servicios NAT, de modo que seleccionar un enrutador apto para NAT debe ser bastante simple.

Para obtener más información, vea la sección [Planear la implementación de DHCP1](#bkmk_NetFndtn_Pln_DHCP-01).

### <a name="bkmk_NetFndtn_Pln_AllSrvrs"></a>Planear la configuración básica de todos los servidores
Para cada servidor de la red principal, debe cambiar el nombre del equipo y asignar y configurar una dirección IPv4 estática y otras propiedades TCP/IP para el equipo.

#### <a name="planning-naming-conventions-for-computers-and-devices"></a>Planear las convenciones de nomenclatura para equipos y dispositivos
Para mantener la coherencia en toda la red, es una buena idea usar nombres coherentes para servidores, impresoras y demás dispositivos. Los nombres de los equipos se pueden usar para ayudar a los usuarios y administradores a identificar con facilidad el propósito y la ubicación del servidor, impresora u otro dispositivo. Por ejemplo, si tiene tres servidores DNS, uno en San Francisco, uno de Los Ángeles y uno en Chicago, podría utilizar la convención de nomenclatura *función de servidor*-*ubicación* - *número*:

-   DNS-DEN-01. Este nombre representa el servidor DNS en Denver, Colorado. Si se agregan más servidores DNS en Denver, se puede incrementar el valor numérico del nombre, como en DNS-DEN-02 y DNS-DEN-03.

-   DNS-SPAS-01. Este nombre representa el servidor DNS en South Pasadena, California.

-   DNS-ORL-01. Este nombre representa el servidor DNS en Orlando, Florida.

En esta guía, la convención de nomenclatura de servidores es muy simple y consiste en la función del servidor principal y un número. Por ejemplo, el controlador de dominio se denomina DC1 y el servidor DHCP, DHCP1.

Se recomienda elegir una convención de nomenclatura antes de instalar la red principal con esta guía.

#### <a name="planning-static-ip-addresses"></a>Planear direcciones IP estáticas
Antes de configurar cada equipo con una dirección IP estática, debe planear las subredes y los intervalos de direcciones IP. Además, debe determinar las direcciones IP de los servidores DNS. Si planea instalar un enrutador que proporcione acceso a otras redes, como subredes adicionales o Internet, debe conocer la dirección IP del enrutador, también llamado puerta de enlace predeterminada, para la configuración de direcciones IP estáticas.

La siguiente tabla proporciona valores de ejemplo para la configuración de direcciones IP estáticas.

|Elementos de configuración|Valores de ejemplo|
|-----------------------|------------------|
|Dirección IP|10.0.0.2|
|Máscara de subred|255.255.255.0|
|Puerta de enlace predeterminada (dirección IP del enrutador)|10.0.0.1|
|Servidor DNS preferido|10.0.0.2|

> [!NOTE]
> Si piensa implementar más de un servidor DNS, también puede planear la dirección IP del servidor DNS alternativo.

### <a name="bkmk_NetFndtn_Pln_AD-DNS-01"></a>Planear la implementación de DC1
Estos son los pasos de planeación claves antes de instalar los servicios de dominio de Active Directory (AD DS) y DNS en DC1.

#### <a name="planning-the-name-of-the-forest-root-domain"></a>Planear el nombre del dominio raíz del bosque
Un primer paso en el proceso de diseño de AD DS es determinar cuántos bosques requiere la organización. Un bosque es el contenedor de AD DS de nivel superior y consta de uno o varios dominios que comparten un esquema común y un catálogo global. Una organización puede tener varios bosques, pero en la mayoría de las organizaciones el diseño con un único bosque es el modelo preferido y el más sencillo de administrar.

Cuando se crea el primer controlador de dominio en la organización, se está creando el primer dominio (también llamado dominio raíz del bosque) y el primer bosque. Sin embargo, antes de realizar esta acción mediante esta guía, debe determinar cuál es el mejor nombre de dominio para la organización. En la mayoría de los casos, se usa el nombre de la organización como nombre del dominio y, muchas veces, dicho nombre de dominio está registrado. Si piensa implementar servidores web basados en Internet orientados externamente para proporcionar información y servicios a sus clientes o socios, elija un nombre de dominio que aún no esté en uso y después regístrelo para que sea propiedad de su organización.

#### <a name="planning-the-forest-functional-level"></a>Planear el nivel funcional del bosque
Durante la instalación de AD DS, debe elegir el nivel funcional del bosque que desea usar. Funcionalidad de dominios y bosques, introducida en Windows Server 2003 Active Directory proporciona una forma de habilitar las características de Active Directory dentro de su entorno de red de dominios o bosques completos. Dependiendo del entorno, hay disponibles varios niveles de funcionalidad del dominio y del bosque.

La funcionalidad de bosque habilita características en todos los dominios del bosque. Existen los siguientes niveles funcionales de bosque disponibles:

-    Windows Server 2008 . Este nivel funcional del bosque es compatible con controladores de dominio que ejecutan Windows Server 2008 y versiones posteriores del sistema operativo Windows Server.

-    Windows Server 2008 R2 . Este nivel funcional del bosque es compatible con controladores de dominio de Windows Server 2008 R2 y controladores de dominio que ejecutan versiones posteriores del sistema operativo Windows Server.

-    Windows Server 2012 . Este nivel funcional del bosque es compatible con controladores de dominio de Windows Server 2012 y controladores de dominio que ejecutan versiones posteriores del sistema operativo Windows Server.

-    Windows Server 2012 R2 . Este nivel funcional del bosque es compatible con controladores de dominio de Windows Server 2012 R2 y controladores de dominio que ejecutan versiones posteriores del sistema operativo Windows Server.

-    Windows Server 2016. Este nivel funcional del bosque solo admite controladores de dominio de Windows Server 2016 y controladores de dominio que ejecutan versiones posteriores del sistema operativo Windows Server.

Si está implementando un nuevo dominio en un bosque nuevo y todos los controladores de dominio van a ejecutar Windows Server 2016, se recomienda configurar AD DS con el nivel funcional del bosque de Windows Server 2016 durante la instalación de AD DS.

> [!IMPORTANT]
> Tras elevar el nivel funcional del bosque, los controladores de dominio que ejecutan sistemas operativos anteriores no podrán incorporarse al bosque. Por ejemplo, si eleva el nivel funcional del bosque a Windows Server 2016, no se puede agregar al bosque controladores de dominio que ejecutan Windows Server 2012 R2 o Windows Server 2008.

Se proporcionan elementos de configuración de ejemplo para AD DS en la tabla siguiente.

|Elementos de configuración:|Valores de ejemplo:|
|------------------------|-------------------|
|Nombre DNS completo|Ejemplos:<br /><br />-   corp.contoso.com<br />-   example.com|
|Nivel funcional del bosque|-    Windows Server 2008 <br />-    Windows Server 2008 R2 <br />-    Windows Server 2012 <br />-    Windows Server 2012 R2 <br />-    Windows Server 2016|
|Ubicación de la carpeta de bases de datos de Active Directory Domain Services|E:\Configuration\\<br /><br />O bien, acepte la ubicación predeterminada.|
|Ubicación de la carpeta de archivos de registro de Active Directory Domain Services|E:\Configuration\\<br /><br />O bien, acepte la ubicación predeterminada.|
|Ubicación de la carpeta SYSVOL de Active Directory Domain Services|E:\Configuration\\<br /><br />O bien, acepte la ubicación predeterminada.|
|Contraseña de administrador para el modo de restauración de directorios|**J\*p2leO4$F**|
|Nombre del archivo de respuesta (opcional)|**AD DS_AnswerFile**|

#### <a name="planning-dns-zones"></a>Planear zonas DNS
En los servidores DNS principales integrados en Active Directory, se crea una zona de búsqueda directa de forma predeterminada durante la instalación del rol Servidor DNS. Una zona de búsqueda directa permite a los equipos y dispositivos consultar la dirección IP de otro equipo o dispositivo a partir de su nombre DNS. Además de una zona de búsqueda directa, se recomienda crear una zona de búsqueda DNS inversa. Con una consulta de búsqueda DNS inversa, un equipo o dispositivo puede detectar el nombre de otro equipo o dispositivo mediante su dirección IP. La implementación de una zona de búsqueda inversa suele mejorar el rendimiento de DNS y aumenta enormemente el acierto de las consultas DNS.

Cuando se crea una zona de búsqueda inversa, se configura en DNS el dominio in-addr.arpa, que se define en los estándares DNS y se reservó en el espacio de nombres DNS de Internet para proporcionar una forma práctica y confiable de llevar a cabo consultas inversas. Para crear el espacio de nombres inverso, se forman subdominios dentro del dominio in-addr.arpa, con la clasificación inversa de los números en la notación decimal con punto de direcciones IP.

El dominio in-addr.arpa se aplica a todas las redes TCP/IP que se basan en el protocolo de Internet versión 4 (IPv4) direccionamiento. El Asistente para nueva zona da por supuesto que se usa este dominio al crear una zona de búsqueda inversa.

Al ejecutar el Asistente para nueva zona, se recomienda realizar las siguientes selecciones:

|Elementos de configuración|Valores de ejemplo|
|-----------------------|------------------|
|Tipo de zona|Se seleccionan **Zona principal** y **Almacenar la zona en Active Directory**|
|Ámbito de replicación de zona de Active Directory|**Para todos los servidores DNS en este dominio**|
|Primera página del asistente Nombre de la zona de búsqueda inversa|**Zona de búsqueda inversa IPv4**|
|Segunda página del asistente Nombre de la zona de búsqueda inversa|ID de red = 10.0.0.|
|Actualizaciones dinámicas|**Permitir sólo actualizaciones dinámicas seguras**|

### <a name="bkmk_NetFndtn_Pln_DomAccess"></a>Planeación de acceso al dominio
Para iniciar sesión en el dominio, el equipo debe ser un equipo miembro del dominio y se debe crear la cuenta de usuario en AD DS antes del intento de inicio de sesión.

> [!NOTE]
> Los equipos individuales que ejecutan Windows cuentan con una base de datos de cuentas de usuario de grupos y usuarios locales que se conoce como la base de datos de cuentas de usuario del Administrador de cuentas de seguridad (SAM). Cuando crea una cuenta de usuario en el equipo local en la base de datos SAM, puede iniciar sesión en el equipo local, pero no en un dominio. Las cuentas de usuario de dominio se crean con Microsoft Management Console (MMC) de Usuarios y equipos de Active Directory en un controlador de dominio, no con grupos o usuarios locales en el equipo local.

Después del primer inicio de sesión correcto con las credenciales de inicio de sesión del dominio, la configuración del inicio de sesión se conserva a menos que el equipo se quite del dominio o la configuración del inicio de sesión se modifique manualmente.

Antes de iniciar una sesión en el dominio:

-   Cree cuentas de usuario en Usuarios y equipos de Active Directory. Cada usuario debe tener una cuenta de usuario de Active Directory Domain Services en Usuarios y equipos de Active Directory. Para obtener más información, vea [Crear una cuenta de usuario en Usuarios y equipos de Active Directory](#BKMK_createUA).

-   Compruebe que la configuración de la dirección IP sea correcta. Para unir un equipo a un dominio, el equipo debe tener una dirección IP. En esta guía, los servidores se configuran con direcciones IP estáticas y los equipos cliente reciben concesiones de direcciones IP procedentes del servidor DHCP. Por este motivo, se debe implementar el servidor DHCP antes de unir los clientes al dominio. Para obtener más información, vea [Implementar DHCP1](#BKMK_deployDHCP01).

-   Una el equipo al dominio. Todos los equipos que proporcionen recursos de red o tengan acceso a ellos deben unirse al dominio. Para obtener más información, vea [Unir equipos servidor al dominio e iniciar sesión](#BKMK_joinlogserver) y [Unir equipos cliente al dominio e iniciar sesión](#BKMK_joinlogclients).

### <a name="bkmk_NetFndtn_Pln_DHCP-01"></a>Planear la implementación de DHCP1
A continuación se describen los principales pasos de planeación necesarios previos a la instalación del rol de servidor DHCP en DHCP1.

#### <a name="planning-dhcp-servers-and-dhcp-forwarding"></a>Planear servidores DHCP y reenvío DHCP
Como los mensajes DHCP son mensajes de difusión, los enrutadores no los reenvían entre subredes. Si tiene varias subredes y desea proporcionar el servicio DHCP para cada subred, realice una de las acciones siguientes:

-   Instalar un servidor DHCP en cada subred

-   Configurar los enrutadores para reenviar los mensajes de difusión DHCP entre subredes y configurar múltiples ámbitos en el servidor DHCP, un ámbito por subred.

En la mayoría de los casos, configurar los enrutadores para reenviar mensajes de difusión DHCP es más rentable que implementar un servidor DHCP en cada segmento físico de la red.

#### <a name="planning-ip-address-ranges"></a>Planear intervalos de direcciones IP
Cada subred debe tener su propio intervalo de direcciones IP únicas. En un servidor DHCP, dichos intervalos se representan con ámbitos.

Un ámbito es una agrupación administrativa de direcciones IP para equipos de una subred que usa el servicio DHCP. El administrador crea primero un ámbito para cada subred física y, a continuación, lo usa para definir los parámetros usados por los clientes.

Un ámbito tiene las siguientes propiedades:

-   Un intervalo de direcciones IP desde el que incluir o excluir las direcciones usadas para las ofertas de concesión de servicio DHCP.

-   Una máscara de subred, que determina el prefijo de subred para una dirección IP determinada.

-   Un nombre de ámbito asignado al crearlo.

-   Valores de duración de la concesión, asignados a los clientes DHCP que reciben las direcciones IP asignadas dinámicamente.

-   Todas las opciones de ámbito DHCP configuradas para la asignación a clientes DHCP (por ejemplo, dirección IP del servidor DNS y dirección IP de la puerta de enlace predeterminada o enrutador).

-   Las reservas se usan opcionalmente para garantizar que un cliente DHCP reciba siempre la misma dirección IP.

Antes de implementar los servidores, cree una lista con las subredes y los intervalos de direcciones IP que desea usar para cada subred.

#### <a name="planning-subnet-masks"></a>Planear máscaras de subred
Las máscaras de subred sirven para distinguir los identificadores de red de los identificadores de host dentro de una dirección IP. Cada máscara de subred es un número de 32 bits que usa grupos de bits consecutivos de todo unos (1) para reconocer el identificador de red y de todo ceros (0) para reconocer las partes del identificador de host de una dirección IP.

Por ejemplo, la máscara de subred que se usa normalmente con la dirección IP 131.107.16.200 es el siguiente número binario de 32 bits:

```
11111111 11111111 00000000 00000000
```

Este número de máscara de subred es 16 bits de uno seguidos de 16 bits cero, que indica que las secciones de Id. de identificador y el host de red de esta dirección IP tienen ambas 16 bits de longitud. Normalmente, esta máscara de subred se muestra en notación decimal con puntos como 255.255.0.0.

La siguiente tabla muestra máscaras de subred para las clases de direcciones de Internet.

|Clase de dirección|Bits para la máscara de subred|Máscara de subred|
|-----------------|------------------------|---------------|
|Clase A|11111111 00000000 00000000 00000000|255.0.0.0|
|Clase B|11111111 11111111 00000000 00000000|255.255.0.0|
|Clase C|11111111 11111111 11111111 00000000|255.255.255.0|

Cuando se crea un ámbito en DHCP y se escribe el intervalo de direcciones IP para el ámbito, DHCP proporciona estos valores predeterminados para las máscaras de subred. Por lo general, los valores de la máscara de subred predeterminados son aceptables para la mayoría de las redes que no tienen requisitos especiales y donde cada segmento de red IP corresponde a una sola red física.

En ciertos casos, se pueden usar máscaras de subred personalizadas para implementar las subredes IP. Con el establecimiento de subredes IP, se puede subdividir la parte del identificador de host predeterminada de una dirección IP para especificar subredes, que son subdivisiones del identificador de red basado en clases original.

Mediante la personalización de la longitud de la máscara de subred, se puede reducir el número de bits que se usan para el identificador de host real.

Para evitar problemas de direccionamiento y enrutamiento, debería asegurarse de que todos los equipos TCP/IP de un segmento de red usan la misma máscara de subred y de que cada equipo o dispositivo tenga una dirección IP única.

#### <a name="planning-exclusion-ranges"></a>Planear intervalos de exclusión
Cuando se crea un ámbito en un servidor DHCP, se especifica un intervalo de direcciones IP que incluye todas las direcciones IP que el servidor DHCP está autorizado a conceder a clientes DHCP, como equipos y otros dispositivos. Si después configura manualmente algunos servidores y otros dispositivos con direcciones IP estáticas del mismo intervalo de direcciones IP que está usando el servidor DHCP, puede crear accidentalmente un conflicto de dirección IP en el que usted y el servidor DHCP tienen asignada la misma dirección IP para diferentes dispositivos.

Para solucionar este problema, puede crear un intervalo de exclusión para el ámbito DHCP. Un intervalo de exclusión es un contiguos intervalo de direcciones IP dentro del intervalo de direcciones IP del ámbito que no se puede usar el servidor DHCP. Si crea un intervalo de exclusión, el servidor DHCP no asigna las direcciones en ese intervalo, lo cual le permite asignar manualmente estas direcciones sin crear un conflicto de direcciones IP.

El servidor DHCP puede excluir direcciones IP de la distribución creando un intervalo de exclusión para cada ámbito. Las exclusiones se deberían usar para todos los dispositivos que están configurados con una dirección IP estática. Las direcciones excluidas deberían incluir todas las direcciones IP asignadas manualmente a otros servidores, clientes no DHCP, estaciones de trabajo sin disco o clientes PPP y de Enrutamiento y acceso remoto.

Se recomienda configurar el intervalo de exclusión con direcciones adicionales en previsión de una futura ampliación de la red. En la tabla siguiente proporciona un intervalo de exclusión de ejemplo para un ámbito con un intervalo de direcciones IP de 10.0.0.1 - 10.0.0.254 y una máscara de subred de 255.255.255.0.

|Elementos de configuración|Valores de ejemplo|
|-----------------------|------------------|
|Dirección IP inicial del intervalo de exclusión|10.0.0.1|
|Dirección IP final del intervalo de exclusión|10.0.0.25|

#### <a name="planning-tcpip-static-configuration"></a>Planear la configuración estática de TCP/IP
Algunos dispositivos, como enrutadores, servidores DHCP y servidores DNS, se deben configurar con una dirección IP estática. Además, es posible que tenga dispositivos adicionales, como impresoras, para los que desee asegurarse de que tengan siempre la misma dirección IP. Reúna en una lista los dispositivos que desee configurar estáticamente para cada subred y, a continuación, planee el intervalo de exclusión que desea usar en el servidor DHCP para asegurarse de que el servidor DHCP no conceda la dirección IP de un dispositivo configurado estáticamente. Un intervalo de exclusión es una secuencia limitada de direcciones IP dentro de un ámbito que está excluida de las ofertas del servicio DHCP. Los intervalos de exclusión garantizan que el servidor no ofrece ninguna de las direcciones incluidas en esos intervalos a los clientes DHCP de la red.

Por ejemplo, si el intervalo de direcciones IP para una subred va de 192.168.0.1 a 192.168.0.254 y tiene diez dispositivos que desea configurar con una dirección IP estática, puede crear un intervalo de exclusión para el ámbito 192.168.0.*x* que incluya diez o más direcciones IP: 192.168.0.1 a 192.168.0.15.

En este ejemplo, se usan diez de las direcciones IP excluidas para configurar servidores y otros dispositivos con direcciones IP estáticas, y quedan cinco direcciones IP más disponibles para la configuración estática de nuevos dispositivos que puede que quiera agregar en el futuro. Con este intervalo de exclusión, se ha dejado al servidor DHCP con un grupo de direcciones que oscila entre 192.168.0.16 y 192.168.0.254.

Elementos de configuración de ejemplo adicional para AD DS y DNS se proporcionan en la tabla siguiente.

|Elementos de configuración|Valores de ejemplo|
|-----------------------|------------------|
|Enlaces de conexión de red|Ethernet|
|Configuración del servidor DNS|DC1.corp.contoso.com|
|Dirección IP del servidor DNS preferido|10.0.0.2|
|Valores del cuadro de diálogo Agregar ámbito<br /><br />1.  Nombre de ámbito<br />2.  Dirección IP inicial<br />3.  Dirección IP final<br />4.  Máscara de subred<br />5.  Puerta de enlace predeterminada (opcional)<br />6.  Duración de la concesión|1.  Subred principal<br />2.  10.0.0.1<br />3.  10.0.0.254<br />4.  255.255.255.0<br />5.  10.0.0.1<br />6. 8 días|
|Modo de funcionamiento del servidor DHCP IPv6|No habilitado|

## <a name="BKMK_deployment"></a>Implementación de red principal
Para implementar una red principal, los pasos básicos son los siguientes:

1.  [Configuración de todos los servidores](#BKMK_configuringAll)

2.  [Implementar DC1](#BKMK_deployADDNS01)

3.  [Unir equipos servidor al dominio e iniciar sesión](#BKMK_joinlogserver)

4.  [Implementar DHCP1](#BKMK_deployDHCP01)

5.  [Unir los equipos cliente al dominio e iniciar sesión](#BKMK_joinlogclients)

6.  [Implementar características opcionales para la autenticación de acceso de red y servicios Web](#BKMK_optionalfeatures)

> [!NOTE]
> -   En esta guía se proporcionan comandos de Windows PowerShell equivalentes para la mayoría de los procedimientos. Antes de ejecutar estas cmdlets en Windows PowerShell, reemplace los valores de ejemplo por los valores apropiados según su implementación de red. Además, debe introducir cada cmdlet en una sola línea en Windows PowerShell, aunque en esta guía podrían aparecer cmdlets individuales en varias líneas debido a las restricciones de formato y la visualización del documento con el explorador u otra aplicación.
> -   Los procedimientos de esta guía no incluyen instrucciones para los casos en los que se abre el cuadro de diálogo **Control de cuentas de usuario** para solicitar permiso para continuar. Si aparece este cuadro de diálogo en respuesta a sus acciones mientras realiza los procedimientos de esta guía, haga clic en **Continuar**.

### <a name="BKMK_configuringAll"></a>Configuración de todos los servidores
Antes de instalar otras tecnologías, como Active Directory Domain Services o DHCP, es importante configurar los siguientes elementos.

-   [El nombre del equipo](#BKMK_rename)

-   [Configurar una dirección IP estática](#BKMK_ip)

Puede usar las secciones siguientes para realizar estas acciones en cada servidor.

El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.

#### <a name="BKMK_rename"></a>El nombre del equipo
Puede usar el procedimiento de esta sección para cambiar el nombre de un equipo. Cambiar el nombre del equipo resulta útil para situaciones en las que el sistema operativo ha creado automáticamente un nombre de equipo que no desea usar.

> [!NOTE]
> Para llevar a cabo este procedimiento con Windows PowerShell, abra PowerShell, escriba los siguientes cmdlets en líneas separadas y después presione ENTRAR. También debe reemplazar *nombreDeEquipo* por el nombre que quiera usar.
>
> `Rename-Computer`*ComputerName*
>
> `Restart-Computer`

###### <a name="to-rename-computers-running-windows-server-2016-windows-server-2012-r2-and-windows-server-2012"></a>Para cambiar el nombre de los equipos que ejecutan Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

1.  En el Administrador del servidor, haga clic en **Servidor local**. El panel de detalles muestra las **propiedades** del equipo.

2.  En **Propiedades**, en **Nombre del equipo**, haga clic en el nombre de equipo existente. Se abre el cuadro de diálogo **Propiedades del sistema**. Haga clic en **Cambiar**. Se abre el cuadro de diálogo **Cambios en el dominio o el nombre del equipo**.

3.  En el cuadro de diálogo **Cambios en el dominio o el nombre del equipo**, escriba un nombre nuevo para el equipo en **Nombre del equipo**. Por ejemplo, si desea asignarle el nombre DC1, escriba **DC1**.

4.  Haga clic dos veces en **Aceptar** y, a continuación, haga clic en **Cerrar**. Si desea reiniciar el equipo de inmediato para completar el cambio de nombre, haga clic en **Reiniciar ahora**. De lo contrario, haga clic en **Reiniciar más tarde**.

> [!NOTE]
> Para obtener información sobre cómo cambiar el nombre de los equipos que ejecutan otros sistemas operativos de Microsoft, consulte [Apéndice A - cambiar el nombre de equipos](#BKMK_A).

#### <a name="BKMK_ip"></a>Configurar una dirección IP estática
Puede usar los procedimientos de este tema para configurar el protocolo de Internet versión 4 (IPv4) propiedades de una conexión de red con una dirección IP estática de direcciones para los equipos que ejecutan Windows Server 2016.

> [!NOTE]
> Para llevar a cabo este procedimiento con Windows PowerShell, abra PowerShell, escriba los siguientes cmdlets en líneas separadas y después presione ENTRAR. También debe reemplazar los nombres de interfaz y las direcciones IP de este ejemplo por los valores que quiera usar para configurar el equipo.
>
> `New-NetIPAddress -IPAddress 10.0.0.2 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24`
>
> `Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 127.0.0.1`

###### <a name="to-configure-a-static-ip-address-on-computers-running-windows-server-2016-windows-server-2012-r2-and-windows-server-2012"></a>Para configurar una dirección IP estática en equipos que ejecutan Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

1.  En la barra de tareas, haga clic con el botón secundario en el icono Red y después haga clic en **Abrir el Centro de redes y recursos compartidos**.

2.  En **centro de redes y recursos compartidos**, haga clic en **cambiar configuración del adaptador**. Se abre la carpeta **Conexiones de red** y muestra las conexiones de red disponibles.

3.  En **Conexiones de red**, haga clic con el botón secundario en la conexión que desea configurar y, a continuación, haga clic en **Propiedades**. Se abre el cuadro de diálogo **Propiedades** de la conexión de red.

4.  En el cuadro de diálogo **Propiedades** de la conexión de red, en **Esta conexión usa los siguientes elementos**, seleccione **Protocolo de Internet versión 4 (TCP/IPv4)** y, a continuación, haga clic en **Propiedades**. Se abre el cuadro de diálogo **Propiedades de protocolo de Internet versión 4 (TCP/IPv4)** .

5.  En **Propiedades de protocolo de Internet versión 4 (TCP/IPv4)** , en la pestaña **General**, haga clic en **Usar la siguiente dirección IP**. En **Dirección IP**, escriba la dirección IP que desea usar.

6.  Presione el tabulador para colocar el cursor en **Máscara de subred**. Se escribe automáticamente un valor predeterminado para la máscara de subred. Acepte la máscara de subred predeterminada o escriba la máscara de subred que quiera usar.

7.  En **Puerta de enlace predeterminada**, escriba la dirección IP de la puerta de enlace predeterminada.

    > [!NOTE]
    > **Puerta de enlace predeterminada** se debe configurar con la misma dirección IP que use en la interfaz de red de área local (LAN) del enrutador. Por ejemplo, si tiene un enrutador conectado a una red de área extensa (como Internet) y a la red LAN, configure la interfaz LAN con la misma dirección IP que vaya a especificar después como la **puerta de enlace predeterminada**. En otro ejemplo, si tiene un enrutador que está conectado a dos redes LAN, donde la LAN A usa el intervalo de direcciones 10.0.0.0/24 y LAN B usa el intervalo de direcciones 192.168.0.0/24, configure la dirección IP del enrutador de la LAN A con una dirección de ese intervalo de direcciones (por ejemplo, 10.0.0.1). Además, en el ámbito DHCP para este intervalo de direcciones, configure la **puerta de enlace predeterminada** con la dirección IP 10.0.0.1. En cuanto a la LAN B, configure su interfaz de enrutador con una dirección de ese intervalo de direcciones (como 192.168.0.1) y después configure el ámbito 192.168.0.0/24 de la LAN B con un valor de **puerta de enlace predeterminada** de 192.168.0.1.

8.  En **Servidor DNS preferido**, escriba la dirección IP del servidor DNS. Si tiene previsto usar el equipo local como servidor DNS preferido, escriba la dirección IP de ese equipo.

9. En **Servidor DNS alternativo**, escriba la dirección IP del servidor DNS alternativo si lo hay. Si tiene previsto usar el equipo local como servidor DNS alternativo, escriba la dirección IP de ese equipo.

10. Haga clic en **Aceptar** y, a continuación, en **Cerrar**.

> [!NOTE]
> Para obtener información sobre cómo configurar una dirección IP estática en equipos que ejecutan otros sistemas operativos de Microsoft, consulte [Apéndice B - configurar direcciones IP estáticas](#BKMK_B).

#### <a name="BKMK_deployADDNS01"></a>Implementar DC1
Para implementar DC1, que es el equipo que ejecuta Servicios de dominio de Active Directory (AD DS) y DNS, debe completar estos pasos en el orden siguiente:

-   Completar los pasos de la sección [Configurar todos los servidores](#BKMK_configuringAll)

-   [Instalar AD DS y DNS para un bosque nuevo](#BKMK_installAD-DNS)

-   [Crear una cuenta de usuario en los equipos y usuarios de Active Directory](#BKMK_createUA)

-   [Asignar pertenencia a grupos](#BKMK_assigngroup)

-   [Configurar una zona de búsqueda inversa DNS](#BKMK_reverse)

**Privilegios administrativos**

Si está instalando una red pequeña y es el único administrador, se recomienda que cree una cuenta de usuario para usted mismo y que, después, agregue su cuenta de usuario como miembro de Administradores de empresas y de Admins. del dominio. Al hacerlo, puede actuar como el administrador para todos los recursos de red más fácilmente. También se recomienda que inicie sesión con esta cuenta solo cuando necesite realizar tareas administrativas y, asimismo, que cree una cuenta de usuario separada para realizar tareas no relacionadas con TI.

Si tiene una organización más grande con varios administradores, consulte la documentación de AD DS para determinar la mejor pertenencia para los empleados de la organización.

**Diferencias entre las cuentas de usuario de dominio y cuentas de usuario en el equipo local**

Una de las ventajas de una infraestructura basada en dominio es que no tiene que crear cuentas de usuario en cada equipo del dominio. Esto ocurre si es un equipo cliente o un servidor.

Debido a esto, no debe crear cuentas de usuario en cada equipo en el dominio. Cree todas las cuentas de usuario en Usuarios y equipos de Active Directory y recurra a los procedimientos anteriores para asignar pertenencia a grupos. De manera predeterminada, todas las cuentas de usuario son miembros del grupo Usuarios del dominio.

Todos los miembros del grupo Usuarios del dominio pueden iniciar sesión en cualquier equipo cliente después de que se haya unido al dominio.

Puede configurar cuentas de usuario para designar los días y horas en que se permite al usuario iniciar sesión en el equipo. Igualmente, puede designar qué equipos puede utilizar cada usuario. Para configurar estos parámetros, abra Usuarios y equipos de Active Directory, busque la cuenta de usuario que desea configurar y haga doble clic en ella. En **Propiedades** de la cuenta de usuario, haga clic en la pestaña **Cuenta** y después en **Horas de inicio de sesión** o **Iniciar sesión en**.

#### <a name="BKMK_installAD-DNS"></a>Instalar AD DS y DNS para un bosque nuevo

Puede usar uno de los procedimientos siguientes para instalar los servicios de dominio de Active Directory (AD DS) y DNS y crear un nuevo dominio en un bosque nuevo. 

El primer procedimiento proporciona instrucciones sobre cómo realizar estas acciones mediante el uso de Windows PowerShell, mientras que el segundo procedimiento muestra cómo instalar AD DS y DNS mediante el administrador del servidor.

>[!IMPORTANT]
>Cuando termine de realizar los pasos de este procedimiento, el equipo se reiniciará automáticamente.

**Instalar AD DS y DNS con Windows PowerShell**

Puede usar los comandos siguientes para instalar y configurar AD DS y DNS. Debe reemplazar el nombre de dominio en este ejemplo con el valor que desea usar para su dominio.

>[!NOTE]
>Para obtener más información acerca de estos comandos de Windows PowerShell, consulte los siguientes temas de referencia.
>- [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/servermanager/install-windowsfeature?view=win10-ps)
>- [Install-ADDSForest](https://docs.microsoft.com/powershell/module/addsdeployment/install-addsforest?view=win10-ps)

Para poder realizar este procedimiento debe pertenecer, como mínimo, al grupo **Administradores**.

- Ejecutar Windows PowerShell como administrador, escriba el siguiente comando y, a continuación, presione ENTRAR:  

`Install-WindowsFeature AD-Domain-Services -IncludeManagementTools`

Cuando haya finalizado correctamente la instalación, se muestra el mensaje siguiente en Windows PowerShell.


    Success Restart Needed  Exit Code   Feature Result
    ------- --------------  ---------   --------------
    True    No              Success     {Active Directory Domain Services, Group P...


- En Windows PowerShell, escriba el comando siguiente, reemplazando el texto **corp.contoso.com** con el nombre de dominio y, a continuación, presione ENTRAR:

````
Install-ADDSForest -DomainName "corp.contoso.com"
````

- Durante el proceso de instalación y configuración, que es visible en la parte superior de la ventana de Windows PowerShell, aparece el mensaje siguiente. Una vez que aparece, escriba una contraseña y, a continuación, presione ENTRAR.

    **SafeModeAdministratorPassword:**

- Después de escribir una contraseña y presione ENTRAR, aparece el siguiente mensaje de confirmación. Escriba la misma contraseña y, a continuación, presione ENTRAR.

    **Confirmar SafeModeAdministratorPassword:**

- Cuando aparezca el mensaje siguiente, escriba la letra **Y** y, a continuación, presione ENTRAR.


~~~
The target server will be configured as a domain controller and restarted when this operation is complete.
Do you want to continue with this operation?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):
~~~

- Si desea, puede leer los mensajes de advertencia que se muestran durante la instalación normal y correcta de AD DS y DNS. Estos mensajes son normales y no son una indicación de error de instalación.

- Una vez finalizada correctamente la instalación, aparecerá un mensaje indicando que va a haber iniciado en el equipo para que pueda reiniciar el equipo. Si hace clic en **cerrar**, inmediatamente se desconecte el equipo y el equipo se reinicia. Si no hace clic en **cerrar**, el equipo se reinicia tras un período de tiempo predeterminado.

- Después de reiniciar el servidor, puede comprobar una instalación correcta de los servicios de dominio de Active Directory y DNS. Abra Windows PowerShell, escriba el siguiente comando y presione ENTRAR.

````
Get-WindowsFeature
````

Los resultados de este comando se muestran en Windows PowerShell y deben ser similares a los resultados en la imagen siguiente. Para las tecnologías instaladas, los corchetes a la izquierda del nombre de la tecnología contienen el carácter **X**y el valor de **estado de instalación** es **instalado**.

![Resultados del comando Get-WindowsFeature](../media/Core-Network-Guide/server-roles-installed.jpg)

**Instalar AD DS y DNS con el administrador del servidor**

1.  En DC1, en **Administrador del servidor**, haga clic en **Administrar** y después, haga clic en **Agregar roles y características**. Se abre el Asistente para agregar roles y características.

2.  En **Antes de comenzar**, haga clic en **Siguiente**.

    > [!NOTE]
    > La página **Antes de comenzar** del Asistente para agregar roles y características no se muestra si ha seleccionado anteriormente **Omitir esta página de forma predeterminada** al ejecutar el asistente.

3.  En **Seleccionar tipo de instalación**, asegúrese de que la opción **Instalación basada en características o en roles** está seleccionada y, a continuación, haga clic en **Siguiente**.

4.  En **Seleccionar servidor de destino**, asegúrese de que la opción **Seleccionar un servidor del grupo de servidores** está seleccionada. En **Grupo de servidores**, asegúrese de que el equipo local está seleccionado. Haz clic en **Siguiente**.

5.  En **Seleccionar roles de servidor**, en **Roles**, haga clic en **Active Directory Domain Services**. En **¿Desea agregar características requeridas para Active Directory Domain Services?** , haga clic en **Agregar características**. Haz clic en **Siguiente**.

6.  En **Seleccionar características**, haga clic en **Siguiente**. En **Active Directory Domain Services**, repase la información proporcionada y haga clic en **Siguiente**.

7.  En **Confirmar selecciones de instalación**, haga clic en **Instalar**. La página Progreso de la instalación muestra el estado durante el proceso de instalación. Cuando el proceso finalice, haga clic en **Promover este servidor a controlador de dominio** en los detalles del mensaje. Se abre el Asistente para configuración de Active Directory Domain Services.

8.  En **Configuración de implementación**, seleccione **Agregar un nuevo bosque**. En **Nombre de dominio raíz**, escriba el nombre de dominio completo (FQDN) del dominio. Por ejemplo, si el FQDN es corp.contoso.com, escriba **corp.contoso.com**. Haz clic en **Siguiente**.

9. En **Opciones del controlador de dominio**, en **Seleccionar nivel funcional del nuevo bosque y dominio raíz**, seleccione el nivel funcional del bosque y del dominio que quiera usar. En **Especificar capacidades del controlador de dominio**, asegúrese de que **Servidor de Sistema de nombres de dominio (DNS)** y **Catálogo global (GC)** están seleccionados. En **Contraseña** y **Confirmar contraseña**, escriba la contraseña del Modo de restauración de servicios de directorio (DSRM) que desea usar. Haz clic en **Siguiente**.

10. En **Opciones de DNS**, haga clic en **Siguiente**.

11. En **Opciones adicionales**, confirme el nombre NetBIOS asignado al dominio y cámbielo solamente si es necesario. Haz clic en **Siguiente**.

12. En **Rutas de acceso**, en **Especificar la ubicación de la base de datos de AD DS, archivos de registro y SYSVOL**, haga lo siguiente:

    -   Acepte los valores predeterminados.

    -   Escriba las ubicaciones de las carpetas que desea usar para **Carpeta de la base de datos**, **Carpeta de archivos de registro** y **Carpeta SYSVOL**.

13. Haz clic en **Siguiente**.

14. En **Revisar opciones**, repase las selecciones realizadas.

15. Si desea exportar la configuración a un script de Windows PowerShell, haga clic en **Ver script**. El script se abre en el Bloc de notas y puede guardarlo en la ubicación de carpeta que desee. Haz clic en **Siguiente**. Las selecciones realizadas se validan en **Comprobación de requisitos previos**. Cuando finalice la comprobación, haga clic en **Instalar**. Cuando Windows se lo solicite, haga clic en **Cerrar**. El servidor se reinicie para completar la instalación de AD DS y DNS.

16. Para comprobar una instalación correcta, vea la consola de administrador del servidor una vez reiniciado el servidor. AD DS y DNS debe aparecer en el panel izquierdo, al igual que los elementos resaltados en la imagen siguiente.

![AD DS y DNS en el administrador del servidor](../media/Core-Network-Guide/server-roles-installed-sm.jpg)

##### <a name="BKMK_createUA"></a>Crear una cuenta de usuario en los equipos y usuarios de Active Directory
Puede usar este procedimiento para crear una cuenta de usuario de dominio en el complemento Microsoft Management Console (MMC) de Usuarios y equipos de Active Directory.

El requisito mínimo para llevar a cabo este procedimiento consiste en pertenecer a **Admins. del dominio** o grupo equivalente.

> [!NOTE]
> Para llevar a cabo este procedimiento con Windows PowerShell, abra PowerShell, escriba el siguiente cmdlet en una línea y después presione ENTRAR. También debe reemplazar el nombre de cuenta de usuario de este ejemplo por el valor que quiera usar.
>
> `New-ADUser -SamAccountName User1 -AccountPassword (read-host "Set user password" -assecurestring) -name "User1" -enabled $true -PasswordNeverExpires $true -ChangePasswordAtLogon $false`
>
> Después de presionar ENTRAR, escriba la contraseña de la cuenta de usuario. Se crea la cuenta y, de forma predeterminada, se concede pertenencia al grupo Usuarios del dominio.
>
> Con el siguiente cmdlet, puede asignar otras pertenencias a grupos para la nueva cuenta de usuario. En el siguiente ejemplo se agrega Usuario1 a los grupos Admins. del dominio y Administradores de empresas. Antes de ejecutar este comando, asegúrese de cambiar el nombre de la cuenta de usuario, el nombre del dominio y los grupos para que coincidan con sus requisitos.
>
> `Add-ADPrincipalGroupMembership -Identity "CN=User1,CN=Users,DC=corp,DC=contoso,DC=com" -MemberOf "CN=Enterprise Admins,CN=Users,DC=corp,DC=contoso,DC=com","CN=Domain Admins,CN=Users,DC=corp,DC=contoso,DC=com"`

###### <a name="to-create-a-user-account"></a>Para crear una cuenta de usuario

1.  En DC1, en Administrador del servidor, haga clic en **Herramientas** y, a continuación, haga clic en **Usuarios y equipos de Active Directory**. Se abre MMC de Usuarios y equipos de Active Directory. Haga clic en el nodo de su dominio si no está seleccionado. Por ejemplo, si el dominio es corp.contoso.com, haga clic en **corp.contoso.com**.

2.  En el panel de detalles, haga clic con el botón secundario en la carpeta a la que desea agregar una cuenta de usuario.

    **¿Dónde?**

    -   Directorio de usuarios y equipos de Active /*nodo del dominio*/*carpeta*

3.  Elija **Nuevo** y haga clic en **Usuario**. El **nuevo objeto - usuario** abre el cuadro de diálogo.

4.  En **Nombre**, escriba el nombre del usuario.

5.  En **Iniciales**, escriba las iniciales del usuario.

6.  En **Apellidos**, escriba los apellidos del usuario.

7.  Modifique **Nombre completo** para agregar iniciales o invertir el orden del nombre y los apellidos.

8.  En **Nombre de inicio de sesión de usuario**, escriba el nombre correspondiente. Haz clic en **Siguiente**.

9. En **Nuevo objeto: usuario**, en **Contraseña** y en **Confirmar contraseña**, escriba la contraseña del usuario y, a continuación, seleccione las opciones apropiadas para la contraseña.

10. Haga clic en **Siguiente**, revise la configuración de la nueva cuenta de usuario y, a continuación, haga clic en **Finalizar**.

##### <a name="BKMK_assigngroup"></a>Asignar pertenencia a grupos
Puede usar este procedimiento para agregar un usuario, equipo o grupo a un grupo en Microsoft Management Console (MMC) de Usuarios y equipos de Active Directory.

El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Admins. del dominio** o grupo equivalente.

###### <a name="to-assign-group-membership"></a>Para asignar la pertenencia a grupos

1.  En DC1, en Administrador del servidor, haga clic en **Herramientas** y, a continuación, haga clic en **Usuarios y equipos de Active Directory**. Se abre MMC de Usuarios y equipos de Active Directory. Haga clic en el nodo de su dominio si no está seleccionado. Por ejemplo, si el dominio es corp.contoso.com, haga clic en **corp.contoso.com**.

2.  En el panel de detalles, haga doble clic en la carpeta que contiene el grupo al cual desea agregar un miembro.

    ¿Dónde?

    -   **Directorio de usuarios y equipos de Active**/*nodo del dominio*/*carpeta que contiene el grupo*

3.  En el panel de detalles, haga clic con el botón secundario en el objeto que desea agregar a un grupo (como un usuario o un equipo) y, a continuación, haga clic en **Propiedades**. El objeto **propiedades** abre el cuadro de diálogo. Haga clic en la pestaña **Miembro del**.

4.  En la pestaña **Miembro del**, haga clic en **Agregar**.

5.  En **Escriba los nombres de objeto que desea seleccionar**, especifique el nombre del grupo al que desea agregar el objeto y, a continuación, haga clic en **Aceptar**.

6.  Para asignar pertenencia a grupos para otros usuarios, grupos o equipos, repita los pasos 4 y 5 de este procedimiento.

##### <a name="BKMK_reverse"></a>Configurar una zona de búsqueda inversa DNS
Puede usar este procedimiento para configurar una zona de búsqueda inversa en el Sistema de nombres de dominio (DNS).

El requisito mínimo para llevar a cabo este procedimiento es la pertenencia al grupo **Admins. del dominio**.

> [!NOTE]
> -   Para organizaciones medianas y grandes, se recomienda que configure y use el grupo DNSAdmins en usuarios y equipos de usuarios de Active Directory. Para obtener más información, vea [Recursos técnicos adicionales](#BKMK_resources).
> -   Para llevar a cabo este procedimiento con Windows PowerShell, abra PowerShell, escriba el siguiente cmdlet en una línea y después presione ENTRAR. También debe reemplazar la zona de búsqueda inversa y los nombres de archivos de zona de este ejemplo por los valores que quiera usar. Asegúrese de invertir el identificador de red para el nombre de zona inversa. Por ejemplo, si el identificador de red es 192.168.0, cree el nombre de la zona de búsqueda inversa **0.168.192.in-addr.arpa**.
>
> `Add-DnsServerPrimaryZone 0.0.10.in-addr.arpa -ZoneFile 0.0.10.in-addr.arpa.dns`

###### <a name="to-configure-a-dns-reverse-lookup-zone"></a>Para configurar una zona de búsqueda inversa DNS

1.  En DC1, en Administrador del servidor, haga clic en **Herramientas** y, a continuación, en **DNS**. Se abre MMC de DNS.

2.  En DNS, haga doble clic en el nombre del servidor para expandir el árbol (si aún no aparece expandido). Por ejemplo, si el nombre del servidor DNS es DC1, haga doble clic en **DC1**.

3.  Seleccione **Zonas de búsqueda inversa**, haga clic con el botón secundario en **Zonas de búsqueda inversa** y, a continuación, haga clic en **Zona nueva**. Se abre el Asistente para nueva zona.

4.  En el **Asistente para nueva zona**, haga clic en **Siguiente**.

5.  En **Tipo de zona**, seleccione **Zona principal**.

6.  Si el servidor DNS es un controlador de dominio grabable, asegúrese de que la opción **Almacenar la zona en Active Directory** está seleccionada. Haz clic en **Siguiente**.

7.  En **Ámbito de replicación de zona de Active Directory**, seleccione **Para todos los servidores DNS que se ejecutan en controladores de dominio en este dominio**, a menos que tenga un motivo específico para elegir una opción diferente. Haz clic en **Siguiente**.

8.  En la primera página del asistente **Nombre de la zona de búsqueda inversa**, seleccione **Zona de búsqueda inversa para IPv4**. Haz clic en **Siguiente**.

9. En la segunda página del asistente **Nombre de la zona de búsqueda inversa**, realice una de las siguientes acciones:

    -   En **Id. de red**, escriba el identificador de red del intervalo de direcciones IP. Por ejemplo, si el intervalo de direcciones IP oscila entre 10.0.0.1 y 10.0.0.254, escriba **10.0.0**.

    -   En **Nombre de la zona de búsqueda inversa**, el nombre de la zona de búsqueda inversa para IPv4 se agrega automáticamente. Haz clic en **Siguiente**.

10. En **Actualización dinámica**, seleccione el tipo de actualizaciones dinámicas que desea permitir. Haz clic en **Siguiente**.

11. En **Finalización del Asistente para nueva zona**, repase sus opciones y, a continuación, haga clic en **Finalizar**.

#### <a name="BKMK_joinlogserver"></a>Unir equipos servidor al dominio e iniciar sesión
Después de que ha instalado los servicios de dominio de Active Directory (AD DS) y ha creado una o varias cuentas de usuario que tienen permisos para unir un equipo al dominio, puede unir servidores de red principal al dominio e inicie sesión en los servidores para poder instalar adicionales tecnologías, como protocolo de configuración dinámica de Host (DHCP).

En todos los servidores que se va a implementar, excepto para el servidor que ejecuta AD DS, haga lo siguiente:

1.  Completar los procedimientos descritos en [Configurar todos los servidores](#BKMK_configuringAll).

2.  Usar las instrucciones de los dos procedimientos siguientes para unir los servidores al dominio e iniciar sesión en ellos para realizar más tareas de implementación:

> [!NOTE]
> Para llevar a cabo este procedimiento con Windows PowerShell, abra PowerShell, escriba el siguiente cmdlet y después presione ENTRAR. También debe reemplazar el nombre de dominio por el nombre que quiera usar.
>
> `Add-Computer -DomainName corp.contoso.com`
>
> Cuando se le pida, escriba el nombre de usuario y la contraseña de una cuenta que tenga permiso para unir un equipo al dominio. Para reiniciar el equipo, escriba el siguiente comando y presione ENTRAR.
>
> `Restart-Computer`

###### <a name="to-join-computers-running--windows-server-2016--windows-server-2012-r2--and--windows-server-2012--to-the-domain"></a>Para unir equipos que ejecutan Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012 al dominio

1.  En el Administrador del servidor, haga clic en **Servidor local**. En el panel de detalles, haga clic en **GRUPO DE TRABAJO**. Se abre el cuadro de diálogo **Propiedades del sistema**.

2.  En el cuadro de diálogo **Propiedades del sistema** , haga clic en **Cambiar**. Se abre el cuadro de diálogo **Cambios en el dominio o el nombre del equipo**.

3.  En **Nombre del equipo**, en **Miembro del**, haga clic en **Dominio** y, a continuación, escriba el nombre del dominio al que quiere unirse. Por ejemplo, si el nombre del dominio es corp.contoso.com, escriba **corp.contoso.com**.

4.  Haga clic en **Aceptar**. Se abre el cuadro de diálogo **Seguridad de Windows**.

5.  En **Cambios en el dominio o el nombre del equipo**, escriba el nombre del usuario en **Nombre de usuario** y la contraseña en **Contraseña** y, a continuación, haga clic en **Aceptar**. Se abrirá el cuadro de diálogo **Cambios en el dominio o el nombre del equipo** y le dará la bienvenida al dominio. Haga clic en **Aceptar**.

6.  El cuadro de diálogo **Cambios en el dominio o el nombre del equipo** muestra un mensaje que le indica que debe reiniciar el equipo para aplicar los cambios. Haga clic en **Aceptar**.

7.  En el cuadro de diálogo **Propiedades del sistema**, en la pestaña **Nombre del equipo**, haga clic en **Cerrar**. Se abrirá el cuadro de diálogo **Microsoft Windows**, que muestra un mensaje que indica de nuevo que debe reiniciar el equipo para aplicar los cambios. Haga clic en **Reiniciar ahora**.

> [!NOTE]
> Para obtener información sobre cómo unir los equipos que ejecutan otros sistemas operativos de Microsoft para el dominio, consulte [Apéndice C - unir equipos al dominio](#BKMK_C).

###### <a name="to-log-on-to-the-domain-using-computers-running-windows-server-2016"></a>Para iniciar sesión en el dominio con equipos que ejecutan Windows Server 2016

1.  Cierre la sesión del equipo o reinicie el equipo.

2.  Presione Ctrl+Alt+Supr. Aparecerá la pantalla de inicio de sesión.

3.  En la esquina inferior izquierda, haga clic en **otro usuario**.

4.  En **nombre de usuario**, escriba el nombre de usuario.

5.  En **Contraseña**, escriba la contraseña del dominio y, a continuación, haga clic en la flecha o presione Entrar.

> [!NOTE]
> Para obtener información sobre cómo iniciar sesión en el dominio con equipos que ejecutan otros sistemas operativos de Microsoft, consulte [apéndice D - inicie sesión en el dominio](#BKMK_D).

#### <a name="BKMK_deployDHCP01"></a>Implementar DHCP1
Antes de implementar este componente de la red principal, debe hacer lo siguiente:

-   Completar los pasos de la sección [Configurar todos los servidores](#BKMK_configuringAll)

-   Realizar los pasos de la sección [Unir equipos servidor al dominio e iniciar sesión](#BKMK_joinlogserver)

Para implementar DHCP1, que es el equipo que ejecuta el rol de servidor del Protocolo de configuración dinámica de host (DHCP), debe completar estos pasos en el siguiente orden:

-   [Instalar el protocolo de configuración dinámica de Host (DHCP)](#BKMK_installDHCP)

-   [Crear y activar un nuevo ámbito DHCP](#BKMK_newscopeDHCP)

> [!NOTE]
> Para llevar a cabo estos procedimientos con Windows PowerShell, abra PowerShell, escriba los siguientes cmdlets en líneas separadas y después presione ENTRAR. También debe reemplazar el nombre de ámbito, los intervalos de inicio y finalización de direcciones IP, la máscara de subred y otros valores de este ejemplo por los valores que quiera usar.
>
> `Install-WindowsFeature DHCP -IncludeManagementTools`
>
> `Add-DhcpServerv4Scope -name "Corpnet" -StartRange 10.0.0.1 -EndRange 10.0.0.254 -SubnetMask 255.255.255.0 -State Active`
>
> `Add-DhcpServerv4ExclusionRange -ScopeID 10.0.0.0 -StartRange 10.0.0.1 -EndRange 10.0.0.15`
>
> `Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.0.1 -ScopeID 10.0.0.0 -ComputerName DHCP1.corp.contoso.com`
>
> `Add-DhcpServerv4Scope -name "Corpnet2" -StartRange 10.0.1.1 -EndRange 10.0.1.254 -SubnetMask 255.255.255.0 -State Active`
>
> `Add-DhcpServerv4ExclusionRange -ScopeID 10.0.1.0 -StartRange 10.0.1.1 -EndRange 10.0.1.15`
>
> `Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.1.1 -ScopeID 10.0.1.0 -ComputerName DHCP1.corp.contoso.com`
>
> `Set-DhcpServerv4OptionValue -DnsDomain corp.contoso.com -DnsServer 10.0.0.2`
>
> `Add-DhcpServerInDC -DnsName DHCP1.corp.contoso.com`

##### <a name="BKMK_installDHCP"></a>Instalar el protocolo de configuración dinámica de Host (DHCP)
Puede usar este procedimiento para instalar y configurar el rol de servidor DHCP con el Asistente para agregar roles y características.

El requisito mínimo para llevar a cabo este procedimiento consiste en pertenecer a **Admins. del dominio** o grupo equivalente.

###### <a name="to-install-dhcp"></a>Para instalar DHCP

1.  En DHCP1, en Administrador del servidor, haga clic en **Administrar** y, a continuación, haga clic en **Agregar roles y características**. Se abre el Asistente para agregar roles y características.

2.  En **Antes de comenzar**, haga clic en **Siguiente**.

    > [!NOTE]
    > La página **Antes de comenzar** del Asistente para agregar roles y características no se muestra si ha seleccionado anteriormente **Omitir esta página de forma predeterminada** al ejecutar el asistente.

3.  En **Seleccionar tipo de instalación**, asegúrese de que la opción **Instalación basada en características o en roles** está seleccionada y, a continuación, haga clic en **Siguiente**.

4.  En **Seleccionar servidor de destino**, asegúrese de que la opción **Seleccionar un servidor del grupo de servidores** está seleccionada. En **Grupo de servidores**, asegúrese de que el equipo local está seleccionado. Haz clic en **Siguiente**.

5.  En **seleccionar Roles de servidor**, en **Roles**, seleccione **servidor DHCP**. En **¿Desea agregar características requeridas para servidor DHCP?** , haga clic en **Agregar características**. Haz clic en **Siguiente**.

6.  En **Seleccionar características**, haga clic en **Siguiente**. En **Servidor DHCP**, repase la información proporcionada y haga clic en **Siguiente**.

7.  En **Confirmar selecciones de instalación**, haga clic en **Reiniciar automáticamente el servidor de destino en caso necesario**. Si se le pide confirmar la selección, haga clic en **Sí** y, a continuación, haga clic en **Instalar**. El **progreso de la instalación** página muestra el estado durante el proceso de instalación. Cuando se completa el proceso, el mensaje "requiere configuración. Instalación correcta en *ComputerName*"se muestra, donde *ComputerName* es el nombre del equipo en el que instaló el servidor DHCP. En la ventana del mensaje, haga clic en **Completar configuración de DHCP**. Se abre el Asistente posterior a la instalación de DHCP. Haz clic en **Siguiente**.

8.  En **Autorización**, escriba las credenciales que desea usar para autorizar el servidor DHCP en Active Directory Domain Services y, a continuación, haga clic en **Confirmar**. Cuando se haya completado la autorización, haga clic en **Cerrar**.

##### <a name="BKMK_newscopeDHCP"></a>Crear y activar un nuevo ámbito DHCP
Puede usar este procedimiento para crear un nuevo ámbito DHCP por medio de Microsoft Management Console (MMC) de DHCP. Cuando complete el procedimiento, el ámbito se activa y el intervalo de exclusión que cree impide que el servidor DHCP conceda direcciones IP que se usen para configurar estáticamente los servidores y otros dispositivos que requieren una dirección IP estática.

El requisito mínimo para realizar este procedimiento consiste en pertenecer a **Administradores de DHCP** o grupo equivalente.

###### <a name="to-create-and-activate-a-new-dhcp-scope"></a>Para crear y activar un nuevo ámbito DHCP

1.  En DHCP1, en Administrador del servidor, haga clic en **Herramientas** y, a continuación, haga clic en **DHCP**. Se abre MMC de DHCP.

2.  En **DHCP**, expanda el nombre del servidor. Por ejemplo, si el nombre del servidor DHCP es DHCP1.corp.contoso.com, haga clic en la flecha abajo junto a **DHCP1.corp.contoso.com**.

3.  Bajo el nombre del servidor, haga clic en **IPv4**y, a continuación, haga clic en **nuevo ámbito**. Se abre el Asistente para ámbito nuevo.

4.  En **Éste es el Asistente para ámbito nuevo**, haga clic en **Siguiente**.

5.  En **Nombre de ámbito**, en **Nombre**, escriba un nombre para el ámbito. Por ejemplo, escriba **Subred 1**.

6.  En **Descripción**, escriba una descripción del nuevo ámbito y haga clic en **Siguiente**.

7.  En **Intervalo de direcciones IP**, realice lo siguiente:

    1.  En **Dirección IP inicial**, especifique la primera dirección IP del intervalo. Por ejemplo, escriba **10.0.0.1**.

    2.  En **Dirección IP final**, especifique la última dirección IP del intervalo. Por ejemplo, escriba **10.0.0.254**. Los valores de los campos **Longitud** y **Máscara de subred** se establecen automáticamente a partir de la dirección IP que haya especificado en **Dirección IP inicial**.

    3.  Si fuera necesario, modifique los valores de **Longitud** o **Máscara de subred** para su esquema de direcciones.

    4.  Haz clic en **Siguiente**.

8.  En **Agregar exclusiones**, realice lo siguiente:

    1.  En **Dirección IP inicial**, especifique la primera dirección IP del intervalo de exclusión. Por ejemplo, escriba **10.0.0.1**.

    2.  En **Dirección IP final**, especifique la última dirección IP del intervalo de exclusión. Por ejemplo, escriba **10.0.0.15**.

9. Haga clic en **Agregar** y, a continuación, en **Siguiente**.

10. En **Duración de la concesión**, modifique los valores predeterminados relativos a **Días**, **Horas** y **Minutos** según sea preciso para la red y, a continuación, haga clic en **Siguiente**.

11. En **Configurar opciones DHCP**, seleccione **Configurar estas opciones ahora** y haga clic en **Siguiente**.

12. En **Enrutador (puerta de enlace predeterminada)** , realice una de las acciones siguientes:

    -   Si no dispone de enrutadores en la red, haga clic en **Siguiente**.

    -   En **Dirección IP**, escriba la dirección IP del enrutador o de la puerta de enlace predeterminada. Por ejemplo, escriba **10.0.0.1**. Haga clic en **Agregar** y, a continuación, en **Siguiente**.

13. En **Nombre de dominio y servidores DNS**, realice lo siguiente:

    1.  En **Dominio primario**, escriba el nombre del dominio DNS que los clientes usan para la resolución de nombres. Por ejemplo, escriba **corp.contoso.com**.

    2.  En **Nombre del servidor**, escriba el nombre del equipo DNS que los clientes usan para la resolución de nombres. Por ejemplo, escriba **DC1**.

    3.  Haga clic en **Resolver**. La dirección IP del servidor DNS se agrega a **Dirección IP**. Haga clic en **Agregar**, espere a que finalice la validación de la dirección IP del servidor DNS y, a continuación, haga clic en **Siguiente**.

14. En **Servidores WINS**, haga clic en **Siguiente**, ya que no hay servidores WINS en la red.

15. En **Activar ámbito**, seleccione **Activar este ámbito ahora**.

16. Haga clic en **Siguiente**y después en **Finalizar**.

> [!IMPORTANT]
> Para crear ámbitos para otras subredes, repita este procedimiento. Use un intervalo de direcciones IP diferente por cada subred que desee implementar y asegúrese de que el reenvío de mensajes DHCP está habilitado en todos los enrutadores que llevan a otras subredes.

### <a name="BKMK_joinlogclients"></a>Unir los equipos cliente al dominio e iniciar sesión

> [!NOTE]
> Para llevar a cabo este procedimiento con Windows PowerShell, abra PowerShell, escriba el siguiente cmdlet y después presione ENTRAR. También debe reemplazar el nombre de dominio por el nombre que quiera usar.
>
> `Add-Computer -DomainName corp.contoso.com`
>
> Cuando se le pida, escriba el nombre de usuario y la contraseña de una cuenta que tenga permiso para unir un equipo al dominio. Para reiniciar el equipo, escriba el siguiente comando y presione ENTRAR.
>
> `Restart-Computer`

##### <a name="to-join-computers-running-windows-10-to-the-domain"></a>Para unir equipos que ejecutan Windows 10 para el dominio

1.  Inicie sesión en el equipo con la cuenta de administrador local.

2.  En **buscar en la web y Windows**, tipo **sistema**. En los resultados de búsqueda, haga clic en **(panel de Control) del sistema**. Se abre el cuadro de diálogo **Sistema**.

3.  En **sistema**, haga clic en **configuración avanzada del sistema**. Se abre el cuadro de diálogo **Propiedades del sistema**. Haga clic en el **nombre_equipo** ficha.

4.  En **nombre_equipo**, haga clic en **cambio**. Se abre el cuadro de diálogo **Cambios en el dominio o el nombre del equipo**.

5.  En **cambios de dominio o el nombre de equipo** , en **miembro de**, haga clic en **dominio**y, a continuación, escriba el nombre del dominio que desea combinar. Por ejemplo, si el nombre del dominio es corp.contoso.com, escriba **corp.contoso.com**.

6.  Haga clic en **Aceptar**. Se abre el cuadro de diálogo **Seguridad de Windows**.

7.  En **Cambios en el dominio o el nombre del equipo**, escriba el nombre del usuario en **Nombre de usuario** y la contraseña en **Contraseña** y, a continuación, haga clic en **Aceptar**. Se abrirá el cuadro de diálogo **Cambios en el dominio o el nombre del equipo** y le dará la bienvenida al dominio. Haga clic en **Aceptar**.

8.  El cuadro de diálogo **Cambios en el dominio o el nombre del equipo** muestra un mensaje que le indica que debe reiniciar el equipo para aplicar los cambios. Haga clic en **Aceptar**.

9. En el cuadro de diálogo **Propiedades del sistema**, en la pestaña **Nombre del equipo**, haga clic en **Cerrar**. Se abrirá el cuadro de diálogo **Microsoft Windows**, que muestra un mensaje que indica de nuevo que debe reiniciar el equipo para aplicar los cambios. Haga clic en **Reiniciar ahora**.

##### <a name="to-join-computers-running-windows-81-to-the-domain"></a>Para unir equipos que ejecutan Windows 8.1 al dominio

1.  Inicie sesión en el equipo con la cuenta de administrador local.

2.  Haga clic en **iniciar**y, a continuación, haga clic en **sistema**. Se abre el cuadro de diálogo **Sistema**.

3.  En **sistema**, haga clic en **configuración avanzada del sistema**. Se abre el cuadro de diálogo **Propiedades del sistema**. Haga clic en el **nombre_equipo** ficha.

4.  En **nombre_equipo**, haga clic en **cambio**. Se abre el cuadro de diálogo **Cambios en el dominio o el nombre del equipo**.

5.  En **cambios de dominio o el nombre de equipo** , en **miembro de**, haga clic en **dominio**y, a continuación, escriba el nombre del dominio que desea combinar. Por ejemplo, si el nombre del dominio es corp.contoso.com, escriba **corp.contoso.com**.

6.  Haga clic en **Aceptar**. Se abre el cuadro de diálogo **Seguridad de Windows**.

7.  En **Cambios en el dominio o el nombre del equipo**, escriba el nombre del usuario en **Nombre de usuario** y la contraseña en **Contraseña** y, a continuación, haga clic en **Aceptar**. Se abrirá el cuadro de diálogo **Cambios en el dominio o el nombre del equipo** y le dará la bienvenida al dominio. Haga clic en **Aceptar**.

8.  El cuadro de diálogo **Cambios en el dominio o el nombre del equipo** muestra un mensaje que le indica que debe reiniciar el equipo para aplicar los cambios. Haga clic en **Aceptar**.

9. En el cuadro de diálogo **Propiedades del sistema**, en la pestaña **Nombre del equipo**, haga clic en **Cerrar**. Se abrirá el cuadro de diálogo **Microsoft Windows**, que muestra un mensaje que indica de nuevo que debe reiniciar el equipo para aplicar los cambios. Haga clic en **Reiniciar ahora**.

##### <a name="to-log-on-to-the-domain-using-computers-running-windows-10"></a>Para iniciar sesión en el dominio con equipos que ejecutan Windows 10

1.  Cierre la sesión del equipo o reinicie el equipo.

2.  Presione Ctrl+Alt+Supr. Aparecerá la pantalla de inicio de sesión.

3.  En la parte inferior izquierda, haga clic en **otro usuario**.

4.  En **Nombre de usuario**, escriba su dominio y nombre de usuario con el formato *dominio\usuario*. Por ejemplo, para iniciar sesión en el dominio de corp.contoso.com con una cuenta llamada **Usuario-01**, escriba **CORP\Usuario-01**.

5.  En **Contraseña**, escriba la contraseña del dominio y, a continuación, haga clic en la flecha o presione Entrar.

### <a name="BKMK_optionalfeatures"></a>Implementar características opcionales para la autenticación de acceso de red y servicios Web
Si piensa implementar servidores de acceso de red, como puntos de acceso inalámbrico o servidores VPN, después de instalar la red principal, se recomienda que implementar tanto NPS como un servidor Web. Para las implementaciones de acceso a redes, se recomienda usar métodos de autenticación segura basada en certificados. Puede usar NPS para administrar directivas de acceso a redes e implementar métodos de autenticación segura. Puede usar un servidor web para publicar la lista de revocación de certificados (CRL) de la autoridad de certificación (CA) que suministra los certificados para la autenticación segura.

> [!NOTE]
> Puede implementar certificados de servidor y otras características por medio de las guías de red principal complementarias. Para obtener más información, vea [Recursos técnicos adicionales](#BKMK_resources).

En la siguiente ilustración se muestra la topología de red de Windows Server Core con servidores NPS y Web.

![Topología de red de Windows Server Core con servidores NPS y Web](../media/Core-Network-Guide/cng16_overview_2.jpg)

En las siguientes secciones se proporciona información sobre cómo agregar servidores NPS y web a la red.

-   [Implementar NPS1](#BKMK_deployNPS1)

-   [Implementar WEB1](#BKMK_IIS)

#### <a name="BKMK_deployNPS1"></a>Implementar NPS1
El Servidor de directivas de redes (NPS) se instala como un paso previo a la implementación de otras tecnologías de acceso a redes, como servidores de red privada virtual (VPN), puntos de acceso inalámbrico y conmutadores de autenticación 802.1X.

Servidor de directivas de redes (NPS) le permite configurar y administrar directivas de red con las siguientes características de forma centralizada: Servidor remoto de servicio de autenticación telefónica de usuario (RADIUS) y proxy RADIUS.

NPS es un componente opcional de una red principal, pero conviene instalarlo si se cumple alguna de las siguientes condiciones:

-   Planea ampliar la red para incluir servidores de acceso remoto que son compatibles con el protocolo RADIUS, como un equipo que ejecuta Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008 y Servicio de enrutamiento y acceso remoto, puerta de enlace de Terminal Services o puerta de enlace de escritorio remoto.


-   Planea implementar la autenticación 802.1X para con cable o el acceso inalámbrico.

Antes de implementar este servicio de rol, debe realizar los pasos siguientes en el equipo que está configurando como un NPS.

-   Completar los pasos de la sección [Configurar todos los servidores](#BKMK_configuringAll)

-   Realizar los pasos de la sección [Unir equipos servidor al dominio e iniciar sesión](#BKMK_joinlogserver)

Para implementar NPS1, que es el equipo que ejecuta el servicio de rol de Servidor de directivas de redes (NPS) del rol de servidor de Servicios de acceso y directivas de redes, debe completar estos pasos:

-   [Planear la implementación de NPS1](#bkmk_NetFndtn_Pln_NPS-01)

-   [Instalar el servidor de directivas de redes (NPS)](#BKMK_installNPS)

-   [Registrar el NPS en el dominio predeterminado](#BKMK_registerNPS)

> [!NOTE]
> Esta guía proporciona instrucciones para implementar NPS en un servidor independiente o máquina virtual denominada NPS1.  Otro modelo de implementación recomendada es la instalación de NPS en un controlador de dominio. Si prefiere instalar NPS en un controlador de dominio en lugar de en un servidor independiente, instale NPS en DC1.

##### <a name="bkmk_NetFndtn_Pln_NPS-01"></a>Planear la implementación de NPS1
Si tiene previsto implementar servidores de acceso a la red, como puntos de acceso inalámbrico o servidores VPN, después de implementar la red principal, se recomienda implementar NPS.

Cuando NPS se usa como un servidor RADIUS, se encarga de autenticar y autorizar las solicitudes de conexión a través de los servidores de acceso a la red. NPS también permite configurar y administrar de forma centralizada las directivas de red que determinan quién puede tener acceso a la red, cómo y cuándo.

A continuación se indican los pasos clave de planeación necesarios previos a la instalación de NPS.

- Planear la base de datos de cuentas de usuario. De forma predeterminada, si se une el servidor que ejecuta NPS a un dominio de Active Directory, NPS realiza la autenticación y autorización con la base de datos de cuentas de usuario de AD DS. En algunos casos, como sucede con las redes grandes que usan NPS como un proxy RADIUS para reenviar solicitudes de conexión a otros servidores RADIUS, puede que desee instalar NPS en un equipo que no sea miembro del dominio.

- Planear la contabilización de cuentas RADIUS. NPS permite registrar los datos de la contabilización de cuentas en una base de datos SQL Server o en un archivo de texto en el equipo local. Si desea usar el registro de SQL Server, planee la instalación y configuración del servidor que ejecuta SQL Server.

##### <a name="BKMK_installNPS"></a>Instalar el servidor de directivas de redes (NPS)
Puede usar este procedimiento para instalar el servidor de directivas de redes (NPS) mediante Agregar Roles y características de asistente. NPS es un servicio de rol del rol de servidor Servicios de acceso y directivas de redes.

> [!NOTE]
> De forma predeterminada, NPS escucha el tráfico RADIUS en los puertos 1812, 1813, 1645 y 1646 en todos los adaptadores de red instalados. Si está habilitado Firewall de Windows con seguridad avanzada al instalar NPS, excepciones de firewall para estos puertos se crean automáticamente durante el proceso de instalación para el protocolo de Internet versión 6 \(IPv6\) y tráfico IPv4. Si los servidores de acceso de red están configurados para enviar tráfico RADIUS a través de puertos distintos de los predeterminados, quite las excepciones creadas en el Firewall de Windows con seguridad avanzada durante la instalación de NPS y cree excepciones para los puertos que usa para Tráfico RADIUS.

**Credenciales administrativas**

Para completar este procedimiento, debe ser miembro del grupo **Admins. del dominio**.

> [!NOTE]
> Para llevar a cabo este procedimiento con Windows PowerShell, abra PowerShell, escriba lo siguiente y después presione ENTRAR.
>
> `Install-WindowsFeature NPAS -IncludeManagementTools`

###### <a name="to-install-nps"></a>Para instalar NPS

1.  En NPS1, en Administrador del servidor, haga clic en **Administrar** y, a continuación, haga clic en **Agregar roles y características**. Se abre el Asistente para agregar roles y características.

2.  En **Antes de comenzar**, haga clic en **Siguiente**.

    > [!NOTE]
    > La página **Antes de comenzar** del Asistente para agregar roles y características no se muestra si ha seleccionado anteriormente **Omitir esta página de forma predeterminada** al ejecutar el asistente.

3.  En **Seleccionar tipo de instalación**, asegúrese de que la opción **Instalación basada en características o en roles** está seleccionada y, a continuación, haga clic en **Siguiente**.

4.  En **Seleccionar servidor de destino**, asegúrese de que la opción **Seleccionar un servidor del grupo de servidores** está seleccionada. En **Grupo de servidores**, asegúrese de que el equipo local está seleccionado. Haz clic en **Siguiente**.

5.  En **seleccionar Roles de servidor**, en **Roles**, seleccione **servicios de acceso y directivas de redes**. Se abre un cuadro de diálogo que pregunta si deben agregar las características necesarias para servicios de acceso y directivas de redes. Haz clic en **Agregar características requeridas**y, a continuación, haz clic en **Siguiente**.

6.  En **Seleccionar características**, haga clic en **Siguiente**. En **Servicios de acceso y directivas de redes**, repase la información proporcionada y haga clic en **Siguiente**.

7.  En **Seleccionar servicios de rol**, haga clic en **Servidor de directivas de redes**.  En **¿Desea agregar características requeridas para Servidor de directivas de redes?** , haga clic en **Agregar características**. Haz clic en **Siguiente**.

8.  En **Confirmar selecciones de instalación**, haga clic en **Reiniciar automáticamente el servidor de destino en caso necesario**. Si se le pide confirmar la selección, haga clic en **Sí** y, a continuación, haga clic en **Instalar**. La página Progreso de la instalación muestra el estado durante el proceso de instalación. Cuando se completa el proceso, el mensaje "instalación correcta en *ComputerName*" se muestra, donde *ComputerName* es el nombre del equipo en el que instaló el servidor de directivas de red. Haga clic en **Cerrar**.

##### <a name="BKMK_registerNPS"></a>Registrar el NPS en el dominio predeterminado
Puede usar este procedimiento para registrar un NPS en el dominio donde el servidor es miembro del dominio.

NPSs deben estar registrados en Active Directory para que tengan permiso para leer las propiedades de marcado de cuentas de usuario durante el proceso de autorización. Registrar un NPS agrega el servidor a la **servidores RAS e IAS** grupo en Active Directory.

**Credenciales administrativas**

Para completar este procedimiento, debe ser miembro del grupo **Admins. del dominio**.

> [!NOTE]
> Para llevar a cabo este procedimiento mediante el uso de comandos de shell (Netsh) de red en Windows PowerShell, abra PowerShell y escriba lo siguiente y, a continuación, presione ENTRAR.
>
> `netsh nps add registeredserver domain=corp.contoso.com server=NPS1.corp.contoso.com`

###### <a name="to-register-an-nps-in-its-default-domain"></a>Para registrar un NPS en el dominio predeterminado

1.  En NPS1, en Administrador del servidor, haga clic en Herramientas y a continuación, haga clic en **Servidor de directivas de redes**. Se abre MMC del Servidor de directivas de redes.

2.  Haga clic con el botón secundario en **NPS (local)** y, a continuación, haga clic en **Registrar servidor en Active Directory**. Se abrirá el cuadro de diálogo **Servidor de directivas de redes**.

3.  En **Servidor de directivas de redes**, haga clic en **Aceptar** y, a continuación, en **Aceptar** de nuevo.

Para obtener más información sobre el servidor de directivas de red, consulte [servidor de directivas de redes (NPS)](../technologies/nps/nps-top.md).

#### <a name="BKMK_IIS"></a>Implementar WEB1

El rol de servidor Web (IIS) en Windows Server 2016 proporciona una plataforma segura, fácil de administrar, modular y extensible para hospedar sitios web, servicios y aplicaciones de forma confiable. Con Internet Information Services (IIS), puede compartir información con usuarios en Internet, una intranet o extranet. IIS es una plataforma web unificada que integra IIS, ASP.NET, servicios FTP, PHP y Windows Communication Foundation (WCF).

Además de lo que le permite publicar una CRL para el acceso mediante equipos miembros del dominio, el rol de servidor servidor Web (IIS) permite configurar y administrar varios sitios web, aplicaciones web y sitios FTP. IIS también proporciona las siguientes ventajas:

-   La seguridad web se refuerza gracias a una superficie reducida de servidor y al aislamiento automático de aplicaciones.

-   Podrá implementar y ejecutar aplicaciones web de ASP.NET, ASP clásico y PHP en el mismo servidor de forma sencilla.

-   Se logra el aislamiento de aplicaciones al proporcionar a los procesos de trabajo una identidad única y una configuración en espacio aislado de manera predeterminada, lo que reduce aún más los riesgos de seguridad.

-   Podrá agregar y eliminar componentes IIS integrados e incluso reemplazarlos fácilmente por módulos personalizados que se adapten a las necesidades del cliente.

-   Aumenta la velocidad del sitio web mediante el almacenamiento en caché dinámico integrado y la compresión mejorada.

Para implementar WEB1, que es el equipo que ejecuta el rol Servidor web (IIS), debe hacer lo siguiente:

-   Completar los pasos de la sección [Configurar todos los servidores](#BKMK_configuringAll)

-   Realizar los pasos de la sección [Unir equipos servidor al dominio e iniciar sesión](#BKMK_joinlogserver)

-   [Instalar el rol de servidor servidor Web (IIS)](#BKMK_install_IIS)

##### <a name="BKMK_install_IIS"></a>Instalar el rol de servidor servidor Web (IIS)
Para completar este procedimiento, debe pertenecer al grupo **Administradores**.

> [!NOTE]
> Para llevar a cabo este procedimiento con Windows PowerShell, abra PowerShell, escriba lo siguiente y después presione ENTRAR.
>
> `Install-WindowsFeature Web-Server -IncludeManagementTools`

1.  En el **Administrador del servidor**, haz clic en **Administrar** y después haz clic en **Agregar roles y características**. Se abre el Asistente para agregar roles y características.

2.  En **Antes de comenzar**, haga clic en **Siguiente**.

    > [!NOTE]
    > La página **Antes de comenzar** del Asistente para agregar roles y características no se muestra si ha seleccionado anteriormente **Omitir esta página de forma predeterminada** al ejecutar el asistente.

3.  En el **Seleccionar tipo de instalación** página, haga clic en **siguiente**.

4.  En el **Seleccionar servidor de destino** página, asegúrese de que el equipo local está seleccionado y, a continuación, haga clic en **siguiente**.

5.  En el **seleccionar roles de servidor** página, desplácese y seleccione **servidor Web (IIS)** . El **agregar características requeridas para servidor Web (IIS)** abre el cuadro de diálogo. Haz clic en **Agregar características requeridas**y, a continuación, haz clic en **Siguiente**.

6.  Haga clic en **Siguiente** hasta haber aceptado todas las configuraciones predeterminadas del servidor web y, a continuación, haga clic en **Instalar**.

7.  Compruebe que todas las instalaciones se realizaron correctamente y, a continuación, haga clic en **Cerrar**.

## <a name="BKMK_resources"></a>Recursos técnicos adicionales
Para obtener más información sobre las tecnologías de esta guía, vea los siguientes recursos:

 Recursos de biblioteca técnica de Windows Server 2012, Windows Server 2012 R2 y Windows Server 2016

-   [Novedades de servicios de dominio de Active Directory (AD DS) en Windows Server 2016](https://technet.microsoft.com/library/mt163897.aspx)

-   [Introducción a los servicios de dominio de Active Directory](https://technet.microsoft.com/library/hh831484.aspx) en https://technet.microsoft.com/library/hh831484.aspx.

-   [Información general del sistema de nombres (DNS) de dominio](https://technet.microsoft.com/library/hh831667.aspx) en https://technet.microsoft.com/library/hh831667.aspx.

-   [Implementar el rol de administradores de DNS](https://technet.microsoft.com/library/cc756152(WS.10).aspx)

-   [Información general de protocolo de configuración de Host (DHCP) dinámico](https://technet.microsoft.com/library/hh831825.aspx) en https://technet.microsoft.com/library/hh831825.aspx.

-   [Información general sobre servicios de acceso y directivas de redes](https://technet.microsoft.com/library/hh831683.aspx) en https://technet.microsoft.com/library/hh831683.aspx.

-   [Introducción a Web Server (IIS)](https://technet.microsoft.com/library/hh831725.aspx) en https://technet.microsoft.com/library/hh831725.aspx.

## <a name="BKMK_appendix"></a>Apéndices A E
Las secciones siguientes contienen información de configuración adicional para los equipos que ejecutan sistemas operativos distintos de Windows Server 2016, Windows 10, Windows Server 2012 y Windows 8. Además, una hoja de cálculo de preparación de red se proporciona para ayudarle con la implementación.

1.  [Apéndice A - cambiar el nombre de los equipos](#BKMK_A)

2.  [Apéndice B - configurar IP estática de direcciones](#BKMK_B)

3.  [Apéndice C - unir equipos al dominio](#BKMK_C)

4.  [Apéndice D - inicie sesión en el dominio](#BKMK_D)

5.  [Apéndice E - hoja de preparación de planeamiento de red principales](#BKMK_E)

## <a name="BKMK_A"></a>Apéndice A - cambiar el nombre de los equipos
Puede usar los procedimientos de esta sección para proporcionar a los equipos que ejecutan Windows Server 2008 R2, Windows 7, Windows Server 2008 y Windows Vista con un nombre de equipo diferente.

-   [Windows Server 2008 R2 y Windows 7](#bkmk_NetFndtn_Pln_rename_R2)

-   [Windows Server 2008 y Windows Vista](#bkmk_NetFndtn_Pln_Renam08)

### <a name="bkmk_NetFndtn_Pln_rename_R2"></a>Windows Server 2008 R2 y Windows 7
El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.

##### <a name="to-rename-computers-running-windows-server-2008-r2-and-windows-7"></a>Para cambiar el nombre de los equipos que ejecutan Windows Server 2008 R2 y Windows 7

1.  Haga clic en **Inicio**, haga clic con el botón secundario en **Equipo**y después haga clic en **Propiedades**. Se abre el cuadro de diálogo **Sistema**.

2.  En **Configuración de nombre, dominio y grupo de trabajo del equipo**, haga clic en **Cambiar configuración**. Se abre el cuadro de diálogo **Propiedades del sistema**.

    > [!NOTE]
    > En los equipos que ejecutan Windows 7, antes del **las propiedades del sistema** abre el cuadro de diálogo, el **User Account Control** abre el cuadro de diálogo que solicita permiso para continuar. Haga clic en **Continuar** para seguir.

3.  Haga clic en **Cambiar**. Se abre el cuadro de diálogo **Cambios en el dominio o el nombre del equipo**.

4.  En **Nombre de equipo**, escriba el nombre del equipo. Por ejemplo, si desea asignarle el nombre DC1, escriba **DC1**.

5.  Haga clic dos veces en **Aceptar**, haga clic en **Cerrar** y, a continuación, haga clic en **Reiniciar ahora** para reiniciar el equipo.

### <a name="bkmk_NetFndtn_Pln_Renam08"></a>Windows Server 2008 y Windows Vista
El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.

##### <a name="to-rename-computers-running-windows-server-2008-and-windows-vista"></a>Para cambiar el nombre de los equipos que ejecutan Windows Server 2008 y Windows Vista

1.  Haga clic en **Inicio**, haga clic con el botón secundario en **Equipo**y después haga clic en **Propiedades**. Se abre el cuadro de diálogo **Sistema**.

2.  En **Configuración de nombre, dominio y grupo de trabajo del equipo**, haga clic en **Cambiar configuración**. Se abre el cuadro de diálogo **Propiedades del sistema**.

    > [!NOTE]
    > En los equipos que ejecutan Windows Vista, antes del **las propiedades del sistema** abre el cuadro de diálogo, el **User Account Control** abre el cuadro de diálogo que solicita permiso para continuar. Haga clic en **Continuar** para seguir.

3.  Haga clic en **Cambiar**. Se abre el cuadro de diálogo **Cambios en el dominio o el nombre del equipo**.

4.  En **Nombre de equipo**, escriba el nombre del equipo. Por ejemplo, si desea asignarle el nombre DC1, escriba **DC1**.

5.  Haga clic dos veces en **Aceptar**, haga clic en **Cerrar** y, a continuación, haga clic en **Reiniciar ahora** para reiniciar el equipo.

## <a name="BKMK_B"></a>Apéndice B - configurar IP estática de direcciones
En este tema se incluyen los procedimientos necesarios para configurar direcciones IP estáticas en equipos que ejecutan los siguientes sistemas operativos:

-   [Windows Server 2008 R2](#bkmk_R2Cng_WS08R2IP)

-   [Windows Server 2008](#bkmk_NetFndtn_Pln_CfgStatic08)

### <a name="bkmk_R2Cng_WS08R2IP"></a>Windows Server 2008 R2
El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.

##### <a name="to-configure-a-static-ip-address-on-a-computer-running-windows-server-2008-r2"></a>Para configurar una dirección IP estática en un equipo que ejecuta Windows Server 2008 R2

1.  Haga clic en **Inicio** y, a continuación, en **Panel de control**.

2.  En el **Panel de control**, haga clic en **Red e Internet**. Se abre **Red e Internet**.

    En **Red e Internet**, haga clic en **Centro de redes y recursos compartidos**. Se abre **Centro de redes y recursos compartidos**.

3.  En **Centro de redes y recursos compartidos**, haga clic en **Cambiar configuración del adaptador**. Se abre **Conexiones de red**.

4.  En **Conexiones de red**, haga clic con el botón secundario en la conexión de red que desea configurar y, a continuación, haga clic en **Propiedades**.

5.  En **Propiedades de conexión de área local**, en **Esta conexión usa los siguientes elementos**, seleccione **Protocolo de Internet versión 4 (TCP/IPv4)** y, a continuación, haga clic en **Propiedades**. Se abre el cuadro de diálogo **Propiedades de protocolo de Internet versión 4 (TCP/IPv4)** .

6.  En **Propiedades de protocolo de Internet versión 4 (TCP/IPv4)** , en la pestaña **General**, haga clic en **Usar la siguiente dirección IP**. En **Dirección IP**, escriba la dirección IP que desea usar.

7.  Presione el tabulador para colocar el cursor en **Máscara de subred**. Se escribe automáticamente un valor predeterminado para la máscara de subred. Acepte la máscara de subred predeterminada o escriba la máscara de subred que quiera usar.

8.  En **Puerta de enlace predeterminada**, escriba la dirección IP de la puerta de enlace predeterminada.

9. En **Servidor DNS preferido**, escriba la dirección IP del servidor DNS. Si tiene previsto usar el equipo local como servidor DNS preferido, escriba la dirección IP de ese equipo.

10. En **Servidor DNS alternativo**, escriba la dirección IP del servidor DNS alternativo si lo hay. Si tiene previsto usar el equipo local como servidor DNS alternativo, escriba la dirección IP de ese equipo.

11. Haga clic en **Aceptar** y, a continuación, en **Cerrar**.

### <a name="bkmk_NetFndtn_Pln_CfgStatic08"></a>Windows Server 2008
El requisito mínimo para realizar este procedimiento es la pertenencia al grupo **Administradores** o grupo equivalente.

##### <a name="to-configure-a-static-ip-address-on-a-computer-running-windows-server-2008"></a>Para configurar una dirección IP estática en un equipo que ejecuta Windows Server 2008

1.  Haga clic en **Inicio** y, a continuación, en **Panel de control**.

2.  En el **Panel de control**, compruebe que la opción **Vista Clásica** está seleccionada y, a continuación, haga doble clic en **Centro de redes y recursos compartidos**.

3.  En **Centro de redes y recursos compartidos**, en **Tareas**, haga clic en **Administrar conexiones de red**.

4.  En **Conexiones de red**, haga clic con el botón secundario en la conexión de red que desea configurar y, a continuación, haga clic en **Propiedades**.

5.  En **Propiedades de conexión de área local**, en **Esta conexión usa los siguientes elementos**, seleccione **Protocolo de Internet versión 4 (TCP/IPv4)** y, a continuación, haga clic en **Propiedades**. Se abre el cuadro de diálogo **Propiedades de protocolo de Internet versión 4 (TCP/IPv4)** .

6.  En **Propiedades de protocolo de Internet versión 4 (TCP/IPv4)** , en la pestaña **General**, haga clic en **Usar la siguiente dirección IP**. En **Dirección IP**, escriba la dirección IP que desea usar.

7.  Presione el tabulador para colocar el cursor en **Máscara de subred**. Se escribe automáticamente un valor predeterminado para la máscara de subred. Acepte la máscara de subred predeterminada o escriba la máscara de subred que quiera usar.

8.  En **Puerta de enlace predeterminada**, escriba la dirección IP de la puerta de enlace predeterminada.

9. En **Servidor DNS preferido**, escriba la dirección IP del servidor DNS. Si tiene previsto usar el equipo local como servidor DNS preferido, escriba la dirección IP de ese equipo.

10. En **Servidor DNS alternativo**, escriba la dirección IP del servidor DNS alternativo si lo hay. Si tiene previsto usar el equipo local como servidor DNS alternativo, escriba la dirección IP de ese equipo.

11. Haga clic en **Aceptar** y, a continuación, en **Cerrar**.

## <a name="BKMK_C"></a>Apéndice C - unir equipos al dominio
Puede usar estos procedimientos para unir equipos que ejecutan Windows Server 2008 R2, Windows 7, Windows Server 2008 y Windows Vista al dominio.

-   [Windows Server 2008 R2 y Windows 7](#BKMK_c1)

-   [Windows Server 2008 y Windows Vista](#BKMK_c2)

> [!IMPORTANT]
> Para unir un equipo a un dominio, debe iniciar sesión en el equipo con la cuenta de administrador local o, si inicia sesión en el equipo con una cuenta de usuario que no tiene credenciales administrativas en el equipo local, debe proporcionar las credenciales para la cuenta de administrador local durante el proceso de unión del equipo al dominio. Además, debe tener una cuenta de usuario en el dominio al que quiere unir el equipo. Durante el proceso de unión del equipo al dominio, se le pedirán las credenciales de su cuenta de dominio (nombre de usuario y contraseña).

### <a name="BKMK_c1"></a>Windows Server 2008 R2 y Windows 7
El requisito mínimo para completar este procedimiento es la pertenencia al grupo **Usuarios del dominio** o grupo equivalente.

##### <a name="to-join-computers-running-windows-server-2008-r2-and-windows-7-to-the-domain"></a>Para unir equipos que ejecutan Windows Server 2008 R2 y Windows 7 al dominio

1.  Inicie sesión en el equipo con la cuenta de administrador local.

2.  Haga clic en **Inicio**, haga clic con el botón secundario en **Equipo**y después haga clic en **Propiedades**. Se abre el cuadro de diálogo **Sistema**.

3.  En **Configuración de nombre, dominio y grupo de trabajo del equipo**, haga clic en **Cambiar configuración**. Se abre el cuadro de diálogo **Propiedades del sistema**.

    > [!NOTE]
    > En los equipos que ejecutan Windows 7, antes del **las propiedades del sistema** abre el cuadro de diálogo, el **User Account Control** abre el cuadro de diálogo que solicita permiso para continuar. Haga clic en **Continuar** para seguir.

4.  Haga clic en **Cambiar**. Se abre el cuadro de diálogo **Cambios en el dominio o el nombre del equipo**.

5.  En **Nombre del equipo**, en **Miembro del**, seleccione **Dominio** y, a continuación, escriba el nombre del dominio al que quiere unirse. Por ejemplo, si el nombre del dominio es corp.contoso.com, escriba **corp.contoso.com**.

6.  Haga clic en **Aceptar**. Se abre el cuadro de diálogo **Seguridad de Windows**.

7.  En **Cambios en el dominio o el nombre del equipo**, escriba el nombre del usuario en **Nombre de usuario** y la contraseña en **Contraseña** y, a continuación, haga clic en **Aceptar**. Se abrirá el cuadro de diálogo **Cambios en el dominio o el nombre del equipo** y le dará la bienvenida al dominio. Haga clic en **Aceptar**.

8.  El cuadro de diálogo **Cambios en el dominio o el nombre del equipo** muestra un mensaje que le indica que debe reiniciar el equipo para aplicar los cambios. Haga clic en **Aceptar**.

9. En el cuadro de diálogo **Propiedades del sistema**, en la pestaña **Nombre del equipo**, haga clic en **Cerrar**. Se abrirá el cuadro de diálogo **Microsoft Windows**, que muestra un mensaje que indica de nuevo que debe reiniciar el equipo para aplicar los cambios. Haga clic en **Reiniciar ahora**.

### <a name="BKMK_c2"></a>Windows Server 2008 y Windows Vista
El requisito mínimo para completar este procedimiento es la pertenencia al grupo **Usuarios del dominio** o grupo equivalente.

##### <a name="to-join-computers-running-windows-server-2008-and-windows-vista-to-the-domain"></a>Para unir equipos que ejecutan Windows Server 2008 y Windows Vista al dominio

1.  Inicie sesión en el equipo con la cuenta de administrador local.

2.  Haga clic en **Inicio**, haga clic con el botón secundario en **Equipo**y después haga clic en **Propiedades**. Se abre el cuadro de diálogo **Sistema**.

3.  En **Configuración de nombre, dominio y grupo de trabajo del equipo**, haga clic en **Cambiar configuración**. Se abre el cuadro de diálogo **Propiedades del sistema**.

4.  Haga clic en **Cambiar**. Se abre el cuadro de diálogo **Cambios en el dominio o el nombre del equipo**.

5.  En **Nombre del equipo**, en **Miembro del**, seleccione **Dominio** y, a continuación, escriba el nombre del dominio al que quiere unirse. Por ejemplo, si el nombre del dominio es corp.contoso.com, escriba **corp.contoso.com**.

6.  Haga clic en **Aceptar**. Se abre el cuadro de diálogo **Seguridad de Windows**.

7.  En **Cambios en el dominio o el nombre del equipo**, escriba el nombre del usuario en **Nombre de usuario** y la contraseña en **Contraseña** y, a continuación, haga clic en **Aceptar**. Se abrirá el cuadro de diálogo **Cambios en el dominio o el nombre del equipo** y le dará la bienvenida al dominio. Haga clic en **Aceptar**.

8.  El cuadro de diálogo **Cambios en el dominio o el nombre del equipo** muestra un mensaje que le indica que debe reiniciar el equipo para aplicar los cambios. Haga clic en **Aceptar**.

9. En el cuadro de diálogo **Propiedades del sistema**, en la pestaña **Nombre del equipo**, haga clic en **Cerrar**. Se abrirá el cuadro de diálogo **Microsoft Windows**, que muestra un mensaje que indica de nuevo que debe reiniciar el equipo para aplicar los cambios. Haga clic en **Reiniciar ahora**.

## <a name="BKMK_D"></a>Apéndice D - inicie sesión en el dominio
Puede usar estos procedimientos para iniciar sesión en el dominio con equipos que ejecutan Windows Server 2008 R2, Windows 7, Windows Server 2008 y Windows Vista.

-   [Windows Server 2008 R2 y Windows 7](#BKMK_d1)

-   [Windows Server 2008 y Windows Vista](#BKMK_d2)

### <a name="BKMK_d1"></a>Windows Server 2008 R2 y Windows 7
El requisito mínimo para completar este procedimiento es la pertenencia al grupo **Usuarios del dominio** o grupo equivalente.

##### <a name="log-on-to-the-domain-using-computers-running-windows-server-2008-r2-and-windows-7"></a>Inicie sesión en el dominio con equipos que ejecutan Windows Server 2008 R2 y Windows 7

1.  Cierre la sesión del equipo o reinicie el equipo.

2.  Presione Ctrl+Alt+Supr. Aparecerá la pantalla de inicio de sesión.

3.  Haga clic en **Cambiar de usuario** y, a continuación, haga clic en **Otro usuario**.

4.  En **Nombre de usuario**, escriba su dominio y nombre de usuario con el formato *dominio\usuario*. Por ejemplo, para iniciar sesión en el dominio de corp.contoso.com con una cuenta llamada **Usuario-01**, escriba **CORP\Usuario-01**.

5.  En **Contraseña**, escriba la contraseña del dominio y, a continuación, haga clic en la flecha o presione Entrar.

### <a name="BKMK_d2"></a>Windows Server 2008 y Windows Vista
El requisito mínimo para completar este procedimiento es la pertenencia al grupo **Usuarios del dominio** o grupo equivalente.

##### <a name="log-on-to-the-domain-using-computers-running-windows-server-2008-and-windows-vista"></a>Inicie sesión en el dominio con equipos que ejecutan Windows Server 2008 y Windows Vista

1.  Cierre la sesión del equipo o reinicie el equipo.

2.  Presione Ctrl+Alt+Supr. Aparecerá la pantalla de inicio de sesión.

3.  Haga clic en **Cambiar de usuario** y, a continuación, haga clic en **Otro usuario**.

4.  En **Nombre de usuario**, escriba su dominio y nombre de usuario con el formato *dominio\usuario*. Por ejemplo, para iniciar sesión en el dominio de corp.contoso.com con una cuenta llamada **Usuario-01**, escriba **CORP\Usuario-01**.

5.  En **Contraseña**, escriba la contraseña del dominio y, a continuación, haga clic en la flecha o presione Entrar.

## <a name="BKMK_E"></a>Apéndice E - hoja de preparación de planeamiento de red principales
Puede usar esta Hoja de preparación de planeamiento de red para recopilar la información necesaria para instalar una red principal. En este tema se proporcionan tablas que contienen los elementos de configuración individuales de cada equipo servidor para el que se debe suministrar información o valores específicos durante la instalación o el proceso de configuración. Se incluyen valores de ejemplo para cada elemento de configuración.

De cara a posibles tareas de planeamiento y seguimiento, también se proporcionan espacios en cada tabla donde se pueden especificar los valores usados para la implementación. Si se registran valores relacionados con la seguridad en estas tablas, se debe almacenar la información en una ubicación segura.

Los siguientes vínculos llevan a las secciones de este tema que proporcionan elementos de configuración y valores de ejemplo asociados con los procedimientos de implementación presentados en esta guía.

1.  [Instalar servicios de dominio de Active Directory y DNS](#BKMK_FndtnPrep_InstallAD)

    -   [Configurar una zona de búsqueda inversa DNS](#BKMK_FndtnPrep_DNSRevrsLook)

2.  [Instalación de DHCP](#BKMK_FndtnPrep_InstallDHCP)

    -   [Creación de un intervalo de exclusión en DHCP](#BKMK_FndtnPrep_DHCP_Exclusn)

    -   [Crear un nuevo ámbito DHCP](#bkmk_NetFndtn_Pln_DHCP_NewScope)

3.  [Instalar el servidor de directivas de red (opcional)](#BKMK_FndtnPrep_InstallNPS)

### <a name="BKMK_FndtnPrep_InstallAD"></a>Instalar servicios de dominio de Active Directory y DNS
Las tablas en esta sección enumeran los elementos de configuración para la preinstalación e instalación de servicios de dominio de Active Directory (AD DS) y DNS.

##### <a name="pre-installation-configuration-items-for-ad-ds-and-dns"></a>Elementos de configuración de la preinstalación para AD DS y DNS
Las siguientes tablas recogen elementos de configuración de la preinstalación tal y como se describen en [Configurar todos los servidores](#BKMK_configuringAll):

-   [Configurar una dirección IP estática](#BKMK_ip)

|Elementos de configuración|Valores de ejemplo|Valores|
|-----------------------|------------------|----------|
|Dirección IP|10.0.0.2||
|Máscara de subred|255.255.255.0||
|Puerta de enlace prefijada|10.0.0.1||
|Servidor DNS preferido|127.0.0.1||
|Servidor DNS alternativo|10.0.0.15||

-   [El nombre del equipo](#BKMK_rename)

|Elemento de configuración|Valor de ejemplo|Valor|
|----------------------|-----------------|---------|
|Nombre de equipo|DC1||

##### <a name="ad-ds-and-dns-installation-configuration-items"></a>Elementos de configuración de instalación de AD DS y DNS
Elementos de configuración para el procedimiento de implementación de una red principal de Windows Server [Instalar AD DS y DNS para un bosque nuevo](#BKMK_installAD-DNS):

|Elementos de configuración|Valores de ejemplo|Valores|
|-----------------------|------------------|----------|
|Nombre DNS completo|corp.contoso.com||
|Nivel funcional del bosque|Windows Server 2003||
|Ubicación de la carpeta de bases de datos de Active Directory Domain Services|E:\Configuration\\<br /><br />O bien, acepte la ubicación predeterminada.||
|Ubicación de la carpeta de archivos de registro de Active Directory Domain Services|E:\Configuration\\<br /><br />O bien, acepte la ubicación predeterminada.||
|Ubicación de la carpeta SYSVOL de Active Directory Domain Services|E:\Configuration\\<br /><br />O bien, acepte la ubicación predeterminada.||
|Contraseña de administrador para el modo de restauración de directorios|J*p2leO4$F||
|Nombre del archivo de respuesta (opcional)|AD DS_AnswerFile||

#### <a name="BKMK_FndtnPrep_DNSRevrsLook"></a>Configurar una zona de búsqueda inversa DNS

|Elementos de configuración|Valores de ejemplo|Valores|
|-----------------------|------------------|----------|
|Tipo de zona:|-Zona principal<br />-Zona secundaria<br />: Zona de rutas internas||
|Tipo de zona<br /><br />**Store de la zona en Active Directory**|-Seleccionado<br />-No seleccionado||
|Ámbito de replicación de zona de Active Directory|-Para todos los servidores DNS en este bosque<br />-Para todos los servidores DNS en este dominio<br />-En todos los controladores de dominio en este dominio<br />-Para todos los controladores de dominio especificados en el ámbito de esta partición de directorio||
|Nombre de zona de búsqueda inversa<br /><br />(tipo de IP)|: Zona de búsqueda inversa IPv4<br />: Zona de búsqueda inversa IPv6||
|Nombre de zona de búsqueda inversa<br /><br />(identificador de red)|10.0.0||

### <a name="BKMK_FndtnPrep_InstallDHCP"></a>Instalación de DHCP
Las tablas de esta sección muestran elementos de configuración para la preinstalación e instalación de DHCP.

##### <a name="pre-installation-configuration-items-for-dhcp"></a>Elementos de configuración de la preinstalación para DHCP
Las siguientes tablas recogen elementos de configuración de la preinstalación tal y como se describen en [Configurar todos los servidores](#BKMK_configuringAll):

-   [Configurar una dirección IP estática](#BKMK_ip)

|Elementos de configuración|Valores de ejemplo|Valores|
|-----------------------|------------------|----------|
|Dirección IP|10.0.0.3||
|Máscara de subred|255.255.255.0||
|Puerta de enlace prefijada|10.0.0.1||
|Servidor DNS preferido|10.0.0.2||
|Servidor DNS alternativo|10.0.0.15||

-   [El nombre del equipo](#BKMK_rename)

|Elemento de configuración|Valor de ejemplo|Valor|
|----------------------|-----------------|---------|
|Nombre de equipo|DHCP1||

##### <a name="dhcp-installation-configuration-items"></a>Elementos de configuración de la instalación de DHCP
Elementos de configuración para el procedimiento de implementación de una red principal de Windows Server [Instalar Protocolo de configuración dinámica de host (DHCP)](#BKMK_installDHCP):

|Elementos de configuración|Valores de ejemplo|Valores|
|-----------------------|------------------|----------|
|Enlaces de conexión de red|Ethernet||
|Configuración del servidor DNS|DC1||
|Dirección IP del servidor DNS preferido|10.0.0.2||
|Dirección IP del servidor DNS alternativo|10.0.0.15||
|Nombre de ámbito|Corp1||
|Dirección IP inicial|10.0.0.1||
|Dirección IP final|10.0.0.254||
|Máscara de subred|255.255.255.0||
|Puerta de enlace predeterminada (opcional)|10.0.0.1||
|Duración de la concesión|8 días||
|Modo de funcionamiento del servidor DHCP IPv6|No habilitado||

#### <a name="BKMK_FndtnPrep_DHCP_Exclusn"></a>Creación de un intervalo de exclusión en DHCP
Elementos de configuración para crear un intervalo de exclusión al crear un ámbito en DHCP.

|Elementos de configuración|Valores de ejemplo|Valores|
|-----------------------|------------------|----------|
|Nombre de ámbito|Corp1||
|Descripción del ámbito|Subred 1 de la oficina principal||
|Dirección IP inicial del intervalo de exclusión|10.0.0.1||
|Dirección IP final del intervalo de exclusión|10.0.0.15||

#### <a name="bkmk_NetFndtn_Pln_DHCP_NewScope"></a>Crear un nuevo ámbito DHCP
Elementos de configuración para el procedimiento de implementación de Windows Server Core [Crear y activar un nuevo ámbito DHCP](#BKMK_newscopeDHCP):

|Elementos de configuración|Valores de ejemplo|Valores|
|-----------------------|------------------|----------|
|Nombre del nuevo ámbito|Corp2||
|Descripción del ámbito|Subred de la oficina central 2||
|(intervalo de direcciones IP)<br /><br />Dirección IP inicial|10.0.1.1||
|(intervalo de direcciones IP)<br /><br />Dirección IP final|10.0.1.254||
|Duración|8||
|Máscara de subred|255.255.255.0||
|(Intervalo de exclusión) Dirección IP inicial|10.0.1.1||
|Dirección IP final del intervalo de exclusión|10.0.1.15||
|Duración de la concesión<br /><br />Days<br /><br />Horas<br /><br />Minutos|-   8<br />-   0<br />-   0||
|Enrutador (puerta de enlace predeterminada)<br /><br />Dirección IP|10.0.1.1||
|Dominio DNS principal|corp.contoso.com||
|Servidor DNS<br /><br />Dirección IP|10.0.0.2||

### <a name="BKMK_FndtnPrep_InstallNPS"></a>Instalar el servidor de directivas de red (opcional)
Las tablas de esta sección muestran elementos de configuración para la preinstalación e instalación de NPS.

##### <a name="pre-installation-configuration-items"></a>Elementos de configuración de la preinstalación
Las tres tablas siguientes muestran elementos de configuración de la preinstalación tal y como se describen en [Configurar todos los servidores](#BKMK_configuringAll):

-   [Configurar una dirección IP estática](#BKMK_ip)

|Elementos de configuración|Valores de ejemplo|Valores|
|-----------------------|------------------|----------|
|Dirección IP|10.0.0.4||
|Máscara de subred|255.255.255.0||
|Puerta de enlace prefijada|10.0.0.1||
|Servidor DNS preferido|10.0.0.2||
|Servidor DNS alternativo|10.0.0.15||

-   [El nombre del equipo](#BKMK_rename)

|Elemento de configuración|Valor de ejemplo|Valor|
|----------------------|-----------------|---------|
|Nombre de equipo|NPS1||

##### <a name="network-policy-server-installation-configuration-items"></a>Elementos de configuración de la instalación del Servidor de directivas de redes
Elementos de configuración para los procedimientos de implementación de Windows Server Core redes NPS [instalar servidor de directivas de redes (NPS)](#BKMK_installNPS) y [registrar NPS en el dominio predeterminado](#BKMK_registerNPS).

-   No se requieren más elementos de configuración para instalar y registrar NPS.

