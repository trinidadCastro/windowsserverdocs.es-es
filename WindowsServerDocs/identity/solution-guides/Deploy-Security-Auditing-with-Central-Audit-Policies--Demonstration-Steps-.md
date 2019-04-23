---
ms.assetid: 22347a94-aeea-44b4-85fb-af2c968f432a
title: Implementar la auditoría de seguridad con directivas de auditoría central (pasos de demostración)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ac2b1643ed151e94c3815abca9a57eb3706c845a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871136"
---
# <a name="deploy-security-auditing-with-central-audit-policies-demonstration-steps"></a>Implementar la auditoría de seguridad con directivas de auditoría central (pasos de demostración)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este escenario, auditará el acceso a los archivos en la carpeta Finance Documents usando la directiva de finanzas que creó en [implementar una directiva de acceso Central &#40;pasos de demostración&#41;](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md). Si un usuario que no está autorizado a acceder a la carpeta intenta acceder a ella, la actividad se captura en el visor de eventos.   
 Para someter a prueba este escenario, se requieren los siguientes pasos.  
  
|Tarea|Descripción|  
|--------|---------------|  
|[Configurar el acceso a objetos Global](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_1)|En este paso, se configura la directiva de acceso a objetos global en el controlador de dominio.|  
|[Configuración de directiva de grupo de actualizaciones](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_2)|Inicie sesión en el servidor de archivos y aplique la actualización de la directiva de grupo.|  
|[Compruebe que se ha aplicado la directiva de acceso a objetos global](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md#BKMK_3)|Visualice los eventos relevantes en el visor de eventos. Los eventos deben incluir metadatos para el país y el tipo de documento.|  
  
## <a name="BKMK_1"></a>Configurar la directiva de acceso del objeto global  
En este paso, se configura la directiva de acceso a objetos global en el controlador de dominio.  
  
#### <a name="to-configure-a-global-object-access-policy"></a>Para configurar una directiva de acceso a objetos global  
  
1.  Inicie sesión en el controlador de dominio DC1 como Contoso\Administrador con la contraseña **pass@word1**.  
  
2.  En Administrador del servidor, seleccione **Herramientas**y haga clic en **Administración de directivas de grupo**.  
  
3.  En el árbol de la consola, haga doble clic en **Dominios**, haga doble clic en **contoso.com**, haga clic en **Contoso**y, a continuación, haga doble clic en **Servidores de archivos**.  
  
4.  Haga clic con el botón secundario en **FlexibleAccessGPO** y haga clic en **Editar**.  
  
5.  Haga doble clic en **Configuración del equipo**, haga doble clic en **Directivas** y, a continuación, haga doble clic en **Configuración de Windows**.  
  
6.  Haga doble clic en **Configuración de seguridad**, haga doble clic en **Configuración de directiva de auditoría avanzada** y, a continuación, haga doble clic en **Directivas de auditoría**.  
  
7.  Haga doble clic en **Acceso a objetos**y, a continuación, haga doble clic en **Auditar sistema de archivos**.  
  
8.  Active la casilla **Configurar los siguientes eventos de auditoría**, active las casillas **Aciertos** y **Errores** y, a continuación, haga clic en **Aceptar**.  
  
9. En el panel de navegación, haga doble clic en **Auditoría de acceso a objetos global** y, a continuación, haga doble clic en **Sistema de archivos**.  
  
10. Active la casilla **Definir esta configuración de directiva** y haga clic en **Configurar**.  
  
11. En el cuadro **Configuración de seguridad avanzada para SACL de archivo global** , haga clic en **Agregar**; luego, haga clic en **Seleccionar una entidad de seguridad**, escriba **Todos**y haga clic en **Aceptar**.  
  
12. En el cuadro **Entrada de auditoría para SACL de archivo global**, seleccione **Control total** en el cuadro **Permisos**.  
  
13. En la sección **Agregar una condición:**, haga clic en **Agregar una condición** y, en las listas desplegables, seleccione   
    [**Recurso**] [**Departamento**] [**Cualquiera de**] [**Valor**] [**Finanzas**].  
  
14. Haga clic en **Aceptar** tres veces para completar la configuración de los parámetros de la directiva de auditoría de acceso a objetos global.  
  
15. En el panel de navegación, haga clic en **Acceso a objetos**y, en el panel de resultados, haga doble clic en **Auditar manipulación de identificadores**. Haga clic en **Configurar los siguientes eventos de auditoría**, **Correcto**y **Error**, haga clic en **Aceptar**y, a continuación, cierre el GPO de acceso flexible.  
  
## <a name="BKMK_2"></a>Actualizar la configuración de directiva de grupo  
En este paso se actualiza la configuración de directiva de grupo después de haberla creado.  
  
#### <a name="to-update-group-policy-settings"></a>Para actualizar la configuración de directiva de grupo  
  
1.  Inicie sesión en el servidor de archivos, FILE1 como Contoso\Administrador con la contraseña **pass@word1**.  
  
2.  Presione la tecla Windows+R y escriba **cmd** para abrir la ventana del símbolo del sistema.  
  
    > [!NOTE]  
    > Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
3.  Escriba **gpupdate /force** y presione ENTRAR.  
  
## <a name="BKMK_3"></a>Compruebe que se ha aplicado la directiva de acceso a objetos global  
Una vez aplicada la configuración de directiva de grupo, puede comprobar si la configuración de directiva de auditoría se ha aplicado correctamente.  
  
#### <a name="to-verify-that-the-global-object-access-policy-has-been-applied"></a>Para comprobar la aplicación de la directiva de acceso a objetos global  
  
1.  Inicie sesión en el equipo cliente, CLIENT1 como Contoso\MReid. Vaya a la carpeta HYPERLINK "file:///\\\\\\\ID_AD_FILE1\\\Finance" \\\ FILE1\Finance documentos y modifique el documento de Word 2.  
  
2.  Inicie sesión en el servidor de archivos, FILE1 como contoso\administrador. Abra el Visor de eventos, busque **Registros de Windows**, seleccione **Seguridad** y confirme que las actividades generaron los eventos de auditoría **4656** y **4663** (aunque no haya establecido SACL de auditoría específicos en los archivos o las carpetas que creó, modificó y eliminó).  
  
> [!IMPORTANT]  
> Se genera un nuevo evento de inicio de sesión en el equipo donde se encuentra el recurso, en nombre del usuario para el cual se está comprobando el acceso efectivo. Al analizar los registros de auditoría de seguridad de la actividad de inicio de sesión de los usuarios para diferenciar entre los eventos de inicio de sesión que se generan debido al acceso efectivo y aquellos que se generan debido al inicio de sesión de usuario interactivo en una red, se incluye información de Nivel de suplantación. Cuando se genera el evento de inicio de sesión debido a un acceso efectivo, el Nivel de suplantación será Identidad. Un inicio de sesión de usuario interactivo en la red suele generar un evento de inicio de sesión con Nivel de suplantación = Suplantación o Delegación.  
  
## <a name="BKMK_Links"></a>Vea también  
  
-   [Escenario: Auditoría de acceso a archivos](Scenario--File-Access-Auditing.md)  
  
-   [Plan para el archivo de auditoría de acceso](Plan-for-File-Access-Auditing.md)  
  
-   [Control de acceso dinámico: Información general del escenario](Dynamic-Access-Control--Scenario-Overview.md)  
  

