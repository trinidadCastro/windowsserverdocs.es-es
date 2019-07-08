---
title: 'Notas de la versión: Problemas importantes en Windows Server 2019'
description: Se resumen problemas críticos que requieren soluciones para evitar bloqueos, faltas de respuesta, errores de instalación y pérdida de datos.
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
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2019
ms.locfileid: "66810738"
---
# <a name="release-notes---important-issues-in-windows-server-2019"></a>Notas de la versión: Problemas importantes en Windows Server 2019

>Se aplica a: Windows Server 2019

En estas notas se resumen los problemas más críticos del sistema operativo Windows Server 2019 junto con soluciones alternativas y formas de evitar los problemas, si se conocen. Para más información sobre los cambios por diseño, nuevas características y soluciones en esta versión, ve [Novedades de Windows Server 2019](whats-new-19.md) y anuncios de equipos de características específicas. A menos que se especifique lo contrario, cada problema notificado se aplica a todas las ediciones y opciones de instalación de Windows Server 2019.  

Este documento se actualiza continuamente. Habida cuenta de que se detectan problemas críticos que necesitan una solución alternativa, se agregan, al igual que las nuevas soluciones alternativas y correcciones, a medida que están disponibles.  

## <a name="release-notes"></a>Notas de la versión

Los problemas conocidos siguientes están presentes en Windows Server 2019.

| Título         | Descripción                            |
| -----         | -----------                            |
| El menú de opciones de instalación durante la configuración del servidor ha truncado el texto en alemán. | Al ejecutar el programa de configuración desde medios de servidor alemán, en la ventana de selección de sistema operativo titulada "Seleccionar el sistema operativo que quieres instalar," en la descripción de las opciones de instalación de experiencia de escritorio faltarán caracteres y otros serán incorrectos al final de la oración. Este es el aspecto que debería tener el texto completo en alemán.<br/>      <br/>`Durch diese Option wird die vollständige grafische Umgebung von Windows installiert, wodurch zusätzlicher Speicherplatz verbraucht wird. Sie kann hilfreich sein, wenn Sie den Windows-Desktop verwenden möchten oder über eine App verfügen, die die grafische Umgebung benötigt.` <br><br>Esto solo afecta a los medios alemanes publicados en disponibilidad pública de 2019 de Windows Server 2019, Windows Server, versión 1809, y Microsoft Hyper-V Server 2019.|
| Imagen de personalización de marca de Windows Server incorrecta durante la configuración de Windows Server, versión 1809 | Durante la experiencia de configuración de Windows Server, versión 1809, la imagen de fondo en algunas pantallas iniciales muestra &quot;Windows Server 2019&quot;.  Como con Windows Server, versiones 1709 y 1803, debería indicar simplemente &quot;Windows Server&quot;.  No hay ningún otro impacto en el producto en cualquier otro, así como tampoco hay impacto en el producto de Windows Server 2019.  El problema se limita a esta imagen durante la configuración de Windows Server, versión 1809, disponible solo para clientes de licencias por volumen que acceden al Centro de servicios de licencias por volumen.<br/> |

### <a name="copyright"></a>Copyright

Este documento se proporciona “tal cual”. La información y las vistas expresadas en este documento, incluidas las direcciones URL y otras referencias a sitios web de Internet, pueden cambiar sin previo aviso.  

Este documento no le proporciona derechos legales sobre ninguna propiedad intelectual en ningún producto de Microsoft. Puedes copiarlo y usarlo como referencia interna.

&copy; 2019 Microsoft Corporation. Todos los derechos reservados.  

Microsoft, Active Directory, Hyper-V, Windows y Windows Server son marcas comerciales registradas o marcas comerciales de Microsoft Corporation en los Estados Unidos u otros países.  

El producto contiene software de filtros gráficos que está basado parcialmente en el trabajo de Independent JPEG Group.  


1.0  
