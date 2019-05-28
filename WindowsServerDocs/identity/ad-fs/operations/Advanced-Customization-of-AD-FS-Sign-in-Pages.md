---
ms.assetid: 882abec8-0189-4f73-99c5-792987168080
title: Personalización avanzada de AD FS Sign-in Pages
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 01/16/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 73ff3fc6df872edd29735ee96c0918144250d5f1
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190042"
---
# <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>Personalización avanzada de AD FS Sign-in Pages

  
## <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>Personalización avanzada de inicio de sesión de AD FS\-en páginas  
AD FS en Windows Server 2012 R2 proporciona integrado\-en soporte técnico para personalizar el inicio de sesión\-experiencia. Para la mayoría de estos escenarios, la compilación\-cmdlets de Windows PowerShell son todo lo que es necesario.  Se recomienda que use la compilación\-comandos de Windows PowerShell para personalizar los elementos estándar para AD FS firmar\-experiencia siempre que sea posible.  Consulte [personalización de inicio de sesión de usuario de AD FS](AD-FS-user-sign-in-customization.md) para obtener más información.  
  
En algunos casos, los administradores de AD FS que desee proporcionar un inicio de sesión adicional\-en las experiencias que no son posibles a través de las existentes comandos de PowerShell que se incluyen en\-cuadro con AD FS. En determinados casos, es posible \(dentro de las instrucciones que proporciona a continuación\) para los administradores puedan personalizar el inicio de sesión\-experiencia aún más mediante la adición de lógica adicional para **onload.js** que se proporciona por AD FS y se ejecutará en todas las páginas de AD FS.  
  
## <a name="things-to-know-before-you-start"></a>Aspectos que debe conocer antes de empezar  
  
-   No se admite cualquier cambio que afecta a los flujos de redirección o modifica los parámetros de protocolo que AD FS funciona con.
  
-   El onload.js original, uno que se incluye con el tema web predeterminado, contiene código que controla la representación de página para diferentes factores de forma. Se recomienda no modificar el contenido de onload.js original, pero solo anexar el código para el onload.js existente que controla la lógica personalizada.  
  
-   AD FS se suministra con una compilada\-en el tema web que se denomina de forma predeterminada. No se puede modificar el onload.js de tema web predeterminado. Para actualizar onload.js, tendrá que crear y utilizar un tema web personalizado para el inicio de sesión de AD FS\-en páginas.  Consulte [personalización de inicio de sesión de usuario de AD FS](AD-FS-user-sign-in-customization.md) para obtener información sobre cómo crear un tema web personalizado.  
  
-   El mismo onload.js se ejecutará en todas las páginas ADFS \(ex. formulario\-en función de página de inicio de sesión, página de detección del dominio de inicio y etc.\). Deberá asegurarse de que solo el código en el script se ejecuta tal como se ha diseñado y no se ejecute inesperadamente.  
  
-   Al hacer referencia a cualquier elemento HTML, asegúrese de que siempre comprueba la existencia del elemento antes de que actúa en el elemento. Esto proporciona la solidez y garantiza que no se ejecutaría la lógica personalizada en las páginas que no contienen este elemento. Simplemente puede ver el código fuente HTML en el inicio de sesión de AD FS\-en las páginas para ver los elementos existentes.  
  
-   Se recomienda encarecidamente que valide sus personalizaciones en un entorno alternativo y probarlos antes de ponerlo out en producción servidores de AD FS. Esto reduce las posibilidades de que los usuarios finales que se expone a estas personalizaciones antes de la validación.  
  
## <a name="customizing-the-ad-fs-sign-in-experience-by-using-onloadjs"></a>Personalizar el inicio de sesión de AD FS\-experiencia utilizando onload.js  
Al personalizar el onload.js para el servicio AD FS, siga estos pasos.  
  
#### <a name="customizing-onloadjs-for-the-ad-fs-service"></a>Personalizar onload.js para el servicio AD FS  
  
1.  Para agregar su lógica personalizada a onload.js, deberá crear primero un tema web personalizado. El tema que se envía\-de\-el\-cuadro se denomina de forma predeterminada. Puedes exportar el tema predeterminado y usarlo para empezar rápidamente. El siguiente cmdlet crea un tema web personalizado, lo que duplica el tema web predeterminado:  
  
    ```  
    New-AdfsWebTheme –Name custom –SourceName default  
  
    ```  
  
2.  A continuación, puede exportar personalizado o predeterminado de tema web para obtener el archivo onload.js. Para exportar un tema web, use el siguiente cmdlet:  
  
    ```  
    Export-AdfsWebTheme –Name default –DirectoryPath c:\theme  
  
    ```  
  
    Encontrará onload.js bajo la carpeta de script en el directorio que especifique en el cmdlet export anterior y agregar su lógica personalizada a la secuencia de comandos \(ver casos de uso en la siguiente sección de ejemplo\).  
  
3.  Realizar la modificación necesaria para personalizar onload.js según sus necesidades.  
  
4.  Actualizar el tema con la onload.js modificada. Para aplicar la actualización onload.js a tema web personalizado, use el siguiente cmdlet:  

     Para AD FS en Windows Server 2012 R2:  

    ```  
    Set-AdfsWebTheme -TargetName custom -AdditionalFileResource @{Uri=’/adfs/portal/script/onload.js’;path="c:\theme\script\onload.js"}  
  
    ```  
    Para AD FS en Windows Server 2016:

     ```  
    Set-AdfsWebTheme -TargetName custom -OnLoadScriptPath "c:\ADFStheme\script\onload.js"   
  
    ```  
  
5.  Para aplicar el tema web personalizado en AD FS, use el siguiente cmdlet:  
  
    ```  
    Set-AdfsWebConfig -ActiveThemeName custom  
    ```  
  
## <a name="additional-customization-examples"></a>Ejemplos de personalización adicionales  
Los siguientes son los ejemplos de código personalizado agregado a onload.js para diferentes fine\-con fines de optimización. Al agregar el código personalizado, por favor, agregue siempre el código personalizado a la parte inferior de la onload.js.  
  
### <a name="example-1-change-sign-in-with-organizational-account-string"></a>Ejemplo 1: cambiar la cadena "Iniciar sesión con la cuenta de organización"  
El valor predeterminado de formulario de AD FS\-en función de inicio de sesión\-en la página tiene un título de "Inicio de sesión con su cuenta profesional" por encima de los cuadros de entrada de usuario.  
  
Si desea reemplazar esta cadena por su propio, puede agregar el código siguiente a onload.js.  
  
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
  
### <a name="example-2-accept-sam-account-name-as-a-login-format-on-an-ad-fs-form-based-sign-in-page"></a>Ejemplo 2: Aceptar SAM\-nombre de la cuenta como un formato de inicio de sesión en un formulario de AD FS\-en función de inicio de sesión\-en la página  
El valor predeterminado de formulario de AD FS\-en función de inicio de sesión\-en la página admite el formato de inicio de sesión de los nombres principales de usuario \(UPN\) \(por ejemplo, **johndoe@contoso.com** \) dominio completo sam o\-nombres de cuenta \( **contoso\\johndoe** o **contoso.com\\johndoe**\). En caso de todos los usuarios provienen del mismo dominio y que sólo saben sam\-nombres de cuenta, es posible que desee admitir el escenario donde los usuarios pueden iniciar sesión en su uso en sam\-solo los nombres de cuenta. Puede agregar el código siguiente a onload.js para admitir este escenario, solo tiene que reemplazar el dominio "contoso.com" en el ejemplo siguiente con el dominio que desea usar.  
  
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
[AD FS Sign-personalización de usuario](AD-FS-user-sign-in-customization.md)  
  

