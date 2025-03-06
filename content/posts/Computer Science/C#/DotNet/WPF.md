+++
title = 'WPF'
date = 2025-02-25T20:14:41.339Z
draft = true
+++

# WPF
[docs](https://learn.microsoft.com/en-us/dotnet/desktop/wpf/overview/?view=netdesktop-9.0)
[docs](https://wpf-tutorial.com/)

WPF is a .NET UI framwork for Windows native applications. The older WPF version uses .NET Framework, but that is not recommended anymore.

It uses **Markup** and **code-behind** to develop interfaces. The markup in **XAML** to implement appearance while the code-behind is for behaviour.

## Markup
XAML  is a XML based markup language. You define windos, dialog boxes, pages, and user controls and then fill them with controls, shapes, and graphics.

```xml
<Window
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    Title="Window with button"
    Width="250" Height="100">

  <!-- Add button to window -->
  <Button Name="button">Click Me!</Button>

</Window>

```

## Code-behind
Because .NET is built on C#, all code is Object-Oriented, which means that the behaviour code for the markup will also be wrapped as a class.

In this example, we take the `Click` event and write a handler for the event.

```xml
<Window
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    x:Class="SDKSample.AWindow"
    Title="Window with button"
    Width="250" Height="100">

  <!-- Add button to window -->
  <Button Name="button" Click="button_Click">Click Me!</Button>

</Window>
```

Here we look at the `xmlns:x` defines a namespace that adds code-behind support. The `x:Class` associates it with the code-behind class.
Because we used a <Window> tag, we'll also have to inherit from the `Window` class.

```C#
using System.Windows;

namespace SDKSample
{
    public partial class AWindow : Window
    {
        public AWindow()
        {
            // InitializeComponent call is required to merge the UI
            // that is defined in markup with this class, including  
            // setting properties and registering event handlers
            InitializeComponent();
        }

        void button_Click(object sender, RoutedEventArgs e)
        {
            // Show message box when button is clicked.
            MessageBox.Show("Hello, Windows Presentation Foundation!");
        }
    }
}
```

## Window
The Window class is the actual window of the application. Typically, you would extend the Window class to make your own:

```C#
namespace Names
{
    /// Interaction logic for MainWindow.xaml
    /// </summary>
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private void ButtonAddName_Click(object sender, RoutedEventArgs e)
        {
            if (!string.IsNullOrWhiteSpace(txtName.Text) && !lstNames.Items.Contains(txtName.Text))
            {
                lstNames.Items.Add(txtName.Text);
                txtName.Clear();
            }
        }
    }
}
```

The XAML is:
```XML
<Window x:Class="WindowsOverview.Window1"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:WindowsOverview"
        >

    <!-- Client area containing the content of the window -->
    
</Window>
```

## App.xaml
This is the root xaml within a WPF app.
### App.xaml.cs
The code-behind for the root xaml.

## DispatcherUnhandledException 
[docs](https://learn.microsoft.com/en-us/dotnet/api/system.windows.application.dispatcherunhandledexception?view=windowsdesktop-8.0)
Global exception handling to catch any exceptions that you haven't already caught.

```Csharp
using System.Windows;
using System.Windows.Threading;

namespace SDKSample
{
    public partial class App : Application
    {
        void App_DispatcherUnhandledException(object sender, DispatcherUnhandledExceptionEventArgs e)
        {
            // Process unhandled exception

            // Prevent default unhandled exception processing
            e.Handled = true;
        }
    }
}
```

## Controls

A control in WPF basically means any UI objects. It can be a Button, Label, TextBox, Menu, and ListBox. It does not necessarily mean that the user has to "control" it like a Button or text input.

You can define the control in XAML or in the code-behind.

```XML
<Window x:Class="Examples.ExampleApp"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        Title="Input Record" Height="Auto" Width="300" SizeToContent="Height">
    <Grid Margin="5">
        <Grid.RowDefinitions>
            <RowDefinition Height="30"/>
            <RowDefinition Height="30"/>
            <RowDefinition Height="30"/>
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>

        <Label>Enter your name:</Label>
        <TextBox Grid.Row="0" Grid.Column="1" Name="FirstName" Margin="2" />

        <Label Grid.Row="1">Enter your address:</Label>
        <TextBox Grid.Row="1" Grid.Column="1" Name="LastName" Margin="2" />

        <Button Grid.Row="2" Grid.Column="0" Name="Reset" Margin="2">Reset</Button>
        <Button Grid.Row="2" Grid.Column="1" Name="Submit" Margin="2">Submit</Button>
    </Grid>
</Window>
```

```csharp
// Grid container which is the content of the Window
Grid container = new() { Margin = new Thickness(5) };
container.RowDefinitions.Add(new RowDefinition());
container.RowDefinitions.Add(new RowDefinition());
container.RowDefinitions.Add(new RowDefinition());
container.ColumnDefinitions.Add(new ColumnDefinition());
container.ColumnDefinitions.Add(new ColumnDefinition());

// Create the two labels, assign the second label to the second row
Label labelName = new() { Content = "Enter your name:" };
container.Children.Add(labelName);
```

### TextBlock

#### TextTrimming

#### Inline formatting

### Label

### TextBox

#### SpellCheck

### Button

### CheckBox

### RadioButton

### Image

### Tooltips

### Tab

### Access Keys

## Panels

### Canvas
### WrapPanel
### StackPanel
### DockPanel
### Grid

### UserControls

Consuming a User control within another element requires adding a reference to the namespace where the UserControl lives.

```XML
xmlns:uc="clr-namespace:WpfTutorialSamples.User_Controls"
```

Then use the `uc` prefix to add the control in.

```xml
<Window x:Class="WpfTutorialSamples.User_Controls.LimitedInputSample"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:uc="clr-namespace:WpfTutorialSamples.User_Controls"
    Title="LimitedInputSample" Height="200" Width="300">
    <Grid Margin="10">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="*" />
    </Grid.RowDefinitions>
    
    <uc:LimitedInputUserControl Title="Enter title:" MaxLength="30" Height="50" />
    <uc:LimitedInputUserControl Title="Enter description:" MaxLength="140" Grid.Row="1" />
    
    </Grid>
</Window>
```

### CustomControls

## Data Binding
