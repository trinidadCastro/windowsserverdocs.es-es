---
ms.assetid: 62708b2e-4090-4cf7-8ae6-a557f31f561f
title: "Descripción del modelo lógico de Active Directory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0f8cdc4789d1b3008f3b53104e5517d4ef1e65b9
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="understanding-the-active-directory-logical-model"></a>Descripción del modelo lógico de Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El diseño de la estructura lógica de los servicios de dominio de Active Directory (AD DS) implica definir las relaciones entre los contenedores en el directorio. Estas relaciones podrían estar basadas en requisitos administrativos, como la delegación de autoridad, o se define por requisitos operativos, como la necesidad de controlar la replicación.  
  
Antes de diseñar la estructura lógica de Active Directory, es importante comprender el modelo lógico de Active Directory. AD DS es una base de datos distribuida que almacena y administra la información sobre recursos de red, así como datos específicos de la aplicación desde aplicaciones habilitadas para directorio. AD DS permite a los administradores organizar los elementos de una red (por ejemplo, los usuarios, equipos y dispositivos) en una estructura jerárquica de contención. El contenedor de nivel superior es el bosque. Dentro de bosques son dominios y dentro de los dominios son unidades organizativas (UO). Esto se denomina el modelo lógico porque es independiente de los aspectos físicos de la implementación, como el número de controladores de dominio necesario dentro de cada topología de red y del dominio.  
  
## <a name="active-directory-forest"></a>Bosque de Active Directory  
Un bosque es una colección de uno o varios dominios de Active Directory que comparten una estructura lógica común, el esquema de directorio (definiciones de clases y atributos), la configuración de directorio (información de sitio y de replicación) y el catálogo global (capacidades de búsqueda para todo el bosque). Dominios en el mismo bosque se vinculan automáticamente con relaciones de confianza transitiva bidireccional.  
  
## <a name="active-directory-domain"></a>Dominio de Active Directory  
Un dominio es una partición en un bosque de Active Directory. Partición de datos permite a las organizaciones replicar datos únicamente a donde se necesita. De este modo, el directorio puede escalar globalmente en una red que tenga limitados ancho de banda disponible. Además, el dominio admite un número de otro core funciones relacionadas con la administración, incluidos:  
  
-   Identidad de usuario de toda la red. Los dominios permiten identidades de usuario a puede crearse una sola vez y se hace referencia en cualquier equipo unido al bosque en el que se encuentra el dominio. Controladores de dominio que conforman un dominio se usan para almacenar las credenciales de usuario (por ejemplo, contraseñas o certificados) y cuentas de usuario de forma segura.  
  
-   Autenticación. Controladores de dominio proporcionan servicios de autenticación para los usuarios y proporcionan datos de autorización adicionales como las pertenencias a grupos de usuario, que pueden usarse para controlar el acceso a recursos de la red.  
  
-   Relaciones de confianza. Dominios extender los servicios de autenticación a los usuarios en dominios fuera de su propio bosque mediante relaciones de confianza.  
  
-   Replicación. El dominio define una partición del directorio que contiene datos suficientes para proporcionar servicios de dominio y, a continuación, se replica entre los controladores de dominio. De este modo, todos los controladores de dominio son del mismo nivel en un dominio y se administran como una unidad.  
  
## <a name="active-directory-organizational-units"></a>Unidades organizativas de Active Directory  
Unidades organizativas pueden usarse para crear una jerarquía de contenedores dentro de un dominio. Unidades organizativas se usan para objetos de grupo para fines administrativos, como la aplicación de la directiva de grupo o la delegación de autoridad. Control (a través de una unidad organizativa y los objetos dentro de él) viene determinada por las listas de control de acceso (ACL) en la unidad organizativa y en los objetos en la unidad organizativa. Para facilitar la administración de un gran número de objetos, AD DS admite el concepto de delegación de autoridad. Mediante la delegación, los propietarios pueden transferir control administrativo total o limitado sobre los objetos a otros usuarios o grupos. Delegación es importante porque ayuda a distribuir la administración de un gran número de objetos en un número de personas que son de confianza para realizar tareas de administración.  
  


