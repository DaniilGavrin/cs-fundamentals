# Алгоритмы

Алгоритмы — это сердце Computer Science. Это пошаговые инструкции для решения задач. Понимание алгоритмов позволяет выбирать оптимальные решения, оценивать их эффективность и создавать новые алгоритмы для уникальных задач. Как и структуры данных, алгоритмы **не зависят от языка программирования**.

---

## 1. Оценка сложности алгоритмов (Big O Notation) 📈

**Big O** описывает, как время выполнения или потребление памяти растет с увеличением размера входных данных (n).

### Основные классы сложности

```text
┌────────────────┬─────────────────────────┬───────────────────────────────┬───────────┬───────────┬───────────────────┐
│ Сложность      │ Название                │ Пример                        │ n=10      │ n=1000    │ n=1,000,000       │
├────────────────┼─────────────────────────┼───────────────────────────────┼───────────┼───────────┼───────────────────┤
│ **O(1)**       │ Константное             │ Доступ к элементу массива     │ 1         │ 1         │ 1                 │
│ **O(log n)**   │ Логарифмическое         │ Бинарный поиск                │ ~3        │ ~10       │ ~20               │
│ **O(n)**       │ Линейное                │ Поиск в массиве               │ 10        │ 1000      │ 1,000,000         │
│ **O(n log n)** │ Линейно-логарифмическое │ Быстрая сортировка            │ ~30       │ ~10,000   │ ~20,000,000       │
│ **O(n²)**      │ Квадратичное            │ Сортировка пузырьком          │ 100       │ 1,000,000 │ 1,000,000,000,000 │
│ **O(2ⁿ)**      │ Экспоненциальное        │ Перебор всех подмножеств      │ 1024      │ ❌         │ ❌                 │
│ **O(n!)**      │ Факториальное           │ Задача коммивояжера (перебор) │ 3,628,800 │ ❌         │ ❌                 │
└────────────────┴─────────────────────────┴───────────────────────────────┴───────────┴───────────┴───────────────────┘
```


> **Важно:** Big O описывает **худший случай** (upper bound). На практике алгоритм может работать быстрее.

### Правила упрощения Big O

1. **Отбрасываем константы:** `O(2n) = O(n)`, `O(n/2) = O(n)`
2. **Отбрасываем младшие члены:** `O(n² + n) = O(n²)`, `O(n³ + n² + n) = O(n³)`
3. **Сумма сложностей:** Если выполняются последовательно → складываем: `O(n) + O(n²) = O(n²)`
4. **Произведение сложностей:** Если вложены друг в друга → умножаем: `O(n) × O(n) = O(n²)`

### Примеры анализа

```python
# O(1) - Константное
def get_first(arr):
    return arr[0]

# O(n) - Линейное
def find_max(arr):
    max_val = arr[0]
    for x in arr:      # n итераций
        if x > max_val:
            max_val = x
    return max_val

# O(n²) - Квадратичное
def print_pairs(arr):
    for i in arr:          # n итераций
        for j in arr:      # n итераций
            print(i, j)    # Всего: n × n = n² операций

# O(log n) - Логарифмическое
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1   # Отбрасываем половину
        else:
            right = mid - 1  # Отбрасываем половину
    return -1
```

---

## 2. Алгоритмы сортировки 🔀

### Сравнительная таблица

```text
┌──────────────────────────┬────────────┬────────────┬────────────┬──────────┬────────────┬──────────────────────────────────┐
│ Алгоритм                 │ Лучший     │ Средний    │ Худший     │ Память   │ Стабильный │ Применение                       │
├──────────────────────────┼────────────┼────────────┼────────────┼──────────┼────────────┼──────────────────────────────────┤
│ **Пузырьковая**          │ O(n)       │ O(n²)      │ O(n²)      │ O(1)     │ ✅ Да       │ Только для обучения              │
│ **Выбором**              │ O(n²)      │ O(n²)      │ O(n²)      │ O(1)     │ ❌ Нет      │ Только для обучения              │
│ **Вставками**            │ O(n)       │ O(n²)      │ O(n²)      │ O(1)     │ ✅ Да       │ Малые массивы (<50)              │
│ **Быстрая (Quick)**      │ O(n log n) │ O(n log n) │ O(n²)      │ O(log n) │ ❌ Нет      │ **Универсальная**                │
│ **Слиянием (Merge)**     │ O(n log n) │ O(n log n) │ O(n log n) │ O(n)     │ ✅ Да       │ Linked lists, внешняя сортировка │
│ **Пирамидальная (Heap)** │ O(n log n) │ O(n log n) │ O(n log n) │ O(1)     │ ❌ Нет      │ Когда мало памяти                │
│ **Timsort**              │ O(n)       │ O(n log n) │ O(n log n) │ O(n)     │ ✅ Да       │ Python `.sort()`, Java `.sort()` │
└──────────────────────────┴────────────┴────────────┴────────────┴──────────┴────────────┴──────────────────────────────────┘
```


### Пузырьковая сортировка (Bubble Sort)

**Идея:** Многократно проходим по массиву, меняя местами соседние элементы, если они в неправильном порядке.

```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):
        swapped = False
        for j in range(0, n - i - 1):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
                swapped = True
        if not swapped:  # Оптимизация: если не было обменов, массив уже отсортирован
            break
```
- **Плюсы:** Простота реализации.
- **Минусы:** Очень медленно O(n²).
- **Применение:** Только в учебных целях.

### Сортировка вставками (Insertion Sort)

**Идея:** Берем элемент и вставляем его на правильное место среди уже отсортированных элементов слева.

```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        key = arr[i]
        j = i - 1
        while j >= 0 and arr[j] > key:
            arr[j + 1] = arr[j]
            j -= 1
        arr[j + 1] = key
```
- **Плюсы:** Эффективен на почти отсортированных данных O(n), стабилен, сортирует "на месте".
- **Минусы:** O(n²) в среднем случае.
- **Применение:** Малые массивы (<50 элементов), финальная стадия QuickSort.

### Быстрая сортировка (QuickSort)

**Идея:** Выбираем опорный элемент (pivot), разделяем массив на две части (меньше pivot и больше pivot), рекурсивно сортируем части.

```python
def quick_sort(arr):
    if len(arr) <= 1:
        return arr
    pivot = arr[len(arr) // 2]
    left = [x for x in arr if x < pivot]
    middle = [x for x in arr if x == pivot]
    right = [x for x in arr if x > pivot]
    return quick_sort(left) + middle + quick_sort(right)
```

**Оптимизированная версия (in-place):**
```python
def quick_sort_inplace(arr, low=0, high=None):
    if high is None:
        high = len(arr) - 1
    if low < high:
        pivot_index = partition(arr, low, high)
        quick_sort_inplace(arr, low, pivot_index - 1)
        quick_sort_inplace(arr, pivot_index + 1, high)
    return arr

def partition(arr, low, high):
    pivot = arr[high]
    i = low - 1
    for j in range(low, high):
        if arr[j] <= pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    arr[i + 1], arr[high] = arr[high], arr[i + 1]
    return i + 1
```
- **Плюсы:** Очень быстро на практике O(n log n), сортирует "на месте".
- **Минусы:** Нестабилен, худший случай O(n²) (решается рандомизацией pivot).
- **Применение:** Стандартная сортировка во многих языках.

### Сортировка слиянием (MergeSort)

**Идея:** Разделяем массив пополам, рекурсивно сортируем половины, сливаем отсортированные половины.

```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    return merge(left, right)

def merge(left, right):
    result = []
    i = j = 0
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    result.extend(left[i:])
    result.extend(right[j:])
    return result
```
- **Плюсы:** Гарантированный O(n log n), стабилен.
- **Минусы:** Требует O(n) дополнительной памяти.
- **Применение:** Сортировка linked lists, внешняя сортировка (когда данные не помещаются в RAM).

### Пирамидальная сортировка (HeapSort)

**Идея:** Строим max-heap из массива, многократно извлекаем максимум (корень) и перестраиваем кучу.

```python
def heap_sort(arr):
    n = len(arr)
    # Построение кучи
    for i in range(n // 2 - 1, -1, -1):
        heapify(arr, n, i)
    # Извлечение элементов
    for i in range(n - 1, 0, -1):
        arr[0], arr[i] = arr[i], arr[0]  # Перемещаем корень в конец
        heapify(arr, i, 0)
    return arr

def heapify(arr, n, i):
    largest = i
    left = 2 * i + 1
    right = 2 * i + 2
    if left < n and arr[left] > arr[largest]:
        largest = left
    if right < n and arr[right] > arr[largest]:
        largest = right
    if largest != i:
        arr[i], arr[largest] = arr[largest], arr[i]
        heapify(arr, n, largest)
```
- **Плюсы:** O(n log n) всегда, сортирует "на месте" O(1).
- **Минусы:** Нестабилен, медленнее QuickSort на практике.
- **Применение:** Когда критична память.

---

## 3. Алгоритмы поиска 🔍

### Линейный поиск (Linear Search)

**Идея:** Последовательно проверяем каждый элемент.

```python
def linear_search(arr, target):
    for i, val in enumerate(arr):
        if val == target:
            return i
    return -1
```
- **Сложность:** O(n)
- **Применение:** Несохраненные массивы, малые n.

### Бинарный поиск (Binary Search)

**Идея:** Сравниваем с серединой, отбрасываем половину, повторяем. **Только для отсортированных массивов!**

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
- **Сложность:** O(log n)
- **Применение:** Поиск в отсортированных данных, бинарный поиск по ответу.

### Поиск в ширину (BFS) и глубину (DFS) на графах

**BFS (Queue):**
```python
from collections import deque

def bfs(graph, start):
    visited = set()
    queue = deque([start])
    visited.add(start)
    while queue:
        vertex = queue.popleft()
        print(vertex)
        for neighbor in graph[vertex]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
```

**DFS (Stack/Recursion):**
```python
def dfs(graph, start, visited=None):
    if visited is None:
        visited = set()
    visited.add(start)
    print(start)
    for neighbor in graph[start]:
        if neighbor not in visited:
            dfs(graph, neighbor, visited)
```

---

## 4. Алгоритмы на графах 🕸️

### Алгоритм Дейкстры (кратчайший путь)

**Идея:** Жадный алгоритм для поиска кратчайшего пути во взвешенном графе без отрицательных весов.

```python
import heapq

def dijkstra(graph, start):
    distances = {node: float('inf') for node in graph}
    distances[start] = 0
    pq = [(0, start)]
    
    while pq:
        current_dist, current_node = heapq.heappop(pq)
        
        if current_dist > distances[current_node]:
            continue
        
        for neighbor, weight in graph[current_node].items():
            distance = current_dist + weight
            if distance < distances[neighbor]:
                distances[neighbor] = distance
                heapq.heappush(pq, (distance, neighbor))
    
    return distances
```
- **Сложность:** O((V+E) log V)
- **Применение:** GPS-навигация, маршрутизация в сетях.

---

## 5. Динамическое программирование (DP) 🧩

**Идея:** Разбиение задачи на перекрывающиеся подзадачи, решение каждой один раз и сохранение результата (memoization/tabulation).

### Классические задачи DP

**1. Числа Фибоначчи:**
```python
# ❌ Рекурсия без мемоизации: O(2^n)
def fib(n):
    if n <= 1:
        return n
    return fib(n-1) + fib(n-2)

# ✅ Мемоизация: O(n)
def fib_memo(n, memo={}):
    if n in memo:
        return memo[n]
    if n <= 1:
        return n
    memo[n] = fib_memo(n-1, memo) + fib_memo(n-2, memo)
    return memo[n]

# ✅ Табуляция: O(n)
def fib_tab(n):
    if n <= 1:
        return n
    dp = [0, 1]
    for i in range(2, n+1):
        dp.append(dp[i-1] + dp[i-2])
    return dp[n]
```

**2. Задача о рюкзаке (Knapsack):**
```python
def knapsack(weights, values, capacity):
    n = len(weights)
    dp = [[0]*(capacity+1) for _ in range(n+1)]
    
    for i in range(1, n+1):
        for w in range(capacity+1):
            if weights[i-1] <= w:
                dp[i][w] = max(dp[i-1][w], dp[i-1][w-weights[i-1]] + values[i-1])
            else:
                dp[i][w] = dp[i-1][w]
    
    return dp[n][capacity]
```

---

## 6. Жадные алгоритмы (Greedy) 💰

**Идея:** На каждом шаге выбираем локально оптимальное решение в надежде, что оно приведет к глобальному оптимуму.

**Пример: Размен монет (для канонических систем):**
```python
def coin_change(coins, amount):
    coins.sort(reverse=True)
    count = 0
    for coin in coins:
        count += amount // coin
        amount %= coin
    return count if amount == 0 else -1
```

**Когда работает:** Задача о рюкзаке (непрерывная), алгоритм Дейкстры, минимальное остовное дерево (Крускал, Прим).

**Когда НЕ работает:** Задача о рюкзаке (0/1), задача коммивояжера.

---

## 7. Разделяй и властвуй (Divide & Conquer) ⚔️

**Идея:** Разбиваем задачу на независимые подзадачи, решаем их рекурсивно, объединяем результаты.

**Примеры:**
- MergeSort, QuickSort
- Бинарный поиск
- Умножение больших чисел (алгоритм Карацубы)
- Поиск ближайшей пары точек

---

## 📊 Сводная таблица алгоритмов

```text
┌────────────────┬───────────────┬──────────────┬──────────────────────────────────┐
│ Категория      │ Алгоритм      │ Сложность    │ Применение                       │
├────────────────┼───────────────┼──────────────┼──────────────────────────────────┤
│ **Сортировка** │ QuickSort     │ O(n log n)   │ Универсальная                    │
│ **Сортировка** │ MergeSort     │ O(n log n)   │ Linked lists, внешняя            │
│ **Сортировка** │ HeapSort      │ O(n log n)   │ Мало памяти                      │
│ **Поиск**      │ Binary Search │ O(log n)     │ Отсортированные массивы          │
│ **Графы**      │ BFS           │ O(V+E)       │ Кратчайший путь (невзвешенный)   │
│ **Графы**      │ DFS           │ O(V+E)       │ Обход, топологическая сортировка │
│ **Графы**      │ Dijkstra      │ O((V+E)logV) │ Кратчайший путь (взвешенный)     │
│ **DP**         │ Fibonacci     │ O(n)         │ Пример перекрывающихся подзадач  │
│ **DP**         │ Knapsack      │ O(nW)        │ Оптимизация ресурсов             │
└────────────────┴───────────────┴──────────────┴──────────────────────────────────┘
```


---

## 🎯 Как выбрать алгоритм?

1. **Нужно отсортировать?** → QuickSort (универсально), MergeSort (linked list), HeapSort (мало памяти).
2. **Поиск в отсортированном?** → Binary Search O(log n).
3. **Кратчайший путь в графе?** → BFS (невзвешенный), Dijkstra (взвешенный без отрицательных весов).
4. **Задача с перекрывающимися подзадачами?** → Динамическое программирование.
5. **Можно решать локально оптимально?** → Жадный алгоритм.

> **Золотое правило:** Сначала поймите ограничения (время, память, размер данных), затем выберите алгоритм с подходящей сложностью.

---

## 💡 Практическое применение

1. **Базы данных:** Сортировка и слияние для JOIN, B-деревья для индексов.
2. **Поисковики:** PageRank (графы), индексация (хэш-таблицы, деревья).
3. **Сети:** Маршрутизация (Dijkstra, Bellman-Ford).
4. **Компрессия:** Huffman coding (жадный алгоритм).
5. **Криптография:** Быстрое возведение в степень (Divide & Conquer).
6. **Машинное обучение:** Градиентный спуск (оптимизация), кластеризация.

> **Запомните:** Знание алгоритмов не сделает вас программистом автоматически. Но оно даст вам инструменты для решения сложных задач эффективно. Практикуйтесь на LeetCode, Codeforces, HackerRank!
