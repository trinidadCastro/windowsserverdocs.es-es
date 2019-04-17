---
ms.assetid: 22347a94-aeea-44b4-85fb-af2c968f432a
title: "Implementar con directivas de auditoría Central (pasos de demostración) de auditoría de seguridad"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ac2b1643ed151e94c3815abca9a57eb3706c845a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="deploy-security-auditing-with-central-audit-policies-demonstration-steps"></a>Implementar con directivas de auditoría Central (pasos de demostración) de auditoría de seguridad

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este escenario, se auditar el acceso a archivos en la carpeta documentos de finanzas mediante la directiva de finanzas que creaste en [implementar una directiva de acceso Central & #40; pasos de demostración & #41;](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md). Si un usuario que no está autorizado para acceder a la carpeta intenta acceder a ella, la actividad se captura en el Visor de eventos.   
 Los siguientes pasos son necesarios para probar este escenario.  
  
|Tarea|Descripción|  
|--------|---------------|  
|[Configurar el acceso a objetos Global](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_1)|En este paso, vas a configurar la directiva de acceso a objetos global en el controlador de dominio.|  
|[Configuración de directiva de grupo de actualización](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_2)|Inicia sesión en el servidor de archivos y aplicar la actualización de directiva de grupo.|  
|[Comprueba que se ha aplicado la directiva de acceso a objetos global](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_3)|Ver los eventos pertinentes en el Visor de eventos. Los eventos deben incluir metadatos para el tipo de país y el documento.|  
  
## <a name="BKMK_1"></a>Configurar la directiva de acceso a objetos global  
En este paso, vas a configurar la directiva de acceso a objetos global en el controlador de dominio.  
  
#### <a name="to-configure-a-global-object-access-policy"></a>Para configurar una directiva de acceso a objetos global  
  
1.  Inicia sesión en el controlador de dominio DC1 como Contoso\Administrador con la contraseña **pass@word1**.  
  
2.  En el administrador del servidor, seleccione **herramientas**y, a continuación, haz clic en **Group Policy Management **.  
  
3.  En el árbol de consola, haz doble clic en **dominios**, haz doble clic en **contoso.com**, haz clic en **Contoso**y, a continuación, haz doble clic en **servidores de archivos **.  
  
4.  Haz clic en **FlexibleAccessGPO**y haz clic en **editar **.  
  
5.  Haz doble clic en **configuración del equipo**, haz doble clic en **directivas**y, a continuación, haz doble clic en **configuración de Windows **.  
  
6.  Haz doble clic en **la configuración de seguridad**, haz doble clic en **configuración de directiva de auditoría avanzada**y, a continuación, haz doble clic en **las directivas de auditoría **.  
  
7.  Haz doble clic en **acceso a objetos**y, a continuación, haz doble clic en **auditar sistema de archivos **.  
  
8.  Selecciona el **configurar los siguientes eventos** casilla de verificación, selecciona el **éxito** y **error** casillas de verificación y, a continuación, haz clic en **Aceptar **.  
  
9. En el panel de navegación, haz doble clic en **auditoría de acceso a objetos Global**y, a continuación, haz doble clic en **sistema de archivos **.  
  
10. Selecciona el **definir esta configuración de directiva** casilla de verificación y haz clic en **configurar **.  
  
11. En la **la configuración de seguridad avanzada para la SACL de archivo Global** cuadro, haz clic en **agregar**, a continuación, haz clic en **seleccionar una entidad de seguridad**, tipo **todos**y, a continuación, haz clic en **Aceptar **.  
  
12. En la **entrada de auditoría para la SACL de archivo Global** cuadro, seleccione **control total** en la **permisos** cuadro.  
  
13. En la **agregar una condición:** sección, haz clic en **agregar una condición** y listas de en la lista desplegable, selecciona   
    [**Recursos**] [**Departamento**] [**Any of**] [**Value**] [**Finanzas**].  
  
14. Haz clic en **Aceptar** configuración de directiva de auditoría de tres veces para completar la configuración del acceso a objetos global.  
  
15. En el panel de navegación, haz clic en **acceso a objetos**y en el panel de resultados, haz doble clic en **manipulación de identificadores de auditoría **. Haz clic en **configurar los siguientes eventos de auditoría**, **éxito**, y **error**, haz clic en **Aceptar**y, a continuación, cierra el GPO de acceso flexible.  
  
## <a name="BKMK_2"></a>Actualizar la configuración de directiva de grupo  
En este paso, vas a actualizar la configuración de directiva de grupo después de haber creado la directiva de auditoría.  
  
#### <a name="to-update-group-policy-settings"></a>Para actualizar la configuración de directiva de grupo  
  
1.  Inicia sesión en el servidor de archivos, archivo1 como Contoso\Administrador con la contraseña **pass@word1**.  
  
2.  Presiona la tecla + R, de Windows, a continuación, escribe **cmd** para abrir una ventana de símbolo del sistema.  
  
    > [!NOTE]  
    > Si la **Control de cuentas de usuario** aparece el cuadro de diálogo, confirma que la acción que se muestra es lo que desea y, a continuación, haz clic en **Sí **.  
  
3.  Tipo **comando gpupdate/force** y, a continuación, presione ENTRAR.  
  
## <a name="BKMK_3"></a>Comprueba que se ha aplicado la directiva de acceso a objetos global  
Después de aplicar la configuración de directiva de grupo, puedes comprobar que la configuración de directiva de auditoría se haya aplicado correctamente.  
  
#### <a name="to-verify-that-the-global-object-access-policy-has-been-applied"></a>Para comprobar que se ha aplicado la directiva de acceso a objetos global  
  
1.  Inicia sesión en el equipo cliente, CLIENTE1 como Contoso\MReid. Busca la carpeta hipervínculo "file:///\\\ID_AD_FILE1\\\Finance" \\\ FILE1\Finance documentos y modificar 2 del documento de Word.  
  
2.  Inicia sesión en el servidor de archivos, archivo1 como Contoso\Administrador. Abre el Visor de eventos, vaya a **registros de Windows**, selecciona **seguridad**y confirme que tus actividades han generado eventos de auditoría **4656** y **4663** (aunque no hubieras definido listas SACL de auditoría explícitas en los archivos o carpetas que creaste, modificaste y eliminaste).  
  
> [!IMPORTANT]  
> Se genera un evento de inicio de sesión de nuevo en el equipo donde se encuentra en nombre del usuario para el que se está comprobando el acceso eficaz el recurso. Cuando se analizan los registros de auditoría de seguridad para el inicio de sesión actividad del usuario, para diferenciar entre los eventos de inicio de sesión que se generan debido al acceso eficaz y aquellos generados a causa de un usuario interactivo red inicia sesión, se incluirá la información del nivel de representación. Cuando se genere el evento de inicio de sesión debido a un acceso eficaz, el nivel de representación será la identidad. Por lo general, un registro del usuario interactivo de red en genera un evento de inicio de sesión con el nivel de representación = suplantación o delegación.  
  
## <a name="BKMK_Links"></a>Consulta también  
  
-   [Escenario: Auditoría de acceso del archivo](Scenario--File-Access-Auditing.md)  
  
-   [Planear el archivo de auditoría de acceso](Plan-for-File-Access-Auditing.md)  
  
-   [Control de acceso dinámico: Información general del escenario](Dynamic-Access-Control--Scenario-Overview.md)  
  

