---
title: Kerberos con nombre de entidad de seguridad de servicio (SPN)
description: Controladora de red admite varios métodos de autenticación para la comunicación con clientes de administración. Puede usar la autenticación basada en Kerberos, X509 autenticación basada en certificados. También tiene la opción de utilizar ninguna autenticación para las implementaciones de prueba.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: bc625de9-ee31-40a4-9ad2-7448bfbfb6e6
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 2459cfa8dfec3de4aa23da7aba192d6eeed7f8ec
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828976"
---
# <a name="kerberos-with-service-principal-name-spn"></a>Kerberos con nombre de entidad de seguridad de servicio (SPN)

>Se aplica a: Windows Server 2019

Controladora de red admite varios métodos de autenticación para la comunicación con clientes de administración. Puede usar la autenticación basada en Kerberos, X509 autenticación basada en certificados. También tiene la opción de utilizar ninguna autenticación para las implementaciones de prueba.

System Center Virtual Machine Manager usa la autenticación basada en Kerberos. Si usas autenticación basada en Kerberos, debe configurar un nombre Principal de servicio (SPN) para la controladora de red en Active Directory. El SPN es un identificador único para la instancia de servicio de controladora de red que usa la autenticación Kerberos para asociar una instancia de servicio con una cuenta de inicio de sesión del servicio. Para obtener más información, consulte [Service Principal Names](https://docs.microsoft.com/windows/desktop/ad/service-principal-names).

## <a name="configure-service-principal-names-spn"></a>Configurar nombres principales de servicio (SPN)

La controladora de red se configura automáticamente el SPN. Todo lo que necesita hacer es proporcionar permisos para las máquinas de controladora de red registrar y modificar el SPN.

1.  En la máquina del controlador de dominio, inicie **equipos y usuarios de Active Directory**.

2.  Seleccione **vista \> avanzada**.

3.  En **equipos**, busque una de las cuentas de equipo de la controladora de red y, a continuación, haga clic en y seleccione **propiedades**.

4.  Seleccione el **seguridad** ficha y haga clic en **avanzadas**.

5.  En la lista, si todas las cuentas de equipo de la controladora de red o de seguridad de un grupo de tener todas las cuentas de equipo de la controladora de red no aparece, haga clic en **agregar** para agregarlo.

6.  Para cada cuenta de equipo de la controladora de red o un grupo de seguridad única que contiene las cuentas de equipo de la controladora de red:

    a.  Seleccione la cuenta o grupo y haga clic en **editar**.

    b.  En, seleccione permisos **validar Escribir servicePrincipalName**.

    d.  Desplácese hacia abajo y en **propiedades** seleccione:

       -  **Read servicePrincipalName**

       -  **Escribir servicePrincipalName**

    e.  Haga clic en **Aceptar** dos veces.

7.  Repita el paso 3 al 6 para cada máquinas de controladora de red.

8.  aCierre **Usuarios y equipos de Active Directory**.

## <a name="failure-to-provide-permissions-for-spn-registrationmodification"></a>Error al proporcionar permisos para el registro SPN o modificación

En un **NEW** operaciones REST en la controladora de red de implementación de Windows Server 2019, si selecciona Kerberos para la autenticación de cliente REST y no conceder permiso para los nodos de controladora de red registrar o modificar el SPN, produce un error impide que la administración de SDN.

Para una actualización desde Windows Server 2016 en Windows Server 2019 y elige Kerberos para la autenticación de cliente REST, las operaciones de REST se bloquea, lo que garantiza la transparencia para las implementaciones de producción existentes. 

Si no está registrado el SPN, autenticación de cliente REST usa NTLM, que es menos seguro. También obtendrá un evento crítico en el canal de administración de **NetworkController Framework** canal de eventos que le pide que proporcione permisos a los nodos de controladora de red para registrar el SPN. Una vez que proporciona permiso, controladora de red registra el SPN automáticamente, y todas las operaciones de cliente utilizan Kerberos.


>[!TIP]
>Por lo general, puede configurar el controlador de red para que use una dirección IP o nombre DNS para las operaciones basadas en REST. Sin embargo, cuando se configura Kerberos, no se puede usar una dirección IP para que las consultas REST de controladora de red. Por ejemplo, puede usar \< https://networkcontroller.consotso.com\>, pero no se puede usar \< https://192.34.21.3\>. Los nombres de entidad de servicio no puede funcionar si se usan direcciones IP.
>
>Si se usa la dirección IP para las operaciones de REST, junto con la autenticación Kerberos en Windows Server 2016, habría sido la comunicación a través de la autenticación NTLM. En este tipo de implementación, una vez que actualice a Windows Server 2019, continuar utilizar la autenticación basada en NTLM. Para mover a la autenticación basada en Kerberos, debe usar nombre DNS del controlador de red para las operaciones de REST y proporcionar permisos para los nodos de controladora de red registrar el SPN.

---