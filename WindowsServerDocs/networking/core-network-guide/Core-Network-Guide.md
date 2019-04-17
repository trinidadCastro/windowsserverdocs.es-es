---
title: Guía básica de red
description: Esta guía brinda instrucciones sobre cómo planear e implementar los componentes básicos necesarios para una red totalmente funcional y un nuevo dominio de Active Directory en un bosque nuevo con Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: b3cd60f7-d380-4712-9a78-0a8f551e1121
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 90390a7b975bc358fb96715d23fe542bc261220e
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="core-network-guide"></a>Guía básica de red

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Esta guía brinda instrucciones sobre cómo planear e implementar los componentes básicos necesarios para una red totalmente funcional y un nuevo dominio de Active Directory en un bosque nuevo.

> [!NOTE]
> Esta guía está disponible para descarga en formato de Microsoft Word desde la Galería de TechNet. Para obtener más información, consulta [Core red guía para Windows Server 2016](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683).

Esta guía contiene las siguientes secciones.

- [Acerca de esta guía](#BKMK_about)

- [Introducción a la red Core](#BKMK_overview)

- [Diseño de la red principal](#BKMK_planning)

- [Implementación de red principal](#BKMK_deployment)

- [Recursos técnicos adicionales](#BKMK_resources)

- [Apéndices A hasta E](#BKMK_appendix)

## <a name="BKMK_about"></a>Acerca de esta guía
Esta guía está diseñada para los administradores de red y del sistema que está instalando una nueva red o que quieran crear una red de dominio para reemplazar a una red que consta de grupos de trabajo. El escenario de implementación que se proporcionan en esta guía es especialmente útil si prevé la necesidad de agregar más características y servicios a la red en el futuro.

Se recomienda que revise el diseño y Guía de implementación para cada una de las tecnologías utilizadas en este escenario de implementación que le ayudarán a determinar si esta guía proporciona los servicios y la configuración que necesitas.

Una red principal es una colección de hardware de red, dispositivos, y necesidades de software que proporciona los servicios fundamentales para la tecnología de la organización de la información (TI).

Una red de Windows Server core proporciona muchas ventajas, incluidas las siguientes acciones.

-   Protocolos principales para la conectividad de red entre equipos y otros dispositivos compatibles con el protocolo o a Internet Protocol (TCP). TCP/IP es un conjunto de protocolos estándar para conectar equipos y crear redes. TCP/IP es un software de protocolo de red incluido con sistemas operativos Microsoft Windows que implementa y admite el conjunto de protocolos de TCP/IP.

-   Asignación dinámica de protocolo de configuración de Host (DHCP) automática IP direcciones a equipos y otros dispositivos que están configurados como clientes DHCP. Configuración manual de direcciones IP en todos los equipos de la red es lenta y menos flexible que proporcionar dinámicamente equipos y otros dispositivos con configuraciones de direcciones IP mediante un servidor DHCP.

-   Servicio de resolución de nombres de sistema de nombres de dominio (DNS). DNS permite a los usuarios, equipos, aplicaciones y servicios buscar las direcciones IP de equipos y dispositivos en la red mediante el nombre de dominio completo del equipo o dispositivo.

-   Un bosque, que es uno o varios dominios de Active Directory que comparten la misma clase definiciones y atributos (esquema), información del sitio y de replicación (configuración) y capacidades de búsqueda para todo el bosque (catálogo global).

-   Crea un dominio raíz del bosque, que es el primer dominio en un bosque nuevo. Los grupos Administradores de empresa y administradores de esquema, que son grupos administrativos de todo el bosque, se encuentran en el dominio raíz. Además, un dominio raíz del bosque, al igual que con otros dominios, es una colección de equipo, los usuarios y los objetos de grupo que están definidos por el Administrador de servicios de dominio de Active Directory (AD DS). Estos objetos comparten un directivas de seguridad y la base de datos de directorio comunes. También pueden compartir relaciones de seguridad con otros dominios si agregar dominios a medida que crece la organización. El servicio de directorio también almacena datos del directorio y permite que los equipos autorizados, aplicaciones y los usuarios acceder a los datos.

-   Un usuario y el equipo cuenta base de datos. El servicio de directorio proporciona una base de datos de cuentas de usuario centralizadas que te permite crear cuentas de usuario y del equipo para usuarios y equipos que están autorizados a conectarse a la red y la red de acceso a recursos, como aplicaciones, bases de datos, archivos compartidos y carpetas, impresoras y.

Una red principal también te permite ampliar la red que la organización crece y cambios en los requisitos de TI. Por ejemplo, con una red principal puede agregar dominios, subredes IP, servicios de acceso remoto, servicios inalámbricos y otras características y los roles de servidor proporcionados por Windows Server 2016.

### <a name="network-hardware-requirements"></a>Requisitos de hardware de red
Para implementar correctamente una red principal, debes implementar hardware de red, incluidos los siguientes:

-   Ethernet, Fast Ethernet o cableado Ethernet GB

-   Un concentrador, nivel 2 o 3 conmutador, enrutador u otro dispositivo que realiza la función de transmisión de tráfico de red entre equipos y dispositivos.

-   Equipos que cumplen los requisitos mínimos de hardware para sus respectivos sistemas operativos de cliente y servidor.

## <a name="what-this-guide-does-not-provide"></a>Lo que no proporciona esta guía
Esta guía no proporciona instrucciones para implementar las siguientes acciones:

-   Hardware de red, como cableado, enrutadores, conmutadores y hubs

-   Recursos de red adicionales, como impresoras y servidores de archivos

-   Conectividad a Internet

-   Acceso remoto

-   Acceso inalámbrico

-   Implementación en equipos cliente

> [!NOTE]
> Equipos con sistemas operativos de cliente de Windows están configurados para recibir concesiones de direcciones IP del servidor DHCP de manera predeterminada. Por lo tanto, no adicionales DHCP o protocolo de Internet versión 4 (IPv4) se requiere la configuración de los equipos cliente.

## <a name="technology-overviews"></a>Información general de la tecnología
Las secciones siguientes proporcionan breve introducción a las tecnologías requiere que se implementan para crear una red principal.

### <a name="active-directory-domain-services"></a>Servicios de dominio de Active Directory
Un directorio es una estructura jerárquica que almacena información sobre los objetos de la red, como los usuarios y equipos. Un servicio de directorio, por ejemplo, AD DS, proporciona los métodos para almacenar datos del directorio y hacer que estos datos estén disponibles para los administradores y usuarios de la red. Por ejemplo, AD DS almacena información sobre las cuentas de usuario, incluidos nombres, direcciones de correo electrónico, las contraseñas y números de teléfono y permite que otros usuarios autorizados en la misma red acceder a esta información.

### <a name="dns"></a>DNS
DNS es un protocolo de resolución de nombre para las redes TCP/IP, como Internet o red de una organización. Un servidor DNS hospeda la información que permite a los equipos cliente y servicios para resolver fácilmente reconoce, nombres DNS alfanuméricos a las direcciones IP que usan los equipos se comuniquen entre sí.

### <a name="dhcp"></a>DHCP
DHCP es un estándar IP para simplificar la administración de configuración IP del host. El estándar DHCP permite el uso de servidores DHCP como una forma de administrar la asignación dinámica de direcciones IP y otros detalles de configuración para los clientes DHCP en la red.

DHCP te permite usar un servidor DHCP para asignar una dirección IP de dinámicamente en un equipo u otro dispositivo, como una impresora, en la red local. Todos los equipos en una red TCP/IP deben tener una dirección IP única, porque la dirección IP y su máscara de subred relacionada identifican el equipo host y la subred a la que está conectado el equipo. Mediante el uso de DHCP, puedes asegurarte de que todos los equipos que están configurados como clientes DHCP reciban una dirección IP que sea apropiada para su ubicación de red y la subred y mediante opciones de DHCP, como la puerta de enlace predeterminada y los servidores DNS, puede proporcionar automáticamente a los clientes DHCP con la información que necesitan para funcionar correctamente en la red.

Redes TCP/IP, DHCP reduce la complejidad y la cantidad de trabajo administrativa para reconfigurar los equipos.

### <a name="tcpip"></a>TCP/IP
TCP/IP en Windows Server 2016 es la siguiente:

-   En función de los protocolos de redes estándar del sector de software de red.

-   Protocolo de red de empresa pueden enrutar que admite la conexión de un equipo basado en Windows para la red de área local (LAN) y entornos de área extensa (WAN) de la red.

-   Tecnologías fundamentales y utilidades para conectar un equipo basado en Windows con diferentes sistemas con el fin de compartir información.

-   Una base para obtener acceso a los servicios de Internet globales, como los servidores de World Wide Web y el protocolo de transferencia de archivos (FTP).

-   Un marco sólida, escalable y plataformas de cliente y servidor.

TCP/IP proporciona utilidades de TCP/IP básicas que permiten a los equipos basados en Windows conectar y compartir información con otros Microsoft y sistemas no son de Microsoft, incluidos:

-    Windows Server 2016

-   Windows 10

-    Windows Server 2012 R2

-   Windows 8.1

-    Windows Server 2012

-   Windows 8

-    Windows Server 2008 R2

-    Windows 7

-    Windows Server 2008

-   Windows Vista

-   Hosts de Internet

-   Sistemas de Apple Macintosh

-   Grandes de sistemas de IBM

-   Sistemas de UNIX y Linux

-   Sistemas abiertos de máquinas virtuales

-   Impresoras de red

-   Tabletas y teléfonos móviles con Ethernet por cable o inalámbrica tecnología 802.11 habilitado

## <a name="BKMK_overview"></a>Introducción a la red Core
La siguiente ilustración muestra la topología de red de Windows Server Core.

![Topología de red de Windows Server Core](../media/Core-Network-Guide/cng16_overview.jpg)

> [!NOTE]
> Esta guía también incluye instrucciones para agregar servidores opcionales de servidor de directivas de redes (NPS) y el servidor Web (IIS) a la topología de red para proporcionar la base para las soluciones de acceso de red segura, como 802.1X por cable e inalámbricas implementaciones que se pueden implementar con guías de complemento de red principal. Para obtener más información, consulta [implementar características opcionales para la autenticación de acceso de red y servicios Web](#BKMK_optionalfeatures).

### <a name="core-network-components"></a>Componentes de la red principal
Siguiente es los componentes de una red principal.

##### <a name="router"></a>Enrutador
Esta guía de implementación proporciona instrucciones para la implementación de una red principal con dos subredes separadas por un enrutador que tenga habilitado el reenvío de DHCP. Sin embargo, puedes implementar un conmutador de nivel 2, un conmutador de nivel 3 o a un concentrador, según los requisitos y recursos. Si implementas un modificador, el modificador debe ser capaces de reenvío de DHCP o debe colocar un servidor DHCP en cada subred. Si implementas un concentrador, puedes implementación una única subred y no es necesario el reenvío de DHCP o un segundo ámbito en el servidor DHCP.

##### <a name="static-tcpip-configurations"></a>Configuraciones de TCP/IP estáticas
Los servidores de esta implementación están configurados con direcciones IPv4 estáticas. Los equipos cliente se configuran de forma predeterminada para recibir concesiones de direcciones IP del servidor DHCP.

##### <a name="active-directory-domain-services-global-catalog-and-dns-server-dc1"></a>Catálogo global de Active Directory los servicios de dominio y el servidor DNS DC1
Tanto los servicios de dominio de Active Directory (AD DS) y el sistema de nombres de dominio (DNS) están instalados en este servidor, denominado DC1, que proporciona el directorio y el nombre de los servicios de resolución para todos los equipos y dispositivos de la red.

##### <a name="dhcp-server-dhcp1"></a>Servidor DHCP DHCP1
El servidor DHCP, denominado DHCP1, está configurado con un ámbito que proporciona concesiones de direcciones de protocolo de Internet (IP) a los equipos de la subred local. El servidor DHCP también puede configurarse con ámbitos adicionales para proporcionar concesiones de direcciones IP a los equipos en otras subredes si está configurado el reenvío de DHCP en los enrutadores.

##### <a name="client-computers"></a>Equipos cliente
Equipos que ejecutan sistemas operativos de cliente de Windows se configuran de forma predeterminada como los clientes DHCP, lo que Obtén direcciones IP y opciones de DHCP automáticamente desde el servidor DHCP.

## <a name="BKMK_planning"></a>Diseño de la red principal
Antes de implementar una red principal, debes planear los siguientes elementos.

-   [Planeación de subredes](#bkmk_NetFndtn_Pln_Subnt)

-   [Planeación de la configuración básica de todos los servidores](#bkmk_NetFndtn_Pln_AllSrvrs)

-   [Planear la implementación de DC1](#bkmk_NetFndtn_Pln_AD-DNS-01)

-   [Planeación de acceso de dominio](#bkmk_NetFndtn_Pln_DomAccess)

-   [Planear la implementación de DHCP1](#bkmk_NetFndtn_Pln_DHCP-01)

Las siguientes secciones proporcionan más detalles sobre cada uno de estos elementos.

> [!NOTE]
> Para obtener ayuda con planear la implementación, consulte también [Apéndice E - hoja de preparación de planeamiento de red de Core](#BKMK_E).

### <a name="bkmk_NetFndtn_Pln_Subnt"></a>Planeación de subredes
En las redes de protocolo o a Internet Protocol (TCP), los enrutadores se usan para interconexión el hardware y software que se usa en segmentos de red física diferentes llamados subredes. Enrutadores también se usan para reenviar paquetes IP entre cada una de las subredes. Determinar el diseño físico de la red, incluido el número de enrutadores y subredes que necesitas, antes de continuar con las instrucciones que aparecen en esta guía.

Además, para configurar los servidores de la red con direcciones IP estáticas, debes determinar el intervalo de direcciones IP que desea usar para la subred donde se encuentran los servidores de red principal. En esta guía, se usan los intervalos de direcciones IP privados 10.0.0.1 - 10.0.0.254 y 10.0.1.1 - 10.0.1.254 como ejemplos, pero puedes usar cualquier intervalo de direcciones IP privadas que prefieras.

> [!IMPORTANT]
> Después de seleccionar los intervalos de direcciones IP que desea usar para cada subred, asegúrate de que configures los enrutadores con una dirección IP desde el mismo intervalo de direcciones IP que se utiliza en la subred donde está instalado el enrutador. Por ejemplo, si el enrutador está configurado con una dirección IP de 192.168.1.1 de manera predeterminada, pero vas a instalar el enrutador en una subred con un intervalo de direcciones IP de 10.0.0.0/24, debe volver a configurar el enrutador para usar una dirección IP del intervalo de direcciones IP 10.0.0.0/24.

Dirección IP privada intervalos están especificados por Internet solicitud de comentarios (RFC) 1918 reconocen las siguientes acciones:

-   10.0.0.0 - 10.255.255.255

-   172.16.0.0 - 172.31.255.255

-   192.168.0.0 - 192.168.255.255

Cuando usas los intervalos de direcciones IP privados como se especifica en RFC 1918, no se puede conectar directamente a Internet mediante una dirección IP privada porque las solicitudes que se va a o desde estas direcciones automáticamente se descartan enrutadores de proveedor de servicio de Internet. Para agregar la conectividad a Internet a la red principal más adelante, debe contrato con un ISP para obtener una dirección IP pública.

> [!IMPORTANT]
> Cuando uses direcciones IP privadas, debes usar algún tipo de servidor de traducción (NAT) de dirección de proxy o de red para convertir los intervalos de direcciones IP privados de la red local a una dirección IP pública que se puede distribuir en Internet. La mayoría de los enrutadores proporcionan servicios NAT, para seleccionar un enrutador que sea capaz de NAT será bastante simple.

Para obtener más información, consulta [planear la implementación de DHCP1](#bkmk_NetFndtn_Pln_DHCP-01).

### <a name="bkmk_NetFndtn_Pln_AllSrvrs"></a>Planeación de la configuración básica de todos los servidores
Para cada servidor en la red principal, debes cambiar el nombre del equipo y asignar y configurar una dirección IPv4 estática y otras propiedades de TCP/IP para el equipo.

#### <a name="planning-naming-conventions-for-computers-and-devices"></a>Planeación de convenciones de nomenclatura para equipos y dispositivos
Por motivos de coherencia en toda la red, es una buena idea usar nombres coherentes para otros dispositivos, impresoras y servidores. Nombres de equipo pueden usarse para ayudar a los usuarios y administradores identifican fácilmente el propósito y la ubicación del servidor, impresora u otro dispositivo. Por ejemplo, si tienes tres servidores DNS, uno de San Francisco, uno de Los Ángeles y otro de Chicago, puede usar la convención de nomenclatura *función server*-*ubicación*-*número*:

-   DNS-DEN-01. Este nombre representa el servidor DNS de Denver, Colorado. Si se agregan más servidores DNS Maria al trabajo, puede incrementar el valor numérico en el nombre, como en DNS-DEN-02 y DNS-DEN-03.

-   DNS-BALNEARIOS-01. Este nombre representa el servidor DNS en Pasadena sur, California.

-   DNS-ORL-01. Este nombre representa el servidor DNS en Orlando, Florida.

Esta guía, la convención de nomenclatura de servidor es muy sencilla y consta de la función de servidor principal y un número. Por ejemplo, el controlador de dominio se denomina DC1 y el servidor DHCP se denomina DHCP1.

Se recomienda que elijas una convención de nomenclatura antes de instalar la red principal uso de esta guía.

#### <a name="planning-static-ip-addresses"></a>Planeación de direcciones IP estáticas
Antes de configurar cada equipo con una dirección IP estática, debes planear tu subredes y los intervalos de direcciones IP. Además, debes determinar las direcciones IP de los servidores DNS. Si vas a instalar un enrutador que proporciona acceso a otras redes, como subredes adicionales o a Internet, debes conocer la dirección IP del enrutador, que también se denomina una puerta de enlace predeterminada para la configuración de dirección IP estática.

La siguiente tabla proporciona ejemplos de valores de configuración de dirección IP estática.

|Elementos de configuración|Valores de ejemplo|
|-----------------------|------------------|
|Dirección IP|10.0.0.2|
|Máscara de subred|255.255.255.0|
|Puerta de enlace predeterminada (dirección IP del enrutador)|10.0.0.1|
|Servidor DNS preferido|10.0.0.2|

> [!NOTE]
> Si va a implementar más de un servidor DNS, también puede planificar la dirección IP del servidor DNS alternativo.

### <a name="bkmk_NetFndtn_Pln_AD-DNS-01"></a>Planear la implementación de DC1
Siguiente es los pasos de programación claves antes de instalar los servicios de dominio de Active Directory (AD DS) y DNS en DC1.

#### <a name="planning-the-name-of-the-forest-root-domain"></a>Planear el nombre de dominio raíz del bosque
Un primer paso en el proceso de diseño de AD DS es determinar cuántos bosques requiere de la organización. Un bosque es el contenedor de AD DS de nivel superior y consta de uno o varios dominios que comparten un esquema común y un catálogo global. Una organización puede tener varios bosques, pero para la mayoría de las organizaciones, un diseño de bosque único es el modelo preferido y el método más fácil de administrar.

Cuando se crea el primer controlador de dominio en la organización, vas a crear el primer dominio (también denominado dominio raíz del bosque) y el primer bosque. Sin embargo, antes de realizar esta acción con esta guía, debes determinar el mejor nombre de dominio para la organización. En la mayoría de los casos, el nombre de la organización se usa como el nombre de dominio y en muchos casos se registra este nombre de dominio. Si vas a implementar externo basados en Internet servidores Web para proporcionar información y servicios para los clientes o asociados, elige un nombre de dominio que ya no está en uso y, a continuación, registrar el nombre de dominio para que lo posee tu organización.

#### <a name="planning-the-forest-functional-level"></a>Planear el nivel funcional del bosque
Durante la instalación de AD DS, debes elegir el nivel funcional del bosque que quieras usar. Funcionalidad de bosque y dominio, introducida en Active Directory de Windows Server 2003, ofrece una forma para habilitar las características de Active Directory en el entorno de red de dominio o bosque todo. Distintos niveles de funcionalidad de dominio y de bosque están disponibles, en función del entorno.

La funcionalidad de bosque habilita características en todos los dominios del bosque. Los siguientes niveles funcionales de bosque están disponibles:

-    Windows Server 2008. Este nivel funcional del bosque admite solo los controladores de dominio que ejecutan Windows Server 2008 y versiones posteriores del sistema operativo Windows Server.

-    Windows Server 2008 R2. Este nivel funcional del bosque admite controladores de dominio de Windows Server 2008 R2 y controladores de dominio que ejecutan versiones posteriores del sistema operativo Windows Server.

-    Windows Server 2012. Este nivel funcional del bosque admite controladores de dominio de Windows Server 2012 y controladores de dominio que ejecutan versiones posteriores del sistema operativo Windows Server.

-    Windows Server 2012 R2. Este nivel funcional del bosque admite controladores de dominio de Windows Server 2012 R2 y controladores de dominio que ejecutan versiones posteriores del sistema operativo Windows Server.

-    Windows Server 2016. Este nivel funcional del bosque admite solo los controladores de dominio de Windows Server 2016 y los controladores de dominio que ejecutan versiones posteriores del sistema operativo Windows Server.

Si vas a implementar un nuevo dominio en un bosque nuevo y todos los controladores de dominio se ejecutará Windows Server 2016, se recomienda configurar AD DS con el nivel funcional del bosque de Windows Server 2016 durante la instalación de AD DS.

> [!IMPORTANT]
> Después de que se genera el nivel funcional del bosque, los controladores de dominio que ejecutan sistemas operativos anteriores no pueden incorporarse del bosque. Por ejemplo, si eleva el nivel funcional del bosque a Windows Server 2016, no se puede agregar al bosque de controladores de dominio que ejecutan Windows Server 2012 R2 o Windows Server 2008.

En la siguiente tabla, se proporcionan los elementos de configuración de ejemplo de AD DS.

|Elementos de configuración:|Valores de ejemplo:|
|------------------------|-------------------|
|Nombre completo de DNS|Ejemplos:<br /><br />-corp.contoso.com<br />-example.com|
|Nivel funcional del bosque|-Windows Server 2008 <br />-Windows Server 2008 R2 <br />-Windows Server 2012 <br />-Windows Server 2012 R2 <br />-Windows Server 2016|
|Ubicación de carpeta de base de datos de servicios de dominio de directorio activo|E:\Configuration\\<br /><br />O bien, acepte la ubicación predeterminada.|
|Ubicación de carpeta de archivos de registro de servicios de dominio de Active Directory|E:\Configuration\\<br /><br />O bien, acepte la ubicación predeterminada.|
|Ubicación de la carpeta SYSVOL de los servicios de dominio de directorio activo|E:\Configuration\\<br /><br />O acepte la ubicación predeterminada|
|Contraseña de administrador del modo de restauración de directorio|**J\ * p2leO4$ F**|
|Nombre de archivo de respuesta (opcional)|**AD DS_AnswerFile**|

#### <a name="planning-dns-zones"></a>Planeación de zonas DNS
En los servidores DNS principales, integrada en Active Directory, se crea una zona de búsqueda hacia adelante de forma predeterminada durante la instalación del rol de servidor DNS. Equipos y dispositivos para consultar de otro equipo o del dispositivo dirección IP en función de su nombre DNS, permite que una zona de búsqueda hacia delante. Además de una zona de búsqueda hacia adelante, se recomienda que crees una zona de búsqueda inversa de DNS. Con un DNS búsqueda inversa de consulta, un equipo o dispositivo puede detectar el nombre de otro equipo o dispositivo con su dirección IP. Implementación de una zona de búsqueda inversa normalmente mejora el rendimiento de DNS y aumenta el éxito de las consultas DNS.

Cuando se crea una zona de búsqueda inversa, el dominio in-addr.arpa, que se define en los estándares DNS y reservado en el espacio de nombres de Internet DNS para proporcionar una forma práctica y confiable para realizar consultas inversas, se configura en DNS. Para crear el espacio de nombres inverso, los subdominios dentro del dominio in-addr.arpa se crean, con el orden inverso de los números de la notación decimal con puntos de direcciones IP.

El dominio in-addr.arpa se aplica a todas las redes TCP/IP que se basan en el protocolo de Internet versión 4 (IPv4) direccionamiento. El Asistente para nueva zona automáticamente se da por hecho que estás usando este dominio cuando se crea una zona de búsqueda inversa.

Mientras se ejecuta al Asistente para nueva zona, se recomiendan las siguientes opciones:

|Elementos de configuración|Valores de ejemplo|
|-----------------------|------------------|
|Tipo de zona|**Zona principal**, y **almacenar la zona en Active Directory** está activada|
|Ámbito de replicación de zona de Active Directory|**Para todos los servidores DNS en este dominio**|
|Página del Asistente de nombre de la zona de búsqueda inversa de primera|**Zona de búsqueda inversa de IPv4**|
|Página del Asistente de zona de búsqueda inversa de segundo nombre|Identificador de red = 10.0.0.|
|Actualizaciones dinámicas|**Permitir solo las actualizaciones dinámicas seguras**|

### <a name="bkmk_NetFndtn_Pln_DomAccess"></a>Planeación de acceso de dominio
Para iniciar sesión en el dominio, el equipo debe ser un equipo miembro del dominio y la cuenta de usuario debe crearse en AD DS antes del intento de inicio de sesión.

> [!NOTE]
> Los equipos individuales que ejecutan Windows tienen un valor local base de datos que se llama a la base de datos de cuentas de usuario de administrador de cuentas de seguridad (SAM) de la cuenta de usuario de grupos y los usuarios. Cuando creas una cuenta de usuario en el equipo local en la base de datos de SAM, puedes iniciar sesión en el equipo local, pero no puede iniciar sesión a un dominio. Cuentas de usuario de dominio se crean con los usuarios de Active Directory y equipos de Microsoft Management Console (MMC) en un controlador de dominio, no con los usuarios locales y grupos en el equipo local.

Después de la primera correcta de inicio de sesión con credenciales de inicio de sesión de dominio, la configuración de inicio de sesión persiste a menos que el equipo se quite del dominio o cambia manualmente la configuración de inicio de sesión.

Antes inicies sesión en el dominio:

-   Crear cuentas de usuario en equipos y usuarios de Active Directory. Cada usuario debe tener una cuenta de usuario de los servicios de dominio de Active Directory en equipos y usuarios de Active Directory. Para obtener más información, consulta [crear una cuenta de usuario en equipos y usuarios de Active Directory](#BKMK_createUA).

-   Asegúrate de la configuración correcta de dirección IP. Para unirte a un equipo al dominio, el equipo debe tener una dirección IP. En esta guía, están configurados con direcciones IP estáticas y los equipos cliente reciban concesiones de direcciones IP del servidor DHCP. Por este motivo, el servidor DHCP debe implementarse antes de unir a los clientes al dominio. Para obtener más información, consulta [implementar DHCP1](#BKMK_deployDHCP01).

-   Unir el equipo al dominio. Cualquier equipo que proporciona u obtiene acceso a recursos de red debe estar unido al dominio. Para obtener más información, consulta [equipos de servidor unirse al dominio y de registro en](#BKMK_joinlogserver) y [unirse a los equipos cliente del dominio y el registro en](#BKMK_joinlogclients).

### <a name="bkmk_NetFndtn_Pln_DHCP-01"></a>Planear la implementación de DHCP1
Siguiente es los pasos de programación claves antes de instalar el rol de servidor DHCP en DHCP1.

#### <a name="planning-dhcp-servers-and-dhcp-forwarding"></a>Planeación de servidores DHCP y reenvío de DHCP
Dado que los mensajes de difusión mensajes DHCP, no se reenvían entre subredes enrutadores. Si tienes varias subredes y desea proporcionar el servicio DHCP para cada subred, debe hacer lo siguiente:

-   Instalar a un servidor DHCP en cada subred

-   Configurar los enrutadores para reenviar los mensajes de difusión DHCP en subredes y configurar varios ámbitos en el servidor DHCP, un ámbito por cada subred.

En la mayoría de los casos, la configuración de enrutadores para reenviar mensajes de difusión DHCP es más rentable que la implementación de un servidor DHCP en cada segmento físico de la red.

#### <a name="planning-ip-address-ranges"></a>Planeación de intervalos de direcciones IP
Cada subred debe tener su propio intervalo de direcciones IP único. Estos intervalos se representan en un servidor DHCP con ámbitos.

Un ámbito es una agrupación administrativa de direcciones IP para equipos de una subred que usan el servicio DHCP. El administrador crea primero un ámbito para cada subred física y, a continuación, usa el ámbito para definir los parámetros usados por los clientes.

Un ámbito tiene las siguientes propiedades:

-   Un intervalo de direcciones IP desde el que se incluya o excluya direcciones usadas para ofertas de concesión de servicio DHCP.

-   Máscara de subred, que determina el prefijo de subred para una dirección IP especificada.

-   Un nombre de ámbito que se asigna cuando se crea.

-   Valores de duración de concesión, que se asignan a los clientes DHCP que reciban asignado de manera dinámica direcciones IP.

-   Las opciones de ámbito DHCP configuradas para la asignación a los clientes DHCP, como la dirección IP del servidor DNS y dirección IP de puerta de enlace de enrutador de forma predeterminada.

-   Opcionalmente, se usan las reservas para garantizar que un cliente DHCP reciba siempre la misma dirección IP.

Antes de implementar los servidores de la lista de las subredes y el intervalo de direcciones IP que quieras usar para cada subred.

#### <a name="planning-subnet-masks"></a>Planeación de máscaras
Identificadores de red y de host en una dirección IP se distinguen mediante una máscara de subred. Cada máscara de subred es un número de 32 bits que se usan los grupos de bits consecutivos de todos los (1) para identificar la red Id. y cero (0) para identificar las partes del identificador de host de una dirección IP.

Por ejemplo, la máscara de subred que normalmente se utiliza con la dirección IP 131.107.16.200 es el siguiente número binario de 32 bits:

```
11111111 11111111 00000000 00000000
```

Este número de máscara de subred es 16 bits uno seguidos de 16 bits cero, que indica que las secciones de Id. de identificador y host de red de esta dirección IP son 16 bits de longitud. Normalmente, esta máscara de subred se muestra en notación decimal como 255.255.0.0.

La siguiente tabla muestra las máscaras de subred para las clases de direcciones de Internet.

|Clase de dirección|Bits de máscara de subred|Máscara de subred|
|-----------------|------------------------|---------------|
|Clase A|11111111 00000000 00000000 00000000|255.0.0.0|
|Clase B|11111111 11111111 00000000 00000000|255.255.0.0|
|Clase C|11111111 11111111 11111111 00000000|255.255.255.0|

Cuando creas un ámbito en DHCP y especificar el intervalo de direcciones IP para el ámbito, DHCP proporciona estos valores de máscara de subred de forma predeterminada. Por lo general, valores de máscara de subred predeterminados son aceptables para la mayoría de las redes sin requisitos especiales y donde cada segmento de red IP corresponde a una sola red física.

En algunos casos, puedes usar máscaras de subred personalizadas para implementar subredes IP. Con las subredes IP, puede subdivide la parte del identificador de host predeterminado de una dirección IP para especificar subredes que son subdivisiones de identificador de red basada en la clase original.

Al personalizar la longitud de máscara de subred, puedes reducir el número de bits que se usan para el Id.

Para evitar problemas de direcciones y enrutamiento, debes asegurarte de que todos los equipos de TCP/IP en un segmento de red utilizan la misma máscara de subred y que cada equipo o dispositivo tiene una dirección IP única.

#### <a name="planning-exclusion-ranges"></a>Planeación de intervalos de exclusión
Cuando creas un ámbito en un servidor DHCP, especifican un intervalo de direcciones IP que incluye todas las direcciones IP que el servidor DHCP puede conceder a los clientes DHCP, como equipos y otros dispositivos. Si, a continuación, ir y configurar manualmente algunos servidores y direcciones IP de otros dispositivos con estático desde el mismo intervalo de direcciones IP que está usando el servidor DHCP, puedes crear accidentalmente un conflicto de dirección IP, donde puedes y el servidor DHCP tienen les asigna la misma dirección IP a diferentes dispositivos.

Para resolver este problema, puedes crear un intervalo de exclusión para el ámbito DHCP. Un intervalo de exclusión es un contiguos intervalo de direcciones IP dentro del intervalo de direcciones IP del ámbito que el servidor DHCP no está permitido usar. Si creas un intervalo de exclusión, el servidor DHCP no asigna las direcciones en ese intervalo, que le permite asignar manualmente estas direcciones sin tener que crear un conflicto de dirección IP.

Puede excluir direcciones IP de distribución por el servidor DHCP mediante la creación de un intervalo de exclusión para cada ámbito. Debes usar las exclusiones para todos los dispositivos que están configurados con una dirección IP estática. Las direcciones excluidas deben incluir todas las direcciones IP que se asignen manualmente a otros servidores, los clientes no DHCP, estaciones de trabajo sin disco o clientes de enrutamiento y acceso remoto o de PPP.

Se recomienda que configures el intervalo de exclusión con las direcciones adicionales para acomodar el crecimiento futura de la red. La siguiente tabla proporciona un intervalo de exclusión de ejemplo para un ámbito con un intervalo de direcciones IP de 10.0.0.1 - 10.0.0.254 y una máscara de subred de 255.255.255.0.

|Elementos de configuración|Valores de ejemplo|
|-----------------------|------------------|
|Dirección IP de inicio del intervalo de exclusión|10.0.0.1|
|Dirección IP final del intervalo de exclusión|10.0.0.25|

#### <a name="planning-tcpip-static-configuration"></a>Planeación de la configuración de TCP/IP estática
Algunos dispositivos, como los enrutadores, los servidores DHCP y servidores DNS, deben estar configurados con una dirección IP estática. Además, puede que dispositivos adicionales, como impresoras, lo que quieres asegurarte de que siempre tienen la misma dirección IP. Enumerar los dispositivos que desea configurar estáticamente para cada subred y, a continuación, planear el intervalo de exclusión que quieras usar en el servidor DHCP para asegurarse de que el servidor DHCP no conceder a la dirección IP de un dispositivo configurado estáticamente. Un intervalo de exclusión es una secuencia limitada de direcciones IP en un ámbito, excluidas de las ofertas de servicio DHCP. Los intervalos de exclusión debe asegurarse de que el servidor no ofrece todas las direcciones de estos intervalos a los clientes DHCP en la red.

Por ejemplo, si el intervalo de direcciones IP para una subred es 192.168.0.1a través de 192.168.0.254 y tienes diez dispositivos que desea configurar con una dirección IP estática, puedes crear un intervalo de exclusión para la 192.168.0. *x* ámbito que incluya diez o más direcciones IP: 192.168.0.1a través de 192.168.0.15.

En este ejemplo, usan diez de las direcciones IP excluidas para configurar servidores y otros dispositivos con direcciones IP estáticas y cinco direcciones IP adicionales se encuentran disponibles para una configuración estática de nuevos dispositivos que desea agregar en el futuro. Con este intervalo de exclusión, el servidor DHCP quedo con un conjunto de direcciones de 192.168.0.16a través de 192.168.0.254.

En la siguiente tabla, se proporcionan los elementos de configuración de ejemplo adicional de AD DS y DNS.

|Elementos de configuración|Valores de ejemplo|
|-----------------------|------------------|
|Enlaces de conexión de red|Ethernet|
|Configuración del servidor DNS|DC1.corp.contoso.com|
|Dirección IP del servidor DNS preferido|10.0.0.2|
|Agregar valores de cuadro de diálogo de ámbito<br /><br />1. Nombre del ámbito de<br />2. A partir de la dirección IP<br />3. La dirección IP final<br />4. Máscara de subred de<br />5. (opcional) de la puerta de enlace predeterminada de<br />6. Duración de la concesión de|1. Principal subred<br />2.  10.0.0.1<br />3.  10.0.0.254<br />4.  255.255.255.0<br />5.  10.0.0.1<br />6. 8 días|
|Modo de operación de servidor DHCP IPv6|No está habilitado|

## <a name="BKMK_deployment"></a>Implementación de red principal
Para implementar una red principal, los pasos básicos son:

1.  [Configuración de todos los servidores](#BKMK_configuringAll)

2.  [Implementar DC1](#BKMK_deployADDNS01)

3.  [Unir el servidor equipos al dominio e iniciar sesión](#BKMK_joinlogserver)

4.  [Implementar DHCP1](#BKMK_deployDHCP01)

5.  [Los equipos cliente de unirse al dominio e iniciar sesión](#BKMK_joinlogclients)

6.  [Implementación de características opcionales para la autenticación de acceso de red y servicios Web](#BKMK_optionalfeatures)

> [!NOTE]
> -   Comandos equivalentes de Windows PowerShell se proporcionan para la mayoría de los procedimientos en esta guía. Antes de ejecutar estos cmdlets de Windows PowerShell, reemplaza los valores de ejemplo con valores que son adecuados para la implementación de red. Además, tienes que escribir cada cmdlet en una sola línea en Windows PowerShell. En esta guía, los cmdlets individuales pueden aparecer en varias líneas debido a restricciones y la visualización del documento de formato por otra aplicación o de su explorador.
> -   Los procedimientos descritos en esta guía no incluyen instrucciones para aquellos casos en que la **Control de cuentas de usuario** abre el cuadro de diálogo para solicitar su permiso para continuar. Si se abre este cuadro de diálogo mientras realizan los procedimientos descritos en esta guía y si el cuadro de diálogo se abrió en respuesta a las acciones, haz clic en **continuar**.

### <a name="BKMK_configuringAll"></a>Configuración de todos los servidores
Antes de instalar otras tecnologías, como los servicios de dominio de Active Directory o DHCP, es importante configurar los siguientes elementos.

-   [Cambiar el nombre del equipo](#BKMK_rename)

-   [Configurar una dirección IP estática](#BKMK_ip)

Puedes usar las siguientes secciones para realizar estas acciones en cada servidor.

Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para realizar estos procedimientos.

#### <a name="BKMK_rename"></a>Cambiar el nombre del equipo
Puedes usar el procedimiento de esta sección para cambiar el nombre de un equipo. Cambiar el nombre del equipo es útil para casos en que el sistema operativo ha creado automáticamente un nombre de equipo que no quieres usar.

> [!NOTE]
> Para realizar este procedimiento mediante Windows PowerShell, abre PowerShell y escribe los siguientes cmdlets en líneas independientes y, a continuación, presione ENTRAR. También debes reemplazar *ComputerName* con el nombre que quieras usar.
>
> `Rename-Computer`*ComputerName*
>
> `Restart-Computer`

###### <a name="to-rename-computers-running-windows-server-2016-windows-server-2012-r2-and-windows-server-2012"></a>Para cambiar el nombre de los equipos que ejecutan Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

1.  En el administrador del servidor, haz clic en **servidor Local**. El equipo **propiedades** se muestran en el panel de detalles.

2.  En **propiedades**, en **nombre de equipo**, haz clic en el nombre de equipo existente. La **propiedades del sistema** abre el cuadro de diálogo. Haz clic en **cambio**. La **cambios de dominio o el nombre de equipo** abre el cuadro de diálogo.

3.  En la **cambios de dominio o el nombre de equipo** cuadro de diálogo **nombre de equipo**, escribe un nombre nuevo para el equipo. Por ejemplo, si quieres un nombre al equipo DC1, escribe **DC1**.

4.  Haz clic en **Aceptar** dos veces y, a continuación, haz clic en **cerrar**. Si quieres reiniciar el equipo inmediatamente para completar el cambio de nombre, haz clic en **reiniciar ahora**. De lo contrario, haz clic en **reiniciar más tarde**.

> [!NOTE]
> Para obtener información sobre cómo cambiar el nombre de los equipos que ejecutan otros sistemas operativos de Microsoft, consulta [apéndice - cambiar el nombre de equipos](#BKMK_A).

#### <a name="BKMK_ip"></a>Configurar una dirección IP estática
Puedes usar los procedimientos descritos en este tema para configurar el protocolo de Internet versión 4 (IPv4) propiedades de una conexión de red con una dirección IP estática de direcciones para equipos que ejecutan Windows Server 2016.

> [!NOTE]
> Para realizar este procedimiento mediante Windows PowerShell, abre PowerShell y escribe los siguientes cmdlets en líneas independientes y, a continuación, presione ENTRAR. También debes reemplazar los nombres de interfaz y direcciones IP en este ejemplo con los valores que quieres usar para configurar el equipo.
>
> `New-NetIPAddress -IPAddress 10.0.0.2 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24`
>
> `Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 127.0.0.1`

###### <a name="to-configure-a-static-ip-address-on-computers-running-windows-server-2016-windows-server-2012-r2-and-windows-server-2012"></a>Para configurar una dirección IP estática en equipos que ejecutan Windows Server 2012, Windows Server 2012 R2 y Windows Server 2016

1.  En la barra de tareas, haz clic en el icono de red y, a continuación, haz clic en **abrir Centro de redes y recursos compartidos**.

2.  En **centro de redes y recursos compartidos**, haz clic en **cambiar la configuración del adaptador**. La **conexiones de red** carpeta abre y muestra las conexiones de red disponible.

3.  En **conexiones de red**, haz clic en la conexión que desea configurar y, a continuación, haz clic en **propiedades**. La conexión de red **propiedades** abre el cuadro de diálogo.

4.  En la conexión de red **propiedades** cuadro de diálogo **esta conexión utiliza los siguientes elementos**, selecciona **protocolo de Internet versión 4 (TCP/IPv4)**y, a continuación, haz clic en **propiedades**. La **propiedades de protocolo de Internet versión 4 (TCP/IPv4)** abre el cuadro de diálogo.

5.  En **propiedades de protocolo de Internet versión 4 (TCP/IPv4)**, en la **General**, haga clic **usar la siguiente dirección IP**. En **dirección IP**, escribe la dirección IP que quieras usar.

6.  Presione tab para colocar el cursor en **máscara de subred**. Un valor predeterminado para la máscara de subred se introduce automáticamente. Acepte la máscara de subred de forma predeterminada, o escribe la máscara de subred que quieras usar.

7.  En **puerta de enlace predeterminada**, escribe la dirección IP de la puerta de enlace predeterminada.

    > [!NOTE]
    > Debes configurar **puerta de enlace predeterminada** con la misma dirección IP que usas en la interfaz de red de área local (LAN) del enrutador. Por ejemplo, si tienes un enrutador que está conectado a una red de área extensa (WAN) como Internet, así como para la LAN, configurar la interfaz de LAN con la misma dirección IP que, a continuación, se especifican como el **puerta de enlace predeterminada**. En otro ejemplo, si tienes un enrutador que está conectado a dos LAN, donde A LAN usa la dirección de intervalo 10.0.0.0/24 y LAN B usa la dirección de intervalo 192.168.0.0/24, configurar la dirección IP del enrutador A LAN con una dirección de ese intervalo de direcciones, como 10.0.0.1. Además, en el ámbito DHCP de este intervalo de direcciones, configurar **puerta de enlace predeterminada** con la dirección IP 10.0.0.1. Para LAN B, configurar la interfaz del enrutador LAN B con una dirección de ese intervalo de direcciones, como 192.168.0.1 y, a continuación, configurar la LAN B ámbito 192.168.0.0/24 con un **puerta de enlace predeterminada** valor de 192.168.0.1.

8.  En **servidor DNS preferido**, escribe la dirección IP del servidor DNS. Si tienes previsto usar el equipo local como el servidor DNS preferido, escribe la dirección IP del equipo local.

9. En **servidor DNS alternativo**, escriba la dirección IP del servidor DNS alternativo, si procede. Si tienes previsto usar el equipo local como un servidor DNS alternativo, escribe la dirección IP del equipo local.

10. Haz clic en **Aceptar**y, a continuación, haz clic en **cerrar**.

> [!NOTE]
> Para obtener información sobre cómo configurar una dirección IP estática en equipos que ejecutan otros sistemas operativos de Microsoft, consulta [Apéndice B - configurar direcciones IP estáticas](#BKMK_B).

#### <a name="BKMK_deployADDNS01"></a>Implementar DC1
Para implementar DC1, que es el equipo ejecuta Servicios de dominio de Active Directory (AD DS) y DNS, debes completar estos pasos en el siguiente orden:

-   Realiza los pasos en la sección [configurar todos los servidores](#BKMK_configuringAll).

-   [Instalar AD DS y DNS para un bosque nuevo](#BKMK_installAD-DNS)

-   [Crear una cuenta de usuario en equipos y usuarios de Active Directory](#BKMK_createUA)

-   [Asignar pertenencia a grupos](#BKMK_assigngroup)

-   [Configurar una zona de búsqueda inversa de DNS](#BKMK_reverse)

**Privilegios administrativos**

Si estás instalando una red pequeña y es el único administrador de la red, se recomienda que crees una cuenta de usuario por TI mismo y, a continuación, agrega tu cuenta de usuario como un miembro de administradores de empresa y administradores de dominio. Si lo haces, será más fácil para TI para que actúe como el Administrador de todos los recursos de red. Se recomienda iniciar sesión con esta cuenta solo cuando necesitas realizar tareas administrativas y que crees una cuenta de usuario independientes para llevar a cabo ajeno a TI tareas relacionadas.

Si tienes una organización mayor con varios administradores, consulta la documentación de AD DS para determinar la pertenencia al grupo mejor para los empleados de la organización.

**Diferencias entre cuentas de usuario de dominio y cuentas de usuario en el equipo local**

Una de las ventajas de una infraestructura basada en el dominio es que no es necesario crear cuentas de usuario en cada equipo del dominio. Esto sucede si el equipo es un equipo cliente o un servidor.

Por este motivo, no debes crear cuentas de usuario en cada equipo en el dominio. Crear todas las cuentas de usuario en equipos y usuarios de Active Directory y usar los procedimientos anteriores para asignar la pertenencia al grupo. De manera predeterminada, todas las cuentas de usuario son miembros del grupo usuarios del dominio.

Todos los miembros del grupo usuarios del dominio pueden iniciar sesión en cualquier equipo cliente después de que está unido al dominio.

Puedes configurar cuentas de usuario para designar los días y horas en que el usuario puede iniciar sesión en el equipo. También puedes designar los equipos que se puede usar cada usuario. Para configurar estas opciones de configuración, abra y equipos de usuarios de Active Directory, busque la cuenta de usuario que va a configurar y haz doble clic en la cuenta. En la cuenta de usuario **propiedades**, haz clic en el **cuenta** pestaña y, a continuación, haz clic en **horas de inicio de sesión** o **iniciar sesión en**.

#### <a name="BKMK_installAD-DNS"></a>Instalar AD DS y DNS para un bosque nuevo

Puedes usar uno de los siguientes procedimientos para instalar servicios de dominio de Active Directory (AD DS) y DNS y para crear un nuevo dominio en un bosque nuevo. 

El primer procedimiento proporciona instrucciones para realizar estas acciones con Windows PowerShell, mientras que el segundo procedimiento muestra cómo instalar AD DS y DNS mediante el administrador del servidor.

>[!IMPORTANT]
>Cuando termines de realizar los pasos de este procedimiento, el equipo se reinicia automáticamente.

**Instalar AD DS y DNS mediante Windows PowerShell**

Puedes usar los siguientes comandos para instalar y configurar AD DS y DNS. Debes reemplazar el nombre de dominio en este ejemplo con el valor que desea usar para el dominio.

>[!NOTE]
>Para obtener más información acerca de estos comandos de Windows PowerShell, consulta los siguientes temas de referencia.
>- [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/server-manager/install-windowsfeature)
>- [Install-ADDSForest](https://technet.microsoft.com/itpro/powershell/windows/adds/deployment/install-addsforest)

Pertenencia a **administradores** es lo mínimo necesario para realizar este procedimiento.

- Ejecutar Windows PowerShell como administrador y escribe el siguiente comando:  

`Install-WindowsFeature AD-Domain-Services -IncludeManagementTools`

Cuando se ha completado correctamente la instalación, se muestra el siguiente mensaje en Windows PowerShell.

    
    Success Restart Needed  Exit Code   Feature Result
    ------- --------------  ---------   --------------
    True    No              Success     {Active Directory Domain Services, Group P...
    

- En Windows PowerShell, escribe el comando siguiente, sustituyendo el texto **corp.contoso.com** con tu nombre de dominio y, a continuación, presiona ENTRAR:

````
Install-ADDSForest -DomainName "corp.contoso.com"
````

- Durante el proceso de instalación y configuración, que está visible en la parte superior de la ventana de PowerShell de Windows, aparece el mensaje siguiente. Después de que aparece, escribe una contraseña y, a continuación, presione ENTRAR.

    **SafeModeAdministratorPassword:**

- Después de escribir una contraseña y presiona ENTRAR, aparece el siguiente mensaje de confirmación. Escribe la misma contraseña y, a continuación, presione ENTRAR.

    **Confirmar SafeModeAdministratorPassword:**

- Cuando aparezca el mensaje siguiente, escribe la letra **Y** y, a continuación, presione ENTRAR.

    
    El servidor de destino se configurado como un controlador de dominio y se reinicie una vez completada esta operación.
    ¿Quieres continuar con esta operación?
    [Y] [A] "yes" Sí a todo [N] No L No a todos los [S] suspende [?] Ayuda (valor predeterminado es "Y"):
    
- Si quieres, puedes leer los mensajes de advertencia que se muestren durante la instalación normal y correcta de AD DS y DNS. Estos mensajes son normales y no son una indicación de error de instalación.

- Después de instalación se realiza correctamente, aparece un mensaje que indica que va a ser cerrado sesión en el equipo para que pueda reiniciar el equipo. Si haces clic en **cerrar**, inmediatamente se cierran el equipo y reinicia el equipo. Si no **cerrar**, el equipo se reinicia tras un período de tiempo predeterminado.

- Después de reinicia el servidor, puedes comprobar la instalación correcta de los servicios de dominio de Active Directory y DNS. Abre Windows PowerShell, escribe el siguiente comando y presione ENTRAR.

````
Get-WindowsFeature
````

Los resultados de este comando se muestran en Windows PowerShell y deben ser similares a los resultados en la imagen siguiente. Para las tecnologías instaladas, los corchetes a la izquierda del nombre de la tecnología contengan el carácter **X**y el valor de **estado de instalación** es **instalado**.

![Resultados del comando Get-WindowsFeature](../media/Core-Network-Guide/server-roles-installed.jpg)

**Instalar AD DS y DNS mediante el administrador del servidor**

1.  En DC1, en **administrador del servidor**, haz clic en **administrar**y, a continuación, haz clic en **agregar Roles y características**. Abre el agregar Roles and Features Wizard.

2.  En **antes de comenzar**, haz clic en **siguiente**.

    > [!NOTE]
    > La **antes de comenzar** no se muestra la página del agregar Roles y características de asistente si anteriormente seleccionaste **omitir esta página predeterminada** cuando se ejecutó el agregar Roles y características de asistente.

3.  En **seleccionar el tipo de instalación**, asegúrate de que **instalación basada en rol o característica** está seleccionado y, a continuación, haz clic en **siguiente**.

4.  En **servidor de destino selecciona**, asegúrate de que **seleccionar un servidor desde el grupo de servidores** está seleccionado. En **grupo de servidores**, asegúrese de que el equipo local está seleccionado. Haz clic en **siguiente**.

5.  En **seleccionar roles de servidor**, en **Roles**, haz clic en **los servicios de dominio de Active Directory**. En **agregar características que son necesarias para los servicios de dominio de Active Directory**, haz clic en **agregar características**. Haz clic en **siguiente**.

6.  En **Select features**, haz clic en **siguiente**y en **los servicios de dominio de Active Directory**, revisa la información que se proporciona y, a continuación, haz clic en **siguiente**.

7.  En **Confirmar selecciones de instalación**, haz clic en **instalar**. La página de progreso de la instalación muestra el estado durante el proceso de instalación. Cuando finalice el proceso, en los detalles del mensaje, haz clic en **promover este servidor a un controlador de dominio**. Abre el Asistente para la configuración de los servicios de dominio de Active Directory.

8.  En **configuración de implementación**, selecciona **agregar un bosque nuevo**. En **nombre de dominio raíz**, escribe el nombre de dominio completo (FQDN) de tu dominio. Por ejemplo, si el FQDN corp.contoso.com, escribe **corp.contoso.com**. Haz clic en **siguiente**.

9. En **opciones del controlador de dominio**, en **seleccione un nivel funcional del bosque y dominio raíz**, selecciona el nivel funcional del bosque y el nivel funcional del dominio que quieras usar. En **especificar funcionalidades de controlador de dominio**, asegúrate de que **servidor de sistema de nombres de dominio (DNS)** y **Global catálogo** están seleccionados. En **contraseña** y **Confirmar contraseña**, escriba la contraseña de modo de restauración de servicios de directorio (DSRM) que quieres usar. Haz clic en **siguiente**.

10. En **opciones DNS**, haz clic en **siguiente**.

11. En **opciones adicionales**, compruebe el nombre NetBIOS que se asigna al dominio y cambiarla si es necesario. Haz clic en **siguiente**.

12. En **rutas de acceso**, en **especificar la ubicación de la base de datos de AD DS, archivos de registro y SYSVOL**, realiza una de las siguientes acciones:

    -   Acepta los valores predeterminados.

    -   Escribe las ubicaciones de carpeta que desea usar para **carpeta base de datos**, **carpeta de archivos de registro**, y **carpeta SYSVOL**.

13. Haz clic en **siguiente**.

14. En **Revisar opciones**, revise las opciones seleccionadas.

15. Si quieres exportar la configuración a un script de PowerShell de Windows, haz clic en **Ver script**. El script se abre en el Bloc de notas, y puede guardar la ubicación de carpeta que quieras. Haz clic en **siguiente**. En **comprobación de requisitos previos**, se validan tus selecciones. Cuando se completa la comprobación, haz clic en **instalar**. Cuando se te pida por Windows, haz clic en **cerrar**. Reinicia el servidor para completar la instalación de AD DS y DNS.

16. Para comprobar la instalación correcta, ver la consola de administrador del servidor después de reiniciar el servidor. AD DS y DNS deben aparecer en el panel izquierdo, como los elementos resaltados en la imagen siguiente.

![En el administrador del servidor DNS y AD DS](../media/Core-Network-Guide/server-roles-installed-sm.jpg)

##### <a name="BKMK_createUA"></a>Crear una cuenta de usuario en equipos y usuarios de Active Directory
Puedes usar este procedimiento para crear una nueva cuenta de usuario de dominio en los usuarios de Active Directory y equipos de Microsoft Management Console (MMC).

Pertenencia a **administradores de dominio**, o equivalente, es lo mínimo necesario para realizar este procedimiento.

> [!NOTE]
> Para realizar este procedimiento mediante Windows PowerShell, abre PowerShell y escribe el siguiente cmdlet en una sola línea y, a continuación, presione ENTRAR. También debes reemplazar el nombre de cuenta de usuario en este ejemplo con el valor que quieras usar.
>
> `New-ADUser -SamAccountName User1 -AccountPassword (read-host "Set user password" -assecurestring) -name "User1" -enabled $true -PasswordNeverExpires $true -ChangePasswordAtLogon $false`
>
> Después de presionar ENTRAR, escribe la contraseña de la cuenta de usuario. La cuenta se crea y, de forma predeterminada, se concede pertenencia al grupo usuarios de dominio.
>
> Con el cmdlet siguiente, puedes asignar a pertenencias a grupos adicionales para la nueva cuenta de usuario. El siguiente ejemplo agrega User1a los grupos Administradores de dominio y administradores de empresa. Asegúrate de antes de ejecutar este comando cambia el nombre de la cuenta de usuario, el nombre de dominio y los grupos para que coincida con los requisitos.
>
> `Add-ADPrincipalGroupMembership -Identity "CN=User1,CN=Users,DC=corp,DC=contoso,DC=com" -MemberOf "CN=Enterprise Admins,CN=Users,DC=corp,DC=contoso,DC=com","CN=Domain Admins,CN=Users,DC=corp,DC=contoso,DC=com"`

###### <a name="to-create-a-user-account"></a>Para crear una cuenta de usuario

1.  En DC1, en el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **equipos y usuarios de Active Directory**. Abre el MMC de equipos y usuarios de Active Directory. Si no está ya seleccionado, haz clic en el nodo de tu dominio. Por ejemplo, si el dominio es corp.contoso.com, haz clic en **corp.contoso.com**.

2.  En el panel de detalles, haz clic en la carpeta en la que quieres agregar una cuenta de usuario.

    **¿Donde?**

    -   Directorio de usuarios y equipos de Active /*nodo del dominio*/*carpeta*

3.  Elija **nueva**y, a continuación, haz clic en **usuario**. La **nuevo objeto - usuario** abre el cuadro de diálogo.

4.  En **nombre**, escribe el nombre del usuario.

5.  En **iniciales**, escriba iniciales del usuario.

6.  En **apellido**, escriba el apellido del usuario.

7.  Modificar **nombre completo** para agregar iniciales o invertir el orden de nombre y apellidos.

8.  En **nombre de inicio de sesión de usuario**, escribe el nombre de inicio de sesión de usuario. Haz clic en **siguiente**.

9. En **nuevo objeto - usuario**, en **contraseña** y **Confirmar contraseña**, escribe la contraseña del usuario y, a continuación, selecciona las opciones de contraseñas adecuada.

10. Haz clic en **siguiente**, revisar la configuración de cuentas de usuario nueva y, a continuación, haz clic en **finalizar**.

##### <a name="BKMK_assigngroup"></a>Asignar pertenencia a grupos
Puedes usar este procedimiento para agregar un usuario, un equipo o un grupo a un grupo de usuarios de Active Directory y equipos de Microsoft Management Console (MMC).

Pertenencia a **administradores de dominio**, o equivalente, es lo mínimo necesario para realizar este procedimiento.

###### <a name="to-assign-group-membership"></a>Para asignar la pertenencia a grupos

1.  En DC1, en el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **equipos y usuarios de Active Directory**. Abre el MMC de equipos y usuarios de Active Directory. Si no está ya seleccionado, haz clic en el nodo de tu dominio. Por ejemplo, si el dominio es corp.contoso.com, haz clic en **corp.contoso.com**.

2.  En el panel de detalles, haz doble clic en la carpeta que contiene el grupo al que quieres agregar un miembro.

    ¿Donde?

    -   **Directorio de usuarios y equipos de Active**/*nodo del dominio*/*carpeta que contiene el grupo*

3.  En el panel de detalles, haz clic en el objeto que quieres agregar a un grupo, como un usuario o equipo y, a continuación, haz clic en **propiedades**. El objeto **propiedades** abre el cuadro de diálogo. Haz clic en el **miembro de** pestaña.

4.  En la **miembro de**, haga clic **agregar**.

5.  En **escriba los nombres de objeto para seleccionar**, escribe el nombre del grupo al que quieres agregar el objeto y, a continuación, haz clic en **Aceptar**.

6.  Para asignar la pertenencia al grupo a otros usuarios, equipos o grupos, repita los pasos 4 y 5 de este procedimiento.

##### <a name="BKMK_reverse"></a>Configurar una zona de búsqueda inversa de DNS
Puedes usar este procedimiento para configurar una zona de búsqueda inversa en el sistema de nombres de dominio (DNS).

Pertenencia a **administradores de dominio** es lo mínimo necesario para realizar este procedimiento.

> [!NOTE]
> -   Para las organizaciones medianas y grandes, se recomienda que configurar y usar el grupo de grupo Administradores DNS en equipos y usuarios de Active Directory. Para obtener más información, consulta [recursos técnicos adicionales](#BKMK_resources)
> -   Para realizar este procedimiento mediante Windows PowerShell, abre PowerShell y escribe el siguiente cmdlet en una sola línea y, a continuación, presione ENTRAR. También debes reemplazar los nombres de zona y zonefile de la búsqueda inversa de DNS en este ejemplo con los valores que quieres usar. Asegúrate de que invertir el Id. de red para el nombre de la zona inversa. Por ejemplo, si el identificador de red es 192.168.0, crear el nombre de la zona de búsqueda inversa **0.168.192.in-addr.arpa**.
>
> `Add-DnsServerPrimaryZone 0.0.10.in-addr.arpa -ZoneFile 0.0.10.in-addr.arpa.dns`

###### <a name="to-configure-a-dns-reverse-lookup-zone"></a>Para configurar una zona de búsqueda inversa de DNS

1.  En DC1, en el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **DNS**. Abre MMC de DNS.

2.  En DNS, si aún no está expandida, haz doble clic en el nombre del servidor para expandir el árbol. Por ejemplo, si el nombre del servidor DNS es DC1, haz doble clic en **DC1**.

3.  Selecciona **zonas de búsqueda inversa**, haz clic en **zonas de búsqueda inversa**y, a continuación, haz clic en **nueva zona**. Abre el Asistente para nueva zona.

4.  En **Asistente para la nueva zona**, haz clic en **siguiente**.

5.  En **tipo de zona**, selecciona **zona principal**.

6.  Si el servidor DNS es un controlador de dominio grabable, asegúrate de que **almacenar la zona en Active Directory** está seleccionado. Haz clic en **siguiente**.

7.  En **ámbito de replicación de Active Directory zona**, selecciona **a todos los servidores DNS que se ejecutan en los controladores de dominio en este dominio**, a menos que tengas un motivo específico para elegir una opción diferente. Haz clic en **siguiente**.

8.  En la primera **nombre de la zona de búsqueda inversa** página, seleccione **zona de búsqueda inversa IPv4**. Haz clic en **siguiente**.

9. En la segunda **nombre de la zona de búsqueda inversa** página, realiza una de las siguientes acciones:

    -   En **Id. de red**, escribe el identificador de red de su intervalo de direcciones IP. Por ejemplo, si el intervalo de direcciones IP es 10.0.0.1a través de 10.0.0.254, escriba **10.0.0**.

    -   En **nombre de la zona de búsqueda inversa**, se agrega automáticamente el nombre de la búsqueda inversa IPv4 zona. Haz clic en **siguiente**.

10. En **actualización dinámica**, selecciona el tipo de actualizaciones dinámicas que desea permitir. Haz clic en **siguiente**.

11. En **completar el Asistente para nueva zona**, revisa las opciones seleccionadas y, a continuación, haz clic en **finalizar**.

#### <a name="BKMK_joinlogserver"></a>Unir el servidor equipos al dominio e iniciar sesión
Después de que ha instalado servicios de dominio de Active Directory (AD DS) y ha creado una o varias cuentas de usuario que tengan permisos para unir un equipo al dominio, puedes unirte a servidores de la red principal para el dominio e iniciar sesión en los servidores para poder instalar tecnologías adicionales, como el protocolo de configuración dinámica de Host (DHCP).

En todos los servidores que vas a implementar, excepto el servidor de AD DS, haz lo siguiente:

1.  Completar los procedimientos que se proporcionan en [configurar todos los servidores](#BKMK_configuringAll).

2.  Usa las instrucciones en los siguientes procedimientos para unirte a los servidores en el dominio y para iniciar sesión en los servidores para realizar tareas de implementación adicionales:

> [!NOTE]
> Para realizar este procedimiento mediante Windows PowerShell, abre PowerShell y escribe el siguiente cmdlet y, a continuación, presione ENTRAR. También debes reemplazar el nombre de dominio con el nombre que quieras usar.
>
> `Add-Computer -DomainName corp.contoso.com`
>
> Cuando se te pida que lo haga, escribe el nombre de usuario y contraseña para una cuenta que tiene permiso para unir un equipo al dominio. Para reiniciar el equipo, escribe el siguiente comando y presione ENTRAR.
>
> `Restart-Computer`

###### <a name="to-join-computers-running--windows-server-2016--windows-server-2012-r2--and--windows-server-2012--to-the-domain"></a>Para unirte a equipos que ejecutan Windows Server 2012, Windows Server 2012 R2 y Windows Server 2016 al dominio

1.  En el administrador del servidor, haz clic en **servidor Local**. En el panel de detalles, haz clic en **grupo de trabajo**. La **propiedades del sistema** abre el cuadro de diálogo.

2.  En la **propiedades del sistema** cuadro de diálogo, haz clic en **cambio**. La **cambios de dominio o el nombre de equipo** abre el cuadro de diálogo.

3.  En **nombre de equipo**, en **miembro de**, haz clic en **dominio**y, a continuación, escribe el nombre del dominio que quieres unirte. Por ejemplo, si el nombre de dominio es corp.contoso.com, escriba **corp.contoso.com**.

4.  Haz clic en **Aceptar**. El **Windows Security** abre el cuadro de diálogo.

5.  En **cambios de dominio o el nombre de equipo**, en **nombre de usuario**, escribe el nombre de usuario y en **contraseña**, escribe la contraseña y, a continuación, haz clic en **Aceptar**. La **cambios de dominio o el nombre de equipo** abre el cuadro de diálogo, le da la bienvenida al dominio. Haz clic en **Aceptar**.

6.  La **cambios de dominio o el nombre de equipo** cuadro de diálogo muestra un mensaje que indica que debes reiniciar el equipo para aplicar los cambios. Haz clic en **Aceptar**.

7.  En la **propiedades del sistema** cuadro de diálogo, en la **nombre de equipo**, haga clic **cerrar**. La **Microsoft Windows** cuadro de diálogo se abre y muestra un mensaje nuevo que indica que debes reiniciar el equipo para aplicar los cambios. Haz clic en **reiniciar ahora**.

> [!NOTE]
> Para obtener información sobre cómo unirse a equipos que ejecutan otros sistemas operativos de Microsoft al dominio, consulta el tema [Apéndice C - unir equipos al dominio](#BKMK_C).

###### <a name="to-log-on-to-the-domain-using-computers-running-windows-server-2016"></a>Para iniciar sesión en el dominio con equipos que ejecutan Windows Server 2016

1.  Cerrar sesión en el equipo o reiniciar el equipo.

2.  Presiona CTRL + ALT + SUPR. Aparecerá la pantalla de inicio de sesión.

3.  En la esquina inferior izquierda, haz clic en **otro usuario**.

4.  En **nombre de usuario**, escribe tu nombre de usuario.

5.  En **contraseña**, escriba la contraseña del dominio y, a continuación, haz clic en la flecha o presione ENTRAR.

> [!NOTE]
> Para obtener información sobre cómo iniciar sesión en el dominio con equipos que ejecutan otros sistemas operativos de Microsoft, consulta [apéndice D - iniciar sesión en el dominio](#BKMK_D).

#### <a name="BKMK_deployDHCP01"></a>Implementar DHCP1
Antes de implementar este componente de la red principal, haz lo siguiente:

-   Realiza los pasos en la sección [configurar todos los servidores](#BKMK_configuringAll).

-   Realiza los pasos en la sección [equipos de servidor unirse al dominio y de registro en](#BKMK_joinlogserver).

Para implementar DHCP1, que es el equipo ejecutando el rol de servidor de protocolo de configuración dinámica de Host (DHCP), debes completar estos pasos en el siguiente orden:

-   [Instalar el protocolo de configuración dinámica de Host (DHCP)](#BKMK_installDHCP)

-   [Crear y activar un nuevo ámbito DHCP](#BKMK_newscopeDHCP)

> [!NOTE]
> Para realizar estos procedimientos mediante Windows PowerShell, abre PowerShell y escribe los siguientes cmdlets en líneas independientes y, a continuación, presione ENTRAR. También debes reemplazar el nombre del ámbito, inicio de dirección IP y los intervalos final, máscara de subred y otros valores en este ejemplo con los valores que quieres usar.
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
Puedes usar este procedimiento para instalar y configurar el rol de servidor DHCP mediante el agregar Roles y características de asistente.

Pertenencia a **administradores de dominio**, o equivalente, es lo mínimo necesario para realizar este procedimiento.

###### <a name="to-install-dhcp"></a>Para instalar DHCP

1.  En DHCP1, en el administrador del servidor, haz clic en **administrar**y, a continuación, haz clic en **agregar Roles y características**. Abre el agregar Roles and Features Wizard.

2.  En **antes de comenzar**, haz clic en **siguiente**.

    > [!NOTE]
    > La **antes de comenzar** no se muestra la página del agregar Roles y características de asistente si anteriormente seleccionaste **omitir esta página predeterminada** cuando se ejecutó el agregar Roles y características de asistente.

3.  En **seleccionar el tipo de instalación**, asegúrate de que **instalación basada en rol o característica** está seleccionado y, a continuación, haz clic en **siguiente**.

4.  En **servidor de destino selecciona**, asegúrate de que **seleccionar un servidor desde el grupo de servidores** está seleccionado. En **grupo de servidores**, asegúrese de que el equipo local está seleccionado. Haz clic en **siguiente**.

5.  En **seleccionar Roles de servidor**, en **Roles**, selecciona **servidor DHCP**. En **agregar características que son necesarias para el servidor DHCP**, haz clic en **agregar características**. Haz clic en **siguiente**.

6.  En **Select features**, haz clic en **siguiente**y en **servidor DHCP**, revisa la información que se proporciona y, a continuación, haz clic en **siguiente**.

7.  En **Confirmar selecciones de instalación**, haz clic en **reiniciar automáticamente el servidor de destino en caso necesario**. Cuando aparezca el mensaje para confirmar esta selección, haz clic en **Sí**y, a continuación, haz clic en **instalar**. La **progreso de la instalación** página muestra el estado durante el proceso de instalación. Cuando se completa el proceso, el mensaje "configuración necesaria. Instalación correcta en *ComputerName*"se muestra, donde *ComputerName* es el nombre del equipo en el que instalaste servidor DHCP. En la ventana de mensajes, haz clic en **configuración DHCP completa**. Abre el Asistente para configuración de DHCP Post-Install posterior. Haz clic en **siguiente**.

8.  En **autorización**, especifica las credenciales que quieres usar para autorizar el servidor DHCP en los servicios de dominio de Active Directory y, a continuación, haz clic en **confirmar**. Una vez completada la autorización, haz clic en **cerrar**.

##### <a name="BKMK_newscopeDHCP"></a>Crear y activar un nuevo ámbito DHCP
Puedes usar este procedimiento para crear un nuevo ámbito DHCP con DHCP Microsoft Management Console (MMC). Cuando termine, el ámbito se activa y el intervalo de exclusión que crees impide que el servidor DHCP alquilar las direcciones IP que se usan para configurar los servidores de forma estática y otros dispositivos que requieran una dirección IP estática.

Pertenencia a **Administradores DHCP**, o equivalente, es lo mínimo necesario para realizar este procedimiento.

###### <a name="to-create-and-activate-a-new-dhcp-scope"></a>Para crear y activar un nuevo ámbito DHCP

1.  En DHCP1, en el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **DHCP**. Abre MMC DHCP.

2.  En **DHCP**, expande el nombre del servidor. Por ejemplo, si el nombre del servidor DHCP es DHCP1.corp.contoso.com, haz clic en la flecha abajo situada junto a **DHCP1.corp.contoso.com**.

3.  Bajo el nombre del servidor, haz clic en **IPv4**y, a continuación, haz clic en **nuevo ámbito**. Abre el Asistente de ámbito nuevo.

4.  En **Asistente para nuevo ámbito**, haz clic en **siguiente**.

5.  En **nombre del ámbito**, en **nombre**, escribe un nombre para el ámbito. Por ejemplo, escribe **subred 1**.

6.  En **descripción**, escribe una descripción para el nuevo ámbito y, a continuación, haz clic en **siguiente**.

7.  En **intervalo de direcciones IP**, haz lo siguiente:

    1.  En **dirección IP inicial**, escribe la dirección IP que es la primera dirección IP en el intervalo. Por ejemplo, escribe **10.0.0.1**.

    2.  En **dirección IP final**, escribe la dirección IP que es la última dirección IP en el intervalo. Por ejemplo, escribe **10.0.0.254**. Valores de **longitud** y **máscara de subred** se escriben automáticamente, en función de la dirección IP que se haya especificado para **dirección IP inicial**.

    3.  Si es necesario, modificar los valores de **longitud** o **máscara de subred**, según corresponda para el esquema de direccionamiento.

    4.  Haz clic en **siguiente**.

8.  En **agregar exclusiones**, haz lo siguiente:

    1.  En **dirección IP inicial**, escribe la dirección IP que es la primera dirección IP en el intervalo de exclusión. Por ejemplo, escribe **10.0.0.1**.

    2.  En **dirección IP final**, escribe la dirección IP que es la última IP de direcciones en el intervalo de exclusión, por ejemplo, escribe **10.0.0.15**.

9. Haz clic en **agregar**y, a continuación, haz clic en **siguiente**.

10. En **duración**, modificar los valores predeterminados de **días**, **horas**, y **minutos**, según corresponda para su red y, a continuación, haz clic en **siguiente**.

11. En **configurar opciones de DHCP**, selecciona **Sí, deseo configurar estas opciones ahora**y, a continuación, haz clic en **siguiente**.

12. En **enrutador (puerta de enlace predeterminada)**, realiza una de las siguientes acciones:

    -   Si no tienes enrutadores en la red, haz clic en **siguiente**.

    -   En **dirección IP**, escribe la dirección IP del enrutador o puerta de enlace predeterminada. Por ejemplo, escribe **10.0.0.1**. Haz clic en **agregar**y, a continuación, haz clic en **siguiente**.

13. En **nombre de dominio y servidores DNS**, haz lo siguiente:

    1.  En **dominio principal**, escribe el nombre de dominio DNS que los clientes usan para la resolución de nombres. Por ejemplo, escribe **corp.contoso.com**.

    2.  En **nombre del servidor**, escribe el nombre de equipo DNS que los clientes usan para la resolución de nombres. Por ejemplo, escribe **DC1**.

    3.  Haz clic en **resolver**. Se agrega la dirección IP del servidor DNS en **dirección IP**. Haz clic en **agregar**, espere a que la validación de direcciones IP servidor DNS completar y, a continuación, haz clic en **siguiente**.

14. En **servidores WINS**, porque no tiene servidores WINS de la red, haz clic en **siguiente**.

15. En **Activar ámbito**, selecciona **Sí, deseo activar este ámbito ahora**.

16. Haz clic en **siguiente**y, a continuación, haz clic en **finalizar**.

> [!IMPORTANT]
> Para crear nuevos ámbitos para subredes adicionales, repite este procedimiento. Usa un intervalo de direcciones IP diferente para cada subred que vas a implementar y asegurarse de que el reenvío de mensajes DHCP está habilitado en todos los enrutadores que conducen a otras subredes.

### <a name="BKMK_joinlogclients"></a>Los equipos cliente de unirse al dominio e iniciar sesión

> [!NOTE]
> Para realizar este procedimiento mediante Windows PowerShell, abre PowerShell y escribe el siguiente cmdlet y, a continuación, presione ENTRAR. También debes reemplazar el nombre de dominio con el nombre que quieras usar.
>
> `Add-Computer -DomainName corp.contoso.com`
>
> Cuando se te pida que lo haga, escribe el nombre de usuario y contraseña para una cuenta que tiene permiso para unir un equipo al dominio. Para reiniciar el equipo, escribe el siguiente comando y presione ENTRAR.
>
> `Restart-Computer`

##### <a name="to-join-computers-running-windows-10-to-the-domain"></a>Para unirte a equipos que ejecutan Windows 10 al dominio

1.  Iniciar sesión en el equipo con la cuenta de administrador local.

2.  En **buscar en la web y Windows**, tipo **sistema**. En los resultados de búsqueda, haz clic en **sistema (panel de Control)**. La **sistema** abre el cuadro de diálogo.

3.  En **sistema**, haz clic en **configuración avanzada del sistema**. La **propiedades del sistema** abre el cuadro de diálogo. Haz clic en el **nombre de equipo** pestaña.

4.  En **nombre de equipo**, haz clic en **cambio**. La **cambios de dominio o el nombre de equipo** abre el cuadro de diálogo.

5.  En **cambios de dominio o el nombre de equipo**, en **miembro de**, haz clic en **dominio**y, a continuación, escribe el nombre del dominio que desea unirse. Por ejemplo, si el nombre de dominio es corp.contoso.com, escriba **corp.contoso.com**.

6.  Haz clic en **Aceptar**. El **Windows Security** abre el cuadro de diálogo.

7.  En **cambios de dominio o el nombre de equipo**, en **nombre de usuario**, escribe el nombre de usuario y en **contraseña**, escribe la contraseña y, a continuación, haz clic en **Aceptar**. La **cambios de dominio o el nombre de equipo** abre el cuadro de diálogo, le da la bienvenida al dominio. Haz clic en **Aceptar**.

8.  La **cambios de dominio o el nombre de equipo** cuadro de diálogo muestra un mensaje que indica que debes reiniciar el equipo para aplicar los cambios. Haz clic en **Aceptar**.

9. En la **propiedades del sistema** cuadro de diálogo, en la **nombre de equipo**, haga clic **cerrar**. La **Microsoft Windows** cuadro de diálogo se abre y muestra un mensaje nuevo que indica que debes reiniciar el equipo para aplicar los cambios. Haz clic en **reiniciar ahora**.

##### <a name="to-join-computers-running-windows-81-to-the-domain"></a>Para unirte a equipos que ejecutan Windows 8.1 en el dominio

1.  Iniciar sesión en el equipo con la cuenta de administrador local.

2.  Haz clic en **inicio**y, a continuación, haz clic en **sistema**. La **sistema** abre el cuadro de diálogo.

3.  En **sistema**, haz clic en **configuración avanzada del sistema**. La **propiedades del sistema** abre el cuadro de diálogo. Haz clic en el **nombre de equipo** pestaña.

4.  En **nombre de equipo**, haz clic en **cambio**. La **cambios de dominio o el nombre de equipo** abre el cuadro de diálogo.

5.  En **cambios de dominio o el nombre de equipo**, en **miembro de**, haz clic en **dominio**y, a continuación, escribe el nombre del dominio que desea unirse. Por ejemplo, si el nombre de dominio es corp.contoso.com, escriba **corp.contoso.com**.

6.  Haz clic en **Aceptar**. El **Windows Security** abre el cuadro de diálogo.

7.  En **cambios de dominio o el nombre de equipo**, en **nombre de usuario**, escribe el nombre de usuario y en **contraseña**, escribe la contraseña y, a continuación, haz clic en **Aceptar**. La **cambios de dominio o el nombre de equipo** abre el cuadro de diálogo, le da la bienvenida al dominio. Haz clic en **Aceptar**.

8.  La **cambios de dominio o el nombre de equipo** cuadro de diálogo muestra un mensaje que indica que debes reiniciar el equipo para aplicar los cambios. Haz clic en **Aceptar**.

9. En la **propiedades del sistema** cuadro de diálogo, en la **nombre de equipo**, haga clic **cerrar**. La **Microsoft Windows** cuadro de diálogo se abre y muestra un mensaje nuevo que indica que debes reiniciar el equipo para aplicar los cambios. Haz clic en **reiniciar ahora**.

##### <a name="to-log-on-to-the-domain-using-computers-running-windows-10"></a>Para iniciar sesión en el dominio con equipos que ejecutan Windows 10

1.  Cerrar sesión en el equipo o reiniciar el equipo.

2.  Presiona CTRL + ALT + SUPR. Aparecerá la pantalla de inicio de sesión.

3.  En la esquina inferior izquierda, haz clic en **otro usuario**.

4.  En **nombre de usuario**, escribe tu nombre de usuario y dominio en el formato *dominio\nombre de usuario*. Por ejemplo, para iniciar sesión en el dominio corp.contoso.com con una cuenta denominada **usuario-01**, tipo **CORP\User-01**.

5.  En **contraseña**, escriba la contraseña del dominio y, a continuación, haz clic en la flecha o presione ENTRAR.

### <a name="BKMK_optionalfeatures"></a>Implementación de características opcionales para la autenticación de acceso de red y servicios Web
Si tienes intención de implementar los servidores de acceso de red, como puntos de acceso inalámbrico o servidores VPN, después de instalar la red principal, se recomienda que implementas un servidor NPS y un servidor Web. Para las implementaciones de acceso de red, se recomienda el uso de métodos de autenticación segura basada en certificados. Puedes usar NPS para administrar las directivas de acceso de red e implementar los métodos de autenticación segura. Puedes usar un servidor Web para publicar la lista de revocación de certificados (CRL) de la entidad de certificación (CA) que proporciona certificados para la autenticación segura.

> [!NOTE]
> Puedes implementar los certificados de servidor y otras características adicionales mediante el uso de guías de complemento de red principal. Para obtener más información, consulta [recursos técnicos adicionales](#BKMK_resources).

La siguiente ilustración muestra la topología de red de Windows Server Core con los servidores Web y NPS agregados.

![Topología de red de Windows Server Core con los servidores Web y NPS agregados](../media/Core-Network-Guide/cng16_overview_2.jpg)

Las secciones siguientes proporcionan información sobre cómo agregar NPS y servidores Web a la red.

-   [Implementar NPS1](#BKMK_deployNPS1)

-   [Implementar WEB1](#BKMK_IIS)

#### <a name="BKMK_deployNPS1"></a>Implementar NPS1
El servidor de servidor de directivas de redes (NPS) está instalado como un paso preparatorio para la implementación de otras tecnologías de acceso de red, como los conmutadores de autenticación 802.1X, servidores de red privada virtual (VPN) y puntos de acceso inalámbrico.

Servidor de directivas de redes (NPS) te permite configurar y administrar directivas de red con las siguientes características de forma centralizada: proxy RADIUS y el servidor de servicio de autenticación remota telefónica de usuario (RADIUS).

NPS es un componente opcional de una red principal, pero debe instalar NPS si se cumple alguna de las siguientes acciones:

-   Piensa expandir la red para incluir los servidores de acceso remoto que son compatibles con el protocolo RADIUS, como un equipo que ejecute Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008 y servicios de enrutamiento y acceso remoto, puerta de enlace de Terminal Services o la puerta de enlace de escritorio remoto.


-   Planear implementar la autenticación 802.1X para por cable o inalámbrica acceso.

Antes de implementar este servicio de rol, debes realizar los siguientes pasos en el equipo que va a configurar como un servidor NPS.

-   Realiza los pasos en la sección [configurar todos los servidores](#BKMK_configuringAll).

-   Realiza los pasos en la sección [equipos de servidor unirse al dominio y a iniciar sesión en](#BKMK_joinlogserver)

Para implementar NPS1, que es el equipo ejecutando el servicio de rol de servidor de directivas de redes (NPS) del rol de servidor de servicios de acceso y directivas de red, debes completar este paso:

-   [Planear la implementación de NPS1](#bkmk_NetFndtn_Pln_NPS-01)

-   [Instalar el servidor de directivas de redes (NPS)](#BKMK_installNPS)

-   [Registrar el servidor NPS en el dominio predeterminado](#BKMK_registerNPS)

> [!NOTE]
> Esta guía brinda instrucciones para la implementación de NPS en un servidor independiente o máquina virtual con nombre NPS1.  Otro modelo de implementación recomendada es la instalación de NPS en un controlador de dominio. Si prefieres instalar NPS en un controlador de dominio en lugar de en un servidor independiente, instale NPS en DC1.

##### <a name="bkmk_NetFndtn_Pln_NPS-01"></a>Planear la implementación de NPS1
Si tienes intención de implementar los servidores de acceso de red, como puntos de acceso inalámbrico o servidores VPN, después de implementar la red principal, se recomienda que implementas NPS.

Cuando se usa NPS como un servidor de servicio de autenticación remota telefónica de usuario (RADIUS), NPS realiza la autenticación y autorización para solicitudes de conexión a través de los servidores de acceso de red. NPS también te permite configurar y administrar directivas de red de forma centralizada que determinar quién puede acceder a la red, cómo pueden acceder a la red y cuándo pueden acceder a la red.

Siguiente es los pasos de programación claves antes de instalar NPS.

- Planea la base de datos de cuentas de usuario. De manera predeterminada, si participa en el servidor que ejecuta NPS a un dominio de Active Directory, NPS realiza la autenticación y autorización con la base de datos de cuentas de usuario de AD DS. En algunos casos, como con las redes de gran tamaño que usar NPS como un proxy RADIUS para reenviar las solicitudes de conexión a otros servidores RADIUS, es posible que desea instalar NPS en un equipo miembro no del dominio.

- Planear la administración de cuentas RADIUS. NPS te permite registrar los datos de cuentas en una base de datos de SQL Server o a un archivo de texto en el equipo local. Si quieres usar el registro de SQL Server, planea la instalación y configuración del servidor que ejecuta SQL Server.

##### <a name="BKMK_installNPS"></a>Instalar el servidor de directivas de redes (NPS)
Puedes usar este procedimiento para instalar el servidor de directivas de redes (NPS) mediante el agregar Roles y características de asistente. NPS es un servicio de rol del rol de servidor de servicios de acceso y directivas de redes.

> [!NOTE]
> De forma predeterminada, NPS escucha para el tráfico RADIUS en los puertos 1812, 1813, 1645 y 1646 en todos los adaptadores de red instalados. Si Firewall de Windows con seguridad avanzada está habilitado cuando instalas NPS, las excepciones del firewall para estos puertos se crean automáticamente durante el proceso de instalación de protocolo de Internet versión 6 \(IPv6\) y tráfico IPv4. Si se configuran los servidores de acceso de red para enviar el tráfico RADIUS sobre puertos que no sean de estos valores predeterminados, quite las excepciones que se crea en el Firewall de Windows con seguridad avanzada durante la instalación de NPS y crear excepciones para los puertos que usas para el tráfico RADIUS.

**Credenciales administrativas**

Para completar este procedimiento, debe ser miembro de la **administradores de dominio** grupo.

> [!NOTE]
> Para realizar este procedimiento mediante Windows PowerShell, abre PowerShell y escribe lo siguiente y, a continuación, presione ENTRAR.
>
> `Install-WindowsFeature NPAS -IncludeManagementTools`

###### <a name="to-install-nps"></a>Para instalar NPS

1.  En NPS1, en el administrador del servidor, haz clic en **administrar**y, a continuación, haz clic en **agregar Roles y características**. Abre el agregar Roles and Features Wizard.

2.  En **antes de comenzar**, haz clic en **siguiente**.

    > [!NOTE]
    > La **antes de comenzar** no se muestra la página del agregar Roles y características de asistente si anteriormente seleccionaste **omitir esta página predeterminada** cuando se ejecutó el agregar Roles y características de asistente.

3.  En **seleccionar el tipo de instalación**, asegúrate de que **instalación basada en rol o característica** está seleccionado y, a continuación, haz clic en **siguiente**.

4.  En **servidor de destino selecciona**, asegúrate de que **seleccionar un servidor desde el grupo de servidores** está seleccionado. En **grupo de servidores**, asegúrese de que el equipo local está seleccionado. Haz clic en **siguiente**.

5.  En **seleccionar Roles de servidor**, en **Roles**, selecciona **servicios de acceso y directivas de redes**. Abre un cuadro de cuadro de diálogo que pregunta si deben agregar características que son necesarias para los servicios de acceso y directivas de redes. Haz clic en **agregar características**y, a continuación, haz clic en **siguiente**.

6.  En **Select features**, haz clic en **siguiente**y en **servicios de acceso y directivas de redes**, revisa la información que se proporciona y, a continuación, haz clic en **siguiente**.

7.  En **seleccione los servicios de rol**, haz clic en **el servidor de directivas de red**.  En **agregar características que son necesarias para el servidor de directivas de red**, haz clic en **agregar características**. Haz clic en **siguiente**.

8.  En **Confirmar selecciones de instalación**, haz clic en **reiniciar automáticamente el servidor de destino en caso necesario**. Cuando aparezca el mensaje para confirmar esta selección, haz clic en **Sí**y, a continuación, haz clic en **instalar**. La página de progreso de la instalación muestra el estado durante el proceso de instalación. Cuando se completa el proceso, el mensaje "se completó la instalación en *ComputerName*" se muestra, donde *ComputerName* es el nombre del equipo en el que instalaste el servidor de directivas de red. Haz clic en **cerrar **.

##### <a name="BKMK_registerNPS"></a>Registrar el servidor NPS en el dominio predeterminado
Puedes usar este procedimiento para registrar un servidor NPS en el dominio donde el servidor es un miembro de dominio.

Servidores NPS deben registrarse en Active Directory para que tengan permiso para leer las propiedades de marcado de cuentas de usuario durante el proceso de autorización. Registrar un servidor NPS agrega el servidor para la **RAS y servidores IAS** grupo en Active Directory.

**Credenciales administrativas**

Para completar este procedimiento, debe ser miembro de la **administradores de dominio** grupo.

> [!NOTE]
> Para realizar este procedimiento mediante comandos de red (Netsh) shell en Windows PowerShell, abre PowerShell y escribe lo siguiente y, a continuación, presione ENTRAR.
>
> `netsh nps add registeredserver domain=corp.contoso.com server=NPS1.corp.contoso.com`

###### <a name="to-register-an-nps-server-in-its-default-domain"></a>Para registrar un servidor NPS en su dominio predeterminado

1.  En NPS1, en el administrador del servidor, haga clic en herramientas y, a continuación, haz clic en **el servidor de directivas de red**. Abre el MMC del servidor de directivas de red.

2.  Haz clic en **NPS (Local)**y, a continuación, haz clic en **Registrar servidor en Active Directory**. La **el servidor de directivas de red** abre el cuadro de diálogo.

3.  En **el servidor de directivas de red**, haz clic en **Aceptar**y, a continuación, haz clic en **Aceptar** nuevamente.

Para obtener más información sobre el servidor de directivas de red, consulta [servidor de directivas de redes (NPS)](../technologies/nps/nps-top.md).

#### <a name="BKMK_IIS"></a>Implementar WEB1

El rol de servidor Web (IIS) en Windows Server 2016, proporciona una plataforma segura, fácil de administrar, modular y extensible para hospedar los sitios Web, servicios y aplicaciones de forma segura. Con Internet Information Services (IIS), puedes compartir información con los usuarios en Internet, intranet o una extranet. IIS es una plataforma web unificado que se integra IIS, ASP.NET, servicios FTP, PHP y Windows Communication Foundation (WCF).

Además de permitir publicar una CRL para el acceso a equipos miembros del dominio, el rol de servidor Web Server (IIS) te permite configurar y administrar varios sitios Web, aplicaciones web y sitios FTP. IIS también ofrece las siguientes ventajas:

-   Maximizar la seguridad de la web a través de un aislamiento de aplicaciones de impresión y automático pies reducida del servidor.

-   Fácil de implementar y ejecutar ASP.NET, ASP clásica y PHP web aplicaciones en el mismo servidor.

-   Conseguir el aislamiento de la aplicación ofreciéndoles a los procesos de trabajo una identidad única y la configuración en un espacio aislado de manera predeterminada, lo que reduce aún más los riesgos de seguridad.

-   Agregar fácilmente, quitar e incluso reemplazar componentes integrados de IIS con módulos personalizados, adecuados para las necesidades de los clientes.

-   Aumentar la velocidad de tu sitio Web a través de compresión mejorada e integrada dinámico almacenamiento en caché.

Para implementar WEB1, que es el equipo que ejecuta el rol de servidor Web Server (IIS), haz lo siguiente:

-   Realiza los pasos en la sección [configurar todos los servidores](#BKMK_configuringAll).

-   Realiza los pasos en la sección [equipos de servidor unirse al dominio y a iniciar sesión en](#BKMK_joinlogserver)

-   [Instalar el rol de servidor Web Server (IIS)](#BKMK_install_IIS)

##### <a name="BKMK_install_IIS"></a>Instalar el rol de servidor Web Server (IIS)
Para completar este procedimiento, debe ser miembro de la **administradores** grupo.

> [!NOTE]
> Para realizar este procedimiento mediante Windows PowerShell, abre PowerShell y escribe lo siguiente y, a continuación, presione ENTRAR.
>
> `Install-WindowsFeature Web-Server -IncludeManagementTools`

1.  En **administrador del servidor**, haz clic en **administrar**y, a continuación, haz clic en **agregar Roles y características**. Abre el agregar Roles and Features Wizard.

2.  En **antes de comenzar**, haz clic en **siguiente**.

    > [!NOTE]
    > La **antes de comenzar** no se muestra la página del agregar Roles y características de asistente si anteriormente seleccionaste **omitir esta página predeterminada** cuando se ejecutó el agregar Roles y características de asistente.

3.  En la **seleccionar el tipo de instalación** página, haz clic en **siguiente**.

4.  En la **servidor de destino selecciona** página, asegúrese de que el equipo local está seleccionado y, a continuación, haz clic en **siguiente**.

5.  En la **seleccionar roles de servidor** página, desplázate a y selecciona **servidor Web (IIS)**. La **agregar características que son necesarias para el servidor Web (IIS)** abre el cuadro de diálogo. Haz clic en **agregar características**y, a continuación, haz clic en **siguiente**.

6.  Haz clic en **siguiente** hasta que haya aceptado el valor todas las de configuración del servidor web y, a continuación, haz clic en **instalar**.

7.  Comprueba que todas las instalaciones tuvieron éxito y, a continuación, haz clic en **cerrar**.

## <a name="BKMK_resources"></a>Recursos técnicos adicionales
Para obtener más información acerca de las tecnologías en esta guía, consulte los siguientes recursos:

 Recursos de la biblioteca técnica de Windows Server 2012, Windows Server 2012 R2 y Windows Server 2016

-   [Novedades de servicios de dominio de Active Directory (AD DS) en Windows Server 2016](https://technet.microsoft.com/en-us/library/mt163897.aspx)

-   [Active Directory Información general de servicios de dominio](https://technet.microsoft.com/library/hh831484.aspx) en https://technet.microsoft.com/library/hh831484.aspx.

-   [Información general de sistema de nombres (DNS) del dominio](https://technet.microsoft.com/library/hh831667.aspx) en https://technet.microsoft.com/library/hh831667.aspx.

-   [Implementar la función de administradores DNS](https://technet.microsoft.com/library/cc756152(WS.10).aspx)

-   [Información general de protocolo de configuración de Host (DHCP) dinámico](https://technet.microsoft.com/library/hh831825.aspx) en https://technet.microsoft.com/library/hh831825.aspx.

-   [Introducción a servicios de acceso y directivas de redes](https://technet.microsoft.com/library/hh831683.aspx) en https://technet.microsoft.com/library/hh831683.aspx.

-   [Introducción a Web Server (IIS)](https://technet.microsoft.com/library/hh831725.aspx) en https://technet.microsoft.com/library/hh831725.aspx.

## <a name="BKMK_appendix"></a>Apéndices A hasta E
Las secciones siguientes contienen información de configuración adicional para equipos que ejecutan sistemas operativos que no sean de Windows Server 2016, Windows 10, Windows Server 2012 y Windows 8. Además, se proporciona una hoja de cálculo de preparación de red para ayudarle con la implementación.

1.  [Apéndice a: - cambiar el nombre de equipos](#BKMK_A)

2.  [El apéndice B - configurar dirección IP estática direcciones](#BKMK_B)

3.  [Apéndice C: - unir equipos al dominio](#BKMK_C)

4.  [Apéndice D: - iniciar sesión en el dominio](#BKMK_D)

5.  [Apéndice E - red principal hoja de preparación de planeamiento](#BKMK_E)

## <a name="BKMK_A"></a>Apéndice a: - cambiar el nombre de equipos
Puedes usar los procedimientos descritos en esta sección para proporcionar los equipos que ejecutan Windows Server 2008 R2, Windows 7, Windows Server 2008 y Windows Vista con un nombre de equipo diferentes.

-   [Windows Server 2008 R2 y Windows 7](#bkmk_NetFndtn_Pln_rename_R2)

-   [Windows Server 2008 y Windows Vista](#bkmk_NetFndtn_Pln_Renam08)

### <a name="bkmk_NetFndtn_Pln_rename_R2"></a>Windows Server 2008 R2 y Windows 7
Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para realizar estos procedimientos.

##### <a name="to-rename-computers-running-windows-server-2008-r2-and-windows-7"></a>Para cambiar el nombre de los equipos que ejecutan Windows Server 2008 R2 y Windows 7

1.  Haz clic en **inicio**, haz clic en **equipo**y, a continuación, haz clic en **propiedades**. La **sistema** abre el cuadro de diálogo.

2.  En **configuración de nombre, dominio y grupo de trabajo del equipo**, haz clic en **cambiar la configuración de**. La **propiedades del sistema** abre el cuadro de diálogo.

    > [!NOTE]
    > En equipos que ejecutan Windows 7, antes del **propiedades del sistema** se abrirá el cuadro de diálogo, la **Control de cuentas de usuario** aparece el cuadro de diálogo que solicita permiso para continuar. Haz clic en **continuar** para continuar.

3.  Haz clic en **cambio**. La **cambios de dominio o el nombre de equipo** abre el cuadro de diálogo.

4.  En **nombre de equipo**, escribe el nombre de tu equipo. Por ejemplo, si quieres un nombre al equipo DC1, escribe **DC1**.

5.  Haz clic en **Aceptar** dos veces, haz clic en **cerrar**y, a continuación, haz clic en **reiniciar ahora** para reiniciar el equipo.

### <a name="bkmk_NetFndtn_Pln_Renam08"></a>Windows Server 2008 y Windows Vista
Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para realizar estos procedimientos.

##### <a name="to-rename-computers-running-windows-server-2008-and-windows-vista"></a>Para cambiar el nombre de los equipos que ejecutan Windows Server 2008 y Windows Vista

1.  Haz clic en **inicio**, haz clic en **equipo**y, a continuación, haz clic en **propiedades**. La **sistema** abre el cuadro de diálogo.

2.  En **configuración de nombre, dominio y grupo de trabajo del equipo**, haz clic en **cambiar la configuración de**. La **propiedades del sistema** abre el cuadro de diálogo.

    > [!NOTE]
    > En equipos que ejecutan Windows Vista, antes del **propiedades del sistema** se abrirá el cuadro de diálogo, la **Control de cuentas de usuario** aparece el cuadro de diálogo que solicita permiso para continuar. Haz clic en **continuar** para continuar.

3.  Haz clic en **cambio**. La **cambios de dominio o el nombre de equipo** abre el cuadro de diálogo.

4.  En **nombre de equipo**, escribe el nombre de tu equipo. Por ejemplo, si quieres un nombre al equipo DC1, escribe **DC1**.

5.  Haz clic en **Aceptar** dos veces, haz clic en **cerrar**y, a continuación, haz clic en **reiniciar ahora** para reiniciar el equipo.

## <a name="BKMK_B"></a>El apéndice B - configurar dirección IP estática direcciones
Este tema proporcionan procedimientos para configurar las direcciones IP estáticas en equipos que ejecutan los sistemas operativos siguientes:

-   [Windows Server 2008 R2](#bkmk_R2Cng_WS08R2IP)

-   [Windows Server 2008](#bkmk_NetFndtn_Pln_CfgStatic08)

### <a name="bkmk_R2Cng_WS08R2IP"></a>Windows Server 2008 R2
Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para realizar este procedimiento.

##### <a name="to-configure-a-static-ip-address-on-a-computer-running-windows-server-2008-r2"></a>Para configurar una dirección IP estática en un equipo que ejecute Windows Server 2008 R2

1.  Haz clic en **inicio**y, a continuación, haz clic en **Panel de Control**.

2.  En **Panel de Control**, haz clic en **red e Internet**. **Red e Internet** se abre.

    En **red e Internet**, haz clic en **centro de redes y recursos compartidos**. **Centro de redes y recursos compartidos** se abre.

3.  En **centro de redes y recursos compartidos**, haz clic en **cambiar la configuración del adaptador**. **Conexiones de red** se abre.

4.  En **conexiones de red**, haz clic en la conexión de red que quieras configurar y, a continuación, haz clic en **propiedades**.

5.  En **propiedades de conexión de área Local**, en **esta conexión utiliza los siguientes elementos**, selecciona **protocolo de Internet versión 4 (TCP/IPv4)**y, a continuación, haz clic en **propiedades**. La **propiedades de protocolo de Internet versión 4 (TCP/IPv4)** abre el cuadro de diálogo.

6.  En **propiedades de protocolo de Internet versión 4 (TCP/IPv4)**, en la **General**, haga clic **usar la siguiente dirección IP**. En **dirección IP**, escribe la dirección IP que quieras usar.

7.  Presione tab para colocar el cursor en **máscara de subred**. Un valor predeterminado para la máscara de subred se introduce automáticamente. Acepte la máscara de subred de forma predeterminada, o escribe la máscara de subred que quieras usar.

8.  En **puerta de enlace predeterminada**, escribe la dirección IP de la puerta de enlace predeterminada.

9. En **servidor DNS preferido**, escribe la dirección IP del servidor DNS. Si tienes previsto usar el equipo local como el servidor DNS preferido, escribe la dirección IP del equipo local.

10. En **servidor DNS alternativo**, escriba la dirección IP del servidor DNS alternativo, si procede. Si tienes previsto usar el equipo local como un servidor DNS alternativo, escribe la dirección IP del equipo local.

11. Haz clic en **Aceptar**y, a continuación, haz clic en **cerrar**.

### <a name="bkmk_NetFndtn_Pln_CfgStatic08"></a>Windows Server 2008
Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para realizar estos procedimientos.

##### <a name="to-configure-a-static-ip-address-on-a-computer-running-windows-server-2008"></a>Para configurar una dirección IP estática en un equipo que ejecute Windows Server 2008

1.  Haz clic en **inicio**y, a continuación, haz clic en **Panel de Control**.

2.  En **Panel de Control**, comprueba que **vista clásica** está seleccionado y, a continuación, haz doble clic en **centro de redes y recursos compartidos**.

3.  En **centro de redes y recursos compartidos**, en **tareas**, haz clic en **administrar conexiones de red**.

4.  En **conexiones de red**, haz clic en la conexión de red que quieras configurar y, a continuación, haz clic en **propiedades**.

5.  En **propiedades de conexión de área Local**, en **esta conexión utiliza los siguientes elementos**, selecciona **protocolo de Internet versión 4 (TCP/IPv4)**y, a continuación, haz clic en **propiedades**. La **propiedades de protocolo de Internet versión 4 (TCP/IPv4)** abre el cuadro de diálogo.

6.  En **propiedades de protocolo de Internet versión 4 (TCP/IPv4)**, en la **General**, haga clic **usar la siguiente dirección IP**. En **dirección IP**, escribe la dirección IP que quieras usar.

7.  Presione tab para colocar el cursor en **máscara de subred**. Un valor predeterminado para la máscara de subred se introduce automáticamente. Acepte la máscara de subred de forma predeterminada, o escribe la máscara de subred que quieras usar.

8.  En **puerta de enlace predeterminada**, escribe la dirección IP de la puerta de enlace predeterminada.

9. En **servidor DNS preferido**, escribe la dirección IP del servidor DNS. Si tienes previsto usar el equipo local como el servidor DNS preferido, escribe la dirección IP del equipo local.

10. En **servidor DNS alternativo**, escriba la dirección IP del servidor DNS alternativo, si procede. Si tienes previsto usar el equipo local como un servidor DNS alternativo, escribe la dirección IP del equipo local.

11. Haz clic en **Aceptar**y, a continuación, haz clic en **cerrar**.

## <a name="BKMK_C"></a>Apéndice C: - unir equipos al dominio
Puedes usar estos procedimientos para unirte a equipos que ejecutan Windows Server 2008 R2, Windows 7, Windows Server 2008 y Windows Vista al dominio.

-   [Windows Server 2008 R2 y Windows 7](#BKMK_c1)

-   [Windows Server 2008 y Windows Vista](#BKMK_c2)

> [!IMPORTANT]
> Para unirte a un equipo a un dominio, debes iniciar sesión el equipo con la cuenta de administrador local, o bien, si has iniciado sesión en el equipo con una cuenta de usuario que no tenga credenciales administrativas del equipo local, debes proporcionar las credenciales de la cuenta de administrador local durante el proceso de unir el equipo al dominio. Además, debes tener una cuenta de usuario en el dominio al que quieres unir el equipo. Durante el proceso de unir el equipo al dominio, se te pedirá sus credenciales de cuenta de dominio (nombre de usuario y contraseña).

### <a name="BKMK_c1"></a>Windows Server 2008 R2 y Windows 7
Pertenencia a **los usuarios del dominio**, o equivalente, es lo mínimo necesario para realizar este procedimiento.

##### <a name="to-join-computers-running-windows-server-2008-r2-and-windows-7-to-the-domain"></a>Para unirte a equipos que ejecutan Windows Server 2008 R2 y Windows 7 en el dominio

1.  Iniciar sesión en el equipo con la cuenta de administrador local.

2.  Haz clic en **inicio**, haz clic en **equipo**y, a continuación, haz clic en **propiedades**. La **sistema** abre el cuadro de diálogo.

3.  En **configuración de nombre, dominio y grupo de trabajo del equipo**, haz clic en **cambiar la configuración de**. La **propiedades del sistema** abre el cuadro de diálogo.

    > [!NOTE]
    > En equipos que ejecutan Windows 7, antes del **propiedades del sistema** se abrirá el cuadro de diálogo, la **Control de cuentas de usuario** aparece el cuadro de diálogo que solicita permiso para continuar. Haz clic en **continuar** para continuar.

4.  Haz clic en **cambio**. La **cambios de dominio o el nombre de equipo** abre el cuadro de diálogo.

5.  En **nombre de equipo**, en **miembro de**, selecciona **dominio**y, a continuación, escribe el nombre del dominio que desea unirse. Por ejemplo, si el nombre de dominio es corp.contoso.com, escriba **corp.contoso.com**.

6.  Haz clic en **Aceptar**. El **Windows Security** abre el cuadro de diálogo.

7.  En **cambios de dominio o el nombre de equipo**, en **nombre de usuario**, escribe el nombre de usuario y en **contraseña**, escribe la contraseña y, a continuación, haz clic en **Aceptar**. La **cambios de dominio o el nombre de equipo** abre el cuadro de diálogo, le da la bienvenida al dominio. Haz clic en **Aceptar**.

8.  La **cambios de dominio o el nombre de equipo** cuadro de diálogo muestra un mensaje que indica que debes reiniciar el equipo para aplicar los cambios. Haz clic en **Aceptar**.

9. En la **propiedades del sistema** cuadro de diálogo, en la **nombre de equipo**, haga clic **cerrar**. La **Microsoft Windows** cuadro de diálogo se abre y muestra un mensaje nuevo que indica que debes reiniciar el equipo para aplicar los cambios. Haz clic en **reiniciar ahora**.

### <a name="BKMK_c2"></a>Windows Server 2008 y Windows Vista
Pertenencia a **los usuarios del dominio**, o equivalente, es lo mínimo necesario para realizar este procedimiento.

##### <a name="to-join-computers-running-windows-server-2008-and-windows-vista-to-the-domain"></a>Para unirte a equipos que ejecutan Windows Server 2008 y Windows Vista al dominio

1.  Iniciar sesión en el equipo con la cuenta de administrador local.

2.  Haz clic en **inicio**, haz clic en **equipo**y, a continuación, haz clic en **propiedades**. La **sistema** abre el cuadro de diálogo.

3.  En **configuración de nombre, dominio y grupo de trabajo del equipo**, haz clic en **cambiar la configuración de**. La **propiedades del sistema** abre el cuadro de diálogo.

4.  Haz clic en **cambio**. La **cambios de dominio o el nombre de equipo** abre el cuadro de diálogo.

5.  En **nombre de equipo**, en **miembro de**, selecciona **dominio**y, a continuación, escribe el nombre del dominio que desea unirse. Por ejemplo, si el nombre de dominio es corp.contoso.com, escriba **corp.contoso.com**.

6.  Haz clic en **Aceptar**. El **Windows Security** abre el cuadro de diálogo.

7.  En **cambios de dominio o el nombre de equipo**, en **nombre de usuario**, escribe el nombre de usuario y en **contraseña**, escribe la contraseña y, a continuación, haz clic en **Aceptar**. La **cambios de dominio o el nombre de equipo** abre el cuadro de diálogo, le da la bienvenida al dominio. Haz clic en **Aceptar**.

8.  La **cambios de dominio o el nombre de equipo** cuadro de diálogo muestra un mensaje que indica que debes reiniciar el equipo para aplicar los cambios. Haz clic en **Aceptar**.

9. En la **propiedades del sistema** cuadro de diálogo, en la **nombre de equipo**, haga clic **cerrar**. La **Microsoft Windows** cuadro de diálogo se abre y muestra un mensaje nuevo que indica que debes reiniciar el equipo para aplicar los cambios. Haz clic en **reiniciar ahora**.

## <a name="BKMK_D"></a>Apéndice D: - iniciar sesión en el dominio
Puedes usar estos procedimientos para iniciar sesión en el dominio con equipos que ejecutan Windows Server 2008 R2, Windows 7, Windows Server 2008 y Windows Vista.

-   [Windows Server 2008 R2 y Windows 7](#BKMK_d1)

-   [Windows Server 2008 y Windows Vista](#BKMK_d2)

### <a name="BKMK_d1"></a>Windows Server 2008 R2 y Windows 7
Pertenencia a **los usuarios del dominio**, o equivalente, es lo mínimo necesario para realizar este procedimiento.

##### <a name="log-on-to-the-domain-using-computers-running-windows-server-2008-r2-and-windows-7"></a>Iniciar sesión en el dominio con equipos que ejecutan Windows Server 2008 R2 y Windows 7

1.  Cerrar sesión en el equipo o reiniciar el equipo.

2.  Presiona CTRL + ALT + SUPR. Aparecerá la pantalla de inicio de sesión.

3.  Haz clic en **cambiar de usuario**y, a continuación, haz clic en **otro usuario**.

4.  En **nombre de usuario**, escribe tu nombre de usuario y dominio en el formato *dominio\nombre de usuario*. Por ejemplo, para iniciar sesión en el dominio corp.contoso.com con una cuenta denominada **usuario-01**, tipo **CORP\User-01**.

5.  En **contraseña**, escriba la contraseña del dominio y, a continuación, haz clic en la flecha o presione ENTRAR.

### <a name="BKMK_d2"></a>Windows Server 2008 y Windows Vista
Pertenencia a **los usuarios del dominio**, o equivalente, es lo mínimo necesario para realizar este procedimiento.

##### <a name="log-on-to-the-domain-using-computers-running-windows-server-2008-and-windows-vista"></a>Iniciar sesión en el dominio con equipos que ejecutan Windows Server 2008 y Windows Vista

1.  Cerrar sesión en el equipo o reiniciar el equipo.

2.  Presiona CTRL + ALT + SUPR. Aparecerá la pantalla de inicio de sesión.

3.  Haz clic en **cambiar de usuario**y, a continuación, haz clic en **otro usuario**.

4.  En **nombre de usuario**, escribe tu nombre de usuario y dominio en el formato *dominio\nombre de usuario*. Por ejemplo, para iniciar sesión en el dominio corp.contoso.com con una cuenta denominada **usuario-01**, tipo **CORP\User-01**.

5.  En **contraseña**, escriba la contraseña del dominio y, a continuación, haz clic en la flecha o presione ENTRAR.

## <a name="BKMK_E"></a>Apéndice E - red principal hoja de preparación de planeamiento
Puedes usar esta hoja de preparación de planeamiento de red para recopilar la información necesaria para instalar una red principal. En este tema se proporciona tablas que contienen los elementos de configuración individuales para cada equipo de servidor para el que debe proporcionar información o valores específicos durante el proceso de instalación o configuración. Se proporcionan ejemplos de valores para cada elemento de configuración.

Para planear y realizar un seguimiento, se proporcionan los espacios en cada tabla para especificar los valores se usan para la implementación. Si inicias sesión valores relacionados con la seguridad en estas tablas, debes almacenar la información en una ubicación segura.

Los siguientes vínculos conducen a las secciones de este tema que proporcionan los elementos de configuración y los valores de ejemplo que se asocian con los procedimientos de implementación presentados en esta guía.

1.  [Instalar DNS y servicios de dominio de Active Directory](#BKMK_FndtnPrep_InstallAD)

    -   [Configurar una zona de búsqueda inversa de DNS](#BKMK_FndtnPrep_DNSRevrsLook)

2.  [Instalación de DHCP](#BKMK_FndtnPrep_InstallDHCP)

    -   [Creación de un intervalo de exclusión en DHCP](#BKMK_FndtnPrep_DHCP_Exclusn)

    -   [Crear un nuevo ámbito DHCP](#bkmk_NetFndtn_Pln_DHCP_NewScope)

3.  [Instalar el servidor de directivas de red (opcional)](#BKMK_FndtnPrep_InstallNPS)

### <a name="BKMK_FndtnPrep_InstallAD"></a>Instalar DNS y servicios de dominio de Active Directory
Las tablas de esta sección enumeran los elementos de configuración para la preinstalación y la instalación de los servicios de dominio de Active Directory (AD DS) y DNS.

##### <a name="pre-installation-configuration-items-for-ad-ds-and-dns"></a>Elementos de configuración de preinstalación de AD DS y DNS
Los siguientes tablas se elementos de configuración de preinstalación de lista como se describe en [configurar todos los servidores](#BKMK_configuringAll):

-   [Configurar una dirección IP estática](#BKMK_ip)

|Elementos de configuración|Valores de ejemplo|Valores|
|-----------------------|------------------|----------|
|Dirección IP|10.0.0.2||
|Máscara de subred|255.255.255.0||
|Puerta de enlace predeterminada|10.0.0.1||
|Servidor DNS preferido|127.0.0.1||
|Servidor DNS alternativo|10.0.0.15||

-   [Cambiar el nombre del equipo](#BKMK_rename)

|Elemento de configuración|Valor de ejemplo|Valor|
|----------------------|-----------------|---------|
|Nombre de equipo|DC1||

##### <a name="ad-ds-and-dns-installation-configuration-items"></a>Elementos de configuración de instalación de AD DS y DNS
Elementos de configuración para conocer el procedimiento de implementación de red de Windows Server Core [instalar AD DS y DNS para un bosque nuevo](#BKMK_installAD-DNS):

|Elementos de configuración|Valores de ejemplo|Valores|
|-----------------------|------------------|----------|
|Nombre completo de DNS|corp.contoso.com||
|Nivel funcional del bosque|Windows Server 2003||
|Ubicación de carpeta de base de datos de los servicios de dominio de Active Directory|E:\Configuration\\<br /><br />O bien, acepte la ubicación predeterminada.||
|Ubicación de carpeta de archivos del registro de los servicios de dominio de Active Directory|E:\Configuration\\<br /><br />O bien, acepte la ubicación predeterminada.||
|Ubicación de la carpeta SYSVOL de los servicios de dominio de directorio activo|E:\Configuration\\<br /><br />O acepte la ubicación predeterminada||
|Contraseña de administrador del modo de restauración de directorio|J * p2leO4$ F||
|Nombre de archivo de respuesta (opcional)|AD DS_AnswerFile||

#### <a name="BKMK_FndtnPrep_DNSRevrsLook"></a>Configurar una zona de búsqueda inversa de DNS

|Elementos de configuración|Valores de ejemplo|Valores|
|-----------------------|------------------|----------|
|Tipo de zona:|-Zona principal<br />-Zona secundaria<br />: Zona de código auxiliar||
|Tipo de zona<br /><br />**Almacenar la zona en Active Directory**|-Seleccionado<br />-No seleccionado||
|Ámbito de replicación de zona de Active Directory|-Para todos los servidores DNS en el bosque<br />-Para todos los servidores DNS en este dominio<br />-Para todos los controladores de dominio en este dominio<br />-Para todos los controladores de dominio especificados en el ámbito de esta partición de directorio||
|Nombre de la zona de búsqueda inversa<br /><br />(Tipo IP)|-Zona de búsqueda inversa IPv4<br />-Zona de búsqueda inversa IPv6||
|Nombre de la zona de búsqueda inversa<br /><br />(Id. de red)|10.0.0||

### <a name="BKMK_FndtnPrep_InstallDHCP"></a>Instalación de DHCP
Las tablas de esta sección muestran los elementos de configuración para la preinstalación y la instalación de DHCP.

##### <a name="pre-installation-configuration-items-for-dhcp"></a>Elementos de configuración de preinstalación de DHCP
Los siguientes tablas se elementos de configuración de preinstalación de lista como se describe en [configurar todos los servidores](#BKMK_configuringAll):

-   [Configurar una dirección IP estática](#BKMK_ip)

|Elementos de configuración|Valores de ejemplo|Valores|
|-----------------------|------------------|----------|
|Dirección IP|10.0.0.3||
|Máscara de subred|255.255.255.0||
|Puerta de enlace predeterminada|10.0.0.1||
|Servidor DNS preferido|10.0.0.2||
|Servidor DNS alternativo|10.0.0.15||

-   [Cambiar el nombre del equipo](#BKMK_rename)

|Elemento de configuración|Valor de ejemplo|Valor|
|----------------------|-----------------|---------|
|Nombre de equipo|DHCP1||

##### <a name="dhcp-installation-configuration-items"></a>Elementos de configuración de instalación de DHCP
Elementos de configuración para conocer el procedimiento de implementación de red de Windows Server Core [protocolo de configuración dinámica de Host (DHCP) instala](#BKMK_installDHCP):

|Elementos de configuración|Valores de ejemplo|Valores|
|-----------------------|------------------|----------|
|Enlaces de conexión de red|Ethernet||
|Configuración del servidor DNS|DC1||
|Dirección IP del servidor DNS preferido|10.0.0.2||
|Dirección IP de servidor DNS alternativo|10.0.0.15||
|Nombre del ámbito|Corp1||
|A partir de la dirección IP|10.0.0.1||
|Dirección IP final|10.0.0.254||
|Máscara de subred|255.255.255.0||
|Puerta de enlace predeterminada (opcional)|10.0.0.1||
|Duración de la concesión|8 días||
|Modo de funcionamiento del servidor DHCP IPv6|No está habilitado||

#### <a name="BKMK_FndtnPrep_DHCP_Exclusn"></a>Creación de un intervalo de exclusión en DHCP
Elementos de configuración para crear un intervalo de exclusión durante la creación de un ámbito en DHCP.

|Elementos de configuración|Valores de ejemplo|Valores|
|-----------------------|------------------|----------|
|Nombre del ámbito|Corp1||
|Descripción del ámbito|Subred office principal 1||
|Dirección IP de exclusión intervalo inicio|10.0.0.1||
|Dirección IP de exclusión intervalo final|10.0.0.15||

#### <a name="bkmk_NetFndtn_Pln_DHCP_NewScope"></a>Crear un nuevo ámbito DHCP
Elementos de configuración para conocer el procedimiento de implementación de red de Windows Server Core [crear y activar un nuevo ámbito DHCP](#BKMK_newscopeDHCP):

|Elementos de configuración|Valores de ejemplo|Valores|
|-----------------------|------------------|----------|
|Nombre del nuevo ámbito|Corp2||
|Descripción del ámbito|Subred office principal 2||
|(Intervalo de direcciones IP)<br /><br />Dirección IP de inicio|10.0.1.1||
|(Intervalo de direcciones IP)<br /><br />Dirección IP final|10.0.1.254||
|Longitud|8||
|Máscara de subred|255.255.255.0||
|(Intervalo de exclusión) Dirección IP de inicio|10.0.1.1||
|Dirección IP de exclusión intervalo final|10.0.1.15||
|Duración de la concesión<br /><br />Días<br /><br />Horas<br /><br />Minutos|-   8<br />-   0<br />-   0||
|Enrutador (puerta de enlace predeterminada)<br /><br />Dirección IP|10.0.1.1||
|Dominio DNS principal|corp.contoso.com||
|Servidor DNS<br /><br />Dirección IP|10.0.0.2||

### <a name="BKMK_FndtnPrep_InstallNPS"></a>Instalar el servidor de directivas de red (opcional)
Las tablas de esta sección muestran los elementos de configuración para la preinstalación y la instalación de NPS.

##### <a name="pre-installation-configuration-items"></a>Elementos de configuración de preinstalación
Tres tablas siguientes muestran los elementos de configuración antes de la instalación como se describe en [configurar todos los servidores](#BKMK_configuringAll):

-   [Configurar una dirección IP estática](#BKMK_ip)

|Elementos de configuración|Valores de ejemplo|Valores|
|-----------------------|------------------|----------|
|Dirección IP|10.0.0.4||
|Máscara de subred|255.255.255.0||
|Puerta de enlace predeterminada|10.0.0.1||
|Servidor DNS preferido|10.0.0.2||
|Servidor DNS alternativo|10.0.0.15||

-   [Cambiar el nombre del equipo](#BKMK_rename)

|Elemento de configuración|Valor de ejemplo|Valor|
|----------------------|-----------------|---------|
|Nombre de equipo|NPS1||

##### <a name="network-policy-server-installation-configuration-items"></a>Elementos de configuración de instalación de servidor de directivas de red
Elementos de configuración de los procedimientos de implementación de Windows Server Core red NPS [instalar el servidor de directivas de redes (NPS)](#BKMK_installNPS) y [registrar el servidor NPS en el dominio predeterminado](#BKMK_registerNPS).

-   No hay elementos de configuración adicionales son necesarias para instalar y registrar NPS.

