---
ms.assetid: 9a06cd41-426f-4cb9-89cf-f5be730e0b79
title: ¿Qué&#39;Novedades en servicios de dominio de Active Directory
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: ''
ms.suite: na
ms.technology: active-directory-domain-services
ms.tgt_pltfrm: na
ms.topic: article
author: Femila
ms.author: billmath
ms.date: 05/31/2017
ms.openlocfilehash: ffa8bcb43b17ae8779c70d499bff27a8f77cce75
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841336"
---
# <a name="what39s-new-in-active-directory-domain-services"></a>¿Qué&#39;Novedades en servicios de dominio de Active Directory 

>Se aplica a: Windows Server 2016

Las siguientes características nuevas en Active Directory Domain Services (AD DS) aumentan la capacidad de las organizaciones a proteger los entornos de Active Directory y les ayuda a migrar a las implementaciones exclusivas en la nube y las implementaciones híbridas, donde son algunas aplicaciones y servicios hospedado en la nube y otros se hospedan en el entorno local. Estas mejoras incluyen:  
  
-   [Administración de acceso con privilegios](https://technet.microsoft.com/library/mt150258.aspx   
)  
  
- [Ampliación de las capacidades de nube a dispositivos Windows 10 a través de Azure Active Directory Join](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-overview/)   
  
- [Experiencias de conectar dispositivos Unidos a Azure AD para Windows 10](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/)   
  
- [Habilitar Microsoft Passport for Work en su organización](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)    
  
-  [Degradación de niveles funcionales del servicio de replicación de archivos (FRS) y Windows Server 2003](ad-ds/active-directory-functional-levels.md)  
  
  
## <a name="BKMK_PAM"></a>Administración de acceso con privilegios  
Administración del acceso con privilegios (PAM) ayuda a mitigar seguridad preocupaciones para los entornos de Active Directory causados por técnicas de robo de credenciales tales pass-the-hash, suplantación de identidad y otros tipos similares de ataques. Proporciona una nueva solución de acceso administrativo que se configura mediante Microsoft Identity Manager (MIM). PAM incluye:  
  
-   Un nuevo bosque de Active Directory, que se aprovisiona con MIM bastión. El bosque bastión tiene una confianza de PAM especial con un bosque existente. Proporciona un nuevo entorno de Active Directory que se sabe que es libre de cualquier actividad malintencionada y aislamiento de un bosque existente para el uso de cuentas con privilegios.  
  
-   Nuevos procesos en MIM para solicitar privilegios administrativos, junto con los nuevos flujos de trabajo en función de la aprobación de solicitudes.  
  
-   Nuevas instantáneas entidades de seguridad (grupos) que se aprovisionan en el bosque bastión MIM en respuesta a solicitudes de privilegios administrativos. Las entidades de seguridad de instantáneas tienen un atributo que hace referencia al SID de un grupo administrativo en un bosque existente. Esto permite que el grupo de instantáneas para acceder a recursos en un bosque existente sin cambiar las listas de control de acceso (ACL).  
  
-   Una característica de vínculos que van a expirar, lo que permite la pertenencia de tiempo limitado en un grupo de instantáneas. Puede agregar un usuario al grupo para suficiente tiempo necesario para realizar una tarea administrativa. La pertenencia a un límite de tiempo se expresa mediante un valor de período de vida (TTL) que se propaga a una duración de vale de Kerberos.  
  
    > [!NOTE]  
    > Vínculos que expiran están disponibles en todos los atributos vinculados. Pero el atributo vinculado memberOf de miembro/relación entre un grupo y un usuario es el ejemplo solo donde una solución completa como PAM está preconfigurada para usar la característica de vínculos que expiran.  
  
-   Mejoras de KDC están integradas en los controladores de dominio de Active Directory para restringir la vigencia del vale de Kerberos para el valor más bajo posible tiempo de vida (TTL) en los casos donde un usuario tiene varios miembros enlazados en tiempo de los grupos administrativos. Por ejemplo, si se agregan a un grupo de límite de tiempo A, entonces cuando inician sesión, la duración de vales (TGT) de concesión de vales de Kerberos es igual a la hora de tener restantes en el grupo A. Si también es un miembro de otro grupo de límite de tiempo B, que tiene un valor de TTL inferior a un grupo, la vigencia del TGT es igual que el tiempo restante en el grupo B.  
  
-   Nuevas capacidades de supervisión que le ayudarán a fácilmente identifican quién ha solicitado acceso, se ha concedido el acceso y qué actividades se han realizado.  
  
**Requisitos**  
  
-   Microsoft Identity Manager  
  
-   Nivel funcional de Windows Server 2012 R2 o superior del bosque de Active Directory.  
  
## <a name="BKMK_AzureADJoin"></a>Unión a Azure AD  
Azure Active Directory Join mejora las experiencias de identidad para enterprise, business y los clientes EDU - con capacidades mejoradas para dispositivos personales y corporativos.  
  
Ventajas:  
  
-   **Disponibilidad de la configuración modernas** en dispositivos de propiedad corporativa de Windows. Servicios de oxígeno ya no requieren una cuenta Microsoft personal: se ejecutan ahora desactivar cuentas de trabajo existentes de los usuarios para garantizar el cumplimiento. Servicios de oxígeno funcionará en equipos que están unidos a un dominio de Windows de forma local, y los equipos y dispositivos que están "unir" con el inquilino de Azure AD ("dominio de la nube"). Esta configuración incluye:  
  
    -   Roaming o personalización, configuración de accesibilidad y las credenciales  
  
    -   Copia de seguridad y restauración  
  
    -   Acceso a Microsoft Store con la cuenta profesional  
  
    -   Notificaciones e iconos dinámicos  
  
-   **Obtener acceso a recursos de la organización** en dispositivos móviles (teléfonos, phablets) que no se pueden unir a un dominio de Windows, independientemente de si están corp corporativos o BYOD  
  
-   **Inicio de sesión único en** a Office 365 y otras aplicaciones de la organización, sitios Web y recursos.  
  
-   **En los dispositivos BYOD**, agregar una cuenta profesional (desde un dominio local o Azure AD) a un dispositivo personal y disfrutar de SSO para recursos de trabajo, a través de aplicaciones y en la web, de manera que le ayuda a garantizar el cumplimiento con nuevas capacidades como la cuenta condicional Atestación de estado del dispositivo y de control.  
  
-   **Integración de MDM** permite la inscripción automática de dispositivos a la MDM (Intune o terceros)  
  
-   **Configurar el modo de "pantalla completa" y dispositivos compartidos** para varios usuarios de su organización  
  
-   **Experiencia del desarrollador** le permite crear aplicaciones que satisfaga las necesidades empresariales y personales contextos con una pila de programación compartida.  
  
-   **Creación de imágenes** opción le permite elegir entre imágenes y permitiendo que los usuarios para configurar dispositivos de propiedad corporativa directamente durante la experiencia de primera ejecución.  
  
Para obtener más información, vea [Windows 10 para empresa: Formas de usar dispositivos para trabajar](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-windows10-devices-overview/?rnd=1).  
  
## <a name="BKMK_IDLocker"></a>Microsoft Passport  
Microsoft Passport es una autenticación basada en claves nuevo enfoque a organizaciones y consumidores, que va más allá de las contraseñas. Esta forma de autenticación se basa en incumplimiento, el robo y resistente a phishing credenciales.  
  
El usuario inicia sesión en el dispositivo con una PIN o biometría información de registro que esté vinculada a un certificado o un par de claves asimétrico. Los proveedores de identidades (IDP) validan al usuario mediante la asignación de la clave pública del usuario a IDLocker y proporciona registro de información a través de la contraseña de una vez (OTP), Phonefactor o un mecanismo de notificación diferente.  
  
Para obtener más información, vea [autenticación de identidades sin contraseñas a través de Microsoft Passport](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport/)  
  
## <a name="BKMK_FRSDeprecation"></a>Degradación de niveles funcionales del servicio de replicación de archivos (FRS) y Windows Server 2003  
Aunque el servicio de replicación de archivos (FRS) y los niveles funcionales de Windows Server 2003 han quedado en desuso en versiones anteriores de Windows Server, admite la repetición que ya no se admite el sistema operativo Windows Server 2003. Como resultado, cualquier controlador de dominio que ejecuta Windows Server 2003 debe quitarse del dominio. El nivel funcional del dominio y bosque debe elevarse al menos Windows Server 2008 para evitar que un controlador de dominio que ejecuta una versión anterior de Windows Server que se agreguen al entorno.  
  
En el Windows Server 2008 y los niveles funcionales de dominio más altos, replicación de servicio de archivos distribuido (DFS) se usa para replicar el contenido de la carpeta SYSVOL entre controladores de dominio. Si crea un nuevo dominio en el nivel funcional del dominio de Windows Server 2008 o posterior, se usa automáticamente la replicación DFS para replicar SYSVOL. Si ha creado el dominio en un nivel funcional inferior, deberá migrar del uso de FRS a replicación DFS para SYSVOL. Para conocer los pasos de migración, puede seguir el [procedimientos en TechNet](https://technet.microsoft.com/library/dd640019(v=WS.10).aspx) o puede hacer referencia a la [simplificado de conjunto de pasos en el blog de contenedor de archivos del equipo de almacenamiento](http://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx).  
  
Los niveles de funcional de bosque y dominio de Windows Server 2003 siguen siendo compatibles, pero las organizaciones deben elevar el nivel funcional a Windows Server 2008 (o superior si es posible) para garantizar la compatibilidad de la replicación de SYSVOL y soporte técnico en el futuro. Además, hay muchas otras ventajas y características disponibles en los niveles funcionales más altos superior. Consulta los recursos siguientes para obtener más información:  
  
-   [Descripción de dominio de Active Directory (AD DS) de niveles funcionales de servicios](ad-ds/active-directory-functional-levels.md)  
  
-   [Elevar el nivel funcional de dominio](https://technet.microsoft.com/library/cc753104.aspx)  
  
-   [Elevar el nivel funcional de bosque](https://technet.microsoft.com/library/cc730985.aspx)  
  
