---
title: Paso 2 configurar el servidor RADIUS
description: Este tema forma parte de la guía deploy Remote Access with OTP Authentication in Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: 0326818f-9144-496c-b946-f82be4eefbd3
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 8c977a124f118076101c0fde4855e42d6e5030aa
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969102"
---
# <a name="step-2-configure-the-radius-server"></a>Paso 2 configurar el servidor RADIUS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Antes de configurar el servidor de acceso remoto para que admita DirectAccess con compatibilidad con OTP, configure el servidor RADIUS.

|Tarea|Descripción|
|----|--------|
|[2,1. configurar los tokens de distribución de software RADIUS](#BKMK_1.1)|En el servidor RADIUS, configure los tokens de distribución de software.|
|[2,2. configurar la información de seguridad de RADIUS](#BKMK_1.2)|En el servidor RADIUS, configure los puertos y el secreto compartido que se usarán.|
|[2,3 agregar la cuenta de usuario para el sondeo de OTP](#BKMK_Probe)|En el servidor RADIUS, cree una nueva cuenta de usuario para el sondeo de OTP.|
|[2,4 sincronizar con Active Directory](#BKMK_Active)|En el servidor RADIUS, cree cuentas de usuario sincronizadas con cuentas de Active Directory.|
|[2,5 configuración del agente de autenticación RADIUS](#BKMK_AuthAgent)|Configure el servidor de acceso remoto como agente de autenticación RADIUS.|

## <a name="21-configure-the-radius-software-distribution-tokens"></a><a name="BKMK_1.1"></a>2,1 configuración de los tokens de distribución de software RADIUS
El servidor RADIUS debe configurarse con la licencia y el software y/o los tokens de distribución de hardware necesarios para que DirectAccess los use con OTP. Este proceso será específico de cada implementación de proveedor de RADIUS.

## <a name="22-configure-the-radius-security-information"></a><a name="BKMK_1.2"></a>2,2 configuración de la información de seguridad de RADIUS
El servidor RADIUS usa puertos UDP para la comunicación y cada proveedor RADIUS tiene sus propios puertos UDP predeterminados para la comunicación entrante y saliente. Para que el servidor RADIUS funcione con el servidor de acceso remoto, asegúrese de que todos los firewalls del entorno estén configurados para permitir el tráfico UDP entre los servidores de DirectAccess y OTP a través de los puertos necesarios según sea necesario.

El servidor RADIUS usa un secreto compartido para la autenticación. Configure el servidor RADIUS con una contraseña segura para el secreto compartido y tenga en cuenta que se utilizará al configurar la configuración del equipo cliente del servidor de DirectAccess para su uso con DirectAccess con OTP.

## <a name="23-adding-user-account-for-otp-probing"></a><a name="BKMK_Probe"></a>2,3 agregar la cuenta de usuario para el sondeo de OTP
En el servidor RADIUS, cree una nueva cuenta de usuario denominada **DAProbeUser** y asígnele la contraseña **DAProbePass**.

## <a name="24-synchronize-with-active-directory"></a><a name="BKMK_Active"></a>2,4 sincronizar con Active Directory
El servidor RADIUS debe tener cuentas de usuario que correspondan a los usuarios de Active Directory que vayan a usar DirectAccess con OTP.

#### <a name="to-synchronize-the-radius-and-active-directory-users"></a>Para sincronizar los usuarios de RADIUS y Active Directory

1.  Grabe la información de usuario de Active Directory para todos los usuarios de DirectAccess con OTP.

2.  Use el procedimiento específico del proveedor para crear cuentas de usuario de **dominio\nombre** de usuario idénticas en el servidor RADIUS que se registraron.

## <a name="25-configure-the-radius-authentication-agent"></a><a name="BKMK_AuthAgent"></a>2,5 configuración del agente de autenticación RADIUS
El servidor de acceso remoto debe estar configurado como un agente de autenticación RADIUS para DirectAccess con la implementación de OTP. Siga las instrucciones del proveedor de RADIUS para configurar el servidor de acceso remoto como agente de autenticación RADIUS.



