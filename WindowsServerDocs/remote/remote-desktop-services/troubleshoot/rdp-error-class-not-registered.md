---
title: Los clientes no se pueden conectar y reciben el mensaje Clase no registrada
description: Solución del error "Clase no registrada" con la conexión a Escritorio remoto.
ms.reviewer: rklemen
ms.topic: troubleshooting
author: kaushika-msft
manager: dcscontentpm
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 52e696bd4229b947ea63a379211192b8664a9f93
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "80857188"
---
# <a name="clients-cant-connect-and-get-the-class-not-registered-error"></a>Los clientes no se pueden conectar y reciben el mensaje "Clase no registrada"

Al intentar conectarse a un equipo remoto mediante un cliente que ejecuta la versión 1709 de Windows 10 o posterior, es posible que el cliente no se pueda conectar mientras el servidor de Host de sesión de Escritorio remoto muestra un mensaje que contiene el código de error "Clase no registrada (0x80040154)".

Este problema se produce si el usuario que intenta conectarse tiene un perfil de usuario obligatorio. Para resolverlo, instale la actualización de Windows 10 de [24 de julio de 2018: KB4338817 (compilación de sistema operativo 16299.579)](https://support.microsoft.com/help/4338817/windows-10-update-kb4338817) actualizaciones de Windows 10.
