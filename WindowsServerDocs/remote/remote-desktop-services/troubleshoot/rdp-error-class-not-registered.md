---
title: Los clientes no se pueden conectar y reciben el mensaje Clase no registrada
description: Solución del error "Clase no registrada" con la conexión a Escritorio remoto.
audience: itpro
ms.custom: na
ms.reviewer: rklemen
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: kaushika-msft
manager: dcscontentpm
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 7a98e894f3eaf47e257ab39c640e93101fd76fc8
ms.sourcegitcommit: c5709021aa98abd075d7a8f912d4fd2263db8803
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/18/2020
ms.locfileid: "76265907"
---
# <a name="clients-cant-connect-and-get-the-class-not-registered-error"></a>Los clientes no se pueden conectar y reciben el mensaje "Clase no registrada"

Al intentar conectarse a un equipo remoto mediante un cliente que ejecuta la versión 1709 de Windows 10 o posterior, es posible que el cliente no se pueda conectar mientras el servidor de Host de sesión de Escritorio remoto muestra un mensaje que contiene el código de error "Clase no registrada (0x80040154)".

Este problema se produce si el usuario que intenta conectarse tiene un perfil de usuario obligatorio. Para resolverlo, instale la actualización de Windows 10 de [24 de julio de 2018: KB4338817 (compilación de sistema operativo 16299.579)](https://support.microsoft.com/help/4338817/windows-10-update-kb4338817) actualizaciones de Windows 10.
