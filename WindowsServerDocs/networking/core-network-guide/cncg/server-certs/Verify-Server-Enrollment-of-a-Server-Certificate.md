---
title: Comprobar la inscripción de servidor de un certificado de servidor
description: Obtenga información acerca de cómo comprobar que los servidores del servidor de directivas de redes han inscrito un certificado de servidor de la entidad de certificación.
manager: brianlic
ms.topic: article
ms.assetid: bd80a018-5a30-47c3-89fc-aacb9f5ad298
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 188e75ed58c3adc0249305d669acbb06d0b6c15d
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98038345"
---
# <a name="verify-server-enrollment-of-a-server-certificate"></a>Comprobar la inscripción de servidor de un certificado de servidor

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para comprobar que los servidores del servidor de directivas de redes (NPS) han inscrito un certificado de servidor de la entidad de certificación (CA).

>[!NOTE]
>Para completar estos procedimientos, lo mínimo que se necesita es pertenecer al grupo **Admins** . del dominio.

## <a name="verify-network-policy-server-nps-enrollment-of-a-server-certificate"></a>Comprobar la inscripción del servidor de directivas de redes (NPS) de un certificado de servidor

Dado que NPS se usa para autenticar y autorizar las solicitudes de conexión de red, es importante asegurarse de que el certificado de servidor que ha emitido para NPSs es válido cuando se usa en directivas de red.

Para comprobar que un certificado de servidor está configurado correctamente y está inscrito en el NPS, debe configurar una directiva de red de prueba y permitir que NPS Compruebe que NPS pueda usar el certificado para la autenticación.

### <a name="to-verify-nps-enrollment-of-a-server-certificate"></a>Para comprobar la inscripción de NPS de un certificado de servidor

1.  En el Administrador del servidor, haga clic en **Herramientas** y, a continuación, haga clic en **Servidor de directivas de redes**. Se abre Microsoft Management Console (MMC) del servidor de directivas de redes.

2.  Haga doble clic en **directivas**, haga clic con el botón secundario en **directivas de red** y haga clic en **nuevo**. Se abre el asistente para nueva directiva de red.

3.  En **especificar el nombre de la Directiva de red y el tipo de conexión**, en **nombre de directiva**, escriba Directiva de **prueba**. Asegúrese de que el **tipo de servidor de acceso a la red** tiene el valor **sin especificar** y, a continuación, haga clic en **siguiente**.

4.  En **especificar condiciones**, haga clic en **Agregar**. En **seleccionar condición**, haga clic en **grupos de Windows** y, a continuación, haga clic en **Agregar**.

5.  En **grupos**, haga clic en **agregar grupos**. En **Seleccionar grupo**, escriba **usuarios del dominio** y, a continuación, presione Entrar. Haga clic en **Aceptar** y luego en **Siguiente**.

6.  En **especificar permiso de acceso**, asegúrese de que está seleccionada la opción **acceso concedido** y, a continuación, haga clic en **siguiente**.

7.  En **configurar métodos de autenticación**, haga clic en **Agregar**. En **Agregar EAP**, haga clic en **Microsoft: EAP protegido (PEAP)** y, a continuación, haga clic en **Aceptar**. En **tipos de EAP**, seleccione **Microsoft: EAP protegido (PEAP)** y, a continuación, haga clic en **Editar**. Se abre el cuadro de diálogo **Editar propiedades de EAP protegido** .

8.  En el cuadro de diálogo **Editar propiedades de EAP protegido** , en **certificado emitido para**, NPS muestra el nombre del certificado de servidor con el formato *ComputerName*. *Dominio*. Por ejemplo, si el NPS se denomina NPS-01 y el dominio es example.com, NPS muestra el certificado **NPS-01.example.com**. Además, en **emisor**, se muestra el nombre de la entidad de certificación y, en **fecha de expiración**, se muestra la fecha de expiración del certificado de servidor. Esto demuestra que el NPS ha inscrito un certificado de servidor válido que puede usar para demostrar su identidad a los equipos cliente que intentan obtener acceso a la red a través de los servidores de acceso a la red, como los servidores de red privada virtual (VPN), los puntos de acceso inalámbricos compatibles con 802.1 X, los servidores de puerta de enlace de Escritorio remoto y los conmutadores Ethernet compatibles con

    > [!IMPORTANT]
    > Si NPS no muestra un certificado de servidor válido y proporciona el mensaje que indica que no se puede encontrar dicho certificado en el equipo local, hay dos causas posibles para este problema. Es posible que directiva de grupo no se actualizara correctamente y que NPS no haya inscrito un certificado de la entidad de certificación. En este caso, reinicie el NPS. Cuando se reinicia el equipo, directiva de grupo se actualiza y puede realizar este procedimiento de nuevo para comprobar que el certificado de servidor está inscrito. Si la actualización de directiva de grupo no resuelve este problema, la plantilla de certificado, la inscripción automática de certificados o ambos no se configuran correctamente. Para resolver estos problemas, empiece por el principio de esta guía y realice todos los pasos de nuevo para asegurarse de que los valores que ha proporcionado son precisos.

9. Cuando haya comprobado la presencia de un certificado de servidor válido, puede hacer clic en **Aceptar** y **Cancelar** para salir del Asistente para nueva Directiva de red.

    > [!NOTE]
    > Como no está completando el asistente, la Directiva de red de prueba no se crea en NPS.



