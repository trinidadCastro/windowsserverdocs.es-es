---
title: Kerberos con nombre de entidad de seguridad de servicio (SPN)
description: La controladora de red admite varios métodos de autenticación para la comunicación con clientes de administración. Puede usar la autenticación basada en Kerberos, la autenticación basada en certificados X509. También tiene la opción de no usar autenticación para implementaciones de prueba.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: bc625de9-ee31-40a4-9ad2-7448bfbfb6e6
ms.author: lizross
author: eross-msft
ms.date: 08/23/2018
ms.openlocfilehash: adf282222674130dcb16b0c7bfe0cf3ff05ed720
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317397"
---
# <a name="kerberos-with-service-principal-name-spn"></a>Kerberos con nombre de entidad de seguridad de servicio (SPN)

>Se aplica a: Windows Server 2019

La controladora de red admite varios métodos de autenticación para la comunicación con clientes de administración. Puede usar la autenticación basada en Kerberos, la autenticación basada en certificados X509. También tiene la opción de no usar autenticación para implementaciones de prueba.

System Center Virtual Machine Manager usa la autenticación basada en Kerberos. Si usa la autenticación basada en Kerberos, debe configurar un nombre de entidad de seguridad de servicio (SPN) para la controladora de red en Active Directory. El SPN es un identificador único para la instancia de servicio de la controladora de red, que usa la autenticación Kerberos para asociar una instancia de servicio a una cuenta de inicio de sesión del servicio. Para obtener más información, consulte [nombres de entidad](https://docs.microsoft.com/windows/desktop/ad/service-principal-names)de seguridad de servicio.

## <a name="configure-service-principal-names-spn"></a>Configurar nombres de entidad de seguridad de servicio (SPN)

La controladora de red configura automáticamente el SPN. Lo único que debe hacer es proporcionar permisos para que los equipos de la controladora de red registren y modifiquen el SPN.

1.  En el equipo del controlador de dominio, inicie **Active Directory usuarios y equipos**.

2.  Seleccione **ver \> opciones avanzadas**.

3.  En **equipos**, busque una de las cuentas de equipo de la controladora de red y, a continuación, haga clic con el botón derecho y seleccione **propiedades**.

4.  Seleccione la pestaña **seguridad** y haga clic en **avanzadas**.

5.  En la lista, si no aparecen todas las cuentas de equipo de la controladora de red o un grupo de seguridad que tenga todas las cuentas de máquina de la controladora de red, haga clic en **Agregar** para agregarlas.

6.  Para cada cuenta de equipo de la controladora de red o un solo grupo de seguridad que contenga las cuentas de equipo de la controladora de red:

    a.  Seleccione la cuenta o el grupo y haga clic en **Editar**.

    b.  En permisos, seleccione **validar escribir servicePrincipalName**.

    d.  Desplácese hacia abajo y, en **propiedades** , seleccione:

       -  **Leer servicePrincipalName**

       -  **Escribir servicePrincipalName**

    e.  Haga clic en **Aceptar** dos veces.

7.  Repita el paso 3-6 para cada máquina de controladora de red.

8.  aCierre **Usuarios y equipos de Active Directory**.

## <a name="failure-to-provide-permissions-for-spn-registrationmodification"></a>Error al proporcionar permisos para la modificación o el registro de SPN

En una **nueva** implementación de Windows Server 2019, si elige Kerberos para la autenticación del cliente REST y no concede permiso para que los nodos de la controladora de red registren o modifiquen el SPN, las operaciones Rest en el controlador de red producirán un error que le impedirá administrar el Sdn.

Para una actualización de Windows Server 2016 a Windows Server 2019 y elige Kerberos para la autenticación de cliente de REST, las operaciones de REST no se bloquean, lo que garantiza la transparencia de las implementaciones de producción existentes. 

Si el SPN no está registrado, la autenticación del cliente de REST utiliza NTLM, que es menos seguro. También recibirá un evento crítico en el canal de administración del canal de eventos de **NetworkController-Framework** pidiéndole que proporcione permisos a los nodos de la controladora de red para registrar el SPN. Una vez que se proporciona el permiso, el controlador de red registra el SPN automáticamente y todas las operaciones de cliente utilizan Kerberos.


>[!TIP]
>Normalmente, puede configurar el controlador de red para que use una dirección IP o un nombre DNS para las operaciones basadas en REST. Sin embargo, cuando se configura Kerberos, no se puede usar una dirección IP para las consultas REST a la controladora de red. Por ejemplo, puede usar \<https://networkcontroller.consotso.com\>, pero no puede usar \<https://192.34.21.3\>. Los nombres de entidad de seguridad de servicio no pueden funcionar si se usan direcciones IP.
>
>Si usa la dirección IP para las operaciones REST junto con la autenticación Kerberos en Windows Server 2016, la comunicación real habría estado sobre la autenticación NTLM. En este tipo de implementación, una vez que actualice a Windows Server 2019, seguirá usando la autenticación basada en NTLM. Para pasar a la autenticación basada en Kerberos, debe usar el nombre DNS de la controladora de red para las operaciones REST y proporcionar permiso para que los nodos de la controladora de red registren el SPN.

---