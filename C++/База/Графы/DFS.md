Поиск в глубину. DFS.

Алгоритм для [[Графы||Графов]] "Поиск в глубину" (DFS) основан на 3 принципах:

1. Обходим граф, посещая каждую вершину только один раз
2. Используем структуру данных [[Stack]] или рекурсивную функцию
3. Обходим граф по соседям, если видим не помеченную смежную вершину, то переходим в смежную вершину.
![[Pasted image 20250215172953.png]]

В аргументах функции dsf_number_comp передаём:
vector < vector < int >> G  - граф, представленный в виде списка смежности, где в каждой i строке каждая ячейка - это номер вершины, с которой смежная вершина с номером i.
vector < int > Mark - вектор посещаемости Mark[i] равен 1, если до вершины с номером i  есть путь, иначе 0. Изначально каждая вершина считается не посещенной.
tmp - вершина, которую мы рассматриваем сейчас.
Трудоёмкость алгоритма O(m), где m - кол-во рёбер. [[Рекурсия||Рекурсивная]] реализация DFS считается классическим вариантом.
ТА САМАЯ РЕАЛИЗАЦИЯ:
```cpp
void dfs_number_comp(vector<vector<int> >& G,
		vector<int>& Mark, int tmp) {
	Mark[tmp] = 1;
	for (int i = 0; i < G[tmp]; i++) {
		if (Mark[G[tmp][i]] == 0) {
			dfs_number_comp(G, Mark, G[tmp][i]);
		}
	}
}

...
int cnt = 0;
for (int i = 1; i <= n; i++) {
	if (Mark[i] == 0) {
		cnt++;
		dfs_number_comp(G, Mark, i);
	}
}

cout << cnt;
```
![[Pasted image 20250215174110.png]]
*на фото ошибка, break лишний в блоке else после удаления из списка*
![[Pasted image 20250215174329.png]]
Улучшим реализацию функции dfs_number_comp. Добавим в аргументы функции:
vector < int > Color - Вектор покраски компонент связности. Color[i] равняется номеру цвета компоненты связности, в которой располагается вершина i.
number_color - Номер цвета компоненты связности.
![[Pasted image 20250215174624.png]]
Новый код:
```cpp
void dfs_color(vector<vector<int> >& G),
		vector<int>$ Mark,
		vector<int>$ Color,
		int number_color,
		int tmp) {
	Mark[tmp] = 1;
	Color[tmp] = number_color;
	for (int i = 0; i < G[tmp].size(); i++) {
		if (Mark[G[tmp][i]] == 0) {
			dfs_color(G, Mark, Color, number_color G[tmp][i]);
		}
	}
}

```
# Проверка графа на двудольность
![[Pasted image 20250215174847.png]]Изменим реализацию функции dfs_color. Поменяем в ней способ использования vector< int> Color, добавим переменную flag для проверки на двудольность.
vector < int > Color - Вектор покраски вершин. Color[i] равняется цвету двудольности для вершины i 0 или 1;
flag - Флаг, проверяющий, что граф двудольный, если flag равняется false, то граф не является двудольным, иначе является;
x - Вершина, из которой мы переходим по ребру (x, y);
x - Вершина, в которую мы переходим по ребру (x, y);
Реализация:
```cpp
void dfs_biparite(vector<vector<int> >& G,
	vector<int>& Mark,
	vector<int>& Color,
	bool& flag,
	int y,
	int x)
{
	Color[y] = (Color[x] + 1) % 2;
	Mark[y] = 1;
	for (int i = 0; i < G[y].size(); i++) {
		if (Mark[G[y][i]] == 0) {
			dfs_biparite(G, Mark, Color, flag, G[y][i], y);
		}
		else if (Color[y] == Color[G[y][i]]) {
			flag = false;
		}
	}
}
```

# Поиск циклов

![[Pasted image 20250215175322.png]]![[Pasted image 20250215175456.png]]
![[Pasted image 20250215175804.png]]
Реализация:
```cpp
void dfs_find_cycle_save(vector<vector<int> >& G,
	vector<int>& Mark,
	bool& flag,
	int y,
	int x)
{
	st.push(y);
	Mark[y] = 1;
	for (int i = 0; i < G[y].size() && flag == flase; i++) {
		if (Mark[G[y][i]] == 0) {
			dfs_find_cycle_save(G, Mark, st, flag, G[y][i], y);
		}
		else if (G[y][i] != x) {
			flag = false;
			st.push(G[y][i]);
		}
	}
	if (flag == false) {
		st.pop();
	}
}
```
![[Pasted image 20250215180357.png]]
![[Pasted image 20250215180710.png]]
