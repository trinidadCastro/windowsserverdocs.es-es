---
ms.assetid: d8adcb68-18e0-41bf-a817-d57344bf2e7d
title: Proxy de aplicación web en Windows Server 2016
description: ''
author: kgremban
manager: femila
ms.date: 07/13/2016
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: web-app-proxy
ms.openlocfilehash: c4e4eb73b7d50c7618ad2c998ee484e660bcfef1
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446766"
---
# <a name="web-application-proxy-in-windows-server-2016"></a>Proxy de aplicación web en Windows Server 2016

>Se aplica a: Windows Server 2016

**Este contenido es relevante para la versión local del Proxy de aplicación Web. Para habilitar el acceso seguro a aplicaciones locales a través de la nube, consulte el [contenido de Azure AD Application Proxy](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/).**  
  
El contenido de esta sección describe las novedades y los cambios en el Proxy de aplicación Web para Windows Server 2016. Las nuevas características y cambios que se muestran aquí son los más probables que tengan el mayor impacto cuando trabaje con la versión preliminar.  
  
## <a name="web-application-proxy-new-features-in-windows-server-2016"></a>Características nuevas de Proxy de aplicación Web en Windows Server 2016
  
- Autenticación previa de publicación de aplicaciones básica HTTP  
  
  HTTP básico es el protocolo de autorización utilizado por muchos protocolos, incluidos ActiveSync, para conectarse a los clientes enriquecidos, incluidos los smartphones, con el buzón de Exchange. Proxy de aplicación Web tradicionalmente interactúa con AD FS mediante redirecciones, que no se admite en los clientes de ActiveSync. Esta nueva versión de Proxy de aplicación Web proporciona compatibilidad para publicar una aplicación mediante HTTP básica al habilitar la aplicación HTTP recibir una no basada en notificaciones de confianza para usuario autenticado para la aplicación al servicio de federación.  
  
  Para obtener más información sobre la publicación básica de HTTP, consulte [publicar aplicaciones utilizando autenticación previa de AD FS](Publishing-Applications-using-AD-FS-Preauthentication.md#publish-an-application-that-uses-http-basic)  
  
- Publicación de dominio comodín de aplicaciones  
  
  Para admitir escenarios como SharePoint 2013, la dirección URL externa de la aplicación ahora puede incluir un carácter comodín para que pueda publicar varias aplicaciones desde dentro de un dominio específico, por ejemplo, https://*.sp-apps.contoso.com. Esto simplificará la publicación de aplicaciones de SharePoint.  
  
- HTTP al redireccionamiento de HTTPS  
  
  Con el fin de asegurarse de que los usuarios pueden acceder a la aplicación, incluso si olvidó escribir la dirección URL HTTPS, el Proxy de aplicación Web ahora admite HTTP al redireccionamiento de HTTPS.  
  
- Publicación HTTP  
  
  Ahora es posible publicar aplicaciones HTTP utilizando autenticación previa de paso a través  
  
- Publicación de aplicaciones de la puerta de enlace de escritorio remoto  
  
  Para obtener más información sobre RDG en Proxy de aplicación Web, consulte [publicar aplicaciones con SharePoint, Exchange y RDG](../web-application-proxy/Publishing-Applications-with-SharePoint,-Exchange-and-RDG.md)  
  
- Nuevo registro de depuración para solucionar mejor el problema y el registro de servicio mejorada para la pista de auditoría completa y control de errores mejorado  
  
  Para obtener más información sobre cómo solucionar problemas, consulte [solución de problemas de Proxy de aplicación Web](https://technet.microsoft.com/library/dn770156.aspx)  
  
- Mejoras de la interfaz de usuario de la consola de administrador  
  
- Propagación de la dirección IP del cliente a las aplicaciones de back-end  
  
## <a name="see-also"></a>Vea también  
  
-   [Novedades en Windows Server 2016](https://technet.microsoft.com/library/dn765472.aspx)  
  
-   [Publicación de aplicaciones con autenticación previa de AD FS](../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md)  
  
-   [Solución de problemas del Proxy de aplicación web](https://technet.microsoft.com/library/dn770156.aspx)  
  


