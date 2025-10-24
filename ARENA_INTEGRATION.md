# Documentación de Integración del Módulo Arena

## Estado Actual del Backend (Módulo Sandbox)

### Endpoints Existentes

#### Iniciar Batalla
- **Endpoint**: `/sandbox/battle/start`
- **Método**: POST
- **Headers Requeridos**: Authorization Bearer Token
- **Cuerpo de la Petición**:
```typescript
{
  userId: string;
  monthlyEnemy: {
    name: string;
    level: number;
    hp: number;
    atk: number;
    abilities: string[];
    rewards: string[];
  }
}
```
- **Respuesta**:
```typescript
{
  battleId: string;
  player: {
    hp: number;
    maxHp: number;
    atk: number;
  };
  enemy: {
    hp: number;
    maxHp: number;
    atk: number;
    name: string;
    level: number;
    abilities: string[];
    rewards: string[];
  }
}
```

#### Atacar Enemigo
- **Endpoint**: `/sandbox/battle/:battleId/attack`
- **Método**: POST
- **Headers Requeridos**: Authorization Bearer Token
- **Respuesta**:
```typescript
{
  damage: number;
  playerHP: number;
  enemyHP: number;
  turnResult: {
    isPlayerTurn: boolean;
    log: string;
  }
  battleStatus: 'ONGOING' | 'PLAYER_WIN' | 'ENEMY_WIN';
}
```

#### Usar Item
- **Endpoint**: `/sandbox/battle/:battleId/use-item`
- **Método**: POST
- **Headers Requeridos**: Authorization Bearer Token
- **Cuerpo de la Petición**:
```typescript
{
  itemType: 'HEALTH_POTION' | 'POISON_POTION';
}
```
- **Respuesta**:
```typescript
{
  itemEffect: {
    type: string;
    value: number;
  };
  playerHP?: number;
  enemyHP?: number;
  remainingItems: {
    healthPotions: number;
    poisonPotions: number;
  }
}
```

#### Reiniciar Batalla
- **Endpoint**: `/sandbox/battle/:battleId/reset`
- **Método**: POST
- **Headers Requeridos**: Authorization Bearer Token
- **Respuesta**: Igual que Iniciar Batalla

## Cambios Requeridos en el Frontend

### 1. Crear Servicio de Arena
```typescript
// services/arenaService.ts
export interface ArenaService {
  iniciarBatalla(): Promise<EstadoBatalla>;
  atacar(battleId: string): Promise<ResultadoAtaque>;
  usarItem(battleId: string, tipoItem: TipoItem): Promise<ResultadoUsoItem>;
  reiniciarBatalla(battleId: string): Promise<EstadoBatalla>;
}
```

### 2. Crear Hook Personalizado
```typescript
// hooks/useArenaBattle.ts
export const useArenaBattle = () => {
  const [estadoBatalla, setEstadoBatalla] = useState<EstadoBatalla | null>(null);
  const [cargando, setCargando] = useState(false);
  const [error, setError] = useState<string | null>(null);

  const iniciarBatalla = async () => {
    // Implementación
  };

  const atacar = async () => {
    // Implementación
  };

  const usarItem = async (tipoItem: TipoItem) => {
    // Implementación
  };

  const reiniciarBatalla = async () => {
    // Implementación
  };

  return {
    estadoBatalla,
    cargando,
    error,
    iniciarBatalla,
    atacar,
    usarItem,
    reiniciarBatalla
  };
};
```

### 3. Interfaces TypeScript Requeridas
```typescript
// types/arena.ts
export interface EstadoBatalla {
  battleId: string;
  player: {
    hp: number;
    maxHp: number;
    atk: number;
  };
  enemy: EstadoEnemigo;
  turnState: {
    isPlayerTurn: boolean;
    log: string[];
  };
  items: {
    healthPotions: number;
    poisonPotions: number;
  };
}

export interface EstadoEnemigo {
  hp: number;
  maxHp: number;
  atk: number;
  name: string;
  level: number;
  abilities: string[];
  rewards: string[];
}

export type TipoItem = 'HEALTH_POTION' | 'POISON_POTION';

export interface ResultadoAtaque {
  damage: number;
  playerHP: number;
  enemyHP: number;
  turnResult: {
    isPlayerTurn: boolean;
    log: string;
  };
  battleStatus: 'ONGOING' | 'PLAYER_WIN' | 'ENEMY_WIN';
}

export interface ResultadoUsoItem {
  itemEffect: {
    type: string;
    value: number;
  };
  playerHP?: number;
  enemyHP?: number;
  remainingItems: {
    healthPotions: number;
    poisonPotions: number;
  };
}
```

### 4. Cambios Necesarios en ArenaSection.tsx

1. Reemplazar la gestión de estado local con el hook `useArenaBattle`:
```typescript
const {
  estadoBatalla,
  cargando,
  error,
  iniciarBatalla,
  atacar,
  usarItem,
  reiniciarBatalla
} = useArenaBattle();
```

2. Actualizar los manejadores de eventos:
- Reemplazar `atacar()` con `atacar()`
- Reemplazar `usarPocionVida()` con `usarItem('HEALTH_POTION')`
- Reemplazar `usarPocionVeneno()` con `usarItem('POISON_POTION')`
- Reemplazar `reset()` con `reiniciarBatalla()`

3. Actualizar referencias de estado:
- Reemplazar variables de estado locales con propiedades de `estadoBatalla`
- Actualizar UI para manejar estados de carga
- Agregar manejo de errores y su visualización

4. Agregar useEffect para la inicialización de la batalla:
```typescript
useEffect(() => {
  if (hoyEsBatalla && !estadoBatalla) {
    iniciarBatalla();
  }
}, [hoyEsBatalla]);
```

### 5. Manejo de Errores

Agregar manejo de errores para:
- Llamadas API fallidas
- Estados de batalla inválidos
- Acciones no autorizadas
- Errores de red

### 6. Estados de Carga

Agregar indicadores de carga para:
- Carga inicial de la batalla
- Acciones de ataque
- Uso de items
- Reinicio de batalla

### 7. Configuración de Entorno

Agregar a tus variables de entorno:
```env
NEXT_PUBLIC_API_URL=http://tu-backend-url
```

## Consideraciones Adicionales

1. **Gestión de Estado**:
- Considerar usar React Query para la gestión del estado del servidor
- Implementar actualizaciones optimistas para mejor UX
- Cachear el estado de batalla apropiadamente

2. **Recuperación de Errores**:
- Implementar lógica de reintentos para peticiones fallidas
- Agregar manejo de reconexión para desconexiones
- Guardar estado de batalla localmente para recuperación

3. **Rendimiento**:
- Implementar debouncing para acciones rápidas
- Cachear datos del enemigo
- Optimizar re-renders

4. **Seguridad**:
- Validar todas las respuestas del servidor
- Implementar límites de error apropiados
- Agregar timeouts en las peticiones

## Próximos Pasos Recomendados

1. Implementar el servicio de Arena
2. Crear y probar el hook useArenaBattle
3. Actualizar el componente ArenaSection
4. Implementar manejo de errores
5. Agregar indicadores de carga
6. Realizar pruebas de integración