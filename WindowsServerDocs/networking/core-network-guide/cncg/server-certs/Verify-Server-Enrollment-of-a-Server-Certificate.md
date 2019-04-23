---
title: Comprobar la inscripción de servidor de un certificado de servidor
description: Este tema forma parte de la Guía de implementación de servidores de certificados para las implementaciones inalámbricas y cableadas 802.1X
manager: brianlic
ms.topic: article
ms.assetid: bd80a018-5a30-47c3-89fc-aacb9f5ad298
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 45ba7a9a7fc5b9622ab1b9a94f38f4bf4de13192
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850906"
---
# <a name="verify-server-enrollment-of-a-server-certificate"></a>Comprobar la inscripción de servidor de un certificado de servidor

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para comprobar que los servidores del servidor de directivas de redes (NPS) han inscrito un certificado de servidor de la entidad de certificación (CA).   
  
>[!NOTE]  
>Pertenencia a la **Admins. del dominio** grupo es el requisito mínimo para completar estos procedimientos.  
  
## <a name="verify-network-policy-server-nps-enrollment-of-a-server-certificate"></a>Comprobar la inscripción del servidor de directivas de redes (NPS) de un certificado de servidor  
  
Dado que NPS se usa para autenticar y autorizar las solicitudes de conexión de red, es importante asegurarse de que el certificado de servidor que se ha emitido a NPSs es válido cuando se usa en las directivas de red.  
  
Para comprobar que un certificado de servidor está configurado correctamente y se inscribe en el NPS, debe configurar una directiva de red de prueba y permiten al NPS comprobar que NPS puede usar el certificado para la autenticación.  
  
### <a name="to-verify-nps-enrollment-of-a-server-certificate"></a>Para comprobar la inscripción de un certificado de servidor NPS  
  
1.  En el administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **servidor de directivas de red**. Se abre la red directiva de servidor de Microsoft Management Console (MMC).  
  
2.  Haga doble clic en **directivas**, haga clic en **las directivas de red**y haga clic en **New**. Se abre el Asistente para nueva directiva de red.  
  
3.  En **especificar el nombre de directiva de red y el tipo de conexión**, en **nombre de la directiva**, tipo **probar directiva**. Asegúrese de que **tipo de servidor de acceso de red** tiene el valor **Unspecified**y, a continuación, haga clic en **siguiente**.  
  
4.  En **especificar condiciones**, haga clic en **agregar**. En **Seleccionar condición**, haga clic en **grupos de Windows**y, a continuación, haga clic en **agregar**.  
  
5.  En **grupos**, haga clic en **agregar grupos**. En **Seleccionar grupo**, tipo **los usuarios del dominio**, y, a continuación, presione ENTRAR. Haga clic en **Aceptar** y luego en **Siguiente**.  
  
6.  En **especificar permisos de acceso**, asegúrese de que **acceso concedido** está seleccionada y, a continuación, haga clic en **siguiente**.  
  
7.  En **configurar métodos de autenticación**, haga clic en **agregar**. En **agregue EAP**, haga clic en **Microsoft: EAP protegido (PEAP)** y, a continuación, haga clic en **Aceptar**. En **tipos de EAP**, seleccione **Microsoft: EAP protegido (PEAP)** y, a continuación, haga clic en **editar**. El **editar propiedades de EAP protegido** abre el cuadro de diálogo.  
  
8.  En el **editar propiedades de EAP protegido** cuadro de diálogo **certificado emitido para**, NPS muestra el nombre de su certificado de servidor en el formato *ComputerName*. *Dominio*. Por ejemplo, si el NPS se denomina NPS-01 y el dominio es ejemplo.com, NPS muestra el certificado **NPS 01.example.com**. Además, en **emisor**, se muestra el nombre de la entidad de certificación y en **fecha de expiración**, se muestra la fecha de expiración del certificado del servidor. Esto demuestra que el NPS se ha inscrito un certificado de servidor válido que puede usar para demostrar su identidad a los equipos cliente que están intentando obtener acceso a la red a través de los servidores de acceso de red, como los servidores de red privada virtual (VPN), 802.1X compatibles con X puntos de acceso inalámbricos, servidores de puerta de enlace de escritorio remoto y 802.1X, conmutadores Ethernet compatibles con X.  
  
    > [!IMPORTANT]  
    > Si NPS no muestra un certificado de servidor válido y proporciona el mensaje que no se encuentra este tipo de certificado en el equipo local, hay dos razones posibles para este problema. Es posible que no se actualizó correctamente la directiva de grupo y el NPS no se ha inscrito un certificado de la CA. En este caso, reinicie el NPS. Cuando se reinicia el equipo, se actualiza la directiva de grupo, y puede realizar este procedimiento para comprobar que el certificado de servidor esté inscrito. Si la actualización de directiva de grupo no resuelve este problema, la plantilla de certificado, la inscripción automática de certificado o ambos no están configurados correctamente. Para resolver estos problemas, comience desde el principio de esta guía y realizar todos los pasos para asegurarse de que la configuración que ha proporcionado sea precisa.  
  
9. Cuando haya comprobado la presencia de un certificado de servidor válido, haga clic en **Aceptar** y **cancelar** para salir del Asistente para nueva directiva de red.  
  
    > [!NOTE]  
    > Dado que no están finalizando al asistente, la directiva de red de prueba no se crea en NPS.  
  


