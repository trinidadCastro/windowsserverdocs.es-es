---
title: Server Certificate Deployment Overview
description: Este tema forma parte de la guía de implementación de certificados de servidor para las implementaciones cableadas e inalámbricas de 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: ca5c3e04-ae25-4590-97f3-0376a9c2a9a2
ms.author: lizross
author: eross-msft
ms.openlocfilehash: f8d1536bf3043df4071c01ebb46cd84665b23b8b
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87992835"
---
# <a name="server-certificate-deployment-overview"></a>Server Certificate Deployment Overview

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se incluyen las siguientes secciones.

-   [Componentes de implementación de certificados de servidor](#bkmk_components)

-   [Información general del proceso de implementación de certificados de servidor](#bkmk_process)

## <a name="server-certificate-deployment-components"></a><a name="bkmk_components"></a>Componentes de implementación de certificados de servidor
Puede usar esta guía para instalar Active Directory servicios de Certificate Server (AD CS) como una entidad de certificación (CA) raíz de empresa y para inscribir certificados de servidor en servidores que ejecutan el servidor de directivas de redes (NPS), el servicio de enrutamiento y acceso remoto (RRAS), o tanto NPS como RRAS.


Si implementa SDN con autenticación basada en certificados, los servidores deben usar un certificado de servidor para demostrar sus identidades a otros servidores, de forma que consigan comunicaciones seguras.

En la ilustración siguiente se muestran los componentes necesarios para implementar certificados de servidor en servidores de la infraestructura de SDN.

![Infraestructura necesaria para la implementación de certificados de servidor](../../../media/Nps-Certs/Nps-Certs.jpg)

> [!NOTE]
> En la ilustración anterior, se representan varios servidores: DC1, CA1, WEB1 y muchos servidores de SDN. En esta guía se proporcionan instrucciones para implementar y configurar CA1 y WEB1, y para configurar DC1, que en esta guía se da por supuesto que ya se ha instalado en la red. Si aún no ha instalado su dominio de Active Directory, puede hacerlo mediante la [Guía de red principal](../../core-network-guide.md) para Windows Server 2016.

Para obtener más información sobre cada elemento que se describe en la ilustración anterior, vea lo siguiente:

-   [CA1](#bkmk_ca1)

-   [WEB1](#bkmk_web1)

-   [DC1](#bkmk_dc1)

-   [NPS1](#bkmk_nps1)

### <a name="ca1-running-the-ad-cs-server-role"></a><a name="bkmk_ca1"></a>CA1 ejecutar el rol de servidor de AD CS
En este escenario, la entidad de certificación (CA) raíz de empresa también es una CA emisora. La entidad de certificación emite certificados a los equipos servidor que tienen los permisos de seguridad correctos para inscribir un certificado. Active Directory servicios de Certificate Server (AD CS) está instalado en CA1.

En el caso de redes de mayor tamaño o en los que las preocupaciones de seguridad proporcionen justificación, puede separar los roles de CA raíz y CA emisora, e implementar CA subordinadas que emiten CA.

En las implementaciones más seguras, la entidad de certificación raíz de empresa se desconecta y se protege físicamente.

#### <a name="capolicyinf"></a>CAPolicy. inf
Antes de instalar AD CS, configure el archivo CAPolicy. inf con la configuración específica de la implementación.

#### <a name="copy-of-the-ras-and-ias-servers-certificate-template"></a>Copia de la plantilla de certificado de **servidores RAS e IAS**
Al implementar certificados de servidor, realice una copia de la plantilla de certificado de **servidores RAS e IAS** y, a continuación, configure la plantilla según sus requisitos y las instrucciones de esta guía.

Utilice una copia de la plantilla en lugar de la plantilla original para que la configuración de la plantilla original se conserve para un posible uso futuro. Configure la copia de la plantilla **servidores RAS e IAS** para que la CA pueda crear certificados de servidor que emita a los grupos de Active Directory usuarios y equipos que especifique.

#### <a name="additional-ca1-configuration"></a>Configuración adicional de CA1
La entidad de certificación publica una lista de revocación de certificados (CRL) que los equipos deben comprobar para asegurarse de que los certificados que se presentan como prueba de identidad son certificados válidos y no se han revocado. Debe configurar la entidad de certificación con la ubicación correcta de la CRL para que los equipos sepan dónde buscar la CRL durante el proceso de autenticación.

### <a name="web1-running-the-web-services-iis-server-role"></a><a name="bkmk_web1"></a>WEB1 que ejecuta el rol de servidor de servicios web (IIS)
En el equipo que ejecuta el rol de servidor servidor Web (IIS), WEB1, debe crear una carpeta en el explorador de Windows para su uso como ubicación de la CRL y el AIA.

#### <a name="virtual-directory-for-the-crl-and-aia"></a>Directorio virtual de la CRL y el AIA
Después de crear una carpeta en el explorador de Windows, debe configurar la carpeta como un directorio virtual en el administrador de Internet Information Services (IIS), así como configurar la lista de control de acceso para el directorio virtual para permitir que los equipos tengan acceso a AIA y CRL una vez publicados allí.

### <a name="dc1-running-the-ad-ds-and-dns-server-roles"></a><a name="bkmk_dc1"></a>DC1 que ejecuta los roles de servidor AD DS y DNS
DC1 es el controlador de dominio y el servidor DNS de la red.

#### <a name="group-policy-default-domain-policy"></a>directiva de grupo Directiva de dominio predeterminada
Después de configurar la plantilla de certificado en la entidad de certificación, puede configurar la Directiva de dominio predeterminada en directiva de grupo para que los certificados se inscriban automáticamente en los servidores NPS y RAS. Directiva de grupo se configura en AD DS en el servidor DC1.

#### <a name="dns-alias-cname-resource-record"></a>Registro de recursos de alias DNS (CNAME)
Debe crear un registro de recursos de alias (CNAME) para el servidor web para asegurarse de que otros equipos puedan encontrar el servidor, así como el AIA y la CRL que se almacenan en el servidor. Además, el uso de un registro de recursos CNAME de alias proporciona flexibilidad para que pueda usar el servidor web para otros propósitos, como el hospedaje de sitios web y FTP.

### <a name="nps1-running-the-network-policy-server-role-service-of-the-network-policy-and-access-services-server-role"></a><a name="bkmk_nps1"></a>NPS1 ejecutar el servicio de rol servidor de directivas de redes del rol de servidor servicios de acceso y directivas de redes
El NPS se instala al realizar las tareas de la guía de red principal de Windows Server 2016, por lo que antes de realizar las tareas de esta guía, ya debe tener uno o varios NPSs instalados en la red.

#### <a name="group-policy-applied-and-certificate-enrolled-to-servers"></a>directiva de grupo aplicado y el certificado inscrito en los servidores
Después de configurar la plantilla de certificado y la inscripción automática, puede actualizar directiva de grupo en todos los servidores de destino. En este momento, los servidores inscriben el certificado de servidor de CA1.

### <a name="server-certificate-deployment-process-overview"></a><a name="bkmk_process"></a>Información general del proceso de implementación de certificados de servidor

> [!NOTE]
> Los detalles de cómo realizar estos pasos se proporcionan en la sección [implementación de certificados de servidor](../../../core-network-guide/cncg/server-certs/Server-Certificate-Deployment.md).

El proceso de configuración de la inscripción de certificados de servidor se produce en estas fases:

1.  En WEB1, instale el rol servidor Web (IIS).

2.  En DC1, cree un registro de alias (CNAME) para el servidor Web, WEB1.

3.  Configure el servidor web para hospedar la CRL de la CA y, a continuación, publique la CRL y copie el certificado de CA raíz de la empresa en el nuevo directorio virtual.

4.  En el equipo donde va a instalar AD CS, asigne una dirección IP estática al equipo, cambie el nombre del equipo, una el equipo al dominio y, a continuación, inicie sesión en el equipo con una cuenta de usuario que sea miembro de los grupos Admins. del dominio y administradores de organización.

5.  En el equipo en el que piensa instalar AD CS, configure el archivo CAPolicy. inf con la configuración específica de la implementación.

6.  Instale el rol de servidor de AD CS y realice una configuración adicional de la CA.

7.  Copie la CRL y el certificado de entidad de certificación de CA1 en el recurso compartido del servidor Web WEB1.

8.  En la CA, configure una copia de la plantilla de certificado de servidores RAS e IAS. La entidad de certificación emite certificados basados en una plantilla de certificado, por lo que debe configurar la plantilla para el certificado de servidor antes de que la CA pueda emitir un certificado.

9.  Configurar la inscripción automática de certificados de servidor en una directiva de grupo. Al configurar la inscripción automática, todos los servidores que haya especificado con Active Directory pertenencias a grupos recibirán automáticamente un certificado de servidor cuando se actualice directiva de grupo en cada servidor. Si agrega más servidores posteriormente, éstos también recibirán automáticamente un certificado de servidor.

10. Actualice directiva de grupo en los servidores. Cuando se actualiza directiva de grupo, los servidores reciben el certificado de servidor, que se basa en la plantilla que configuró en el paso anterior. El servidor utiliza este certificado para demostrar su identidad a los equipos cliente y otros servidores durante el proceso de autenticación.

    > [!NOTE]
    > Todos los equipos miembros de dominio reciben automáticamente el certificado de la entidad de certificación raíz de empresa sin la configuración de la inscripción automática. Este certificado es diferente del certificado de servidor que se configura y se distribuye mediante la inscripción automática. El certificado de la CA se instala automáticamente en el almacén de certificados de entidades de certificación raíz de confianza para todos los equipos miembros del dominio para que confíen en los certificados emitidos por esta CA.

10. Compruebe que todos los servidores hayan inscrito un certificado de servidor válido.