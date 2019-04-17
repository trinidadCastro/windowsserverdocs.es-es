---
ms.assetid: 53ee93e2-09ea-4f8b-adb7-c24c59f055ea
title: "¿Cómo se usan los URI en AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 305bf0cece742c961604dacda7e27b8eac8065e5
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
>Se aplica a: Windows Server 2016, Windows Server 2012 R2

# <a name="how-uris-are-used-in-ad-fs"></a>¿Cómo se usan los URI en AD FS
Un identificador uniforme de recursos \(URI\) es una cadena de caracteres que se usa como un identificador único.  En AD FS, los URI se usan para identificar las direcciones de red de partners y objetos de configuración.  Cuando se usa para identificar las direcciones de red de partners, el URI es siempre una dirección URL.  Cuando se usa para identificar los objetos de configuración, el URI puede ser un URN o una dirección URL.  Para obtener más información general acerca de los URI, consulta [RFC 2396](https://go.microsoft.com/fwlink/?LinkId=48289) y [RFC 3986](https://go.microsoft.com/fwlink/?LinkId=90453).  
  
## <a name="uris-as-partner-network-addresses"></a>URI como direcciones de red de partners  
Las siguientes son las direcciones URL de direcciones de red que normalmente se controlan mediante los administradores en AD FS.  
  
-   Las direcciones URL de los servicios de federación, incluidos WS\ federación, SAML, WS\ confianza, metadatos de federación, WS\ MetadataExchange, privacidad y direcciones URL de la organización  
  
-   Las direcciones URL de usuario de confianza una confianza de terceros, incluidos WS\ federación, SAML y direcciones URL de metadatos de federación  
  
-   Las direcciones URL de una relación de confianza de proveedor de reclamaciones, incluidas WS\ federación, SAML y direcciones URL de metadatos de federación  
  
## <a name="uris-as-object-identifiers"></a>URI como identificadores de objeto  
La siguiente tabla describe los identificadores que normalmente se controlan mediante los administradores en AD FS.  
  
|Nombre del identificador|Descripción|Comparaciones|  
|-------------------|---------------|---------------|  
|Identificador de servicios de federación|Este identificador se utiliza para identificar los servicios de federación.  Usuarios de confianza que usan notificaciones de esta servicios de federación, así como proveedores de notificaciones que dice servicios de federación de este problema se usa.|Cuando un usuario solicita reclamaciones de un proveedor de notificaciones para este servicio de federación, el identificador de servicios de federación se usará para identificar el destino para las notificaciones.<br /><br />Cuando este servicio de federación recibe las notificaciones de un proveedor de notificaciones, comprueba para garantizar que las notificaciones se aplican para ella mediante la búsqueda de su identificador de servicios de federación.<br /><br />Cuando un usuario de confianza está recibiendo notificaciones de servicios de federación de este, el usuario de confianza se comprueba que el emisor de las notificaciones coincide con el identificador de servicios de federación.|  
|Identificador de terceros de confianza|Este identificador se utiliza para identificar el usuario de confianza a los servicios de federación de este.  Se usa cuando se generen notificaciones para el usuario de confianza.|Cuando un usuario solicita reclamaciones de servicios de federación de este para el usuario de confianza, el identificador de terceros de confianza se usará para identificar el usuario de confianza para los que deberían estar dirigidas las notificaciones.  Esta comparación se realiza usando prefijo \(see below\) coincidente.<br /><br />Cuando el usuario de confianza recibe las notificaciones, buscará su identificador en el token de seguridad para garantizar que las notificaciones están destinadas a él.|  
|Identificador de proveedor de notificaciones|Este identificador se utiliza para identificar el proveedor de notificaciones para los servicios de federación de este.  Se usa al recibir notificaciones desde el proveedor de notificaciones.|Cuando este servicio de federación está recibiendo notificaciones desde el proveedor de notificaciones, este servicio de federación comprueba que el emisor de las notificaciones coincide con el identificador de proveedor de notificaciones.|  
|Tipo de notificación|Este identificador se usa para definir el tipo de notificación.  Se usa este servicios de federación, proveedores de notificaciones y usuarios de confianza al enviar y recibir notificaciones.|Cuando el servicio de federación recibe notificaciones de un proveedor de notificaciones, las reglas de notificación asociadas con la confianza de proveedor correspondiente reclamaciones permiten al administrador comparar los tipos de notificación y procesar las reclamaciones.  Las reglas de notificación asociadas con una usuario de confianza confianza de terceros también permiten que el administrador comparar los tipos de notificación de las notificaciones proveniente de las reglas de confianza del proveedor de notificaciones y decide que dice emitir.|  
  
## <a name="uri-prefix-matching-for-relying-party-identifiers"></a>La coincidencia con identificadores de terceros de confianza de prefijo URI  
La sintaxis de ruta de acceso de un URI está organizada jerárquicamente y está delimitada por todo "\ /"caracteres o todos":" caracteres.  Por lo tanto, la ruta de acceso se puede dividir en secciones de la ruta de acceso en función de carácter delimitador.  Cuando prefijo coincidentes, cada sección debe ser una coincidencia completa según las reglas de coincidencia \ (estas reglas rigen la carcasa de matches\). Para obtener más información acerca de las coincidencias de reglas, consulta la RFC mencionado anteriormente.  
  
Cuando un usuario de confianza se identifica en una solicitud al servicio de federación, AD FS usa lógica de coincidencia de prefijos para determinar si hay una confianza de terceros de confianza correspondiente en la base de datos de configuración de AD FS.  
  
Por ejemplo, si el identificador de terceros de confianza en la base de datos de configuración de AD FS \(URI1\) es un prefijo para el identificador de terceros de confianza en la entrante solicitar \(URI2\), a continuación, deben cumplirse las siguientes:  
  
-   Finales delimitadores \(slashes and colons\) de ruta de acceso se deben omitir secciones o las autoridades  
  
-   Las partes de esquema y autoridad de URI1 y URI2 deben ser una coincidencia exacta mayúsculas de minúsculas  
  
-   Cada sección de la ruta de acceso de URI1 debe ser una coincidencia exacta \ (según la chosen\ entre mayúsculas y minúsculas) a la sección correspondiente de la ruta de acceso de URI2  
  
-   URI2 puede tener más secciones de ruta de acceso que URI1, pero URI1 no debe tener más secciones de ruta de acceso que URI2  
  
-   URI1 no puede tener más secciones de ruta de acceso que URI2  
  
-   Si URI1 tiene una cadena de consulta, debe coincidir exactamente con una cadena de consulta URI2  
  
-   Si URI1 tiene un fragmento, debe coincidir con exactamente a un fragmento de URI2  
  
La siguiente tabla proporciona ejemplos adicionales.  
  
|Usar el identificador de terceros en la base de datos de configuración de AD FS|Usar el identificador de terceros en el mensaje de solicitud|¿Solicitar el identificador de la configuración de coincidencias de identificador?|Motivo|  
|------------------------------------------------------------|-----------------------------------------------|------------------------------------------------------------|----------|  
|http:///\/contoso.com|http:///\/contoso.com|TRUE|Coincidencia exacta|  
|http:///\/contoso.com\/|http:///\/contoso.com|TRUE|Se omiten barras diagonales finales|  
|http:///\/contoso.com|http:///\/contoso.com\/|TRUE|Se omiten barras diagonales finales|  
|http:///\/contoso.com|http:///\/contoso.com\/hr|TRUE|URI1 no tiene el esquema de ruta de acceso y coincidencias y autoridad para URI2|  
|http:///\/contoso.com\/hr|http:///\/contoso.com\/hr\/Web|TRUE|Coincidencia de primera secciones de la ruta de acceso, URI1 no tiene segunda ruta de acceso sección|  
|http:///\/contoso.com\/hr|http:///\/contoso.com\/hr\/Web\/?m\=t|TRUE|Mismos motivos, como se indicó anteriormente, cadena de consulta no modifica nada|  
|http:///\/contoso.com\/hr\/|http:///\/contoso.com\/hrw\/Main|"FALSE"|Ruta de acceso de URI1 sección 1 no coincide con la sección de ruta de acceso de URI2 1|  
|http:///\/contoso.com\/hr|http:///\/contoso.com|"FALSE"|URI1 tiene más secciones de ruta de acceso que URI2|  
|http:///\/contoso.com\/hr|http:///\/contoso.com\/hrweb|"FALSE"|Primeras secciones de la ruta de acceso no coinciden|  
|http:///\/contoso.com\/?m\=t|http:///\/contoso.com\/?m\=f|"FALSE"|Partes de la cadena de consulta no coinciden|  
|https:\/\/contoso.com|http:///\/contoso.com|"FALSE"|No coinciden con los elementos de esquema|  
|http:///\/STS.contoso.com|http:///\/contoso.com|"FALSE"|Partes de la entidad no coinciden|  
|http:///\/contoso.com|http:///\/STS.contoso.com|"FALSE"|Partes de la entidad no coinciden|  
  

