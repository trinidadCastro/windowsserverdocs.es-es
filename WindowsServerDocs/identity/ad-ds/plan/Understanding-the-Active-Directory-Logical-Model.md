---
ms.assetid: 62708b2e-4090-4cf7-8ae6-a557f31f561f
title: Descripción del modelo lógico de Active Directory
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: cf2b997d601d42a47282df0ed95382e471233ff6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80821618"
---
# <a name="understanding-the-active-directory-logical-model"></a>Descripción del modelo lógico de Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El diseño de la estructura lógica para Active Directory Domain Services (AD DS) implica la definición de las relaciones entre los contenedores del directorio. Estas relaciones pueden estar basadas en requisitos administrativos, como la delegación de autoridad, o pueden estar definidas por requisitos operativos, como la necesidad de controlar la replicación.  
  
Antes de diseñar la estructura lógica de Active Directory, es importante comprender el modelo lógico de Active Directory. AD DS es una base de datos distribuida que almacena y administra información acerca de los recursos de red, así como datos específicos de la aplicación de aplicaciones habilitadas para el directorio. AD DS permite a los administradores organizar los elementos de una red (como usuarios, equipos y dispositivos) en una estructura de contención jerárquica. El contenedor de nivel superior es el bosque. Dentro de los bosques, los dominios y dentro de los dominios son unidades organizativas (OU). Esto se denomina modelo lógico porque es independiente de los aspectos físicos de la implementación, como el número de controladores de dominio necesarios dentro de cada dominio y topología de red.  
  
## <a name="active-directory-forest"></a>Bosque de Active Directory  
Un bosque es una colección de uno o más dominios Active Directory que comparten una estructura lógica común, un esquema de directorio (definiciones de clase y atributo), la configuración de directorios (información de la replicación y del sitio) y el catálogo global (capacidades de búsqueda en todo el bosque). Los dominios del mismo bosque se vinculan automáticamente con relaciones de confianza transitivas bidireccionales.  
  
## <a name="active-directory-domain"></a>Dominio de Active Directory  
Un dominio es una partición de un bosque de Active Directory. La creación de particiones de datos permite a las organizaciones replicar datos solo en donde sea necesario. De esta manera, el directorio se puede escalar globalmente a través de una red con un ancho de banda disponible limitado. Además, el dominio admite varias funciones principales relacionadas con la administración, entre las que se incluyen:  
  
-   Identidad de usuario en toda la red. Los dominios permiten que las identidades de usuario se creen una vez y se haga referencia a ellas en cualquier equipo unido al bosque en el que se encuentra el dominio. Los controladores de dominio que componen un dominio se utilizan para almacenar de forma segura las cuentas de usuario y las credenciales de usuario (como contraseñas o certificados).  
  
-   Autenticación. Los controladores de dominio proporcionan servicios de autenticación para los usuarios y proporcionan datos de autorización adicionales, como la pertenencia a grupos de usuarios, que se pueden usar para controlar el acceso a los recursos de la red.  
  
-   Relaciones de confianza. Los dominios pueden extender los servicios de autenticación a los usuarios de dominios ajenos a su propio bosque por medio de confianzas.  
  
-   Replicación. El dominio define una partición del directorio que contiene los datos suficientes para proporcionar servicios de dominio y, a continuación, lo Replica entre los controladores de dominio. De esta manera, todos los controladores de dominio son del mismo nivel en un dominio y se administran como una unidad.  
  
## <a name="active-directory-organizational-units"></a>Unidades organizativas de Active Directory  
Las unidades organizativas se pueden usar para formar una jerarquía de contenedores dentro de un dominio. Las unidades organizativas se usan para agrupar objetos con fines administrativos, como la aplicación de directiva de grupo o la delegación de autoridad. El control (en una unidad organizativa y los objetos que contiene) viene determinado por las listas de control de acceso (ACL) de la unidad organizativa y los objetos de la unidad organizativa. Para facilitar la administración de un gran número de objetos, AD DS admite el concepto de delegación de autoridad. Mediante la delegación, los propietarios pueden transferir el control administrativo completo o limitado sobre los objetos a otros usuarios o grupos. La delegación es importante porque ayuda a distribuir la administración de un gran número de objetos en una serie de personas que son de confianza para realizar tareas de administración.  
  


