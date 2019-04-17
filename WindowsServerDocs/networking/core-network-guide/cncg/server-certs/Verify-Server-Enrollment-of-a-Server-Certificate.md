---
title: Comprobar el servidor de inscripción de un certificado de servidor
description: Este tema es parte de la Guía de certificados de servidor de implementación para implementaciones de conexión inalámbrica y cableadas 802.1X
manager: brianlic
ms.topic: article
ms.assetid: bd80a018-5a30-47c3-89fc-aacb9f5ad298
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d8ff51fa83972e2fc73ee54628eeb89e2927046d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="verify-server-enrollment-of-a-server-certificate"></a>Comprobar el servidor de inscripción de un certificado de servidor

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este procedimiento para comprobar que los servidores de servidor de directivas de redes (NPS) han inscribir un certificado de servidor de la entidad de certificación (CA).   
  
>[!NOTE]  
>Pertenencia a la **administradores de dominio** grupo es lo mínimo necesario para completar estos procedimientos.  
  
## <a name="verify-network-policy-server-nps-enrollment-of-a-server-certificate"></a>Comprueba la inscripción de servidor de directivas de redes (NPS) de un certificado de servidor  
  
Porque NPS se usa para autenticar y autorizar las solicitudes de conexión de red, es importante asegurarse de que el certificado de servidor que ha emitido a servidores NPS es válido cuando se usa en las directivas de red.  
  
Para comprobar que un certificado de servidor está configurado correctamente y se inscribe en el servidor NPS, debe configurar una directiva de red de prueba y permitir NPS comprobar que NPS puede usar el certificado de autenticación.  
  
### <a name="to-verify-nps-server-enrollment-of-a-server-certificate"></a>Para comprobar la inscripción del servidor NPS de un certificado de servidor  
  
1.  En el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **el servidor de directivas de red**. Abre la red directiva servidor Microsoft Management Console (MMC).  
  
2.  Haz doble clic en **directivas**, haz clic en **las directivas de red**y haz clic en **nueva**. Abre el Asistente para la nueva directiva de red.  
  
3.  En **especificar el nombre de directiva de red y el tipo de conexión**, en **nombre de la directiva**, tipo **probar directiva**. Asegúrate de que **tipo de servidor de acceso de red** tiene el valor **sin especificar**y, a continuación, haz clic en **siguiente**.  
  
4.  En **especificar condiciones**, haz clic en **agregar**. En **seleccione condición**, haz clic en **grupos de Windows**y, a continuación, haz clic en **agregar**.  
  
5.  En **grupos**, haz clic en **agregar grupos**. En **seleccionar un grupo**, tipo **los usuarios del dominio**, y, a continuación, presione ENTRAR. Haz clic en **Aceptar**y, a continuación, haz clic en **siguiente**.  
  
6.  En **especificar el permiso de acceso**, asegúrate de que **acceso otorgado** está seleccionado y, a continuación, haz clic en **siguiente**.  
  
7.  En **configurar métodos de autenticación**, haz clic en **agregar**. En **Agregar EAP**, haz clic en **Microsoft: EAP protegido (PEAP)**y, a continuación, haz clic en **Aceptar**. En **tipos de EAP**, selecciona **Microsoft: EAP protegido (PEAP)**y, a continuación, haz clic en **editar**. La **editar propiedades de EAP protegido** abre el cuadro de diálogo.  
  
8.  En la **editar propiedades de EAP protegido** cuadro de diálogo **certificado emitido para**, NPS muestra el nombre de su certificado de servidor en el formato *ComputerName*. *Dominio*. Por ejemplo, si el servidor NPS se denomina NPS-01 y el dominio es example.com, NPS muestra el certificado **NPS-01.example.com**. Además, en **emisor**, se muestra el nombre de la entidad de certificación y en **fecha de expiración**, se muestra la fecha de expiración del certificado del servidor. Esto demuestra que el servidor NPS ha inscribir un certificado de servidor válido que puede usar para probar su identidad en equipos cliente que intentan obtener acceso a la red a través de los servidores de acceso de red, como los servidores de red privada virtual (VPN), 802.1X 802.1X, servidores de puerta de enlace de escritorio remoto y puntos de acceso inalámbrico compatible con X Ethernet compatible con X cambia.  
  
    > [!IMPORTANT]  
    > Si NPS no muestra un certificado de servidor válida y proporciona el mensaje que este certificado no se encuentra en el equipo local, hay dos causas posibles de este problema. Es posible que no se actualizó correctamente la directiva de grupo y el servidor NPS no inscribe un certificado de la CA. En este caso, reinicie el servidor NPS. Cuando se reinicie el equipo, se actualiza la directiva de grupo, y puedes realizar este procedimiento para comprobar que el certificado de servidor se inscribe. Si la actualización de directiva de grupo no resuelve este problema, la plantilla de certificado, inscripción automática de certificados o ambos no están configurados correctamente. Para resolver estos problemas, iniciar al principio de esta guía y realizar todos los pasos nuevamente para asegurarse de que la configuración que has proporcionado sea precisa.  
  
9. Cuando haya comprobado la presencia de un certificado de servidor válido, puedes hacer clic en **Aceptar** y **cancelar** salir del Asistente para la nueva directiva de red.  
  
    > [!NOTE]  
    > Dado que no son completar al asistente, la directiva de red de prueba no se crea en NPS.  
  


