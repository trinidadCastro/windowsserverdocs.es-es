---
ms.assetid: 2c76e81a-c2eb-439f-a89f-7d3d70790244
title: "Implementar el cifrado de archivos de Office (pasos de demostración)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 529000c60a80ee33fc2aa7d09370d8ac1e06311c
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="deploy-encryption-of-office-files-demonstration-steps"></a>Implementar el cifrado de archivos de Office (pasos de demostración)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Contoso Finance Department tiene un número de servidores de archivos que almacenan sus documentos. Estos documentos pueden ser documentación general o pueden tener un impacto empresarial de alto (Repercusión). Por ejemplo, se considera cualquier documento que contiene información confidencial por Contoso, tener un impacto empresarial de alto. Contoso quiere garantizar que toda la documentación de su tiene una cantidad mínima de protección y que su documentación Repercusión está restringido a las personas adecuadas. Para ello, Contoso está explorando utilizando la infraestructura de clasificación de archivo (FCI) y que está disponible en Windows Server 2012 AD RMS. Al usar FCI, Contoso se clasifican todos los documentos en el servidor de archivos, en función del contenido y, a continuación, usar AD RMS para aplicar la directiva de los derechos adecuados.  
  
En este escenario, realizarás los pasos siguientes:  
  
|Tarea|Descripción|  
|--------|---------------|  
|[Habilitar las propiedades de recurso](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_1.1)|Habilitar la **impacto** y **información personal identificable** propiedades de recurso.|  
|[Crear reglas de clasificación](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_2)|Crear las siguientes reglas de clasificación: **regla de clasificación de Repercusión** y **regla de clasificación PII **.|  
|[Usar las tareas de administración de archivos para proteger automáticamente documentos con AD RMS](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_3)|Crear una tarea de administración de archivos que usa automáticamente AD RMS para proteger los documentos con alto información personalmente identificable (PII). Solo los miembros del grupo FinanceAdmin tendrá acceso a los documentos que contienen PII alto.|  
|[Ver los resultados](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_4)|Examinar la clasificación de los documentos y observar cómo cambian a medida que cambia el contenido del documento. También comprueba cómo el documento obtiene protegido por AD RMS.|  
|[Comprobar la protección de AD RMS](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_5)|Comprueba que el documento está protegido con AD RMS.|  
|||  
  
## <a name="BKMK_1.1"></a>Paso 1: Habilitar las propiedades de recurso  
  
#### <a name="to-enable-resource-properties"></a>Para habilitar las propiedades de recurso  
  
1.  En el Administrador de Hyper-V, conecta al servidor ID_AD_DC1. Inicia sesión en el servidor mediante Contoso\Administrador con la contraseña **pass@word1**.  
  
2.  Centro de administración de abrir Active Directory y haz clic en **vista de árbol **.  
  
3.  Expande **CONTROL de acceso dinámico**y selecciona **propiedades de recurso **.  
  
4.  Desplázate hacia abajo hasta la **impacto** propiedad en el **nombre para mostrar** columna. Haz clic en **impacto**y, a continuación, haz clic en **habilitar **.  
  
5.  Desplázate hacia abajo hasta la **información personal identificable** propiedad en el **nombre para mostrar** columna. Haz clic en **información personal identificable**y, a continuación, haz clic en **habilitar **.  
  
6.  Para publicar las propiedades del recurso en el **lista Global de recursos**, en el panel izquierdo, haz clic en **recurso propiedad muestra**y, a continuación, haz doble clic en **lista Global de recursos de propiedad **.  
  
7.  Haz clic en **agregar**y, a continuación, desplácese hacia abajo y haz clic en **impacto** para agregarla a la lista. Haz lo mismo **información personal identificable **. Haz clic en **Aceptar** dos veces para finalizar.  
  
![guías de solución](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  
  
El siguiente cmdlet de Windows PowerShell o cmdlets de realizar la misma función que el procedimiento anterior. Especifique cada cmdlet en una sola línea, aunque pueden aparecer ajustados en varias líneas debido a restricciones de formato.  
  
```  
Set-ADResourceProperty -Enabled:$true -Identity:"CN=Impact_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com"  
Set-ADResourceProperty -Enabled:$true -Identity:"CN=PII_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com" 
```  
  
## <a name="BKMK_2"></a>Paso 2: Crear las reglas de clasificación  
Este paso explica cómo crear el **gran impacto** regla de clasificación. Esta regla buscará el contenido de documentos y si se encuentra la cadena "Información confidencial de Contoso", clasificará en este documento como tener impacto empresarial de alto. Esta clasificación invalidará cualquier clasificación asignado previamente del impacto empresarial de baja.  
  
También creará una **PII alto** regla. Esta regla busca el contenido de documentos, y si se encuentra un número de la seguridad Social, clasifica el documento como si tuvieran PII alto.  
  
#### <a name="to-create-the-high-impact-classification-rule"></a>Para crear la regla de clasificación de gran impacto  
  
1.  En el Administrador de Hyper-V, conecta al servidor ID_AD_FILE1. Inicia sesión en el servidor mediante Contoso\Administrador con la contraseña **pass@word1**.  
  
2.  Necesitas actualizar las propiedades del recurso Global de Active Directory. Abre Windows PowerShell y escribe: `Update-FSRMClassificationPropertyDefinition`, y, a continuación, presione ENTRAR. Cierra Windows PowerShell.  
  
3.  Abre el Administrador de recursos del servidor de archivos. Para abrir el Administrador de recursos del servidor de archivos, haz clic en **inicio**, tipo **Administrador de recursos del servidor de archivos**y, a continuación, haz clic en **Administrador de recursos del servidor de archivos**.  
  
4.  En el panel izquierdo del Administrador de recursos del servidor de archivos, expanda **administración de clasificaciones**y, a continuación, selecciona **reglas de clasificación **.  
  
5.  En la **acciones** panel, haz clic en **configurar la programación de clasificación **. En la **clasificación automática**, selecciona **Habilitar programación fija**, selecciona un **día de la semana**y, a continuación, selecciona el **permitir clasificación continua para los nuevos archivos** casilla de verificación. Haz clic en **Aceptar**.  
  
6.  En la **acciones** panel, haz clic en **crear la regla de clasificación **. Se abrirá la **crear la regla de clasificación** cuadro de diálogo.  
  
7.  En la **nombre de la regla**, escriba **alto impacto empresarial **.  
  
8.  En la **descripción**, escriba **determina si el documento tiene un impacto empresarial alto en función de la presencia de la cadena "Información confidencial de Contoso"**  
  
9. En la **ámbito**, haga clic **establecer propiedades de administración de la carpeta**, selecciona **uso de las carpetas**, haz clic en **agregar**, a continuación, haz clic en **examinar**, ve a documentos D:\Finance como la ruta de acceso, haz clic en **Aceptar**y, a continuación, elige un valor de propiedad denominado **agrupar archivos** y haz clic en **cerrar **. Una vez que se establecen las propiedades de administración, en la **ámbito de regla** ficha seleccione **agrupar archivos **.  
  
10. Haz clic en el **clasificación** pestaña.  En **elegir un método para asignar la propiedad a archivos**, selecciona **clasificador contenido** de la lista desplegable.  
  
11. En **elegir una propiedad para asignar a los archivos**, selecciona **impacto** de la lista desplegable.  
  
12. En **especificar un valor**, selecciona **alto** de la lista desplegable.  
  
13. Haz clic en **configurar** en **parámetros **.  En la **parámetros de clasificación** cuadro de diálogo, en la **tipo de expresión** lista, selecciona **cadena **. En la **expresión**, escriba: **Contoso confidencial**y, a continuación, haz clic en **Aceptar **.  
  
14. Haz clic en el **evaluación tipo** pestaña.  Haz clic en **volver a evaluar los valores de propiedad**, haz clic en **sobrescribir**la existente valor y, a continuación, haz clic en **Aceptar** a fin.  
  
![guías de solución](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  
  
El siguiente cmdlet de Windows PowerShell o cmdlets de realizar la misma función que el procedimiento anterior. Especifique cada cmdlet en una sola línea, aunque pueden aparecer ajustados en varias líneas debido a restricciones de formato.  
  
```  
Update-FSRMClassificationPropertyDefinition  
$date = Get-Date  
$AutomaticClassificationScheduledTask = New-FsrmScheduledTask -Time $date -Weekly @(3, 2, 4, 5,1,6,0) -RunDuration 0;  
Set-FsrmClassification -Continuous -schedule $AutomaticClassificationScheduledTask  
New-FSRMClassificationRule -Name "High Business Impact" -Property "Impact_MS" -Description "Determines if the document has a high business impact based on the presence of the string 'Contoso Confidential'" -PropertyValue "3000" -Namespace @("D:\Finance Documents") -ClassificationMechanism "Content Classifier" -Parameters @("StringEx=Min=1;Expr=Contoso Confidential") -ReevaluateProperty Overwrite  
```  
  
#### <a name="to-create-the-high-pii-classification-rule"></a>Para crear la regla de clasificación de alto PII  
  
1.  En el Administrador de Hyper-V, conecta al servidor ID_AD_FILE1. Inicia sesión en el servidor mediante Contoso\Administrador con la contraseña **pass@word1**.  
  
2.  En el escritorio, abra la carpeta llamada **expresiones regulares**y, a continuación, abre el documento de texto denominado **RegEx NSS **. Resaltar y copiar la siguiente cadena de expresión regular: **^ (! 000)([0-7]\d{2}|7([0-7]\d|7[012])) ([-]?) (?! ¡00) \d\d\3 (! \d 0000) {4} $**. Esta cadena se usará más adelante en este paso por lo tanto, guardarla en el Portapapeles.  
  
3.  Abre el Administrador de recursos del servidor de archivos. Para abrir el Administrador de recursos del servidor de archivos, haz clic en **inicio**, tipo **Administrador de recursos del servidor de archivos**y, a continuación, haz clic en **Administrador de recursos del servidor de archivos**.  
  
4.  En el panel izquierdo del Administrador de recursos del servidor de archivos, expanda **administración de clasificaciones**y, a continuación, selecciona **reglas de clasificación **.  
  
5.  En la **acciones** panel, haz clic en **configurar la programación de clasificación **. En la **clasificación automática**, selecciona **Habilitar programación fija**, selecciona un **día de la semana**y, a continuación, selecciona el **permitir clasificación continua para los nuevos archivos** casilla de verificación. Haz clic en Aceptar.  
  
6.  En la **nombre de la regla**, escriba **PII alto **. En la **descripción**, escriba **determina si el documento tiene un alto PII en función de la presencia de un número de la seguridad Social.**  
  
7.  Haz clic en el **ámbito**, selecciona el **agrupar archivos** casilla de verificación.  
  
8.  Haz clic en el **clasificación** pestaña.  En **elegir un método para asignar la propiedad a archivos**, selecciona **clasificador contenido** de la lista desplegable.  
  
9. En **elegir una propiedad para asignar a los archivos**, selecciona **información personal identificable** de la lista desplegable.  
  
10. En **especificar un valor**, selecciona **alto** de la lista desplegable.  
  
11. Haz clic en **configurar** en **parámetros **.   
    En la **parámetros de clasificación**ventana, en la **tipo de expresión** lista, selecciona **expresión Regular **. En la **expresión** cuadro, pegar el texto desde el Portapapeles: **^ (! 000)([0-7]\d{2}|7([0-7]\d|7[012])) ([-]?) (?! ¡00) \d\d\3 (! \d 0000) {4} $**y, a continuación, haz clic en **Aceptar **.  
  
    > [!NOTE]  
    > Esta expresión permitirá números de la seguridad Social no válidos. Esto nos permite usar números de la seguridad Social ficticios en la demostración.  
  
12. Haz clic en el **evaluación tipo** pestaña.  Selecciona **volver a evaluar los valores de propiedad**, **sobrescribir**la existente valor y, a continuación, haz clic en **Aceptar** a fin.  
  
![guías de solución](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  
  
El siguiente cmdlet de Windows PowerShell o cmdlets de realizar la misma función que el procedimiento anterior. Especifique cada cmdlet en una sola línea, aunque pueden aparecer ajustados en varias líneas debido a restricciones de formato.  
  
```  
New-FSRMClassificationRule -Name "High PII" -Description "Determines if the document has a high PII based on the presence of a Social Security Number." -Property "PII_MS" -PropertyValue "5000" -Namespace @("D:\Finance Documents") -ClassificationMechanism "Content Classifier" -Parameters @("RegularExpressionEx=Min=1;Expr=^(?!000)([0-7]\d{2}|7([0-7]\d|7[012]))([ -]?)(?!00)\d\d\3(?!0000)\d{4}$") -ReevaluateProperty Overwrite  
```  
  
Ahora debería tener dos reglas de clasificación:  
  
-   Impacto empresarial alto  
  
-   PII alto  
  
## <a name="BKMK_3"></a>Paso 3: Usar las tareas de administración de archivos para proteger automáticamente documentos con AD RMS  
Ahora que has creado las reglas para clasificar automáticamente los documentos en función de contenido, el siguiente paso es crear una tarea de administración de archivos que se usa AD RMS para proteger automáticamente determinados documentos en función de su clasificación. En este paso, vas a crear una tarea de administración de archivos que protege automáticamente los documentos con un alto PII. Solo los miembros del grupo FinanceAdmin tendrá acceso a los documentos que contienen PII alto.  
  
#### <a name="to-protect-documents-with-ad-rms"></a>Para proteger los documentos con AD RMS  
  
1.  En el Administrador de Hyper-V, conecta al servidor ID_AD_FILE1. Inicia sesión en el servidor mediante Contoso\Administrador con la contraseña **pass@word1**.  
  
2.  Abre el Administrador de recursos del servidor de archivos. Para abrir el Administrador de recursos del servidor de archivos, haz clic en **inicio**, tipo **Administrador de recursos del servidor de archivos**y, a continuación, haz clic en **Administrador de recursos del servidor de archivos**.  
  
3.  En el panel izquierdo, selecciona **tareas de administración de archivos **. En la **acciones** panel, selecciona **crear la tarea de administración de archivos **.  
  
4.  En la **nombre de la tarea:**, escriba **PII alto **. En la **descripción**, escriba **protección de RMS automática de documentos alta PII **.  
  
5.  Haz clic en el **ámbito**, selecciona el **agrupar archivos** casilla de verificación.  
  
6.  Haz clic en el **acción** pestaña. En **tipo**, selecciona **RMS cifrado **. Haz clic en **examinar** para seleccionar una plantilla y, a continuación, selecciona el **Contoso Finance administrador solo** plantilla.  
  
7.  Haz clic en el **condición** pestaña y, a continuación, haz clic en **agregar **. En **propiedad**, selecciona **información personal identificable **. En **operador**, selecciona **igual **. En **valor**, selecciona **alto **. Haz clic en **Aceptar**.  
  
8.  Haz clic en el **programación** pestaña. En la **programación** sección, haz clic en **semanales**y, a continuación, selecciona **el domingo **. Ejecutar la tarea una vez a la semana, garantizará que capturar todos los documentos que pueden haberse perdidos debido a una interrupción del servicio o a otro evento perjudicial.  
  
9. En la **operación continua** sección, selecciona **ejecutar archivos de nuevo continuamente en tareas**y, a continuación, haz clic en **Aceptar **. Ahora debería tener una tarea de administración de archivos llamada PII alto.  
  
![guías de solución](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  
  
El siguiente cmdlet de Windows PowerShell o cmdlets de realizar la misma función que el procedimiento anterior. Especifique cada cmdlet en una sola línea, aunque pueden aparecer ajustados en varias líneas debido a restricciones de formato.  
  
```  
$fmjRmsEncryption = New-FSRMFmjAction -Type 'Rms' -RmsTemplate 'Contoso Finance Admin Only'  
$fmjCondition1 = New-FSRMFmjCondition -Property 'PII_MS' -Condition 'Equal' -Value '5000'  
$date = get-date  
$schedule = New-FsrmScheduledTask -Time $date -Weekly @('Sunday')    
$fmj1=New-FSRMFileManagementJob -Name "High PII" -Description "Automatic RMS protection for high PII documents" -Namespace @('D:\Finance Documents') -Action $fmjRmsEncryption -Schedule $schedule -Continuous -Condition @($fmjCondition1)  
```  
  
## <a name="BKMK_4"></a>Paso 4: Ver los resultados  
Es el momento de echa un vistazo a la nueva clasificación automática y las reglas de protección de AD RMS en acción. En este paso se examinará la clasificación de los documentos y observar cómo cambian a medida que cambia el contenido del documento.  
  
#### <a name="to-view-the-results"></a>Para ver los resultados  
  
1.  En el Administrador de Hyper-V, conecta al servidor ID_AD_FILE1. Inicia sesión en el servidor mediante Contoso\Administrador con la contraseña **pass@word1**.  
  
2.  En el Explorador de Windows, ve a documentos D:\Finance.  
  
3.  Haz clic en el documento docs y haz clic en **propiedades**. Haz clic en el **clasificación** pestaña y observe que la propiedad impacto tiene actualmente ningún valor. Haz clic en **cancelar **.  
  
4.  Haz clic en el **solicitud de aprobación en el documento de alquiler**y, a continuación, selecciona **propiedades **.  
  
5.  Haz clic en el **clasificación** pestaña y ten en cuenta que **la propiedad de la información de identificación personal** actualmente no tiene ningún valor. Haz clic en **cancelar **.  
  
6.  Cambiar a CLIENTE1. Inicia sesión desactivar cualquier usuario que ha iniciado sesión y, a continuación, inicia sesión como Contoso\MReid con la contraseña **pass@word1**.  
  
7.  En el escritorio, abra el **Finanzas documentos** carpeta compartida.  
  
8.  Abre el **docs** documento. En la parte inferior del documento, verá la palabra **confidencial **. Modificarla para leer: **Contoso confidencial **. Guardar el documento y cerrarla.  
  
9. Abre el **solicitud de aprobación para contratación** documento. En la **sociales #:**, escriba: 777-77-7777. Guardar el documento y cerrarla.  
  
    > [!NOTE]  
    > Es podrán que tengas que esperar 30 segundos para la clasificación para que se produzca.  
  
10. Cambiar a ID_AD_FILE1. En el Explorador de Windows, ve a documentos D:\Finance.  
  
11. Haz clic en el documento docs y haz clic en **propiedades **. Haz clic en el **clasificación** pestaña. Ten en cuenta que la **impacto** ahora se establece la propiedad en **alto **. Haz clic en **cancelar **.  
  
12. Haz clic en la solicitud de aprobación contratación documento y haga clic en **propiedades **.  
  
13. . Haz clic en el **clasificación** pestaña. Ten en cuenta que la **información personal identificable** ahora se establece la propiedad en **alto **. Haz clic en **cancelar **.  
  
## <a name="BKMK_5"></a>Paso 5: Comprobar la protección con AD RMS  
  
#### <a name="to-verify-that-the-document-is-protected"></a>Para comprobar que el documento está protegido  
  
1.  Cambiar a ID_AD_CLIENT1.  
  
2.  Abre el **solicitud de aprobación para contratación** documento.  
  
3.  Haz clic en **Aceptar** para permitir que el documento para conectarse a tu servidor de AD RMS.  
  
4.  Ahora puedes ver que el documento se ha protegido por AD RMS porque contiene un número de la seguridad Social.  
  

