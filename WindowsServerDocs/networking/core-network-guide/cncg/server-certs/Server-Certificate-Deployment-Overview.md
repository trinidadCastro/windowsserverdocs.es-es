---
title: Introducción de implementación de certificados de servidor
description: Este tema es parte de la Guía de certificados de servidor de implementación para implementaciones de conexión inalámbrica y cableadas 802.1X
manager: brianlic
ms.topic: article
ms.assetid: ca5c3e04-ae25-4590-97f3-0376a9c2a9a2
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4650c99325def63fd0df25989c661230fada3dac
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="server-certificate-deployment-overview"></a>Introducción de implementación de certificados de servidor

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Este tema contiene las siguientes secciones.  
  
-   [Componentes de implementación de certificados de servidor](#bkmk_components)
  
-   [Información general sobre el proceso de implementación certificado servidor](#bkmk_process)
  
## <a name="bkmk_components"></a>Componentes de implementación de certificados de servidor
Puedes usar a esta guía para instalar servicios de certificados de Active Directory (AD CS) como una entidad de certificación de raíz (CA) de empresa y para inscribir los certificados de servidor a servidores que ejecutan el servicio de servidor de directivas de redes (NPS), enrutamiento y acceso remoto (RRAS), o NPS y RRAS.


Si implementas SDN con la autenticación basada en certificados, los servidores deben usar un certificado de servidor para probar sus identidades a otros servidores, de modo que disponer de comunicaciones seguras.
  
La siguiente ilustración muestra los componentes necesarios para implementar certificados de servidor en servidores en tu infraestructura SDN.
  
![Infraestructura necesaria de implementación de certificados de servidor](../../../media/Nps-Certs/Nps-Certs.jpg)  
  
> [!NOTE]  
> En la ilustración anterior, se muestran varios servidores: DC1, CA1, WEB1 y SDN muchos servidores. Esta guía brinda instrucciones para implementar y configurar CA1 y WEB1 y para configurar DC1, que en esta guía se da por hecho que ya se ha instalado en la red. Si no has instalado ya tu dominio de Active Directory, puede hacerlo mediante la [guía básica de red](https://technet.microsoft.com/library/mt604042.aspx) para Windows Server 2016.  
  
Para obtener más información sobre cada elemento que se muestra en la ilustración anterior, consulta los siguientes temas:  
  
-   [CA1](#bkmk_ca1)  
  
-   [WEB1](#bkmk_web1)  
  
-   [DC1](#bkmk_dc1)  
  
-   [NPS1](#bkmk_nps1)  
  
### <a name="bkmk_ca1"></a>CA1 ejecuta el rol de servidor de AD CS  
En este escenario, la entidad de certificación (CA) raíz de empresa también es una entidad de certificación emisora. La CA emite certificados a los equipos de servidor que tienen los permisos de seguridad correcta para inscribir un certificado. Los servicios de certificados de Active Directory (AD CS) está instalado en CA1.  
  
Para redes grandes o donde los problemas de seguridad proporcionan la justificación, puedes separar las funciones de raíz, CA y emisora e implementar CA subordinadas que son la CA emisora.  
  
En las implementaciones más seguras, se ha extraído de la CA de raíz de la empresa físicamente seguras y sin conexión.   
  
#### <a name="capolicyinf"></a>CAPolicy.inf  
Antes de instalar AD CS, configurar el archivo CAPolicy.inf con una configuración específica para la implementación.  
  
#### <a name="copy-of-the-ras-and-ias-servers-certificate-template"></a>Copia de la **servidores RAS e IAS** plantilla de certificado  
Cuando implementas certificados de servidor, hacer una copia de la **servidores RAS e IAS** plantilla de certificado y, a continuación, configurar la plantilla de acuerdo con los requisitos y las instrucciones que aparecen en esta guía.   
  
Usar una copia de la plantilla, en lugar de la plantilla original para que se conserva la configuración de la plantilla original para uso futuro posible. Configurar la copia de la **servidores RAS e IAS** plantilla para que la entidad de certificación pueda crear certificados de servidor que emite a los grupos de usuarios de Active Directory y equipos que especifiques.  
  
#### <a name="additional-ca1-configuration"></a>Configuración adicional de CA1  
La CA, publica una lista de revocación de certificados (CRL) que deben comprobar equipos para garantizar que los certificados que se presentan a ellos como prueba de identidad son válidos y no se han revocado. Debes configurar la entidad de certificación con la ubicación correcta de la CRL para que los equipos sepan dónde buscar la CRL durante el proceso de autenticación.  
  
### <a name="bkmk_web1"></a>Web1 ejecuta el rol de servidor de servicios Web (IIS)  
En el equipo que ejecuta el rol de servidor Web Server (IIS), WEB1, debes crear una carpeta en el Explorador de Windows para su uso como la ubicación CRL y AIA.  
  
#### <a name="virtual-directory-for-the-crl-and-aia"></a>Directorio virtual CRL y AIA  
Después de crear una carpeta en el Explorador de Windows, debes configurar la carpeta como un directorio virtual en Internet Information Services (IIS) Manager, así como configurar la lista de control de acceso del directorio virtual para permitir que los equipos para tener acceso a las AIA y CRL después de que se publiquen allí.  
  
### <a name="bkmk_dc1"></a>DC1 ejecutando los roles de servidor DNS y AD DS  
DC1 es el controlador de dominio y el servidor DNS de la red.  
  
#### <a name="group-policy-default-domain-policy"></a>Directiva de dominio predeterminada de directiva de grupo  
Después de configurar la plantilla de certificado en la entidad de certificación, puedes configurar la directiva de dominio predeterminada en la directiva de grupo para que los certificados son inscripción automática en servidores NPS y RAS. Directiva de grupo está configurada en AD DS en el servidor DC1.  
  
#### <a name="dns-alias-cname-resource-record"></a>DNS alias el registro de recursos (CNAME)  
Debes crear un registro de recursos de alias (CNAME) para el servidor Web para garantizar que otros equipos pueden encontrar el servidor, así como la AIA y la CRL que se almacenan en el servidor. Además, mediante un alias CNAME registro de recurso proporciona flexibilidad para que puedan usar el servidor Web para otros fines, como el alojamiento de sitios Web y FTP.  
  
### <a name="bkmk_nps1"></a>NPS1 ejecutando el servicio de rol de servidor de directivas de red del rol de servidor de servicios de acceso y directivas de redes  
El servidor NPS está instalado al realizar las tareas en la Guía de red de Windows Server 2016 principal, de modo que antes de realizar las tareas en esta guía, ya debe tener uno o más servidores NPS instalan en la red.  
  
#### <a name="group-policy-applied-and-certificate-enrolled-to-servers"></a>Aplica la directiva de grupo y certificado inscritos en los servidores  
Después de haber configurado la plantilla de certificado e inscripción automática, puede actualizar la directiva de grupo en todos los servidores de destino. En este momento, los servidores inscriben el certificado de servidor de CA1.  
  
### <a name="bkmk_process"></a>Información general sobre el proceso de implementación certificado servidor  
  
> [!NOTE]  
> Se proporcionan los detalles de cómo realizar estos pasos en la sección [implementación de certificados de servidor](../../../core-network-guide/cncg/server-certs/Server-Certificate-Deployment.md).  
  
El proceso de configuración de inscripción de certificados de servidor se produce en estas fases:  
  
1.  En WEB1, instala el rol de servidor Web (IIS).  
  
2.  En DC1, crea un registro de alias (CNAME) de tu servidor Web, WEB1.  
  
3.  Configurar tu servidor Web para hospedar la CRL de la entidad de certificación, a continuación, publicar la CRL y copia el certificado de CA de raíz de empresa en el directorio virtual nuevo.  
  
4.  En el equipo donde vas a instalar AD CS, asignar una dirección IP estática del equipo, cambiar el nombre del equipo y, a continuación, unir el equipo al dominio y, a continuación, iniciar sesión en el equipo con una cuenta de usuario que sea miembro del grupo de administradores de dominio y administradores de empresa.  
  
5.  En el equipo donde vas a instalar AD CS, configurar el archivo CAPolicy.inf con valores que son específicas de la implementación.  
  
6.  Instalar el rol de servidor de AD CS y realizar una configuración adicional de la CA.  
  
7.  Copia el certificado de CA y de CRL de CA1 al recurso compartido en el servidor Web WEB1.  
  
8.  En la CA, configurar una copia de la plantilla de certificado servidores RAS e IAS. La CA emite certificados basados en una plantilla de certificado, por lo que debes configurar la plantilla para el certificado de servidor antes de la CA puede emitir un certificado.  
  
9.  Configurar la inscripción automática de certificado de servidor en la directiva de grupo. Cuando configuras la inscripción automática, todos los servidores que especificaste con la pertenencia a grupos de Active Directory automáticamente reciben un certificado de servidor cuando se actualiza la directiva de grupo en cada servidor. Si agregas más servidores más adelante, se reciben automáticamente un certificado de servidor demasiado.  
  
10. Actualizar la directiva de grupo en los servidores. Cuando se actualiza la directiva de grupo, los servidores de reciban el certificado de servidor, que se basa en la plantilla que has configurado en el paso anterior. Este certificado se usa el servidor para probar su identidad en los equipos cliente y otros servidores durante el proceso de autenticación.  
  
    > [!NOTE]  
    > Todos los equipos miembros del dominio recibirán automáticamente el certificado de CA de raíz de la empresa sin la configuración de la inscripción automática. Este certificado es distinto que el certificado de servidor que vas a configuran y distribuir mediante el uso de la inscripción automática. El certificado de CA se instala automáticamente en el almacén de certificados de entidades de certificación raíz de confianza para todos los equipos miembro de dominio para que se confiar en los certificados emitidos por esta entidad emisora.   
  
10. Comprueba que todos los servidores han inscrito un certificado de servidor válido.  
  


