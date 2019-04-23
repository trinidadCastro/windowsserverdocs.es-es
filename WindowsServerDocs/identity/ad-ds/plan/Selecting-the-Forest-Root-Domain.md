---
ms.assetid: ef4ef4a9-8969-4ad0-bd17-b2bb24f36ef6
title: Seleccionar el dominio raíz del bosque
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c669a5c8103fba52140147957823db978f8b6955
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871356"
---
# <a name="selecting-the-forest-root-domain"></a>Seleccionar el dominio raíz del bosque

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El primer dominio que se implementa en un bosque de Active Directory se denomina dominio raíz del bosque. Este dominio sigue siendo el dominio raíz del bosque para el ciclo de vida de la implementación de AD DS.  
  
El dominio raíz del bosque contiene los grupos Administradores de empresas y administradores de esquema. Estos grupos de administrador de servicio se usan para administrar las operaciones de nivel de bosque, como la adición y eliminación de dominios y la implementación de cambios en el esquema.  
  
Seleccione el dominio raíz del bosque consiste en determinar si uno de los dominios de Active Directory en el diseño de dominio pueda funcionar como dominio raíz del bosque o si necesita implementar un dominio raíz del bosque dedicado.  
  
Para obtener información sobre cómo implementar un dominio raíz del bosque, vea [implementar un dominio raíz del bosque de Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
  
## <a name="choosing-a-regional-or-dedicated-forest-root-domain"></a>Elegir un dominio raíz del bosque regionales o dedicado

Si va a aplicar un modelo de dominio único, el dominio individual que funciona como dominio raíz del bosque. Si está aplicando varios modelos de dominio, puede elegir implementar un dominio raíz del bosque dedicado o seleccione un dominio regional para que funcione como dominio raíz del bosque.  
  
### <a name="dedicated-forest-root-domain"></a>Dominio raíz del bosque dedicado

Un dominio raíz del bosque dedicado es un dominio que se crea específicamente para que funcione como la raíz del bosque. No tiene ninguna cuenta de usuario que no sean las cuentas de administrador de servicio para el dominio raíz del bosque. Además, no representa ninguna región geográfica en la estructura de dominios. Todos los demás dominios del bosque son elementos secundarios del dominio raíz del bosque dedicado.  

Utilizando una raíz de bosque dedicado ofrece las siguientes ventajas:  

- Separación operativa de los administradores de servicios del bosque de los administradores de servicios de dominio. En un entorno de dominio único, los miembros del grupo Administradores integrado y Admins. del dominio pueden utilizar las herramientas estándar y los procedimientos para hacer los miembros de los grupos Administradores de empresas y administradores de esquema. En un bosque que usa un dominio raíz del bosque dedicado, los miembros del grupo de administradores integrado en los dominios regionales y Admins. del dominio no pueden hacer miembros de los grupos de administrador de servicios de nivel de bosque mediante el uso de procedimientos y las herramientas estándar.  
- Protección frente a cambios operacionales en otros dominios. Un dominio raíz del bosque dedicado no representa una región geográfica determinada en la estructura de dominios. Por este motivo, no se ve afectado por reorganizaciones u otros cambios producidos en el cambio de nombre o la reestructuración de dominios.  
- Actúa como una raíz neutra para que no aparezca ningún país o región que esté subordinado a otra región. Es posible que algunas organizaciones prefieren evitar la apariencia de un país o región está subordinada a otro país o región en el espacio de nombres. Cuando se usa un dominio raíz del bosque dedicado, todos los dominios regionales pueden elementos del mismo nivel en la jerarquía de dominios.  

En un entorno de dominio de configuración regional múltiple en el que se usa una raíz de bosque dedicado, la replicación del dominio raíz del bosque tiene un impacto mínimo en la infraestructura de red. Esto es porque la raíz del bosque solo hospeda las cuentas de administrador de servicio. La mayoría de las cuentas de usuario en el bosque y otros datos específicos de dominio se almacenan en los dominios regionales.  
  
Una desventaja de utilizar un dominio raíz del bosque dedicado es que crea una sobrecarga administrativa adicional para admitir el dominio adicional.  
  
### <a name="regional-domain-as-a-forest-root-domain"></a>Dominio regional como un dominio raíz del bosque

Si elige no implementar un dominio raíz de bosque dedicada, debe seleccionar un dominio regional para que funcione como dominio raíz del bosque. Este dominio es el dominio primario de todos los demás dominios regionales y será el primer dominio que se implementa. El dominio raíz del bosque contiene cuentas de usuario y se administra de la misma manera que se administran los demás dominios regionales. La principal diferencia es que también incluye los grupos Administradores de empresas y administradores de esquema.  
  
Crea la ventaja de seleccionar un dominio regional que funcione como dominio raíz del bosque es que no crea la sobrecarga de administración adicionales que mantiene un dominio adicional. Seleccione un dominio regional adecuado para ser la raíz del bosque, por ejemplo, el dominio que representa su sede central o la región que tiene las conexiones de red más rápidas. Si es difícil para su organización seleccionar un dominio regional es el dominio raíz del bosque, puede elegir usar un modelo de raíz de bosque dedicado en su lugar.  
  
## <a name="assigning-the-forest-root-domain-name"></a>Asignar el nombre de dominio raíz de bosque

El nombre de dominio raíz de bosque es también el nombre del bosque. El nombre raíz del bosque es un nombre de sistema de nombres de dominio (DNS) que consta de un prefijo y un sufijo en forma de prefix.suffix. Por ejemplo, una organización podría tener el nombre de raíz de bosque corp.contoso.com. En este ejemplo, corp es el prefijo y contoso.com es el sufijo.  
  
Seleccione el sufijo de una lista de nombres existentes en la red. Para el prefijo, seleccione un nuevo nombre que no se ha utilizado previamente en la red. Si se adjunta un nuevo prefijo para un sufijo existente, cree un espacio de nombres único. Crear un nuevo espacio de nombres para los servicios de dominio de Active Directory (AD DS), se garantiza que las infraestructuras DNS existentes no necesitan modificarse para dar cabida a AD DS.  
  
### <a name="selecting-a-suffix"></a>Seleccionar un sufijo

Para seleccionar un sufijo para el dominio raíz del bosque:  
  
1. Póngase en contacto con el propietario del DNS para la organización para obtener una lista de sufijos DNS registrados que se usan en la red que va a hospedar AD DS. Tenga en cuenta que los sufijos utilizados en la red interna pueden ser diferentes que los sufijos utilizados externamente. Por ejemplo, una organización podría utilizar contosopharma.com en Internet y contoso.com en la red corporativa interna.  
  
2. Consulte al propietario DNS para seleccionar un sufijo para su uso con AD DS. Si no existe ningún sufijo adecuado, registre un nombre nuevo con una autoridad de nombres de Internet.  
  
Se recomienda usar nombres de DNS que se registran con una autoridad de Internet en el espacio de nombres de Active Directory. Solo los nombres registrados se garantiza que sea globalmente único. Si más adelante la otra organización registra el mismo nombre de dominio DNS (o si se combina con la organización, adquiere o se adquiere por otra compañía que usa el mismo nombre DNS), las dos infraestructuras no interactúan entre sí.  
  
> [!CAUTION]  
> No utilice nombres de DNS de etiqueta única. Para obtener más información, consulte información acerca de cómo configurar Windows para dominios con nombres DNS de etiqueta única ([https://go.microsoft.com/fwlink/?LinkId=106631](https://go.microsoft.com/fwlink/?LinkId=106631)). Además, no se recomienda usar sufijos no registrados, como. local.  
  
### <a name="selecting-a-prefix"></a>Seleccionar un prefijo

Si ha elegido un sufijo registrado que ya está en uso en la red, seleccione un prefijo para el nombre de dominio raíz de bosque mediante las reglas de prefijo en la tabla siguiente. Agregue un prefijo que no está actualmente en uso para crear un nuevo nombre subordinado. Por ejemplo, si el nombre de raíz DNS es contoso.com, puede crear concorp.contoso.com de nombre de dominio de Active Directory bosque raíz si el espacio de nombres concorp.contoso.com no está en uso en la red. Esta nueva rama del espacio de nombres se dedicará a AD DS y se puede integrar fácilmente con la implementación de DNS existente.  
  
Si ha seleccionado un dominio regional para que funcione como un dominio raíz del bosque, debe seleccionar un nuevo prefijo para el dominio. Dado que el nombre de dominio raíz de bosque afecta a todos los demás nombres de dominio del bosque, un nombre regionales podría no ser adecuado. Si usa un nuevo sufijo que no está actualmente en uso en la red, puede usar como el nombre de dominio raíz de bosque sin elegir un prefijo adicional.  
  
En la tabla siguiente se enumera las reglas para la selección de un prefijo para un nombre DNS registrado.  
  
|Regla|Explicación|  
|--------|---------------|  
|Seleccione un prefijo que no es probable que quede obsoleta.|Evite nombres como una línea de producto o sistema operativo que podrían cambiar en el futuro. Se recomienda usar nombres genéricos como corp o ds.|  
|Seleccione un prefijo que incluye caracteres estándar de Internet.|A-z, a-z, 0-9 y (-), pero no totalmente numérica.|  
|Incluir 15 caracteres o menos en el prefijo.|Si elige una longitud de prefijo de 15 caracteres o menos, el nombre de NetBIOS es el mismo que el prefijo.|  
  
Es importante para el propietario del DNS de Active Directory para que funcione con el propietario DNS para la organización para obtener la propiedad del nombre que se usará para el espacio de nombres de Active Directory. Para obtener más información sobre cómo diseñar una infraestructura DNS para admitir AD DS, consulte [crear un diseño de la infraestructura DNS](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md).  
  
## <a name="documenting-the-forest-root-domain-name"></a>Documentar el nombre de dominio raíz de bosque

Documentar el prefijo de DNS y el sufijo que seleccionó para el dominio raíz del bosque. En este momento, identifique qué dominio será la raíz del bosque. Puede agregar la información de nombre de dominio de raíz de bosque a la hoja de cálculo "Planeación de dominio" que creó para documentar el plan para los nombres de dominio y dominios nuevos y actualizados. Para abrirlo, descargue Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip desde [trabajo ayudas para Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558) y abra el "Dominio planeación" (DSSLOGI_5.doc).
