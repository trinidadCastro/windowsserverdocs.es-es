---
title: Instalar licencias de acceso de cliente de RDS
description: Obtén información sobre cómo instalar las CAL para los clientes de Escritorio remoto.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 09/20/2016
manager: dongill
ms.openlocfilehash: 3a9f73418bd4da67c97db30a3272588afc287b95
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "80860448"
---
# <a name="install-rds-client-access-licenses-on-the-remote-desktop-license-server"></a>Instalar licencias de acceso de cliente de RDS en el servidor de licencias de Escritorio remoto

>Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016

Usa la información siguiente para instalar las licencias de acceso de cliente (CAL) de Servicios de Escritorio remoto en el servidor de licencias. Una vez que se hayan instalado las CAL, el servidor de licencias las emitirá a los usuarios según corresponda.

Ten en cuenta que necesitas conectividad a Internet en el equipo que ejecuta el Administrador de licencias de Escritorio remoto, pero no en el equipo que ejecuta el servidor de licencias.

1. En el servidor de licencias (normalmente el primer Agente de conexión a Escritorio remoto), abre el Administrador de licencias de Escritorio remoto.
2. Haz clic con el botón derecho en el servidor de licencias y, luego, haz clic en **Instalar licencias**.
3. En la página de bienvenida, haz clic en **Siguiente**.
4. Selecciona el programa del que adquiriste las CAL de RDS y, luego, haz clic en **Siguiente**. Si eres proveedor de servicios, selecciona **Service Provider License Agreement** (Contrato de licencia de proveedor de servicios).
5. Escribe la información del programa de licencias. En la mayoría de los casos, será el código de licencia o un número de contrato, pero esto varía en función del programa de licencias que uses.
6. Haga clic en **Siguiente**.
7. Selecciona la versión del producto, el tipo de licencia y la cantidad de licencias para tu entorno y, luego, haga clic en **Siguiente**. El Administrador de licencias se pone en contacto con el Centro de activación de Microsoft (Clearinghouse) para validar y recuperar las licencias.
8.  Haga clic en **Finalizar** para completar el proceso.