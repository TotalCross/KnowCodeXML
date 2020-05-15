# Knowcode - Library to allow developers to run Android XML UI + TotalCross on Linux ARM, iOS, Android and more...
KnowCode is a project that was borned to solve a problem that is: how can developers build their screens faster based on the beautiful images made by designers?

So, there is two main blocks:
* Computer vision to understand the image and transform it in a XML file
* Read a XML file, parse to Totalcross and run on Linux ARM, iOS, Android and more...

Here we have the second block, named KnowCodeXML in an alpha version.

## KnowcodeXML

KnowcodeXML is a library that interprets xml files and generates Totalcross screens ready to run on Android, IoS and Linux ARM devices. Today, we are in a alpha version, interpreting  XML files from the main Android layouts: ConstraintLayout, LinearLayout, RelativeLayout and AbstractLayout.

You need to import the jar file or configure maven to use KnowcodeXML in your project. 
After that, you have to import both classes, *XmlContainerFactory* and *XmlContainerLayout*. These classes are the door to other projects can use Knowcode Library. 

![my project KC](https://imgur.com/QGvfHN6.png)

The *XmlContainerFactory* class reads the xml file and realizes which layout to create by reading the xml file until it finds an identification of layout.
Once the layout has been identified, KnowcodeXML calls the abstract class that has all the common methods for layouts and instantiates one of the layout classes. The image below shows the class structure belonging to the project.

![class KC](https://imgur.com/uPAkxQt.png)

### Steps to use Knowcode
* Create a Totalcross Project 
* Import the .jar file of Knowcode (export .jar of the Knowcode Project)
* Import classes *XmlContainerFactory.java* and *XmlContainerLayout.java*
* Instantiate the container of your xml file using *XmlContainerFactory.create*
* Show your Window =)

		Container cont = XmlContainerFactory.create("embeddedWorldSample.xml"); 
		MainWindow.getMainWindow().swap(cont);		
 * To use the components of xml file, use the method *getControlByID* passing like parameter the id of xml file
 
		Control control = ((XmlContainerLayout) container).getControlByID("@+id/btRegister");
		control.setBackColor(Color.BRIGHT);
### Sample
We have some projects on github using this API, like [HelloKnowcode](https://github.com/TotalCross/HelloKnowcode). 
A Hello World project that just open a XML file, swap this on the screen and add some functionality to the buttons: 

		Container cont = XmlContainerFactory.create("embeddedWorldSample.xml"); 
		MainWindow.getMainWindow().swap(cont);
		
		Control minusButton = ((XmlContainerLayouts) container).getControlByID("@+id/minus");
		Label insideTempLabel = (Label) ((XmlContainerLayouts) container).getControlByID("@+id/insideTempLabel");
		
		minusButton.addPressListener(new PressListener() {
			@Override
			public void controlPressed(ControlEvent e) {
				try {
					String tempString = insideTempLabel.getText();
					int temp;
					temp = Convert.toInt(tempString);
					insideTempLabel.setText(Convert.toString(--temp));
				} catch (InvalidNumberException e1) {
					e1.printStackTrace();
				}
			}
		});
The create method of *XmlContainerFactory* class returns a Container object of the layout to put on the window with swap method.
The method *getControlByID* of the class *XmlContainerLayouts* returns a Control object created of the XML file. The screen of this file is shown below.


![EW Sample](https://imgur.com/x3HFDFC.png)


## Another Samples

* [HomeApplianceXML](https://github.com/TotalCross/HomeApplianceXML)
* [KnowcodeSample](https://github.com/TotalCross/KnowcodeSample)
* [ToradexLauncherSample](https://github.com/TotalCross/ToradexLauncherSample)






