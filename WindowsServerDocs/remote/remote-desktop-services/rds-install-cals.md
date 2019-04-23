---
title: Instalar licencias de acceso de cliente RDS
description: Obtenga información sobre cómo instalar las CAL para los clientes de escritorio remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 09/20/2016
manager: dongill
ms.openlocfilehash: 2f283b51acc869704a52f09bebc228660cdfbc38
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870576"
---
# <a name="install-rds-client-access-licenses-on-the-remote-desktop-license-server"></a>Instalar licencias de acceso de cliente RDS en el servidor de licencias de escritorio remoto

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Use la información siguiente para instalar licencias de acceso de cliente de servicios de escritorio remoto (CAL) en el servidor de licencias. Una vez que se instalan las CAL, el servidor de licencias emitirá a los usuarios según corresponda.

Tenga en cuenta que necesita conectividad a Internet en el equipo que ejecuta el Administrador de licencias de escritorio remoto, pero no en el equipo que ejecuta el servidor de licencias.

1. En el servidor de licencias (normalmente la primera RD Connection Broker), abra el Administrador de licencias de escritorio remoto.
2. Haga clic en el servidor de licencias y, a continuación, haga clic en **instalar licencias**.
3. Haga clic en **siguiente** en la página de bienvenida.
4. Seleccione el programa adquirió las CAL de RDS de y, a continuación, haga clic en **siguiente**. Si es un proveedor de servicios, seleccione **contrato de licencia de proveedor de servicios**.
5. Escriba la información de su programa de licencias. En la mayoría de los casos, será el código de licencia o un número de contrato, pero esto varía en función del programa de licencia que está utilizando.
6. Haz clic en **Siguiente**.
7. Seleccione la versión del producto, el tipo de licencia y el número de licencias para su entorno y, a continuación, haga clic en **siguiente**. El Administrador de licencias pone en contacto con Microsoft Clearinghouse para validar y recuperar sus licencias.
8.  Haga clic en **Finalizar** para completar el proceso.