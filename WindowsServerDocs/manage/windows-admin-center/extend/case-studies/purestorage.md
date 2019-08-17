---
title: 'Caso práctico del SDK del centro de administración de Windows: almacenamiento puro'
description: 'Caso práctico del SDK del centro de administración de Windows: almacenamiento puro'
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 1/7/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: cfd9d31d388b9acb1a4a4fa40b3975b235a8634b
ms.sourcegitcommit: 0467b8e69de66e3184a42440dd55cccca584ba95
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2019
ms.locfileid: "69546619"
---
# <a name="pure-storage-extension"></a>Extensión de almacenamiento puro

## <a name="providing-end-to-end-array-management-for-windows-admin-center"></a>Proporcionar administración de matrices de un extremo a otro para el centro de administración de Windows 

El [almacenamiento puro](https://www.purestorage.com/) proporciona soluciones de almacenamiento de datos empresariales, todo el flash que proporcionan arquitectura centrada en los datos para acelerar su negocio con el fin de obtener una ventaja competitiva.  Pura es un asociado de Microsoft Gold, certificado para Microsoft Windows Server, y desarrolla integraciones técnicas para soluciones clave de Microsoft como Azure, Hyper-V, SQL Server, System Center, Windows PowerShell y Windows SMB. Puro anunció recientemente una versión preliminar técnica de una extensión que admite la versión más reciente del centro de administración de Windows, que proporciona una vista de un solo panel en productos FlashArray puros.  A partir de esta extensión, se permite a los usuarios realizar tareas de supervisión, ver las métricas de rendimiento en tiempo real y administrar los volúmenes de almacenamiento y los iniciadores.

En un primer momento, cuando el centro de administración de Windows se conocía como "proyecto Honolulu", vio puro el valor de poder ofrecer a los clientes y asociados la capacidad de administrar varios FlashArrays de almacenamiento puros desde el único panel de vidrio que proporciona el centro de administración de Windows.

Cuando se empezó a investigar el caso de uso con "Project Honolulu", se dio cuenta de inmediato el potencial para proporcionar una experiencia de administración unificada entre el centro de administración de Windows y FlashArray. En estrecha colaboración con el equipo de ingeniería del centro de administración de Windows, que ayudó a definir los detalles de implementación de las características. Pure también puede proporcionar comentarios en las primeras fases del centro de administración de Windows y realizar contribuciones al equipo de Microsoft. 

![Extensión de almacenamiento puro](../../media/extend-case-study-purestorage/purestorage-1.png)

> <cite>"Hemos integrado un conjunto de características que imita nuestra interfaz Web de FlashArray para habilitar la administración directa desde el centro de administración de Windows. Nuestros clientes y asociados se beneficiarán de un solo panel de vidrio, en lugar de tener que trabajar con dos herramientas de administración diferentes. Además de las ventajas de la administración, los clientes podrán administrar contextualmente los servidores de Windows que están conectados a FlashArray.</cite>
>
> --Barkz, director técnico de soluciones de Microsoft & integración, almacenamiento puro

Las características que se incluyen en la extensión de la solución de almacenamiento pura incluyen:
- Conectarse a varios FlashArrays.
- Ver los detalles de FlashArray, como IOPs, ancho de banda, latencia, reducción de datos y administración del espacio. Estos son los mismos detalles que obtiene de la GUI de administración de FlashArray.
- Ver los grupos host configurados que se usan para habilitar el acceso a volumen compartido para los hosts de Windows Server y los volúmenes compartidos en clúster (CSV).
- Ver hosts: toda la información de conectividad está disponible, incluidos los nombres de host, el nombre completo iSCSI (iqn) y los nombres World Wide Name (WWNs).
- Administrar volúmenes: Esto incluye la capacidad de crear y destruir volúmenes. Una vez que se destruye un volumen, se colocará en el cubo de elementos destruidos y tendrá que erradicarlo de la GUI de administración de FlashArray principal.
- Administrar iniciadores: se trata de una de las características más interesantes que proporcionan contexto a los servidores individuales administrados por la implementación del centro de administración de Windows. Puede ver los discos conectados (volúmenes) en servidores de Windows individuales, comprobar si se ha instalado o configurado múltiples rutas de e/s (MPIO) y crear o montar nuevos volúmenes.

Se ha creado un [vídeo de demostración](https://youtu.be/IFAeCAd6V2g) en el que se muestran todas las características que proporciona la extensión de solución de almacenamiento pura. 

En la captura de pantalla siguiente se muestra cómo ver qué discos (volúmenes) están conectados a un host de Windows Server específico. Además de ver los detalles de conectividad, se comprueba si se ha configurado la e/s de múltiples rutas.

![Extensión de almacenamiento puro](../../media/extend-case-study-purestorage/purestorage-2.png)

Además de ver los discos, se pueden crear volúmenes nuevos e montarlos inmediatamente en el host sin tener que usar la herramienta Administración de discos de Windows.

![Extensión de almacenamiento puro](../../media/extend-case-study-purestorage/purestorage-3.png)

Desde la publicación de la versión Technical Preview, los comentarios de los clientes recopilados hasta ahora han sido muy positivos y también nos ha proporcionado información sobre las diferentes características que se pueden agregar en futuras versiones. 

Recursos adicionales:
- [Entrada de blog del anuncio de extensión de almacenamiento puro](https://blog.purestorage.com/tech-preview-of-the-pure-storage-extension-for-windows-admin-center/)
- Podcast de [PureReport](https://itunes.apple.com/podcast/windows-admin-center-extension-from-pure-storage/id1392639991?i=1000424316130&mt=2)
