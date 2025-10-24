# Documentación de Integración: Módulo de Desafíos

## Endpoints API

### 1. Obtener Todos los Desafíos
- **Método**: GET
- **Endpoint**: `/challenges`
- **Autenticación**: JWT Token requerido en header
- **Headers**:
  ```
  Authorization: Bearer <token>
  ```
- **Response**:
  ```typescript
  Challenge[] {
    id: string;
    name: string;
    description: string;
    count: number;
    target: number;
    reward: {
      coins: number;
      item: string;
    };
    status: 'active' | 'completed';
    createdAt: string;
    updatedAt: string;
  }
  ```

### 2. Obtener Desafío Específico
- **Método**: GET
- **Endpoint**: `/challenges/:id`
- **Autenticación**: JWT Token requerido en header
- **Headers**:
  ```
  Authorization: Bearer <token>
  ```
- **Response**: Objeto Challenge
- **Errores**:
  - 404: "Desafío no encontrado"

### 3. Crear Desafío
- **Método**: POST
- **Endpoint**: `/challenges`
- **Autenticación**: JWT Token requerido en header
- **Headers**:
  ```
  Authorization: Bearer <token>
  Content-Type: application/json
  ```
- **Body**:
  ```typescript
  {
    name: string;
    description: string;
    target: number;
    reward: {
      coins: number;
      item: string;
    }
  }
  ```
- **Response**: Objeto Challenge creado

### 4. Incrementar Contador de Desafío
- **Método**: PUT
- **Endpoint**: `/challenges/:id/count`
- **Autenticación**: JWT Token requerido en header
- **Headers**:
  ```
  Authorization: Bearer <token>
  ```
- **Response**: Objeto Challenge actualizado
- **Errores**:
  - 404: "Desafío no encontrado o ya completado"

## Implementación en el Frontend

### Hook useChallenge

```typescript
import { useState, useCallback } from 'react';

interface UseChallengeProps {
  onSuccess?: () => void;
  onError?: (error: Error) => void;
}

export const useChallenge = ({ onSuccess, onError }: UseChallengeProps) => {
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<Error | null>(null);
  const [challenges, setChallenges] = useState<Challenge[]>([]);

  const fetchChallenges = useCallback(async () => {
    try {
      setLoading(true);
      const response = await fetch('/api/challenges', {
        headers: {
          Authorization: `Bearer ${localStorage.getItem('token')}`,
        },
      });
      
      if (!response.ok) throw new Error('Error al obtener desafíos');
      
      const data = await response.json();
      setChallenges(data);
      onSuccess?.();
    } catch (err) {
      setError(err as Error);
      onError?.(err as Error);
    } finally {
      setLoading(false);
    }
  }, [onSuccess, onError]);

  const incrementChallenge = useCallback(async (id: string) => {
    try {
      setLoading(true);
      const response = await fetch(`/api/challenges/${id}/count`, {
        method: 'PUT',
        headers: {
          Authorization: `Bearer ${localStorage.getItem('token')}`,
        },
      });

      if (!response.ok) throw new Error('Error al actualizar desafío');

      const updatedChallenge = await response.json();
      setChallenges(prev => 
        prev.map(challenge => 
          challenge.id === id ? updatedChallenge : challenge
        )
      );
      onSuccess?.();
    } catch (err) {
      setError(err as Error);
      onError?.(err as Error);
    } finally {
      setLoading(false);
    }
  }, [onSuccess, onError]);

  return {
    challenges,
    loading,
    error,
    fetchChallenges,
    incrementChallenge
  };
};
```

### Uso en Componentes

```typescript
// En ChallengeSection.tsx
const ChallengeSection = () => {
  const { challenges, loading, error, fetchChallenges } = useChallenge({
    onError: (error) => {
      // Mostrar notificación de error
    }
  });

  useEffect(() => {
    fetchChallenges();
  }, [fetchChallenges]);

  if (loading) return <LoadingSpinner />;
  if (error) return <ErrorMessage error={error} />;

  return (
    <div>
      {challenges.map(challenge => (
        <ChallengeCard 
          key={challenge.id} 
          challenge={challenge} 
        />
      ))}
    </div>
  );
};

// En ChallengeCard.tsx
interface ChallengeCardProps {
  challenge: Challenge;
}

const ChallengeCard = ({ challenge }: ChallengeCardProps) => {
  const { incrementChallenge, loading } = useChallenge({
    onSuccess: () => {
      // Mostrar notificación de éxito
    },
    onError: (error) => {
      // Mostrar notificación de error
    }
  });

  const handleProgress = async () => {
    await incrementChallenge(challenge.id);
  };

  return (
    <Card>
      <h3>{challenge.name}</h3>
      <p>{challenge.description}</p>
      <ProgressBar 
        current={challenge.count} 
        target={challenge.target} 
      />
      <Button 
        onClick={handleProgress}
        disabled={loading || challenge.status === 'completed'}
      >
        {loading ? 'Actualizando...' : 'Registrar Progreso'}
      </Button>
      {challenge.status === 'completed' && <Badge type="success">Completado</Badge>}
    </Card>
  );
};
```

## Tipos TypeScript

```typescript
interface Challenge {
  id: string;
  name: string;
  description: string;
  count: number;
  target: number;
  reward: {
    coins: number;
    item: string;
  };
  status: 'active' | 'completed';
  createdAt: string;
  updatedAt: string;
}

interface CreateChallengeDto {
  name: string;
  description: string;
  target: number;
  reward: {
    coins: number;
    item: string;
  };
}
```

## Consideraciones Importantes

1. **Manejo de Estado**:
   - Usar el hook `useChallenge` para gestionar el estado global de los desafíos
   - Implementar caché si es necesario para mejorar el rendimiento
   - Considerar usar React Query o SWR para un mejor manejo del estado del servidor

2. **Autenticación**:
   - Asegurar que el token JWT esté disponible
   - Manejar la expiración del token
   - Redireccionar al login si el token no es válido

3. **UX/UI**:
   - Mostrar estados de carga apropiados
   - Implementar mensajes de error claros
   - Actualizar la UI inmediatamente al completar acciones
   - Mostrar notificaciones de éxito/error

4. **Optimizaciones**:
   - Implementar debounce en las actualizaciones frecuentes
   - Considerar la paginación si hay muchos desafíos
   - Usar memo para componentes que no necesitan re-renders frecuentes

5. **Seguridad**:
   - Validar todas las respuestas del servidor
   - No almacenar información sensible en el estado
   - Implementar timeouts en las llamadas API