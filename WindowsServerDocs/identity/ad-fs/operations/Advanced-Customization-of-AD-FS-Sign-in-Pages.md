---
ms.assetid: 882abec8-0189-4f73-99c5-792987168080
title: Personalización avanzada de páginas de inicio de sesión de AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 01/16/2019
ms.topic: article
ms.openlocfilehash: dd0fa97024f286d90f096da9fa132f40c1a78aae
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87956672"
---
# <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>Personalización avanzada de páginas de inicio de sesión de AD FS


## <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>Personalización avanzada de páginas de inicio de sesión de AD FS \-
AD FS en Windows Server 2012 R2 proporciona \- compatibilidad integrada para personalizar la \- experiencia de inicio de sesión. En el caso de una mayoría de estos escenarios, los \- cmdlets de Windows PowerShell integrados son todos necesarios.  Se recomienda usar los \- comandos integrados de Windows PowerShell para personalizar los elementos estándar para AD FS \- experiencia de inicio de sesión siempre que sea posible.  Consulte [Personalización de inicio de sesión de usuario de AD-FS](AD-FS-user-sign-in-customization.md) para obtener más información.

En algunos casos, es posible que los administradores de AD FS deseen proporcionar experiencias de inicio de sesión adicionales \- que no sean posibles a través de los comandos de PowerShell existentes que se incluyen en \- box con AD FS. En algunos casos, es factible dentro de \( las instrucciones que se proporcionan a continuación \) para que los administradores personalicen \- aún más la experiencia de inicio de sesión agregando lógica adicional a **onload.js** proporcionada por AD FS y se ejecutará en todas las páginas de AD FS.

## <a name="things-to-know-before-you-start"></a>Cosas que hay que saber antes de empezar

-   No se admite cualquier cambio que afecte a los flujos de redirección o a los modificadores de protocolo que AD FS funciona con.

-   El onload.js original, el que viene con el tema Web predeterminado, contiene código que controla la representación de página para diferentes factores de forma. Se recomienda no modificar el contenido original del onload.js, sino solo anexar el código al onload.js existente que controla la lógica personalizada.

-   AD FS se suministra con un \- tema Web integrado que se denomina default. No se puede modificar el onload.js del tema Web predeterminado. Para actualizar onload.js, tiene que crear y usar un tema web personalizado para las páginas de inicio de sesión de AD FS \- .  Consulte [Personalización de inicio de sesión de usuario de AD-FS](AD-FS-user-sign-in-customization.md) para obtener información sobre cómo crear un tema web personalizado.

-   El mismo onload.js se ejecutará en todas las páginas de ADFS \( . \-Página de inicio de sesión basada en formularios, página de detección del dominio de inicio, etc. \) Debe asegurarse de que el código del script se ejecute solo cuando esté diseñado y no se ejecute de forma inesperada.

-   Al hacer referencia a cualquier elemento HTML, asegúrese de comprobar siempre la existencia del elemento antes de actuar en el elemento. Esto proporciona solidez y garantiza que la lógica personalizada no se ejecutará en páginas que no contengan este elemento. Simplemente puede ver el origen HTML en las \- páginas de inicio de sesión de AD FS para ver los elementos existentes.

-   Se recomienda encarecidamente validar las personalizaciones en un entorno alternativo y probarlas antes de implementarlas en los servidores de AD FS de producción. Esto reduce las posibilidades de que los usuarios finales se expongan a estas personalizaciones antes de la validación.

## <a name="customizing-the-ad-fs-sign-in-experience-by-using-onloadjs"></a>Personalización de la \- experiencia de inicio de sesión de AD FS mediante onload.js
Siga los pasos que se indican a continuación al personalizar el onload.js para el servicio AD FS.

#### <a name="customizing-onloadjs-for-the-ad-fs-service"></a>Personalización de onload.js para el servicio AD FS

1.  Para agregar la lógica personalizada a onload.js, primero debe crear un tema web personalizado. El tema que se envía fuera \- del \- \- cuadro se denomina predeterminado. Puedes exportar el tema predeterminado y usarlo para empezar rápidamente. El siguiente cmdlet crea un tema web personalizado, que duplica el tema Web predeterminado:

    ```
    New-AdfsWebTheme –Name custom –SourceName default

    ```

2.  Después, puede exportar el tema web personalizado o predeterminado para obtener onload.js archivo. Para exportar un tema Web, use el siguiente cmdlet:

    ```
    Export-AdfsWebTheme –Name default –DirectoryPath c:\theme

    ```

    Encontrará onload.js en la carpeta script del directorio que especifique en el cmdlet Export anterior y agregue la lógica personalizada al script \( . Consulte casos de uso en la sección ejemplo siguiente \) .

3.  Realice las modificaciones necesarias para personalizar onload.js en función de sus necesidades.

4.  Actualice el tema con el onload.js modificado. Use el siguiente cmdlet para aplicar el onload.js de actualización al tema web personalizado:

     Para AD FS en Windows Server 2012 R2:

    ```
    Set-AdfsWebTheme -TargetName custom -AdditionalFileResource @{Uri='/adfs/portal/script/onload.js';path="c:\theme\script\onload.js"}

    ```
    Para AD FS en Windows Server 2016:

     ```
    Set-AdfsWebTheme -TargetName custom -OnLoadScriptPath "c:\theme\script\onload.js"

    ```

5.  Para aplicar el tema web personalizado a AD FS, use el siguiente cmdlet:

    ```
    Set-AdfsWebConfig -ActiveThemeName custom
    ```

## <a name="additional-customization-examples"></a>Ejemplos de personalización adicionales
A continuación se muestran ejemplos de código personalizado que se agrega a onload.js para diferentes \- propósitos de optimización. Al agregar el código personalizado, anexe siempre el código personalizado a la parte inferior del onload.js.

### <a name="example-1-change-sign-in-with-organizational-account-string"></a>Ejemplo 1: cambiar la cadena "iniciar sesión con la cuenta de organización"
La \- Página de inicio de sesión basada en formularios AD FS predeterminada \- tiene un título de "iniciar sesión con su cuenta de organización" en los cuadros de entrada del usuario.

Si desea reemplazar esta cadena por su propia cadena, puede Agregar el código siguiente a onload.js.

```
// Sample code to change "Sign in with organizational account" string.

// Check whether the loginMessage element is present on this page.
var loginMessage = document.getElementById('loginMessage');
if (loginMessage)
{
       // loginMessage element is present, modify its properties.
       loginMessage.innerHTML = 'Your company description text';
}

```

### <a name="example-2-accept-sam-account-name-as-a-login-format-on-an-ad-fs-form-based-sign-in-page"></a>Ejemplo 2: aceptar \- el nombre de cuenta SAM como formato de inicio de sesión en una AD FS \- Página de inicio de sesión basada en formularios \-
La \- Página de inicio de sesión basada en formularios AD FS predeterminada \- admite el formato de inicio de sesión de los nombres principales de usuario \( UPN \) \( , <strong>johndoe@contoso.com</strong> \) o nombres de cuenta SAM calificados de dominio \- \( **contoso \\ ** contoso.com o ** \\ ** \) . En caso de que todos los usuarios provengan del mismo dominio y solo sepan acerca de los nombres de cuenta SAM, puede que \- desee admitir el escenario en el que los usuarios pueden iniciar sesión usando \- solo los nombres de cuenta SAM. Puede Agregar el código siguiente a onload.js para admitir este escenario, simplemente reemplace el dominio "contoso.com" en el ejemplo siguiente por el dominio que desea usar.

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
[Personalización de inicio de sesión de AD FS usuario](AD-FS-user-sign-in-customization.md)


