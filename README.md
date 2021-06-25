# EugeneFomin
Практика ИСТБ 20-4
#include<iostream>
#include<ctime>
#include <string>
#include<cstdlib>
using namespace std;
bool fill(int** a, int n)		//n^2
{
	int count = 0;
	bool negative = false;
	for (int i = 0; i < n; i++)
		for (int j = 0; j < n; j++)
		{
			cin>>a[i][j];
			if (a[i][j] < 0 || a[i][j]>255 || cin.fail())
			{
				if(cin.fail())
					a[i][j]=-1;
				cin.clear();
				cin.ignore(32627,'\n');
				negative = true;
			}
		}
	return !negative;
}
void err(int** a, int n)
{
	int count=0;
	for(int i=0;i<n;i++)
		for(int j=0;j<n;j++)
		{
			if (a[i][j] < 0 || a[i][j]>255 || cin.fail())
			cout <<'\n'<< ++count << ")\ni : " << i + 1 << "\nj : " << j + 1 << "\n";		
		}	
}
void print(int** a, int n)			//n^2
{
	for (int i = 0;
	 i < n; i++)
	{
		cout << "\n\n";
		for (int j = 0; j < n; j++)
		{
			cout << "\t" << a[i][j];
		}
		cout << "\n";
	}
	cout << "\n";
}
int average(int** a, int l, int c)
{
	int p;
	p=a[l][c] + (a[l - 1][c - 1] + a[l - 1][c] + a[l - 1][c + 1] + a[l][c - 1] + a[l][c + 1] + a[l + 1][c - 1] + a[l + 1][c] + a[l + 1][c + 1]);
	return p/= 9;
}
int avg11(int** a)
{
	int p;
	p = a[0][0] + a[0][1] + a[1][0] + a[1][1];
	return p /= 4;
}
int avg1n(int** a, int n)
{
	int p;
	n -= 1;
	p = a[0][n] + a[0][n - 1] + a[1][n] + a[1][n - 1];
	return p /= 4;
}
int avgn1(int** a, int n)
{
	int p;
	n -= 1;
	p = a[n][0] + a[n][1] + a[n - 1][1] + a[n - 1][0];
	return p /= 4;
}
int avgnn(int** a, int n)
{
	int p;
	n -= 1;
	p = a[n][n] + a[n][n - 1] + a[n - 1][n - 1] + a[n - 1][n];
	return p /= 4;
}
int avg0j(int** a, int i, int j)
{
	int p;
	p = a[i][j] + a[i][j - 1] + a[i][j + 1] + a[i + 1][j] + a[i + 1][j - 1] + a[i + 1][j + 1];
	return p /= 6;
}
int avgi0(int** a, int i, int j)
{
	int p;
	p = a[i][j] + a[i - 1][j] + a[i + 1][j] + a[i + 1][j + 1] + a[i][j + 1] + a[i - 1][j + 1];
	return p /= 6;
}
int avgnj(int** a, int i, int j)
{
	int p;
	p = a[i][j] + a[i][j + 1] + a[i][j - 1] + a[i - 1][j - 1] + a[i - 1][j] + a[i - 1][j + 1];
	return p /= 6;
}
int avgin(int** a, int i, int j)
{
	int p;
	p = a[i][j] + a[i - 1][j] + a[i + 1][j] + a[i + 1][j - 1] + a[i][j - 1] + a[i - 1][j - 1];
	return p /= 6;
}
void blur(int** a, int n)
{
	int** matrix = new int* [n];
	for (int i = 0; i < n; i++)
	{
		matrix[i] = new int[n];	//n
	}
	for (int i = 0; i < n; i++)
	{
		for (int j = 0; j < n; j++)		//n^2
		{
			if (i == 0 && j == 0)
			{
				matrix[i][j] = avg11(a);
				continue;
			}
			if (i == 0 && j == n - 1)
			{
				matrix[i][j] = avg1n(a, n);
				continue;
			}
			if (i == n - 1 && j == 0)
			{
				matrix[i][j] = avgn1(a, n);
				continue;
			}
			if (i == n - 1 && j == n - 1)
			{
				matrix[i][j] = avgnn(a, n);
				continue;
			}
			if (i == 0)
			{
				matrix[i][j] = avg0j(a, i, j);
				continue;
			}
			if (j == 0)
			{
				matrix[i][j] = avgi0(a, i, j);
				continue;
			}
			if (i == n - 1)
			{
				matrix[i][j] = avgnj(a, i, j);
				continue;
			}
			if (j == n - 1)
			{
				matrix[i][j] = avgin(a, i, j);
				continue;
			}
			matrix[i][j] = average(a, i, j);
		}
	}
	cout << "==========================================================================================================================\n";
	print(matrix, n);
}
int main()
{
	setlocale(LC_ALL, "");
	srand(time(0));
	string str="";
	int N=0;
	cout << "N\n";
	do
	{
		cin >> N;
		if (cin.fail()|| N<=1)
		{
			cout << "Ошибка\n";
			cin.clear();
		}
		cin.ignore(32767, '\n');
	} while (N <= 1);
	int** matrix = new int* [N];
	for (int i = 0; i < N; i++)
		matrix[i] = new int[N];		//n
	if (fill(matrix, N))
	{
		blur(matrix, N);	//n+n^2
	}
	else
		{
		err(matrix,N);
		print(matrix, N);
		}
	return 0;// n+n^2
}
