---
title: Administrar aplicaciones en Windows Server Essentials
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ae89c46a-0afd-4858-9150-ec97650f45a4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e98d661ac71697bc0e38b6a25fe2f9d2b0b7254f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="manage-applications-in-windows-server-essentials"></a>Administrar aplicaciones en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 El servidor de escritorio en Windows Server Essentials y Windows Server 2012 R2 con el rol de Windows Server Essentials Experience instalado hace posible realizar tareas de administración comunes. Para realizar estas tareas, consulta los siguientes temas:  
  
-   [Tareas de administración de aplicaciones en el panel](Manage-Applications-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Instalar o quitar complementos mediante el panel](Manage-Applications-in-Windows-Server-Essentials.md#BKMK_2)  
  
##  <a name="BKMK_1"></a>Tareas de administración de aplicaciones en el panel  
 La **aplicaciones** proporciona la página de administración del panel:  
  
-   Una lista de complementos instalados, que muestra:  
  
    -   El nombre del servicio en línea o el complemento  
  
    -   El estado de actualización de complemento  
  
    -   El estado de la suscripción de complemento  
  
    -   El nombre de la compañía o editor que se está distribuyendo el complemento  
  
-   Un panel de tareas que incluye un conjunto de tareas para administrar un complemento seleccionado  
  
-   Una lista de complementos que están disponibles para descargar e instalar desde Microsoft Pinpoint  
  
 La siguiente tabla describen las diferentes tareas de administración de complementos que están disponibles en el panel de servidor. Algunas de las tareas son específicos del complemento, por lo que solo son visibles cuando se selecciona un complemento en la lista.  
  
|Nombre de la tarea|Descripción|  
|---------------|-----------------|  
|Quitar el complemento|Quita el complemento seleccionado desde el servidor y de todos los demás equipos de la red.|  
|Instalar el complemento en equipos de la red|Ayudarle a programar la instalación del complemento seleccionado en todos los otros equipos de la red.|  
|Obtener ayuda con el complemento|Abre el Explorador de Internet en un sitio Web desde el que puedes encontrar soluciones a problemas y obtener más información sobre un complemento seleccionado.|  
|Actualizar el complemento|Te ayuda a descargar e instalar actualizaciones para los complementos que están instalados en los servidores y equipos de la red.|  
|Renovar la suscripción de complemento|Abre el Explorador de Internet en un sitio Web desde el que se puede renovar la suscripción de complemento.|  
|Lee la declaración de privacidad para el complemento|Abre el Explorador de Internet en un sitio Web desde el que puedes ver la declaración de privacidad.|  
|¿Cómo instalar o quitar complementos|Abre el Explorador de Internet en una página web que muestra el tema de Ayuda de asunto.|  
  
##  <a name="BKMK_2"></a>Instalar o quitar complementos mediante el panel  
 Un complemento es una aplicación de software que proporciona características adicionales y la funcionalidad de tu servidor. Hay un creciente número de complementos de Microsoft y otros proveedores de Software independientes (ISV).  
  
 Antes de que se puede aprovechar la funcionalidad extendida un complemento proporciona, primero debes instalar el complemento en el servidor.  
  
#### <a name="to-install-an-add-in-from-microsoft-pinpoint"></a>Para instalar un complemento de Microsoft Pinpoint  
  
1.  En el panel del servidor, haz clic en **aplicaciones**y, a continuación, haz clic en el **Microsoft Pinpoint** pestaña.  Aparece una lista de complementos disponibles.  
  
2.  Haz clic en el complemento que quieres instalar. Aparecerá la página de información de complemento.  
  
3.  En la página de información del complemento, haz clic en descargar y sigue las instrucciones en pantalla para descargar e instalar el complemento.  
  
4.  Sigue las instrucciones del Asistente para instalar el complemento.  
  
5.  Cuando finalice la instalación, reinicia el escritorio, abra el **aplicaciones** página del servidor de escritorio y comprueba que el complemento aparece en la vista de lista.  
  
#### <a name="to-install-an-add-in-from-another-provider"></a>Para instalar un complemento de otro proveedor  
  
1.  Abre el Explorador de Windows y busca la ubicación del archivo de instalación de complementos.  
  
2.  Haz doble clic en el archivo para ejecutar al Asistente para la instalación.  
  
3.  Sigue las instrucciones del Asistente para instalar el complemento.  
  
4.  Cuando finalice la instalación, reinicia el escritorio, abra el **aplicaciones** página y, a continuación, comprueba que el complemento aparece en la vista de lista.  
  
#### <a name="to-remove-an-add-in"></a>Para quitar un complemento  
  
1.  Abre el panel del servidor.  
  
2.  Haz clic en el **aplicaciones** pestaña.  
  
3.  En la **Add-ins** pestaña, selecciona el complemento que quieres quitar y, a continuación, haz clic en **quitar el complemento**.  
  
4.  En la **quitar complemento** ventana, haz clic en **quitar**.  
  
    > [!NOTE]
    >  Debes reiniciar el panel para quitar completamente el complemento.  
  
## <a name="see-also"></a>Consulta también  
  
-   [Información general del panel](Overview-of-the-Dashboard-in-Windows-Server-Essentials.md)  
  
-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)
