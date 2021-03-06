﻿keywords:batch,Lifecycle,life cycle, life-cycle,Break,BreakPoint, Break Point, Watch, Watch Window, Output window
We'll need to automatically iterate the data in the "Order_Details" table. For that we'll use a **BusinessProcess**
* Add a new item, based on the "BusinessProcess" template and call it "GetOrderStatistics"
![2017 02 27 08H53 58](2017-02-27_08h53_58.png)
* Add the Order_Details table to the controller, and set the Controller's `From` Property with that table.

```csdiff
using System;
using System.Collections.Generic;
using System.Text;
using System.Windows.Forms;
using Firefly.Box;
using ENV;
using ENV.Data;
using System.Diagnostics;

namespace Northwind.Training.SimpleScreen
{
    public class GetOrderStatistics : BusinessProcessBase
    {
+       public readonly Models.Order_Details Order_Details = new Models.Order_Details();

        public GetOrderStatistics()
        {
+           From = Order_Details;
        }
        public void Run()
        {
            Execute();
        }
    }
}

```
* Note that you can remove it's `public` modifier, since we'll only use this table inside this controller.
```csdiff
public class GetOrderStatistics : BusinessProcessBase
{
-   public readonly Models.Order_Details Order_Details = new Models.Order_Details();
+   readonly Models.Order_Details Order_Details = new Models.Order_Details();

    public GetOrderStatistics()
    {
        From = Order_Details;
    }
    public void Run()
    {
        Execute();
    }
}
```
* Place a "Break Point" (using the <kbd>F9</kbd> keyboard shortcut) on the call to `Execute` within the `Run` method, and run our controller
* Use the "Controllers" developer tool to run the 'GetOrderStatistics' BusinessProcess
* Visual studio will "Break" into the code and highlight our `Execute` method  
![2017 02 27 08H58 31](2017-02-27_08h58_31.png)
* We'll add the `Counter` property to the "Watch" Window, to see how many rows get's processed  
![2017 02 27 08H59 33](2017-02-27_08h59_33.png)
* We'll then press <kbd>F10</kbd> to "Break" on next line, and we'll see that the value of `Counter` in the "Watch" window change to reflect the number of rows in the Order_Details table.  
![2017 02 27 09H00 41](2017-02-27_09h00_41.png)
### Business Process Life-Cycle
* Review the BusinessProcess Life-Cycle - make the following changes:
```csdiff
public GetOrderStatistics()
{
+   Debug.WriteLine("Constructor");
    From = Order_Details;
}
public void Run()
{
+   Debug.WriteLine("Before the Execute");
    Execute();
+   Debug.WriteLine("After the Execute");
}

```
* Add code for all the controller life cycle events (just copy)
```csdiff
protected override void OnLoad()
{
    Debug.WriteLine("\tOnLoad");
}
protected override void OnStart()
{
    Debug.WriteLine("\t\tOnStart");
}
protected override void OnEnterRow()
{
    Debug.WriteLine("\t\t\tOnEnterRow");
}
protected override void OnLeaveRow()
{
    Debug.WriteLine("\t\t\tOnLeaveRow");
}
protected override void OnSavingRow()
{
    Debug.WriteLine("\t\t\tOnSavingRow");
}
protected override void OnEnd()
{
    Debug.WriteLine("\t\tOnEnd");
}
protected override void OnUnLoad()
{
    Debug.WriteLine("\tOnUnLoad");
}
```
* Run the code and review the output in Visual Studio "Output" window 
* Change the `OnEnterRow` method to reflect the values of the OrderID and the ProductID
```csdiff
protected override void OnEnterRow()
{
+   Debug.WriteLine("\t\t\tOnEnterRow Order:" + Order_Details.OrderID + " Product:" + Order_Details.ProductID);
}
```
* Run the code and review the output in Visual Studio "Output" window 
<iframe width="560" height="315" src="https://www.youtube.com/embed/7lcdz6ACF8o?list=PL1DEQjXG2xnKS0Zo7h-PrExXZ18hGxhvA" frameborder="0" allowfullscreen></iframe>


