# VECTORIA — Unity Project Conventions

## Coordenadas y física
- Posiciones en `lat/lon double` en AircraftState. Conversión a world en CoordConverter.cs
- Altitud siempre en `feet (float)`. Convertir a FL solo para display (FL = feet / 100)
- Velocidad en `knots (float)` internamente. Nunca m/s en domain logic
- Distancias en `NM (nautical miles)` para separación. Nunca unidades world directamente
- Heading en `degrees 0-359 float`. 0 = Norte magnético

## Arquitectura
- AircraftEntity (MonoBehaviour) nunca contiene lógica de dominio — delega a AircraftState
- AircraftState (plain C# class) es serializable, testeable, sin dependencias Unity
- Eventos de dominio via `C# Action<T>`, no UnityEvent
- Nunca usar `GameObject.Find` — siempre inyección o [SerializeField]
- UI solo lee estado, nunca lo modifica directamente

## Rendering
- GL.Lines en OnRenderObject() para el radar scope. Nunca LineRenderer para blips
- Material: Shader.Find("Hidden/Internal-Colored") con lazy init en EnsureMaterial()
- Colores desde VColors.cs — nunca hardcodear Color values en los renderers
- RadarRenderer no conoce GameObjects de aviones — recibe posiciones como Vector2[]

## Naming
- Clases: PascalCase — AircraftEntity, ConflictDetector
- Métodos públicos: PascalCase — GetSeparationNM()
- Campos privados: _camelCase — _currentHeading
- Constantes: SCREAMING_SNAKE — MIN_SEPARATION_NM
- JSON keys: snake_case — { "icao_code": "SKBO" }

## Testing
- ConflictDetector tiene cobertura de tests unitarios >80%
- AircraftPhysics tiene tests de integración por tipo de maniobra
- Nunca mergear a main con tests fallidos
- Tests en Assets/Tests/EditMode/ para lógica pura