---
title: Recuperación de bosques de AD - grupos de RID generar
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c37bc129-a5e0-4219-9ba7-b4cf3a9fc9a4
ms.technology: identity-adds
ms.openlocfilehash: c8f91226e10ea6681933d5a5dc00b92f5ab2179c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862986"
---
# <a name="ad-forest-recovery---raising-the-value-of-available-rid-pools"></a>Recuperación de bosques de AD: generar el valor de los grupos de RID disponibles 

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Use el siguiente procedimiento para elevar el valor de identificador relativo (RID) de grupos que el maestro de operaciones de RID asignará después de restaura ese controlador de dominio. Al aumentar el valor de los grupos de RID disponibles, puede asegurarse de que ningún controlador de dominio asigna un RID para una entidad de seguridad que se creó después de la copia de seguridad que se usó para restaurar el dominio. 

## <a name="about-active-directory-rid-pools-and-ridavailablepool"></a>Acerca de los grupos de RID de Active Directory y rIDAvailablePool

Cada dominio tiene un objeto **CN = RID Manager$, CN = System, DC**=<*nombre_dominio*>. Este objeto tiene un atributo denominado **rIDAvailablePool**. Este valor de atributo mantiene el espacio global de RID para todo el dominio. El valor es un número entero grande con partes superior e inferior. La parte superior define el número de entidades de seguridad que se pueden asignar para cada dominio (0x3FFFFFFF o simplemente más de mil millones de 1). La parte inferior es el número de RID que se han asignado en el dominio. 
  
> [!NOTE]
> En Windows Server 2016 y 2012, ha aumentado el número de entidades de seguridad que se pueden asignar a más de 2 millones. Para obtener más información, consulte [la emisión de RID administrar](https://technet.microsoft.com/library/jj574229.aspx). 
  
- Valor de ejemplo: 4611686014132422708  
- Parte baja: 2100 (principio del siguiente grupo RID que se asignen)  
- Parte superior: 1073741823 (número total de los RID que se pueden crear en un dominio)  
  
Al aumentar el valor del entero grande, aumente el valor de la parte baja. Por ejemplo, si agrega 100 000 en el valor de ejemplo de 4611686014132422708 de una suma de 4611686014132522708, la nueva parte baja es 102100. Esto indica que el siguiente grupo RID que se va a asignar el maestro de RID se iniciará con 102100 en lugar de 2100. 
  
### <a name="to-raise-the-value-of-available-rid-pools-using-adsiedit-and-the-calculator"></a>Para aumentar el valor de grupos de RID disponibles mediante adsiedit y la calculadora

1. Abra el administrador del servidor, haga clic en **herramientas** y haga clic en **ADSI Edit**.
2. El botón derecho, seleccione **conectarse a** y conecte el contexto de nomenclatura predeterminado y haga clic en **Aceptar**.
   ![Edición de ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi1.png) 
3. Vaya a la siguiente ruta de acceso de nombre distintivo: **CN = RID Manager$, CN = System, DC =<domain name>**.
   ![Edición de ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi2.png) 
3. Haga clic en y seleccionar las propiedades de CN = RID Manager$. 
4. Seleccione el atributo **rIDAvailablePool**, haga clic en **editar**y, a continuación, copie el valor de entero grande en el Portapapeles.
   ![Edición de ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi3.png)  
5. Iniciar la calculadora y desde el **vista** menú, seleccione **modo científico**. 
6. Agregar 100 000 al valor actual.
   ![Edición de ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi4.png) 
7. Mediante ctrl-c, o la **copia** comando desde el **editar** menú, copie el valor en el Portapapeles. 
8. En el cuadro de diálogo de edición de adsiedit, pegue este nuevo valor. 
   ![Edición de ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi5.png) 
9. Haga clic en **Aceptar** en el cuadro de diálogo y **aplicar** en la hoja de propiedades para actualizar la **rIDAvailablePool** atributo. 
  
### <a name="to-raise-the-value-of-available-rid-pools-using-ldp"></a>Para aumentar el valor de uso de LDP disponibles grupos de RID  
  
1. En el símbolo del sistema, escriba el siguiente comando y presione ENTRAR:  
   **ldp**  
2. Haga clic en **conexión**, haga clic en **Connect**, escriba el nombre del Administrador de RID y, a continuación, haga clic en **Aceptar**. 
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp1.png)
3. Haga clic en **conexión**, haga clic en **enlazar**, seleccione **enlazar con credenciales** y escriba sus credenciales administrativas y, a continuación, haga clic en **Aceptar**. 
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp2.png)
4. Haga clic en **vista**, haga clic en **árbol** y, a continuación, escriba la siguiente ruta de acceso de nombre distintivo:  CN = RID Manager$, CN = System, DC =*nombre de dominio*  
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp3.png)
5. Haga clic en **examinar**y, a continuación, haga clic en **modificar**. 
6. Agregar 100 000 a actual **rIDAvailablePool** valor y, a continuación, escriba la suma en **valores**. 
7. En **Dn**, tipo `cn=RID Manager$,cn=System,dc=` *< nombre de dominio\>*. 
8. En **Editar atributo de entrada**, tipo `rIDAvailablePool`. 
9. Seleccione **reemplazar** como la operación y, a continuación, haga clic en **ENTRAR**.
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp4.png) 
10. Haga clic en **ejecutar** para ejecutar la operación. Haga clic en **Cerrar**.
11. Para validar el cambio, haga clic en **vista**, haga clic en **árbol**y, a continuación, escriba la siguiente ruta de acceso de nombre distintivo:   CN = RID Manager$, CN = System, DC =*nombre de dominio*.   Compruebe el **rIDAvailablePool** atributo. 
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp5.png)

## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación de bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación de bosques de AD: procedimientos](AD-Forest-Recovery-Procedures.md)
