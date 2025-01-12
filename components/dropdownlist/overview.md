---
title: Overview
page_title: DropDown List for Blazor Overview
description: Overview of the DropdownList for Blazor
slug: components/dropdownlist/overview
tags: telerik,blazor,dropdownlist,dropdown,list,overview
published: True
position: 0
---

# DropDownList Overview

The DropDownList component allows the user to choose an option from a predefined set of choices presented in a dropdown popup. The developer can control the data, sizes, and various appearance options like class and [templates]({%slug components/dropdownlist/templates%}).

To use a Telerik DropDownList for Blazor

1. add the `TelerikDropDownList` tag
1. populate its `Data` property with the collection of items you want in the dropdown
1. set the `TextField` and `ValueField` properties to point to the corresponding names of the model
1. Set the `Value` property to the intial value of the model.

>caption Basic dropdownlist data binding and ValueChanged event handling

@[template](/_contentTemplates/common/issues-and-warnings.md#generic-component-event-issue)

````CSHTML
@using Telerik.Blazor.Components.DropDownList

<TelerikDropDownList Data="@myDdlData" TextField="MyTextField" ValueField="MyValueField" ValueChanged="@MyValueChangedHandler" bind-Value="@selectedValue">
</TelerikDropDownList>

@functions {
	//in a real case, the model is usually in a separate file
	//the model type and value field type must be provided to the dropdpownlist
	public class MyDdlModel
	{
		public int MyValueField { get; set; }
		public string MyTextField { get; set; }
	}

	IEnumerable<MyDdlModel> myDdlData = Enumerable.Range(1, 20).Select(x => new MyDdlModel { MyTextField = "item " + x, MyValueField = x });

	int selectedValue { get; set; } = 3; //usually the current value should come from the model data

	void MyValueChangedHandler(object newValue)
	{
		//the data item (model) that the user selected
		Console.WriteLine((newValue as MyDdlModel).MyTextField);
	}
}

````

>caption The result from the code snippet above

![](images/dropdownlist-basic-screenshot.jpg)

>caption Component namespace and reference

````CSHTML
@using Telerik.Blazor.Components.DropDownList

<TelerikDropDownList ref="@myDdlRef" Data="@myDdlData" TextField="MyTextField" ValueField="MyValueField" Value="3">
</TelerikDropDownList>
@functions {
	//the type of the generic component is determined by the type of the model you pass to it, and the type of its value field
	Telerik.Blazor.Components.DropDownList.TelerikDropDownList<MyDdlModel, int> myDdlRef;

	public class MyDdlModel
	{
		public int MyValueField { get; set; }
		public string MyTextField { get; set; }
	}

	IEnumerable<MyDdlModel> myDdlData = Enumerable.Range(1, 20).Select(x => new MyDdlModel { MyTextField = "item " + x, MyValueField = x });
}
````

The DropDownList provides the following features:

* `Class` - the CSS class that will be rendered on the main wrapping element of the dropdownlist.
* `Data` - allows you to provide the data source. Required.
* `DefaultItem` - sets the hint that is shown if no other item is selected. Set this property to an instance of the model class to which the dropdown is bound.
* `Enabled` - whether the component is enabled.
* `Height` - the height of the main element. See the [Dimensions]({%slug common-features/dimensions%}) article.
* `PopupHeight` - the height of the expanded dropdown list element.
* `T` - the type of the model to which the component is bound. Required. Determines the type of the reference object.
* `TItem` - the type of the value field from the model to which the component is bound. Required. Determines the type of the reference object.
* `TabIndex` - the `tabindex` attribute rendered on the dropdown.
* `TextField` - the name of the field from the model that will be shown to the user. Defaults to `Text`.
* `ValueField` - the name of the field from the model that will be the underlying `value`. Defaults to `Value`.
* `Value` and `bind-Value`- get/set the value of the component, can be used for binding. If you set it to a value allowed by the model class value field, the corresponding item from the data collection will be pre-selected. Use the `bind-Value` syntax for two-way binding, for example, to a variable of your own. 
    
    The `Value` and `ValueField` can be of types:

    * `number` (such as `int`, `double` and so on)
    * `string`
    * `Guid`
    * `Enum`

* `Width` - the width of the dropdown and the main element.
* Templates - they allow you to control the rendering of items in the component. See the [Templates]({%slug components/dropdownlist/templates%}) article for more details.
* Validation - see the [Input Validation]({%slug common-features/input-validation%}) article for more details.


## Missing Value or Data

The DropDownList component attempts to infer the type of its model and value based on the provided `Data` and initial `Value`. In case you cannot provide either of them, you need to set the corresponding type properties to the `TItem` and `TValue` properties as shown below.

>caption DropDownList configuration if you cannot provide Value or Data

````CSHTML
@using Telerik.Blazor.Components.DropDownList

<TelerikDropDownList Data="@myDdlData" TextField="MyTextField" ValueField="MyValueField" TValue="int" TItem="MyDdlModel">
</TelerikDropDownList>
@functions {
	public class MyDdlModel //TItem matches the type of the model
	{
		public int MyValueField { get; set; } //TValue matches the type of the value field
		public string MyTextField { get; set; }
	}

	IEnumerable<MyDdlModel> myDdlData = Enumerable.Range(1, 20).Select(x => new MyDdlModel { MyTextField = "item " + x, MyValueField = x });
	
	//the same configuration applies if the "myDdlData" object is null initially and is populated on some event
}

````


## Examples


>caption Validating a dropdownlist. See the comments in the code for details on the behavior

````CSHTML
@using Telerik.Blazor.Components.DropDownList
@using System.ComponentModel.DataAnnotations

<EditForm Model="@person" OnValidSubmit="@HandleValidSubmit">
	<DataAnnotationsValidator />
	<ValidationSummary />
	<p class="gender">
		Gender: <TelerikDropDownList bind-Value="@person.Gender" DefaultItem="@ddlHint"
								   Data="@genders" TextField="MyTextField" ValueField="MyValueField">
				</TelerikDropDownList>
		<ValidationMessage For="@(() => person.Gender)"></ValidationMessage>
	</p>

	<button type="submit">Submit</button>
</EditForm>

@functions {
	// Usually the model classes would be in different files
	public class Person
	{
		[Required(ErrorMessage = "Gender is mandatory.")]//the value field in the dropdown model must be null in the default item
		[Range(1, 3, ErrorMessage = "Please select your gender.")] //limits the fourth option just to showcase this is honored
		public int? Gender { get; set; }
	}

	public class MyDdlModel
	{
		//nullable so the default item can allow required field validation
		//alternatively, use a range validator and put a value out of that range for the default item
		public int? MyValueField { get; set; }
		public string MyTextField { get; set; }
	}

	Person person = new Person();

	MyDdlModel ddlHint = new MyDdlModel { MyValueField = null, MyTextField = "Gender" };

	IEnumerable<MyDdlModel> genders = new List<MyDdlModel>
{
		new MyDdlModel {MyTextField = "female", MyValueField = 1},
		new MyDdlModel {MyTextField = "male", MyValueField = 2},
		new MyDdlModel {MyTextField = "other", MyValueField = 3},
		new MyDdlModel {MyTextField = "I'd rather not say", MyValueField = 4}
	};

	void HandleValidSubmit()
	{
		Console.WriteLine("OnValidSubmit");
	}
}
````


>caption Get selected item from external code

````CSHTML
@using Telerik.Blazor.Components.DropDownList

<TelerikDropDownList ref="@myDdlRef" Data="@myDdlData" TextField="MyTextField" ValueField="MyValueField" Value="5">
</TelerikDropDownList>
<TelerikButton OnClick="@GetSelectedItem">Get Selected Item</TelerikButton> @result
@functions {
	string result;
	void GetSelectedItem()
	{
		if (myDdlRef.SelectedDataItem != null)
		{
			result = (myDdlRef.SelectedDataItem as MyDdlModel).MyTextField;
		}
		else
		{
			result = "no item selected";
		}

		StateHasChanged();
	}

	//the type of the generic component is determined by the type of the model you pass to it, and the type of its value field
	Telerik.Blazor.Components.DropDownList.TelerikDropDownList<MyDdlModel, int> myDdlRef;

	public class MyDdlModel
	{
		public int MyValueField { get; set; }
		public string MyTextField { get; set; }
	}

	IEnumerable<MyDdlModel> myDdlData = Enumerable.Range(1, 20).Select(x => new MyDdlModel { MyTextField = "item " + x, MyValueField = x });
}
````




## See Also

  * [Live Demo: DropDownList](https://demos.telerik.com/blazor-ui/dropdownlist/index)
  * [Live Demo: DropDownList Validation](https://demos.telerik.com/blazor-ui/dropdownlist/validation)
  
  