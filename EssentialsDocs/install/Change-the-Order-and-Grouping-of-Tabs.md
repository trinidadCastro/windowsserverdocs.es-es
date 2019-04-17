---
title: "Cambiar el orden y el agrupamiento de las pestañas"
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 79a417fd-1b3e-47ab-ae33-bb1faf95c86d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 578c5619cfdf076bb2735254494f393d56d35713
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="change-the-order-and-grouping-of-tabs"></a>Cambiar el orden y el agrupamiento de las pestañas

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Puedes cambiar el orden de las pestañas en el panel para que tu pestaña sea la primera (en la izquierda) en la fila de pestañas. Para ello, debes agregar una entrada en el registro. También puede afectar a la agrupación de las pestañas agregando entradas al registro. El orden de las pestañas puede ser tu pestaña principal seguida de Microsoft integradas las pestañas, seguida de cualquiera de tus pestañas adicionales y, a continuación, seguido de las pestañas de terceros.  
  
## <a name="change-the-order-of-the-tabs-in-the-dashboard"></a>Cambiar el orden de las pestañas en el panel  
 Debes agregar el identificador del complemento que muestra la pestaña en el registro para definir el orden.  
  
#### <a name="to-display-your-tab-first-in-the-list-of-tabs"></a>Para tu pestaña aparezca en primer lugar en la lista de pestañas  
  
1.  En el equipo de referencia, haz clic en **inicio**, escribe **regedit**y, a continuación, presiona **ENTRAR**.  
  
2.  En el panel izquierdo, expande **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE**, expanda **Microsoft**y después expande **Windows Server**. Si la **OEM** clave no existe, debes completar los siguientes pasos para crearla:  
  
    1.  Haz clic en **Windows Server**, elija **nueva**y, a continuación, haz clic en **clave**.  
  
    2.  Tipo **OEM** para el nombre de la clave.  
  
3.  Haz clic en **OEM**, elija **nueva**y, a continuación, haz clic en **valor de cadena**.  
  
4.  Tipo **DashboardMainTabID** como el nombre de cadena y después presiona **ENTRAR**.  
  
5.  Haz clic en la nueva cadena en el panel derecho y, a continuación, haz clic en **modificar**.  
  
6.  Escribe el GUID que se definió para la pestaña de nivel superior y, a continuación, presione **ENTRAR**.  
  
     Para obtener más información sobre cómo crear e identificar pestañas de nivel superior, consulta [crear una pestaña de nivel superior](https://msdn.microsoft.com/library/gg513957) en el SDK de soluciones de Windows Server.  
  
7.  Guarda los cambios en el registro.  
  
8.  También debes incluir el GUID para tu pestaña de nivel superior principal en la lista de identificadores para agrupar pestañas. Para ello, siga los pasos enumerados en **cambiar el agrupamiento de las pestañas en el panel**.  
  
## <a name="change-the-grouping-of-tabs-in-the-dashboard"></a>Cambiar el agrupamiento de las pestañas en el panel  
 Puedes asegurarte de que tus pestañas se agrupan juntas e incluidas en la lista de pestañas de Microsoft integradas, agregando los identificadores al registro.  
  
#### <a name="to-change-the-grouping-of-tabs"></a>Para cambiar el agrupamiento de las pestañas  
  
1.  Si regedit no está abierto, haz clic en **inicio**, escribe **regedit**y, a continuación, presiona **ENTRAR**.  
  
2.  En el panel izquierdo, expande **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE**, expanda **Microsoft**y después expande **Windows Server**.  
  
3.  Haz clic en **OEM**, elija **nueva**y, a continuación, haz clic en **clave**.  
  
4.  Tipo **DashboardAddins** como el nombre de clave y después presiona **ENTRAR**.  
  
5.  Haz clic en **DashboardAddins**, elija **nueva**y, a continuación, haz clic en **valor de cadena**.  
  
6.  Escribe el identificador de GUID para tu pestaña como nombre de la cadena. No se necesita ningún valor.  
  
7.  Repite los pasos 5 y 6 para cada pestaña y subpestaña.  
  
8.  Guarda los cambios del registro.  
  
## <a name="see-also"></a>Consulta también  
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](Testing-the-Customer-Experience.md)