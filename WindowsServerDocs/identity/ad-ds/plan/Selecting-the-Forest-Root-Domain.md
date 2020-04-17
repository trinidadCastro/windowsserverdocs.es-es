---
ms.assetid: ef4ef4a9-8969-4ad0-bd17-b2bb24f36ef6
title: Seleccionar el dominio raíz del bosque
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: abc02bec101b39a66a78da871f838d2585d89377
ms.sourcegitcommit: af1cf89632d62a94943d3ad9f6b5234b88499278
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81524930"
---
# <a name="selecting-the-forest-root-domain"></a>Seleccionar el dominio raíz del bosque

> Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El primer dominio que se implementa en un bosque de Active Directory se denomina dominio raíz del bosque. Este dominio sigue siendo el dominio raíz del bosque del ciclo de vida de la implementación de AD DS.

El dominio raíz del bosque contiene los grupos administradores de empresas y administradores de esquema. Estos grupos de administradores de servicios se utilizan para administrar operaciones de nivel de bosque, como la adición y eliminación de dominios y la implementación de cambios en el esquema.

La selección del dominio raíz del bosque implica determinar si uno de los dominios Active Directory del diseño del dominio puede funcionar como dominio raíz del bosque o si necesita implementar un dominio raíz del bosque dedicado.

Para obtener información acerca de la implementación de un dominio raíz del bosque, vea [implementar un dominio raíz del bosque de Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731174(v=ws.10)).

## <a name="choosing-a-regional-or-dedicated-forest-root-domain"></a>Elección de un dominio raíz de bosque regional o dedicado

Si está aplicando un modelo de dominio único, el dominio único funciona como el dominio raíz del bosque. Si va a aplicar un modelo de varios dominios, puede elegir implementar un dominio raíz de bosque dedicado o seleccionar un dominio regional para que funcione como dominio raíz del bosque.

### <a name="dedicated-forest-root-domain"></a>Dominio raíz del bosque dedicado

Un dominio raíz del bosque dedicado es un dominio que se crea específicamente para que funcione como la raíz del bosque. No contiene ninguna cuenta de usuario distinta de las cuentas de administrador de servicios para el dominio raíz del bosque. Además, no representa ninguna región geográfica en la estructura de dominios. El resto de dominios del bosque son secundarios del dominio raíz del bosque dedicado.

El uso de una raíz de bosque dedicada proporciona las siguientes ventajas:

- Separación operativa de los administradores de servicios de bosque de los administradores de servicios de dominio. En un entorno de un solo dominio, los miembros de los grupos Admins. del dominio y administradores integrados pueden usar las herramientas y los procedimientos estándar para que sean miembros de los grupos administradores de empresas y administradores de esquema. En un bosque que utiliza un dominio raíz de bosque dedicado, los miembros de los grupos administradores de dominio y administradores integrados en los dominios regionales no pueden ser miembros de los grupos de administradores de servicios de nivel de bosque mediante el uso de procedimientos y herramientas estándar.
- Protección contra cambios operativos en otros dominios. Un dominio raíz del bosque dedicado no representa una región geográfica determinada en la estructura del dominio. Por esta razón, no se ve afectado por las reorganizaciones u otros cambios que dan como resultado el cambio de nombre o la reestructuración de dominios.
- Actúa como una raíz neutra para que no aparezca ningún país o región que esté subordinado a otra región. Es posible que algunas organizaciones prefieran evitar la aparición de que un país o región esté subordinado a otro país o región del espacio de nombres. Cuando se usa un dominio raíz de bosque dedicado, todos los dominios regionales pueden estar en el mismo nivel en la jerarquía de dominios.

En un entorno de varios dominios regionales en el que se usa una raíz de bosque dedicada, la replicación del dominio raíz del bosque tiene un impacto mínimo en la infraestructura de red. Esto se debe a que la raíz del bosque solo hospeda las cuentas de administrador de servicios. La mayoría de las cuentas de usuario en el bosque y otros datos específicos del dominio se almacenan en los dominios regionales.

Una desventaja del uso de un dominio raíz del bosque dedicado es que crea una sobrecarga de administración adicional para admitir el dominio adicional.

### <a name="regional-domain-as-a-forest-root-domain"></a>Dominio regional como dominio raíz del bosque

Si decide no implementar un dominio raíz del bosque dedicado, debe seleccionar un dominio regional para que funcione como dominio raíz del bosque. Este dominio es el dominio primario de todos los demás dominios regionales y será el primer dominio que implemente. El dominio raíz del bosque contiene cuentas de usuario y se administra de la misma manera que se administran los demás dominios regionales. La principal diferencia es que también incluye los grupos administradores de empresas y administradores de esquema.

La ventaja de seleccionar un dominio regional para que funcione como dominio raíz del bosque es que no crea la sobrecarga de administración adicional que el mantenimiento de un dominio adicional crea. Seleccione un dominio regional adecuado para que sea la raíz del bosque, como el dominio que representa la oficina central o la región que tiene las conexiones de red más rápidas. Si es difícil para su organización seleccionar un dominio regional para que sea el dominio raíz del bosque, puede usar en su lugar un modelo de raíz de bosque dedicado.

## <a name="assigning-the-forest-root-domain-name"></a>Asignar el nombre de dominio raíz del bosque

El nombre de dominio raíz del bosque es también el nombre del bosque. El nombre de la raíz del bosque es un nombre del sistema de nombres de dominio (DNS) que consta de un prefijo y un sufijo con el formato prefijo. sufijo. Por ejemplo, una organización podría tener el nombre raíz del bosque corp.contoso.com. En este ejemplo, Corp es el prefijo y contoso.com es el sufijo.

Seleccione el sufijo en una lista de nombres existentes en la red. En el prefijo, seleccione un nuevo nombre que no se haya usado previamente en la red. Al adjuntar un nuevo prefijo a un sufijo existente, se crea un espacio de nombres único. La creación de un nuevo espacio de nombres para Active Directory Domain Services (AD DS) garantiza que no es necesario modificar ninguna infraestructura DNS existente para acomodar AD DS.

### <a name="selecting-a-suffix"></a>Seleccionar un sufijo

Para seleccionar un sufijo para el dominio raíz del bosque:

1. Póngase en contacto con el propietario DNS de la organización para obtener una lista de sufijos DNS registrados que se usan en la red que hospedará AD DS. Tenga en cuenta que los sufijos usados en la red interna pueden ser diferentes de los sufijos usados externamente. Por ejemplo, una organización podría usar contosopharma.com en Internet y contoso.com en la red corporativa interna.

2. Consulte al propietario de DNS para seleccionar un sufijo y usarlo con AD DS. Si no existe ningún sufijo adecuado, registre un nuevo nombre con una autoridad de nomenclatura de Internet.

Se recomienda usar nombres DNS registrados con una autoridad de Internet en el espacio de nombres Active Directory. Solo se garantiza que los nombres registrados son únicos globalmente. Si otra organización registra más adelante el mismo nombre de dominio DNS (o si la organización combina con, adquiere o adquiere otra compañía que usa el mismo nombre DNS), las dos infraestructuras no pueden interactuar entre sí.

> [!CAUTION]
> No use nombres DNS de etiqueta única. Para obtener más información, vea ([implementación y funcionamiento de Active Directory dominios que se configuran mediante nombres DNS de etiqueta única](https://go.microsoft.com/fwlink/?LinkId=106631)). Además, no se recomienda usar sufijos no registrados, como. local.

### <a name="selecting-a-prefix"></a>Seleccionar un prefijo

Si elige un sufijo registrado que ya está en uso en la red, seleccione un prefijo para el nombre de dominio raíz del bosque con las reglas de prefijo de la tabla siguiente. Agregue un prefijo que no esté actualmente en uso para crear un nuevo nombre subordinado. Por ejemplo, si el nombre de la raíz DNS es contoso.com, puede crear el nombre de dominio raíz del bosque de Active Directory concorp.contoso.com si el espacio de nombres concorp.contoso.com no está ya en uso en la red. Esta nueva rama del espacio de nombres se dedicará a AD DS y se puede integrar fácilmente con la implementación de DNS existente.

Si seleccionó un dominio regional para que funcione como dominio raíz del bosque, es posible que deba seleccionar un nuevo prefijo para el dominio. Dado que el nombre de dominio raíz del bosque afecta a todos los demás nombres de dominio del bosque, es posible que un nombre basado en regiones no sea adecuado. Si usa un nuevo sufijo que no se está usando actualmente en la red, puede usarlo como nombre de dominio raíz del bosque sin elegir un prefijo adicional.

En la tabla siguiente se enumeran las reglas para seleccionar un prefijo para un nombre DNS registrado.

|Regla|Explicación|
|--------|---------------|
|Seleccione un prefijo que no sea probable que quede obsoleto.|Evite nombres como una línea de productos o un sistema operativo que podrían cambiar en el futuro. Se recomienda usar nombres genéricos como Corp o DS.|
|Seleccione un prefijo que incluya solo caracteres estándar de Internet.|A-Z, a-z, 0-9 y (-), pero no totalmente numérica.|
|Incluya 15 caracteres o menos en el prefijo.|Si elige una longitud de prefijo de 15 caracteres o menos, el nombre NetBIOS es el mismo que el prefijo.|

Es importante que el propietario de DNS Active Directory funcione con el propietario DNS de la organización para obtener la propiedad del nombre que se utilizará para el espacio de nombres Active Directory. Para obtener más información sobre cómo diseñar una infraestructura DNS para admitir AD DS, consulte [creación de un diseño de infraestructura de DNS](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md).

## <a name="documenting-the-forest-root-domain-name"></a>Documentar el nombre de dominio raíz del bosque

Documente el prefijo y el sufijo DNS que seleccione para el dominio raíz del bosque. En este punto, identifique el dominio que será la raíz del bosque. Puede Agregar la información del nombre de dominio raíz del bosque a la hoja de cálculo "planeamiento de dominios" que creó para documentar el plan de dominios nuevos y actualizados y los nombres de dominio. Para abrirlo, descargue Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip de la [ayuda del trabajo para el kit de implementación de Windows Server 2003](https://www.microsoft.com/download/details.aspx?id=9608) y abra "planeamiento de dominios" (DSSLOGI_5. doc).
