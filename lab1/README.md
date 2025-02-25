# Лабораторная работа №1
## Цель: 
Разработать библиотеку для работы с  N-арным деревом на выбранном императивном языке программирования (например, C++, Java, Python).
## Задача:
N-арное дерево. Вставка узла в дерево. Удаление узла из дерева.
## Определения:
_Дерево_ — связный ациклический граф.[1] Связность означает наличие маршрута между любой парой вершин, ацикличность — отсутствие циклов.

_N-арное дерево_ — это дерево, в котором степени вершин не превосходят N+1.
Реализация
### Структура Treenode
В начале кода представлена структура Treenode.Она представляет узел дерева.Каждый узел содержит значение(data) и вектор указателей на его дочерние узлы(children).
```c++
#include<iostream>
#include<vector>
using namespace std;
struct Treenode {
	int data;
	vector <Treenode*> children; 
   };
```
### Функция добавления узла addChild принимает на вход указатель на корневой узел дерева и значение арности дерева(максимальное количество дочерних узлов).
```c++
void addChild(Treenode*& parent, int n) {
    if (!parent) {
        cout << "Ошибка: Дерево не создано! Создайте корень сначала.\n";
        return;
    }
    int o; int depth;
    cout << "Введите уровень (1 это корень)" << endl;
    cin >> depth;
    Treenode* g = parent;
    if (depth == 1) {
        if (parent->children.size() < n) {
            int value;
            cout << "Введите значение нового узла: ";
            cin >> value;
            Treenode* newNode = new Treenode{ value, {} };
            parent->children.push_back(newNode);
            cout << "Узел добавлен к корню!\n";
        }
        else {
            cout << "Ошибка: Достигнута максимальная арность (" << n << ") у корня.\n";
        }
        return;
    }
    for (int i = 0; i < depth - 1; i++) {
        if (g->children.empty()) {
            cout << "Ошибка: У этого узла нет потомков.\n";
            return;
        }
        cout << "Доступные узлы: ";
        for (int j = 0; j < g->children.size(); j++) {
            cout << j + 1 << ": " << g->children[j]->data << " ";
        }
        cout << "Введите номер узла " << endl;
        cin >> o;
        if (o < 1 || o > g->children.size()) {
            cout << "Ошибка: Неверный номер узла!\n";
            return;
        }
        g = g->children[o - 1];
    }
    cout << "Дети текущего узла: ";
    for (int j = 0; j < g->children.size(); j++) {
        cout << g->children[j]->data;
    }
    int value;
    cout << "Введите значение " << endl;
    cin >> value;
    if (g->children.size() < n) {
        Treenode* k = new Treenode;
        k->data = value;
        g->children.push_back(k);
        cout << "Узел добавлен!\n";
        return;
    }
    else {
        cout << "Ошибка: Достигнута максимальная арность (" << n << ").\n";
    }
}
```

##№ Разбор функции: 
Функция позволяет пользователю добавлять узлы в дерево, выбирая уровень, 
а затем путь к нужному узлу. Если указанный уровень — корень (```depth == 1```), 
  узел добавляется непосредственно в ```parent```.  
 
 Если ```depth > 1```, происходит последовательный выбор узлов на каждом уровне.  
 Если у выбранного узла нет детей, предлагается создать новый узел или отменить операцию. 
 ### Функция удаления выбранного узла.
 Функция ```deleteNode``` yдаляет узел ```nodeToDelete``` из списка потомков ```parent``` и рекурсивно удаляет все его дочерние узлы.
 
 ```c++
void deleteNode(Treenode* parent, Treenode* nodeToDelete) {
    if (!parent || !nodeToDelete)
        return;
    for (int i = 0; i < parent->children.size(); i++) {
        if (parent->children[i] == nodeToDelete) {
            for (int j = i; j < parent->children.size()-1; j++) {
                parent->children[j] = parent->children[j + 1];
            }
            parent->children.resize(parent->children.size() - 1);
            break;
        }
    }
    while (!nodeToDelete->children.empty()) {
        deleteNode(nodeToDelete, nodeToDelete->children.back());
    }
    delete nodeToDelete;
}
void findtodelete(Treenode* parent) {
    if (!parent) {
        cout << "Ошибка: Дерево пустое!\n";
        return;
    }
    int o; int depth;
    cout << "Введите уровень дерева" << endl;
    cin >> depth;
    Treenode* g = parent;
    for (int i = 0; i < depth; i++) {
       
        for (int j = 0; j < g->children.size(); j++) {
            cout << j + 1 << ":"<< g->children[j]->data << " ";
        }
        cout << "Выберите номер узла " << endl;
        
        cin >> o;
        if (o < 1 || o > g->children.size()) {
            cout << "Ошибка: Неверный номер узла!\n";
            return;
        }
        parent = g;
        g = g->children[o - 1];

        deleteNode(parent, g);
    }
}
```
### Разбор Функций:
## Функция   ```findtodelete```  

Проверяет, существует ли дерево (```parent```). Если нет — выводит ошибку.
Запрашивает у пользователя уровень ```depth```, на котором нужно удалить узел.
Начинает поиск узла, спускаясь ```depth``` уровней вниз:
Выводит список дочерних узлов текущего узла.
Пользователь выбирает, к какому потомку перейти.
Если номер введен неверно, выводит ошибку и завершает работу.
После нахождения нужного узла вызывает ```deleteNode```, передавая в него родителя и найденный узел.
## Функция   ```deleteNode```
Ищет ```nodeToDelete``` среди детей parent.
Если находит, сдвигает оставшиеся элементы вектора ```children``` влево.
Уменьшает размер ```children``` (удаляя последний дубликат).
Рекурсивно удаляет всех потомков ```nodeToDelete```, начиная с последних элементов (чтобы избежать ошибок).
Освобождает память (```delete nodeToDelete```).
# Заключение
Реализованные функции позволяют эффективно вставлять и удалять узлы в N-арном дереве.

 
 
