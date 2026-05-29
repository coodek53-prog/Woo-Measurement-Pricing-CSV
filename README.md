# Woo-Measurement-Pricing-CSV
Plugin para WordPress y WooCommerce que calcula el precio de un producto variable según las medidas que el cliente introduce en la unidad visible configurada en la tienda.

Plugin desarrollado por Coodek para la marca Dekorty.

Qué hace
Añade campos Ancho y Alto en la ficha del producto variable, visibles en cm o mm.
Calcula el precio en tiempo real cuando el cliente elige una variación e introduce sus medidas.
Sustituye visualmente el precio mostrado en la ficha por el total calculado cuando la medida es válida.
Usa una tabla de tarifas subida por CSV desde la edición del producto.
Permite aplicar recargos por atributo seleccionado con importe fijo o multiplicador, por ejemplo pa_mecanismo=motorizado:25 o pa_tejido=screen:*1.15.
Es compatible con Markup by Attribute for WooCommerce y suma automáticamente los markups guardados en los términos de atributos globales.
Guarda una copia del CSV en la carpeta uploads de WordPress para que quede persistido en el servidor.
Mantiene el precio calculado en carrito, checkout y pedido.
Soporta tanto matrices simples Widht,Height,Price como reglas por rango/variación más avanzadas.
Permite definir por separado la unidad visible en tienda y la unidad usada por el CSV importado.
Cómo funciona
El administrador activa el cálculo por medidas en el producto variable.
Define el salto de ancho/alto en centímetros dentro del panel.
Elige la unidad del CSV importado (mm o cm) según tu archivo.
Sube un CSV con las tarifas del producto.
Opcionalmente añade recargos por atributo desde la misma pantalla del plugin.
El cliente selecciona una variación, introduce ancho y alto en la unidad visible y WooCommerce muestra el precio calculado.
Los mínimos y máximos válidos ya no se configuran manualmente en el plugin: se toman siempre de las reglas importadas desde el CSV.

Visualización en directo
El precio total de la cortina se muestra en una caja destacada dentro de la ficha.
Además, el plugin actualiza el precio visible del propio producto en WooCommerce para que el cliente vea el total en directo.
La caja de medidas muestra en pequeño los mínimos y máximos derivados del CSV importado.
El precio aparece junto a la cantidad y el botón Añadir al carrito.
El formulario evita envíos accidentales mientras no exista un precio calculado válido.
Si la medida no es válida o falta información, el precio base del producto se restaura automáticamente.
Recargos por atributo
En el producto variable o en la pantalla dedicada Productos > Importar CSV de tarifas, añade una regla por línea:

pa_mecanismo=motorizado:25
pa_tejido=screen:*1.15
pa_color=negro:10,50
tejido=premium:15
El formato es atributo=valor:recargo.

25 o 10,50: suma un importe fijo al precio calculado por CSV.
*1.15 o x1.15: multiplica el precio calculado por CSV por ese factor.
Para el mismo atributo y valor puedes combinar ambas cosas con dos líneas distintas, por ejemplo una con *1.15 y otra con 25.
También puedes usar varias líneas del mismo atributo y valor con importes distintos. El plugin conserva todos los recargos diferentes y solo elimina duplicados exactos.
El plugin acepta pa_tejido, tejido o attribute_pa_tejido. Primero aplica los multiplicadores sobre el precio calculado por CSV y después suma los recargos fijos.

Cómo añadirlos paso a paso
Abre el producto variable en WooCommerce.
Ve al bloque Precio por medidas o a Productos > Importar CSV de tarifas.
Busca el campo Recargos por atributo.
Escribe una regla por cada combinación que quieras aplicar.
Formato general:

atributo=valor:recargo
Qué poner en cada parte:

atributo: el slug o nombre técnico del atributo. Recomendado: pa_tejido, pa_color, pa_mecanismo.
valor: el slug o valor exacto de la opción seleccionada por el cliente. Ejemplos: screen, negro, motorizado.
recargo: el importe fijo o el multiplicador.
Ejemplos prácticos:

pa_tejido=screen:25
pa_tejido=screen:*1.15
pa_color=negro:10,50
pa_mecanismo=motorizado:35
Casos habituales:

Si quieres sumar 25 € cuando el tejido sea screen: pa_tejido=screen:25
Si quieres multiplicar el precio por 1,15 cuando el tejido sea screen: pa_tejido=screen:*1.15
Si quieres aplicar multiplicador + importe fijo a la vez, usa dos líneas:
pa_tejido=screen:*1.15
pa_tejido=screen:25
Si quieres sumar dos recargos fijos distintos para la misma opción, también puedes hacerlo:
pa_tejido=screen:10
pa_tejido=screen:25
Consejos:

Si el atributo es global de WooCommerce, usa preferiblemente pa_ delante.
El valor debe coincidir con la opción real del atributo en WooCommerce.
Puedes escribir importes con coma decimal: 10,50
El multiplicador debe escribirse como *1.15 o x1.15
El plugin aplica primero los multiplicadores y después suma los recargos fijos.
Compatibilidad con Markup by Attribute
Si Markup by Attribute for WooCommerce está activo, Woo Measurement Pricing CSV detecta los markups definidos en los términos de atributos globales y los añade al precio calculado por medidas.

Los importes fijos se suman o restan directamente.
Los porcentajes se calculan sobre el precio base obtenido del CSV para esa medida.
Si Markup by Attribute ya guardó un importe aplicado para ese producto, se usa ese importe antes que recalcularlo desde el término.
Se respeta la opción de redondeo de Markup by Attribute cuando está disponible.
Formato del CSV adjunto por Dekorty
El plugin ya acepta el formato del archivo real que compartiste:

Widht,Height,Price
"60,00","60,00","68,00"
"60,00","80,00","71,00"
"80,00","60,00","77,00"
Comportamiento aplicado:

Widht y Height aceptan compatibilidad con datos anteriores y siguen la unidad del CSV importado que configures para ese producto, incluso si Excel exporta valores como 600,00 o 800,00.
Si el archivo real está en milímetros, configura la unidad del CSV en mm y vuelve a importar el archivo para regenerar correctamente los breakpoints guardados.
Al guardar el producto, el plugin también puede reimportar automáticamente el CSV ya guardado en el servidor para regenerar las reglas con la unidad seleccionada.
Price es un precio cerrado para ese tramo.
Si el cliente introduce una medida intermedia, el plugin busca el siguiente tramo superior disponible en la tabla.
Ejemplo: 61,5 cm x 79 cm usa la tarifa de 80 cm x 80 cm.
Si el CSV no trae variation_id, sku ni atributos, la tabla se aplica a todas las variaciones del producto.
Formatos de precio soportados en el CSV
price: precio fijo para ese rango.
price_per_m2: precio por metro cuadrado.
base_price: importe fijo que se suma al cálculo por m2.
min_price: precio mínimo aplicable aunque el cálculo por m2 sea menor.
Fórmula de ejemplo:

precio_final = max(min_price, base_price + ((ancho_mm / 1000) * (alto_mm / 1000) * price_per_m2))
Identificación de la variación
variation_id
sku
Columnas de atributos como attribute_pa_color, attribute_pa_apertura, etc.
Si no hay identificadores, las reglas se consideran genéricas para todo el producto.
Encabezados admitidos
variation_id, sku
Widht, Height, Price
width_cm, height_cm
width_mm, height_mm
min_width_cm, max_width_cm, min_height_cm, max_height_cm
min_width_mm, max_width_mm, min_height_mm, max_height_mm
price, fixed_price, price_per_m2, base_price, min_price, label
Alias en español: ancho_cm, alto_cm, ancho_min_cm, ancho_max_cm, alto_min_cm, alto_max_cm, ancho_mm, alto_mm, ancho_min_mm, ancho_max_mm, alto_min_mm, alto_max_mm, precio, precio_fijo, precio_m2, precio_base, precio_minimo, tarifa
El typo Widht también está soportado porque viene así en el CSV real.
Si el archivo histórico viene realmente en milímetros, la unidad del CSV configurada en el producto es la que manda para interpretar correctamente las medidas importadas.
CSV de ejemplo
Widht,Height,Price
"60,00","60,00","68,00"
"60,00","80,00","71,00"
"80,00","60,00","77,00"
"80,00","80,00","81,00"
"100,00","100,00","95,00"
Notas
El CSV se procesa al guardar o actualizar el producto.
El CSV también se guarda físicamente en wp-content/uploads/.
La unidad visible de la tienda y la unidad del CSV son independientes.
En Apache o LiteSpeed el directorio queda bloqueado por .htaccess. En Nginx conviene añadir una regla del servidor para denegar el acceso directo a wp-content/uploads/wvpc-csv/.
El plugin ya no guarda la URL pública del CSV importado y usa nombres internos impredecibles para reducir exposición si el servidor no aplica esa regla.
Si no existe una tarifa para la combinación de variación y medidas, el producto no se puede añadir al carrito.
El plugin guarda en el pedido el ancho, el alto y la tarifa aplicada mostrando cm al usuario, pero mantiene los mm internamente para no romper datos anteriores.
Para CSV nuevos se recomienda usar columnas explícitas width_cm, height_cm, min_width_cm, max_width_cm, min_height_cm y max_height_cm.
El ejemplo de referencia facilitado por el usuario fue [dekorty.es/cortinas-enrollables](https://dekorty.es/cortinas-verticales), pero no pude inspeccionar su HTML desde esta sesión porque el sitio devolvió timeout; por eso la implementación replica el patrón estándar de medidas a medida en WooCommerce.
