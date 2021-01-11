---
title: Roles de Servicios de Escritorio remoto
description: Obtenga información sobre los roles de un entorno de Servicios de Escritorio remoto en Windows Server.
ms.author: helohr
ms.date: 07/06/2018
ms.topic: article
author: heidilohr
manager: lizross
ms.openlocfilehash: f99ad1529eb0882fc979240bab6f82f8779f0d49
ms.sourcegitcommit: 9e19436bd8b20af60284071ab512405aebfbec83
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/29/2020
ms.locfileid: "97811422"
---
# <a name="remote-desktop-services-roles"></a>Roles de Servicios de Escritorio remoto

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019 y Windows Server 2016

En este artículo se describen los roles de un entorno de Servicios de Escritorio remoto.

## <a name="remote-desktop-session-host"></a>Host de sesión de escritorio remoto

El host de sesión de Escritorio remoto contiene las aplicaciones y escritorios basados en sesiones que compartes con los usuarios. Los usuarios acceden a estos escritorios y aplicaciones mediante alguno de los clientes de Escritorio remoto que se ejecutan en Windows, MacOS, iOS y Android. Los usuarios también pueden conectarse mediante un explorador compatible con el cliente web.

Puedes organizar los equipos de escritorio y las aplicaciones en uno o varios servidores host de sesión de Escritorio remoto, denominados "colecciones". Puedes personalizar estas colecciones para determinados grupos de usuarios dentro de cada inquilino. Por ejemplo, puedes crear una colección en la que un grupo de usuarios concreto puede acceder a aplicaciones específicas, pero nadie fuera del grupo designado podrá acceder a esas aplicaciones.

Para implementaciones pequeñas, puedes instalar las aplicaciones directamente en los servidores host de sesión de Escritorio remoto. Para implementaciones más grandes, se recomienda crear una imagen base y aprovisionar máquinas virtuales desde ella.

Puedes expandir las colecciones mediante la adición de máquinas virtuales del servidor host de sesión de Escritorio remoto a una granja de colecciones en la que cada máquina virtual de este servidor se asigna al mismo conjunto de disponibilidad. Esto proporciona una mayor disponibilidad de la colección y aumenta el escalado para admitir más usuarios o aplicaciones con mucha actividad de recursos.

En la mayoría de los casos, varios usuarios comparten el mismo servidor host de sesión de Escritorio remoto, el cual usa recursos de Azure de la manera más eficaz para una solución de hospedaje de escritorio. En esta configuración, los usuarios deben iniciar sesión en las colecciones con cuentas no administrativas. También puedes proporcionar a algunos usuarios acceso administrativo completo a su escritorio remoto mediante la creación de colecciones de escritorios de sesión personal.

Puedes personalizar aún más los escritorios mediante la creación y carga de un disco duro virtual con el sistema operativo Windows Server que puedes usar como plantilla para crear nuevas máquinas virtuales de host de sesión de Escritorio remoto.

Para obtener más información, consulta los artículos siguientes:

* [Servicios de Escritorio remoto: Almacenamiento de datos seguro](rds-plan-secure-data-storage.md)
* [Carga de un disco duro virtual generalizado y cómo usarlo para crear nuevas máquinas virtuales en Azure](/azure/virtual-machines/windows/upload-generalized-managed?toc=/azure/virtual-machines/windows/toc.json)
* [Actualización de una colección de host de sesión de Escritorio remoto (plantilla de ARM)](https://azure.microsoft.com/resources/templates/rds-update-rdsh-collection/)

## <a name="remote-desktop-connection-broker"></a>Agente de conexión a Escritorio remoto

El servicio Agente de conexión a Escritorio remoto administra las conexiones de Escritorio remoto de entrada a las granjas de servidores host de sesión de Escritorio remoto. El Agente de conexión a Escritorio remoto controla las conexiones de las colecciones de escritorios completos y las de aplicaciones remotas. El Agente de conexión a Escritorio remoto puede equilibrar la carga entre los servidores de la colección al realizar nuevas conexiones. Si se desconecta una sesión, el Agente de conexión a Escritorio remoto volverá a conectar al usuario al servidor host de sesión de Escritorio remoto correcto y a la sesión interrumpida, la cual todavía existe en la granja del host de sesión de Escritorio remoto.

Deberás instalar certificados de seguridad que coincidan en el servidor del Agente de conexión a Escritorio remoto y en el cliente para que admita el inicio de sesión único y la publicación de aplicaciones. Cuando desarrollas o pruebas una red, puedes usar un certificado autofirmado y autogenerado. Sin embargo, los servicios publicados requieren un certificado digital de una entidad de certificación de confianza. El nombre que le asignes al certificado debe ser el mismo que el nombre de dominio completo (FQDN) de la máquina virtual del Agente de conexión a Escritorio remoto.

Puedes instalar el agente de conexión a Escritorio remoto de Windows Server 2016 en la misma máquina virtual que AD DS para reducir los costos. Si necesitas escalar horizontalmente para incluir más usuarios, también puedes agregar más máquinas virtuales del Agente de conexión a Escritorio remoto en el mismo conjunto de disponibilidad para crear un clúster de Agente de conexión a Escritorio remoto.

Para poder crear este clúster, debes implementar una base de datos de Azure SQL en el entorno del inquilino o crear un grupo de disponibilidad AlwaysOn de SQL Server.

Para obtener más información, consulta los artículos siguientes:

* [Adición del servidor de Agente de conexión a Escritorio remoto para la implementación y la configuración de alta disponibilidad](rds-connection-broker-cluster.md)
* [Base de datos SQL](desktop-hosting-service.md#sql-database) en el servicio de hospedaje de escritorio.

## <a name="remote-desktop-gateway"></a>Puerta de enlace de Escritorio remoto

La puerta de enlace de Escritorio remoto concede a los usuarios de las redes públicas acceso a los escritorios y aplicaciones de Windows hospedados en los servicios en la nube de Microsoft Azure.

Este componente usa la capa de sockets seguros (SSL) para cifrar el canal de comunicación entre los clientes y el servidor. La máquina virtual de la puerta de enlace de Escritorio remoto debe ser accesible mediante una dirección IP pública que permita las conexiones TCP entrantes al puerto 443 y las conexiones UDP entrantes al puerto 3391. Esto permite a los usuarios conectarse a través de Internet mediante el protocolo de transporte de comunicaciones HTTPS y el protocolo UDP, respectivamente.

Los certificados digitales instalados en el servidor y el cliente deben coincidir para que esto funcione. Cuando desarrollas o pruebas una red, puedes usar un certificado autofirmado y autogenerado. Sin embargo, un servicio publicado requiere un certificado de una entidad de certificación de confianza. El nombre del certificado debe coincidir con el FQDN utilizado para acceder a la puerta de enlace de Escritorio remoto, tanto si este es el nombre DNS que se muestra externamente de la dirección IP pública o si es el registro DNS de CNAME que apunta a esta dirección.

Para los inquilinos con menos usuarios, los roles de acceso web de Escritorio remoto y puerta de enlace de Escritorio remoto se pueden combinar en una sola máquina virtual para reducir los costos. También puedes agregar más máquinas virtuales de puerta de enlace de Escritorio remoto a una granja de servidores de puerta de enlace de Escritorio remoto para aumentar la disponibilidad del servicio y escalar horizontalmente para incluir a más usuarios. Las máquinas virtuales de las granjas de servidores de puerta de enlace de Escritorio remoto se deben configurar en un conjunto con equilibrio de carga. La afinidad de IP no es necesaria si usa la puerta de enlace de Escritorio remoto en una máquina virtual de Windows Server 2016, pero sí lo es cuando la ejecuta en una de Windows Server 2012 R2.

Para obtener más información, consulta los artículos siguientes:

* [Agregar alta disponibilidad al front-end web de la puerta de enlace de Escritorio remoto y web de RD](rds-rdweb-gateway-ha.md)
* [Servicios de Escritorio remoto: Acceso desde cualquier lugar](rds-plan-access-from-anywhere.md)
* [Servicios de Escritorio remoto: Autenticación multifactor](rds-plan-mfa.md)

## <a name="remote-desktop-web-access"></a>Acceso web de Escritorio remoto.

Acceso web de Escritorio remoto permite a los usuarios acceder a escritorios y aplicaciones por medio de un portal web y los inicia mediante la aplicación cliente de Escritorio remoto de Microsoft nativa del dispositivo. Puedes usar el portal web para publicar los escritorios y aplicaciones de Windows en dispositivos cliente de Windows y en otros que no son de Windows, y puedes también publicar selectivamente escritorios o aplicaciones para usuarios o grupos específicos.

El acceso web de Escritorio remoto necesita Internet Information Services (IIS) para funcionar correctamente. Una conexión segura de protocolo de transferencia de hipertexto (HTTPS) proporciona un canal de comunicación cifrado entre los clientes y el servidor web de Escritorio remoto. La máquina virtual de acceso web de Escritorio remoto debe ser accesible mediante una dirección IP pública que permita las conexiones TCP entrantes al puerto 443 de forma que los usuarios del inquilino puedan conectarse desde Internet mediante el protocolo de transporte de comunicaciones HTTPS.

Se deben instalar certificados de seguridad coincidentes en el servidor y en los clientes. Con fines de prueba y desarrollo, este puede ser un certificado autofirmado y autogenerado. En caso de un servicio publicado, se debe obtener un certificado de seguridad de una entidad de certificación de confianza. El nombre del certificado debe coincidir con el nombre de dominio completo (FQDN) utilizado para el acceso web de Escritorio remoto. Entre los FQDN posibles se incluyen el nombre DNS que se muestra externamente para la dirección IP pública y el registro DNS de CNAME que apunta a esta.

Para los inquilinos con menos usuarios, puedes reducir los costos combinando las cargas de trabajo de acceso web de Escritorio remoto y las de puerta de enlace de Escritorio remoto en una sola máquina virtual. También puedes agregar más máquinas virtuales de acceso web de Escritorio remoto a una granja de servidores de acceso web de Escritorio remoto para aumentar la disponibilidad del servicio y escalar horizontalmente para incluir a más usuarios. En una granja de servidores de acceso web de Escritorio remoto con varias máquinas virtuales, tendrás que configurar las máquinas virtuales en un conjunto con equilibrio de carga.

Para más información sobre cómo configurar el acceso web de Escritorio remoto, consulta los artículos siguientes:

* [Configurar el cliente de web de Escritorio remoto para los usuarios](clients/remote-desktop-web-client-admin.md)
* [Crear e implementar una colección de Servicios de Escritorio remoto](rds-create-collection.md)
* [Creación de una colección de servicios de Escritorio remoto para que se ejecuten escritorios y aplicaciones](rds-create-collection.md)

## <a name="remote-desktop-licensing"></a>Administración de licencias de Escritorio remoto

Los servidores activados de administración de licencias de Escritorio remoto permiten a los usuarios conectarse a los servidores del host de sesión de Escritorio remoto que hospedan los escritorios y aplicaciones del inquilino. Los entornos de inquilino normalmente vienen con el servidor de administración de licencias de Escritorio remoto ya instalado, pero para entornos hospedados tendrás que configurar el servidor en modo por usuario.

El proveedor de servicios necesita suficientes licencias de acceso de suscriptor de RDS para abarcar a todos los usuarios autorizados únicos (no simultáneos) que inician sesión en el servicio cada mes. Los proveedores de servicios pueden adquirir Servicios de infraestructura de Microsoft Azure directamente y pueden adquirir las licencias de acceso de suscriptor mediante el programa del Contrato de licencias del proveedor de servicios (SPLA) de Microsoft. Los clientes que buscan una solución de escritorio hospedada deben adquirir la solución completa (Azure y RDS) del proveedor de servicios.

Los inquilinos pequeños pueden reducir los costos combinando los componentes del servidor de archivos y de administración de licencias de Escritorio remoto en una única máquina virtual. Para proporcionar una mayor disponibilidad del servicio, los inquilinos pueden implementar dos máquinas virtuales de servidor de licencias de Escritorio remoto en el mismo conjunto de disponibilidad. Todos los servidores de Escritorio remoto en el entorno del inquilino se asocian a ambos servidores de licencias de Escritorio remoto para que los usuarios puedan conectarse a las nuevas sesiones incluso si uno de los servidores deja de funcionar.

Para obtener más información, consulta los artículos siguientes:

* [Licencia para la implementación de RDS con licencias de acceso de cliente (CAL)](rds-client-access-license.md)
* [Activación del servidor de licencias de Servicios de Escritorio remoto](rds-activate-license-server.md)
* [Seguimiento de las licencias de acceso de cliente para Servicios de Escritorio remoto (CAL de RDS)](rds-track-cals.md)
* [Programa de licencias por volumen de Microsoft: Opciones de licencia para los proveedores de servicios](https://www.microsoft.com/Licensing/licensing-programs/spla-program.aspx)
