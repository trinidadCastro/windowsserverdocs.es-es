---
ms.assetid: 9a06cd41-426f-4cb9-89cf-f5be730e0b79
title: "¿Qué & #39; s nuevo en los servicios de dominio de Active Directory"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: 
ms.suite: na
ms.technology: active-directory-domain-services
ms.tgt_pltfrm: na
ms.topic: article
author: Femila
ms.author: billmath
ms.date: 05/31/2017
ms.openlocfilehash: 56716e2e0d633a19e9c6e02bc829874bbe863833
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="what39s-new-in-active-directory-domain-services"></a>¿Qué & #39; s nuevo en los servicios de dominio de Active Directory 

>Se aplica a: Windows Server 2016

Las siguientes características nuevas en los servicios de dominio de Active Directory (AD DS) mejoran la capacidad para las organizaciones a proteger los entornos de Active Directory y ayudarles a migrar a las implementaciones de solo en la nube y las implementaciones híbridas, donde se hospedan algunas aplicaciones y servicios en la nube y otras personas se hospedan en local. Las mejoras incluyen:  
  
-   [Administración de acceso con privilegios](https://technet.microsoft.com/library/mt150258.aspx   
)  
  
- [Amplía las capacidades de nube a los dispositivos de Windows 10 a través unir de Azure Active Directory](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-overview/)   
  
- [Experiencias de conectar dispositivos Unidos a un dominio a Azure AD para Windows 10](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-devices-group-policy/)   
  
- [Habilitar Microsoft Passport for Work en la organización](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-passport-deployment/)    
  
-  [Desuso de niveles funcionales de servicio de replicación de archivos (FRS) y Windows Server 2003](ad-ds/active-directory-functional-levels.md)  
  
  
## <a name="BKMK_PAM"></a>Administración de acceso con privilegios  
Administración de acceso a ellos (PAM) ayuda a mitigar seguridad cuestiones para entornos de Active Directory causados por técnicas de robo de credenciales tales pass-the-hash, suplantación de identidad lanza y tipos de ataques similares. Proporciona una nueva solución de acceso administrativo que se configura mediante el Administrador de identidad de Microsoft (MIM). Presenta PAM:  
  
-   Un nuevo bosque de Active Directory, que se aprovisiona MIM baluarte. El bosque de baluarte tiene una relación de confianza PAM especial con un bosque existente. Proporciona un nuevo entorno de Active Directory que se sabe que es gratuita de actividades malintencionadas y aislamiento de un bosque existente para el uso de cuentas con privilegios.  
  
-   Nuevos procesos en MIM para solicitar privilegios administrativos, junto con los nuevos flujos de trabajo en función de la aprobación de solicitudes.  
  
-   Nueva sombra entidades de seguridad (grupos) que se ha aprovisionado en el bosque baluarte por MIM en respuesta a las solicitudes de privilegios administrativos. Entidades de seguridad de sombra tienen un atributo que hace referencia al SID de un grupo administrativo de un bosque existente. Esto permite que al grupo de instantáneas para acceder a los recursos en un bosque existente sin modificar las listas de control de acceso (ACL).  
  
-   Característica vínculos caduca, lo que permite el límite de tiempo la pertenencia a un grupo de instantáneas. Un usuario puede agregarse al grupo de suficientes tiempo necesario para realizar una tarea administrativa. La pertenencia a un límite de tiempo se expresa con un valor de tiempo de vida (TTL) que se propaga a la duración de un vale de Kerberos.  
  
    > [!NOTE]  
    > Vínculos caducados están disponibles en todos los atributos vinculados. Pero el atributo de miembro o miembro vinculado relación entre un grupo y un usuario es el ejemplo solo en una solución completa como PAM está preconfigurada para utilizar la característica de vínculos que expiran.  
  
-   Mejoras de KDC están integradas los controladores de dominio de Active Directory para restringir la duración de vale Kerberos para el valor mínimo posible tiempo de vida (TTL) en los casos donde un usuario tiene varias suscripciones controlado por tiempo en grupos administrativos. Por ejemplo, si se agregan a un grupo de límite de tiempo A, al iniciar sesión, la duración de vales (TGT) de concesión de vales Kerberos es igual a la vez tienes restante en el grupo A. Si también eres miembro de otro grupo controlado por tiempo B, que tiene un valor de TTL inferior a un grupo, la duración TGT es igual que el tiempo restante en el grupo B.  
  
-   Nuevas capacidades de supervisión para que resulte fácil identifican que solicita acceso, se concedió el acceso y las actividades que se realizaron.  
  
**Requisitos**  
  
-   Microsoft Identity Manager  
  
-   Nivel funcional de Windows Server 2012 R2 o posterior de bosque de Active Directory.  
  
## <a name="BKMK_AzureADJoin"></a>Unión a Azure AD  
Azure Active Directory unirse a la mejora de las experiencias de identidad de empresa, las empresas y clientes EDU - con funciones mejoradas para los dispositivos corporativos y personales.  
  
Ventajas:  
  
-   **Disponibilidad de configuración modernos** en corp pertenecientes a los dispositivos de Windows. Servicios de oxígeno ya no requieren una cuenta de Microsoft personal: ahora se ejecutan en cuentas de trabajo existente de usuarios para garantizar la conformidad. Servicios de oxígeno funcionarán en equipos que están unidos a un dominio local de Windows, y los equipos y dispositivos que son "Unidos" a tu inquilino de Azure AD ("dominio de nube"). Estas opciones incluyen:  
  
    -   Itinerancia o personalización, configuración de accesibilidad y credenciales  
  
    -   Copia de seguridad y restauración  
  
    -   Acceso a Microsoft Store con una cuenta profesional  
  
    -   Iconos dinámicos y notificaciones  
  
-   **Acceder a recursos organizativos** en dispositivos móviles (teléfonos, tabléfonos) que no se pueden unir a un dominio de Windows, independientemente de si están pertenecientes a corp o BYOD  
  
-   **Inicio de sesión único en** a Office 365 y otras aplicaciones de la organización, sitios Web y recursos.  
  
-   **En dispositivos BYOD**, agregar una cuenta de trabajo (de un dominio local o Azure AD) a un dispositivo de propiedad personal y disfruta de SSO para recursos de trabajo, a través de aplicaciones y en la web, de manera que ayuda a garantiza la conformidad con las nuevas funcionalidades, como la atestación condicional Control de cuentas y el estado del dispositivo.  
  
-   **Integración de MDM** permite inscripción automática de dispositivos para el sistema MDM (Intune o terceros)  
  
-   **Configurar el modo "quiosco" y que comparten dispositivos** para varios usuarios en la organización  
  
-   **Experiencia del desarrollador** permite crear aplicaciones que responden a la empresa y personales contextos con una pila de programación compartida.  
  
-   **Creación de imágenes** opción te permite elegir entre imágenes y permitiendo que los usuarios configurar sus dispositivos pertenecientes a corp directamente durante la experiencia de primera ejecución.  
  
Para obtener más información, consulta [para la empresa de Windows 10: formas de usar dispositivos para el trabajo](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-windows10-devices-overview/?rnd=1).  
  
## <a name="BKMK_IDLocker"></a>Microsoft Passport  
Microsoft Passport es una nueva autenticación basada en claves abordar las organizaciones y los consumidores, que va más allá de las contraseñas. Este tipo de autenticación se basa en una infracción, el robo y resistente a phishing credenciales.  
  
El usuario inicia sesión en el dispositivo con un registro de datos biométrico o PIN de la información que está vinculado a un certificado o un par de claves asimétrico. Los proveedores de identidad (IDP) validar al usuario mediante la asignación de la clave pública del usuario para IDLocker y proporciona el registro de información a través de la contraseña de una vez (OTP), Phonefactor o un mecanismo de notificación diferentes.  
  
Para obtener más información, consulta [autenticar identidades sin contraseñas a través de Microsoft Passport](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-passport/)  
  
## <a name="BKMK_FRSDeprecation"></a>Desuso de niveles funcionales de servicio de replicación de archivos (FRS) y Windows Server 2003  
Aunque el servicio de replicación de archivos (FRS) y los niveles funcionales de Windows Server 2003 quedaron en desuso en versiones anteriores de Windows Server, lleva repetir que ya no se admite el sistema operativo Windows Server 2003. Como resultado, debe quitarse cualquier controlador de dominio que ejecuta Windows Server 2003 del dominio. El nivel funcional del bosque y dominio debe emitirse para al menos Windows Server 2008 para evitar que un controlador de dominio que se ejecuta una versión anterior de Windows Server desde que se agregan en el entorno.  
  
A Windows Server 2008 y más altos niveles funcionales de dominio, la replicación de servicio de archivos distribuido (DFS) se usa para replicar el contenido de la carpeta SYSVOL entre controladores de dominio. Si creas un nuevo dominio en el nivel funcional de dominio de Windows Server 2008 o una versión posterior, la replicación DFS automáticamente se usa para replicar SYSVOL. Si has creado el dominio de nivel inferior funcional, tendrás que migrar usen FRS para la replicación DFS para SYSVOL. Para ver los pasos de migración, puedes seguir la [procedimientos en TechNet](https://technet.microsoft.com/library/dd640019(v=WS.10).aspx) o puede hacer referencia a la [mejorada conjunto de pasos en el blog de contenedor de archivos del equipo de almacenamiento](http://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx).  
  
Los niveles funcionales de bosque y dominio de Windows Server 2003 se siguen admitiendo, pero las organizaciones deben generar el nivel funcional a Windows Server 2008 (o superior si es posible) para garantizar la compatibilidad de replicación de SYSVOL y soporte técnico en el futuro. Además, hay muchas otras ventajas y las características disponibles en los niveles funcionales superior. Consulta los siguientes recursos para obtener más información:  
  
-   [Descripción de dominio de Active Directory (AD DS) de niveles funcionales de servicios](ad-ds/active-directory-functional-levels.md)  
  
-   [Aumentar el nivel funcional de dominio](https://technet.microsoft.com/library/cc753104.aspx)  
  
-   [Aumentar el nivel funcional del bosque](https://technet.microsoft.com/library/cc730985.aspx)  
  
