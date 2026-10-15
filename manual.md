Elaborar en formato markdown un manual con las anotaciones más habituales de OpenAPI para documentar una API REST con Spring Boot.

Se deben incluir las anotaciones que trabajan:

- A nivel de toda la API
- A nivel de un controlador completo
- A nivel de un endpoint
- A nivel de un DTO y sus atributos.

Incluye una descripción de para qué sirve la anotación y cuales son sus atributos. Si sus atributos son a su vez anotaciones, incluye recursivamente una descripción de la misma.

El manual puede incluir también ejemplos de fragmentos de código con el uso de las anotaciones.

-------------

MANUAL DE ANOTACIONES OPENAPI

1. A nivel de toda la API:
Incluye aquellas que se usan una única vez, generalmente, sobre la clase principal o la clase de configuración

Tenemos las siguietnes anotaciones:
- @OpenApiDefinition: define los metadatos a nivel global del documento generado, como el título, versión, etc.
En cuanto a sus atributos, encontramos varios como lo info(metadatos descriptivos como la descipción, contacto o licencia), servers(lsita de entornos URL donde están hosteadas la API), security(esquemas de seguridad por defecto que se aplian a toda la API), tags(definir globalmente las etiquetas, como los endoponints) y externalDocs(enlaces a documentación externa)

Ejemplo:
@OpenAPIDefinition(
    info = @Info(title = "Product API", version = "1.0"),
    servers = { @Server(url = "http://localhost:8080", description = "Local") }
)
@SpringBootApplication
public class PrimerEjemploApplication { }´

2. A nivel de un controlador completo: son colocadas sobre la clase anotada con @RestController, aplicandose así a todos sus endpoints (excepto los sobreescritos)

Tenemos las siguientes anotaciones:
- @Tag: sirve a nivel interno para agrupar en una carpeta invisible, gestionada por Swagger UI. Esta anotación presenta un par de atributos, name y description, los cuales sirven para lo que indican sus nombres

Ejemplo:
@Tag(name = "Productos", description = "Operaciones CRUD sobre productos")
@RestController
@RequestMapping("/products")
public class ProductController { }

- @SecirutyRequirement: es la misma anotación descrita en el apartado anterior, la diferencia es que esta está posicionada a nivel de clase

Ejemplo:
@SecurityRequirement(name = "bearerAuth")
@RestController
public class ProductController { }

- @Hidden: sirve para ocultar por completo un controlador, así como todos sus endpoints. No presenta atributos

Ejemplo:
@Hidden
public class CategoryController { }


3. A nivel de un endpoint: son las que se colocan sobre el método en cuestión a documentar.

Tenemos las siguentes:
- @Operation: describe una operación HTTP concreta. Tiene atributos como summary, description, operationId, tags, deprecated, hidden e incluso anotaciones como atributos, como lo son @Parameter[], @RequestBody, @ApiResopnse[] y @SecurityRequirement[]

Ejmplo:
@Operation(
    summary = "Obtener un producto por id",
    description = "Devuelve los datos completos de un producto a partir de su identificador"
)
@GetMapping("/{id}")
public ResponseEntity<Product> getById(@PathVariable Long id) { ... }

- @ApiResponses y @ApiRsonse: el primero es un contenedor que agrupa varias del segundo. En el segundo, podemoas añadirle atributos como responseCode, description o incluso content para indicarle el cuerpo de la respuesta para ese código.

Ejmplo:
@Operation(summary = "Obtener un producto por id")
@ApiResponses({
    @ApiResponse(responseCode = "200", description = "Producto encontrado",
        content = @Content(schema = @Schema(implementation = Product.class))),
    @ApiResponse(responseCode = "404", description = "Producto no encontrado", content = @Content)
})
@GetMapping("/{id}")
public ResponseEntity<Product> getById(@PathVariable Long id) { ... }

- @Parameter: documenta un parámetro indvidual de la operación cuando se necesita añadir información que Spring no lo hace automáticamente, tiene atributos como name, description, required, in, example y la anotación @Schema para indicar el tipo y restricción de parámetro.

Ejemplo:
@Operation(summary = "Buscar productos por nombre")
@GetMapping
public List<Product> search(
    @Parameter(description = "Texto a buscar en el nombre", example = "teclado")
    @RequestParam String name) { ... }

- @RequestBody: esta anotación también pertenece al framework de Spring, por lo que al usarla, hay que tener en cuenta que esté importada desde la librería de Swagger. Contiene atributos como description, required y una anotación @Content[] para indicar el tipo de contenido y el schema separado.

Ejemplo:
@Operation(summary = "Crear un producto")
@io.swagger.v3.oas.annotations.parameters.RequestBody(
    description = "Datos del nuevo producto", required = true)
@PostMapping
public ResponseEntity<Product> create(@org.springframework.web.bind.annotation.RequestBody ProductDTO dto) { ... }


4. A nivel de un DTO y sus atributos: se colocan sobre las clases DTO y sobre cada uno de sus campos, para completar el esquema de datos de Swagger. 

Tenemos las siguientes anotaciones:
- @Schema: 

- @ArraySchema: 



--> Formatear para un printeo bonito