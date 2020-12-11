---
description: 'Más información acerca de: proxy de aplicación web en Windows Server'
ms.assetid: 0b3587b2-219f-43d8-88b4-1254eaa8b910
title: Proxy de aplicación web en Windows Server 2016
ms.topic: article
ms.author: kgremban
author: eross-msft
ms.openlocfilehash: c95f47d057008249ca2ebc852e97b68f90d604e8
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97046473"
---
# <a name="web-application-proxy-in-windows-server"></a>Proxy de aplicación web en Windows Server 2016

>Se aplica a: Windows Server&reg; 2016

**Este contenido es relevante para la versión local del proxy de aplicación Web. Para habilitar el acceso seguro a aplicaciones locales a través de la nube, consulte el [contenido del proxy de aplicación Azure ad](/azure/active-directory/manage-apps/application-proxy).**

El contenido de esta sección describe las novedades y los cambios en el proxy de aplicación web para Windows Server 2016. Las nuevas características y los cambios que se muestran aquí son los que probablemente tengan mayor impacto al trabajar con la versión preliminar.

## <a name="web-application-proxy-new-features"></a>Nuevas características del proxy de aplicación Web

- Autenticación previa para la publicación de aplicaciones básicas HTTP

  HTTP Basic es el protocolo de autorización usado por muchos protocolos, incluido ActiveSync, para conectar clientes enriquecidos, incluidos smartphones, con el buzón de Exchange. El proxy de aplicación Web suele interactuar con AD FS mediante redireccionamientos que no se admiten en los clientes de ActiveSync. Esta nueva versión del proxy de aplicación web proporciona compatibilidad para publicar una aplicación mediante HTTP básico habilitando la aplicación HTTP para que reciba una relación de confianza para usuario autenticado no notificaciones para la aplicación en el Servicio de federación.

  Para obtener más información sobre la publicación HTTP Basic, consulte [publicación de aplicaciones con la autenticación previa de AD FS](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md) .

- Publicación de aplicaciones con un dominio comodín

  Para admitir escenarios como SharePoint 2013, la dirección URL externa de la aplicación ahora puede incluir un carácter comodín que le permita publicar varias aplicaciones desde dentro de un dominio concreto, por ejemplo, https://*. SP-apps. contoso. com. Esto simplificará la publicación de aplicaciones de SharePoint.

- Redireccionamiento de HTTP a HTTPS

  Para asegurarse de que los usuarios puedan tener acceso a la aplicación, aunque no tengan que escribir HTTPS en la dirección URL, el proxy de aplicación web ahora admite la redirección de HTTP a HTTPS.

- Publicación HTTP

  Ahora es posible publicar aplicaciones HTTP mediante la autenticación previa de paso a través.

- Publicación de aplicaciones de puerta de enlace de Escritorio remoto

  Para más información sobre RDG en el proxy de aplicación Web, consulte [publicación de aplicaciones con SharePoint, Exchange y RDG](../web-application-proxy/Publishing-Applications-with-SharePoint,-Exchange-and-RDG.md)

- Nuevo registro de depuración para una mejor solución de problemas y un registro de servicio mejorado para la pista de auditoría completa y el control de errores mejorado

  Para obtener más información sobre la solución de problemas, consulte [solución de problemas de proxy de aplicación web](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn770156(v=ws.11))

- Mejoras de la interfaz de usuario de Consola de administrador

- Propagación de la dirección IP del cliente a las aplicaciones de back-end

## <a name="see-also"></a>Consulte también

-   [Novedades en Windows Server 2016](../../../get-started/whats-new-in-windows-server-2016.md)

-   [Publicación de aplicaciones con autenticación previa de AD FS](../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md)

-   [Solución de problemas del Proxy de aplicación web](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn770156(v=ws.11))

