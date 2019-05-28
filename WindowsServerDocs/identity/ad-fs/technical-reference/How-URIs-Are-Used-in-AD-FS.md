---
ms.assetid: 53ee93e2-09ea-4f8b-adb7-c24c59f055ea
title: Uso de URI en AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 677d3136305cbddd29f2fd782be33ae1e824d096
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188551"
---
# <a name="how-uris-are-used-in-ad-fs"></a>Uso de URI en AD FS
Un identificador uniforme de recursos \(URI\) es una cadena de caracteres que se usa como identificador único.  En AD FS, los URI se usan para identificar las direcciones de red de asociados y los objetos de configuración.  Cuando se usa para identificar las direcciones de red de asociados, el URI es siempre una dirección URL.  Cuando se usa para identificar los objetos de configuración, el URI puede ser un nombre de recursos uniforme (URN) o una dirección URL.  Para obtener información general sobre los URI, consulte [RFC 2396](https://go.microsoft.com/fwlink/?LinkId=48289) y [RFC 3986](https://go.microsoft.com/fwlink/?LinkId=90453).  
  
## <a name="uris-as-partner-network-addresses"></a>URI como direcciones de red de asociados  
A continuación se muestran las direcciones URL de red que administran con más frecuencia los administradores de AD FS.  
  
-   Las direcciones URL del servicio de federación, incluidos WS\-Federation, SAML, WS\-confía, los metadatos de federación WS\-MetadataExchange, privacidad y direcciones URL de la organización  
  
-   Las direcciones URL de una entidad de confianza, incluidos WS\-Federation, SAML y direcciones URL de metadatos de federación  
  
-   Las direcciones URL de una confianza de proveedor de notificaciones, incluidos WS\-Federation, SAML y direcciones URL de metadatos de federación  
  
## <a name="uris-as-object-identifiers"></a>URI como identificadores de objetos  
En la tabla siguiente se describen los identificadores que administran con más frecuencia los administradores de AD FS.  
  
|Nombre de identificador|Descripción|Comparaciones|  
|-------------------|---------------|---------------|  
|Identificador del Servicio de federación|Este identificador se usa para identificar el Servicio de federación.  Lo usan los usuarios de confianza que utilizan notificaciones del Servicio de federación y los proveedores de notificaciones que emiten notificaciones al Servicio de federación.|Cuando un usuario solicite notificaciones de un proveedor de notificaciones para este Servicio de federación, el identificador del Servicio de federación se usará para identificar el destino de las notificaciones.<br /><br />Cuando este Servicio de federación reciba las notificaciones de un proveedor de notificaciones, comprobará que las notificaciones estén dentro del ámbito del proveedor buscando su identificador del Servicio de federación.<br /><br />Cuando un usuario de confianza reciba notificaciones de este Servicio de federación, el usuario de confianza comprobará que el emisor de las notificaciones coincida con el identificador del Servicio de federación.|  
|Identificador de usuario de confianza|Este identificador se usa para identificar al usuario de confianza del Servicio de federación.  Se usa al emitir notificaciones al usuario de confianza.|Cuando un usuario solicite notificaciones de este Servicio de federación para el usuario de confianza, el identificador del usuario de confianza se usará para identificar al usuario de confianza para el que deberían estar destinadas las notificaciones.  Esta comparación se realiza mediante la coincidencia de prefijo \(vea más abajo\).<br /><br />Cuando el usuario de confianza reciba las notificaciones, comprobará su identificador en el token de seguridad para asegurarse de que las notificaciones están destinadas a él.|  
|Identificador del proveedor de notificaciones|Este identificador se usa para identificar al proveedor de notificaciones de este Servicio de federación.  Se usa al recibir notificaciones del proveedor de notificaciones.|Cuando este Servicio de federación reciba notificaciones del proveedor de notificaciones, este Servicio de federación comprobará que el emisor de las notificaciones coincida con el identificador del proveedor de notificaciones.|  
|Tipo de notificación|Este identificador se usa para definir el tipo de notificación.  Lo usan este Servicio de federación, los proveedores de notificaciones y los usuarios de confianza al enviar y recibir notificaciones.|Cuando el Servicio de federación recibe notificaciones de un proveedor de notificaciones, las reglas de notificaciones asociadas a la relación de confianza para proveedor de notificaciones correspondiente permiten al administrador comparar tipos de notificaciones y procesar notificaciones.  Las reglas de notificaciones asociadas a una relación de confianza para usuario autenticado también permiten al administrador comparar tipos de notificaciones a partir de las notificaciones que salen de las reglas de relación de confianza para proveedor de notificaciones y decidir qué notificaciones emitirá.|  
  
## <a name="uri-prefix-matching-for-relying-party-identifiers"></a>Coincidencia de prefijos URI para los identificadores de usuario de confianza  
La sintaxis de ruta de acceso de un identificador URI está organizada jerárquicamente y está delimitada por todos los "\/"caracteres o todos":" caracteres.  Por lo tanto, puede que la ruta de acceso se divida en secciones de ruta de acceso según el carácter delimitador.  Cuando la coincidencia de prefijos, cada sección debe ser una coincidencia total según las reglas de coincidencia \(estas normas rigen el uso de mayúsculas y minúsculas de coincidencias\). Para obtener más información sobre las reglas de coincidencia, consulte las RFC mencionadas anteriormente.  
  
Cuando un usuario de confianza se identifica en una solicitud al Servicio de federación, AD FS usa la lógica de coincidencia de prefijos para determinar si hay una relación de confianza para usuario autenticado coincidente en la base de datos de configuración de AD FS.  
  
Por ejemplo, si el identificador del usuario autenticado en la base de datos de configuración de AD FS \(URI1\) es un prefijo para el identificador del usuario autenticado en la solicitud entrante \(URI2\), a continuación, deben cumplirse los siguientes:  
  
-   Los delimitadores finales \(barras diagonales y dos puntos\) de ruta de acceso se deben omitir secciones o entidades  
  
-   Los partes del esquema y de la autoridad de URI1 y URI2 deben ser una coincidencia exacta sin distinción entre mayúsculas y minúsculas.  
  
-   Cada sección de ruta de acceso de URI1 debe ser una coincidencia exacta \(basándose en las mayúsculas y minúsculas elegida\) a la sección de ruta de acceso correspondiente de uri2.  
  
-   Puede que URI2 tenga más secciones de ruta de acceso que URI1, pero URI1 no debe tener más secciones de ruta de acceso que URI2.  
  
-   URI1 no puede tener más secciones de ruta de acceso que URI2.  
  
-   Si URI1 tiene una cadena de consulta, debe coincidir exactamente con una cadena de consulta de URI2.  
  
-   Si URI1 tiene un fragmento, debe coincidir exactamente con un fragmento de URI2.  
  
La tabla siguiente incluye ejemplos adicionales.  
  
|Identificador de usuario de confianza en la base de datos de configuración de AD FS|Identificador de usuario de confianza en el mensaje de las solicitudes|¿El identificador de la solicitud coincide con el identificador de configuración?|Reason|  
|------------------------------------------------------------|-----------------------------------------------|------------------------------------------------------------|----------|  
|http:\/\/contoso.com|http:\/\/contoso.com|TRUE|Coincidencia exacta|  
|http:\/\/contoso.com\/|http:\/\/contoso.com|TRUE|Se ignoran las barras diagonales finales|  
|http:\/\/contoso.com|http:\/\/contoso.com\/|TRUE|Se ignoran las barras diagonales finales|  
|http:\/\/contoso.com|http:\/\/contoso.com\/hr|TRUE|URI1 no tiene ninguna ruta de acceso y coincide con el esquema y la autoridad para URI2|  
|http:\/\/contoso.com\/hr|http:\/\/contoso.com\/hr\/web|TRUE|Las primeras secciones de la ruta de acceso coinciden, URI1 no tiene una segunda sección de ruta de acceso.|  
|http:\/\/contoso.com\/hr|http:\/\/contoso.com\/hr\/web\/? m\=t|TRUE|Las mismas razones que anteriormente, la cadena de consulta no cambia nada.|  
|http:\/\/contoso.com\/hr\/|http:\/\/contoso.com\/hrw\/main|FALSE|La sección 1 de la ruta de acceso de URI1 no coincide con la sección 1 de la ruta de acceso de URI2|  
|http:\/\/contoso.com\/hr|http:\/\/contoso.com|FALSE|URI1 tiene más secciones de ruta de acceso que URI2.|  
|http:\/\/contoso.com\/hr|http:\/\/contoso.com\/hrweb|FALSE|Las primeras secciones de la ruta de acceso no coinciden.|  
|http:\/\/contoso.com\/?m\=t|http:\/\/contoso.com\/?m\=f|FALSE|Las partes de la cadena de consulta no coinciden.|  
|https:\/\/contoso.com|http:\/\/contoso.com|FALSE|Las partes del esquema no coinciden.|  
|http:\/\/sts.contoso.com|http:\/\/contoso.com|FALSE|Las partes de la autoridad no coinciden.|  
|http:\/\/contoso.com|http:\/\/sts.contoso.com|FALSE|Las partes de la autoridad no coinciden.|  
  

