---
ms.assetid: 87196b65-a356-409f-9af0-b5950797d668
title: Apéndice A - revisión de términos de clave de AD DS
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 5434db4124c471c613f159dec28e27dee70e7086
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852026"
---
# <a name="appendix-a-reviewing-key-ad-ds-terms"></a>Apéndice A: Revisión de términos de clave de AD DS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Los siguientes términos son pertinentes para el proceso de implementación para Windows Server 2008 Active Directory Domain Services (AD DS).  
  
## <a name="active-directory-domain"></a>Dominio de Active Directory  
Una unidad administrativa en una red de equipos que, para mayor comodidad de administración, agrupa varias funcionalidades, incluidos los siguientes:  
  
-   **Identidad de usuario de toda la red**. En los dominios, las identidades de usuario se pueden crear una vez y, a continuación, hace referencia en cualquier equipo que está unido al bosque donde se encuentra el dominio. Controladores de dominio que forman un dominio almacenan cuentas de usuario y las credenciales de usuario, como contraseñas o certificados, de forma segura.  
  
-   **Autenticación**. Controladores de dominio proporcionen servicios de autenticación para los usuarios. También, que suministra los datos de autorización adicionales, como las pertenencias de usuario. Los administradores pueden usar estos servicios para controlar el acceso a los recursos de la red.  
  
-   **Las relaciones de confianza**. Dominios extienden los servicios de autenticación a los usuarios de otros dominios en su propio bosque mediante confianzas bidireccionales automáticas. Dominios también amplían los servicios de autenticación a los usuarios en dominios de otros bosques mediante confianzas de bosque o confianzas externas creadas manualmente.  
  
-   **Administración de directivas**. Un dominio es un ámbito de las directivas administrativas, tales como reglas de reutilización de contraseñas y la complejidad de contraseña.  
  
-   **Replicación**. Un dominio define una partición del árbol del directorio que proporciona los datos que es adecuada para proporcionar los servicios necesarios y que se replica entre controladores de dominio. De este modo, todos los controladores de dominio son del mismo nivel en un dominio y se administran como una unidad.  
  
## <a name="active-directory-forest"></a>Bosque de Active Directory  
Una colección de uno o varios dominios de Active Directory que comparten una estructura lógica, el esquema de directorio y configuración de red, así como las relaciones de confianza transitiva bidireccional de automático. Cada bosque es una instancia única del directorio y define un límite de seguridad.  
  
## <a name="active-directory-functional-level"></a>Nivel funcional de Active Directory  
Una configuración de AD DS habilita características avanzadas de AD DS de todo el dominio o bosque.  
  
## <a name="migration"></a>Migración  
El proceso de mover un objeto de un dominio de origen a un dominio de destino, mientras se conservan o se modifican las características del objeto para que sea accesible en el nuevo dominio.  
  
## <a name="domain-restructure"></a>Reestructuración de dominios  
Un proceso de migración que implica la modificación de la estructura de dominios de un bosque. La reestructuración de dominios puede implicar la consolidación o la adición de dominios, y puede tener lugar entre bosques o dentro de un bosque.  
  
## <a name="domain-consolidation"></a>Consolidación de dominios  
Proceso de reestructuración que implica la eliminación de dominios de AD DS al combinar su contenido con el contenido de otros dominios.  
  
## <a name="domain-upgrade"></a>Actualización de dominios  
El proceso de actualización del servicio de directorio de un dominio a una versión posterior del servicio de directorio. Esto incluye la actualización del sistema operativo en todos los controladores de dominio y elevar el nivel funcional de AD DS en su caso.  
  
## <a name="in-place-domain-upgrade"></a>Actualización de dominio local  
El proceso de actualizar los sistemas operativos de todos los controladores de dominio en un dominio determinado, por ejemplo, la actualización de Windows Server 2003 a Windows Server 2008 y elevar el nivel funcional del dominio, si procede, dejando los objetos de dominio, por ejemplo, los usuarios y grupos, en su lugar.  
  
## <a name="forest-root-domain"></a>Dominio raíz del bosque  
El primer dominio que se crea en el bosque de Active Directory. Este dominio se designa automáticamente como dominio raíz del bosque. Proporciona la base para la infraestructura de bosque de Active Directory.  
  
## <a name="regional-domain"></a>Dominio regional  
Un dominio secundario que se crea en una región geográfica para optimizar el tráfico de replicación.  
  


