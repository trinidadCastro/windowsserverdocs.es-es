---
title: Roles de Servicios de Escritorio remoto
description: Describe los componentes de un servicio de hospedaje de escritorio.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/06/2018
ms.topic: article
author: heidilohr
manager: dougkim
ms.openlocfilehash: 48efbffa4f82b707b63e33e6416da43eb105221f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844726"
---
# <a name="remote-desktop-services-roles"></a>Roles de Servicios de Escritorio remoto

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este artículo se describe los roles dentro de un entorno de servicios de escritorio remoto.

## <a name="remote-desktop-session-host"></a>Host de sesión de Escritorio remoto

El Host de sesión de escritorio remoto (RD Session Host) contiene las aplicaciones basadas en sesiones y los escritorios que comparte con usuarios. Los usuarios obtener estos escritorios y aplicaciones a través de uno de los clientes de escritorio remoto que se ejecutan en Windows, MacOS, iOS y Android. Los usuarios también pueden conectarse a través de un explorador compatible con el cliente web.

Puede organizar los equipos de escritorio y aplicaciones en uno o varios servidores Host de sesión de escritorio remoto, denominados "colecciones". Puede personalizar estas colecciones para determinados grupos de usuarios dentro de cada inquilino. Por ejemplo, puede crear una colección donde un grupo de usuarios específico puede tener acceso a aplicaciones específicas, pero nadie fuera del grupo designado no podrá tener acceso a esas aplicaciones.

Para implementaciones pequeñas, puede instalar aplicaciones directamente en los servidores Host de sesión de escritorio remoto. Para implementaciones más grandes, se recomienda crear una imagen base y aprovisionamiento de máquinas virtuales desde esa imagen.

Puede expandir las colecciones mediante la adición de máquinas virtuales de servidor de Host de sesión de escritorio remoto a un conjunto de recopilación con cada máquina virtual RDSH dentro de una colección asignada al mismo conjunto de disponibilidad. Esto proporciona una mayor disponibilidad de la colección y aumenta la escala para admitir más usuarios o aplicaciones con mucha actividad de recursos.

En la mayoría de los casos, varios usuarios comparten el mismo servidor Host de sesión de escritorio remoto, que usa recursos de Azure para una solución de hospedaje de escritorio de manera más eficiente. En esta configuración, deben iniciar sesión en los usuarios a colecciones con cuentas no administrativas. También puedes usar algunos usuarios acceso administrativo completo a su escritorio remoto mediante la creación de colecciones de escritorios de sesión personal.

Puede personalizar aún más mediante la creación y carga un disco duro virtual con el sistema operativo Windows Server que puede usar como plantilla para crear nuevas máquinas virtuales de Host de sesión de escritorio remoto de equipos de escritorio.

Para obtener más información, consulta los artículos siguientes:

* [Servicios de escritorio remoto: almacenamiento de datos seguro](rds-plan-secure-data-storage.md)
* [Cargar un VHD generalizado y usarlo para crear nuevas máquinas virtuales en Azure](https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json)
* [Actualizar colección RDSH (plantilla ARM)](https://azure.microsoft.com/resources/templates/rds-update-rdsh-collection/)

## <a name="remote-desktop-connection-broker"></a>Agente de conexión a Escritorio remoto

Agente de conexión de escritorio remoto (RD Connection Broker) administra las conexiones a Escritorio remoto entrantes a las granjas de servidores de Host de sesión de escritorio remoto. Agente de conexión a Escritorio remoto controla las conexiones a ambas colecciones de colecciones de RemoteApp y escritorios completas. Agente de conexión a Escritorio remoto puede equilibrar la carga entre servidores de la colección al realizar conexiones nuevas. Si se desconecta una sesión, agente de conexión a Escritorio remoto se volverá a conectar el usuario para el servidor Host de sesión de escritorio remoto y su sesión interrumpida, lo que sigue existiendo en la granja de servidores Host de sesión de escritorio remoto.

Deberá instalar certificados digitales en el servidor de agente de conexión a Escritorio remoto y el cliente para admitir el inicio de sesión único y publicación de aplicaciones de coincidencia. Al desarrollar o probar una red, puede usar un certificado autofirmado y generado automáticamente. Sin embargo, la fecha de publicación de servicios requieren un certificado digital de una entidad de certificación de confianza. El nombre que asigne el certificado debe ser el mismo como el interno totalmente calificado nombre de dominio (FQDN) de la máquina virtual de agente de conexión a Escritorio remoto.

Puede instalar al agente de conexión a Escritorio remoto de Windows Server 2016 en la misma máquina virtual como AD DS para reducir los costos. Si necesita escalar horizontalmente a más usuarios, también puede agregar más máquinas virtuales de agente de conexión a Escritorio remoto en el mismo conjunto de disponibilidad para crear un clúster de agente de conexión a Escritorio remoto.

Para poder crear un clúster de agente de conexión a Escritorio remoto, debe implementar una base de datos de SQL Azure en el entorno del inquilino o crear un grupo de disponibilidad AlwaysOn de SQL Server.

Para obtener más información, consulta los artículos siguientes:

* [Agregar el servidor de agente de conexión a Escritorio remoto a la implementación y configuración de alta disponibilidad](rds-connection-broker-cluster.md)
* [Base de datos SQL](desktop-hosting-service.md#sql-database) en el servicio de hospedaje de escritorio.

## <a name="remote-desktop-gateway"></a>Puerta de enlace de Escritorio remoto

Puerta de enlace de escritorio remoto (puerta de enlace de RD) concede a los usuarios en el acceso a redes públicas en equipos de escritorio de Windows y las aplicaciones hospedadas en servicios de nube de Microsoft Azure.

El componente de puerta de enlace de escritorio remoto usa capa de Sockets seguros (SSL) para cifrar el canal de comunicaciones entre clientes y el servidor. La máquina virtual de puerta de enlace de escritorio remoto deben ser accesible a través de una dirección IP pública que permita las conexiones TCP entrantes al puerto 443 y las conexiones UDP entrantes al puerto 3391. Esto permite a los usuarios conectarse a través de internet mediante el protocolo de transporte de comunicaciones HTTPS y el protocolo UDP, respectivamente.

Los certificados digitales instalados en el servidor y cliente deben coincidir para que funcione. Al desarrollar o probar una red, puede usar un certificado autofirmado y generado automáticamente. Sin embargo, un servicio publicado requiere un certificado de una entidad de certificación de confianza. El nombre del certificado debe coincidir con el FQDN utilizado para tener acceso a la puerta de enlace de escritorio remoto, si el FQDN es el nombre DNS externa de la dirección IP pública o el registro DNS CNAME que apunte a la dirección IP pública.

Para los inquilinos con menos usuarios, los roles de acceso Web de escritorio remoto y puerta de enlace de escritorio remoto se pueden combinar en una sola máquina virtual para reducir los costos. También puede agregar más máquinas virtuales de puerta de enlace de escritorio remoto a una granja de servidores de puerta de enlace de escritorio remoto para aumentar la disponibilidad del servicio y escalar horizontalmente a más usuarios. Las máquinas virtuales de mayor tamaño granjas de servidores de puerta de enlace de escritorio remoto debe configurarse en un conjunto con equilibrio de carga. Afinidad de IP no es necesaria cuando se usa la puerta de enlace de escritorio remoto en una máquina virtual de Windows Server 2016, pero es cuando se está ejecutando en una máquina virtual de Windows Server 2012 R2.

Para obtener más información, consulta los artículos siguientes:

* [Agregar alta disponibilidad a la parte delantera de web Web de escritorio remoto y puerta de enlace](rds-rdweb-gateway-ha.md)
* [Servicios de escritorio remoto - acceso desde cualquier lugar](rds-plan-access-from-anywhere.md)
* [Servicios de escritorio remoto - autenticación multifactor](rds-plan-mfa.md)

## <a name="remote-desktop-web-access"></a>Acceso Web a Escritorio remoto

Remoto acceso Web de escritorio (acceso Web de RD) permite a los usuarios acceso a escritorios y aplicaciones a través de un portal web y se inicia a través de la aplicación de cliente de escritorio remoto de Microsoft nativa del dispositivo. Puede usar el portal web para publicar los escritorios de Windows y aplicaciones para Windows y dispositivos cliente que no son de Windows y puede publicar también selectivamente escritorios o aplicaciones a usuarios o grupos específicos.

Acceso Web de RD necesita Internet Information Services (IIS) funcione correctamente. Una conexión de protocolo seguro (HTTPS) proporciona un canal de comunicación cifrada entre los clientes y el servidor Web de escritorio remoto. La máquina virtual de acceso Web de RD debe ser accesible a través de una dirección IP pública que permita las conexiones TCP entrantes al puerto 443 para permitir que los usuarios del inquilino se conectan desde internet mediante el protocolo de transporte de comunicaciones HTTPS.

Coincidencia de los certificados digitales debe instalarse en el servidor y los clientes. Para fines de prueba y desarrollo, esto puede ser un certificado autofirmado y generado automáticamente. Para un servicio de la fecha de publicación, se debe obtener el certificado digital de una entidad de certificación de confianza. El nombre del certificado debe coincidir con el completo dominio nombre (FQDN) utilizado para tener acceso a acceso Web de escritorio remoto. FQDN posibles incluye el nombre DNS con orientación externa para la dirección IP pública y el registro DNS CNAME que apunte a la dirección IP pública.

Para los inquilinos con menos usuarios, puede reducir los costos mediante la combinación de las cargas de trabajo acceso Web de escritorio remoto y puerta de enlace de escritorio remoto en una sola máquina virtual. También puede agregar más máquinas virtuales de Web de escritorio remoto a una granja de servidores de acceso Web de RD para aumentar la disponibilidad del servicio y escalar horizontalmente a más usuarios. En una granja de servidores de acceso Web de RD con varias máquinas virtuales, tendrá que configurar las máquinas virtuales en un conjunto con equilibrio de carga.

Para obtener más información sobre cómo configurar el acceso Web de RD, consulte los artículos siguientes:

* [Configurar el cliente web de escritorio remoto para los usuarios](clients/remote-desktop-web-client-admin.md)
* [Crear e implementar una colección de servicios de escritorio remoto](rds-create-collection.md)
* [Crear una colección de servicios de escritorio remoto para escritorios y aplicaciones se ejecuten](rds-create-collection.md)

## <a name="remote-desktop-licensing"></a>Administración de licencias de Escritorio remoto

Servidores de licencias de escritorio remoto (RD Licensing) activados permiten a los usuarios conectarse a los servidores de Host de sesión de escritorio remoto que se hospedan los equipos de escritorio y aplicaciones del inquilino. Entornos del inquilino normalmente vienen con el servidor de licencias de escritorio remoto ya está instalado, pero para entornos alojados tendrá que configurar el servidor en modo por el usuario.

El proveedor de servicios debe suficientes licencias de acceso de suscriptor (sal) de RDS para cubrir todos los únicos (no simultáneos) los usuarios autorizados que inician sesión en el servicio de cada mes. Los proveedores de servicios pueden adquirir servicios de infraestructura de Microsoft Azure directamente y adquiera sal a través del programa contrato de licencia de proveedor de servicios de Microsoft (SPLA). Los clientes que buscan una solución de escritorio hospedada deben adquirir la solución hospedada completa (Azure y RDS) del proveedor de servicios.

Los inquilinos pequeños pueden reducir los costos mediante la combinación de los componentes de licencias de escritorio remoto en una sola máquina virtual y el servidor de archivos. Para proporcionar mayor disponibilidad del servicio, los inquilinos pueden implementar dos máquinas virtuales de servidor de licencias de escritorio remoto en el mismo conjunto de disponibilidad. Todos los servidores de escritorio remoto en el entorno del inquilino están asociados a ambos servidores de licencias de escritorio remoto para que los usuarios pueden conectarse a las nuevas sesiones incluso si uno de los servidores deja de funcionar.

Para obtener más información, consulta los artículos siguientes:

* [Licencia de su implementación de RDS con licencias de acceso de cliente (CAL)](rds-client-access-license.md)
* [Activar el servidor de licencias de servicios de escritorio remoto](rds-activate-license-server.md)
* [Realizar un seguimiento de las licencias de acceso de cliente de servicios de escritorio remoto (CAL de RDS)](rds-track-cals.md)
* [Licencias por volumen de Microsoft: las opciones para los proveedores de servicios de licencia](https://www.microsoft.com/en-us/Licensing/licensing-programs/spla-program.aspx)