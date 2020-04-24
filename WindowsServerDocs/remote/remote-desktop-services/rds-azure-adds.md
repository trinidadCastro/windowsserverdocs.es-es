---
title: Azure AD Domain Services y Servicios de Escritorio remoto
description: Aprende a integrar Azure AD Domain Services en tu implementación de RDS.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 10/02/2017
ms.topic: article
author: christianmontoya
ms.localizationpriority: medium
ms.openlocfilehash: bc70300df8bc8aef78371f4b84fe697bf8e66749
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "80852988"
---
# <a name="integrate-azure-ad-domain-services-with-your-rds-deployment"></a>Integración de Azure AD Domain Services con la implementación de RDS

Puedes usar [Azure AD Domain Services](/azure/active-directory-domain-services/active-directory-ds-overview) (Azure AD DS) en la implementación de Servicios de Escritorio remoto en lugar de Windows Server Active Directory. Azure AD DS te permite usar las identidades de Azure AD existentes con las cargas de trabajo de Windows clásicas.

Con Azure AD DS, puedes: 
- Crear un entorno de Azure con un dominio local para las organizaciones creadas en la nube. 
- Crear un entorno aislado de Azure con las mismas identidades usadas para el entorno local y el entorno en línea, sin necesidad de crear una VPN de sitio a sitio o ExpressRoute. 

Cuando termines de integrar Azure AD DS en la implementación de Escritorio remoto, la arquitectura tendrá un aspecto similar al siguiente:

![Diagrama de arquitectura que muestra RDS con Azure AD DS](media/aadds-rds.png)

Para ver una comparación de esta arquitectura con otros escenarios de implementación de RDS, consulta [Arquitecturas de Servicios de Escritorio remoto](desktop-hosting-logical-architecture.md).

Para conocer mejor Azure AD DS, consulta [Introducción a Azure AD DS](/azure/active-directory-domain-services/active-directory-ds-overview) y [Cómo decidir si Azure AD DS es la solución más adecuada para su caso](/azure/active-directory-domain-services/active-directory-ds-comparison).

Usa la siguiente información para implementar Azure AD DS con RDS.

## <a name="prerequisites"></a>Requisitos previos

Antes de que proporciones las identidades de Azure AD que se van a usar en una implementación de RDS, [configura Azure AD para guardar las contraseñas con hash de las identidades de los usuarios](/azure/active-directory-domain-services/active-directory-ds-getting-started-password-sync). Las organizaciones que se han creado en la nube no necesitan realizar cambios adicionales en sus directorios. Sin embargo, las organizaciones locales deben permitir que los hashes de contraseña se sincronicen y almacenen en Azure AD, lo cual puede que no esté permitido para algunas organizaciones. Los usuarios tendrán que restablecer sus contraseñas después de realizar este cambio de configuración.

## <a name="deploy-azure-ad-ds-and-rds"></a>Implementación de Azure AD DS y RDS 
Usa los pasos siguientes para implementar Azure AD DS y RDS.

1. Habilita [Azure AD DS](/azure/active-directory-domain-services/active-directory-ds-getting-started). Ten en cuenta que en el artículo vinculado se realiza lo siguiente:
   - Se explica la creación de los grupos de Azure AD adecuados para la administración de dominios.
   - Se destaca cuándo deberías obligar a los usuarios a cambiar su contraseña para que sus cuentas puedan funcionar con Azure AD DS.
   
2. Configura RDS. Puedes usar una plantilla de Azure o implementar RDS manualmente.
   - Usa la [plantilla existente de AD](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/). Asegúrate de personalizar los siguientes elementos:
   
     - **Configuración**
       - **Grupo de recursos**: usa el grupo de recursos en el que deseas crear los recursos de RDS.
         > [!NOTE] 
         > En este momento, este debe ser el mismo grupo de recursos en el que existe la red virtual de Azure Resource Manager.

       - **Prefijo de etiqueta DNS**: Escribe la dirección URL que quieres que los usuarios usen para tener acceso web a Escritorio remoto.
       - **Nombre de dominio de AD**: Escribe el nombre completo de la instancia de Azure AD, por ejemplo, "contoso.onmicrosoft.com" o "contoso.com".
       - **Nombre de red virtual de AD** y **Nombre de subred de AD**: Especifica los mismos valores que usaste cuando creaste la red virtual de Azure Resource Manager. Esta es la subred a la que se conectarán los recursos de RDS.
       - **Nombre de usuario del administrador** y **Contraseña del administrador**: Escribe las credenciales de un usuario administrador que sea miembro del grupo **Administradores de AAD DC** en Azure AD.
   
     - **Plantilla**
        - Elimina todas las propiedades de **dnsServers**: después de seleccionar **Editar plantilla** en la página de plantilla de inicio rápido de Azure, busca "dnsServers" y elimina la propiedad. 

           Por ejemplo, antes de eliminar la propiedad **dnsServers**:
      
           ![Plantilla de inicio rápido de Azure con la propiedad dnsSettings](media/rds-remove-dnssettings-before.png)

           Y este es el mismo archivo después de eliminar la propiedad:

           ![Plantilla de inicio rápido de Azure con la propiedad dnsSettings eliminada](media/rds-remove-dnssettings-after.png)
   
   - [Implementación manual de RDS](rds-deploy-infrastructure.md). 

