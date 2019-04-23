---
title: Instalar MultiPoint Services
description: Obtenga información sobre cómo instalar y configurar servicios MultiPoint en Windows Server 2016
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f6f8970b-de3f-4255-b2a1-5472a16ed02f
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 52a824bbca3e9f2e1c7823601f6208ae19ae50ef
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866226"
---
# <a name="install-multipoint-services"></a>Instalar MultiPoint Services
Si va a instalar a un servidor desde el principio, siga estas instrucciones para instalar MultiPoint Services.  

Después de haber instalado Windows Server 2016 correctamente inicie sesión como administrador. Utilice el administrador del servidor donde puede habilitar MultiPoint Services. El administrador del servidor se abre automáticamente al iniciarse. En, seleccione el panel **agregar roles y características** para habilitar los servicios de MultiPoint y siga las instrucciones del asistente.

En la sección para el tipo de instalación es posible que vaya con el 
- Instalación basada en roles o basada en características o
- Instalación de servicios de escritorio remoto

Para las implementaciones estándares de MultiPoint Services, se recomienda para seleccionar la instalación de servicios de escritorio remoto que permite que seleccionar el rol Servicios MultiPoint en tipo de implementación convenientemente. Para la instalación basada en roles deberá seleccionar **MultiPoint Services** en la lista de roles. El servidor se reiniciará tras una instalación correcta.  
  
## <a name="configure-your-primary-station"></a>Configurar la estación principal  
  
1.  En el **crear una estación de MultiPoint Server** página, escriba la letra especificada desde el teclado para ese monitor. La entrada de la clave correcta asocia el teclado y mouse de esa estación.  
2.  Inicie sesión como administrador.  