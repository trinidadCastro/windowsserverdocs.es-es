---
title: 'Solución de problemas de AD FS: Fiddler'
description: En este documento se describe lo que es Fiddler y cómo instalar y configurar Fiddler para solucionar problemas de notificaciones de AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/02/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: f2abf1a0b844e8e8799458f5237d7a80059880ff
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366149"
---
# <a name="ad-fs-troubleshooting---fiddler"></a>Solución de problemas de AD FS: Fiddler
Fiddler es una herramienta que se puede usar para capturar el tráfico Web HTTP/HTTPS.  Esta herramienta se puede usar para ayudar en la solución de problemas del proceso de emisión de notificaciones.  Al examinar el tráfico, se puede comprender mejor dónde se divide la interacción.  En este documento se describe cómo instalar y configurar Fiddler para capturar AD FS tráfico.  Para obtener un seguimiento de Fiddler de ejemplo con WS-Federation, consulte [AD FS Troubleshooting-Fiddler-WS-Federation](ad-fs-tshoot-fiddler-ws-fed.md) .

## <a name="download-and-install-fiddler"></a>Descarga e instalación de Fiddler
Puede descargar Fiddler [aquí](https://www.telerik.com/download/fiddler).  Una vez descargado, continúe e instálelo.

## <a name="configure-fiddler-to-capture-ad-fs-traffic"></a>Configuración de Fiddler para capturar el tráfico de AD FS
Con el fin de capturar el tráfico AD FS, es necesario configurar Fiddler para descifrar el tráfico SSL. 

### <a name="configure-the-fiddler-ssl-certificate"></a>Configurar el certificado SSL de Fiddler
 Use el procedimiento siguiente para configurar Fiddler para descifrar el tráfico SSL.

1.  Abrir Fiddler
2.  En la parte superior, en **herramientas**, seleccione **Opciones de Fiddler**.
3.  Haga clic en la pestaña HTTPS.
4.  Coloque una marca de verificación en **descifrar el tráfico HTTPS** y seleccione **desde exploradores solo** en la lista desplegable.
5.  Coloque una marca de verificación en **omitir errores de certificado de servidor**.
6.  Haga clic en **Aceptar**.

![Fiddler](media/ad-fs-tshoot-fiddler/fiddler1.png)

## <a name="next-steps"></a>Pasos siguientes

- [Solución de problemas de AD FS](ad-fs-tshoot-overview.md)