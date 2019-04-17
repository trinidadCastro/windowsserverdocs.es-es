---
title: 'Notas de la versión: problemas importantes en Windows Server 2019'
description: Se resumen problemas críticos que requieren soluciones para evitar bloqueos, francesa, pérdida de datos y de error de instalación
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 134aab85-664f-4d55-87ef-9e5fd098371f
author: coreyp-at-msft
ms.author: coreyp
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: d5d84bb1cd204a5419271cc3668343f8ac9c9a54
ms.sourcegitcommit: dbb4738fdac3b7911952ff11f1eaed9649d6567a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "9149895"
---
# Notas de la versión: problemas importantes en Windows Server 2019

>Se aplica a: Windows Server2019

Estas notas resumen los problemas más importantes en Windows Server&reg; del sistema operativo de 2019, incluyendo las formas para evitar o solucionar los problemas, si se conocen. Para obtener información acerca de los cambios por diseño, nuevas características y correcciones incluidas en esta versión, vea [What ' s New in Windows Server 2019](whats-new-19.md) y anuncios de equipos de características específicas. A menos que se especifique lo contrario, cada problema notificado se aplica a todas las ediciones y opciones de instalación de Windows Server 2019.  

Este documento se actualiza continuamente. Habida cuenta de que se detectan problemas críticos que necesitan una solución alternativa, se agregan, al igual que las nuevas soluciones alternativas y correcciones, a medida que están disponibles.  
  
## Notas de la versión
Los siguientes problemas conocidos están presentes en Windows Server 2019. 
<table border="1" rules="rows">
  <thead align="left" valign="middle">
    <tr>
      <th>Título</th>
      <th>Descripción</th>
    </tr>
  </thead>
  <tbody align="left" valign="middle">
    <tr>
      <td>Menú de opciones de instalación durante la instalación de servidor truncado texto en alemán</td>
      <td>Cuando se ejecuta el programa de instalación desde un medio de servidor alemán, en la ventana de selección de sistema operativo titulada "Seleccionar el sistema operativo que se va a instalar," Descripción de las opciones de instalación de experiencia de escritorio tendrá caracteres que faltan e incorrectos al final de la frase. Este es el texto en alemán completo como debe aparecer.  
      <br/>
      <p><i>Durch diese opción wird die vollständige grafische Umgebung van Windows installiert, wodurch zusätzlicher Speicherplatz verbraucht wird. SIE kann hilfreich sus, wenn Sie den las disponen de aplicación de escritorio de Windows verwenden möchten o über eine, die die grafische Umgebung benötigt.</i> </p>
      <p>Esto solo afecta a los medios de alemán lanzados en disponibilidad pública de Windows Server 2019, Windows Server, versión 1809 y Microsoft Hyper-V Server 2019.</p></td>
    </tr>
    <tr>
      <td>Imagen de personalización de marca de Windows Server incorrecta durante la instalación de Windows Server, versión 1809  </td>
      <td>Durante la experiencia de instalación de Windows Server, versión 1809, la imagen de fondo en algunas pantallas iniciales muestra "Windows Server 2019".  Como con Windows Server, versión 1709 y 1803, simplemente se debería indicar "Windows Server".  No hay ningún impacto en cualquier otro lugar en el producto y no hay ningún impacto en el producto Windows Server 2019.  El problema está limitado a esta uno imagen durante la instalación de Windows Server, versión 1809, disponible solo para los clientes de licencias por volumen tener acceso al centro de servicio de licencias por volumen.  
      </td>
    </tr>
  </tbody>
</table>


### Copyright  
Este documento se proporciona “tal cual”. La información y las vistas expresadas en este documento, incluidas las direcciones URL y otras referencias a sitios web de Internet, pueden cambiar sin previo aviso.  

Este documento no le proporciona derechos legales sobre ninguna propiedad intelectual en ningún producto de Microsoft. Puede copiar y usar este documento para su referencia interna.  

&copy;Microsoft Corporation de 2019. Todos los derechos reservados.  

Microsoft, Active Directory, Hyper-V, Windows y Windows Server son marcas comerciales registradas o marcas comerciales de Microsoft Corporation en los Estados Unidos u otros países.  

El producto contiene software de filtros gráficos que está basado parcialmente en el trabajo de Independent JPEG Group.  


1.0  
