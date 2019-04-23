---
ms.assetid: 62708b2e-4090-4cf7-8ae6-a557f31f561f
title: Descripción del modelo lógico de Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0e909d4e9c1fb26aa0f7cb97a7dc06192db5cc21
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834276"
---
# <a name="understanding-the-active-directory-logical-model"></a>Descripción del modelo lógico de Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Diseñar la estructura lógica para los servicios de dominio de Active Directory (AD DS) implica definir las relaciones entre los contenedores de su directorio. Estas relaciones podrían basarse en los requisitos administrativos, como la delegación de autoridad, o pueden definirse por los requisitos operativos, tales como la necesidad de controlar la replicación.  
  
Antes de diseñar la estructura lógica de Active Directory, es importante comprender el modelo lógico de Active Directory. AD DS es una base de datos distribuida que almacena y administra información acerca de los recursos de red, así como datos específicos de la aplicación de las aplicaciones habilitadas para directorio. AD DS permite a los administradores organizar los elementos de una red (por ejemplo, los usuarios, equipos y dispositivos) en una estructura de contención jerárquica. El contenedor de nivel superior es el bosque. En bosques son dominios y dentro de los dominios son unidades organizativas (OU). Esto se denomina modelo lógico porque es independiente de los aspectos físicos de la implementación, como el número de controladores de dominio necesarios en cada topología de red y dominio.  
  
## <a name="active-directory-forest"></a>Bosque de Active Directory  
Un bosque es una colección de uno o varios dominios de Active Directory que comparten una estructura lógica común, el esquema de directorio (definiciones de clase y atributo), configuración de directorio (información de sitio y replicación) y (búsqueda en todo el bosque del catálogo global capacidades). Dominios en el mismo bosque se vinculan automáticamente con las relaciones de confianza transitiva bidireccional.  
  
## <a name="active-directory-domain"></a>Dominio de Active Directory  
Un dominio es una partición en un bosque de Active Directory. Creación de particiones de datos permite a las organizaciones replicar datos solo a donde sea necesario. De este modo, el directorio puede adaptar globalmente a través de una red que tiene limitado el ancho de banda disponible. Además, el dominio admite un número de núcleo de otra funciones relacionadas con la administración, incluidas:  
  
-   Identidad de usuario de toda la red. Los dominios permiten las identidades de usuario se crean una vez y se hace referencia en cualquier equipo unido al bosque donde se encuentra el dominio. Controladores de dominio que forman un dominio se usan para almacenar de forma segura las cuentas de usuario y las credenciales de usuario (como contraseñas o certificados).  
  
-   Autenticación. Los controladores de dominio proporcionan servicios de autenticación para los usuarios y proporcionan datos de autorización adicional, como pertenencias a grupos de usuario, que pueden usarse para controlar el acceso a recursos de la red.  
  
-   Relaciones de confianza. Dominios pueden ampliar los servicios de autenticación a los usuarios en dominios fuera de su propio bosque mediante confianzas.  
  
-   Replicación. El dominio define una partición del directorio que contiene datos suficientes para proporcionar servicios de dominio y, a continuación, se replica entre los controladores de dominio. De este modo, todos los controladores de dominio son del mismo nivel en un dominio y se administran como una unidad.  
  
## <a name="active-directory-organizational-units"></a>Unidades organizativas de Active Directory  
Las unidades organizativas se pueden usar para formar una jerarquía de contenedores dentro de un dominio. Las unidades organizativas se usan para agrupar objetos para fines administrativos, como la aplicación de directiva de grupo o delegación de autoridad. Control (a través de una unidad organizativa y los objetos dentro de él) viene determinada por las listas de control de acceso (ACL) en la unidad organizativa y en los objetos en la unidad organizativa. Para facilitar la administración de grandes cantidades de objetos, AD DS es compatible con el concepto de delegación de autoridad. Por medio de la delegación, los propietarios pueden transferir control administrativo total o limitado sobre los objetos a otros usuarios o grupos. La delegación es importante porque ayuda a distribuir la administración de grandes cantidades de objetos en un número de personas que son de confianza para realizar tareas de administración.  
  


