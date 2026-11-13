---
name: cinematic-scene-compositing
description: "Глубина, маски и перекрытия. Использовать для задач профессиональной 2D-анимации, когда требуется глубина, маски и перекрытия."
---

# Глубина, маски и перекрытия

## Входные данные
Изучи задачу пользователя, доступный стек, длительность, fps, референсы и ограничения. Не запускай рендер, если запрошена только библиотека или документация.

## Процедура
Создай scene graph и локальные группы: environment, actors, hand props, foreground, lights, screen overlays. Сортируй по world foot Y только внутри сопоставимых плоскостей; мосты и окна требуют отдельных групп и masks. Window pass: outside → creature clipped to opening → glass/reflections → frame → interior hero. Для реквизита вычисляй world=parentWorld*socketLocal*propLocal. Используй premultiplied alpha последовательно.

## Практика реализации
Все параметры и состояния вычисляй из абсолютного времени. Разделяй model, world и screen space. В Remotion используй useCurrentFrame()/useVideoConfig(); в PixiJS применяй локальные transforms контейнеров. Не полагайся на CSS transition или накопление deltaTime при offline seek. Храни параметры в JSON, проверяй границы диапазонов и воспроизводимость.

```ts
// Общий контракт события; один timestamp для картинки и звука.
type Event = {id: string; time: number; duration: number};
const progress = (t: number, e: Event) =>
  Math.max(0, Math.min(1, (t - e.time) / Math.max(e.duration, 1e-6)));
```

## Проверка качества
Рука не уходит за корпус при контакте; заоконный персонаж не закрывает раму; нет светлых alpha fringes; тестируй пересечения на промежуточных кадрах.

Сохрани параметры, контрольные кадры/метрики только если задача включает видеопроизводство. Не объявляй профессиональное качество доказанным одним автоматическим тестом. При недоступном референсе или инструменте явно зафиксируй ограничение.

## Сочетания
advanced-character-animation, cinematic-2d-lighting.
