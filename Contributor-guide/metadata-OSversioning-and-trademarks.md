# <a name="metadata-and-version-identifiers"></a>Identificadores de los metadatos y la versión

Aquí es lo que necesita saber sobre marcas comerciales, control de versiones y los metadatos para los artículos de repositorio windowsserverdocs-pr. Los autores del artículo son responsabilidad suya asegurarse de que cumplan estos estándares y requisitos de los artículos.

## <a name="trademarks"></a>Marcas comerciales
No use los símbolos de marca comercial después de las referencias del producto en los artículos en la biblioteca técnica. No son necesarias porque technet.microsoft.com, docs.microsoft.com y otros canales de publicación oficial de Microsoft incluyen un [Trademark](https://www.microsoft.com/trademarks) vínculo de pie de página a una lista de marcas comerciales de Microsoft. Ese vínculo cumple el requisito legal. Para más información, consulte la Guía de [tres](https://microsoft.sharepoint.com/sites/LCAWeb/Home/Copyrights-Trademarks-and-Patents/Trademarks/Trademark-List-and-Usage), bajo "sitios Web" y la Guía de estilo de escritura de Microsoft [copyright y marcas comerciales](https://worldready.cloudapp.net/Styleguide/Read?id=2700&topicid=26696) página, debajo de "Las páginas Web en Microsoft.com" 

## <a name="versioning"></a>Control de versiones
Varios tipos de control de versiones se aplican a los artículos de este repositorio: 

-  Versiones de fila que indica que un artículo se aplica a la versión del producto se realiza de dos maneras:
    - Manualmente en una sola línea en el artículo, por lo que es visible como texto en un artículo publicado.
    - Metadatos, se describe a continuación.
-  Control de versiones de contenido de origen se controla mediante el historial de Github en el archivo. 

## <a name="metadata-structure-and-format"></a>Formato y estructura de los metadatos

- Poner metadatos en la parte superior del archivo, el encabezado H1.
- Separe el bloque del resto del contenido del archivo con solo tres guiones en las líneas primeros y últimas del bloque. No incluya cualquier otro texto en estas líneas.
- Coloque cada par nombre/valor de metadatos en una línea independiente.
- Utilice uno de los patrones de sintaxis siguientes, dependiendo de si los metadatos requieren valores predefinidos o acepte los valores personalizados. 

        ---
        name1: predefined-value
        name2: "my custom value"
        ---

## <a name="metadata-names-and-values"></a>Los valores y nombres de metadatos

Ciertos metadatos son necesarios para todos los archivos que se publican como artículos en la biblioteca técnica de TechNet, mientras que otros metadatos se recomienda, aunque no es necesario. En algunos casos, se permiten solo ciertos valores para una parte específica de los metadatos. 

|Nombre|Valor|
|---|---|
|title|Valor personalizado que coincida con el encabezado H1. Determina lo que se muestra en la pestaña del explorador.|
|description|Se muestra en los resultados de búsqueda y afecta a SEO. Incluir las palabras clave adecuadas pero mantener la longitud inferior a 160 caracteres.|
|Autor|Alias de Github del autor del artículo principal|
|ms.author|Alias MSFT de desarrollador de autor o el contenido de su artículo responsable del área de tecnología que se explica en el artículo.|
|ms.date|Fecha de última actualización del contenido. No se actualizan para que los cambios solo metadatos. Use el formato mm/dd/aaaa.|
|ms.prod|Identifica la versión de Windows Server para informes, BI según predefinido [valor](https://microsoft.sharepoint.com/teams/STBCSI/Insights/_layouts/15/WopiFrame.aspx?sourcedoc=%7b7A321BF1-0611-4184-84DA-A0E964C435FA%7d&file=WEDCS_MasterList_CSIValues.xlsx&action=default&IsList=1&ListId=%7b46B17C8A-CD7E-47ED-A1B6-F2B654B55E2B%7d&ListItemId=969).|
|ms.technology|Identifica el área de tecnología para informes, BI según predefinido [valor](https://microsoft.sharepoint.com/teams/STBCSI/Insights/_layouts/15/WopiFrame.aspx?sourcedoc=%7b7A321BF1-0611-4184-84DA-A0E964C435FA%7d&file=WEDCS_MasterList_CSIValues.xlsx&action=default&IsList=1&ListId=%7b46B17C8A-CD7E-47ED-A1B6-F2B654B55E2B%7d&ListItemId=969)|
|ms.asset|GUID que identifica el artículo en todas las configuraciones regionales para informes de BI. Artículos se migran desde la edición anterior sistemas ya tienen esto. Para artículos creados en Github, use una herramienta como [ https://guidgenerator.com/ ](https://guidgenerator.com/).| 

## <a name="troubleshooting"></a>Solución de problemas

Los metadatos que aparece en la parte superior de un artículo publicado se produce cuando el archivo de origen tiene errores de formato. Algunos errores comunes son:

- Falta el triple guión en las líneas primeros y últimas de bloque o un número incorrecto de guiones.
- Los metadatos no siguen la sintaxis requerida: \<nombre\>:\<único espacio\>\<valor >

Busque el archivo de éstos y otros errores obvios, envíe un PR para publicar el archivo actualizado. Si se queda atascado, correo electrónico el alias de los revisores de incorporación de cambios: wssc-pra@microsoft.com

## <a name="see-also"></a>Vea también
Los metadatos usados en este repositorio se basa en los metadatos usados en los servicios de contenido & International \(CSI\). Obtener más información, incluidos los metadatos opcionales, está disponible en [ http://aka.ms/skyeye/meta ](http://aka.ms/skyeye/meta).
Para recursos de business intelligence, consulte el equipo de BI de CSI [wiki](https://microsoft.sharepoint.com/teams/STBCSI/Insights/Selfserve%20BI%20wiki/Self-serve%20BI%20wiki.aspx).