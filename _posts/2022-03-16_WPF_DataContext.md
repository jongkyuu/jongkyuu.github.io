---
title: "[WPF] Data Context (DotnetSkool 강의)"
excerpt:

categories:
  - WPF
tags:
  - [C#, WPF, Client]

toc: true
toc_sticky: true

date: 2022-03-16
last_modified_at: 2022-03-16
---

## Data Context Property in WPF

- WPF uses a dependency property called as Data Context property to set the source of bindings.
  - WPF는 데이터 컨텍스트 속성이라는 종속성 속성을 사용하여 바인딩 소스를 설정합니다.
- If any other source is not specified for data bindings then WPF by default searches the Data Context.
  - 데이터 바인딩에 대해 다른 소스가 지정되지 않은 경우 WPF는 기본적으로 데이터 컨텍스트를 검색합니다.
- There's no default source for the Data Context property, you need to assign it some value otherwise binding will be null.
  - 데이터 컨텍스트 속성에 대한 기본 소스가 없습니다. 일부 값을 할당해야 합니다. 그렇지 않으면 바인딩이 null이 됩니다.
- So when ever you are using binding you are binding with data context of the corresponding element.
  - 따라서 바인딩을 사용할 때마다 해당 요소의 데이터 컨텍스트와 바인딩됩니다.

```xml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="80"/>
        <ColumnDefinition Width="*"/>
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="30"/>
        <RowDefinition Height="30"/>
        <RowDefinition Height="30"/>
    </Grid.RowDefinitions>

    <TextBlock Margin="4" Text="First Name" VerticalAlignment="Center"/>
    <TextBox Margin="4" Text="{Binding Path=FirstName}" Grid.Column="1"/>

    <TextBlock Margin="4" Text="Last Name" Grid.Row="1" VerticalAlignment="Center"/>
    <TextBox Margin="4" Text="{Binding Path=LastName}" Grid.Column="1" Grid.Row="1"/>

    <TextBlock Margin="4" Text="Age" Grid.Row="2" VerticalAlignment="Center"/>
    <TextBox Margin="4" Text="{Binding Path=Age}" Grid.Column="1" Grid.Row="2"/>
</Grid>
```

```cs
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();

        Person obj = new Person()
        {
            FirstName = "John",
            LastName = "Smith",
            Age = 20
        };

        // Data Context를 설정하기 전에는 Binding이 Null이 된다
        DataContext = obj;
    }
}
```
