# Módulo de Desafíos (Challenges)

## Propósito del Módulo
El módulo de desafíos es una parte crucial del sistema de gamificación que busca motivar a los usuarios a mantener hábitos financieros saludables a través de retos estructurados.

### Tipos de Desafíos
1. **Diarios (DAILY)**
   - Retos pequeños y alcanzables
   - Ejemplo: "Registra todos tus gastos del día"
   - Recompensas rápidas para mantener el engagement

2. **Semanales (WEEKLY)**
   - Objetivos de mediano plazo
   - Ejemplo: "Ahorra el 10% de tus ingresos esta semana"
   - Balance entre dificultad y recompensa

3. **Mensuales (MONTHLY)**
   - Metas más ambiciosas
   - Ejemplo: "Reduce gastos en entretenimiento un 20%"
   - Mayores recompensas y puntos

4. **Especiales (SPECIAL)**
   - Eventos únicos o temporales
   - Ejemplo: "Reto de fin de año: Plan de ahorro navideño"
   - Recompensas exclusivas

### Sistema de Recompensas
- **Puntos**: Para subir de nivel y desbloquear funciones
- **Diamantes**: Moneda premium para recompensas especiales

### Estados de los Desafíos
- **ACTIVE**: Desafío en curso
- **COMPLETED**: Completado exitosamente
- **FAILED**: No se cumplieron los objetivos
- **EXPIRED**: Tiempo límite alcanzado

## Pasos de Implementación

### 1. Estructura Base (Ya implementada)
- [x] Schema de desafíos definido
- [ ] Crear `challenges.module.ts`
- [ ] Crear `challenges.service.ts`
- [ ] Crear `challenges.controller.ts`

### 2. DTOs
Crear en carpeta `dto/`:
- [ ] `create-challenge.dto.ts`
  ```typescript
  - title: string
  - description: string
  - type: ChallengeType
  - points: number
  - diamonds: number
  - startDate: Date
  - endDate: Date
  - conditions: object
  ```
- [ ] `update-challenge.dto.ts`
- [ ] `challenge-response.dto.ts`

### 3. Servicio (challenges.service.ts)
Implementar:
- [ ] `createChallenge()`
- [ ] `getAllChallenges()`
- [ ] `getChallengeById()`
- [ ] `getUserChallenges()`
- [ ] `joinChallenge()`
- [ ] `updateChallengeProgress()`
- [ ] `completeChallenge()`
- [ ] `checkChallengeStatus()`
- [ ] `awardChallengeRewards()`

### 4. Controlador (challenges.controller.ts)
Endpoints:
- [ ] POST `/challenges` - Crear nuevo desafío
- [ ] GET `/challenges` - Listar desafíos
- [ ] GET `/challenges/:id` - Obtener desafío específico
- [ ] GET `/challenges/user/:userId` - Desafíos del usuario
- [ ] POST `/challenges/:id/join` - Unirse a un desafío
- [ ] PATCH `/challenges/:id/progress` - Actualizar progreso
- [ ] POST `/challenges/:id/complete` - Completar desafío

### 5. Sistema de Validación
- [ ] Validar fechas (inicio/fin)
- [ ] Verificar elegibilidad del usuario
- [ ] Validar condiciones de completitud
- [ ] Verificar no duplicación de participación
- [ ] Validar estados de transición

### 6. Integración
- [ ] Con módulo de usuarios
- [ ] Con sistema de recompensas
- [ ] Con sistema de notificaciones
- [ ] Con sistema de logros

### 7. Pruebas
- [ ] Pruebas unitarias del servicio
- [ ] Pruebas de integración
- [ ] Pruebas e2e
- [ ] Pruebas de validación de estados

## Ejemplos de Desafíos

### Desafío Diario
```json
{
  "title": "Registro Diario",
  "description": "Registra todos tus gastos de hoy",
  "type": "daily",
  "points": 50,
  "diamonds": 1,
  "conditions": {
    "minTransactions": 3,
    "requireCategories": true
  }
}
```

### Desafío Semanal
```json
{
  "title": "Semana de Ahorro",
  "description": "Ahorra 10% de tus ingresos",
  "type": "weekly",
  "points": 200,
  "diamonds": 5,
  "conditions": {
    "savingsPercentage": 10,
    "baseAmount": "userIncome"
  }
}
```

## Consideraciones de Seguridad
1. Validar permisos de creación/modificación
2. Prevenir manipulación de progreso
3. Verificar integridad de datos
4. Implementar rate limiting
5. Auditar acciones críticas

## Monitoreo y Métricas
- Tasa de participación en desafíos
- Porcentaje de completitud
- Desafíos más populares
- Tasa de abandono
- Impacto en retención de usuarios