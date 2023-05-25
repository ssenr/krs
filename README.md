# krs
script.

Oh My God AE Scripting is such a pain in da damn ass

# Resources

https://community.adobe.com/t5/after-effects-discussions/read-all-keyframes-from-a-layer/td-p/4462837

https://community.adobe.com/t5/after-effects-discussions/after-effects-cc-2017-14-0-new-scripting-functionality/td-p/8625058

DecomposeText

RepositionAnchorPoint

pt_ShiftLayers

Ease and Wizz

Layers2Grid
	
KeyTweak

rd: Scooter

CS6 Scripting Guide

https://ae-scripting.docsforadobe.dev/index.html

https://extendscript.docsforadobe.dev/extendscript-toolkit/configuring-the-toolkit-window.html

I think I figured out the solution to this.

1) Recursively traverse through all the PropertyGroups in the layer.

2) If you find a property(do propertyType!=PropertyType.PROPERTY) push it into a properties array.

3) When this recursive operation completes, go through all the collected properties to see if there are any keyframes in each property.

CODE:
```JS
var RootPropGroup = app.project.activeItem.selectedLayers[0].text.animator;

function GetAllProperties(PropGroup)

{

    var props = new Array();

    var n = PropGroup.numProperties;

    for(var i=1; i<=n; i++)

    {

        props.push(PropGroup.property(i));

    }

    return props;

}

var Props = GetAllProperties(RootPropGroup);

var PropGroupArray  = new Array();

var PropArray = new Array();

ClassifyPropsAndPropGroups(Props);

//Given an array, it segregates the elements into properties and property groups

function ClassifyPropsAndPropGroups(arr)

{

      for(var i = 0; i<arr.length; i++)

    {

        if(arr.propertyType != PropertyType.PROPERTY)

        {

            PropGroupArray.push(arr)

        }

        else

        {

            PropArray.push((arr));

        }

    }

}

//Recursively go through all the property groups and find all properties

while(PropGroupArray.length!=0)

{

     //Get all the props from the last available property group,

    var arr = GetAllProperties(PropGroupArray.pop());

     //Classify between Props and prop groups

    ClassifyPropsAndPropGroups(arr);

}

//Finally store all the properties with keyframes into one array

var PropsWithKeys = new Array();

for(var i = 0; i < PropArray.length; i++)

{

    if(PropArray.numKeys != 0)

    {

        PropsWithKeys.push(PropArray)

    }

}
```
