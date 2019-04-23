---
title: Planear cuentas de usuario para el entorno de MultiPoint Services
description: Información de planificación de cuentas de usuario de MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d47be540-e891-47bd-85da-6df4bbf93b2f
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 02862c1a317dfe5deff75be4a80595c8dc8bc3f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864176"
---
# <a name="plan-user-accounts-for-your-multipoint-services-environment"></a>Planear cuentas de usuario para el entorno de MultiPoint Services
La mejor manera de implementar cuentas de usuario de MultiPoint Services depende del tamaño y complejidad de la implementación:  
  
-   **Cuentas de usuario locales** : para una implementación pequeña con sólo unos pocos equipos de servicios de ejecución MultiPoind y algunos de los usuarios, le resultará más conveniente utilizar *cuentas de usuario locales* que se crean en MultiPoint Services. Puede crear una cuenta individual para cada persona que se use el sistema, o cree una cuenta genérica para cada estación, que cualquiera puede utilizar para iniciar sesión. Los administradores de multiPoint Services creación y administración cuentas de usuario local mediante MultiPoint Manager. Las cuentas locales pueden ser administradores, tienen derechos administrativos limitados o ser usuarios normales sin acceso al escritorio de MultiPoint Services o MultiPoint Manager.  
  
-   **Las cuentas de dominio** -si el entorno tiene muchos equipos que ejecuten MultiPoint Services y muchos usuarios, probablemente encontrará lo más útil para configurar un Active Directory Domain Services \(AD DS\) el dominio y utilice *cuentas de usuario de dominio*, que permiten al usuario tener acceso a su propio perfil de usuario y la configuración desde cualquier estación en el dominio. Las cuentas de usuario de dominio deben crearse en el controlador de dominio por un administrador de dominio.  
  
> [!NOTE]  
> Las siguientes secciones describen escenarios que podrían implementar cuentas de usuario local en MultiPoint Services. Si usas cuentas de usuario de dominio, consulte el escenario "uno o más servidores MultiPoint en un entorno de red de dominio" en [escenarios de ejemplo: Las cuentas de usuario de multiPoint Services](Example-scenarios--MultiPoint-Services-user-accounts.md).  
  
## <a name="planning-local-user-accounts"></a>Planeación de cuentas de usuario local  
Tenga en cuenta las siguientes secciones las ventajas, desventajas y los requisitos de varias maneras de implementar cuentas de usuario local compartida o individual en su entorno de Windows MultiPoint Services.  
  
### <a name="use-individual-local-user-accounts"></a>Usar cuentas de usuario locales individuales  
Al crear cuentas de usuario local, tendrá los dos enfoques de opción.  Asignar a cada usuario a un servidor específico que ejecuta MultiPoint Services y la creación de una sola cuenta para cada usuario. O bien, crear cuentas de usuario local para todos los usuarios en cada equipo que ejecuta Multipoint services. Una ventaja clave de la implementación de cuentas de usuario individuales es que cada usuario tiene su propia experiencia de escritorio Windows que incluye las carpetas privadas para almacenar los datos. 
  
Desde una perspectiva de administración del sistema, podría ser más conveniente asignar usuarios a un determinado equipo de MultiPoint Services. Por ejemplo, si tiene dos servidores con cinco estaciones de MultiPoint, puede crear cuentas de usuario locales, como se muestra en la tabla siguiente.  
  
**Tabla 1: Asignación de cuentas de usuario local a equipos específicos que ejecuta MultiPoint Services**  
  
|Equipo A|Equipo B|  
|--------------|--------------|  
|UserAccount_01|UserAccount_06|  
|UserAccount_02|UserAccount_07|  
|UserAccount_03|UserAccount_08|  
|UserAccount_04|UserAccount_09|  
|UserAccount_05|UserAccount_10|  
  
En este escenario, cada usuario tiene una sola cuenta en un equipo determinado. Por lo tanto, todos los usuarios con una cuenta local en un equipo, puede iniciar sesión en sus cuentas desde cualquier estación asociada con el equipo A. Sin embargo, estos usuarios no pueden tener acceso a sus cuentas si usan una estación asociada con el equipo B y viceversa. Una ventaja de este enfoque es que, al conectar siempre en el mismo equipo, los usuarios siempre pueden buscar y acceder a sus archivos.  
  
En cambio, también es posible replicar las cuentas de usuario individual en todos los equipos que ejecuta MultiPoint Services, como se muestra en la tabla siguiente.  
  
**Tabla 2: Replicación de las cuentas de usuario en todos los equipos que ejecuta MultiPoint Services**  
  
|Equipo A|Equipo B|  
|--------------|--------------|  
|UserAccount_01|UserAccount_01|  
|UserAccount_02|UserAccount_02|  
|UserAccount_03|UserAccount_03|  
|UserAccount_04|UserAccount_04|  
|UserAccount_05|UserAccount_05|  
  
Una ventaja de este enfoque es que los usuarios tengan una cuenta de usuario local en cada MultiPoint Services disponibles. Sin embargo, las desventajas es posible que superan esta ventaja. Por ejemplo, incluso si el nombre de usuario y la contraseña para una persona determinada son los mismos en ambos equipos, las cuentas no están vinculadas entre sí. Por lo tanto, si un usuario inicia sesión en su o guarda un archivo de su cuenta en el equipo el lunes y, a continuación, inicia sesión en su cuenta en el equipo B el martes, quien no podrá tener acceso al archivo guardado previamente en el equipo además A. , la replicación de las cuentas de usuario en varios equipos aumenta los requisitos de almacenamiento y la sobrecarga administrativos.  
  
### <a name="use-generic-local-user-accounts"></a>Usar cuentas de usuario local genérico  
Si el sistema MultiPoint Services no está conectado a un dominio y no desea crear una cuenta individual para cada usuario, puede crear cuentas genéricas para cada estación. Por ejemplo, si tiene dos equipos que ejecuten MultiPoint Services y cinco estaciones están asociadas a cada equipo, podría decidir crear cuentas de usuario similares a los que se muestra en la tabla siguiente.  
  
**Tabla 3: Creación de cuentas de usuario genérica, una cuenta cada estación**  
  
|Equipo A|Equipo B|  
|--------------|--------------|  
|Computer_A-Station_01|Computer_B-Station_01|  
|Computer_A-Station_02|Computer_B-Station_02|  
|Computer_A-Station_03|Computer_B-Station_03|  
|Computer_A-Station_04|Computer_B-Station_04|  
|Computer_A-Station_05|Computer_B-Station_05|  
  
En este escenario, todas las cuentas de estación tiene la misma contraseña y las contraseñas y nombres de cuenta de usuario genérica que están disponibles para todos los usuarios. Una ventaja de este enfoque es que la sobrecarga de administrar las cuentas de usuario es probable que sea menor que si se utilizan cuentas individuales, porque normalmente hay menos estaciones que los usuarios. Además, se elimina la sobrecarga ocasionada por la replicación de las cuentas de usuario en cada servidor.  
  
Otra opción es crear cuentas genéricas en cada servidor. Cada usuario inicia sesión en un servidor como la misma cuenta. Para ello, debe habilitar varias sesiones por cuenta. Puede simplificar aún más utilizando el mismo nombre de cuenta y la contraseña en todos los servidores. Esto simplifica el inicio de sesión para los usuarios, que solo necesitan conocer un nombre de cuenta y contraseña para usar cualquier estación en cualquier servidor. Debe tenerse en cuenta que en este escenario todos los usuarios pueden ver cualquier cambio que realice cualquier usuario. Por ejemplo, si un archivo se guarda en el escritorio, todos los usuarios pueden ver el archivo.  
  
> [!IMPORTANT]  
> Es importante comprender que cuando los usuarios comparten una cuenta de usuario, una por cada servidor o uno por cada estación, los archivos guardados en el servidor, incluso los archivos guardados en Mis documentos - no son privados. Cualquier usuario que inicia sesión con la cuenta tiene acceso a esos archivos. Cuando se usa una cuenta de cada estación, si un usuario guarda los archivos en Mis documentos en una estación, el usuario no tiene acceso a esos archivos en una estación diferente. Lo mismo ocurre al iniciar sesión en equipos diferentes de MultiPoint Services.  
  
Para permitir que los usuarios tener acceso a sus archivos desde cualquier estación, puede usar un servidor de archivos, crear un recurso compartido de archivos para cada cuenta de usuario o permitir que los usuarios almacenen sus documentos personales en una unidad flash USB u otro dispositivo de almacenamiento privado. Las unidades flash USB individuales permiten que los usuarios individuales almacenar documentos privados, aunque comparten una cuenta de usuario en un MultiPoint Services.