---
ms.assetid: f052dfcd-dace-4485-8d0a-cc7df5cf3751
title: Introducción a Active Directory Domain Services
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 069cdb493cd0ad442e8922ec67c2b9cc6b2ec5fc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858176"
---
# <a name="active-directory-domain-services-overview"></a>Introducción a Active Directory Domain Services

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


Un directorio es una estructura jerárquica que almacena información acerca de los objetos de la red. Un servicio de directorio, como los servicios de dominio de Active Directory (AD DS), proporciona los métodos para almacenar datos de directorio y hacer que estos datos estén disponibles para los administradores y usuarios de la red. Por ejemplo, AD DS almacena información acerca de las cuentas de usuario, como nombres, contraseñas, números de teléfono y así sucesivamente y permite que otros usuarios autorizados en la misma red tener acceso a esta información.

Active Directory almacena información acerca de los objetos de la red y facilita esta información para administradores y usuarios a encontrar y usar. Active Directory utiliza un almacén de datos estructurado como base para una organización lógica y jerárquica de información del directorio.

Este almacén de datos, también conocido como el directorio contiene información acerca de los objetos de Active Directory. Normalmente, estos objetos incluyen los recursos compartidos, como servidores, volúmenes, impresoras y las cuentas de usuario y equipo de red. Para obtener más información sobre el almacén de datos de Active Directory, consulte [almacén de datos de directorio](https://technet.microsoft.com/library/cc736627(v=ws.10).aspx).

Seguridad se integra con Active Directory mediante la autenticación de inicio de sesión y control de acceso a objetos en el directorio. Con un único inicio de sesión, los administradores pueden administrar datos de directorio y la organización a lo largo de su red y los usuarios autorizados pueden acceder a recursos en cualquier parte de la red. La administración basada en directiva facilita la administración de incluso las redes más complejas. Para obtener más información acerca de la seguridad de Active Directory, consulte [información general sobre seguridad](../../plan/security-best-practices/best-practices-for-securing-active-directory.md).

Active Directory también incluye:
* Un conjunto de reglas, **el esquema**, que define las clases de objetos y atributos incluidos en el directorio, las restricciones y límites de las instancias de estos objetos y el formato de sus nombres. Para obtener más información acerca del esquema, vea el esquema.


* Un **catálogo global** que contiene información sobre todos los objetos en el directorio. Esto permite que los usuarios y administradores para buscar información de directorio con independencia del dominio en el directorio contiene realmente los datos. Para obtener más información acerca del catálogo global, consulte el rol del catálogo global.


* Un **mecanismo de consulta e índice**, de modo que los objetos y sus propiedades se pueden publicar y encontrar los usuarios de red o las aplicaciones. Para obtener más información sobre cómo consultar el directorio, consulte Buscar información del directorio.


* Un **servicio de replicación** que distribuye los datos de directorio en una red. Todos los controladores de dominio en un dominio participan en la replicación y contienen una copia completa de toda la información de directorio de su dominio. Cualquier cambio en los datos del directorio se replica en todos los controladores de dominio del dominio. Para obtener más información acerca de la replicación de Active Directory, consulte Introducción a la replicación.

## <a name="understanding-active-directory"></a>Descripción de Active Directory
 Esta sección proporciona vínculos a los conceptos de Active Directory:
 
* [Estructura de Active Directory y las tecnologías de almacenamiento](https://technet.microsoft.com/library/cc759186(v=ws.10).aspx)
* [Roles de controlador de dominios](https://technet.microsoft.com/library/cc786438(v=ws.10).aspx) 
* Esquema de Active Directory 
* [Descripción de confianzas](https://technet.microsoft.com/library/cc771294(v=ws.10).aspx) 
* [Tecnologías de replicación de Active Directory](https://technet.microsoft.com/library/cc786438(v=ws.10).aspx) 
* [Búsqueda de Active Directory y las tecnologías de publicación](https://technet.microsoft.com/library/cc775686(v=ws.10).aspx) 
* Interoperación con DNS y directiva de grupo 
* [Descripción del esquema](https://technet.microsoft.com/library/cc759402(v=ws.10).aspx) 

Para obtener una lista detallada de los conceptos de Active Directory, consulte [descripción de Active Directory](https://technet.microsoft.com/library/cc781408(v=ws.10).aspx). 


