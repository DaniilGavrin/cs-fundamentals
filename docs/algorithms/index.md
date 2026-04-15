# Алгоритмы и структуры данных

## Big O нотация

Оценка сложности алгоритмов по времени и памяти.

| Сложность | Название | Пример |
|-----------|----------|--------|
| O(1) | Константная | Доступ к элементу массива |
| O(log n) | Логарифмическая | Бинарный поиск |
| O(n) | Линейная | Поиск в массиве |
| O(n log n) | Линейно-логарифмическая | Быстрая сортировка |
| O(n²) | Квадратичная | Сортировка пузырьком |
| O(2ⁿ) | Экспоненциальная | Задача коммивояжёра |

## Основные структуры данных

### Массивы (Arrays)
- Непрерывная область памяти
- Быстрый доступ по индексу O(1)
- Медленная вставка/удаление O(n)

### Связные списки (Linked Lists)
- Элементы связаны указателями
- Быстрая вставка/удаление O(1)
- Медленный доступ O(n)

### Стеки (Stack)
- LIFO (Last In, First Out)
- Операции: push, pop, peek

### Очереди (Queue)
- FIFO (First In, First Out)
- Операции: enqueue, dequeue

### Хэш-таблицы (Hash Tables)
- Быстрый поиск, вставка, удаление O(1) в среднем
- Коллизии решаются цепочками или открытой адресацией

### Деревья (Trees)
- Иерархическая структура
- Бинарные деревья поиска (BST)
- Сбалансированные деревья (AVL, Red-Black)

### Графы (Graphs)
- Вершины и рёбра
- Обход: DFS, BFS
- Алгоритмы: Дейкстра, Прима, Крускала

## Алгоритмы сортировки

```bash
# Пузырьковая сортировка - O(n²)
for i in range(n):
    for j in range(0, n-i-1):
        if arr[j] > arr[j+1]:
            arr[j], arr[j+1] = arr[j+1], arr[j]

# Быстрая сортировка - O(n log n)
def quicksort(arr):
    if len(arr) <= 1:
        return arr
    pivot = arr[len(arr) // 2]
    left = [x for x in arr if x < pivot]
    middle = [x for x in arr if x == pivot]
    right = [x for x in arr if x > pivot]
    return quicksort(left) + middle + quicksort(right)
```

## Алгоритмы поиска

### Линейный поиск
```python
def linear_search(arr, target):
    for i, val in enumerate(arr):
        if val == target:
            return i
    return -1
```

### Бинарный поиск
```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```
