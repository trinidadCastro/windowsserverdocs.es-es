---
title: Recopilación de datos de usuario del servidor de directivas de redes
description: Información que se usa para ayudar a los usuarios a autenticar a los usuarios mediante el servidor de directivas de redes en Windows Server 2016.
author: MicrosoftGuyJFlo
manager: mtillman
ms.author: joflore
ms.reviewer: richagi
ms.custom: it-pro
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.date: 05/01/2018
ms.openlocfilehash: def65c174ff608301f8d4f35ef1ce19818103e61
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859378"
---
# <a name="network-policy-server-user-data-collection"></a>Recopilación de datos de usuario del servidor de directivas de redes

En este documento se explica cómo buscar información de usuario recopilada por el servidor de directivas de redes (NPS) en el evento que desea quitar.

>[!Note]
>Si le interesa ver o eliminar datos personales, consulte la guía de Microsoft en las solicitudes de los [asuntos de datos de Windows para el sitio de RGPD](https://docs.microsoft.com/microsoft-365/compliance/gdpr-dsr-windows) . Si está buscando información general sobre RGPD, consulte la [sección RGPD del portal de confianza de servicios](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

## <a name="information-collected-by-nps"></a>Información recopilada por NPS

- Marca de tiempo
- Marca de tiempo del evento
- Nombre de usuario
- Nombre de usuario completo
- Dirección IP del cliente
- Proveedor del cliente
- Nombre descriptivo del cliente
- Tipo de autenticación
- Muchos otros campos relacionados con el protocolo RADIUS

## <a name="gather-data-from-nps"></a>Recopilación de datos de NPS

Si los datos de cuentas están habilitados y configurados, los registros de los intentos de autenticación de NPS de un usuario se pueden obtener de SQL Server o de los archivos de registro en función de la configuración. 

Si se configuran los datos de cuentas para SQL Server, consulte todos los registros donde User_Name = `'<username>'`.

Si se configuran los datos de cuentas para un archivo de registro, busque el `<username>` para buscar todas las entradas de registro en el archivo de registro.

Las entradas del registro de eventos de servicios de acceso y directivas de redes se consideran duplicadas en los datos de cuentas y no es necesario recopilarlas.

Si los datos de cuentas no están habilitados, los registros de los intentos de autenticación de NPS de un usuario se pueden obtener del registro de eventos de servicios de acceso y directivas de redes buscando el `<username>`.
