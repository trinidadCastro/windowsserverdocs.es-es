---
description: 'Más información acerca de: revisar el rol del servidor proxy de Federación en el asociado de recurso'
ms.assetid: 14aa112d-ae31-4181-97e4-92623b5c9270
title: Revisar el rol del Servidor proxy de federación del asociado de recurso
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: d0419028c4b2a9cc411cef81dabbb0ddb2da7044
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049973"
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-resource-partner"></a>Revisar el rol del Servidor proxy de federación del asociado de recurso

Un servidor proxy de Federación en Servicios de federación de Active Directory (AD FS) \( AD FS \) puede funcionar en uno o varios de los roles siguientes, dependiendo de cómo se configure el servidor para satisfacer las necesidades de la organización del asociado de recurso:

-   **Detección de asociado de cuenta**: un equipo cliente de Internet debe identificar qué asociado de cuenta lo autenticará. El cliente busca el asociado de cuenta mediante un formulario web de detección de asociado de cuenta \( discoverclientrealm. aspx \) , que se almacena en el servidor proxy de Federación del asociado de recurso. Si se configura más de un asociado de cuenta en el complemento de administración \- de AD FS, aparece un menú desplegable en \- el cliente con todos los asociados de cuenta disponibles que están visibles para los equipos cliente de Internet que tienen acceso al formulario web de detección de asociados de cuenta. Puedes cambiar cómo se muestra el formulario web de detección de asociados de cuenta a los equipos cliente personalizando el archivo discoverclientrealm.aspx.

-   **Redirección de tokens de seguridad**: el servidor proxy de Federación del asociado de cuenta envía los tokens de seguridad al asociado de recurso. El proxy de servidor de Federación de recursos acepta estos tokens y los pasa al servidor de Federación del asociado de recurso. A continuación, el servidor de Federación de recursos emite un token de seguridad que está enlazado a un servidor Web de recursos específico. Después, el proxy de servidor de Federación de recursos redirige el token al cliente.

En Resumen, un proxy de servidor de Federación de recursos facilita el proceso de inicio de sesión federado redirigiendo los equipos cliente a un servidor de Federación que puede autenticar a los clientes. Un proxy de servidor de Federación de recursos también actúa como proxy para los tokens de seguridad de cliente para los servidores de Federación de recursos.

> [!NOTE]
> Cuando sea necesario ayudar a reducir la cantidad de hardware y el número de certificados necesarios, el servidor proxy de Federación se puede ubicar en el mismo equipo que el servidor Web.

## <a name="see-also"></a>Consulte también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

