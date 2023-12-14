---  
title: WPF如何使用触发器  
date: 2015-11-02 20:29:11+08:00  
tags:   
canonicalURL: https://blog.csdn.net/sinat_32124195/article/details/49591553  
share: true  
slug: wpf-trigger  
---  
  
## 一 属性触发器  
1.WPF 如何实现下面的功能  
![20151102204649328.png](/images/20151102204649328.png)  
2.下面就是利用属性触发器实现 (如果不懂可以看看《WPF 高级编程》这本书)  
```xml  
 <!--给编辑资料添加属性触发器,字体颜色变色，提醒用户可以点击-->  
<Style x:Key="styPropert" TargetType="{x:Type CheckBox}">  
	  
	<Style.Triggers>  
		<Trigger Property="IsMouseOver" Value="true">  
			<Setter Property="BorderBrush" Value="Yellow"></Setter>  
		</Trigger>  
	</Style.Triggers>  
</Style>  
```  
3.如果针对一个这种情况你可能会使用 MouseEnter 和 MouseLeave 两个事件,但是如果这种情况多的话，差不多相同的重复写，还有你可以多个 Label 共同作用这两个事件，这样也确实变的简单了，但是你利用属性触发器代码变得就会变得更清晰和更优美  
```C#  
//利用这两个事件实现上图简单的功能  
private void label1_MouseEnter(object sender, MouseEventArgs e)  
{  
	label1.Foreground = Brushes.Blue;  
	//将鼠标指针变成手的形状  
	label1.Cursor = Cursors.Hand;  
}  
  
private void label1_MouseLeave(object sender, MouseEventArgs e)  
{  
	label1.Foreground = Brushes.Black;  
  
}  
```  
## 二 多触发器   
1.我现在做一个简单的关键字变色（只是为了演示多触发器），本来是应该需要正则表达式来完成  
![20151123000136792.png](/images/20151123000136792.png)  
对于这种功能的实现你完全可以使用一个事件来写，但是今天只是想说说触发器如何使用  
```xml  
<Window.Resources>  
	<Style x:Key="styColor" TargetType="{x:Type ComboBox}">  
		<Style.Triggers>  
			<MultiTrigger>  
			     
				<MultiTrigger.Conditions>  
					<!--被选择，并且内容为是整个字体变颜色-->  
					<Condition Property="SelectedIndex" Value="0"></Condition>  
					<Condition Property="Text" Value="WPF"></Condition>  
				</MultiTrigger.Conditions>  
				<!--满足上面两个条件，就可以触发这个属性-->  
				<MultiTrigger.Setters>  
					<Setter Property="Foreground" Value="Red"></Setter>  
				</MultiTrigger.Setters>  
			</MultiTrigger>  
		</Style.Triggers>  
	</Style>  
</Window.Resources>  
```  
2.这个是 comboBox 的代码和绑定，这个很简单  
```xml  
<ComboBox Height="23" HorizontalAlignment="Left" Margin="73,96,0,0" Name="comboBox1" VerticalAlignment="Top" Width="120" Style="{StaticResource styColor}" SelectedIndex="0">  
		<ComboBoxItem>PHP</ComboBoxItem>  
		<ComboBoxItem >WPF</ComboBoxItem>  
		<ComboBoxItem>Python</ComboBoxItem>  
</ComboBox>  
```  
  
## 三 数据触发器  
1.这个我要说的是，如何后台类的一个属性绑定到前台，来触发属性  
  
目的是：现在我点击按钮更改类中的一个属性值 ，而这个属性值 (Text="Hello World") 刚好触发了数据触发器，触发后更改 label 的字体颜体  
```xml  
<Window.Resources>  
        <Style x:Key="styData" TargetType="{x:Type Label}">  
            <Style.Triggers>  
                <DataTrigger Binding="{Binding Path=Text}" Value="Hello World">  
                    <!--如果后台类的属性Text等于Hello World时，就会触发属性，将字体颜色改变红色-->  
                    <Setter Property="Foreground" Value="Red"></Setter>  
                </DataTrigger>  
            </Style.Triggers>  
        </Style>  
</Window.Resources>  
```  
2.控件资源绑定  
```xml  
<Grid Background="Bisque">  
        <Label Content="name:lbl" Height="37" HorizontalAlignment="Left" Margin="224,110,0,0" Name="lbl" VerticalAlignment="Top" Width="auto" Style="{StaticResource styData}" />  
        <Button Content="输出,Hello world" Height="auto" HorizontalAlignment="Left" Margin="224,204,0,0" Name="btnPrint" VerticalAlignment="Top" Width="auto" Click="btnPrint_Click" />  
    </Grid>  
```  
3.下来就是后台的代码实现，并且还有一个重要**细节**  
**如何想实现，还要对 label 的一个的**DataContext**赋值，而这个值必须是该属性的所在类的实例（我这是是 this）**  
```c#  
 private void btnPrint_Click(object sender, RoutedEventArgs e)  
        {  
            //如何单击按钮.给属性赋值,这个就会触发前台的数据触发器  
            Text = "Hello World";  
            lbl.DataContext = this;  
        }  
        string _text;  
        //前台绑定该属性  
        public string Text  
        {  
            get { return _text; }  
            set { _text = value; }  
        }  
```