---
title: 'Notas de la versión: problemas importantes en Windows Server 2019'
description: Se resumen problemas críticos que requieren soluciones alternativas para evitar bloqueos, dependientes, pérdida de datos y de error de instalación
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
ms.assetid: 134aab85-664f-4d55-87ef-9e5fd098371f
author: jasongerend
ms.author: jgerend
manager: jasgroce
ms.localizationpriority: medium
ms.date: 06/07/2019
ms.openlocfilehash: 515255c301d343aa1b83bcfb506f2e3baa6ca969
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810738"
---
# <a name="release-notes---important-issues-in-windows-server-2019"></a>Notas de la versión: problemas importantes en Windows Server 2019

>Se aplica a: Windows Server 2019

Estas notas resumen los problemas más críticos en el sistema operativo de Windows Server 2019, como las maneras de evitar o solucionar los problemas, si se conoce. Para obtener información acerca de los cambios por diseño, nuevas características y correcciones en esta versión, consulte [What ' s New in Windows Server 2019](whats-new-19.md) y anuncios de equipos de características específicas. A menos que se especifique lo contrario, cada problema notificado se aplica a todas las ediciones y opciones de instalación de Windows Server 2019.  

Este documento se actualiza continuamente. Habida cuenta de que se detectan problemas críticos que necesitan una solución alternativa, se agregan, al igual que las nuevas soluciones alternativas y correcciones, a medida que están disponibles.  

## <a name="release-notes"></a>Notas de la versión

Los problemas conocidos siguientes están presentes en Windows Server 2019.

| Título         | Descripción                            |
| -----         | -----------                            |
| Menú de opciones de instalación durante la instalación del servidor ha trunca el texto en alemán | Cuando se ejecuta el programa de instalación desde medios de alemán de servidor, en la ventana de selección de sistema operativo titulada, "Seleccione el sistema operativo que desea instalar," la descripción de las opciones de instalación de experiencia de escritorio tendrán caracteres incorrectos y que faltan al final de la oración. Este es el texto en alemán completa como debe aparecer.<br/>      <br/>`Durch diese Option wird die vollständige grafische Umgebung von Windows installiert, wodurch zusätzlicher Speicherplatz verbraucht wird. Sie kann hilfreich sein, wenn Sie den Windows-Desktop verwenden möchten oder über eine App verfügen, die die grafische Umgebung benötigt.` <br><br>Esto solo afecta a los medios de alemán publicados en la disponibilidad pública de 2019 de servidor de Windows, Windows Server, versión 1809 y Microsoft Hyper-V Server 2019.|
| Imagen de personalización de marca de Windows Server incorrecta durante la instalación de Windows Server, versión 1809 | Durante la experiencia de instalación de Windows Server, versión 1809, pantallas de la imagen de fondo en algunas inicial muestra &quot;Windows Server 2019&quot;.  Como con Windows Server, versión 1709 y 1803, esto debería decir simplemente &quot;Windows Server&quot;.  No hay ningún impacto en el producto en cualquier otro, y no hay ningún impacto en el producto de Windows Server 2019.  El problema se limita a este una imagen durante la instalación de Windows Server, versión 1809, disponible solo para clientes de licencias por volumen acceso el centro de servicio de licencias por volumen.<br/> |

### <a name="copyright"></a>Copyright

Este documento se proporciona “tal cual”. La información y las vistas expresadas en este documento, incluidas las direcciones URL y otras referencias a sitios web de Internet, pueden cambiar sin previo aviso.  

Este documento no le proporciona derechos legales sobre ninguna propiedad intelectual en ningún producto de Microsoft. Puedes copiarlo y usarlo como referencia interna.

&copy; 2019 Microsoft Corporation. Todos los derechos reservados.  

Microsoft, Active Directory, Hyper-V, Windows y Windows Server son marcas comerciales registradas o marcas comerciales de Microsoft Corporation en los Estados Unidos u otros países.  

El producto contiene software de filtros gráficos que está basado parcialmente en el trabajo de Independent JPEG Group.  


1.0  
