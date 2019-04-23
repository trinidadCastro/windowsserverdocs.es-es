---
ms.assetid: 01988844-df02-4952-8535-c87aefd8a38a
title: Deploy Automatic File Classification (Demonstration Steps)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 77fb8cc6e13cb82e4d07808c3ae77757a4b2de79
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826786"
---
# <a name="deploy-automatic-file-classification-demonstration-steps"></a>Deploy Automatic File Classification (Demonstration Steps)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema te enseñamos a habilitar propiedades de recurso en Active Directory y a crear reglas de clasificación en el servidor de archivos para, luego, asignar valores a las propiedades de recurso de archivos del servidor de archivos. En este ejemplo, crearemos las siguientes reglas de clasificación:  
  
-   Una regla de clasificación de contenido que busca un conjunto de archivos para la cadena "Contoso confidencial". Si esa cadena se detecta en un archivo, la propiedad de recurso Impact se establece en Alta en dicho archivo.  
  
-   Una regla de clasificación de contenido que busque en un conjunto de archivos una expresión regular que coincida con un número de la Seguridad Social 10 veces como mínimo en un archivo. Si se cumple el patrón, el archivo se clasifica como que tiene información de identificación personal y, como tal, la propiedad de recurso Personally Identifiable Information se establece en Alta.  
  
**En este documento**  
  
-   [Paso 1: Crear definiciones de propiedad de recurso](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step1)  
  
-   [Paso 2: Crear una regla de clasificación de contenido de cadena](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step2)  
  
-   [Paso 3: Crear una regla de clasificación de contenido de expresión regular](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step3)  
  
-   [Paso 4: Compruebe que los archivos se han clasificado](Deploy-Automatic-File-Classification--Demonstration-Steps-.md#BKMK_Step4)  
  
> [!NOTE]  
> Este tema incluye cmdlets de Windows PowerShell de ejemplo que puede usar para automatizar algunos de los procedimientos descritos. Para más información, consulta [Uso de cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_Step1"></a>Paso 1: crear definiciones de propiedad de recurso  
Las propiedades de recurso Impact y Personally Identifiable Information se habilitan para que la infraestructura de clasificación de archivos pueda usarlas para etiquetar los archivos que se analizan en una carpeta compartida de red.  
  
[Realice este paso mediante Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-create-resource-property-definitions"></a>Para crear definiciones de propiedad de recurso  
  
1.  En el controlador de dominio, inicia sesión en el servidor como miembro del grupo de seguridad Admins. del dominio.  
  
2.  Abre el Centro de administración de Active Directory. En Administrador del servidor, haz clic en **Herramientas** y, después, en **Centro de administración de Active Directory**.  
  
3.  Expande **Control de acceso dinámico** y, luego, haz clic en **Propiedades de recurso**.  
  
4.  Haz clic con el botón secundario en **Impact** y, luego, haz clic en **Habilitar**.  
  
5.  Haz clic con el botón secundario en **Personally Identifiable Information** y, luego, haz clic en **Habilitar**.  
  
![guías de soluciones](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
```  
Set-ADResourceProperty '"Enabled:$true '"Identity:'CN=Impact_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'   
Set-ADResourceProperty '"Enabled:$true '"Identity:'CN=PII_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
```  
  
## <a name="BKMK_Step2"></a>Paso 2: crear una regla de clasificación de contenido de cadena  
Una regla de clasificación de contenido de cadena analiza un archivo en busca de una cadena específica. Si la cadena se encuentra, se podrá configurar el valor de una propiedad de recurso. En este ejemplo, se examinará cada archivo en una carpeta compartida de red y busque la cadena "Contoso confidencial". Si la hallamos, el archivo asociado se clasificará como que tiene un gran impacto empresarial.  
  
[Realice este paso mediante Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
#### <a name="to-create-a-string-content-classification-rule"></a>Para crear una regla de clasificación de contenido de cadena  
  
1.  Inicia sesión en el servidor de archivos como miembro del grupo de seguridad Administradores.  
  
2.  En el símbolo del sistema de Windows PowerShell, escribe **Update-FsrmClassificationPropertyDefinition** y presiona ENTRAR. Esto hará que las definiciones de propiedades creadas en el controlador de dominio se sincronicen en el servidor de archivos.  
  
3.  Abra el Administrador de recursos del servidor de archivos. En el Administrador del servidor, haz clic en **Herramientas** y, luego, en **Administrador de recursos del servidor de archivos**.  
  
4.  Expande **Administración de clasificaciones**, haz clic con el botón secundario en **Reglas de clasificación**y, luego, haz clic en **Configurar programación de clasificación**.  
  
5.  Activa la casilla **Habilitar programación fija** y la casilla **Permitir clasificación continua para archivos nuevos**, elige el día de la semana en que quieras ejecutar la clasificación y, luego, haz clic en **Aceptar**.  
  
6.  Haga clic con el botón secundario en **Reglas de clasificación**y, a continuación, en **Crear regla de clasificación**.  
  
7.  En el cuadro **Nombre de regla** de la pestaña **General**, escribe un nombre de regla como, por ejemplo, **Contoso Confidential**.  
  
8.  En la pestaña **Ámbito**, haz clic en **Agregar** y elige las carpetas que deben incluirse en esta regla (por ejemplo, D:\Finance Documents).  
  
    > [!NOTE]  
    > También puedes escoger un nombre dinámico para el ámbito. Para obtener más información sobre los espacios de nombres dinámico para reglas de clasificación, vea [Novedades en el Administrador de recursos del servidor de archivos en Windows Server 2012 \[redirigido\]](assetId:///d53c603e-6217-4b98-8508-e8e492d16083).  
  
9. Configura lo siguiente en la pestaña **Clasificación**:  
  
    -   En el cuadro **Elija un método para asignar una propiedad a los archivos**, procura que **Clasificador de contenido** esté seleccionado.  
  
    -   En el cuadro **Elija una propiedad para asignar a los archivos** , haz clic en **Impact**.  
  
    -   En el cuadro **Especifique un valor** , haz clic en **Alta**.  
  
10. En el encabezado **Parámetros**, haz clic en **Configurar**.  
  
11. En la columna **Tipo de expresión** , selecciona **Cadena**.  
  
12. En la columna **Expresión**, escribe **Contoso Confidential** y haz clic en **Aceptar**.  
  
13. En la pestaña **Tipo de evaluación**, activa la casilla **Volver a evaluar los valores de propiedad existentes**, haz clic en **Sobrescribir el valor existente** y, luego, haz clic en **Aceptar**.  
  
![guías de soluciones](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
```  
$date = Get-Date  
$AutomaticClassificationScheduledTask = New-FsrmScheduledTask -Time $date -Weekly @(3, 2, 4, 5,1,6,0) -RunDuration 0;$AutomaticClassificationScheduledTask  
Set-FsrmClassification -Continuous -schedule $AutomaticClassificationScheduledTask  
New-FSRMClassificationRule -Name 'Contoso Confidential' -Property "Impact_MS" -PropertyValue "3000" -Namespace @('D:\Finance Documents') -ClassificationMechanism "Content Classifier" -Parameters @("StringEx=Min=1;Expr=Contoso Confidential") -ReevaluateProperty Overwrite  
```  
  
## <a name="BKMK_Step3"></a>Paso 3: crear una regla de clasificación de contenido de expresión regular  
Una regla de clasificación de contenido de expresión regular analiza un archivo en busca de un patrón que coincida con una expresión regular. Si una cadena coincide con la expresión regular, se podrá configurar el valor de una propiedad de recurso. En este ejemplo, analizaremos cada archivo de una carpeta compartida de red en busca de una cadena que coincida con el patrón de un número de la Seguridad Social (XXX-XX-XXXX). Si encontramos el patrón, el archivo asociado se clasificará como que tiene información de identificación personal.  
  
[Realice este paso mediante Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep3)  
  
#### <a name="to-create-a-regular-expression-content-classification-rule"></a>Para crear una regla de clasificación de contenido de expresión regular  
  
1.  Inicia sesión en el servidor de archivos como miembro del grupo de seguridad Administradores.  
  
2.  En el símbolo del sistema de Windows PowerShell, escribe **Update-FsrmClassificationPropertyDefinition** y presiona ENTRAR. Esto hará que las definiciones de propiedades creadas en el controlador de dominio se sincronicen en el servidor de archivos.  
  
3.  Abra el Administrador de recursos del servidor de archivos. En el Administrador del servidor, haz clic en **Herramientas** y, luego, en **Administrador de recursos del servidor de archivos**.  
  
4.  Haga clic con el botón secundario en **Reglas de clasificación**y, a continuación, en **Crear regla de clasificación**.  
  
5.  En el cuadro **Nombre de regla** de la pestaña **General** , escribe un nombre para la regla de clasificación como, por ejemplo, PII Rule.  
  
6.  En la pestaña **Ámbito**, haz clic en **Agregar** y elige las carpetas que deben incluirse en esta regla (por ejemplo, D:\Finance Documents).  
  
7.  Configura lo siguiente en la pestaña **Clasificación**:  
  
    -   En el cuadro **Elija un método para asignar una propiedad a los archivos**, procura que **Clasificador de contenido** esté seleccionado.  
  
    -   En el cuadro **Elija una propiedad para asignar a los archivos** , haz clic en **Personally Identifiable Information**.  
  
    -   En el cuadro **Especifique un valor** , haz clic en **Alta**.  
  
8.  En el encabezado **Parámetros**, haz clic en **Configurar**.  
  
9. En la columna **Tipo de expresión** , selecciona **Expresión regular**.  
  
10. En el **expresión** columna, escriba **^ (?!) 000) ([0-7] \d{2}| 7([0-7]\d|7[012])) ([-]?) (?! 00) \d\d\3 (?! 0000) \d{4}$**  
  
11. En la columna **Mínimo de repeticiones** , escribe **10**y haz clic en **Aceptar**.  
  
12. En la pestaña **Tipo de evaluación**, activa la casilla **Volver a evaluar los valores de propiedad existentes**, haz clic en **Sobrescribir el valor existente** y, luego, haz clic en **Aceptar**.  
  
![guías de soluciones](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
```  
New-FSRMClassificationRule -Name "PII Rule" -Property "PII_MS" -PropertyValue "5000" -Namespace @('D:\Finance Documents') -ClassificationMechanism "Content Classifier" -Parameters @("RegularExpressionEx=Min=10;Expr=^(?!000)([0-7]\d{2}|7([0-7]\d|7[012]))([ -]?)(?!00)\d\d\3(?!0000)\d{4}$") -ReevaluateProperty Overwrite  
```  
  
## <a name="BKMK_Step4"></a>Paso 4: comprobar que los archivos se han clasificado correctamente  
Para comprobar que los archivos están correctamente clasificados, puedes ver las propiedades de un archivo que se creó en la carpeta especificada en las reglas de clasificación.  
  
#### <a name="to-verify-that-the-files-are-classified-correctly"></a>Para comprobar que los archivos se han clasificado correctamente  
  
1.  Ejecuta las reglas de clasificación en el servidor de archivos por medio del Administrador de recursos del servidor de archivos.  
  
    1.  Haz clic en **Administración de clasificaciones**, haz clic con el botón secundario en **Reglas de clasificación** y, luego, haz clic en **Ejecutar clasificación con todas las reglas ahora**.  
  
    2.  Haz clic en la opción **Esperar a que termine la clasificación** y, luego, haz clic en **Aceptar**.  
  
    3.  Cierra el informe de clasificación automática.  
  
    4.  Para ello, puedes usar el siguiente comando de Windows PowerShell: **Start-FSRMClassification ' "RunDuration 0 - confirmar: $false**  
  
2.  Ve a la carpeta que se especificó en las reglas de clasificación, como D:\Finance Documents.  
  
3.  Haz clic con el botón secundario en un archivo de esa carpeta y, luego, haz clic en **Propiedades**.  
  
4.  Haz clic en la pestaña **Clasificación** y confirma que el archivo está bien clasificado.  
  
## <a name="BKMK_Links"></a>Vea también  
  
-   [Escenario: Obtenga información sobre los datos mediante el uso de clasificación](Scenario--Get-Insight-into-Your-Data-by-Using-Classification.md)  
  
-   [Planificar la clasificación automática de archivos](https://docs.microsoft.com/previous-versions/orphan-topics/ws.11/jj574209(v%3dws.11))  

  
-   [Control de acceso dinámico: Información general del escenario](Dynamic-Access-Control--Scenario-Overview.md)  
  

