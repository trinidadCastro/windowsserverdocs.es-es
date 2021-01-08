---
title: Paso 2 planear la implementación del servidor RADIUS
description: Obtenga información acerca de cómo planear el servidor de autenticación de contraseña de un solo uso (OTP).
manager: brianlic
ms.topic: article
ms.assetid: 2d6ad863-02a5-49b0-9aff-d189e78b2b80
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 5658c4c3a323bf9e0df5af8cf57dad85d98fe8dd
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98040165"
---
# <a name="step-2-plan-the-radius-server-deployment"></a>Paso 2 planear la implementación del servidor RADIUS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Después de implementar un solo servidor de acceso remoto, planee el servidor de autenticación de contraseña de un solo tiempo (OTP).

|Tarea|Descripción|
|----|--------|
|2,1 planear el servidor RADIUS|Para el servidor de autenticación OTP, el acceso remoto en Windows Server 2016 y Windows Server 2012 admite cualquier servidor OTP habilitado para RADIUS que admita el protocolo de autenticación de contraseña (PAP).|

## <a name="21-plan-the-radius-server"></a><a name="BKMK_1.1"></a>2,1 planear el servidor RADIUS
Tenga en cuenta lo siguiente al planear un servidor RADIUS para la autenticación de OTP:

-   Para la mayoría de los tipos de implementaciones de OTP, debe configurar el servidor de acceso remoto como agente RADIUS. Para obtener más información, consulte la documentación del proveedor de OTP.

-   Para todas las implementaciones de OTP, debe sincronizar los usuarios de Active Directory con el servidor RADIUS.

-   No es necesario que el servidor RADIUS sea un miembro del dominio.

-   Al implementar el servidor RADIUS, se configura un secreto compartido y el número de puerto para el tráfico RADIUS. Tome nota de estos detalles; son necesarios cuando se configura el servidor de acceso remoto.

Puede ver una guía del laboratorio de pruebas de ejemplo que configura la autenticación de OTP con un servidor RSA SecurID en la [Guía del laboratorio de pruebas: demostración de DirectAccess con autenticación OTP y RSA SecurID](../../../directaccess/tlg-otp-securid/test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid.md).



