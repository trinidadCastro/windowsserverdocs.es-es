---
title: Caso práctico de Windows Admin Center SDK - almacenamiento puro
description: Caso práctico de Windows Admin Center SDK - almacenamiento puro
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 1/7/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 25018474fd22d05804ecc7faafbd633fbb4db269
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849926"
---
# <a name="pure-storage-extension"></a>Extensión de almacenamiento puro

## <a name="providing-end-to-end-array-management-for-windows-admin-center"></a>Proporcionar administración de la matriz to-End para Windows Admin Center 

[Almacenamiento puro](https://www.purestorage.com/) proporciona enterprise, soluciones de almacenamiento de datos de memoria flash que entregar arquitectura centradas en datos para acelerar su negocio para una ventaja competitiva.  Puro es un Microsoft Gold Partner para Microsoft Windows Server y desarrolla integraciones técnicas clave soluciones de Microsoft, como Azure, Hyper-V, SQL Server, System Center, Windows PowerShell y SMB de Windows. Puro anunció recientemente una vista previa técnica de una extensión que admiten la versión más reciente de Windows Admin Center que proporciona una vista de panel único en productos FlashArray puro.  De esta extensión, los usuarios estarán equipados desde una herramienta para llevar a cabo tareas de supervisión, ver las métricas de rendimiento en tiempo real y administrar los iniciadores y los volúmenes de almacenamiento.

Al principio en adelante, cuando Windows Admin Center se conocía como "Proyecto Honolulu", puro vio el valor de poder proporcionar a los clientes y asociados de la capacidad de administrar varios FlashArrays pura de almacenamiento desde el panel único que proporciona Windows Admin Center.

Cuando puro inicia investiga el caso de uso con "Proyecto Honolulu" obtienen inmediatamente la posibilidad de proporcionar una experiencia de administración unificada entre Windows Admin Center y FlashArray. Puro en estrecha colaboración con el equipo de ingeniería de Windows Admin Center, que ayudó a definir los detalles de implementación de las características. También pudo puro proporcionar comentarios en las primeras etapas de Windows Admin Center y realizar contribuciones al equipo de Microsoft. 

![Extensión de almacenamiento puro](../../media/extend-case-study-purestorage/purestorage-1.png)

> <cite>"Hemos integrado un conjunto de características que se asemeje a nuestra interfaz web de FlashArray para habilitar la administración directa desde dentro de Windows Admin Center. Nuestros clientes y socios se beneficiarán de un solo panel de vidrio en comparación con la necesidad de trabajar con dos herramientas de administración diferente. Además el único punto de administración de beneficios que los clientes podrán realizar contextualmente administrar servidores de Windows que están conectados a la FlashArray."</cite>
>
> --Barkz, Director técnico de Microsoft Solutions & Integration, almacenamiento puro

Las características que se incluyen en la extensión de la solución de almacenamiento puro incluyen:
- Conectarse a varios FlashArrays.
- Ver los detalles de FlashArray, incluida la IOPs, ancho de banda, latencia, reducción de datos y administración del espacio. Estos son todos los mismos detalles que obtendrá de la GUI de administración FlashArray.
- Ver grupos host configurados que se usan para habilitar el acceso al volumen compartido para hosts de Windows Server y volúmenes compartidos en clúster (CSV).
- La vista Hosts, Toda la información de conectividad está disponible como nombres de Host iSCSI (iqn) del nombre completo y los nombres World Wide Name (WWN).
- Administrar volúmenes, Esto incluye la capacidad de crear y destruir volúmenes. Una vez que se destruye un volumen se colocará en el depósito de elementos destruido y deberá Eradicate desde la GUI de administración principal FlashArray.
- Administrar los iniciadores, Esta es una de las características más interesantes que proporciona contexto a los servidores individuales que se está administrando mediante la implementación de Windows Admin Center. Puede ver los discos conectados (volúmenes) con los servidores individuales de Windows, compruebe si la E/S de múltiples rutas (MPIO) está instalado o no está configurado y montaje o crear nuevos volúmenes.

Un [vídeo de demostración](https://youtu.be/IFAeCAd6V2g) creado que muestra todas las características que proporciona la extensión de la solución de almacenamiento puro. 

La siguiente captura de pantalla muestra ver qué discos (volúmenes) conectados a un host específico de Windows Server. Además de ver los detalles de conectividad, comprobamos si se configura la E/S de múltiples rutas.

![Extensión de almacenamiento puro](../../media/extend-case-study-purestorage/purestorage-2.png)

Además de ver los discos, se pueden crear nuevos volúmenes y montar inmediatamente en el host sin tener que usar la herramienta de administración de discos de Windows.

![Extensión de almacenamiento puro](../../media/extend-case-study-purestorage/purestorage-3.png)

Puesto que liberar nuestro Technical Preview, los comentarios del cliente recopilados hasta ahora ha sido muy positivo y también nos ha proporcionado información detallada sobre las diferentes características para agregar en futuras versiones. 

Recursos adicionales:
- [Entrada de blog de anuncio de extensión de almacenamiento puro](https://blog.purestorage.com/tech-preview-of-the-pure-storage-extension-for-windows-admin-center/)
- [PureReport](https://itunes.apple.com/us/podcast/windows-admin-center-extension-from-pure-storage/id1392639991?i=1000424316130&mt=2) podcast
