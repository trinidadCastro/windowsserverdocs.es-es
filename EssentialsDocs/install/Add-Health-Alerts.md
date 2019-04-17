---
title: Agregar alertas de estado
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 270e0aac-dc42-46f3-a20b-a68ffbded06d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: cf0c062b92c687f5f7b33b419eafdca2dd3bbbfc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="add-health-alerts"></a>Agregar alertas de estado

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Un complemento de estado proporciona definiciones de alertas, comprobaciones de estado y reparaciones para problemas de red. Un complemento de estado consta de los archivos xml que anotan código o datos que se usan para evaluar la información de estado de una característica específica. Complementos de estado se crean los desarrolladores y administradores los instalan en los equipos servidor y cliente.  
  
 Consulte la [SDK de soluciones de Windows Server](https://go.microsoft.com/fwlink/?LinkID=248648) para obtener más información sobre cómo crear un complemento de estado.  
  
## <a name="installing-health-add-in-files"></a>Instalar archivos de complemento de estado  
 Después de que el desarrollador haya creado los archivos xml, debes colocar una copia de los archivos en la ubicación adecuada en los equipos servidor y cliente.  
  
#### <a name="to-install-the-xml-files-on-the-server"></a>Para instalar los archivos xml en el servidor  
  
1.  En la **%ProgramFiles%\Windows Server\Bin\Feature Definitions** carpeta, crear una nueva carpeta llamada **MyHealthAddIn**. Esta carpeta se puede dar cualquier nombre. Se recomienda que el nombre de la carpeta sea el mismo que el nombre de la característica.  
  
2.  Copia el Definition.xml y los archivos de config Definition.xml en la nueva carpeta.  
  
3.  Si has creado archivos binarios para condiciones o acciones, también deberá copiar los archivos en **%ProgramFiles%\Windows Server\Bin**.  
  
 Equipos cliente ejecutan una tarea programada cada seis horas que extrae los archivos XML en la ubicación adecuada. Puedes forzar la sincronización entre el equipo cliente y el servidor al ejecutar la tarea de forma manual.  
  
#### <a name="to-install-the-xml-files-on-the-client-computer"></a>Para instalar los archivos xml en el equipo cliente  
  
1.  Abre el programador de tareas.  
  
2.  Ejecutar el **HealthDefintionUpdateTask** en el programador de tareas.  
  
    > [!NOTE]
    >  Esta tarea no instala archivos binarios. Debes copiar manualmente los archivos binarios en la **%ProgramFiles%\Windows Server\Bin** carpeta en el equipo cliente.  
  
## <a name="see-also"></a>Consulta también  
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](Testing-the-Customer-Experience.md)