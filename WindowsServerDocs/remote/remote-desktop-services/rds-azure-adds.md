---
title: Servicios de dominio de AD de Azure y servicios de escritorio remoto
description: Obtenga información sobre cómo integrar los servicios de dominio de AD de Azure en la implementación de RDS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 10/02/2017
ms.tgt_pltfrm: na
ms.topic: article
author: christianmontoya
ms.localizationpriority: medium
ms.openlocfilehash: e60cf70f1f91ad87046bedf024fe9afc459075b6
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "1614524"
---
# <a name="integrate-azure-ad-domain-services-with-your-rds-deployment"></a>Integrar los servicios de dominio de AD de Azure con la implementación de RDS

Puede usar los [Servicios de dominio de AD de Azure](/azure/active-directory-domain-services/active-directory-ds-overview) (Azure AD DS) en su implementación de servicios de escritorio remoto en el lugar de Active Directory de Windows Server. Azure AD DS le permite usar sus identidades de Azure AD existentes con las cargas de trabajo de Windows clásicos.

Con Azure AD DS se puede: 
- Crear un entorno de Azure con un dominio local para las organizaciones de nacimiento-in-the-cloud. 
- Crear un entorno aislado de Azure con las mismas identidades usadas para su local y el entorno en línea, sin necesidad de crear una VPN de sitio a sitio o de ExpressRoute. 

Al finalizar la integración de Azure AD DS en la implementación de escritorio remoto, su arquitectura tendrá un aspecto similar al siguiente:

![Un diagrama de arquitectura que muestra RDS con Azure AD DS](media/aadds-rds.png)

Para ver cómo se compara esta arquitectura con otros escenarios de implementación de RDS, desproteger [las arquitecturas de servicios de escritorio remoto](desktop-hosting-logical-architecture.md).

Para obtener una mejor comprensión de Azure AD DS, consulte la [información general de Azure AD DS](/azure/active-directory-domain-services/active-directory-ds-overview) y [cómo decidir si Azure AD DS es el adecuado para su caso de uso](/azure/active-directory-domain-services/active-directory-ds-comparison).

Use la siguiente información para implementar Azure AD DS con RDS.

## <a name="prerequisites"></a>Requisitos previos

Antes puede llevar sus identidades de Azure AD para usar en una implementación de RDS, [Configurar Azure AD para guardar las contraseñas con algoritmo hash de las identidades de los usuarios](/azure/active-directory-domain-services/active-directory-ds-getting-started-password-sync). Las organizaciones de nacimiento-in-the-cloud no es necesario realizar ningún cambio adicional en su directorio; Sin embargo, las organizaciones locales necesitan permitir que los hashes de contraseña debe sincronizarse y se almacenan en Azure AD, que puede no estar autorizada para algunas organizaciones. Los usuarios tendrán que restablecer sus contraseñas después de realizar este cambio de configuración.

## <a name="deploy-azure-ad-ds-and-rds"></a>Implementar RDS y Azure AD DS 
Siga estos pasos para implementar Azure AD DS y RDS.

1. Habilitar [Azure AD DS](/azure/active-directory-domain-services/active-directory-ds-getting-started). Tenga en cuenta que el artículo vinculado hace lo siguiente:
   - Recorra la creación de la adecuada grupos de Azure AD para administración de dominio.
   - Resalte cuando haya que forzar a los usuarios cambiar su contraseña de modo que sus cuentas pueden trabajar con Azure AD DS.
   
2. Configurar RDS. Puede usar una plantilla de Azure o implementación manual de RDS.
   - Use la [plantilla existente AD](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/). Asegúrese de personalizar los siguientes elementos:
   
      - **Configuración**
         - **Grupo de recursos**: usar el grupo de recursos donde desea crear los recursos RDS.
         > [!NOTE] 
         > Actualmente, este debe ser el mismo grupo de recursos donde se encuentra la red virtual del Administrador de recursos de Azure.

         - **Prefijo de etiqueta de DNS**: escriba la dirección URL que desea que los usuarios a usar para tener acceso Web de escritorio remoto.
         - **Nombre de dominio de AD**: escriba el nombre completo de la instancia de Azure AD, por ejemplo, "contoso.onmicrosoft.com" o "contoso.com".
         - **Nombre de Vnet de AD** y el **Nombre de la subred de Ad**: especificar los mismos valores que usó cuando creó la red virtual del Administrador de recursos de Azure. Se trata de la subred a la que se conectarán los recursos RDS.
         - **Nombre de usuario Admin** y la **Contraseña de administrador**: especifique las credenciales de un usuario permisos de administrador que sea miembro del grupo de **Administradores de controlador de dominio AAD** en Azure AD.
   
      - **Plantilla**
         - Quitar todas las propiedades de **dnsServers**: después de seleccionar **Editar plantilla** de la página de plantilla de inicio rápido de Azure, busque "dnsServers" y quitar la propiedad. 

            Por ejemplo, antes de quitar la propiedad **dnsServers** :
      
            ![Plantilla de inicio rápido de Azure con dnsSettings (propiedad)](media/rds-remove-dnssettings-before.png)

            Y aquí es el mismo archivo después de quitar la propiedad:

            ![Plantilla de inicio rápido de Azure con ha quitado la propiedad dnsSettings](media/rds-remove-dnssettings-after.png)
   
   - [Implementar RDS manualmente](rds-deploy-infrastructure.md). 

