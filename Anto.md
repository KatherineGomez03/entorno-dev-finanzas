# Antooooo
## Módulo de Recompensas/Logros (se llama rewards en el proyecto)

## Propósito del Módulo

1. **Motivación del Usuario**
   - Fomentar buenos hábitos financieros
   - Mantener el compromiso del usuario con sus metas de ahorro
   - Crear una experiencia interactiva y gratificante

2. **Tipos de Recompensas**


3. **Casos de Uso**
   - Recompensar al usuario por alcanzar una mision
   - Premiar la consistencia en el registro de gastos
   - Crear competencia sana entre usuarios
   - Incentivar el uso regular de la aplicación

4. **Sistema de Niveles**
   - **Nivel Principiante (1-5)**
     * Desbloqueo de consejos básicos financieros
     * Insignias de primeros pasos
     * Acceso a gráficos básicos
     * Temas básicos de personalización

   - **Nivel Intermedio (6-15)**
     * Herramientas de análisis de gastos avanzadas
     * Comparativas mensuales detalladas
     * Acceso a metas de ahorro grupales
     * Insignias de progreso destacado
     * Temas exclusivos de nivel intermedio

   - **Nivel Experto (16-25)**
     * Análisis predictivo de gastos
     * Reportes financieros personalizados
     * Herramientas de inversión básicas
     * Insignias de experto financiero
     * Temas premium exclusivos

   - **Nivel Maestro (26+)**
     * Asesoramiento financiero IA
     * Herramientas de planificación avanzada
     * Acceso a comunidad exclusiva
     * Insignias legendarias
     * Personalización completa de la interfaz

5. **Integración con el Sistema**
   - Se conecta con el módulo de usuarios para tracking de progreso
   - Interactúa con el sistema de transacciones para validar logros
   - Permite personalización de la experiencia del usuario
   - Facilita la retención de usuarios a largo plazo

## 1. Estructura Base
- [x] Esquema de recompensas definido
- [ ] Crear `rewards.module.ts`
- [ ] Crear `rewards.service.ts`
- [ ] Crear `rewards.controller.ts`

## 2. DTOs (Data Transfer Objects)
Crear en la carpeta `dto/`:
- [ ] `create-reward.dto.ts`
  - Validaciones para nombre, descripción, tipo y costo
  - Validación de tipo usando el enum RewardType
  - Validación opcional para imageUrl (debe ser URL válida)
  - Validación para conditions (debe ser objeto válido)

- [ ] `update-reward.dto.ts`
  - Extender de create-reward.dto
  - Hacer todos los campos opcionales
  
- [ ] `reward-response.dto.ts`
  - DTO para transformar la respuesta
  - Incluir transformación de fechas
  - Excluir campos sensibles si los hubiera

## 3. Servicio (rewards.service.ts)
Implementar los siguientes métodos:
- [ ] `create(createRewardDto)`
- [ ] `findAll(filters?)`
- [ ] `findOne(id)`
- [ ] `update(id, updateRewardDto)`
- [ ] `remove(id)`
- [ ] `unlockReward(userId, rewardId)`
- [ ] `checkUserEligibility(userId, rewardId)`
- [ ] `getUnlockedRewards(userId)`
- [ ] `getAvailableRewards(userId)`

## 4. Controlador (rewards.controller.ts)
Implementar endpoints:
- [ ] POST `/rewards` - Crear recompensa
- [ ] GET `/rewards` - Listar todas las recompensas
- [ ] GET `/rewards/:id` - Obtener una recompensa
- [ ] PATCH `/rewards/:id` - Actualizar recompensa
- [ ] DELETE `/rewards/:id` - Eliminar recompensa
- [ ] POST `/rewards/:id/unlock` - Desbloquear recompensa
- [ ] GET `/rewards/user/:userId` - Obtener recompensas del usuario
- [ ] GET `/rewards/available` - Obtener recompensas disponibles

## 5. Validaciones y Lógica de Negocio
- [ ] Implementar validación de condiciones para desbloquear recompensas
- [ ] Verificar saldo/puntos del usuario antes de desbloquear recompensas
- [ ] Manejar expiración de recompensas
- [ ] Implementar lógica para recompensas tipo BONUS
- [ ] Validar disponibilidad antes de permitir desbloqueo

## 6. Integración
- [ ] Integrar con el módulo de usuarios
- [ ] Actualizar el módulo de autenticación para incluir permisos de recompensas
- [ ] Añadir middleware de validación de permisos
- [ ] Integrar con el sistema de notificaciones (si existe)

## 7. Pruebas
- [ ] Crear pruebas unitarias para el servicio
- [ ] Crear pruebas e2e para los endpoints
- [ ] Probar diferentes escenarios de desbloqueo
- [ ] Probar expiración de recompensas
- [ ] Probar validaciones de condiciones

## 8. Documentación (hace un .md plis!)
- [ ] Documentar endpoints
- [ ] Documentar tipos de recompensas
- [ ] Documentar condiciones posibles
- [ ] Crear ejemplos de uso
- [ ] Documentar integración con otros módulos

## Notas Importantes
- Asegúrate de manejar correctamente las transacciones al desbloquear recompensas
- Implementa logging para acciones importantes
- Considera implementar caché para recompensas frecuentemente accedidas
- Mantén un historial de recompensas desbloqueadas
- Considera implementar webhooks para eventos importantes

## Consideraciones de Seguridad
- Validar permisos en cada endpoint
- Implementar rate limiting para endpoints críticos
- Validar input en todos los endpoints
- Asegurar que usuarios solo puedan ver/modificar sus propias recompensas
- Implementar auditoría de acciones críticas

