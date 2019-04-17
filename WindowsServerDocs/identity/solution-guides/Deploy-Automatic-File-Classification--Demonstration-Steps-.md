---
ms.assetid: 01988844-df02-4952-8535-c87aefd8a38a
title: "Implementar la clasificación de archivo automático (pasos de demostración)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1c5c0fa221e0d7375216426f838ba37bee852984
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-automatic-file-classification-demonstration-steps"></a>Implementar la clasificación de archivo automático (pasos de demostración)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se explica cómo habilitar las propiedades de recurso en Active Directory, crear reglas de clasificación en el servidor de archivos y, a continuación, asignar valores a las propiedades del recurso de archivos en el servidor de archivos. En este ejemplo, se crean las siguientes reglas de clasificación:  
  
-   Una regla de clasificación de contenido que busca un conjunto de archivos para la cadena 'Contoso confidencial'. Si la cadena se encuentra en un archivo, se establece la propiedad del recurso impacto en alto en el archivo.  
  
-   Una regla de clasificación de contenido que busca un conjunto de archivos para una expresión regular que coincida con un número de la seguridad social al menos 10 veces en un archivo. Si se encuentra el patrón, el archivo se clasifica como información de identificación personal y la propiedad de recursos de la información de identificación personal está configurada como alta.  
  
**En este documento**  
  
-   [Paso 1: Crear recurso de definiciones de propiedad](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step1)  
  
-   [Paso 2: Crear una regla de clasificación de contenido de cadena](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step2)  
  
-   [Paso 3: Crear una regla de clasificación de contenido de la expresión regular](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step3)  
  
-   [Paso 4: Comprobar que se clasifican los archivos](Deploy-Automatic-File-Classification--Demonstration-Steps-.md#BKMK_Step4)  
  
> [!NOTE]  
> Este tema incluye cmdlets de Windows PowerShell de muestra que puedes usar para automatizar algunos de los procedimientos descritos. Para obtener más información, consulta [uso de Cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_Step1"></a>Paso 1: Crear recurso de definiciones de propiedad  
Para que la infraestructura de clasificación de archivo pueda usar estas propiedades de recurso para etiquetar los archivos que se analizan en una carpeta compartida de red, se habilitan las propiedades del recurso impacto y la información de identificación personal.  
  
[Este paso mediante Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-create-resource-property-definitions"></a>Crear recurso de definiciones de propiedad  
  
1.  En el controlador de dominio, inicie sesión en el servidor como miembro del grupo Administradores de dominio.  
  
2.  Abre el centro de administración de Active Directory. En el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **centro de administración de Active Directory**.  
  
3.  Expande **Control de acceso dinámico**y, a continuación, haz clic en **propiedades de recurso**.  
  
4.  Haz clic en **impacto**y, a continuación, haz clic en **habilitar **.  
  
5.  Haz clic en **información personal identificable**y, a continuación, haz clic en **habilitar **.  
  
![guías de solución](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  
  
El siguiente cmdlet de Windows PowerShell o cmdlets de realizar la misma función que el procedimiento anterior. Especifique cada cmdlet en una sola línea, aunque pueden aparecer ajustados en varias líneas debido a restricciones de formato.  
  
```  
Set-ADResourceProperty '"Enabled:$true '"Identity:'CN=Impact_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'   
Set-ADResourceProperty '"Enabled:$true '"Identity:'CN=PII_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
```  
  
## <a name="BKMK_Step2"></a>Paso 2: Crear una regla de clasificación de contenido de cadena  
Una regla de clasificación de contenido de la cadena busca una cadena específica. Si se encuentra la cadena, se puede configurar el valor de propiedad de un recurso. En este ejemplo, se examinará cada archivo en una carpeta compartida de red y busca la cadena 'Contoso confidencial'. Si se encuentra la cadena, el archivo asociado se clasifica como tener un impacto empresarial alto.  
  
[Este paso mediante Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
#### <a name="to-create-a-string-content-classification-rule"></a>Para crear una regla de clasificación de contenido de cadena  
  
1.  Iniciar sesión como miembro del grupo de seguridad de los administradores el servidor de archivos.  
  
2.  En el símbolo del sistema de Windows PowerShell, escribe **actualización FsrmClassificationPropertyDefinition** y, a continuación, presione ENTRAR. Esto sincronizará las definiciones de propiedad que se crean en el controlador de dominio al servidor de archivos.  
  
3.  Abre el Administrador de recursos del servidor de archivos. En el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **Administrador de recursos del servidor de archivos**.  
  
4.  Expande **administración de clasificaciones**, haz clic en **reglas de clasificación**y, a continuación, haz clic en **configurar la programación de clasificación**.  
  
5.  Selecciona el **Habilitar programación fija** casilla de verificación, selecciona el **permitir clasificación continua para los nuevos archivos** casilla de verificación, elige un día de la semana para ejecutar la clasificación y, a continuación, haz clic en **Aceptar**.  
  
6.  Haz clic en **reglas de clasificación**y, a continuación, haz clic en **crear la regla de clasificación**.  
  
7.  En la **General** pestaña la **nombre de la regla**, escriba un nombre de regla como **Contoso confidencial**.  
  
8.  En la **ámbito**, haga clic **agregar**y elige las carpetas que deberían estar incluidas en esta regla, como documentos D:\Finance.  
  
    > [!NOTE]  
    > También puede elegir un espacio de nombres dinámicas para el ámbito. Para obtener más información acerca de los espacios de nombres dinámico para reglas de clasificación, consulta [Novedades en el Administrador de recursos del servidor de archivos en Windows Server 2012 \[redirected\]](assetId:///d53c603e-6217-4b98-8508-e8e492d16083).  
  
9. En la **clasificación** pestaña, configura lo siguiente:  
  
    -   En la **elegir un método para asignar una propiedad a archivos** cuadro, asegúrate de que **clasificador contenido** está seleccionado.  
  
    -   En la **elegir una propiedad para asignar a los archivos** cuadro, haz clic en **impacto**.  
  
    -   En la **especificar un valor** cuadro, haz clic en **alto**.  
  
10. En la **parámetros** rumbo, haz clic en **configurar**.  
  
11. En la **tipo de expresión** columna, selecciona **cadena**.  
  
12. En la **expresión** columna, escribe **Contoso confidencial**y, a continuación, haz clic en **Aceptar**.  
  
13. En la **evaluación tipo**, selecciona el **volver a evaluar los valores de propiedad** casilla de verificación, haz clic en **sobrescribir el valor existente**y, a continuación, haz clic en **Aceptar**.  
  
![guías de solución](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  
  
El siguiente cmdlet de Windows PowerShell o cmdlets de realizar la misma función que el procedimiento anterior. Especifique cada cmdlet en una sola línea, aunque pueden aparecer ajustados en varias líneas debido a restricciones de formato.  
  
```  
$date = Get-Date  
$AutomaticClassificationScheduledTask = New-FsrmScheduledTask -Time $date -Weekly @(3, 2, 4, 5,1,6,0) -RunDuration 0;$AutomaticClassificationScheduledTask  
Set-FsrmClassification -Continuous -schedule $AutomaticClassificationScheduledTask  
New-FSRMClassificationRule -Name 'Contoso Confidential' -Property "Impact_MS" -PropertyValue "3000" -Namespace @('D:\Finance Documents') -ClassificationMechanism "Content Classifier" -Parameters @("StringEx=Min=1;Expr=Contoso Confidential") -ReevaluateProperty Overwrite  
```  
  
## <a name="BKMK_Step3"></a>Paso 3: Crear una regla de clasificación de contenido de la expresión regular  
Una regla de clasificación de la expresión regular busca un patrón que coincide con la expresión regular. Si se encuentra una cadena que coincida con la expresión regular, se puede configurar el valor de propiedad de un recurso. En este ejemplo, se examinará cada archivo en una carpeta compartida de red y busca una cadena que coincida con el patrón de un número de la seguridad social (XXX-XX-XXXX). Si se encuentra el patrón, el archivo asociado se clasifica como información de identificación personal.  
  
[Este paso mediante Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep3)  
  
#### <a name="to-create-a-regular-expression-content-classification-rule"></a>Para crear una regla de clasificación de contenido de la expresión regular  
  
1.  Iniciar sesión como miembro del grupo de seguridad de los administradores el servidor de archivos.  
  
2.  En el símbolo del sistema de Windows PowerShell, escribe **actualización FsrmClassificationPropertyDefinition**, y, a continuación, presione ENTRAR. Esto sincronizará las definiciones de propiedad que se crean en el controlador de dominio al servidor de archivos.  
  
3.  Abre el Administrador de recursos del servidor de archivos. En el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **Administrador de recursos del servidor de archivos**.  
  
4.  Haz clic en **reglas de clasificación**y, a continuación, haz clic en **crear la regla de clasificación**.  
  
5.  En la **General** pestaña la **nombre de la regla**, escriba un nombre para la regla de clasificación, como PII regla.  
  
6.  En la **ámbito**, haga clic **agregar**y, a continuación, elige las carpetas que deberían estar incluidas en esta regla, como documentos D:\Finance.  
  
7.  En la **clasificación** pestaña, configura lo siguiente:  
  
    -   En la **elegir un método para asignar una propiedad a archivos** cuadro, asegúrate de que **clasificador contenido** está seleccionado.  
  
    -   En la **elegir una propiedad para asignar a los archivos** cuadro, haz clic en **información personal identificable**.  
  
    -   En la **especificar un valor** cuadro, haz clic en **alto**.  
  
8.  En la **parámetros** rumbo, haz clic en **configurar**.  
  
9. En la **tipo de expresión** columna, selecciona **expresión Regular**.  
  
10. En la **expresión** columna, escribe **^ (! 000)([0-7]\d{2}|7([0-7]\d|7[012])) ([-]?) (?! ¡00) \d\d\3 (! 0000) \d {4} $**  
  
11. En la **mínimo repeticiones** columna, escribe **10**y, a continuación, haz clic en **Aceptar**.  
  
12. En la **evaluación tipo**, selecciona el **volver a evaluar los valores de propiedad** casilla de verificación, haz clic en **sobrescribir el valor existente**y, a continuación, haz clic en **Aceptar**.  
  
![guías de solución](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  
  
El siguiente cmdlet de Windows PowerShell o cmdlets de realizar la misma función que el procedimiento anterior. Especifique cada cmdlet en una sola línea, aunque pueden aparecer ajustados en varias líneas debido a restricciones de formato.  
  
```  
New-FSRMClassificationRule -Name "PII Rule" -Property "PII_MS" -PropertyValue "5000" -Namespace @('D:\Finance Documents') -ClassificationMechanism "Content Classifier" -Parameters @("RegularExpressionEx=Min=10;Expr=^(?!000)([0-7]\d{2}|7([0-7]\d|7[012]))([ -]?)(?!00)\d\d\3(?!0000)\d{4}$") -ReevaluateProperty Overwrite  
```  
  
## <a name="BKMK_Step4"></a>Paso 4: Comprobar que los archivos se clasifican correctamente  
Puedes comprobar que los archivos se clasifican correctamente mediante la visualización de las propiedades de un archivo que se creó en la carpeta especificada en las reglas de clasificación.  
  
#### <a name="to-verify-that-the-files-are-classified-correctly"></a>Para comprobar que los archivos se clasifican correctamente  
  
1.  En el servidor de archivos, ejecutar las reglas de clasificación mediante el Administrador de recursos del servidor de archivos.  
  
    1.  Haz clic en **administración de clasificaciones**, haz clic en **reglas de clasificación**y, a continuación, haz clic en **ejecutar clasificación con todas las reglas ahora**.  
  
    2.  Haz clic en el **esperar de clasificación completar** opción y, a continuación, haz clic en **Aceptar**.  
  
    3.  Cierre el informe de clasificación automática.  
  
    4.  Puede hacerlo mediante Windows PowerShell con el siguiente comando: **inicio FSRMClassification ' "RunDuration 0 - confirmar: $false**  
  
2.  Navega a la carpeta que se especificó en las reglas de clasificación, como documentos D:\Finance.  
  
3.  Haz clic en un archivo en esa carpeta y, a continuación, haz clic en **propiedades**.  
  
4.  Haz clic en el **clasificación** pestaña y comprueba que el archivo se clasifica correctamente.  
  
## <a name="BKMK_Links"></a>Consulta también  
  
-   [Escenario: Obtener información sobre los datos mediante el uso de clasificación](Scenario--Get-Insight-into-Your-Data-by-Using-Classification.md)  
  
-   [Planear la clasificación automática de archivos](assetId:///e3c3bb4b-3034-42b7-b391-8ef5f5851955)  
  
-   [Control de acceso dinámico: Información general del escenario](Dynamic-Access-Control--Scenario-Overview.md)  
  

