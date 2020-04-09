---
title: Planear cuentas de usuario para el entorno de MultiPoint Services
description: Planeación de la información de cuentas de usuario en Multipoint Services
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: d47be540-e891-47bd-85da-6df4bbf93b2f
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 28ee7a1475ec55352fe344842b8df7633abb9137
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853398"
---
# <a name="plan-user-accounts-for-your-multipoint-services-environment"></a>Planear cuentas de usuario para el entorno de MultiPoint Services
La mejor manera de implementar cuentas de usuario en Multipoint Services depende del tamaño y la complejidad de la implementación:  
  
-   **Cuentas de usuario locales** : para una pequeña implementación con solo unos pocos equipos que ejecutan servicios de MultiPoind y pocos usuarios, puede que le resulte más conveniente usar *cuentas de usuario locales* creadas en Multipoint Services. Puede crear una cuenta individual para cada persona que vaya a usar el sistema o crear una cuenta genérica para cada estación, que cualquier usuario puede usar para iniciar sesión. Los administradores de Multipoint Services crean y administran cuentas de usuario locales mediante Multipoint Manager. Las cuentas locales pueden ser administradores, tener derechos administrativos limitados o ser usuarios normales sin acceso a multipoint Services Desktop o Multipoint Manager.  
  
-   **Cuentas de dominio** : Si su entorno tiene muchos equipos que ejecutan Multipoint Services y muchos usuarios, es probable que le resulte más útil configurar una Active Directory Domain Services \(AD DS\) dominio y usar *cuentas de usuario de dominio*, lo que permite a los usuarios tener acceso a su propio perfil de usuario y configuración desde cualquier estación del dominio. Un administrador de dominio debe crear las cuentas de usuario de dominio en el controlador de dominio.  
  
> [!NOTE]  
> En las secciones siguientes se describen los escenarios que se pueden implementar para las cuentas de usuario locales en Multipoint Services. Si usa cuentas de usuario de dominio, consulte el escenario "uno o varios servidores Multipoint en un entorno de red de dominio" en [escenarios de ejemplo: cuentas de usuario de Multipoint Services](Example-scenarios--MultiPoint-Services-user-accounts.md).  
  
## <a name="planning-local-user-accounts"></a>Planeación de cuentas de usuario locales  
En las secciones siguientes se tienen en cuenta las ventajas, las desventajas y los requisitos de varias formas de implementar cuentas de usuario locales compartidas o individuales en el entorno de Windows MultiPoint Services.  
  
### <a name="use-individual-local-user-accounts"></a>Usar cuentas de usuario locales individuales  
Al crear cuentas de usuario locales, tiene la opción dos enfoques.  Asigne cada usuario a un servidor determinado que ejecute Multipoint Services y cree una sola cuenta para cada usuario. O bien, cree cuentas de usuario locales para todos los usuarios en cada equipo que ejecute Multipoint Services. Una ventaja clave de la implementación de cuentas de usuario individuales es que cada usuario tiene su propia experiencia de escritorio de Windows que incluye carpetas privadas para almacenar datos. 
  
Desde la perspectiva de la administración del sistema, la asignación de usuarios a un equipo de Multipoint Services específico puede ser más conveniente. Por ejemplo, si tiene dos servidores multipoint con cinco estaciones cada uno, puede crear cuentas de usuario locales, tal como se muestra en la tabla siguiente.  
  
**Tabla 1: asignación de cuentas de usuario locales a equipos específicos que ejecutan Multipoint Services**  
  
|Equipo A|Equipo B|  
|--------------|--------------|  
|UserAccount_01|UserAccount_06|  
|UserAccount_02|UserAccount_07|  
|UserAccount_03|UserAccount_08|  
|UserAccount_04|UserAccount_09|  
|UserAccount_05|UserAccount_10|  
  
En este escenario, cada usuario tiene una sola cuenta en un equipo determinado. Por lo tanto, todos los usuarios que tengan una cuenta local en el equipo A pueden iniciar sesión en ella o en su cuenta desde cualquier estación asociada con el equipo A. Sin embargo, estos usuarios no pueden tener acceso a sus cuentas si usan una estación asociada al equipo B y viceversa. Una ventaja de este enfoque es que, al conectarse siempre al mismo equipo, los usuarios siempre pueden buscar y acceder a sus archivos.  
  
Por el contrario, también es posible replicar cuentas de usuario individuales en todos los equipos que ejecutan Multipoint Services, tal y como se muestra en la tabla siguiente.  
  
**Tabla 2: replicación de cuentas de usuario en todos los equipos que ejecutan Multipoint Services**  
  
|Equipo A|Equipo B|  
|--------------|--------------|  
|UserAccount_01|UserAccount_01|  
|UserAccount_02|UserAccount_02|  
|UserAccount_03|UserAccount_03|  
|UserAccount_04|UserAccount_04|  
|UserAccount_05|UserAccount_05|  
  
Una ventaja de este enfoque es que los usuarios tienen una cuenta de usuario local en cada Multipoint Services disponible. Sin embargo, las desventajas pueden superar esta ventaja. Por ejemplo, incluso si el nombre de usuario y la contraseña de una persona determinada son iguales en ambos equipos, las cuentas no se vinculan entre sí. Por lo tanto, si un usuario inicia sesión en su cuenta en el equipo A el lunes, guarda un archivo y, a continuación, inicia sesión en su cuenta del equipo B el martes, no podrá obtener acceso al archivo guardado anteriormente en el equipo A. Además, la replicación de cuentas de usuario en varios equipos aumenta la sobrecarga administrativa y los requisitos de almacenamiento.  
  
### <a name="use-generic-local-user-accounts"></a>Usar cuentas de usuario locales genéricas  
Si el sistema Multipoint Services no está conectado a un dominio y no desea crear una cuenta individual para cada usuario, puede crear cuentas genéricas para cada estación. Por ejemplo, si tiene dos equipos que ejecutan Multipoint Services y cinco estaciones están asociadas a cada equipo, es posible que decida crear cuentas de usuario similares a las que se muestran en la tabla siguiente.  
  
**Tabla 3: creación de cuentas de usuario genéricas, una cuenta por estación**  
  
|Equipo A|Equipo B|  
|--------------|--------------|  
|Computer_A Station_01|Computer_B Station_01|  
|Computer_A Station_02|Computer_B Station_02|  
|Computer_A Station_03|Computer_B Station_03|  
|Computer_A Station_04|Computer_B Station_04|  
|Computer_A Station_05|Computer_B Station_05|  
  
En este escenario, todas las cuentas de estación tienen la misma contraseña y las contraseñas y los nombres de cuenta de usuario genérica están disponibles para todos los usuarios. Una ventaja de este enfoque es que la sobrecarga de administrar cuentas de usuario es probable que sea menor que si se usan cuentas individuales, ya que normalmente hay menos estaciones que usuarios. Además, la sobrecarga que se produce al replicar cuentas de usuario en cada servidor se elimina.  
  
Otra opción consiste en crear cuentas genéricas en cada servidor. Cada usuario inicia sesión en un servidor como la misma cuenta. Para permitirlo, debe habilitar varias sesiones por cuenta. Puede simplificar aún más mediante el uso del mismo nombre de cuenta y la misma contraseña en todos los servidores. Esto simplifica el inicio de sesión de los usuarios, que solo necesitan conocer un nombre y una contraseña de cuenta para usar cualquier estación en cualquier servidor. Debe tenerse en en este escenario que todos los usuarios pueden ver cualquier cambio que realice cualquier usuario. Por ejemplo, si se guarda un archivo en el escritorio, todos los usuarios podrán ver el archivo.  
  
> [!IMPORTANT]  
> Es importante comprender que cuando los usuarios comparten una cuenta de usuario, ya sea por servidor o por estación, los archivos guardados en el servidor (incluso los archivos guardados en mis documentos) no son privados. Cualquier usuario que inicie sesión con la cuenta tendrá acceso a esos archivos. Cuando se usa una cuenta por estación, si un usuario guarda archivos en mis documentos en una estación, el usuario no tiene acceso a esos archivos en una estación diferente. Lo mismo ocurre cuando se inicia sesión en distintos equipos Multipoint Services.  
  
Para permitir que los usuarios tengan acceso a sus archivos desde cualquier estación, puede usar un servidor de archivos, crear un recurso compartido de archivos para cada cuenta de usuario o permitir que los usuarios almacenen sus documentos personales en una unidad flash USB u otro dispositivo de almacenamiento privado. Las unidades flash USB individuales permiten a los usuarios individuales almacenar documentos privados incluso si comparten una cuenta de usuario en Multipoint Services.