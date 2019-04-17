---
ms.assetid: 87196b65-a356-409f-9af0-b5950797d668
title: "Apéndice a: - revisar términos de clave de AD DS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ce7c0b639ded498b15200dfd594b3f34d6492dce
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="appendix-a-reviewing-key-ad-ds-terms"></a>Apéndice A: claves AD DS términos de revisión

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Los siguientes términos son relevantes para el proceso de implementación para Windows Server 2008 Active Directory Domain Services (AD DS).  
  
## <a name="active-directory-domain"></a>Dominio de Active Directory  
Una unidad administrativa en una red del equipo que, para mayor comodidad de administración, grupos de varias de las funcionalidades, incluido lo siguiente:  
  
-   **Toda la red identidad de usuario**. En los dominios, las identidades de usuario se pueden crearse una sola vez y, a continuación, se hace referencia en cualquier equipo que está unido al bosque en el que se encuentra el dominio. Controladores de dominio que conforman un dominio almacenan cuentas de usuario y las credenciales de usuario, como contraseñas o certificados, de forma segura.  
  
-   **Autenticación**. Controladores de dominio proporcionan servicios de autenticación para los usuarios. También proporcionan datos de autorización adicional, como las pertenencias a grupos de usuario. Los administradores pueden usar estos servicios para controlar el acceso a recursos de la red.  
  
-   **Relaciones de confianza**. Dominios extienden los servicios de autenticación a los usuarios de otros dominios en su propio bosque mediante confianzas bidireccionales automáticas. También dominios extienden los servicios de autenticación a los usuarios en los dominios de otros bosques mediante confianzas de bosque o confianzas externas creadas manualmente.  
  
-   **Administración de directivas**. Un dominio es un ámbito de las directivas administrativas, como reglas para la reutilización de contraseñas y la complejidad de contraseña.  
  
-   **Replicación**. Un dominio define una partición del árbol de directorio que proporciona los datos que es adecuada para proporcionar servicios necesarios y que se replica entre controladores de dominio. De este modo, todos los controladores de dominio son del mismo nivel en un dominio y se administran como una unidad.  
  
## <a name="active-directory-forest"></a>Bosque de Active Directory  
Una colección de uno o varios dominios de Active Directory que comparten una estructura lógica común, el esquema de directorio y configuración de red, así como las relaciones de confianza automática, bidireccional transitiva. Cada bosque es una instancia única del directorio y define un límite de seguridad.  
  
## <a name="active-directory-functional-level"></a>Nivel funcional de Active Directory  
Un AD DS que permite características avanzadas de AD DS de todo el dominio o bosque.  
  
## <a name="migration"></a>Migración  
El proceso de mover un objeto de un dominio de origen a un dominio de destino, mientras conserva o modificar las características del objeto para que sea accesible en el nuevo dominio.  
  
## <a name="domain-restructure"></a>Reestructuración de dominios  
Un proceso de migración que implica cambiar la estructura de dominio de un bosque. Una reestructuración de dominio puede implicar la consolidación o la adición de dominios, y puede tener lugar entre bosques o dentro de un bosque.  
  
## <a name="domain-consolidation"></a>Consolidación del dominio  
Proceso de reestructuración que implica la eliminación de dominios de AD DS mediante la combinación de su contenido con el contenido de otros dominios.  
  
## <a name="domain-upgrade"></a>Actualización de dominio  
El proceso de actualización del servicio de directorio de un dominio a una versión posterior del servicio de directorio. Esto incluye la actualización del sistema operativo en todos los controladores de dominio y generar el nivel funcional de AD DS cuando corresponda.  
  
## <a name="in-place-domain-upgrade"></a>Dominio actualización en contexto  
El proceso de actualizar los sistemas operativos de todos los controladores de dominio de un dominio, por ejemplo, actualizar Windows Server 2003 a Windows Server 2008 y generar el nivel funcional del dominio, si procede, dejando los objetos de dominio, como los usuarios y grupos, en su lugar.  
  
## <a name="forest-root-domain"></a>Dominio raíz del bosque  
El primer dominio que se crea en el bosque de Active Directory. En este dominio automáticamente se designa como dominio raíz del bosque. Proporciona la base para la infraestructura de bosque de Active Directory.  
  
## <a name="regional-domain"></a>Dominio regional  
Un dominio secundario que se crea en una región geográfica para optimizar el tráfico de replicación.  
  


