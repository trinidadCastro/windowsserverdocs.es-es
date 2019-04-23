---
title: Servicios de dominio de Azure AD y servicios de escritorio remoto
description: Obtenga información sobre cómo integrar Azure AD Domain Services en su implementación de RDS.
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860516"
---
# <a name="integrate-azure-ad-domain-services-with-your-rds-deployment"></a>Integrar Azure AD Domain Services con la implementación de RDS

Puede usar [Azure AD Domain Services](/azure/active-directory-domain-services/active-directory-ds-overview) (Azure AD DS) en la implementación de servicios de escritorio remoto en lugar de Windows Server Active Directory. Azure AD DS le permite usar las identidades de Azure AD existentes con las cargas de trabajo de Windows clásicos.

Con Azure AD DS, puede: 
- Crear un entorno de Azure con un dominio local para las organizaciones nacidos-en la nube. 
- Crear un entorno con aislamiento de Azure con las mismas identidades usadas para el local y el entorno en línea, sin necesidad de crear una VPN de sitio a sitio o ExpressRoute. 

Cuando termine de integración de Azure AD DS en la implementación de escritorio remoto, la arquitectura tendrá un aspecto similar al siguiente:

![Un diagrama de arquitectura que muestra RDS con Azure AD DS](media/aadds-rds.png)

Para ver cómo esta arquitectura se compara con otros escenarios de implementación de RDS, consulte [arquitecturas de servicios de escritorio remoto](desktop-hosting-logical-architecture.md).

Para obtener una mejor comprensión de Azure AD DS, consulte el [información general de Azure AD DS](/azure/active-directory-domain-services/active-directory-ds-overview) y [cómo decidir si es adecuado para su caso de uso de Azure AD DS](/azure/active-directory-domain-services/active-directory-ds-comparison).

Use la siguiente información para implementar Azure AD DS con RDS.

## <a name="prerequisites"></a>Requisitos previos

Antes de poner las identidades de Azure AD para usar en una implementación de RDS, [configurar Azure AD para guardar las contraseñas con hash de las identidades de usuarios](/azure/active-directory-domain-services/active-directory-ds-getting-started-password-sync). Nacido en de la nube a las organizaciones no es necesario realizar cambios adicionales en su directorio. Sin embargo, las organizaciones locales deben permitir los hash de contraseña que se sincronizan y se almacenan en Azure AD, que puede no estar autorizado para algunas organizaciones. Los usuarios tendrán que restablecer sus contraseñas después de realizar este cambio de configuración.

## <a name="deploy-azure-ad-ds-and-rds"></a>Implementar RDS y Azure AD DS 
Siga estos pasos para implementar Azure AD DS y RDS.

1. Habilitar [Azure AD DS](/azure/active-directory-domain-services/active-directory-ds-getting-started). Tenga en cuenta que el artículo vinculado hace lo siguiente:
   - Explica cómo crear adecuado grupos de Azure AD para la administración de dominios.
   - Resaltar cuando es posible que deba forzar a los usuarios cambiar su contraseña para que sus cuentas pueden trabajar con Azure AD DS.
   
2. Configuración de RDS. Puede usar una plantilla de Azure o implementar RDS manualmente.
   - Use la [plantilla existente de AD](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/). Asegúrese de personalizar los siguientes elementos:
   
      - **Configuración**
         - **Grupo de recursos**: Use el grupo de recursos donde desea crear los recursos RDS.
         > [!NOTE] 
         > En estos momentos, este debe ser el mismo grupo de recursos donde existe la red virtual de Azure resource manager.

         - **Prefijo de etiqueta DNS**: Escriba la dirección URL que desea que los usuarios usen para tener acceso a Web a Escritorio remoto.
         - **Nombre de dominio AD**: Escriba el nombre completo de la instancia de Azure AD, por ejemplo, "contoso.onmicrosoft.com" o "contoso.com".
         - **Nombre de red virtual de AD** y **nombre de subred de Ad**: Especifique los mismos valores que usó cuando creó la red virtual de Azure resource manager. Se trata de la subred a la que se conectarán los recursos RDS.
         - **Nombre de usuario administrador** y **contraseña de administrador**: Escriba las credenciales de un usuario de administrador que es un miembro de la **AAD DC Administrators** grupo en Azure AD.
   
      - **plantilla**
         - Quitar todas las propiedades de **dnsServers**: después de seleccionar **Editar plantilla** desde la página de plantilla de inicio rápido de Azure, busque "dnsServers" y quite la propiedad. 

            Por ejemplo, antes de quitar el **dnsServers** propiedad:
      
            ![Plantilla de inicio rápido de Azure con la propiedad dnsSettings](media/rds-remove-dnssettings-before.png)

            Y aquí es el mismo archivo después de quitar la propiedad:

            ![Plantilla de inicio rápido de Azure con la propiedad dnsSettings quitada](media/rds-remove-dnssettings-after.png)
   
   - [Implementación manual de RDS](rds-deploy-infrastructure.md). 

