# TypeScript - Genéricos

[Volver a Inicio](../README.md)

> En TypeScript, los símbolos "< >" (también conocidos como "diamantes") se utilizan para indicar tipos genéricos.

- Los tipos genéricos permiten que las clases, interfaces y funciones trabajen con diferentes tipos sin perder la consistencia y seguridad de tipos que ofrece TypeScript.
- Esto proporciona una gran flexibilidad y permite la reutilización del código, ya que la función desarrollada puede recibir argumentos de tipos diferentes, y no estará limitada a uno solo.

## Ejemplo de Función sin utilización de Genéricos

- La siguiente función sólo nos permitirá recibir argumentos de tipo "string", por lo que no podremos utilizarla para imprimir "números", "booleanos" o ningún tipó de dato que sea distinto a un "string".

```ts
function imprimeArgumento(arg: string): void {
  console.log(typeof arg);
  console.log(arg);
}

imprimeArgumento('Hola Mundo!!!');
// Al ejecutar recibimos por consola:
// string
// Hola Mundo!!!

// Lo siguiente dará error, ya que la función espera un "string":
// No se puede asignar un argumento de tipo "number" al parámetro de tipo "string".
imprimeArgumento(7);
```

## Uso de < > en Tipos Genéricos

- El uso de Genéricos nos permitirá reutilizar la función para recibir cualquier tipo de dato.
- En el siguiente ejemplo, "T" es un parámetro de tipo genérico que actúa como un marcador de posición para el tipo que será especificado en el momento de la invocación de la función.
- El tipo específico puede ser cualquier cosa, como un número, una cadena, un objeto, etc.
- Es decir, "T" tomará el tipo de dato del argumento con el que sea invocada la función.

```ts
function imprimeArgumento<T>(arg: T): void {
  console.log(typeof arg);
  console.log(arg);
}

imprimeArgumento('Hola Mundo!!!');
// Al ejecutar recibimos por consola:
// string
// Hola Mundo!!!

imprimeArgumento(7);
// Al ejecutar recibimos por consola:
// number
// 7

imprimeArgumento({ nombre: 'Homero' });
// Al ejecutar recibimos por consola:
// object
// { nombre: 'Homero' }
```

### Utilizar Genéricos para indicar tipo de dato de retorno

- Podemos tomar el tipo de dato ("T") para indicar que la función retornará ese tipo de dato:

```ts
function imprimeArgumento<T>(arg: T): T {
  console.log(typeof arg);
  console.log(arg);
  return arg;
}
```

### Recibiendo múltiples Genéricos

- Tembién se nos permite recibir más de un Genérico.
- En el siguiente ejemplo, recibimos dos genéricos:
  - Uno al que llamaremos "T", corresponderá al tipo de dato del primer argumento recibido.
  - Otro al que llamaremos "U", corresponderá al tipo de dato del segundo argumento recibido.
  - Los nombres pueden ser cualquiera elegido por nosotros, pero por lo general se los suele llamar con una letra en mayúscula (Por ejemplo "T", que simboliza a "Type").
- También utilizaremos "T" y "U" para indicar que la función retornará un array con dos elementos, el primero de tipo "T" y el segundo de tipo "U".

```ts
function imprimeArgumento<T, U>(arg1: T, arg2: U): [T, U] {
  console.log(arg1, arg2);
  return [arg1, arg2];
}

imprimeArgumento('Edad', 30);
// Al ejecutar recibimos por consola:
// Edad 30

imprimeArgumento('DNI', 12345678);
// Al ejecutar recibimos por consola:
// DNI 12345678
```

### Especificación Explícita de Genéricos

- El tipo de dato es tomado de forma "Implícita" (es decir, de forma automática) por TypeScript desde el argumento enviado.
- También podemos especificar el tipo de dato que corresponde al argumento al momento de invocar a la función (en cuyo caso decimos que lo indicamos de forma "Explícita"), lo cual hacemos utilizando los símbolos de diamante:

```ts
function imprimeArgumento<T, U>(arg1: T, arg2: U): [T, U] {
  console.log(arg1, arg2);
  return [arg1, arg2];
}

imprimeArgumento<string, number>('Edad', 30);

imprimeArgumento<string, number>('DNI', 12345678);
```

## Uso de Genéricos (< >) en el Contexto de InjectRepository

- En el siguiente caso, "< >" se usa en el contexto de un decorador para la inyección de dependencias en NestJS:

```ts
@Injectable()
export class UsersRepository {
  constructor(
    @InjectRepository(Users) private usersRepository: Repository<Users>,
  ) {}
}
```

### Explicación del código:

- `@Injectable()`: Este es un decorador que marca la clase UsersRepository como un servicio que puede ser inyectado en otros lugares utilizando el mecanismo de inyección de dependencias de NestJS.
- `@InjectRepository(Users)`: Este es un decorador específico de TypeORM en el contexto de NestJS que inyecta un repositorio de TypeORM para la entidad Users en el constructor de la clase.
- `Repository<Users>`:
  - Repository es una clase genérica proporcionada por TypeORM.
  - `<Users>` indica que el Repository operará sobre entidades de tipo Users.
    - Esto significa que usersRepository será un repositorio que maneja operaciones CRUD y consultas específicas para entidades Users.
  - Función de < > en este Contexto
    - Especificar Tipos: El uso de `<Users>` después de Repository indica que el repositorio está específicamente diseñado para manejar la entidad Users. Esto proporciona seguridad de tipo y autocompletado en el entorno de desarrollo.
- Inyección de Dependencias: @InjectRepository(Users) se asegura de que el repositorio correcto para la entidad Users sea inyectado en el servicio UsersRepository.

## Resumen

> Los símbolos < > en TypeScript se utilizan para definir tipos genéricos, que permiten crear componentes reutilizables que pueden trabajar con diferentes tipos de datos. En el contexto de NestJS y TypeORM, se utilizan para especificar qué tipo de entidad manejará un repositorio, proporcionando seguridad de tipo y facilitando la inyección de dependencias.
