---
title: 'Solución de problemas de AD FS: Fiddler'
description: En este documento se describe lo que es Fiddler y cómo instalar y configurar Fiddler para solucionar problemas de notificaciones de AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/02/2018
ms.topic: article
ms.openlocfilehash: e631d9b1dd93b9f3eb3d224fa5e2007e3661e21c
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87972032"
---
# <a name="ad-fs-troubleshooting---fiddler"></a>Solución de problemas de AD FS: Fiddler
Fiddler es una herramienta que se puede usar para capturar el tráfico Web HTTP/HTTPS.  Esta herramienta se puede usar para ayudar en la solución de problemas del proceso de emisión de notificaciones.  Al examinar el tráfico, se puede comprender mejor dónde se divide la interacción.  En este documento se describe cómo instalar y configurar Fiddler para capturar AD FS tráfico.  Para obtener un seguimiento de Fiddler de ejemplo con WS-Federation, consulte [AD FS Troubleshooting-Fiddler-WS-Federation](ad-fs-tshoot-fiddler-ws-fed.md) .

## <a name="download-and-install-fiddler"></a>Descarga e instalación de Fiddler
Puede descargar Fiddler [aquí](https://www.telerik.com/download/fiddler).  Una vez descargado, continúe e instálelo.

## <a name="configure-fiddler-to-capture-ad-fs-traffic"></a>Configuración de Fiddler para capturar el tráfico de AD FS
Con el fin de capturar el tráfico AD FS, es necesario configurar Fiddler para descifrar el tráfico SSL.

### <a name="configure-the-fiddler-ssl-certificate"></a>Configurar el certificado SSL de Fiddler
 Use el procedimiento siguiente para configurar Fiddler para descifrar el tráfico SSL.

1.  Abra Fiddler
2.  En la parte superior, en **herramientas**, seleccione **Opciones de Fiddler**.
3.  Haga clic en la pestaña HTTPS.
4.  Coloque una marca de verificación en **descifrar el tráfico HTTPS** y seleccione **desde exploradores solo** en la lista desplegable.
5.  Coloque una marca de verificación en **omitir errores de certificado de servidor**.
6.  Haga clic en **Aceptar**.

![Fiddler](media/ad-fs-tshoot-fiddler/fiddler1.png)

## <a name="next-steps"></a>Pasos a seguir

- [Solución de problemas de AD FS](ad-fs-tshoot-overview.md)