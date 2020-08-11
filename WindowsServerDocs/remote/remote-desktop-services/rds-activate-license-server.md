---
title: Activación del servidor de licencias de Servicios de Escritorio remoto
description: Instalación y activación del servidor de licencias de Escritorio remoto
ms.topic: article
ms.assetid: eb24ddd2-0361-41fe-bd6b-c7c63427cb71
author: lizap
ms.author: elizapo
ms.date: 09/20/2016
manager: dongill
ms.openlocfilehash: 0caf683c95bcaaa8838028bb78c1209ccd5c916c
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87948908"
---
# <a name="activate-the-remote-desktop-services-license-server"></a>Activación del servidor de licencias de Servicios de Escritorio remoto

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019 y Windows Server 2016

El servidor de licencias de Escritorio remoto emite licencias de acceso de cliente (CAL) para usuarios y dispositivos cuando acceden al host de sesión del Escritorio remoto. Puedes activar el servidor de licencias mediante el Administrador de licencias de Escritorio remoto.

## <a name="install-the-rd-licensing-role"></a>Instalación del rol de licencias de Escritorio remoto

1. Inicia sesión en el servidor que quieres utilizar como servidor de licencias mediante una cuenta de administrador.
2. En el Administrador del servidor, haz clic en **Resumen de funciones** y, a continuación, haz clic en **Agregar funciones**.
   Haz clic en **Siguiente** en la primera página del Asistente para roles.
3. Selecciona **Servicios de escritorio remoto** y, después, haz clic sucesivamente en **Siguiente** y **Siguiente** en la página Servicios de Escritorio remoto.
4. Selecciona **Administración de licencias de Escritorio remoto** y, después, haz clic en **Siguiente**.
5. Configura el dominio: selecciona **Configure a discovery scope for this license server** (Configurar un ámbito de detección para este servidor de licencias), haz clic en **Este dominio** y, a continuación, haz clic en **Siguiente**.
6. Haga clic en **Instalar**.

## <a name="activate-the-license-server"></a>Activación del servidor de licencias

1. Abre el Administrador de licencias de Escritorio remoto: haz clic en **Inicio > Herramientas administrativas > Servicios de Escritorio remoto > Administrador de licencias de Escritorio remoto**.
2. Haz clic con el botón derecho en el servidor de licencias y, después, haz clic en **Activar servidor**.
3. En la página principal, haz clic en **Siguiente**.
4. Para el método de conexión, selecciona **Conexión automática (recomendado)** y, después, haz clic en **Siguiente**.
5. Escribe la información de tu empresa (tu nombre, el nombre de la empresa, tu región geográfica) y haz clic en **Siguiente**.
6. Opcionalmente, puedes especificar cualquier otra información de la empresa (por ejemplo, el correo electrónico y las direcciones de la empresa) y, después, haz clic en **Siguiente**.
7. Asegúrate de que **Iniciar el Asistente para instalar licencias ahora** no está seleccionado (instalaremos las licencias en un paso posterior) y, a continuación, haz clic en **Siguiente**.

Tu servidor de licencias ya está listo para comenzar a emitir y administrar licencias.