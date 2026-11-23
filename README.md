# Práctica en Odoo

**Nombre:Abad Matute** 
**Número de cuenta:0801200314916** 

## Respuestas

### Paso 1. Catálogo de equipos

**1. ¿Qué pasa con un equipo cuando se desmarca el campo Activo? ¿Por qué es mejor que borrarlo?**

Cuando desmarco Activo el equipo queda archivado: ya no sale en la lista ni en las búsquedas, y tampoco aparece para elegirlo en otros formularios, pero sigue guardado en la base de datos. Lo puedo ver otra vez con el filtro "Archivados" y volver a activarlo. Es mejor que borrarlo porque no se pierde la información, por ejemplo los préstamos viejos que tenían ese equipo siguen mostrando cuál fue. Si lo borro esa referencia se pierde o Odoo no me deja borrarlo.

**2. ¿Para qué sirve copy=False en el campo codigo?**

Sirve para que cuando se duplica un equipo el código de inventario no se copie al nuevo registro. Como cada equipo tiene su propio código así se evita que queden dos equipos con el mismo código, y como el campo es obligatorio hay que ponerle uno nuevo.

### Paso 2. Registro de préstamos

**1. Este modelo usa _name y _inherit al mismo tiempo. ¿Qué hace cada uno aquí?**

_name le da el nombre al modelo nuevo, prestamo.prestamo, y con eso Odoo crea su propia tabla en la base de datos. _inherit hace que ese modelo nuevo reciba los campos y métodos de mail.thread.

**2. ¿Qué hace tracking=True en el campo state?**

Hace que cada vez que cambia el estado, Odoo deje un registro automático.

### Paso 3. Equipos del préstamo

**1. En fields.One2many("prestamo.linea", "prestamo_id"), ¿qué indica cada parámetro?**

El primero, "prestamo_linea" es el modelo donde están los registros relacionados o sea las líneas. El segundo, "prestamo_id" es el campo Many2one dentro de prestamo.linea que apunta de regreso al préstamo.

**2. ¿Qué pasa con las líneas cuando se borra un préstamo? ¿Qué parte del código lo define?**

Se borran automáticamente junto con el préstamo. Lo define ondelete="cascade" en el campo prestamo_id del modelo prestamo_linea.

### Paso 4. Flujo de estados

**1. Los botones ya se ocultan según el estado. ¿Por qué los métodos vuelven a validar el estado?**

Porque ocultar el botón solo cambia lo que se ve en la pantalla, pero el método se puede llamar de otras formas por ejemplo desde otra pestaña que tenga el formulario desactualizado, desde código o desde la API. La validación en Python es la que de verdad protege los datos, porque se ejecuta en el servidor sin importar desde dónde se llamó.

**2. ¿Qué hace prestamo.linea_ids.equipo_id.write(...)? ¿Por qué no hace falta un for para cada equipo?**

prestamo.linea_ids.equipo_id devuelve un recordset con todos los equipos de las líneas del préstamo, y write cambia el estado de todos esos equipos al mismo tiempo. No hace falta un for porque en Odoo los recordsets pueden tener varios registros, y write se aplica a todos los registros que contiene.

### Paso 5. Reglas del préstamo

**1. El campo equipo_id ya filtra los equipos disponibles. ¿Por qué hace falta el TODO 5?**

Porque el domain solo filtra la lista en el momento en que elijo el equipo. Después de agregarlo el equipo puede cambiar de estado, por ejemplo si lo mandan a mantenimiento o si se presta en otro préstamo al mismo tiempo.

**2. ¿En qué momento se ejecuta un método con @api.constrains? ¿Y la validación de action_prestar?**

El método con @api.constrains se ejecuta solo cada vez que se guarda un registro, al crearlo o modificarlo. Si lanza el error, el registro no se guarda. La validación de action_prestar solo se ejecuta cuando el usuario pulsa el botón Prestar, por eso un préstamo con 3 equipos de estudiante sí se puede guardar en borrador, pero no se puede prestar.