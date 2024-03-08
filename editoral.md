## Розбір другого контесту про графи

### Обход в ширину

Задача вимагає простої реалізації алгоритму пошуку в ширину з лекції. Звернітся до коментарів в коді для деталей

```
#include <iostream>
#include <string>
#include <algorithm>
#include<vector>
#include <queue>
using namespace std;

int main()
{
	vector<vector<int> > g;
	int n, s, f;
	cin >> n >> s >> f;
	s--, f--;
	g.resize(n);
    // Вводимо граф в форматі матриці суміжності і переводимо його в формат списку суміжності 
	for (int i = 0; i < n; i++)
		for (int j = 0; j < n; j++)
		{
			int a;
			cin >> a;
			if (a == 1)
			{
				g[i].push_back(j);
			}
		}
        

    // Сам пошук в ширину
	queue<int> q; черга
	q.push(s); 
	vector<bool> used(n); // використані вершини 
	vector<int> d(n), p(n); // масиви відстаней та "попередніх" вершин на шляху
	used[s] = true;
	p[s] = -1;
	while (!q.empty())
	{
		int v = q.front();
		q.pop(); // Беремо найближчу невикористану 
		for (int i = 0; i< g[v].size(); i++)
		{
			int to = g[v][i]; // проходимо по ребрах 
			if (!used[to])
			{
				used[to] = true;
				q.push(to);
				d[to] = d[v] + 1; // оновлюємо відстань 
				p[to] = v;
			}
		}
	}
	cout << d[f];
}
```

### Алгоритм Дейкстры

Задача аналогічна минулій, але для реалізації потрібно використати алгоритм Дейкстри з лекції, через зваженість ребер графа. 

```
#include <iostream>
#include<string>
#include <algorithm>
using namespace std;
const long long inf = 1000000000;


long long g[2001][2001]; // Відстані між вершинами 
long long d[2001]; // Відстань від стартової до вершини
long long u[2001]; // Які вже використали 
int main()
{
	int n, s, f;
	cin >> n >> s >> f;
	s--;
	f--;
    // Вводимо граф матрицею суміжності. Ставимо нескінченності на місцях, де немає ребер.
	for (int i = 0; i < n; i++)
	{
		for (int j = 0; j < n; j++)
		{
			long long tmp;
			cin >> tmp;
			g[i][j] = tmp;
			if (tmp < 0)
			{
				g[i][j] = inf; 
			}
		}
	}
	for (int i = 0; i < n; i++)
	{
		d[i] = inf;
		u[i] = 0;
	}
	d[s] = 0;
	for (int i = 0; i < n; i++)
	{
        // Шукаємо першу невикористану вершину 
		long long pos = -1;
		for (int j = 0; j < n; j++)
		{
			if (!u[j] && (pos == -1 || d[j] < d[pos]))
			{
				pos = j;
			}

		}
		if (d[pos] == inf)
		{
			break;
		}
		u[pos] = true; 
		for (int m = 0; m < n; m++) // Проходимося по усім її ребрах
		{
			if (u[m] != 1 && g[pos][m] != inf && d[pos] != inf && (d[pos] + g[pos][m] < d[m]))
				d[m] = d[pos] + g[pos][m];
		}
	}
	if (d[f] == inf)
	{
		cout << "-1";
		return 0;
	}
	cout << d[f];

}
```
### Найкоротша відстань

Задача повністю ідентична першій. Для прикладу надається код алгоритмом дейкстри, хоча тут достатньо і звичайного пошуку в ширину.

```
#include <iostream>
#include<string>
#include <algorithm>
using namespace std;
const long long inf = 1000000000;


long long g[2001][2001];
long long d[2001];
long long u[2001];

int main()
{
	int n, s, f=0;
	cin >> n >>s;
	s--;
	for (int i = 0; i < n; i++)
	{
		for (int j = 0; j < n; j++)
		{
			long long tmp;
			cin >> tmp;
			g[i][j] = tmp;
			if (tmp == 0)
			{
				g[i][j] = inf;
			}
		}
	}
	for (int i = 0; i < n; i++)
	{
		d[i] = inf;
		u[i] = 0;
	}
	d[s] = 0;
	for (int i = 0; i < n; i++)
	{
		long long pos = -1;
		for (int j = 0; j < n; j++)
		{
			if (!u[j] && (pos == -1 || d[j] < d[pos]))
			{
				pos = j;
			}

		}
		if (d[pos] == inf)
		{
			break;
		}
		u[pos] = true;
		for (int m = 0; m < n; m++)
		{
			if (u[m] != 1 && g[pos][m] != inf && d[pos] != inf && (d[pos] + g[pos][m] < d[m]))
				d[m] = d[pos] + g[pos][m];
		}
	}
	if (d[0] == inf)
	{
		cout << "-1";
	}
	else
	{
		cout << d[0];

	}
	
	for (f = 1; f < n; f++)
	{
		if (d[f] == inf)
		{
			cout << " -1";
		}
		else
		{
			cout <<" "<< d[f];

		}
	}
	cout << '\n';

}
```

### Переміщення коня

Основна складність задачі - побудувати граф. Вона вже була розібрана в минулому розборі - https://gist.github.com/anakib1/b8c92f693b8dbe713e0263a73ca97073. 

### Три держави

Задача була замінена.

### Видалення клітинок
Для цього потрібно знайти, скільки компонент зв'язності представляє собою граф, який утворится, якщо об'єднати кожні дві клітинки зі спільною стороною. 

Алгоритм в задачах такого типу наступний:
- Будуємо граф з складної структури в звичайному вигляді
- Розв'язуємо на ньому задачі
```
#include <iostream>
#include<string>
#include<vector>

using namespace std;
vector<vector<int> > u;
int n, m;
void c(int i, int j) // Функція, яка виконує пошук в глибину з клітинки 
	{
		if (u[i][j] == 0)
			return;
		u[i][j] = 0;
        // Розглядаємо усі можливі переходи - вліво, вниз, вправо, вверх. 
		if (i >= 1)
		{
			c(i - 1, j);
			
		}
		if (i < m - 1)
			c(i + 1, j);
		if (j >= 1)
			c(i, j - 1);
		if (j < n - 1)
			c(i, j + 1);

	}
int main()
{
	cin >> m>>n;
	u.resize(m);
	for (int i = 0; i < m; i++)
	{
		for (int j = 0; j < n; j++)
		{
			char c;
			cin >> c;
			int k = c == '.' ? 0 : 1;
			u[i].push_back(k);
		}
	}
	int r = 0;
	for (int i = 0; i < m; i++)
	{
		for (int j = 0; j < n; j++)
		{
			if (u[i][j]) // Кожна нова не використана клітинка - нова компонента зв'язності графа. 
			{
				r++;
				c(i, j);
			}
		}
	}
	cout << r << '\n';	
}
```

### Заправки

Аналогічно минулій задачі. Будуємо граф, в якому ваги ребер - вартість проїзду з одного міста в інше, розв'язуємо задачу найкротшого шляху на отриманому графі.
```
#include <iostream>
#include <string>
#include <algorithm>
using namespace std;

const int MAX = 110;
const int inf = 11111111;
static int g[MAX][MAX], u[MAX], d[MAX], pr[MAX];
int n;
int main()
{
	cin >> n;
	for (int i = 1; i <= n; i++)
	{
		cin >> pr[i];
	}
	for (int i = 0; i < MAX; i++)
	{
		for(int j=0; j<MAX;j++)
		g[i][j] = inf;
		u[i] = 0;
		d[i] = inf;
	}
	d[1] = 0;
	int m;
	cin >> m;
	for (int o = 1; o <= m; o++)
	{
		int a, b;
		cin >>a >> b;
		g[a][b] = pr[a];
		g[b][a] = pr[b];
	} // Будуємо граф з даних про заправки 
    // Звичайний алгоритм дейкстри 
	for (int i = 1; i < n; i++)
	{
		int v = -1, m_in=inf;
		for (int j = 1; j <= n; j++)
		{
			if (!u[j] && d[j] < m_in)
			{
				m_in = d[j];
				v = j;
			}
		}
		if (v == -1)
			break;
		for (int j = 1; j <= n; j++)
		{
			if (!u[j])
			{

				if (d[v] + g[v][j] < d[j])

					d[j] = d[v] + g[v][j];
			}
			
		}
		u[v] = 1;
	}
	if (d[n] == inf)
	{
		cout << "-1\n";
	}
	else
	{
		cout << d[n];
	}
	return 0;
	
}
```

### Магічна машинка

Задача по факту знову представляє з себе неявний граф - можемо поєднати ребром довжини 1 числа, між якими можливі переходи. Після чого зробити пошук найкоротшого шляху на такому графі!

```
#include <iostream>
using namespace std;
int dp[10000];
int sum(int a)
{
  int res = 0;
  while(a > 0)
  {
      res+=a%10;
      a/=10;
  }
  return res;
} // Допоміжна функція, яка рахує суму цифр числа. 

void solve(int v)
{ // Спочатку всі можливі переходи "вниз"
    if(v - 2 > 0 && dp[v] + 1 < dp[v - 2])
      {
        dp[v - 2] = dp[v] + 1;
        solve(v - 2);
      }
      //Переходи "вверх"
    int k = sum(v);
    if(k + v <= 9999 && dp[v] + 1 < dp[k + v] )
    {
      dp[k + v] = dp[v] + 1;
      solve(k + v);
    }
    if(v * 3 <= 9999 && dp[v] + 1 <dp[v * 3])
    {
      dp[v * 3] = dp[v] + 1;
      solve(v * 3);
    }
}
int main() 
{
  int a, b;
  cin >> a >> b;
  for(int i = 0; i <10000; i++)
    dp[i] = 2 * 1e9;
  dp[a] = 0;
  solve(a);// Починаємо пошук.
  cout << dp[b];
}
```

### Найкоротший шлях

В цій задачі потрібно написати пошук в ширину, однак вивести не лише відстань, а й сам шлях. Для цього в пригоді стануть масиви історії - для кожної вершини ми будемо зберігати, з якої вершини в неї дійшли в алгоритмі пошуку в ширину. Так, ми зможемо пройтися по цьому масиву від шуканої вершини до початкової і знайти весь шлях.

```
#include <iostream>
#include <string>
#include <algorithm>
#include<vector>
#include <queue>
using namespace std;

int main()
{
	vector<vector<int> > g;
	int n, m, s, f;
	cin >> n >>m>> s >> f;
	s--, f--;
	g.resize(n);
	for (int i = 0; i < m; i++)
	{
		int a, b;
		cin >> a >> b;
		a--, b--;
		g[a].push_back(b);
		g[b].push_back(a);
	} // Читаємо граф 

	vector<bool> u(n);
	queue<int> q;
	q.push(s);
	vector<bool> used(n);
	vector<int> d(n), p(n);
	used[s] = true;
	p[s] = -1; // Початкові речі для пошуку в ширину
	while (!q.empty())
	{
		int v = q.front();
		q.pop();
		for (int i = 0; i < g[v].size(); i++)
		{
			int to = g[v][i];
			if (!used[to])
			{
				used[to] = true;
				q.push(to);
				d[to] = d[v] + 1;
				p[to] = v; // ставимо масив історії - ми прийшли в вершину to з вершитни v
			}
		}
	}
	if (!used[f])
	{
		cout << "-1\n";
		return 0;
	}
	else {
		cout << d[f]<<'\n';

		vector<int> path;
		for (int v = f; v != -1; v = p[v])
			path.push_back(v); // Проходимося по шляху, додаємо його в вектор 
		reverse(path.begin(), path.end()); // Розвертаємо шлях і виводимо. 
		cout << path[0]+1;
		for (int i = 1; i < path.size(); i++)
			cout<<" " << path[i] + 1;
	}
}
```

### Дейкстра

Як і сказано в умові задачі, потрібно написати алгоритм Дейкстри. Абсолютно аналогічна другій задачі.