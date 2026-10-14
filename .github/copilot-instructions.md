# Estándares de desarrollo .NET

## Estructura y modularidad

- Mantén un solo `.csproj` mientras el juego, la consola y sus pruebas puedan compilarse como una sola aplicación; crea otro proyecto solo para una frontera real, como pruebas automatizadas.
- Al crecer el código, agrupa archivos por responsabilidad en carpetas como `Game`, `Players` y `ConsoleUI`; evita carpetas sin una función clara.
- Declara una clase por archivo y usa el mismo nombre para el archivo y la clase.
- Mantén `Program.cs` como punto de composición: delega reglas, estado del tablero, interacción de consola y marcador a componentes con responsabilidades separadas.

## Principios de diseño

- **SRP, responsabilidad única:** cada clase debe tener un motivo principal para cambiar; por ejemplo, `Board` administra casillas y reglas, mientras `ConsoleRenderer` dibuja el tablero.
- **OCP, abierto a extensión y cerrado a modificación:** agrega nuevos tipos de jugador sin reescribir el flujo de partida; por ejemplo, incorpora `ComputerPlayer` junto a `HumanPlayer` mediante un contrato común.
- **LSP, sustitución:** cualquier implementación de `IPlayer` debe poder ocupar el lugar de otra sin romper el turno; por ejemplo, tanto el jugador humano como el computador deben devolver una jugada válida o un resultado de cancelación definido.
- **ISP, interfaces pequeñas:** cada consumidor debe depender solo de las operaciones que usa; por ejemplo, separa la obtención de jugadas (`IPlayer`) del dibujo del tablero (`IBoardRenderer`).
- **DIP, inversión de dependencias:** la lógica de partida no debe depender directamente de `Console`; por ejemplo, recibe jugadores y un renderer por sus contratos, y deja sus implementaciones de consola en el punto de composición.

## Nombres e idioma

- Escribe nombres de tipos, métodos, propiedades, variables y archivos en inglés, de forma consistente con las convenciones de .NET.
- Usa `PascalCase` para tipos y miembros públicos, y `camelCase` para parámetros y variables locales.
- Elige nombres completos y descriptivos como `currentPlayer` y `drawCount`; evita abreviaturas como `t`, `b` o `ganX`.
- Mantén en español los mensajes visibles para las personas que juegan; no mezcles idiomas dentro de los identificadores.

## Errores y validación de entradas

- Lee números con `int.TryParse`; no uses `int.Parse` para entradas de consola.
- Trata `Console.ReadLine()` igual a `null` como fin de entrada y termina o cancela la partida de forma controlada.
- Cuando cambien las reglas, añade pruebas para las ocho líneas ganadoras y el empate; incluye explícitamente la diagonal secundaria (casillas 3, 5 y 7).
- Antes de cambiar el tablero, valida que la casilla esté entre 1 y 9 y que esté libre; una entrada inválida no debe cambiar el turno ni el estado.
- Informa el motivo de cada entrada rechazada y vuelve a pedirla sin finalizar el proceso con una excepción.
- Reserva excepciones para fallos inesperados; no las uses para controlar entradas inválidas o el flujo normal de la partida.

## Documentación

- Añade comentarios XML (`///`) a cada tipo o miembro público nuevo; documenta su propósito, parámetros, retorno y excepciones cuando corresponda.
- Mantén los comentarios centrados en contratos, decisiones o restricciones que no sean evidentes; actualízalos o elimínalos cuando cambie el comportamiento.
- Actualiza el `README.md` cuando cambien el objetivo del juego, las instrucciones de compilación/pruebas o las reglas que ve quien juega.

## Forma de trabajar

- En cada solicitud, trabaja solo la etapa del plan pedida; no adelantes etapas posteriores sin que se soliciten.
- No ejecutes `dotnet run`; valida cambios con `dotnet build` y, cuando existan, con las pruebas automatizadas pertinentes.
- Mantén los cambios limitados a la etapa solicitada y no alteres código ajeno a ese alcance.

## Bitácora de cambios con IA

Después de cada cambio que hagas en el proyecto, agrega una entrada al final de docs/bitacora-ia.md con este formato:

## AAAA-MM-DD HH:mm · Etapa X.Y · Título corto
- Cambio: qué se hizo, en una o dos líneas.
- Motivo: el hallazgo del diagnóstico o la etapa del plan que lo pide.
- Archivos: los archivos creados o modificados.
- Verificación: cómo se comprobó.
- Decisión del equipo: pendiente

Para la fecha y la hora, ejecuta en la terminal: Get-Date -Format "yyyy-MM-dd HH:mm"
No borres ni cambies entradas anteriores. Si no hiciste cambios, no agregues entrada.