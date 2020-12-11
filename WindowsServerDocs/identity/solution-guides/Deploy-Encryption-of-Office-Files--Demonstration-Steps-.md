---
description: Más información acerca de cómo implementar el cifrado de archivos de Office (pasos de demostración)
ms.assetid: 2c76e81a-c2eb-439f-a89f-7d3d70790244
title: Deploy Encryption of Office Files (Demonstration Steps)
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: dbefe3b92a1e400d525b4dc57393cc5189521d44
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97046553"
---
# <a name="deploy-encryption-of-office-files-demonstration-steps"></a>Deploy Encryption of Office Files (Demonstration Steps)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El departamento financiero de Contoso tiene varios servidores de archivos que almacenan sus documentos. Estos documentos pueden ser documentación general o pueden tener un alto impacto para la empresa (HBI). Por ejemplo, Contoso considera que todos los documentos que contengan información confidencial tienen un alto impacto para la empresa. Contoso quiere asegurarse de que toda su documentación tenga una protección mínima y que su documentación HBI se restrinja a las personas adecuadas. Para ello, Contoso está explorando el uso de la infraestructura de clasificación de archivos (FCI) y AD RMS que está disponible en Windows Server 2012. Con FCI, Contoso clasificará todos los documentos de su servidor de archivos según el contenido y, después, usará AD RMS para aplicar la directiva de derechos apropiada.

En este escenario, realizará los siguientes pasos:

|Tarea|Descripción|
|--------|---------------|
|[Habilitar las propiedades de recursos](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_1.1)|Habilita las propiedades de recursos **Impacto** e **Información de identificación personal**.|
|[Crear reglas de clasificación](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_2)|Crea las siguientes reglas de clasificación: **Regla de clasificación HBI** y **Regla de clasificación PII**.|
|[Usar tareas de administración de archivos para proteger los documentos automáticamente con AD RMS](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_3)|Crea una tarea de administración de archivos que use automáticamente AD RMS para proteger los documentos con información de identificación personal (PII). Solo los miembros del grupo FinanceAdmin tendrán acceso a los documentos que contienen PII.|
|[Visualización de los resultados](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_4)|Examina la clasificación de los documentos y observa cómo cambian cuando cambias el contenido del documento. Comprueba también cómo se protege el documento con AD RMS.|
|[Comprobar la protección de AD RMS](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_5)|Comprueba que el documento está protegido con AD RMS.|
|||

## <a name="step-1-enable-resource-properties"></a><a name="BKMK_1.1"></a>Paso 1: Habilitar las propiedades de recursos

#### <a name="to-enable-resource-properties"></a>Para habilitar las propiedades de recursos

1. En el Administrador de Hyper-V, conecta con el servidor ID_AD_DC1. Inicie sesión en el servidor con Contoso\administrador con la contraseña <strong>pass@word1</strong> .

2. Abre el Centro de administración de Active Directory y haz clic en **Vista de árbol**.

3. Expande **CONTROL DE ACCESO DINÁMICO** y selecciona **Propiedades de recursos**.

4. Desplázate hacia abajo hasta la propiedad **Impacto** en la columna **Nombre para mostrar**. Haz clic con el botón secundario en **Impact** y, luego, haz clic en **Habilitar**.

5. Desplázate hacia abajo hasta la propiedad **Información de identificación personal** en la columna **Nombre para mostrar**. Haz clic con el botón secundario en **Personally Identifiable Information** y, luego, haz clic en **Habilitar**.

6. Para publicar las propiedades de recursos en la **Lista de recursos global**, en el panel izquierdo, haz clic en **Listas de propiedades de recursos** y, después, haz doble clic en **Lista de propiedades de recursos global**.

7. Haz clic en **Agregar**, desplázate hacia abajo y haz clic en **Impacto** para agregarla a la lista. Haz lo mismo para **Información de identificación personal**. Haz clic en **Aceptar** dos veces para finalizar.

![guías de soluciones ](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif) * *_<em>comandos equivalentes de Windows PowerShell</em>_* _

Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.

```
Set-ADResourceProperty -Enabled:$true -Identity:"CN=Impact_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com"
Set-ADResourceProperty -Enabled:$true -Identity:"CN=PII_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com"
```

## <a name="step-2-create-classification-rules"></a><a name="BKMK_2"></a>Paso 2: Crear reglas de clasificación
En este paso se explica cómo crear la regla de clasificación _ *impacto alto*. Esta regla buscará en el contenido de los documentos y, si se encuentra la cadena "Contoso Confidential", clasificará este documento como que tiene un gran impacto en el negocio. Esta clasificación invalidará la clasificación previamente asignada de bajo impacto para la empresa.

También crearás una regla **Alta PII**. Esta regla busca en el contenido de los documentos y, si encuentra un número de la Seguridad Social, clasifica el documento como contenido de alta PII.

#### <a name="to-create-the-high-impact-classification-rule"></a>Para crear la regla de clasificación de alto impacto

1. En el Administrador de Hyper-V, conecta con el servidor ID_AD_FILE1. Inicie sesión en el servidor con Contoso\administrador con la contraseña <strong>pass@word1</strong> .

2. Tienes que actualizar las propiedades de recursos globales desde Active Directory. Abra Windows PowerShell, escriba `Update-FSRMClassificationPropertyDefinition`y, luego, presione ENTRAR. Cierra Windows PowerShell.

3. Abra el Administrador de recursos del servidor de archivos. Para abrir el Administrador de recursos del servidor de archivos haz clic en **Inicio**, escribe **administrador de recursos del servidor de archivos** y haz clic en **Administrador de recursos del servidor de archivos**.

4. En el panel izquierdo del Administrador de recursos del servidor de archivos, expande **Administración de clasificaciones** y selecciona **Reglas de clasificación**.

5. En el panel **Acciones**, haz clic en **Configurar programación de clasificación**. En la pestaña **Clasificación automática**, selecciona **Habilitar programación fija**, selecciona un **Día de la semana** y, después, activa la casilla **Permitir clasificación continua para archivos nuevos**. Haga clic en **OK**.

6. En el panel **Acciones**, haz clic en **Crear regla de clasificación**. Se abrirá el cuadro de diálogo **Crear regla de clasificación**.

7. En el cuadro **Nombre de regla**, escribe **Alto impacto para la empresa**.

8. En el cuadro **Descripción** , escriba **determina si el documento tiene un alto impacto empresarial en función de la presencia de la cadena "Contoso Confidential"** .

9. En la pestaña **Ámbito**, haz clic en **Establecer propiedades de administración de carpetas**, selecciona **Uso de carpeta**, haz clic en **Agregar** y en **Examinar**, ve a D:\Finance Documents como ruta de acceso, haz clic en **Aceptar** y, después, elige un valor de propiedad llamado **Archivos de grupo** y haz clic en **Cerrar**. Una vez establecidas las propiedades de administración, en la pestaña **Ámbito de regla**, selecciona **Archivos de grupo**.

10. Haga clic en la pestaña **clasificación** .  En **Elija un método para asignar la propiedad a los archivos**, seleccione **clasificador de contenido** en la lista desplegable.

11. En **Elija una propiedad para asignar a los archivos**, selecciona **Impacto** en la lista desplegable.

12. En **Especifique un valor**, selecciona **Alto** en la lista desplegable.

13. Haz clic en **Configurar**, en **Parámetros**.  En el cuadro de diálogo **Parámetros de clasificación**, en la lista **Tipo de expresión**, selecciona **Cadena**. En el cuadro **Expresión** , escribe: **Contoso Confidencial**, y haz clic en **Aceptar**.

14. Haga clic en la pestaña **tipo de evaluación** .  Haga clic en **volver a evaluar los valores de propiedad existentes**, haga clic en **sobrescribir** el valor existente y, a continuación, haga clic en **Aceptar** para finalizar.

![guías de soluciones ](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif) * *_<em>comandos equivalentes de Windows PowerShell</em>_* _

Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.

```
Update-FSRMClassificationPropertyDefinition
$date = Get-Date
$AutomaticClassificationScheduledTask = New-FsrmScheduledTask -Time $date -Weekly @(3, 2, 4, 5,1,6,0) -RunDuration 0;
Set-FsrmClassification -Continuous -schedule $AutomaticClassificationScheduledTask
New-FSRMClassificationRule -Name "High Business Impact" -Property "Impact_MS" -Description "Determines if the document has a high business impact based on the presence of the string 'Contoso Confidential'" -PropertyValue "3000" -Namespace @("D:\Finance Documents") -ClassificationMechanism "Content Classifier" -Parameters @("StringEx=Min=1;Expr=Contoso Confidential") -ReevaluateProperty Overwrite
```

#### <a name="to-create-the-high-pii-classification-rule"></a>Para crear la regla de clasificación de alta PII

1. En el Administrador de Hyper-V, conecta con el servidor ID_AD_FILE1. Inicie sesión en el servidor con Contoso\administrador con la contraseña <strong>pass@word1</strong> .

2. En el escritorio, abra la carpeta denominada _ * expresiones regulares * * y, a continuación, abra el documento de texto llamado **Regex-SSN**. Resalte y copie la siguiente cadena de expresión regular:  **^ (?! 000) ([0-7] \d {2} | 7 ([0-7] \d | 7 [012])) ([-]?) (?! 00) \d\d\3 (?! 0000) \d {4} $**. Esta cadena se usará más adelante en este paso; guárdala en el portapapeles.

3. Abra el Administrador de recursos del servidor de archivos. Para abrir el Administrador de recursos del servidor de archivos haz clic en **Inicio**, escribe **administrador de recursos del servidor de archivos** y haz clic en **Administrador de recursos del servidor de archivos**.

4. En el panel izquierdo del Administrador de recursos del servidor de archivos, expande **Administración de clasificaciones** y selecciona **Reglas de clasificación**.

5. En el panel **Acciones**, haz clic en **Configurar programación de clasificación**. En la pestaña **Clasificación automática**, selecciona **Habilitar programación fija**, selecciona un **Día de la semana** y, después, activa la casilla **Permitir clasificación continua para archivos nuevos**. Haga clic en Aceptar.

6. En el cuadro **Nombre de regla**, escribe **Alta PII**. En el cuadro **Descripción**, escribe **Determina si el documento tiene una alta PII para la empresa en función de la presencia de un número de la Seguridad Social**.

7. Haz clic en la pestaña **Ámbito** y activa la casilla **Archivos de grupo**.

8. Haga clic en la pestaña **clasificación** .  En **Elija un método para asignar la propiedad a los archivos**, seleccione **clasificador de contenido** en la lista desplegable.

9. En **Elija una propiedad para asignar a los archivos**, selecciona **Información de identificación personal** en la lista desplegable.

10. En **Especifique un valor**, selecciona **Alto** en la lista desplegable.

11. Haz clic en **Configurar**, en **Parámetros**.
    En el panel **Parámetros de clasificación**, en la lista **Tipo de expresión** , selecciona **Expresión regular**. En el cuadro **expresión** , pegue el texto del portapapeles: **^ (?! 000) ([0-7] \d {2} | 7 ([0-7] \d | 7 [012])) ([-]?) (?! 00) \d\d\3 (?! 0000) \d {4} $** y, a continuación, haga clic en **Aceptar**.

    > [!NOTE]
    > Esta expresión permitirá números de la Seguridad Social no válidos. Esto nos permite usar números de la Seguridad Social ficticios en la demostración.

12. Haga clic en la pestaña **tipo de evaluación** .  Seleccione **volver a evaluar los valores de propiedad existentes**, **sobrescribir** el valor existente y, a continuación, haga clic en **Aceptar** para finalizar.

![guías de soluciones ](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif) * *_<em>comandos equivalentes de Windows PowerShell</em>_* _

Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.

```
New-FSRMClassificationRule -Name "High PII" -Description "Determines if the document has a high PII based on the presence of a Social Security Number." -Property "PII_MS" -PropertyValue "5000" -Namespace @("D:\Finance Documents") -ClassificationMechanism "Content Classifier" -Parameters @("RegularExpressionEx=Min=1;Expr=^(?!000)([0-7]\d{2}|7([0-7]\d|7[012]))([ -]?)(?!00)\d\d\3(?!0000)\d{4}$") -ReevaluateProperty Overwrite
```

Ahora deberías tener dos reglas de clasificación:

-   Repercusión empresarial alta

-   Alta PII

## <a name="step-3-use-file-management-tasks-to-automatically-protect-documents-with-ad-rms"></a><a name="BKMK_3"></a>Paso 3: Usar tareas de administración de archivos para proteger los documentos automáticamente con AD RMS
Ahora que ha creado reglas para clasificar documentos automáticamente en función del contenido, el siguiente paso es crear una tarea de administración de archivos que use AD RMS para proteger automáticamente determinados documentos en función de su clasificación. En este paso, crearás una tarea de administración de archivos que protege automáticamente todos los documentos con una alta PII. Solo los miembros del grupo FinanceAdmin tendrán acceso a los documentos que contienen PII.

#### <a name="to-protect-documents-with-ad-rms"></a>Para proteger los documentos con AD RMS

1. En el Administrador de Hyper-V, conecta con el servidor ID_AD_FILE1. Inicie sesión en el servidor con Contoso\administrador con la contraseña <strong>pass@word1</strong> .

2. Abra el Administrador de recursos del servidor de archivos. Para abrir Administrador de recursos de servidor de archivos, haga clic en _ * Inicio * *, escriba **Administrador de recursos del servidor de archivos** y, a continuación, haga clic en servidor de **archivos administrador de recursos**.

3. En el panel izquierdo, selecciona **Tareas de administración de archivos**. En el panel **Acciones**, selecciona **Crear tarea de administración de archivos**.

4. En el campo **Nombre de tarea:**, escribe **Alta PII**. En el campo **Descripción**, escribe **Protección automática con RMS para documentos de alta PII**.

5. Haz clic en la pestaña **Ámbito** y activa la casilla **Archivos de grupo**.

6. Haga clic en la pestaña **acción** . En **tipo**, seleccione **cifrado de RMS**. Haz clic en **Examinar** y selecciona la plantilla **Contoso Finance Admin Only**.

7. Haz clic en la pestaña **Condición** y haz clic en **Agregar**. En **Propiedad**, selecciona **Información de identificación personal**. En **Operador**, selecciona **Igual**. En **Valor**, selecciona **Alto**. Haga clic en **OK**.

8. Haga clic en la pestaña **programación** . En la sección **programación** , haga clic en **semanalmente** y, a continuación, seleccione **Sunday**. Al ejecutar la tarea una vez a la semana, te aseguras de capturar los documentos que se puedan haber perdido debido a una interrupción del servicio u otro evento disruptivo.

9. En la sección **Operación continua**, selecciona **Ejecutar continuamente en archivos nuevos** y haz clic en **Aceptar**. Ahora deberías tener una tarea de administración de archivos llamada Alta PII.

![guías de soluciones ](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif) * *_<em>comandos equivalentes de Windows PowerShell</em>_* _

Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.

```
$fmjRmsEncryption = New-FSRMFmjAction -Type 'Rms' -RmsTemplate 'Contoso Finance Admin Only'
$fmjCondition1 = New-FSRMFmjCondition -Property 'PII_MS' -Condition 'Equal' -Value '5000'
$date = get-date
$schedule = New-FsrmScheduledTask -Time $date -Weekly @('Sunday')
$fmj1=New-FSRMFileManagementJob -Name "High PII" -Description "Automatic RMS protection for high PII documents" -Namespace @('D:\Finance Documents') -Action $fmjRmsEncryption -Schedule $schedule -Continuous -Condition @($fmjCondition1)
```

## <a name="step-4-view-the-results"></a><a name="BKMK_4"></a>Paso 4: Ver los resultados
Es el momento de echar un vistazo a las nuevas reglas de clasificación automática y protección de AD RMS en acción. En este paso, examinarás la clasificación de los documentos y observarás cómo cambian cuando cambias el contenido del documento.

#### <a name="to-view-the-results"></a>Para ver los resultados

1. En el Administrador de Hyper-V, conecta con el servidor ID_AD_FILE1. Inicie sesión en el servidor con Contoso\administrador con la contraseña <strong>pass@word1</strong> .

2. En el Explorador de Windows, ve a D:\Finance Documents.

3. Haga clic con el botón secundario en el documento Finance Memo y haga clic en _ * propiedades * *. Haga clic en la pestaña **clasificación** y observe que la propiedad impacto no tiene ningún valor en este momento. Haga clic en **Cancelar**.

4. Haz clic con el botón derecho en el documento **Request for approval to Hire** y selecciona **Propiedades**.

5. Haz clic en la pestaña **Clasificación** y observa que la propiedad **Información de identificación personal** no tiene ningún valor. Haga clic en **Cancelar**.

6. Cambia a CLIENT1. Cierre la sesión de cualquier usuario que haya iniciado sesión y, a continuación, inicie sesión como Contoso\MReid con la contraseña <strong>pass@word1</strong> .

7. En el escritorio, abre la carpeta compartida **Finance Documents**.

8. Abre el documento **Finance Memo**. Cerca de la parte inferior del documento, verás la palabra **Confidential**. Cámbiala por: **Contoso Confidential**. Guarde el documento y ciérrelo.

9. Abre el documento **Request for Approval to Hire**. En la sección **Social Security#:** , escribe: 777-77-7777. Guarde el documento y ciérrelo.

    > [!NOTE]
    > Puede que tengas que esperar 30 segundos para que se produzca la clasificación.

10. Cambia de nuevo a ID_AD_FILE1. En el Explorador de Windows, ve a D:\Finance Documents.

11. Haga clic con el botón derecho en el documento Finance Memo y haz clic en **Propiedades**. Haga clic en la pestaña **clasificación** . Observe que la propiedad **Impact** ahora está establecida en **alta**. Haga clic en **Cancelar**.

12. Haz clic con el botón derecho en el documento Request for Approval to Hire y selecciona **Propiedades**.

13. . Haga clic en la pestaña **clasificación** . Observe que la propiedad **información de identificación personal** ahora está establecida en **alto**. Haga clic en **Cancelar**.

## <a name="step-5-verify-protection-with-ad-rms"></a><a name="BKMK_5"></a>Paso 5: Comprobar la protección con AD RMS

#### <a name="to-verify-that-the-document-is-protected"></a>Para comprobar que el documento está protegido

1.  Cambia de nuevo a ID_AD_CLIENT1.

2.  Abre el documento **Request for approval to Hire**.

3.  Haz clic en **Aceptar** para que el documento se conecte con el servidor de AD RMS.

4.  Ahora verás que el documento ha sido protegido con AD RMS porque contiene un número de la Seguridad Social.


