---
title: 'AD FS solución de problemas: Fiddler'
description: Este documento describe qué es Fiddler y cómo instalar y configurar Fiddler para solucionar problemas de notificaciones de AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/02/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 822300d0e4b6ae462a3c942e22530bbed5f93e86
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865636"
---
# <a name="ad-fs-troubleshooting---fiddler"></a>AD FS solución de problemas: Fiddler
Fiddler es una herramienta que puede usarse para capturar el tráfico web HTTP/HTTPS.  Esta herramienta puede usarse para ayudar a solucionar problemas del proceso de emisión de notificación.  Examinando el tráfico podemos obtener una mejor comprensión de donde se interrumpe la interacción.  Este documento describe cómo instalar y configurar Fiddler para capturar el tráfico de AD FS.  Para un seguimiento de fiddler de ejemplo con WS-Federation consulte [WS-Federation de AD FS solución de problemas - Fiddler:](ad-fs-tshoot-fiddler-ws-fed.md)

## <a name="download-and-install-fiddler"></a>Descargue e instale Fiddler
Puede descargar Fiddler [aquí](https://www.telerik.com/download/fiddler).  Una vez que haya descargado continúe e instálelo.

## <a name="configure-fiddler-to-capture-ad-fs-traffic"></a>Configure Fiddler para capturar el tráfico de AD FS
Para capturar el tráfico de AD FS, es necesario configurar Fiddler para descifrar el tráfico SSL. 

### <a name="configure-the-fiddler-ssl-certificate"></a>Configuración del certificado SSL de Fiddler
 Use el procedimiento siguiente para configurar Fiddler para descifrar el tráfico SSL.

1.  Abra Fiddler
2.  En la parte superior, en **herramientas**, seleccione **opciones de Fiddler**.
3.  Haga clic en la pestaña HTTPS.
4.  Coloque una marca de verificación **tráfico HTTPS descifrar** y seleccione **desde exploradores solo** en la lista desplegable.
5.  Coloque una marca de verificación **omitir los errores de certificado de servidor**.
6.  Haga clic en **Aceptar**.

![Fiddler](media/ad-fs-tshoot-fiddler/fiddler1.png)

## <a name="next-steps"></a>Pasos siguientes

- [Solución de problemas de AD FS](ad-fs-tshoot-overview.md)