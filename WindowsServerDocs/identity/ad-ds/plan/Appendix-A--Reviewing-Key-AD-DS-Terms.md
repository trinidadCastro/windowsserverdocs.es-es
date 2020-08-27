---
ms.assetid: 87196b65-a356-409f-9af0-b5950797d668
title: 'Apéndice A: revisar los términos de la clave AD DS'
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 2e899b3f61a23e29a3ecd6a312e50b596ce4e172
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/26/2020
ms.locfileid: "88941245"
---
# <a name="appendix-a-reviewing-key-ad-ds-terms"></a>Anexo A: revisión de términos de clave de AD DS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Los siguientes términos son relevantes para el proceso de implementación de Windows Server 2008 Active Directory Domain Services (AD DS).

## <a name="active-directory-domain"></a>Dominio de Active Directory
Una unidad administrativa en una red de equipos que, para facilitar la administración, agrupa varias funcionalidades, entre las que se incluyen las siguientes:

-   **Identidad de usuario en toda la red**. En los dominios, las identidades de usuario se pueden crear una vez y después se hace referencia a ellas en cualquier equipo que esté unido al bosque en el que se encuentra el dominio. Controladores de dominio que componen las cuentas de usuario y las credenciales de usuario del almacén de dominio, como contraseñas o certificados, de forma segura.

-   **Autenticación**. Los controladores de dominio proporcionan servicios de autenticación para los usuarios. También proporcionan datos de autorización adicionales, como la pertenencia a grupos de usuarios. Los administradores pueden usar estos servicios para controlar el acceso a los recursos de la red.

-   **Relaciones de confianza**. Los dominios extienden los servicios de autenticación a los usuarios de otros dominios de su propio bosque mediante confianzas bidireccionales automáticas. Los dominios también extienden los servicios de autenticación a los usuarios de dominios de otros bosques mediante confianzas de bosque o confianzas externas creadas manualmente.

-   **Administración de directivas**. Un dominio es un ámbito de directivas administrativas, como la complejidad de la contraseña y las reglas de reutilización de contraseñas.

-   **Replicación**. Un dominio define una partición del árbol de directorios que proporciona los datos adecuados para proporcionar los servicios necesarios y que se replican entre los controladores de dominio. De esta manera, todos los controladores de dominio son del mismo nivel en un dominio y se administran como una unidad.

## <a name="active-directory-forest"></a>Bosque de Active Directory
Colección de uno o más dominios de Active Directory que comparten una estructura lógica común, un esquema de directorio y una configuración de red, así como relaciones de confianza transitivas, bidireccionales y automáticas. Cada bosque es una instancia única del directorio y define un límite de seguridad.

## <a name="active-directory-functional-level"></a>Active Directory nivel funcional
Un valor de AD DS que habilita las características avanzadas para todo el dominio o para AD DS todo el bosque.

## <a name="migration"></a>Migración
Proceso de mover un objeto de un dominio de origen a un dominio de destino, a la vez que se conservan o modifican las características del objeto para que sea accesible en el nuevo dominio.

## <a name="domain-restructure"></a>Reestructuración de dominios
Proceso de migración que implica el cambio de la estructura de dominio de un bosque. Una reestructuración de dominio puede implicar la consolidación o la adición de dominios, y puede tener lugar entre bosques o dentro de un bosque.

## <a name="domain-consolidation"></a>Consolidación de dominios
Proceso de reestructuración que implica la eliminación de AD DS dominios combinando su contenido con el contenido de otros dominios.

## <a name="domain-upgrade"></a>Actualización de dominio
Proceso de actualización del servicio de directorio de un dominio a una versión posterior del servicio de directorio. Esto incluye la actualización del sistema operativo en todos los controladores de dominio y la elevación del nivel funcional AD DS cuando proceda.

## <a name="in-place-domain-upgrade"></a>Actualización de dominio local
Proceso de actualización de los sistemas operativos de todos los controladores de dominio de un dominio determinado, por ejemplo, la actualización de Windows Server 2003 a Windows Server 2008 y la elevación del nivel funcional del dominio, si es aplicable, y el abandono de objetos de dominio, como usuarios y grupos, en su lugar.

## <a name="forest-root-domain"></a>Dominio raíz del bosque
Primer dominio que se crea en el bosque de Active Directory. Este dominio se designa automáticamente como dominio raíz del bosque. Proporciona la base para la infraestructura de bosque de Active Directory.

## <a name="regional-domain"></a>Dominio regional
Un dominio secundario que se crea en una región geográfica para optimizar el tráfico de replicación.



