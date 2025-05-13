#cs

using System.Windows.Controls;
using System.Windows;
using System;
using System.Reflection.Metadata;
using System.Security.Claims;
using System.Text;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Data;
using System.Windows.Documents;
using System.Windows.Input;
using System.Windows.Media;
using System.Windows.Media.Animation;
using System.Windows.Media.Imaging;
using System.Windows.Media.Media3D;
using System.Windows.Navigation;
using System.Windows.Shapes;
using System.Xml.Linq;

namespace WpfApp1
{
    /// <summary>
    /// Interaction logic for MainWindow.xaml
    /// </summary>
    public partial class MainWindow : Window
    {

        private double x = 0;
        private double y = 0;
        private string z = "";
        private bool drob = false;

        public MainWindow()
        {
            InitializeComponent();
        }
        void ClickOnNumber(object sender, RoutedEventArgs e)
        {
            var btn = (Button)sender;
            var number = (string)btn.Content;

            if (y == 0)
            {
                Screen.Text = number;
            }
            else
            {
                Screen.Text += number;
            }

            if (drob)
            {
                y = y * 10 + Convert.ToInt32(number);
                y = y / 10;
            }
            else
            {
                y = y * 10 + Convert.ToInt32(number);
            }


        }

        void ClickChangeSign(object sender, RoutedEventArgs e)
        {
            Screen.Text = Convert.ToString(-y);
            y = -y;
        }

        void ClickDelete(object sender, RoutedEventArgs e)
        {
            string r = Screen.Text;
            if (r.Length > 0)
            {
                
                if (r.EndsWith("+") || r.EndsWith("-") || r.EndsWith("*") || r.EndsWith("/"))
                {
                    z = "";
                }

                
                r = r.Substring(0, r.Length - 1);

                
                if (string.IsNullOrEmpty(r))
                {
                    r = "0";
                }
                Screen.Text = r;

                if (double.TryParse(Screen.Text, out double parsedValue))
                {
                    y = parsedValue;  
                    drob = r.Contains(",");  
                }
                else
                {
                    y = 0; 
                    drob = false;
                }
            }
            else
            {
                
                Screen.Text = "0";
                y = 0;
                drob = false;
            }
        }

        void ClickOnSign(object sender, RoutedEventArgs e)
        {
            var btn = (Button)sender;
            var sign = (string)btn.Content;
            if (z == "")
            {
                x = y;
                y = 0;
            }
            else
            {
                switch (z)
                {
                    case "+":
                        x = x + y;
                        break;
                    case "-":
                        x = x - y;
                        break;
                    case "*":
                        x = x * y;
                        break;
                    case "/":
                        if (y != 0)
                        {
                            x = x / y;
                            
                        }
                        else
                        {
                            MessageBox.Show("Деление на ноль!");
                            return;
                        }
                        break;

                    
                }
            }

            y = 0;
            z = sign;
            Screen.Text = x + z;
            drob = false;
        }

        void ClickOnEquals(object sender, RoutedEventArgs e)
        {
            if (string.IsNullOrEmpty(z))
            {
                Screen.Text = y.ToString();
                x = y;
                y = 0;
                return;
            }

            if (z == "/" && y == 0)
            {
                MessageBox.Show("Деление на ноль!");
                return; // Прерываем выполнение функции
            }

            switch (z)
            {
                case "+": x = x + y;break;
                case "-":x = x - y; break;
                case "*": x = x * y;break;
                case "/": x = x / y;break;
            }

            Screen.Text = x.ToString();
            y = 0;
            z = "";

        }

        void ClickClean(object sender, RoutedEventArgs e)
        {
            x = 0;
            y = 0;
            z = "";
            drob = false;
            Screen.Text = "0";
        }


        void ClickOnDot(object sender, RoutedEventArgs e)
        {
            if (!Screen.Text.Contains(",")) 
            {
                if (Screen.Text == "0") 
                {
                    Screen.Text = "0,";
                }
                else
                {
                    Screen.Text += ","; 
                }
            }
            drob = true;

        }
    }
}





#xaml
<Window x:Class="WpfApp1.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:WpfApp1"
        mc:Ignorable="d"
        Title="MainWindow" Height="700" Width="500">
    <Grid>

        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition/>
            <ColumnDefinition/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>

        <Grid.RowDefinitions>
            <RowDefinition/>
            <RowDefinition/>
            <RowDefinition/>
            <RowDefinition/>
            <RowDefinition/>
            <RowDefinition/>
        </Grid.RowDefinitions>

        <TextBlock x:Name="Screen" Text="0" Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="4" FontSize="25"/>

        <Button x:Name="B1" Content="1" Grid.Column="0" Grid.Row="2" FontSize="25" Click="ClickOnNumber" />
        <Button x:Name="B2" Content="2" Grid.Column="1" Grid.Row="2" FontSize="25" Click="ClickOnNumber"/>
        <Button x:Name="B3" Content="3" Grid.Column="2" Grid.Row="2" FontSize="25" Click="ClickOnNumber"/>

        <Button x:Name="B4" Content="4" Grid.Column="0" Grid.Row="3" FontSize="25" Click="ClickOnNumber"/>
        <Button x:Name="B5" Content="5" Grid.Column="1" Grid.Row="3" FontSize="25" Click="ClickOnNumber"/>
        <Button x:Name="B6" Content="6" Grid.Column="2" Grid.Row="3" FontSize="25" Click="ClickOnNumber" />

        <Button x:Name="B7" Content="7" Grid.Column="0" Grid.Row="4" FontSize="25" Click="ClickOnNumber" />
        <Button x:Name="B8" Content="8" Grid.Column="1" Grid.Row="4" FontSize="25" Click="ClickOnNumber"/>
        <Button x:Name="B9" Content="9" Grid.Column="2" Grid.Row="4" FontSize="25" Click="ClickOnNumber"/>

        <Button x:Name="B_plus_minus" Content="+/-" Grid.Column="0" Grid.Row="5" FontSize="25" Click="ClickChangeSign" />
        <Button x:Name="B0" Content="0" Grid.Column="1" Grid.Row="5" FontSize="25" Click="ClickOnNumber"/>
        <Button x:Name="B_dot" Content="." Grid.Column="2" Grid.Row="5" FontSize="25" Click="ClickOnDot" />

        <Button x:Name="B_plus"  Content="+" Grid.Column="3" Grid.Row="2" FontSize="25" Click="ClickOnSign" />
        <Button x:Name="B_minus" Content="-" Grid.Column="4" Grid.Row="3" FontSize="25" Click="ClickOnSign" />
        <Button x:Name="B_equal" Content="=" Grid.Column="4" Grid.Row="4" FontSize="25" Click="ClickOnEquals"/>

        <Button x:Name="B_del" Content="del" Grid.Column="0" Grid.Row="1" FontSize="25" Click="ClickDelete"/>
        <Button x:Name="B_clean" Content="c" Grid.Column="1" Grid.Row="1" FontSize="25" Click="ClickClean"/>
        <Button x:Name="B_multiply" Content="*" Grid.Column="2" Grid.Row="1" FontSize="25" Click="ClickOnSign"/>
        <Button x:Name="B_division" Content="/" Grid.Column="3" Grid.Row="1" FontSize="25" Click="ClickOnSign"/>



    </Grid>
</Window>


машинка

import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import pandas as pd
from sklearn.datasets import load_iris
from sklearn.preprocessing import StandardScaler

iris = load_iris()
X = iris.data
features = ['Длина чашелистика', 'Ширина чашелистика', 'Длина лепестка', 'Ширина лепестка']

# Масштабируем данные
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)


# Реализация KMeans
def my_kmeans(X, k, max_iters=100, plot_each_step=False):
    np.random.seed()
    centroids = X[np.random.choice(X.shape[0], k, replace=False)]

    for it in range(max_iters):
        # Расчет расстояний и присвоение меток
        distances = np.linalg.norm(X[:, np.newaxis] - centroids, axis=2)
        labels = np.argmin(distances, axis=1)

        # Визуализация (если требуется)
        if plot_each_step:
            visualize_clusters(X, labels, centroids, it)

        # Обновление центроидов
        new_centroids = np.array([
            X[labels == i].mean(axis=0) if len(X[labels == i]) > 0 else centroids[i]
            for i in range(k)
        ])

        # Проверка на сходимость
        if np.allclose(centroids, new_centroids):
            break
        centroids = new_centroids

    return labels, centroids


def visualize_clusters(X, labels, centroids, iteration):

    plt.figure(figsize=(6, 5))
    for i in np.unique(labels):
        cluster = X[labels == i]
        plt.scatter(cluster[:, 0], cluster[:, 1], label=f'Кластер {i}')
    plt.scatter(centroids[:, 0], centroids[:, 1], color='black', marker='x', s=200, label='Центроиды')
    plt.title(f'Итерация {iteration + 1}')
    plt.xlabel('Признак 1 (норм.)')
    plt.ylabel('Признак 2 (норм.)')
    plt.legend()
    plt.grid(True)
    plt.show()


def calculate_wcss(X, k_range):
    #Вычисляет WCSS для различных значений k
    wcss = []
    for k in k_range:
        labels, centroids = my_kmeans(X, k, plot_each_step=False)
        sse = sum(np.sum((X[labels == i] - centroids[i]) ** 2) for i in range(k))
        wcss.append(sse)
    return wcss


# Определяем диапазон k и вычисляем WCSS
k_values = list(range(1, 11))
wcss_values = calculate_wcss(X_scaled, k_values)

# Определяем оптимальное количество кластеров
threshold = 0.1
optimal_k = 1
for i in range(1, len(wcss_values)):
    drop = (wcss_values[i - 1] - wcss_values[i]) / wcss_values[i - 1]
    if drop < threshold:
        optimal_k = i
        break
else:
    optimal_k = k_values[-1]

print(f"\nОптимальное количество кластеров: {optimal_k}")

# метод локтя
plt.figure(figsize=(8, 5))
plt.plot(k_values, wcss_values, marker='o')
plt.title('Метод локтя для выбора оптимального количества кластеров')
plt.xlabel('Количество кластеров k')
plt.ylabel('WCSS')
plt.grid(True)
plt.show()

# Финальный запуск KMeans с визуализацией
labels, centroids = my_kmeans(X_scaled, optimal_k, plot_each_step=True)

# Построим pairplot со всеми попарными проекциями
df = pd.DataFrame(X_scaled, columns=features)
df['Кластер'] = labels

# Уникальные пары признаков для визуализации
pairs = [
    ("Длина чашелистика", "Ширина чашелистика"),
    ("Длина чашелистика", "Длина лепестка"),
    ("Длина чашелистика", "Ширина лепестка"),
    ("Ширина чашелистика", "Длина лепестка"),
    ("Ширина чашелистика", "Ширина лепестка"),
    ("Длина лепестка", "Ширина лепестка"),
]

# Визуализация пар признаков
for x, y in pairs:
    plt.figure(figsize=(6, 5))
    sns.scatterplot(data=df, x=x, y=y, hue='Кластер', palette='Set1')
    plt.title(f'{x} vs {y}')
    plt.grid(True)
    plt.show()
