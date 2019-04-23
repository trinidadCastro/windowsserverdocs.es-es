---
title: Server Certificate Deployment Overview
description: Este tema forma parte de la Guía de implementación de servidores de certificados para las implementaciones inalámbricas y cableadas 802.1X
manager: brianlic
ms.topic: article
ms.assetid: ca5c3e04-ae25-4590-97f3-0376a9c2a9a2
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0cafa4bdeb80b22d6bac4ad09bcae9436cda97c8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877826"
---
# <a name="server-certificate-deployment-overview"></a>Server Certificate Deployment Overview

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se incluyen las siguientes secciones.  
  
-   [Componentes de implementación del certificado de servidor](#bkmk_components)
  
-   [Introducción al proceso de implementación de certificado de servidor](#bkmk_process)
  
## <a name="bkmk_components"></a>Componentes de implementación del certificado de servidor
Puede usar a esta guía para instalar servicios de certificados de Active Directory (AD CS) como una entidad de certificación de raíz (CA) empresarial y para inscribir certificados de servidor en servidores que ejecutan el servicio servidor de directivas de redes (NPS), enrutamiento y acceso remoto (RRAS), o NPS y RRAS.


Si implementa SDN con la autenticación basada en certificados, los servidores deben usar un certificado de servidor para demostrar su identidad a otros servidores para poder disponer de comunicaciones seguras.
  
La siguiente ilustración muestra los componentes necesarios para implementar certificados de servidor en servidores de la infraestructura SDN.
  
![Infraestructura necesaria de implementación del certificado de servidor](../../../media/Nps-Certs/Nps-Certs.jpg)  
  
> [!NOTE]  
> En la ilustración anterior, se muestran varios servidores: DC1, CA1, WEB1 y muchos de los servidores SDN. Esta guía proporciona instrucciones para implementar y configurar CA1 y WEB1 y para configurar DC1, que en esta guía se da por supuesto que ya ha instalado en la red. Si no ha instalado ya su dominio de Active Directory, puede hacerlo mediante el uso de la [Guía de red principal](https://technet.microsoft.com/library/mt604042.aspx) para Windows Server 2016.  
  
Para obtener más información sobre cada elemento que se muestra en la ilustración anterior, vea lo siguiente:  
  
-   [CA1](#bkmk_ca1)  
  
-   [WEB1](#bkmk_web1)  
  
-   [DC1](#bkmk_dc1)  
  
-   [NPS1](#bkmk_nps1)  
  
### <a name="bkmk_ca1"></a>CA1 que ejecuta el rol de servidor de AD CS  
En este escenario, la entidad de certificación (CA) raíz de empresa también es una CA emisora. La entidad de certificación emite certificados a equipos servidores que tengan los permisos de seguridad correctas para inscribir un certificado. Los servicios de certificados de Active Directory (AD CS) se instala en CA1.  
  
Para redes de mayor tamaño o donde los problemas de seguridad proporcionan una justificación, puede separar los roles de la entidad emisora raíz y entidad de certificación emisora e implementar las CA subordinadas que son la CA emisora.  
  
En las implementaciones más seguras, se toma la entidad de certificación raíz de empresa físicamente segura y sin conexión.   
  
#### <a name="capolicyinf"></a>Archivo CAPolicy.inf  
Antes de instalar AD CS, configurar el archivo CAPolicy.inf con una configuración específica para la implementación.  
  
#### <a name="copy-of-the-ras-and-ias-servers-certificate-template"></a>Copia de la **servidores RAS e IAS** plantilla de certificado  
Al implementar los certificados de servidor, realice una copia de la **servidores RAS e IAS** plantilla de certificado y, a continuación, configure la plantilla según sus requisitos y las instrucciones que aparecen en esta guía.   
  
Utilizar una copia de la plantilla en lugar de la plantilla original para que la configuración de la plantilla original se conserva para su uso futuro posibles. Configurar la copia de la **servidores RAS e IAS** plantilla para que la entidad de certificación puede crear certificados de servidor que emite a los grupos de usuarios de Active Directory y los equipos que especifique.  
  
#### <a name="additional-ca1-configuration"></a>Configuración adicional de CA1  
La entidad emisora de certificados publica una lista de revocación de certificados (CRL) que deben comprobar los equipos para asegurarse de que los certificados que se presentan a ellos como prueba de identidad son certificados válidos y no se han revocado. Debe configurar la entidad de certificación con la ubicación correcta de la CRL para que los equipos sepan dónde buscar la CRL durante el proceso de autenticación.  
  
### <a name="bkmk_web1"></a>Web1 que ejecuta el rol de servidor de servicios Web (IIS)  
En el equipo que ejecuta el rol de servidor servidor Web (IIS), WEB1, debe crear una carpeta en el Explorador de Windows para su uso como la ubicación de la CRL y AIA.  
  
#### <a name="virtual-directory-for-the-crl-and-aia"></a>Directorio virtual para la CRL y AIA  
Después de crear una carpeta en el Explorador de Windows, debe configurar la carpeta como un directorio virtual en el Administrador de Internet Information Services (IIS), así como configurar la lista de control de acceso del directorio virtual para permitir que los equipos para tener acceso el AIA y CRL una vez que se publiquen allí.  
  
### <a name="bkmk_dc1"></a>DC1 ejecutando los roles de servidor AD DS y DNS  
DC1 es el controlador de dominio y servidor DNS en la red.  
  
#### <a name="group-policy-default-domain-policy"></a>Directiva de dominio predeterminado de directiva de grupo  
Después de configurar la plantilla de certificado en la entidad de certificación, puede configurar la directiva predeterminada de dominio en la directiva de grupo para que los certificados estén inscritos automáticamente en servidores NPS y RAS. Directiva de grupo está configurada en AD DS en el servidor DC1.  
  
#### <a name="dns-alias-cname-resource-record"></a>Registro de recursos de alias (CNAME) de DNS  
Debe crear un registro de recursos de alias (CNAME) para el servidor Web para asegurarse de que otros equipos puedan encontrar el servidor, así como el AIA y la CRL que se almacenan en el servidor. Además, mediante un registro de recurso CNAME alias proporciona flexibilidad para que pueda usar el servidor Web para otros propósitos, como el alojamiento de sitios Web y FTP.  
  
### <a name="bkmk_nps1"></a>NPS1 que ejecuta el servicio de rol de servidor de directivas de red del rol de servidor Servicios de acceso y directivas de redes  
NPS se instala al realizar las tareas en la Guía de red de Windows Server 2016 Core, por lo que antes de realizar las tareas de esta guía, debe tener ya instalación NPSs uno o más en su red.  
  
#### <a name="group-policy-applied-and-certificate-enrolled-to-servers"></a>Aplica la directiva de grupo y el certificado inscrito a servidores  
Después de configurar la plantilla de certificado y la inscripción automática, puede actualizar la directiva de grupo en todos los servidores de destino. En este momento, los servidores de inscriben el certificado de servidor de CA1.  
  
### <a name="bkmk_process"></a>Introducción al proceso de implementación de certificado de servidor  
  
> [!NOTE]  
> Se proporcionan los detalles de cómo realizar estos pasos en la sección [implementación del certificado de servidor](../../../core-network-guide/cncg/server-certs/Server-Certificate-Deployment.md).  
  
Se produce el proceso de inscripción de certificados de servidor de configuración en estas fases:  
  
1.  En WEB1, instale el rol servidor Web (IIS).  
  
2.  En DC1, cree un registro de alias (CNAME) para el servidor Web, WEB1.  
  
3.  Configurar el servidor Web para hospedar la CRL de la entidad de certificación, a continuación, publicar la CRL y copie el certificado de CA raíz de empresa en el nuevo directorio virtual.  
  
4.  En el equipo donde va a instalar AD CS, asignar el equipo una dirección IP estática, el nombre del equipo y, a continuación, unir el equipo al dominio y, a continuación, inicie sesión en el equipo con una cuenta de usuario que sea miembro de los grupos Admins. del dominio y administradores de empresas.  
  
5.  En el equipo donde va a instalar AD CS, configurar el archivo CAPolicy.inf con parámetros que son específicas de la implementación.  
  
6.  Instale el rol de servidor de AD CS y realizar una configuración adicional de la entidad de certificación.  
  
7.  Copie el certificado CRL y entidad de certificación de CA1 al recurso compartido en el servidor Web WEB1.  
  
8.  En la entidad de certificación, configure una copia de la plantilla de certificado servidores RAS e IAS. La entidad de certificación emite certificados basados en una plantilla de certificado, por lo que debe configurar la plantilla para el certificado de servidor antes de que la CA pueda emitir un certificado.  
  
9.  Configurar la inscripción automática de certificados de servidor en una directiva de grupo. Al configurar la inscripción automática, todos los servidores que ha especificado con la pertenencia a grupos de Active Directory automáticamente reciben un certificado de servidor cuando se actualiza la directiva de grupo en cada servidor. Si agrega más servidores posteriormente, recibirán automáticamente un certificado de servidor demasiado.  
  
10. Actualizar directiva de grupo de servidores. Cuando se actualiza la directiva de grupo, los servidores reciben el certificado de servidor, que se basa en la plantilla que ha configurado en el paso anterior. Este certificado se usa el servidor para demostrar su identidad a los equipos cliente y otros servidores durante el proceso de autenticación.  
  
    > [!NOTE]  
    > Todos los equipos miembros del dominio reciben automáticamente el certificado de la CA raíz de empresa sin necesidad de la configuración de la inscripción automática. Este certificado es diferente que el certificado de servidor, podrá configurar y distribuir mediante el uso de la inscripción automática. El certificado de CA se instala automáticamente en el almacén de certificados entidades emisoras raíz de confianza para todos los equipos miembros del dominio para que confíen los certificados emitidos por esta CA.   
  
10. Compruebe que todos los servidores han inscrito un certificado de servidor válido.  
  


