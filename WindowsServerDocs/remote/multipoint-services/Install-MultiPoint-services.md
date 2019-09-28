---
title: Instalar MultiPoint Services
description: Obtenga información sobre cómo instalar y configurar Multipoint Services en Windows Server 2016
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f6f8970b-de3f-4255-b2a1-5472a16ed02f
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 442699afe40ee67e4cd4f13572d1a482f675b84a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395384"
---
# <a name="install-multipoint-services"></a>Instalar MultiPoint Services
Si va a instalar un servidor desde cero, siga estas instrucciones para instalar Multipoint Services.  

Después de instalar Windows Server 2016, inicie sesión correctamente como administrador. Use el Administrador del servidor en el que puede habilitar Multipoint Services. El Administrador del servidor se abre automáticamente en el inicio. En el panel, seleccione **Agregar roles y características** para habilitar Multipoint Services y siga las instrucciones del asistente.

En la sección para el tipo de instalación, puede ir con el 
- Instalación basada en características o en roles o
- Servicios de Escritorio remoto instalación

En el caso de las implementaciones estándar de Multipoint Services, se recomienda seleccionar la instalación de Servicios de Escritorio remoto que le permite seleccionar de forma cómoda el rol Multipoint Services en tipo de implementación. Para la instalación basada en roles, deberá seleccionar **Multipoint Services** en la lista de roles. El servidor se reiniciará después de la instalación correcta.  
  
## <a name="configure-your-primary-station"></a>Configuración de la estación principal  
  
1.  En la página **crear una estación de MultiPoint Server** , escriba la letra especificada del teclado de ese monitor. La entrada de clave correcta asocia el teclado y el mouse para esa estación.  
2.  Inicie sesión como administrador.  