---
title: 'AD FS solución de problemas: extremos de AD FS'
description: Este documento describe cómo solucionar problemas de los extremos de AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 13b830c0317341280bd87499e3abd8dcd1a33fc2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857566"
---
# <a name="ad-fs-troubleshooting---ad-fs-metadata-endpoints"></a>AD FS solución de problemas: puntos de conexión de metadatos de AD FS
Los puntos de conexión proporcionan acceso a la funcionalidad del servidor de federación de AD FS, como la publicación de metadatos de federación.  Para comprobar que el servidor de AD FS está respondiendo a solicitudes web, podemos comprobar los diferentes extremos.


## <a name="federation-metadata-test"></a>Prueba de metadatos de federación
Federación pasiva se refiere a escenarios donde el explorador se redirige a la página de inicio de sesión de AD FS.  Probando el extremo de metadatos podemos determinar si el servidor de AD FS está respondiendo a solicitudes web en estos escenarios pasivos.  Utilice el siguiente procedimiento para probar el punto de conexión.

1.  Mediante un explorador web, navegue hasta el punto de conexión de metadatos de federación de AD FS.  Por ejemplo:  https://sts.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml
2. El archivo xml debe descargar localmente en su equipo.
3. Ábralo y compruebe que contiene información similar a la siguiente información: ![Pasivo](media/ad-fs-tshoot-endpoints/meta2.png)

## <a name="ws-mex-test-active-test"></a>Prueba de WS-MEX (prueba activa)
WS-MetaDataExchange es un protocolo de servicios web y forma parte de la hoja de ruta de WS-Federation.  Un mensaje SOAP utiliza para solicitar los metadatos.  Probando el punto de conexión podemos determinar si el servidor de AD FS está respondiendo a las solicitudes web para WS-MetaDataExchange.  Utilice el siguiente procedimiento para probar el punto de conexión.
1.  Mediante un explorador web, navegue hasta el punto de conexión de metadatos de federación de AD FS.  Por ejemplo:  https://sts.contoso.com/adfs/services/trust/mex
2. El archivo xml debe mostrarse en el explorador automáticamente.  Debe parecerse a la imagen siguiente:

![Activo](media/ad-fs-tshoot-endpoints/meta3.png)


## <a name="next-steps"></a>Pasos siguientes

- [Solución de problemas de AD FS](ad-fs-tshoot-overview.md)