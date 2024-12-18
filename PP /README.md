

# Расчетная работа 

## Цель: 🧐

Ознакомиться с основами теории графов, способами представления графов, базовыми алгоритмами для работы с разными видами графов.

## Условие задания: 🤯

>*Вариант 5.29*

Реализовать на C++ код, который может найти дерево кратчайших путей.

Используется неориентированный граф.

Граф представляется в виде списка инцидентности.

### Ключевые понятия 🙈
`Граф` - математическая абстракция реальной системы любой природы, объекты которой обладают парными связями.

`Неориентированный граф` —  граф, рёбрам которого не присвоено направление.

`Список смежности(инцидентности)` - один из способов представления графа в виде коллекции списков вершин. Каждой вершине графа соответствует список, состоящий из «соседей» этой вершины.

`Дерево` - это связный граф без циклов, петель и кратных рёбер.

`О́стовное де́рево графа` — это дерево, подграф данного графа, с тем же числом вершин, что и у исходного графа.

`Подграф` — это часть графа, в которой мы берем некоторые его вершины и ребра.

`Путь в графе` – это последовательность рёбер, в которой конец каждого ребра (кроме последнего) совпадает с началом следующего.
Замкнутый путь называется циклом. 

`Цикл` — граф, состоящий из единственного цикла, или, другими словами, некоторого числа вершин, соединённых замкнутой цепью

`Смежность` — понятие, используемое в отношении только двух рёбер либо только двух вершин: Два ребра, инцидентные одной вершине, называются смежными; две вершины, инцидентные одному ребру, также называются смежными. 

`Инцидентность` - вершина и ребро называются инцидентными, если вершина является для этого ребра концевой.

## Алгоритм Прима 😜

Решение задачи основано на алгоритме Прима.

# Описание алгоритма 🥰

Сам алгоритм имеет очень простой вид. Искомый минимальный остов строится постепенно, добавлением в него рёбер по одному. Изначально остов полагается состоящим из единственной вершины (её можно выбрать произвольно). Затем выбирается ребро минимального веса, исходящее из этой вершины, и добавляется в минимальный остов. После этого остов содержит уже две вершины, и теперь ищется и добавляется ребро минимального веса, имеющее один конец в одной из двух выбранных вершин, а другой — наоборот, во всех остальных, кроме этих двух. И так далее, т.е. всякий раз ищется минимальное по весу ребро, один конец которого — уже взятая в остов вершина, а другой конец — ещё не взятая, и это ребро добавляется в остов (если таких рёбер несколько, можно взять любое). Этот процесс повторяется до тех пор, пока остов не станет содержать все вершины (или, что то же самое, n-1 ребро).

В итоге будет построен остов, являющийся минимальным. Если граф был изначально не связен, то остов найден не будет (количество выбранных рёбер останется меньше n-1).

Исходный граф:

![{30BE580D-6C92-4E0F-963D-22AC2E6B711E}](https://github.com/user-attachments/assets/c510f25e-b70b-4b50-a7c3-906afc04dbd5)


### Шаг 1: Начальная вершина

- **Выбираем вершину** A как начальную.
- Инцидентные ребра:
  - A → D (5)
  - A → B (7)
- Минимальное ребро: **A → D (5)**.
- Добавляем его в дерево \(T = \{ (A, D) \}\).

### Шаг 2: Добавление вершины D

- **Вершина D** инцидентна ребрам:
  - D → B (9)
  - D → E (15)
  - D → F (6)
- Минимальное ребро: **D → F (6)**.
- Добавляем его в дерево \(T = \{ (A, D), (D, F) \}\).

### Шаг 3: Добавление вершины F

- **Вершина F** инцидентна ребрам:
  - F → E (8)
  - F → G (11)
- Минимальное ребро: **F → E (8)**.
- Добавляем его в дерево \(T = \{ (A, D), (D, F), (F, E) \}\).

### Шаг 4: Добавление вершины E

- **Вершина E** инцидентна ребрам:
  - E → B (7)
  - E → C (5)
  - E → G (9)
- Минимальное ребро: **E → C (5)**.
- Добавляем его в дерево \(T = \{ (A, D), (D, F), (F, E), (E, C) \}\).

### Шаг 5: Добавление вершины C

- Единственное оставшееся минимальное ребро: **E → G (9)**.
- Добавляем его в дерево \(T = \{ (A, D), (D, F), (F, E), (E, C), (E, G) \}\).

### Итоговое минимальное остовное дерево 😱

Ребра остовного дерева:

T = \{ (A, D), (D, F), (F, E), (E, C), (E, G) \}


![{CC6C7562-581F-49AD-8B29-B45C30873DE3}](https://github.com/user-attachments/assets/093ae4c9-0035-44b0-931f-fd874ad2d566)

## Реализация на C++

Код, выполняющий алгоритм
``` c++
#include <iostream>
#include <vector>
#include <utility>
#include <limits>

using namespace std;

class Graph {
public:
    int V;  // Количество вершин
    int E;  // Количество рёбер
    vector<vector<int>> incidenceMatrix;  // Матрица инцидентности

    Graph(int V, int E) : V(V), E(E), incidenceMatrix(V, vector<int>(E, 0)), adjList(V) {}

    // Метод для построения графа на основе матрицы инцидентности
    void addEdgeFromIncidenceMatrix() {
        for (int j = 0; j < E; ++j) {
            int u = -1, v = -1;  // Две инцидентные вершины для каждого ребра

            // Ищем две вершины, инцидентные этому ребру
            for (int i = 0; i < V; ++i) {
                if (incidenceMatrix[i][j] == 1) {
                    if (u == -1)
                        u = i;  // Первая инцидентная вершина
                    else
                        v = i;  // Вторая инцидентная вершина
                }
            }

            if (u != -1 && v != -1) {
                int weight;
                cout << "Введите вес для ребра (" << u << ", " << v << "): ";
                while (!(cin >> weight) || weight <= 0) {
                    cin.clear();
                    cin.ignore(numeric_limits<streamsize>::max(), '\n');
                    cout << "Некорректный ввод. Введите положительное значение веса: ";
                }
                // Добавляем ребро в список смежности
                adjList[u].push_back(make_pair(v, weight));
                adjList[v].push_back(make_pair(u, weight));
            }
        }
    }

    // Список смежности для алгоритма Прима
    vector<vector<pair<int, int>>> adjList;
};

// Алгоритм Прима для построения минимального остовного дерева
void primMST(Graph& graph) {
    vector<bool> inMST(graph.V, false);      // Вершины, входящие в MST
    vector<int> key(graph.V, INT_MAX);       // Минимальные веса для каждой вершины
    vector<int> parent(graph.V, -1);         // Родительские вершины для MST

    key[0] = 0;  // Начинаем с вершины 0

    for (int count = 0; count < graph.V - 1; ++count) {
        int minKey = INT_MAX;
        int u = -1;

        // Находим вершину с минимальным значением ключа
        for (int v = 0; v < graph.V; ++v) {
            if (!inMST[v] && key[v] < minKey) {
                minKey = key[v];
                u = v;
            }
        }

        if (u == -1) break;  // Если все вершины посещены, завершаем цикл

        inMST[u] = true;

        // Обновляем ключевые значения и родительские вершины соседей
        for (const auto& neighbor : graph.adjList[u]) {
            int v = neighbor.first;
            int weight = neighbor.second;

            if (!inMST[v] && weight < key[v]) {
                parent[v] = u;
                key[v] = weight;
            }
        }
    }

    // Вывод минимального остовного дерева
    cout << "Минимальное остовное дерево (MST):\n";
    for (int i = 1; i < graph.V; ++i) {
        if (parent[i] != -1) {
            cout << parent[i] << " - " << i << endl;
        }
    }
}

int main() {
    system("chcp 1251 > nul");
    int n, m;

    cout << "Введите количество вершин: ";
    while (!(cin >> n) || n <= 0) {
        cin.clear();
        cin.ignore(numeric_limits<streamsize>::max(), '\n');
        cout << "Некорректный ввод. Введите положительное число вершин: ";
    }

    cout << "Введите количество рёбер: ";
    while (!(cin >> m) || m <= 0) {
        cin.clear();
        cin.ignore(numeric_limits<streamsize>::max(), '\n');
        cout << "Некорректный ввод. Введите положительное число рёбер: ";
    }

    Graph graph(n, m);

    cout << "Введите матрицу инцидентности (" << n << " строк, " << m << " столбцов):\n";
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < m; ++j) {
            while (!(cin >> graph.incidenceMatrix[i][j]) || (graph.incidenceMatrix[i][j] != 0 && graph.incidenceMatrix[i][j] != 1)) {
                cin.clear();
                cin.ignore(numeric_limits<streamsize>::max(), '\n');
                cout << "Некорректный ввод. Введите 0 или 1: ";
            }
        }
    }

    graph.addEdgeFromIncidenceMatrix();

    primMST(graph);

    return 0;
}
```
## Разбор кода: 😈
- `#include <iostream>` - библиотека для ввода \ вывода
- 
- `#include <vector>` - библиотека позволяющая использовать векторы

- `#include <utility>` - библиотека необходимая для использования пар объектов

- `using namespace std` - подключение пространства имен `std`

- `#include <limits>` — подключение для работы с максимально возможными значениями (например, INT_MAX).
-  `class Graph {` - объявление класса `Graph`, при помощи которого список смежности(инцидентности), фактически в виде матрицы, будет сохраняться в памяти компьютера
   - `int V` — количество вершин в графе.
   - `int E` — количество рёбер в графе.
   - `vector<vector<int>> incidenceMatrix`  матрица инцидентности, которая представляет связь между рёбрами и вершинами. В этой матрице строки соответствуют 
 вершинам, а столбцы рёбрам. Если вершина инцидентна ребру, то в матрице будет стоять 1, иначе — 0.
- `addEdgeFromIncidenceMatrix()` — метод для построения графа на основе матрицы инцидентности. Он находит пары вершин, инцидентные каждому ребру, и добавляет эти рёбра в список смежности `(adjList)`, а также запрашивает у пользователя вес рёбер.
- `void primMST(Graph& graph)`- Объявление функции `primMST`, которая находит дерево кратчайших путей.
  - `vector<bool> inMST` — вектор, который отслеживает, вошла ли вершина в минимальное остовное дерево (MST).
  - `vector<int> key` — вектор ключевых значений для каждой вершины, инициализированный максимальными значениями. Эти значения будут обновляться, когда будет найдено более короткое соединение для вершины.
  - `vector<int> parent` — вектор, который хранит информацию о родительских вершинах для каждой вершины в дереве.
  - Цикл `for` — основной цикл, который находит вершины с минимальными весами, не включёнными в MST, и обновляет ключи для их соседей. Этот процесс повторяется до тех пор, пока все вершины не будут добавлены в MST.
- Основная функция `main`:
    -Ввод данных: Пользователь вводит количество вершин и рёбер, а затем матрицу инцидентности (матрицу связи между вершинами и рёбрами). Ввод матрицы инцидентности производится построчно, и для каждого элемента проверяется корректность ввода (0 или 1).
  - Создание графа: После ввода матрицы инцидентности, вызывается метод `addEdgeFromIncidenceMatrix()`, который на основе матрицы инцидентности строит граф в виде списка смежности.
  - Запуск алгоритма Прима: После этого вызывается функция `primMST(graph)`, которая находит минимальное остовное дерево с использованием алгоритма Прима.
- Итог:
Программа позволяет пользователю вводить граф в виде матрицы инцидентности, затем строит граф, используя эту матрицу, и находит минимальное остовное дерево (MST) с помощью алгоритма Прима. В процессе работы программы также запрашиваются веса рёбер для построения списка смежности, а затем выводится минимальное остовное дерево.

### Пример работы программы 😵

- Возьмем граф:



![надо](https://github.com/user-attachments/assets/77ed0302-205f-4ed5-9beb-e1c89f687ee0)

- Составим для него матрицу инцидентности:



![{7F5F7801-12F1-4F8A-94AF-C96A8F151C4D}](https://github.com/user-attachments/assets/e0bd9c08-014a-4182-a7b4-e94700f8512b)

- Запускаем прогрмму вносим все необходимые значения.




![{87D55169-B1A6-41AC-A6DC-3C4E2050A432}](https://github.com/user-attachments/assets/b5071f79-9b72-467c-93a8-3363aa8d15e2)
- Получаем результат:



 ![{B62CF8B9-F75D-4F5A-9B38-1BF3E18D3D15}](https://github.com/user-attachments/assets/e59d1946-99a0-45f3-8ae9-fa5cc87fa9c6)

## Вывод
В результате я приобрела следующие навыки:
- изучила основы теории графов
- изучила базовые алгоритмы для работы с графами
- изучила способы представления графов
- изучила базовые алгоритмы работы с векторами в C++

  


