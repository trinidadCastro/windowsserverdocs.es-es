---
ms.assetid: 9a06cd41-426f-4cb9-89cf-f5be730e0b79
title: Novedades de Active Directory Domain Services
ms.prod: windows-server
ms.technology: active-directory-domain-services
ms.topic: article
author: Femila
ms.author: billmath
ms.date: 05/31/2017
ms.openlocfilehash: 179064fdc958537190ddf5ce62475ac5d56fee07
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86965677"
---
# <a name="whats-new-in-active-directory-domain-services"></a>Novedades de Active Directory Domain Services 

>Se aplica a: Windows Server 2016

Las siguientes características nuevas de Active Directory Domain Services (AD DS) mejoran la capacidad para que las organizaciones protejan Active Directory entornos y les ayuden a migrar a implementaciones solo en la nube e implementaciones híbridas, donde algunas aplicaciones y servicios se hospedan en la nube y otros se hospedan de forma local. Estas mejoras incluyen:  
  
-   [Privileged Access Management](/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services)  
  
- [Extensión de las funcionalidades de nube a dispositivos de Windows 10 a través de Azure Active Directory join](/azure/active-directory/devices/overview)   
  
- [Conexión de dispositivos Unidos a un dominio a Azure AD para experiencias de Windows 10](/azure/active-directory/devices/hybrid-azuread-join-plan)   
  
- [Habilitación de Microsoft Passport for Work en la organización](/windows/security/identity-protection/hello-for-business/hello-identity-verification)    
  
-  [Desuso del servicio de replicación de archivos (FRS) y los niveles funcionales de Windows Server 2003](ad-ds/active-directory-functional-levels.md)  
  
  
## <a name="privileged-access-management"></a><a name="BKMK_PAM"></a>Privileged Access Management  
Privileged Access Management (PAM) ayuda a mitigar los problemas de seguridad de los entornos de Active Directory causados por técnicas de robo de credenciales, como Pass-The-hash, Spear phishing y otros tipos similares de ataques. Proporciona una nueva solución de acceso administrativo que se configura mediante Microsoft Identity Manager (MIM). PAM presenta:  
  
-   Un nuevo bosque de Active Directory bastión, aprovisionado por MIM. El bosque bastión tiene una confianza de PAM especial con un bosque existente. Proporciona un nuevo entorno de Active Directory que se sabe que está libre de cualquier actividad malintencionada y el aislamiento de un bosque existente para el uso de cuentas con privilegios.  
  
-   Nuevos procesos en MIM para solicitar privilegios administrativos, junto con nuevos flujos de trabajo en función de la aprobación de las solicitudes.  
  
-   Nuevas entidades de seguridad de instantáneas (grupos) que MIM aprovisiona en el bosque bastión en respuesta a solicitudes de privilegios administrativos. Las entidades de seguridad de sombra tienen un atributo que hace referencia al SID de un grupo administrativo en un bosque existente. Esto permite que el grupo de instantáneas tenga acceso a los recursos de un bosque existente sin cambiar las listas de control de acceso (ACL).  
  
-   Característica de vínculos que van a expirar, que permite la pertenencia enlazada a tiempo en un grupo de instantáneas. Se puede Agregar un usuario al grupo durante el tiempo necesario para realizar una tarea administrativa. La pertenencia enlazada a tiempo se expresa mediante un valor de período de vida (TTL) que se propaga a la duración de un vale de Kerberos.  
  
    > [!NOTE]  
    > Los vínculos que van a expirar están disponibles en todos los atributos vinculados. Pero la relación de atributo miembro y miembro de vinculación entre un grupo y un usuario es el único ejemplo en el que una solución completa como PAM está preconfigurada para usar la característica de vínculos que expiran.  
  
-   Las mejoras de KDC están integradas en Active Directory controladores de dominio para restringir la duración de los vales Kerberos al valor de período de vida (TTL) más bajo posible en los casos en los que un usuario tiene varias pertenencias enlazadas a tiempo en grupos administrativos. Por ejemplo, si se agrega a un grupo enlazado a tiempo a, al iniciar sesión, la vigencia del vale de concesión de vales (TGT) de Kerberos es igual a la hora restante en el grupo A. Si también es miembro de otro grupo de límites de tiempo B, que tiene un TTL menor que el grupo A, la duración del TGT es igual al tiempo restante en el grupo B.  
  
-   Nuevas capacidades de supervisión para ayudarle a identificar fácilmente quién solicitó acceso, qué acceso se ha concedido y qué actividades se han realizado.  
  
**Requisitos**  
  
-   Administrador de identidades de Microsoft  
  
-   Active Directory nivel funcional del bosque de Windows Server 2012 R2 o posterior.  
  
## <a name="azure-ad-join"></a><a name="BKMK_AzureADJoin"></a>Azure AD join  
Azure Active Directory join mejora las experiencias de identidad de los clientes empresariales, empresariales y de EDU, con capacidades mejoradas para dispositivos corporativos y personales.  
  
Ventajas:  
  
-   **Disponibilidad de la configuración moderna** en dispositivos Windows de la empresa. Los servicios de oxígeno ya no requieren un cuenta de Microsoft personal: ahora se ejecutan en las cuentas de trabajo existentes de los usuarios para garantizar el cumplimiento. Los servicios de oxígeno funcionarán en equipos Unidos a un dominio de Windows local y equipos y dispositivos que están "Unidos" a su inquilino de Azure AD ("dominio en la nube"). Esta configuración incluye:  
  
    -   Itinerancia o personalización, configuración de accesibilidad y credenciales  
  
    -   Copias de seguridad y restauración  
  
    -   Acceso a Microsoft Store con cuenta profesional  
  
    -   Iconos dinámicos y notificaciones  
  
-   **Acceder a recursos** de la organización en dispositivos móviles (teléfonos, phablets) que no se pueden unir a un dominio de Windows, tanto si son de propiedad corporativa como de BYOD  
  
-   **Inicio de sesión único** en Office 365 y otras aplicaciones, sitios web y recursos de la organización.  
  
-   **En los dispositivos BYOD**, agregue una cuenta profesional (desde un dominio local o Azure ad) a un dispositivo de propiedad personal y disfrute del inicio de sesión único en los recursos de trabajo, a través de aplicaciones y en la web, de forma que ayude a garantizar el cumplimiento de nuevas capacidades, como el control de cuentas condicional y la atestación de estado del dispositivo.  
  
-   La **integración de MDM** le permite inscribir automáticamente dispositivos en su MDM (Intune o de terceros).  
  
-   **Configurar el modo "quiosco" y los dispositivos compartidos** para varios usuarios de la organización  
  
-   La **experiencia del desarrollador** le permite crear aplicaciones que satisfagan los contextos empresariales y personales con una pila de programas compartida.  
  
-   La opción de **creación de imágenes** permite elegir entre imágenes y permitir que los usuarios configuren dispositivos corporativos directamente durante la primera ejecución.  
  
Para obtener más información, consulte [Windows 10 para empresa: formas de usar dispositivos para trabajar](/azure/active-directory/devices/overview).  
  
## <a name="microsoft-passport"></a><a name="BKMK_IDLocker"></a>Microsoft Passport  
Microsoft Passport es un nuevo enfoque de autenticación basada en claves para organizaciones y consumidores que va más allá de las contraseñas. Esta forma de autenticación se basa en credenciales de infracción, robo y resistente a phish.  
  
El usuario inicia sesión en el dispositivo con una información de inicio de sesión biométrica o PIN que está vinculada a un certificado o un par de claves asimétricas. Los proveedores de identidades (IDP) validan al usuario mediante la asignación de la clave pública del usuario a IDLocker y proporcionan información de inicio de sesión a través de una contraseña de una hora (OTP), Phonefactor o un mecanismo de notificación diferente.  
  
Para obtener más información, vea [autenticar identidades sin contraseñas a través de Microsoft Passport](/windows/security/identity-protection/hello-for-business/hello-identity-verification)  
  
## <a name="deprecation-of-file-replication-service-frs-and-windows-server-2003-functional-levels"></a><a name="BKMK_FRSDeprecation"></a>Desuso del servicio de replicación de archivos (FRS) y los niveles funcionales de Windows Server 2003  
Aunque el servicio de replicación de archivos (FRS) y los niveles funcionales de Windows Server 2003 estaban en desuso en versiones anteriores de Windows Server, se repite que el sistema operativo Windows Server 2003 ya no se admite. Como resultado, todos los controladores de dominio que ejecuten Windows Server 2003 deben quitarse del dominio. El nivel funcional de dominio y bosque debe elevarse al menos a Windows Server 2008 para evitar que un controlador de dominio que ejecuta una versión anterior de Windows Server se agregue al entorno.  
  
En los niveles funcionales de dominio de Windows Server 2008 y superior, se usa la Replicación del sistema de archivos distribuido (DFS) para replicar el contenido de la carpeta SYSVOL entre controladores de dominio. Si creas un nuevo dominio en el nivel funcional del dominio de Windows Server 2008 o superior, se usa automáticamente Replicación DFS para replicar SYSVOL. Si has creado el dominio en un nivel funcional inferior, tendrás que migrar del uso de FRS a la replicación DFS para SYSVOL. En el caso de los pasos de migración, puedes seguir los [procedimientos de TechNet](../storage/dfs-replication/migrate-sysvol-to-dfsr.md) o puedes consultar el [conjunto de pasos simplificado en el blog de contenedor de archivos del equipo de almacenamiento](https://techcommunity.microsoft.com/t5/storage-at-microsoft/bg-p/FileCAB).  
  
Los niveles funcionales del dominio y del bosque de Windows Server 2003 siguen siendo compatibles, pero las organizaciones deben elevar el nivel funcional a Windows Server 2008 (o superior si es posible) para garantizar la compatibilidad con la replicación de SYSVOL y la compatibilidad en el futuro. Además, hay muchas otras ventajas y características disponibles en los niveles funcionales más altos. Consulta los recursos siguientes para obtener más información:  
  
-   [Descripción de los niveles funcionales de Active Directory Domain Services (AD DS)](ad-ds/active-directory-functional-levels.md)  
  
-   [Elevar el nivel funcional del dominio](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753104(v=ws.11))  
  
-   [Elevar el nivel funcional del bosque](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc730985(v=ws.11))  
  
