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

