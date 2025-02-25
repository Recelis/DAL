+++
title = 'WPF'
date = 2025-02-25T20:14:41.339Z
draft = true
+++

# WPF
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


