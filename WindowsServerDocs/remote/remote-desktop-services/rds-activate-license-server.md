---
title: Activar el servidor de licencias de servicios de escritorio remoto
description: Instalar y activar el servidor de licencias de escritorio remoto
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eb24ddd2-0361-41fe-bd6b-c7c63427cb71
author: lizap
ms.author: elizapo
ms.date: 09/20/2016
manager: dongill
ms.openlocfilehash: d28ceac9cde0ee2d4c92867bdd90d5c463a8cd4c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888016"
---
# <a name="activate-the-remote-desktop-services-license-server"></a>Activar el servidor de licencias de servicios de escritorio remoto

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Los servicios de escritorio remoto licencias de acceso de cliente (CAL) de servidor problemas de licencias a usuarios y dispositivos cuando acceden a los Host de sesión de escritorio remoto. Puede activar el servidor de licencias mediante el Administrador de licencias de escritorio remoto. 

## <a name="install-the-rd-licensing-role"></a>Instalar el rol de licencias de escritorio remoto

1. Inicie sesión en el servidor que desee utilizar como el servidor de licencias con una cuenta de administrador.
2. En el administrador del servidor, haga clic en **resumen de funciones**y, a continuación, haga clic en **agregar Roles**.
   Haga clic en **siguiente** en la primera página del Asistente para funciones.
3. Seleccione **servicios de escritorio remoto**y, a continuación, haga clic en **siguiente**y, a continuación, **siguiente** en la página de servicios de escritorio remoto.
4. Seleccione **licencias de escritorio remoto**y, a continuación, haga clic en **siguiente**.
5. Configurar el dominio: seleccione **configurar un ámbito de detección para este servidor de licencias**, haga clic en **este dominio**y, a continuación, haga clic en **siguiente**.
6. Haga clic en **Instalar**.

## <a name="activate-the-license-server"></a>Activación del servidor de licencias

1. Abra el Administrador de licencias de escritorio remoto: haga clic en **Inicio > Herramientas administrativas > Servicios de escritorio remoto > Administrador de licencias de escritorio remoto**.
2. Haga clic en el servidor de licencias y, a continuación, haga clic en **activar servidor**.
3. Haga clic en **siguiente** en la página de bienvenida.
4. El método de conexión, seleccione **conexión automática (recomendado)** y, a continuación, haga clic en **siguiente**.
5. Escriba la información de su empresa (su nombre, el nombre de la empresa, la región geográfica) y, a continuación, haga clic en **siguiente**.
6. Opcionalmente, escriba cualquier otra información de la compañía (por ejemplo, direcciones de correo electrónico y la empresa) y, a continuación, haga clic en **siguiente**. 
7. Asegúrese de que **iniciar el Asistente para instalar licencias ahora** no está seleccionada (vamos a instalar las licencias en un paso posterior) y, a continuación, haga clic en **siguiente**.

El servidor de licencias está preparado para iniciar la emisión y administración de licencias. 