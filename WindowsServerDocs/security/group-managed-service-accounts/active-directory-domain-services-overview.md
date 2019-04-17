---
title: "Información general de servicios de dominio de Active Directory"
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: na
ms.suite: na
ms.technology: security-auditing
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6cfe9479-5d17-41d5-939a-891e5233fdca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 3d0e849edbff3a481ffd28f83d7f14089030920d
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="active-directory-domain-services-overview"></a>Información general de servicios de dominio de Active Directory

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016
  
Un directorio es una estructura jerárquica que almacena información sobre los objetos de la red. Un servicio de directorio, como los servicios de dominio de Active Directory (AD DS), proporciona los métodos para almacenar datos del directorio y hacer que estos datos estén disponibles para los administradores y usuarios de la red. Por ejemplo, AD DS almacena información acerca de las cuentas de usuario, como nombres, contraseñas, números de teléfono y así sucesivamente y permite que otros usuarios autorizados en la misma red acceder a esta información.  
  
Active Directory almacena información sobre los objetos de la red y facilita esta información para administradores y usuarios a encontrar y usar. Active Directory usa un almacén de datos estructurados como base para una organización lógica y jerárquica de información del directorio.  
  
Este almacén de datos, también conocida como el directorio contiene información sobre los objetos de Active Directory. Estos objetos suelen incluyen recursos compartidos como servidores, volúmenes, impresoras y las cuentas de usuario y del equipo de red. Para obtener más información sobre el almacén de datos de Active Directory, consulte [almacén de datos del directorio](https://technet.microsoft.com/library/cc736627(v=ws.10).aspx).  
  
Seguridad se integra con Active Directory mediante la autenticación de inicio de sesión y control de acceso a objetos en el directorio. Con un único inicio de sesión, los administradores pueden administrar datos de directorio y de la organización a través de su red y los usuarios autorizados pueden acceder a recursos en cualquier parte de la red. Administración basada en directivas facilita la administración de incluso las redes más complejas. Para obtener más información sobre la seguridad de Active Directory, consulta Introducción a la seguridad.  
  
Active Directory también incluye:  
* Un conjunto de reglas, **el esquema**, que define las clases de objetos y atributos contenidos en el directorio, las restricciones y límites de instancias de estos objetos y el formato de sus nombres. Para obtener más información acerca del esquema, consulta el esquema.  
  
  
* Un **catálogo global** que contiene información sobre todos los objetos en el directorio. Esto permite que los usuarios y administradores para encontrar información de directorio independientemente de qué dominio en el directorio realmente contiene los datos. Para obtener más información sobre el catálogo global, consulta el rol del catálogo global.  
  
  
* Un **mecanismo de índices y consulta**, de modo que pueden publicarse y encontrar los usuarios de red o aplicaciones objetos y sus propiedades. Para obtener más información sobre cómo consultar el directorio, consulta buscar información del directorio.  
  
  
* Un **servicio de replicación** que distribuye los datos de directorio en una red. Todos los controladores de dominio de un dominio participan en la replicación y contienen una copia completa de toda la información de directorio de su dominio. Cualquier cambio en los datos del directorio se replica en todos los controladores de dominio del dominio. Para obtener más información acerca de la replicación de Active Directory, consulta Introducción a la replicación.  
  
## <a name="understanding-active-directory"></a>Descripción de Active Directory  
 Esta sección proporcionan vínculos a los conceptos básicos de Active Directory:  
   
* [Estructura de Active Directory y tecnologías de almacenamiento](https://technet.microsoft.com/library/cc759186(v=ws.10).aspx)  
* [Funciones de controlador de dominios](https://technet.microsoft.com/library/cc786438(v=ws.10).aspx)   
* Esquema de Active Directory   
* [Descripción de las confianzas](https://technet.microsoft.com/library/cc771294(v=ws.10).aspx)   
* [Tecnologías de replicación de Active Directory](https://technet.microsoft.com/library/cc786438(v=ws.10).aspx)   
* [Búsqueda de Active Directory y tecnologías de publicación](https://technet.microsoft.com/library/cc775686(v=ws.10).aspx)   
* Interoperar con DNS y directiva de grupo   
* [Descripción del esquema](https://technet.microsoft.com/library/cc759402(v=ws.10).aspx)   
  
Para obtener una lista detallada de los conceptos de Active Directory, consulte [descripción Active Directory](https://technet.microsoft.com/library/cc781408(v=ws.10).aspx).   

