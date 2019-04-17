---
ms.assetid: ef4ef4a9-8969-4ad0-bd17-b2bb24f36ef6
title: "Selección de dominio raíz del bosque"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d87273df1e42e1fa88df09e0b86d4c86ea75ee3f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="selecting-the-forest-root-domain"></a>Selección de dominio raíz del bosque

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El primer dominio que se implementa en un bosque de Active Directory se denomina dominio raíz del bosque. En este dominio permanece dominio raíz del bosque durante el ciclo de vida de la implementación de AD DS.  
  
El dominio raíz contiene los grupos Administradores de empresa y administradores de esquema. Estos grupos de administrador de servicio se usan para administrar las operaciones de nivel de bosque, como la adición y eliminación de dominios y la implementación de los cambios en el esquema.  
  
Selección de dominio raíz del bosque consiste en determinar si uno de los dominios de Active Directory en el diseño de dominio puede funcionar como dominio raíz del bosque o si es necesario implementar un dominio raíz del bosque dedicado.  
  
Para obtener información acerca de la implementación de un dominio raíz del bosque, vea [implementar Windows Server 2008 dominio raíz del bosque](https://technet.microsoft.com/library/cc731174.aspx).  
  
## <a name="choosing-a-regional-or-dedicated-forest-root-domain"></a>Elección de un dominio raíz del bosque regional o dedicado  
Si estás aplicando un modelo de dominio único, el dominio solo funciona como dominio raíz del bosque. Si estás aplicando un modelo de dominio múltiple, puedes implementar un dominio raíz del bosque dedicado o selecciona un dominio regional para que funcione como dominio raíz del bosque.  
  
### <a name="dedicated-forest-root-domain"></a>Dominio raíz del bosque dedicado  
Dominio raíz del bosque dedicado es un dominio que se crea específicamente para que funcione como la raíz del bosque. No contiene ninguna cuenta de usuario que no sean las cuentas de administrador de servicios de dominio raíz del bosque. Además, no representa cualquier región geográfica de la estructura del dominio. Todos los demás dominios del bosque son elementos secundarios del dominio raíz del bosque dedicado.  
  
Uso de una raíz de bosque dedicado ofrece las siguientes ventajas:  
  
-   Operativa separación de los administradores de servicio de bosque de los administradores de servicios de dominio. En un entorno de dominio único, los miembros de los administradores de dominio y grupos de administradores integrados usar herramientas estándar y los procedimientos para ser miembros del grupo de administradores de empresa y administradores de esquema. En un bosque que usa un dominio raíz del bosque dedicado, los miembros de los administradores de dominio y grupo de administradores integrado en los dominios regionales no pueden hacer a sí mismos miembros de los grupos de administrador de servicios de nivel de bosque mediante el uso de herramientas estándar y procedimientos.  
  
-   Protección frente a cambios de funcionamiento de otros dominios. Dominio raíz del bosque dedicado no representa una región geográfica determinada de la estructura del dominio. Por este motivo, no se ve afectado por reorganizaciones u otros cambios que se convertirán en el cambio de nombre o reestructuración de dominios.  
  
-   Actúa como una raíz independiente para que no país o región parece subordinadas a otra región. Algunas organizaciones puede que prefieras evitar que aparezca un país o región está subordinada a otro país o región en el espacio de nombres. Cuando usas un dominio raíz del bosque dedicado, todos los dominios regionales pueden sistemas del mismo nivel en la jerarquía de dominios.  
  
En un entorno de varios dominios de configuración regional en el que se usa una raíz del bosque dedicado, la replicación de dominio raíz del bosque tiene un impacto mínimo en la infraestructura de red. Esto es porque la raíz del bosque hospeda solo las cuentas de administrador de servicio. La mayoría de las cuentas de usuario en el bosque y otros datos específicos del dominio se almacenan en los dominios regionales.  
  
Un inconveniente de utilizar un dominio raíz del bosque dedicado es que crea una carga administrativa adicional para admitir el dominio adicional.  
  
### <a name="regional-domain-as-a-forest-root-domain"></a>Dominio regional como dominio raíz del bosque  
Si eliges no implementar un dominio raíz del bosque dedicado, debes seleccionar un dominio regional para que funcione como dominio raíz del bosque. En este dominio es el dominio primario de todos los otros dominios regionales y será el primer dominio que implementas. El dominio raíz contiene las cuentas de usuario y se administra en la misma manera que se administran los otros dominios regionales. La principal diferencia es que también incluye los grupos Administradores de empresa y administradores de esquema.  
  
Crea la ventaja de selección de un dominio regional a función como dominio raíz del bosque es que no se crea la sobrecarga adicional de administración que mantiene un dominio adicional. Selecciona un dominio regional adecuado sea la raíz del bosque, por ejemplo, el dominio que representa la sede central o la región que tiene las conexiones de red más rápidas. Si es difícil para la organización seleccionar un dominio regional es el dominio raíz del bosque, puede usar un modelo de raíz del bosque dedicado en su lugar.  
  
## <a name="assigning-the-forest-root-domain-name"></a>Asignar el nombre de dominio raíz de bosque  
El nombre de dominio raíz de bosque también es el nombre del bosque. El nombre de raíz del bosque es un nombre de sistema de nombres de dominio (DNS) que consta de un prefijo y un sufijo en forma de prefix.suffix. Por ejemplo, una organización podría tener el nombre de la raíz de bosque corp.contoso.com. En este ejemplo, corp es el prefijo y contoso.com es el sufijo.  
  
Selecciona el sufijo de una lista de nombres existentes en la red. Con el prefijo, selecciona un nombre nuevo que no ha utilizado anteriormente en la red. Asociando un nuevo prefijo a un sufijo existente, crear un único espacio de nombres. Crear un nuevo espacio de nombres para los servicios de dominio de Active Directory (AD DS), se garantiza que cualquier infraestructura DNS no necesita modificarse para dar cabida a AD DS.  
  
### <a name="selecting-a-suffix"></a>Seleccionar un sufijo  
Para seleccionar un sufijo de dominio raíz del bosque:  
  
1.  Ponte en contacto con el propietario DNS para la organización para obtener una lista de sufijos DNS registrados que se usan en la red que se hospedará en AD DS. Ten en cuenta que los sufijos usados en la red interna pueden ser diferentes de los sufijos usados de manera externa. Por ejemplo, una organización puede usar contosopharma.com en Internet y en contoso.com de la red corporativa interna.  
  
2.  Consulte el propietario DNS para seleccionar un sufijo para su uso con AD DS. Si no existe ningún sufijo adecuado, registrar un nombre nuevo con una entidad de nombres de Internet.  
  
Te recomendamos que uses nombres DNS que se registran con una entidad de Internet en el espacio de nombres de Active Directory. Solo los nombres registrados se garantiza que sea único global. Si más adelante otra organización registra el mismo nombre de dominio DNS (o si se combina con la organización, adquiere o se adquiere por otra compañía que usa el mismo nombre DNS), las dos infraestructuras no pueden interactuar entre sí.  
  
> [!CAUTION]  
> No uses nombres DNS de etiqueta única. Para obtener más información, consulta la información sobre cómo configurar Windows para dominios con nombres DNS de etiqueta única ([https://go.microsoft.com/fwlink/?LinkId=106631](https://go.microsoft.com/fwlink/?LinkId=106631)). Además, no recomendamos usar sufijos no registrados, como local.  
  
### <a name="selecting-a-prefix"></a>Seleccionar un prefijo  
Si has elegido un sufijo registrado que ya está en uso en la red, seleccione un prefijo de nombre de dominio de raíz de bosque mediante el uso de las reglas de prefijo en la siguiente tabla. Agrega un prefijo que no está actualmente en uso para crear un nuevo nombre subordinado. Por ejemplo, si el nombre de la raíz DNS es contoso.com, puedes crear concorp.contoso.com de nombre de dominio de Active Directory bosque raíz si la concorp.contoso.com de espacio de nombres no está en uso en la red. Esta nueva bifurcación del espacio de nombres se dedican a AD DS y se puede integrar fácilmente con la implementación de DNS existente.  
  
Si ha seleccionado un dominio regional para que funcione como un dominio raíz del bosque, debes seleccionar un prefijo nuevo para el dominio. Dado que el nombre de dominio raíz de bosque afecta a todos los otros nombres de dominio en el bosque, un nombre regionales no puede ser apropiado. Si estás usando un sufijo nuevo que no está actualmente en uso en la red, puedes usarlo como el nombre de dominio raíz de bosque sin elegir un prefijo adicional.  
  
La siguiente tabla enumera las reglas para seleccionar un prefijo de un nombre DNS registrado.  
  
|Regla|Explicación|  
|--------|---------------|  
|Selecciona un prefijo que no es probable que quedar obsoletos.|Evitar que los nombres, como una línea o el sistema operativo que se puede cambiar en el futuro. Te recomendamos que uses nombres genéricos como corp o ds.|  
|Selecciona un prefijo que incluye caracteres estándar de Internet.|A la Z, z, 0-9 y (-), pero no totalmente numéricos.|  
|Incluir 15 caracteres o menos en el prefijo.|Si eliges una longitud de prefijo de 15 caracteres o menos, el nombre NetBIOS es el mismo que el prefijo.|  
  
Es importante para el propietario de DNS de Active Directory para que funcione con el propietario DNS para la organización para obtener la propiedad del nombre que se usará para el espacio de nombres de Active Directory. Para obtener más información sobre el diseño de una infraestructura DNS para admitir AD DS, consulta [crear un diseño de la infraestructura de DNS](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md).  
  
## <a name="documenting-the-forest-root-domain-name"></a>Documentar el nombre de dominio raíz de bosque  
El prefijo DNS y el sufijo que selecciones para el dominio raíz del documento. En este punto, identifica qué dominio será la raíz del bosque. Puedes agregar la información de nombre de dominio de raíz de bosque a la hoja de cálculo "Dominio planear" que creaste para tu plan para dominios nuevos y actualizados y los nombres de dominio del documento. Para abrirlo, descargar Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip de trabajo Aid para Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)) y abre "Dominio planear" (DSSLOGI_5.doc).  
  


