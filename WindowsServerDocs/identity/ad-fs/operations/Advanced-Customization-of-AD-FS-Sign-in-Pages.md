---
ms.assetid: 882abec8-0189-4f73-99c5-792987168080
title: "Personalización de AD FS sesión páginas avanzada"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/13/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ea01d0ff2a38c4fef2f68091608d777d8412e91b
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>Personalización de AD FS sesión páginas avanzada

>Se aplica a: Windows Server 2016, Windows Server 2012 R2
  
## <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>Personalización avanzada de AD FS Sign\ en páginas  
AD FS en Windows Server 2012 R2 proporciona built\ de soporte técnico para personalizar la experiencia de sign\. Para la mayoría de estos escenarios, los cmdlets de Windows PowerShell en built\ son todo lo que es necesario.  Se recomienda que uses los comandos de Windows PowerShell en built\ para personalizar los elementos estándar de AD FS sign\ experiencia siempre que sea posible.  Consulta [personalización de inicio de sesión del usuario de AD FS](AD-FS-user-sign-in-customization.md) para obtener más información.  
  
En algunos casos, los administradores de AD FS quiera proporcionar experiencias de inicio de sesión sign\ adicionales que no son posibles mediante los comandos de PowerShell existentes que se envían in\-box con AD FS. En determinados casos, es factible \ (dentro de las directrices incluidas below\) para los administradores personalizar aún más la experiencia en sign\ mediante la adición de lógica adicional para **onload.js** que se proporciona por AD FS y se ejecutará en todas las páginas de AD FS.  
  
## <a name="things-to-know-before-you-start"></a>Cosas que debes saber antes de empezar  
  
-   Cualquier cambio que afecta a los flujos de redirección o modifica los parámetros de protocolo que AD FS funciona con no es compatible.
  
-   El onload.js original, uno que viene con el tema de web predeterminado, contiene código que controla el procesamiento de página para diferentes factores de forma. Se recomienda no modifica el contenido de onload.js original, pero solo anexar el código a la onload.js existente que controla la lógica personalizada.  
  
-   AD FS incluye un tema built\ en web que se llama de forma predeterminada. No se puede modificar la onload.js del tema de web predeterminado. Para actualizar onload.js, tienes que crear y usar un tema personalizado web para AD FS sign\ en páginas.  Consulta [personalización de inicio de sesión del usuario de AD FS](AD-FS-user-sign-in-customization.md) para obtener información sobre cómo crear un tema personalizado web.  
  
-   El mismo onload.js se ejecutará en todos los ADFS páginas \(ex. página de inicio de sesión basado en form\, página de detección de dominio de inicio y etcetera. \). Debes asegurarte de que se ejecute el código en el script solo tal como se ha diseñado y no se ejecuta inesperadamente.  
  
-   Al hacer referencia a cualquier elemento HTML, asegúrate de que siempre comprobar la existencia del elemento antes de actúa en el elemento. Esto proporciona solidez y garantiza que no se ejecutaría la lógica personalizada en las páginas que no contienen este elemento. Simplemente, puede ver el código fuente HTML en las páginas de sign\ de AD FS para ver los elementos existentes.  
  
-   Se recomienda encarecidamente para validar las personalizaciones en un entorno alternativo y probarlas antes de quitarla incorporar en producción servidores de AD FS. Esto reduce la posibilidad de que los usuarios finales que se exponen a estas personalizaciones antes de la validación.  
  
## <a name="customizing-the-ad-fs-sign-in-experience-by-using-onloadjs"></a>Personalizar la experiencia de sign\ AD FS mediante onload.js  
Usa los siguientes pasos cuando quieras personalizar la onload.js para el servicio de AD FS.  
  
#### <a name="customizing-onloadjs-for-the-ad-fs-service"></a>Personalizar onload.js para el servicio AD FS  
  
1.  Para agregar la lógica personalizada para onload.js, debes crear primero un tema personalizado web. El tema que esté enviada out\-descripción\-the\-box se llama de forma predeterminada. Puedes exportar el tema predeterminado y usarlo para que pueda comenzar rápidamente. El siguiente cmdlet crea un tema de web personalizado, lo que duplica el tema de web predeterminado:  
  
    ```  
    New-AdfsWebTheme –Name custom –SourceName default  
  
    ```  
  
2.  A continuación, puede exportar personalizado o web tema para obtener el archivo onload.js predeterminado. Para exportar un tema de la web, usa el cmdlet siguiente:  
  
    ```  
    Export-AdfsWebTheme –Name default –DirectoryPath c:\theme  
  
    ```  
  
    Encontrarás onload.js en la carpeta de script en el directorio que especificas en el cmdlet de exportación anterior y agregar la lógica personalizada para el script \ (consulta casos de uso en el below\ de sección de ejemplo).  
  
3.  Hacer la modificación necesaria para personalizar onload.js según sus necesidades.  
  
4.  Actualizar el tema con el onload.js modificado. Usa el siguiente cmdlet para aplicar la actualización onload.js a tema personalizado web:  
  
    ```  
    Set-AdfsWebTheme -TargetName custom -AdditionalFileResource @{Uri=’/adfs/portal/script/onload.js’;path="c:\theme\script\onload.js"}  
  
    ```  
  
5.  Para aplicar el tema personalizado web a AD FS, usa el cmdlet siguiente:  
  
    ```  
    Set-AdfsWebConfig -ActiveThemeName custom  
    ```  
  
## <a name="additional-customization-examples"></a>Ejemplos de personalización adicionales  
El siguiente es los ejemplos de código personalizado que se agregan a onload.js fine\-tune diferentes fines. Al agregar el código personalizado, ten siempre anexar el código personalizado a la parte inferior de la onload.js.  
  
### <a name="example-1-change-sign-in-with-organizational-account-string"></a>Ejemplo 1: cambiar la cadena "Que iniciar sesión con cuenta corporativa"  
La AD FS basado en form\ sign\ en página predeterminada tiene un título de "Iniciar sesión con tu cuenta corporativa" encima de los cuadros de entrada del usuario.  
  
Si quieres reemplazar esta cadena con su propia cadena, puedes agregar el siguiente código a onload.js.  
  
```  
// Sample code to change “Sign in with organizational account” string.  
  
// Check whether the loginMessage element is present on this page.  
var loginMessage = document.getElementById('loginMessage');  
if (loginMessage)  
{  
       // loginMessage element is present, modify its properties.  
       loginMessage.innerHTML = 'Your company description text';  
}  
  
```  
  
### <a name="example-2-accept-sam-account-name-as-a-login-format-on-an-ad-fs-form-based-sign-in-page"></a>Ejemplo 2: acepta el nombre de cuenta de SAM\ como un formato de inicio de sesión en una AD FS form\ sign\ de página  
El formato de AD FS basado en form\ sign\ en la página es compatible con inicio de sesión de dominio o nombres de usuario Principal \(UPNs\) \(for example, ** johndoe@contoso.com **\) predeterminado completo nombres de cuenta de sam\ \ (**contoso\\johndoe** o **contoso.com\\johndoe**\). En caso de que todos los usuarios llegan desde el mismo dominio y solo sepa sobre los nombres de cuenta sam\, quieres admitir el escenario donde los usuarios iniciar sesión en usarlos solo los nombres de cuenta sam\. Puede agregar el código siguiente a onload.js para admitir este escenario, simplemente reemplaza el dominio "contoso.com" en el ejemplo a continuación con el dominio que quieras usar.  
  
```  
if (typeof Login != 'undefined'){  
    Login.submitLoginRequest = function () {   
    var u = new InputUtil();  
    var e = new LoginErrors();  
    var userName = document.getElementById(Login.userNameInput);  
    var password = document.getElementById(Login.passwordInput);  
    if (userName.value && !userName.value.match('[@\\\\]'))   
    {  
        var userNameValue = 'contoso.com\\' + userName.value;  
        document.forms['loginForm'].UserName.value = userNameValue;  
    }  
  
    if (!userName.value) {  
       u.setError(userName, e.userNameFormatError);  
       return false;  
    }  
  
    if (!password.value)   
    {  
        u.setError(password, e.passwordEmpty);  
        return false;  
    }  
    document.forms['loginForm'].submit();  
    return false;  
};  
}  
  
```  
  
## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión del usuario FS de anuncios](AD-FS-user-sign-in-customization.md)  
  

