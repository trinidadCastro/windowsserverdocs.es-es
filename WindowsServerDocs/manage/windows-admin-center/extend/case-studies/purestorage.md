---
title: Caso práctico de Windows Admin Center SDK - almacenamiento pura
description: Caso práctico de Windows Admin Center SDK - almacenamiento pura
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 1/7/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 25018474fd22d05804ecc7faafbd633fbb4db269
ms.sourcegitcommit: ebeec824f802f020d0ece17524ba43b1baeba893
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2019
ms.locfileid: "8995371"
---
# Extensión de almacenamiento pura

## Proporcionar la administración de la matriz de principio a fin para Windows Admin Center 

[Almacenamiento pura](https://www.purestorage.com/) proporciona de empresa, soluciones de almacenamiento de datos de memoria flash que ofrecen arquitectura centradas en datos para acelerar el negocio para una ventaja competitiva.  Pura es un Partner Gold de Microsoft, certificados para Microsoft Windows Server y desarrolla aplicaciones integradas técnicas para las principales soluciones de Microsoft como Azure, Hyper-V, SQL Server, System Center, Windows PowerShell y SMB de Windows. Pura anunció recientemente una vista previa técnica de una extensión compatible con la versión más reciente de Windows Admin Center que proporciona una vista de panel único en los productos de FlashArray pura.  De esta extensión, los usuarios están habilitados de una herramienta para llevar a cabo tareas de supervisión, ver las métricas de rendimiento en tiempo real y administrar los iniciadores y los volúmenes de almacenamiento.

Desde el principio, cuando Windows Admin Center se conocía como "Proyecto Honolulu", pura ha visto el valor de ser capaz de proporcionar a los clientes y partners de la capacidad para administrar varios FlashArrays pura de almacenamiento desde el panel único que proporciona Windows Admin Center.

Cuando pura empieza a investigar el caso de uso de "Proyecto Honolulu" observaron inmediatamente la posibilidad de proporcionar una experiencia de administración unificada entre Windows Admin Center y FlashArray. Pura estrechamente colabora con el equipo de ingeniería de Windows Admin Center, lo que ayudó a definir los detalles de implementación para las funciones. También pudo pura proporcionar comentarios en las primeras fases de Windows Admin Center y tomar las contribuciones al equipo de Microsoft. 

![Extensión de almacenamiento pura](../../media/extend-case-study-purestorage/purestorage-1.png)

> <cite>"Hemos integrado un conjunto de características que se asemeja a nuestra interfaz de web FlashArray para habilitar la administración directa desde dentro de Windows Admin Center. Nuestros clientes y partners se beneficiarán de un panel único en comparación con la necesidad de trabajar con dos diferentes herramientas de administración. Además del punto único de administración beneficia a los clientes podrán sea administrar servidores de Windows que están conectados a la FlashArray."</cite>
>
> --Barkz, Director técnico de soluciones de Microsoft e integración, almacenamiento pura

Las características que se incluyen en la extensión de la solución de almacenamiento pura incluyen:
- Conexión a varios FlashArrays.
- Ver los detalles de FlashArray, incluida la e/s por segundo, ancho de banda, latencia, reducción de datos y la administración del espacio. Estos son todos los mismos detalles que obtienes desde la GUI de administración de FlashArray.
- Ver grupos de host configurado que se usan para habilitar el acceso de volumen compartido para hosts de Windows Server y volúmenes compartidos clúster (CSV).
- Hosts de vista: Toda la información de conectividad está disponible como nombres de Host, iSCSI nombre completo (IQNs) y World Wide Names (WWNs).
- Administrar volúmenes, Esto incluye la capacidad de crear y destruir volúmenes. Una vez que un volumen se destruye, se colocará en el período de elementos destruido y tendrás que Eradicate desde la GUI de administración de FlashArray principal.
- Administrar los iniciadores: Esta es una de las características más interesantes proporcionar contexto a los servidores individuales administrados por la implementación de Windows Admin Center. Puedes ver los discos conectados (volúmenes) a los servidores de Windows individuales, comprueba si E/S de múltiples rutas (MPIO) está instalado y configurado y montaje o crear nuevos volúmenes.

Se ha creado un [vídeo de demostración](https://youtu.be/IFAeCAd6V2g) que muestra todas las características que proporciona la extensión de la solución de almacenamiento pura. 

La siguiente captura de pantalla ilustra ver qué discos (volúmenes) están conectados a un host específico de Windows Server. Además de ver los detalles de conectividad, comprobamos si se ha configurado la E/S de múltiples rutas.

![Extensión de almacenamiento pura](../../media/extend-case-study-purestorage/purestorage-2.png)

Además de ver los discos, los nuevos volúmenes pueden crearse y montados inmediatamente a la host sin tener que usar la herramienta de administración de discos de Windows.

![Extensión de almacenamiento pura](../../media/extend-case-study-purestorage/purestorage-3.png)

Dado que liberar nuestro Technical Preview, los comentarios de los clientes recopilados hasta ahora ha sido muy positiva y también nos ha proporcionado información sobre las diferentes características agregar en futuras versiones. 

Recursos adicionales:
- [Entrada de blog de anuncio de extensión de almacenamiento pura](https://blog.purestorage.com/tech-preview-of-the-pure-storage-extension-for-windows-admin-center/)
- [PureReport](https://itunes.apple.com/us/podcast/windows-admin-center-extension-from-pure-storage/id1392639991?i=1000424316130&mt=2) podcast
