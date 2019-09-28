---
title: 'AD FS solución de problemas: puntos de conexión de AD FS'
description: En este documento se describe cómo solucionar problemas de AD FS puntos de conexión
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 807b5c5de14bf6a43419d0b9d2d3a4e6953d0075
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366225"
---
# <a name="ad-fs-troubleshooting---ad-fs-metadata-endpoints"></a>AD FS solución de problemas: puntos de conexión de metadatos AD FS
Los extremos proporcionan acceso a la funcionalidad de servidor de Federación de AD FS, como la publicación de metadatos de Federación.  Para comprobar que el servidor de AD FS responde a las solicitudes Web, podemos comprobar los distintos puntos de conexión.


## <a name="federation-metadata-test"></a>Prueba de metadatos de Federación
La Federación pasiva hace referencia a escenarios en los que el explorador se redirige a la página de inicio de sesión de AD FS.  Al probar el punto de conexión de metadatos, podemos determinar si el servidor de AD FS responde a las solicitudes web en estos escenarios pasivos.  Utilice el siguiente procedimiento para probar el punto de conexión.

1.  Mediante un explorador Web, vaya a la AD FS extremo de metadatos de Federación.  Por ejemplo: https://sts.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml
2. El archivo XML debe descargarse localmente en el equipo.
3. Ábralo y compruebe que contiene información similar a la información siguiente: ![Passive @ no__t-1

## <a name="ws-mex-test-active-test"></a>Prueba WS-MEX (prueba activa)
WS-MetaDataExchange es un protocolo de servicios web y forma parte del mapa de ruta de WS-Federation.  Usa un mensaje SOAP para solicitar los metadatos.  Al probar el punto de conexión, podemos determinar si el servidor de AD FS responde a las solicitudes Web de WS-MetaDataExchange.  Utilice el siguiente procedimiento para probar el punto de conexión.
1.  Mediante un explorador Web, vaya a la AD FS extremo de metadatos de Federación.  Por ejemplo: https://sts.contoso.com/adfs/services/trust/mex
2. El archivo XML se debe mostrar automáticamente en el explorador.  Debe ser similar a la imagen siguiente:

![Activo](media/ad-fs-tshoot-endpoints/meta3.png)


## <a name="next-steps"></a>Pasos siguientes

- [Solución de problemas de AD FS](ad-fs-tshoot-overview.md)