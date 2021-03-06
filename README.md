# Knowcode - Library to allow developers to run Android XML UI + TotalCross on Linux ARM, iOS, Android and more...
KnowCode is a project that was born to solve a problem, which is: 
How can developers build their screens faster based on images made by designers?

So, there is two main tasks:
* Computer vision to understand the image and convert it to a XML file
* Read a XML file, parse to Totalcross and run on Linux ARM, iOS, Android and more...

Here we have the second task, named KnowCodeXML.
KnowcodeXML is a library that interprets Android XML files and generates Totalcross screens ready to run on Android, iOS and Linux ARM devices.

## How to Install Knowcode on your Project
* Create a Totalcross Project 
  * You can create a simple hello world project like [Hello World project](https://learn.totalcross.com/documentation/get-started/install#create-a-hello-world-project).
	If you does not have the enviroment setup, follow this changes [Install TotalCross](https://learn.totalcross.com/documentation/get-started/install#install-the-totalcross-plugin)
* Use TotalCross sdk 7.0.1 or higher and add the dependency on file pom.xml
	* if you are using totalcross 6, please use knowcode 1.13
```xml
<dependency>
    <groupId>com.totalcross</groupId>
    <artifactId>totalcross-sdk</artifactId>
    <version>7.0.1</version>
</dependency>
```

* Add KnowcodeXML dependency
```xml
<dependency>
    <groupId>com.totalcross.knowcode</groupId>
    <artifactId>KnowCodeXML</artifactId>
    <version>1.14</version>
</dependency>
```
* Put totalcross-maven-plugin version 2.0.1 or higher
```xml
<groupId>com.totalcross</groupId>
<artifactId>totalcross-maven-plugin</artifactId>
<version>2.0.2</version>
```
* So, that's it! Now let's start using it!

## Create a Hello World Project

* Put the xml file on your folder resources (src/main/resources). Here we have a simple xml of [login screen](https://github.com/TotalCross/HelloKnowcode/blob/master/src/main/resources/simpleScreen.xml).
* Import the class *XmlContainerFactory* on the *MainWindow* class and make this changes on *initUI* method
```java
import com.totalcross.knowcode.parse.XmlContainerFactory;
public class HelloKnowcode extends MainWindow {
    @Override
    public void initUI() {
        Container cont = XmlContainerFactory.create("simplescreen.xml");
        MainWindow.getMainWindow().swap(cont);
    }
}
 ```
 * Run the project, we can see the screen! 

## Make changes on your screen

 * To change the components of xml file, use the method *getControlByID* passing like parameter the id of xml file. Here we just change the button color to ilustrate.
 ```java
import com.totalcross.knowcode.parse.XmlContainerLayout;
import com.totalcross.knowcode.parse.XmlContainerFactory;
public class HelloKnowcode extends MainWindow {
	@Override
	public void initUI() {
		Container cont = XmlContainerFactory.create("simpleScreen.xml");
		MainWindow.getMainWindow().swap(cont);
		Control control = ((XmlContainerLayout) container).getControlByID("@+id/btRegister");
		control.setBackColor(Color.BRIGHT);
	}
}
```

 * If you have to add some components on the screen or make some change before swap the window, use the CustomInitUI Interface
```java
import com.totalcross.knowcode.parse.XmlContainerLayout;
import com.totalcross.knowcode.parse.CustomInitUI;
import com.totalcross.knowcode.parse.XmlContainerFactory;
public class HelloKnowcode extends MainWindow {
@Override
	public void initUI() {
		Container cont = XmlContainerFactory.create("simpleScreen.xml");
		XmlContainerLayout xmlContainerLayout = (XmlContainerLayout)cont;

		xmlContainerLayout.setCustomInitUI(new CustomInitUI() {
			public void postInitUI(XmlContainerLayout contLayout) {
				Button btCancel = new Button("Cancel");
				btCancel.setBackColor(Color.BRIGHT);
				Control btRegister = contLayout.getControlByID("@+id/btRegister");
				btRegister.setBackColor(Color.BRIGHT);
				int posBtCancel = btRegister.getY()+btRegister.getHeight();

				contLayout.add(btCancel, Container.CENTER, posBtCancel+2, Container.PARENTSIZE, Container.PREFERRED);
			}
		});
	MainWindow.getMainWindow().swap(cont);
}
```
* Build the project like any other TotalCross project. See [here](https://learn.totalcross.com/documentation/get-started/install#package-your-application)
  
## How KnowcodeXML works
We support all main Android layouts: ConstraintLayout, LinearLayout and RelativeLayout.

You just have to configure maven to use KnowcodeXML in your Totalcross project. After that, import the needed classes, *XmlContainerFactory* and *XmlContainerLayout*. These classes are the entry points to projects that use Knowcode Library. If you have to add some components that does not in your xml file, import the *CustomInitUI* too

![my project KC](https://imgur.com/fW7kgeC.png)

The *XmlContainerFactory* class reads the XML file and defines which layout to create based on the wrapping layout from XML. Once the layout has been identified, KnowcodeXML calls the abstract class that has all the common methods for layouts and instantiates one of the layout classes. The image below shows the class structure belonging to the project.

![class KC](https://imgur.com/oV08WZO.png)
### Sample
We have some projects on github using this API, like [HomeApplianceXML](https://github.com/TotalCross/HomeApplianceXML), a sample application that illustrate the use of API in a Home Appliance Controller.

![EW Sample](https://imgur.com/jkBlar1.png)

The create method of *XmlContainerFactory* class returns a Container object of the layout to put on the window with swap method.
The method *getControlByID* of the class *XmlContainerLayouts* returns a Control object created of the XML file.
```java
public void initUI() {
    XmlContainerLayout xmlCont = (XmlContainerLayout) XmlContainerFactory.create("xml/homeApplianceXML2.xml");
    swap(xmlCont);

    Button plus = (Button) xmlCont.getControlByID("@+id/plus");
    ...

    plus.addPressListener(new PressListener() {
        @Override
        public void controlPressed(ControlEvent e) {
            // TODO
            try {
                String tempString = insideTempLabel.getText();
                int temp;
                temp = Convert.toInt(tempString);
                insideTempLabel.setText(Convert.toString(++temp));

            } catch (InvalidNumberException e1) {
                // TODO Auto-generated catch block
                e1.printStackTrace();
            }
        }
    });
}
```
## More Samples and Tutorials

* [HelloKnowcode](https://github.com/TotalCross/embedded-samples/tree/main/hello-knowcode)
* [HomeApplianceXML](https://github.com/TotalCross/HomeApplianceXML)
* [KnowcodeSample](https://github.com/TotalCross/embedded-samples/tree/main/knowcode-sample)
* [ToradexLauncherSample](https://github.com/TotalCross/embedded-samples/tree/main/toradex-launcher-sample)
* [Android XML + Totalcross = Rich UI/UX on Linux ARM](https://www.youtube.com/watch?v=7o3p14wQPsE)
* [Creating and using your own external Java library for your TotalCross Applications](https://www.youtube.com/watch?v=Cq5yEPTmZWI)






